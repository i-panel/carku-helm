# Carku Helm Chart Repository

This repository contains a Helm chart which can be used to bootstrap a production-ready Carku GPS Tracking Server on a Kubernetes cluster using the Helm package manager.

### Prerequisites
- Kubernetes 1.12+
- Helm 3.1.0
- PV provisioner support in the underlying infrastructure

### Installing the Chart
To install the chart with the release name `my-carku`, run the following commands:
```
helm repo add carku https://carku.github.io/carku-helm/
helm install my-carku carku/carku
```
These commands deploy Carku server configured with a MySQL database. You can customize the deployment using a custom values file, or command-line arguments.

### Accessing the Application
After installation, you can get the application URL depending on your `service.type`:

#### 1. ClusterIP (Default)
To access the application locally, use port-forwarding:
```bash
export POD_NAME=$(kubectl get pods --namespace default -l "app.kubernetes.io/name=carku,app.kubernetes.io/instance=my-carku" -o jsonpath="{.items[0].metadata.name}")
export CONTAINER_PORT=$(kubectl get pod --namespace default $POD_NAME -o jsonpath="{.spec.containers[0].ports[0].containerPort}")
kubectl --namespace default port-forward $POD_NAME 8080:$CONTAINER_PORT
```
Then visit: `http://127.0.0.1:8080`

#### 2. NodePort
```bash
export NODE_PORT=$(kubectl get --namespace default -o jsonpath="{.spec.ports[0].nodePort}" services my-carku)
export NODE_IP=$(kubectl get nodes --namespace default -o jsonpath="{.items[0].status.addresses[0].address}")
echo http://$NODE_IP:$NODE_PORT
```

#### 3. LoadBalancer
Wait for the LoadBalancer IP to be available:
```bash
kubectl get svc --namespace default -w my-carku
```
Once available, get the URL:
```bash
export SERVICE_IP=$(kubectl get svc --namespace default my-carku --template "{{range (index .status.loadBalancer.ingress 0)}}{{.}}{{end}}")
echo http://$SERVICE_IP:80
```

### Private Image Authentication (GHCR)
Since the Carku image is stored in a private GitHub Container Registry (GHCR), you need to create a `docker-registry` secret in your Kubernetes cluster to allow pulling the image.

1. **Create a GitHub Personal Access Token (PAT):**
   - Go to GitHub Settings -> Developer settings -> Personal access tokens -> Tokens (classic).
   - Generate a new token with at least `read:packages` scope.

2. **Create the Kubernetes Secret:**
   Run the following command in the namespace where you intend to install the chart:
   ```bash
   kubectl create secret docker-registry ghcr-pull-secret \
     --docker-server=ghcr.io \
     --docker-username=<YOUR_GITHUB_USERNAME> \
     --docker-password=<YOUR_GITHUB_PAT> \
     --docker-email=<YOUR_EMAIL>
   ```

3. **Configure the Chart:**
   Ensure your `values.yaml` includes the secret name in the `imagePullSecrets` section:
   ```yaml
   imagePullSecrets:
     - name: ghcr-pull-secret
   ```

### Uninstalling the Chart
To uninstall/delete the `my-carku` deployment, run the following command:
```
helm del my-carku
```
The command removes all the Kubernetes components associated with the deplyoment except the PersistentVolume objects, for safety reasons.

### Parameters
The chart incorporates every configuration file setting, which can be found [ here](https://www.carku.org/configuration-file/)

### Image flavors
The [carku container image](https://github.com/carku/carku-docker) comes in _different flavors_. Currently available are
* alpine
* debian
* ubuntu
If none is specified during installation of this chart, the default is alpine based.

To set your preferred flavor, you can use `--set image.flavor=debian` for example, or, for more reusability, append the following lines to your installation's `values.yaml`
```yaml
image:
  flavor: "debian"
```

### Core Components and Toggles
You can enable or disable various components of the Carku stack using the `--set` flag or by modifying `values.yaml`.

| Component | Key | Default | Description |
|-----------|-----|---------|-------------|
| **MySQL** | `mysql.enabled` | `true` | Internal MySQL database for Carku. |
| **PostgreSQL** | `postgresql.enabled` | `false` | Internal PostgreSQL database (supports TimescaleDB). |
| **Redis HA** | `redis-ha.enabled` | `false` | Redis High Availability cluster for broadcast/coordination. |
| **Nominatim** | `nominatim.enabled` | `false` | Geocoding service (requires S3 for data persistence). |
| **Ingress** | `ingress.enabled` | `false` | Expose the Carku web interface via an Ingress controller. |
| **Autoscaling** | `autoscaling.enabled` | `false` | Enable Horizontal Pod Autoscaler (HPA). |
| **External Service** | `externalService.enabled` | `false` | LoadBalancer service for GPS tracker protocols (ports 5001+). |

### Automatic Integration
When `nominatim.enabled` is set to `true`, the chart automatically configures the Traccar geocoder to use the internal Nominatim service. You don't need to manually set the `geocoder.url` unless you want to use an external provider.

**Example: Enabling Redis, Ingress and Nominatim**
```bash
helm install my-carku carku/carku \
  --set redis-ha.enabled=true \
  --set ingress.enabled=true \
  --set ingress.hosts[0].host=carku.yourdomain.com \
  --set nominatim.enabled=true \
  --set nominatim.ingress.enabled=true \
  --set nominatim.ingress.hosts[0].host=nominatim.yourdomain.com
```

### Nominatim Integration
Nominatim provides geocoding services. It requires an initial setup (CREATE mode) to import data into a PostgreSQL database and archive it to S3 storage, followed by a stable deployment (RESTORE mode).

#### Initial Deployment Flow:
1. **Configure S3 Storage:** Ensure you have access keys for your S3-compatible object storage (e.g., AWS or ArvanCloud Storage).
2. **Enable Canary for Initialization:**
   Set `nominatim.enabled=true`, `nominatim.canary.enabled=true`, and provide S3 credentials in `values.yaml` (or via `--set`).
3. **Wait for Import:** Wait for the canary pod to create the database and upload its archive to S3.
4. **Switch to Stable:**
   Disable canary (`nominatim.canary.enabled=false`) and enable stable (`nominatim.stable.enabled=true`).

#### Configuration:
In your `values.yaml`:
```yaml
nominatim:
  enabled: true
  config:
    pbfUrl: "http://download.geofabrik.de/asia/iran-latest.osm.pbf"
    dataLabel: "iran-260525"
    awsRegion: "ir-thr-at1"
    s3Bucket: "nominatim-data"
  s3:
    accessKeyId: "your-access-key"
    secretAccessKey: "your-secret-key"
  canary:
    enabled: false # Set to true for initial import or data update
  stable:
    enabled: true
    replicas: 3
```

#### Updating Data:
To update the database with new PBF data:
1. Deploy the canary deployment alongside the stable deployment (`nominatim.canary.enabled=true`).
2. Wait for the archive to be updated in S3.
3. Delete the canary deployment.
4. Restart/Rolling update the stable deployment to restore from the new archive.

#### Accessing Nominatim:
Once the stable deployment is running, you can access Nominatim internally within the cluster at:
`http://my-carku-nominatim:80/search`

To access it from outside the cluster for testing:
```bash
kubectl port-forward svc/my-carku-nominatim 8081:80
```
Then visit: `http://127.0.0.1:8081/search`

### PostgreSQL & TimescaleDB Integration
For large production environments, it is recommended to use PostgreSQL with the TimescaleDB extension for optimized time-series data storage.

To enable PostgreSQL with TimescaleDB:
```bash
helm install my-carku carku/carku \
  --set mysql.enabled=false \
  --set postgresql.enabled=true \
  --set postgresql.timescaledb.enabled=true
```

### High-Scale Optimization
To support a large number of devices and users, you can enable system-level optimizations via `values.yaml`. These settings help the operating system and the JVM handle tens of thousands of concurrent connections.

#### 1. System-level (sysctl & ulimit)
Enable the optimization block to apply kernel-level tweaks:
```yaml
optimization:
  sysctl:
    enabled: true        # Requires privileged init container
    maxMapCount: 250000
    fileMax: 250000
    portRange: "1024 65535"
  ulimit:
    enabled: true
    nofile: 50000
```

#### 2. Java Virtual Machine (JVM)
Increase the heap size for the Java process to handle more data in memory:
```yaml
command: ["java", "-Xms1G", "-Xmx1G", "-Djava.net.preferIPv4Stack=true"]
```

#### 3. Traccar Configuration
Set appropriate timeouts to clear stale connections:
```yaml
carku:
  server:
    timeout: 120 # Slightly higher than device reporting interval
```

**Note:** Enabling `optimization.sysctl.enabled` requires a privileged init container. Ensure your Kubernetes cluster allows privileged containers if you enable this feature.
