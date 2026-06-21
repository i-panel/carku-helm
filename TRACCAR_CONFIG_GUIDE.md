
# راهنمای جامع پارامترهای پیکربندی Traccar | Traccar Configuration Parameters Reference

> **نکته مهم برای کاربران این Helm Chart:**  
> فایل `carku.xml` که توسط ConfigMap ساخته می‌شود، از متغیرهای محیطی (Environment Variables) برای مقادیر حساس مانند `database.user` و `database.password` استفاده می‌کند. بنابراین در بخش `database` پارامترهای `user` و `password` در ConfigMap به صورت `{{ env "DATABASE_USER" }}` و `{{ env "DATABASE_PASSWORD" }}` درج می‌شوند. برای اعمال تنظیمات خود، می‌توانید این متغیرها را از طریق `secret.yaml` یا با تعریف `env` در `values.yaml` مقداردهی کنید.

این سند کلیه پارامترهای موجود در فایل پیکربندی XML سیستم ردیابی Traccar را به همراه توضیحات انگلیسی (اصلی) و ترجمهٔ روان فارسی ارائه می‌دهد. توضیحات انگلیسی عیناً از مستندات رسمی استخراج شده‌اند. بخش‌های «یادداشت اضافی» در صورت نیاز برای فهم بهتر افزوده شده‌اند.

> **نکته:** پارامترها به گروه‌های منطقی تقسیم شده‌اند. هر پارامتر دارای برچسب محدوده (Scope) است که نشان می‌دهد در کدام بخش قابل تنظیم است:  
> `config` = در فایل XML پیکربندی • `device` = در تنظیمات دستگاه • `server` = در تنظیمات سرور • `user` = در تنظیمات کاربر

---

## ۱. پروتکل‌های ارتباطی (Protocol Parameters)

پارامترهایی که با `[protocol]` مشخص شده‌اند، به‌جای `[protocol]` باید نام پروتکل واقعی (مانند `osmand`, `teltonika` و غیره) قرار گیرد.

### `[protocol].address`
- **Scope:** `config`
- **English:** Network interface for the protocol. If not specified, the server will bind to all interfaces. Multiple addresses (for example, one IPv4 and one IPv6) can be supplied as a comma-separated list.
- **فارسی:** رابط شبکه برای پروتکل. اگر مشخص نشود، سرور به همهٔ رابط‌ها متصل می‌شود. می‌توان چند آدرس (مثلاً یک IPv4 و یک IPv6) را با کاما جدا کرد.

### `[protocol].port`
- **Scope:** `config`
- **English:** Port number for the protocol. Most protocols use TCP on the transport layer. Some protocols use UDP. Some support both TCP and UDP.
- **فارسی:** شماره پورت پروتکل. اکثر پروتکل‌ها از TCP استفاده می‌کنند. برخی UDP و برخی هر دو را پشتیبانی می‌کنند.

### `[protocol].devices`
- **Scope:** `config`
- **English:** List of devices for polling protocols. The list should contain unique IDs separated by commas. Used only for polling protocols.
- **فارسی:** فهرست دستگاه‌ها برای پروتکل‌های polling. شناسه‌های یکتا با کاما جدا شوند. فقط برای پروتکل‌های poll شونده کاربرد دارد.

### `[protocol].interval`
- **Scope:** `config`
- **English:** Polling interval in seconds. Used only for polling protocols.
- **فارسی:** فاصلهٔ زمانی poll شدن به ثانیه. فقط برای پروتکل‌های poll شونده.

### `[protocol].ssl`
- **Scope:** `config`
- **English:** Enable SSL support for the protocol. Not all protocols support this.
- **فارسی:** فعال‌سازی پشتیبانی SSL برای پروتکل. همهٔ پروتکل‌ها این قابلیت را ندارند.

### `[protocol].timeout`
- **Scope:** `config`
- **English:** Connection timeout value in seconds. Because sometimes there is no way to detect a lost TCP connection, old connections stay in an open state. On most systems there is a limit on the number of open connections, so this leads to problems with establishing new connections when the number of devices is high or device data connections are unstable.
- **فارسی:** مدت‌زمان timeout اتصال به ثانیه. از آنجا که گاهی تشخیص قطع شدن اتصال TCP ممکن نیست، اتصالات قدیمی باز می‌مانند. در بیشتر سیستم‌ها تعداد اتصالات باز محدود است، بنابراین این موضوع در تعداد بالای دستگاه‌ها یا اتصالات ناپایدار مشکل ایجاد می‌کند.

### `devicePassword`
- **Scope:** `device`
- **English:** Device password. Commonly used in some protocols for sending commands.
- **فارسی:** رمز عبور دستگاه. معمولاً در برخی پروتکل‌ها برای ارسال فرمان استفاده می‌شود.

### `[protocol].devicePassword`
- **Scope:** `config`
- **English:** Device password. Commonly used in some protocols for sending commands.
- **فارسی:** رمز عبور دستگاه (در سطح پروتکل). معمولاً برای ارسال فرمان در برخی پروتکل‌ها کاربرد دارد.

### `[protocol].mask`
- **Scope:** `config`
- **English:** Default protocol mask to use. Currently used only by Skypatrol protocol.
- **فارسی:** ماسک پیش‌فرض پروتکل. در حال حاضر فقط توسط پروتکل Skypatrol استفاده می‌شود.

### `[protocol].messageLength`
- **Scope:** `config`
- **English:** Custom message length. Currently used only by H2 protocol for specifying binary message length.
- **فارسی:** طول پیام سفارشی. در حال حاضر فقط برای پروتکل H2 جهت تعیین طول پیام باینری استفاده می‌شود.

### `[protocol].extended`
- **Scope:** `config`
- **English:** Enable extended functionality for the protocol. The reason it's disabled by default is that not all devices support it.
- **فارسی:** فعال‌سازی قابلیت‌های گسترده پروتکل. دلیل غیرفعال بودن پیش‌فرض این است که همهٔ دستگاه‌ها از آن پشتیبانی نمی‌کنند.

### `[protocol].utf8`
- **Scope:** `config`
- **English:** Decode string as UTF8 instead of ASCII. Only applicable for some protocols.
- **فارسی:** رمزگشایی رشته به صورت UTF8 به‌جای ASCII. فقط برای برخی پروتکل‌ها قابل استفاده است.

### `[protocol].can`
- **Scope:** `config`
- **English:** Enable CAN decoding for the protocol. Similar to 'extended' configuration, it's not supported for some devices.
- **فارسی:** فعال‌سازی رمزگشایی CAN برای پروتکل. مشابه تنظیم `extended`، برخی دستگاه‌ها از آن پشتیبانی نمی‌کنند.

### `[protocol].ack`
- **Scope:** `config`, `device`
- **Default:** `false`
- **English:** Indicates whether server acknowledgment is required. Only applicable for some protocols.
- **فارسی:** تعیین می‌کند که آیا تأییدیه سرور (ACK) لازم است یا خیر. فقط برای برخی پروتکل‌ها کاربرد دارد.

### `[protocol].ignoreFixTime`
- **Scope:** `config`
- **English:** Ignore device-reported fix time. Useful in case some devices report invalid time. Currently only available for GL200 protocol.
- **فارسی:** نادیده گرفتن زمان ارائه‌شده توسط دستگاه. در مواردی که دستگاه زمان نادرست گزارش می‌دهد مفید است. در حال حاضر فقط برای پروتکل GL200 موجود است.

### `[protocol].decodeLow`
- **Scope:** `config`
- **English:** Decode additional TK103 attributes. Not supported for some devices.
- **فارسی:** رمزگشایی ویژگی‌های اضافی TK103. بعضی دستگاه‌ها پشتیبانی نمی‌کنند.

### `[protocol].longDate`
- **Scope:** `config`
- **English:** Use long date format for Atrack protocol.
- **فارسی:** استفاده از فرمت تاریخ بلند برای پروتکل Atrack.

### `[protocol].decimalFuel`
- **Scope:** `config`
- **English:** Use decimal fuel value format for Atrack protocol.
- **فارسی:** استفاده از فرمت اعشاری برای مقدار سوخت در پروتکل Atrack.

### `[protocol].custom`
- **Scope:** `config`
- **English:** Indicates additional custom attributes for Atrack protocol.
- **فارسی:** ویژگی‌های سفارشی اضافی برای پروتکل Atrack.

### `[protocol].form`
- **Scope:** `config`
- **English:** Custom format string for Atrack protocol.
- **فارسی:** رشته قالب سفارشی برای پروتکل Atrack.

### `[protocol].frameMask`
- **Scope:** `config`
- **English:** Frame mask for Atrack protocol.
- **فارسی:** ماسک فریم برای پروتکل Atrack.

### `[protocol].config`
- **Scope:** `config`
- **English:** Protocol configuration. Required for some devices for decoding incoming data.
- **فارسی:** پیکربندی پروتکل. برای برخی دستگاه‌ها جهت رمزگشایی داده‌های ورودی ضروری است.

### `[protocol].alarmMap`
- **Scope:** `config`
- **English:** Alarm mapping for Atrack protocol.
- **فارسی:** نگاشت هشدار برای پروتکل Atrack.

### `[protocol].prefix`
- **Scope:** `config`
- **English:** Indicates whether TAIP protocol should have prefixes for messages.
- **فارسی:** تعیین می‌کند که آیا پروتکل TAIP باید پیشوندهایی برای پیام‌ها داشته باشد.

### `[protocol].server`
- **Scope:** `config`
- **English:** Some devices require server address confirmation. Use this parameter to configure correct public address.
- **فارسی:** برخی دستگاه‌ها نیاز به تأیید آدرس سرور دارند. با این پارامتر آدرس عمومی صحیح را تنظیم کنید.

### `[protocol].speed`
- **Scope:** `config`
- **English:** Speed units for the protocol. Possible values: knots (default), kmh, mps, mph.
- **فارسی:** واحد سرعت برای پروتکل. مقادیر ممکن: knots (پیش‌فرض), kmh, mps, mph.

### `[protocol].format0`
- **Scope:** `config`
- **Default:** `"TSPRXAB27GHKLMnaicz*U!"`
- **English:** Custom format string 0 for GlobalSat protocol.
- **فارسی:** رشته قالب سفارشی ۰ برای پروتکل GlobalSat.

### `[protocol].format1`
- **Scope:** `config`
- **Default:** `"SARY*U!"`
- **English:** Custom format string 1 for GlobalSat protocol.
- **فارسی:** رشته قالب سفارشی ۱ برای پروتکل GlobalSat.

### `[protocol].reportColumns`
- **Scope:** `config`
- **Default:** `"1,2,3,4"`
- **English:** Custom report columns for Genx protocol.
- **فارسی:** ستون‌های گزارش سفارشی برای پروتکل Genx.

### `suntech.protocolType`
- **Scope:** `config`, `device`
- **English:** Protocol type for Suntech.
- **فارسی:** نوع پروتکل برای Suntech.

### `suntech.hbm`
- **Scope:** `config`, `device`
- **English:** Suntech HBM configuration value.
- **فارسی:** مقدار پیکربندی HBM مربوط به Suntech.

### `[protocol].includeAdc`
- **Scope:** `config`, `device`
- **English:** Format includes ADC value.
- **فارسی:** قالب شامل مقدار ADC می‌شود.

### `[protocol].includeRpm`
- **Scope:** `config`, `device`
- **English:** Format includes RPM value.
- **فارسی:** قالب شامل مقدار RPM می‌شود.

### `[protocol].includeTemp`
- **Scope:** `config`, `device`
- **English:** Format includes temperature values.
- **فارسی:** قالب شامل مقادیر دما می‌شود.

### `[protocol].disableCommands`
- **Scope:** `config`
- **English:** Disable commands for the protocol. Not all protocols support this option.
- **فارسی:** غیرفعال کردن فرمان‌ها برای پروتکل. همهٔ پروتکل‌ها این گزینه را پشتیبانی نمی‌کنند.

### `[protocol].format`
- **Scope:** `config`, `device`
- **English:** Protocol format. Used by protocols that have configurable message format.
- **فارسی:** قالب پروتکل. توسط پروتکل‌هایی استفاده می‌شود که قالب پیام قابل تنظیم دارند.

### `[protocol].dateFormat`
- **Scope:** `device`
- **English:** Protocol date format. Used by protocols that have configurable date format.
- **فارسی:** قالب تاریخ پروتکل. برای پروتکل‌هایی که قالب تاریخ قابل تنظیم دارند.

### `decoder.timezone`
- **Scope:** `config`, `device`
- **English:** Device time zone. Most devices report UTC time, but in some cases devices report local time, so this parameter needs to be configured for the server to be able to decode the time correctly.
- **فارسی:** منطقه زمانی دستگاه. بیشتر دستگاه‌ها زمان UTC گزارش می‌دهند، اما در برخی موارد زمان محلی ارسال می‌شود. این پارامتر باید تنظیم شود تا سرور بتواند زمان را درست رمزگشایی کند.

### `orbcomm.accessId`
- **Scope:** `config`
- **English:** ORBCOMM API access id.
- **فارسی:** شناسه دسترسی API سرویس ORBCOMM.

### `orbcomm.password`
- **Scope:** `config`
- **English:** ORBCOMM API password.
- **فارسی:** رمز عبور API سرویس ORBCOMM.

### `smartcar.managementToken`
- **Scope:** `config`
- **English:** Smartcar management token used for webhook verification.
- **فارسی:** توکن مدیریت Smartcar که برای تأیید webhook استفاده می‌شود.

### `osmand.minAccuracy`
- **Scope:** `config`
- **Default:** `10.0`
- **English:** Minimum accuracy to include. If the value is lower, it will be set to zero.
- **فارسی:** حداقل دقت برای درج. اگر مقدار کمتر باشد، صفر در نظر گرفته می‌شود.

### `[protocol].alternative`
- **Scope:** `config`, `device`
- **Default:** `false`
- **English:** Use alternative format for the protocol of commands.
- **فارسی:** استفاده از قالب جایگزین برای پروتکل فرمان‌ها.

### `[protocol].language`
- **Scope:** `config`, `device`
- **Default:** `false`
- **English:** Protocol format includes a language field.
- **فارسی:** قالب پروتکل شامل یک فیلد زبان می‌شود.

---

## ۲. سرور و وضعیت (Server & Status)

### `server.buffering.threshold`
- **Scope:** `config`
- **Default:** `3000L`
- **English:** If not zero, enable buffering of incoming data to handle ordering locations. The value is threshold for buffering in milliseconds.
- **فارسی:** اگر صفر نباشد، بافرینگ داده‌های ورودی برای مرتب‌سازی موقعیت‌ها فعال می‌شود. مقدار آن آستانه بافرینگ به میلی‌ثانیه است.

### `server.timeout`
- **Scope:** `config`
- **Default:** `1800`
- **English:** Server wide connection timeout value in seconds. See protocol timeout for more information.
- **فارسی:** زمان timeout کلی اتصالات سرور به ثانیه. برای اطلاعات بیشتر به timeout پروتکل مراجعه کنید.

### `server.delayAcknowledgement`
- **Scope:** `config`
- **English:** Send device responses immediately before writing it in the database.
- **فارسی:** ارسال پاسخ به دستگاه بلافاصله، قبل از نوشتن در پایگاه داده.

### `server.nettyBossThreads`
- **Scope:** `config`
- **Default:** `0`
- **English:** Number of Netty boss threads. If not specified or zero, Netty default value is used.
- **فارسی:** تعداد نخ‌های boss در Netty. اگر مشخص نشود یا صفر باشد، مقدار پیش‌فرض Netty استفاده می‌شود.

### `server.nettyThreads`
- **Scope:** `config`
- **Default:** `0`
- **English:** Number of Netty worker threads. If not specified or zero, Netty default value is used.
- **فارسی:** تعداد نخ‌های worker در Netty. اگر مشخص نشود یا صفر باشد، مقدار پیش‌فرض Netty استفاده می‌شود.

### `server.statistics`
- **Scope:** `config`
- **Default:** `"https://www.traccar.org/analytics/"`
- **English:** Address for uploading aggregated anonymous usage statistics. Uploaded information is the same as what you can see on the statistics screen in the web app. It does not include any sensitive data (e.g. locations).
- **فارسی:** آدرس بارگذاری آمار مصرف تجمیعی و ناشناس. اطلاعات ارسالی همان چیزی است که در صفحه آمار وب‌اپ مشاهده می‌کنید. شامل داده‌های حساس (مانند موقعیت‌ها) نیست.

### `fuelDropThreshold`
- **Scope:** `server`, `device`
- **Default:** `0.0`
- **English:** Fuel drop threshold value. When fuel level drops from one position to another by more than this value, an event is generated.
- **فارسی:** آستانه کاهش سوخت. هنگامی که سطح سوخت بین دو موقعیت بیش از این مقدار کاهش یابد، رویداد تولید می‌شود.

### `fuelIncreaseThreshold`
- **Scope:** `server`, `device`
- **Default:** `0.0`
- **English:** Fuel increase threshold value. When fuel level increases from one position to another by more than this value, an event is generated.
- **فارسی:** آستانه افزایش سوخت. وقتی سطح سوخت بیش از این مقدار افزایش یابد، رویداد تولید می‌شود.

### `fuelCapacity`
- **Scope:** `device`
- **English:** Device fuel tank capacity in liters.
- **فارسی:** ظرفیت باک سوخت دستگاه به لیتر.

### `speedLimit`
- **Scope:** `server`, `device`
- **Default:** `0.0`
- **English:** Speed limit value in knots.
- **فارسی:** محدودیت سرعت به نات.

### `proximityEnterDistance`
- **Scope:** `device`
- **Default:** `0.0`
- **English:** Distance in meters at which a linked device entering this range triggers a proximity enter event. 0 to disable.
- **فارسی:** فاصله به متر که اگر دستگاه مرتبط وارد این محدوده شود رویداد ورود مجاورتی رخ می‌دهد. مقدار ۰ غیرفعال.

### `proximityExitDistance`
- **Scope:** `device`
- **Default:** `0.0`
- **English:** Distance in meters at which a linked device leaving this range triggers a proximity exit event. 0 to disable.
- **فارسی:** فاصله به متر که اگر دستگاه مرتبط از این محدوده خارج شود رویداد خروج مجاورتی رخ می‌دهد. ۰ غیرفعال.

### `unaccompaniedDistance`
- **Scope:** `device`
- **Default:** `0.0`
- **English:** Distance in meters that defines "near" for the unaccompanied motion event. If the device starts moving with no linked device within this distance, an event is generated. 0 to disable.
- **فارسی:** فاصله به متر که برای رویداد حرکت بدون همراه تعریف می‌شود. اگر دستگاه شروع به حرکت کند و هیچ دستگاه مرتبطی در این فاصله نباشد، رویداد تولید می‌شود. ۰ غیرفعال.

### `disableShare`
- **Scope:** `server`
- **English:** Disable device sharing on the server.
- **فارسی:** غیرفعال کردن اشتراک‌گذاری دستگاه در سرور.

### `status.timeout`
- **Scope:** `config`
- **Default:** `600L`
- **English:** If no data is reported by a device for the given amount of time, status changes from online to unknown. Value is in seconds. Default timeout is 10 minutes.
- **فارسی:** اگر دستگاهی برای این مدت داده‌ای گزارش نکند، وضعیت از آنلاین به نامشخص تغییر می‌کند. مقدار به ثانیه. پیش‌فرض ۱۰ دقیقه.

### `status.ignoreOffline`
- **Scope:** `config`
- **English:** List of protocol names to ignore offline status. Can be useful to not trigger status change when devices are configured to disconnect after reporting a batch of data.
- **فارسی:** فهرست نام پروتکل‌هایی که وضعیت آفلاین آنها نادیده گرفته شود. برای جلوگیری از تغییر وضعیت وقتی دستگاه‌ها پس از ارسال یک دسته داده قطع می‌شوند.

---

## ۳. پایگاه داده (Database)

### `database.memory`
- **Scope:** `config`
- **English:** Enable in-memory database instead of an SQL database.
- **فارسی:** استفاده از پایگاه داده درون‌حافظه‌ای به‌جای SQL.

### `database.driverFile`
- **Scope:** `config`
- **English:** Path to the database driver JAR file. Traccar includes drivers for MySQL, PostgreSQL and H2 databases. If you use one of those, you don't need to specify this parameter.
- **فارسی:** مسیر فایل JAR درایور پایگاه داده. Traccar درایورهای MySQL، PostgreSQL و H2 را به همراه دارد. در صورت استفاده از این‌ها نیازی به تنظیم این پارامتر نیست.

### `database.driver`
- **Scope:** `config`
- **English:** Database driver Java class. For H2 use 'org.h2.Driver'. MySQL driver class name is 'com.mysql.jdbc.Driver'.
- **فارسی:** کلاس جاوا درایور پایگاه داده. برای H2 از `org.h2.Driver` و برای MySQL از `com.mysql.jdbc.Driver` استفاده کنید.

### `database.url`
- **Scope:** `config`
- **English:** Database connection URL. By default Traccar uses H2 database.
- **فارسی:** URL اتصال به پایگاه داده. به‌طور پیش‌فرض Traccar از H2 استفاده می‌کند.

### `database.user`
- **Scope:** `config`
- **English:** Database user name. Default administrator user for H2 database is 'sa'.
- **فارسی:** نام کاربری پایگاه داده. کاربر مدیر پیش‌فرض برای H2 برابر `sa` است.
- **یادداشت چارت Helm:** این مقدار در ConfigMap به صورت `{{ env "DATABASE_USER" }}` درج می‌شود. مقدار واقعی از Secret یا متغیر محیطی تأمین می‌گردد.

### `database.password`
- **Scope:** `config`
- **English:** Database user password. Default password for H2 admin (sa) user is empty.
- **فارسی:** رمز عبور کاربر پایگاه داده. رمز پیش‌فرض کاربر `sa` در H2 خالی است.
- **یادداشت چارت Helm:** مانند `database.user`، از متغیر محیطی `DATABASE_PASSWORD` استفاده می‌کند.

### `database.changelog`
- **Scope:** `config`
- **Default:** `"./schema/changelog-master.xml"`
- **English:** Path to Liquibase master changelog file.
- **فارسی:** مسیر فایل changelog اصلی Liquibase.

### `database.maxPoolSize`
- **Scope:** `config`
- **Default:** `20`
- **English:** Database connection pool size.
- **فارسی:** اندازه مخزن اتصالات پایگاه داده.

### `database.checkConnection`
- **Scope:** `config`
- **Default:** `"SELECT 1"`
- **English:** SQL query to check connection status. Default value is 'SELECT 1'. For Oracle database you can use 'SELECT 1 FROM DUAL'.
- **فارسی:** پرس‌وجوی SQL برای بررسی وضعیت اتصال. پیش‌فرض `SELECT 1`. برای Oracle می‌توانید از `SELECT 1 FROM DUAL` استفاده کنید.

### `database.saveOriginal`
- **Scope:** `config`
- **English:** Store original HEX or string data as "raw" attribute in the corresponding position.
- **فارسی:** ذخیره دادهٔ خام HEX یا رشته‌ای به‌عنوان ویژگی "raw" در موقعیت مربوطه.

### `database.throttleUnknown`
- **Scope:** `config`
- **English:** Throttle unknown device database queries when it sends repeated requests.
- **فارسی:** محدود کردن پرس‌وجوهای پایگاه داده برای دستگاه‌های ناشناس در صورت درخواست‌های مکرر.

### `database.registerUnknown`
- **Scope:** `config`
- **English:** Automatically register unknown devices in the database.
- **فارسی:** ثبت خودکار دستگاه‌های ناشناس در پایگاه داده.

### `database.registerUnknown.defaultCategory`
- **Scope:** `config`
- **English:** Default category for auto-registered devices.
- **فارسی:** دسته‌بندی پیش‌فرض برای دستگاه‌های ثبت‌شده خودکار.

### `database.registerUnknown.defaultGroupId`
- **Scope:** `config`
- **English:** The group id assigned to auto-registered devices.
- **فارسی:** شناسه گروهی که به دستگاه‌های ثبت‌شده خودکار اختصاص می‌یابد.

### `database.registerUnknown.regex`
- **Scope:** `config`
- **English:** Automatically register unknown devices with regex filter.
- **فارسی:** ثبت خودکار دستگاه‌های ناشناس با فیلتر regex.

### `database.positionPeriod`
- **Scope:** `config`
- **English:** Limit latest position queries to a certain time period in seconds. This is useful for TimescaleDB to avoid scanning all time chunks. Default value is 7776000 seconds (90 days). Zero value disables the limit.
- **فارسی:** محدود کردن پرس‌وجوهای آخرین موقعیت به یک بازهٔ زمانی مشخص به ثانیه. برای TimescaleDB مفید است. پیش‌فرض ۷۷۷۶۰۰۰ ثانیه (۹۰ روز). مقدار صفر این محدودیت را غیرفعال می‌کند.

### `database.saveEmpty`
- **Scope:** `config`
- **English:** Store empty messages as positions. For example, heartbeats.
- **فارسی:** ذخیره پیام‌های خالی (مانند heartbeat) به‌عنوان موقعیت.

### `database.maxLifetime`
- **Scope:** `config`
- **English:** Maximum lifetime in milliseconds of a connection in the pool.
- **فارسی:** حداکثر طول عمر یک اتصال در مخزن، به میلی‌ثانیه.

### `database.positionBatchInterval`
- **Scope:** `config`
- **Default:** `0L`
- **English:** If not zero, enable batching of position inserts. The value is the flush interval in milliseconds; positions accumulated during this window are written as a single JDBC batch. Trades up to this much latency for higher throughput on busy servers.
- **فارسی:** اگر صفر نباشد، درج دسته‌ای موقعیت‌ها فعال می‌شود. مقدار، فاصلهٔ تخلیه به میلی‌ثانیه است. موقعیت‌هایی که در این بازه جمع شوند، به‌صورت یک دسته JDBC نوشته می‌شوند. تأخیر تا این اندازه را با توان عملیاتی بالاتر در سرورهای شلوغ معاوضه می‌کند.

### `database.positionBatchSize`
- **Scope:** `config`
- **Default:** `100`
- **English:** Maximum number of positions written in a single batch. Default value is 100, which is safe across all supported databases including SQL Server (with its 2100 parameter limit). Postgres and MySQL can typically handle much larger batches; raise this for higher drain rate on busy servers.
- **فارسی:** حداکثر تعداد موقعیت‌هایی که در یک دسته نوشته می‌شود. پیش‌فرض ۱۰۰ که برای تمام پایگاه‌های داده پشتیبانی‌شده از جمله SQL Server ایمن است. Postgres و MySQL می‌توانند دسته‌های بزرگتری را پردازش کنند؛ برای سرورهای شلوغ می‌توانید این مقدار را افزایش دهید.

---

## ۴. کاربران و LDAP

### `users.defaultDeviceLimit`
- **Scope:** `config`
- **Default:** `-1`
- **English:** Device limit for self registered users. Default value is -1, which indicates no limit.
- **فارسی:** محدودیت تعداد دستگاه برای کاربران ثبت‌نام‌شده خودکار. ۱- یعنی بدون محدودیت.

### `users.defaultExpirationDays`
- **Scope:** `config`
- **English:** Default user expiration for self registered users. Value is in days. By default no expiration is set.
- **فارسی:** تاریخ انقضای پیش‌فرض کاربران خودکار (به روز). به‌طور پیش‌فرض بدون انقضا.

### `ldap.url`
- **Scope:** `config`
- **English:** LDAP server URL.
- **فارسی:** URL سرور LDAP.

### `ldap.user`
- **Scope:** `config`
- **English:** LDAP server login.
- **فارسی:** نام کاربری ورود به LDAP.

### `ldap.password`
- **Scope:** `config`
- **English:** LDAP server password.
- **فارسی:** رمز عبور LDAP.

### `ldap.force`
- **Scope:** `config`
- **English:** Force LDAP authentication.
- **فارسی:** احراز هویت اجباری از طریق LDAP.

### `ldap.base`
- **Scope:** `config`
- **English:** LDAP user search base.
- **فارسی:** پایگاه جستجوی کاربر در LDAP.

### `ldap.idAttribute`
- **Scope:** `config`
- **Default:** `"uid"`
- **English:** LDAP attribute used as user id. Default value is 'uid'.
- **فارسی:** ویژگی LDAP که به‌عنوان شناسه کاربر استفاده می‌شود. پیش‌فرض `uid`.

### `ldap.nameAttribute`
- **Scope:** `config`
- **Default:** `"cn"`
- **English:** LDAP attribute used as username. Default value is 'cn'.
- **فارسی:** ویژگی LDAP برای نام کاربری. پیش‌فرض `cn`.

### `ldap.mailAttribute`
- **Scope:** `config`
- **Default:** `"mail"`
- **English:** LDAP attribute used as user email. Default value is 'mail'.
- **فارسی:** ویژگی LDAP برای ایمیل کاربر. پیش‌فرض `mail`.

### `ldap.searchFilter`
- **Scope:** `config`
- **English:** LDAP custom search filter. If not specified, '({idAttribute}=:login)' will be used as a filter.
- **فارسی:** فیلتر جستجوی سفارشی LDAP. اگر مشخص نشود از `({idAttribute}=:login)` استفاده می‌شود.

### `ldap.adminFilter`
- **Scope:** `config`
- **English:** LDAP custom admin search filter.
- **فارسی:** فیلتر جستجوی سفارشی برای مدیران در LDAP.

### `ldap.adminGroup`
- **Scope:** `config`
- **English:** LDAP admin user group. Used if custom admin filter is not specified.
- **فارسی:** گروه کاربری مدیران در LDAP. اگر فیلتر سفارشی مدیران مشخص نشده باشد استفاده می‌شود.

---

## ۵. OpenID Connect

### `openid.clients`
- **Scope:** `config`
- **English:** List of OpenID Connect clients for the built-in provider. Value should be a comma-separated list of 'clientId:clientSecret:redirectUri' entries. Multiple redirect URIs can be specified using '|' as a separator.
- **فارسی:** فهرست کلاینت‌های OpenID Connect برای ارائه‌دهنده داخلی. باید با فرمت `clientId:clientSecret:redirectUri` و با کاما جدا شوند. چند redirect URI را می‌توان با `|` جدا کرد.

### `openid.force`
- **Scope:** `config`
- **English:** Force OpenID Connect authentication. When enabled, the Traccar login page will be skipped and users are redirected to the OpenID Connect provider.
- **فارسی:** احراز هویت اجباری OpenID Connect. در این صورت صفحه ورود Traccar رد شده و کاربران به ارائه‌دهنده OpenID هدایت می‌شوند.

### `openid.allowRegistration`
- **Scope:** `config`
- **English:** Allow new users authenticated via OpenID Connect to be auto-created even when the server registration setting is disabled. When false, OpenID logins for unknown users are rejected unless the server has registration enabled.
- **فارسی:** اجازه ایجاد خودکار کاربران احراز هویت‌شده با OpenID حتی اگر ثبت‌نام در سرور غیرفعال باشد. اگر false باشد، کاربران ناشناس OpenID رد می‌شوند مگر اینکه ثبت‌نام سرور فعال باشد.

### `openid.clientId`
- **Scope:** `config`
- **English:** OpenID Connect Client ID. This is a unique ID assigned to each application you register with your identity provider. Required to enable SSO.
- **فارسی:** شناسه کلاینت OpenID Connect. شناسه یکتایی که از ارائه‌دهنده هویت دریافت می‌کنید. برای فعال‌سازی SSO ضروری است.

### `openid.clientSecret`
- **Scope:** `config`
- **English:** OpenID Connect Client Secret. This is a secret assigned to each application you register with your identity provider. Required to enable SSO.
- **فارسی:** رمز کلاینت OpenID Connect. رمزی که ارائه‌دهنده هویت به برنامه شما اختصاص می‌دهد. برای SSO ضروری است.

### `openid.issuerUrl`
- **Scope:** `config`
- **English:** OpenID Connect Issuer (Base) URL. This is used to automatically configure the authorization, token and user info URLs if provided.
- **فارسی:** URL صادرکننده OpenID Connect (آدرس پایه). برای پیکربندی خودکار سایر آدرس‌ها استفاده می‌شود.

### `openid.authUrl`
- **Scope:** `config`
- **English:** OpenID Connect Authorization URL.
- **فارسی:** URL مجوزدهی OpenID Connect.

### `openid.tokenUrl`
- **Scope:** `config`
- **English:** OpenID Connect Token URL.
- **فارسی:** URL توکن OpenID Connect.

### `openid.userInfoUrl`
- **Scope:** `config`
- **English:** OpenID Connect User Info URL.
- **فارسی:** URL اطلاعات کاربر OpenID Connect.

### `openid.groupsClaimName`
- **Scope:** `config`
- **Default:** `"groups"`
- **English:** OpenID Connect group scope claim name. If this is not provided, Traccar will use the "groups" scope name.
- **فارسی:** نام claim مربوط به گروه در OpenID. در صورت عدم تنظیم، Traccar از `groups` استفاده می‌کند.

### `openid.allowGroup`
- **Scope:** `config`
- **English:** OpenID Connect group to restrict access to. If this is not provided, all OpenID users will have access to Traccar.
- **فارسی:** گروه OpenID که دسترسی به آن محدود شود. اگر تنظیم نشود، همه کاربران OpenID به Traccar دسترسی خواهند داشت.

### `openid.adminGroup`
- **Scope:** `config`
- **English:** OpenID Connect group to grant admin access. If this is not provided, no groups will be granted admin access.
- **فارسی:** گروه OpenID که دسترسی مدیر به آن داده شود. در غیر این صورت هیچ گروهی دسترسی مدیر نخواهد داشت.

---

## ۶. رابط وب (Web Interface)

### `web.address`
- **Scope:** `config`
- **English:** Optional parameter to specify a network interface for the web interface to bind to. By default, the server will bind to all available interfaces.
- **فارسی:** پارامتر اختیاری برای مشخص کردن رابط شبکه‌ای که رابط وب به آن متصل شود. به‌طور پیش‌فرض سرور به همه رابط‌ها متصل می‌شود.

### `web.port`
- **Scope:** `config`
- **Default:** `8082`
- **English:** Web interface TCP port number. By default, Traccar uses port 8082. To avoid specifying port in the browser you can set it to 80 (default HTTP port).
- **فارسی:** شماره پورت TCP رابط وب. پیش‌فرض ۸۰۸۲. برای حذف نیاز به نوشتن پورت در مرورگر، می‌توانید آن را ۸۰ قرار دهید.

### `web.path`
- **Scope:** `config`
- **Default:** `"./web"`
- **English:** Path to the web app folder.
- **فارسی:** مسیر پوشه برنامه وب.

### `web.override`
- **Scope:** `config`
- **Default:** `"./override"`
- **English:** Path to a folder with overrides. It can be used for branding to keep custom logos in a separate place.
- **فارسی:** مسیر پوشه فایل‌های بازنویسی (override). می‌توانید برای برندسازی و لوگوهای سفارشی استفاده کنید.

### `web.timeout`
- **Scope:** `config`
- **Default:** `300000L`
- **English:** WebSocket connection timeout in milliseconds. Default timeout is 5 minutes.
- **فارسی:** زمان timeout اتصال WebSocket به میلی‌ثانیه. پیش‌فرض ۵ دقیقه.

### `web.sessionTimeout`
- **Scope:** `config`
- **English:** Authentication session timeout in seconds. By default, there is no timeout.
- **فارسی:** زمان timeout نشست احراز هویت به ثانیه. به‌طور پیش‌فرض نامحدود.

### `web.console`
- **Scope:** `config`
- **English:** Enable database access console via '/console' URL. Use only for debugging. Never use in production.
- **فارسی:** فعال‌سازی کنسول دسترسی به پایگاه داده از طریق `/console`. فقط برای اشکال‌زدایی. هرگز در محیط عملیاتی استفاده نکنید.

### `web.debug`
- **Scope:** `config`
- **English:** Server debug version of the web app. Not recommended to use for performance reasons.
- **فارسی:** نسخه اشکال‌زدایی برنامه وب. به دلایل کارایی توصیه نمی‌شود.

### `web.serviceAccountToken`
- **Scope:** `config`
- **English:** A token to log in as a virtual admin account. Can be used to restore access in case of issues with regular admin login.
- **فارسی:** توکنی برای ورود به‌عنوان حساب مدیر مجازی. برای بازیابی دسترسی در صورت بروز مشکل با ورود عادی مدیر.

### `web.origin`
- **Scope:** `config`
- **English:** Cross-origin resource sharing origin header value.
- **فارسی:** مقدار هدر Origin برای CORS.

### `web.cacheControl`
- **Scope:** `config`
- **Default:** `"max-age=3600,public"`
- **English:** Cache control header value. By default, resources are cached for one hour.
- **فارسی:** مقدار هدر کنترل کش. به‌طور پیش‌فرض منابع یک ساعت کش می‌شوند.

### `web.localizationPath`
- **Scope:** `config`
- **Default:** `"./templates/translations"`
- **English:** Path to localization files.
- **فارسی:** مسیر فایل‌های بومی‌سازی.

### `web.url`
- **Scope:** `config`
- **English:** Public URL for the web app. Used for notifications, report links, and OpenID Connect. If not provided, Traccar will attempt to get a URL from the server IP address, but it might be a local address.
- **فارسی:** آدرس عمومی برنامه وب. برای اعلان‌ها، لینک گزارش‌ها و OpenID Connect استفاده می‌شود. اگر تنظیم نشود، Traccar سعی می‌کند از IP سرور آدرس بسازد که ممکن است محلی باشد.

### `web.showUnknownDevices`
- **Scope:** `config`
- **English:** Show logs from unknown devices.
- **فارسی:** نمایش لاگ‌های دستگاه‌های ناشناس.

### `web.shareDevice.commands`
- **Scope:** `config`
- **English:** Enable commands for a shared device.
- **فارسی:** فعال‌سازی فرمان‌ها برای دستگاه به‌اشتراک‌گذاشته‌شده.

### `web.shareDevice.reports`
- **Scope:** `config`
- **English:** Enable reports for a shared device.
- **فارسی:** فعال‌سازی گزارش‌ها برای دستگاه به‌اشتراک‌گذاشته‌شده.

### `web.mcp.enable`
- **Scope:** `config`
- **English:** Enable MCP service.
- **فارسی:** فعال‌سازی سرویس MCP.

### `totpEnable`
- **Scope:** `server`
- **English:** Enable TOTP authentication on the server.
- **فارسی:** فعال‌سازی احراز هویت TOTP در سرور.

### `totpForce`
- **Scope:** `server`
- **English:** Server attribute that indicates that TOTP authentication is required for new users.
- **فارسی:** ویژگی سرور که نشان‌دهنده اجباری بودن TOTP برای کاربران جدید است.

---

## ۷. رسانه و فرمان‌ها (Media & Commands)

### `media.path`
- **Scope:** `config`
- **Default:** `"./media"`
- **English:** Path to the media folder. Server stores audio, video and photo files in that folder.
- **فارسی:** مسیر پوشه رسانه‌ها (عکس، ویدئو، صدا). زیرپوشه‌ها به‌ازای هر دستگاه با شناسه یکتا ساخته می‌شوند.

### `command.sender`
- **Scope:** `device`
- **English:** Command sender type for the device. This overrides standard data or text commands with API-based commands.
- **فارسی:** نوع ارسال‌کننده فرمان برای دستگاه. این تنظیم جایگزین فرمان‌های داده‌ای یا متنی استاندارد با فرمان‌های مبتنی بر API می‌شود.

### `command.client.serviceAccount`
- **Scope:** `config`
- **English:** Firebase service account JSON for push commands.
- **فارسی:** JSON حساب سرویس Firebase برای فرمان‌های push.

### `command.findHub.url`
- **Scope:** `device`
- **English:** Google Find Hub service URL.
- **فارسی:** URL سرویس Google Find Hub.

### `command.findHub.key`
- **Scope:** `device`
- **English:** Google Find Hub service API key.
- **فارسی:** کلید API سرویس Google Find Hub.

---

## ۸. اعلان‌ها (Notifications)

### `notificator.types`
- **Scope:** `config`
- **Default:** `"web,mail,command"`
- **English:** Enabled notification options. Comma-separated string is expected. Example: web,mail,sms
- **فارسی:** گزینه‌های فعال اعلان. با کاما جدا شوند. مثال: `web,mail,sms`

### `notificator.timeThreshold`
- **Scope:** `config`
- **Default:** `15 * 60 * 1000L` (15 دقیقه)
- **English:** If the event time is too old, we should not send notifications. This parameter is the threshold value in milliseconds. Default value is 15 minutes.
- **فارسی:** اگر زمان رویداد بیش از این حد قدیمی باشد، اعلان ارسال نشود. مقدار به میلی‌ثانیه. پیش‌فرض ۱۵ دقیقه.

### `notificator.traccar.key`
- **Scope:** `config`
- **English:** Traccar notification API key.
- **فارسی:** کلید API اعلان‌های Traccar.

### `notificator.firebase.serviceAccount`
- **Scope:** `config`
- **English:** Firebase service account JSON for push notifications.
- **فارسی:** JSON حساب سرویس Firebase برای اعلان‌های push.

### `notificator.firebase.mode`
- **Scope:** `config`
- **Default:** `"direct"`
- **English:** Firebase message delivery mode. Supported values: `direct` (notification payload only), `data` (data-only payload), `mixed` (both).
- **فارسی:** حالت تحویل پیام Firebase. مقادیر: `direct` (فقط بدنه اعلان)، `data` (فقط داده)، `mixed` (هر دو).

### `notificator.pushover.user`
- **Scope:** `config`
- **English:** Pushover notification user name.
- **فارسی:** نام کاربری Pushover.

### `notificator.pushover.token`
- **Scope:** `config`
- **English:** Pushover notification user token.
- **فارسی:** توکن کاربر Pushover.

### `notificator.telegram.key`
- **Scope:** `config`
- **English:** Telegram notification API key.
- **فارسی:** کلید API ربات تلگرام.

### `notificator.telegram.chatId`
- **Scope:** `config`
- **English:** Telegram notification chat id to post messages to.
- **فارسی:** شناسه چت تلگرام برای ارسال پیام‌ها.

### `notificator.telegram.sendLocation`
- **Scope:** `config`, `user`
- **English:** Telegram notification send location message.
- **فارسی:** ارسال پیام موقعیت مکانی در تلگرام.

### `notificator.telegram.proxy.url`
- **Scope:** `config`
- **English:** Telegram notification proxy URL.
- **فارسی:** URL پروکسی برای اعلان‌های تلگرام.

### `notificator.whatsapp.token`
- **Scope:** `config`
- **English:** WhatsApp Cloud API permanent access token.
- **فارسی:** توکن دسترسی دائمی WhatsApp Cloud API.

### `notificator.whatsapp.phoneNumberId`
- **Scope:** `config`
- **English:** WhatsApp Cloud API phone number id.
- **فارسی:** شناسه شماره تلفن در WhatsApp Cloud API.

### `notificator.whatsapp.templateName`
- **Scope:** `config`
- **English:** WhatsApp Cloud API template name.
- **فارسی:** نام قالب در WhatsApp Cloud API.

### `notificator.whatsapp.templateLanguage`
- **Scope:** `config`
- **Default:** `"en_US"`
- **English:** WhatsApp Cloud API template language code.
- **فارسی:** کد زبان قالب WhatsApp Cloud API.

### `notification.expiration.user`
- **Scope:** `config`
- **English:** Enable user expiration email notification.
- **فارسی:** فعال‌سازی اعلان ایمیل انقضای کاربر.

### `notification.expiration.user.reminder`
- **Scope:** `config`
- **English:** User expiration reminder. Value in milliseconds.
- **فارسی:** یادآوری انقضای کاربر (میلی‌ثانیه).

### `notification.expiration.device`
- **Scope:** `config`
- **English:** Enable device expiration email notification.
- **فارسی:** فعال‌سازی اعلان ایمیل انقضای دستگاه.

### `notification.expiration.device.reminder`
- **Scope:** `config`
- **English:** Device expiration reminder. Value in milliseconds.
- **فارسی:** یادآوری انقضای دستگاه (میلی‌ثانیه).

### `notification.block.users`
- **Scope:** `config`
- **English:** Block notifications for specific users. The value should be a comma-separated list of internal user ids.
- **فارسی:** مسدود کردن اعلان‌ها برای کاربران خاص. لیست شناسه‌های داخلی کاربران که با کاما جدا می‌شوند.

---

## ۹. گزارش‌ها (Reports)

### `report.periodLimit`
- **Scope:** `config`
- **English:** Maximum time period for reports in seconds. Can be useful to prevent users from requesting unreasonably long reports. By default, there is no limit.
- **فارسی:** حداکثر بازه زمانی مجاز برای گزارش‌ها به ثانیه. برای جلوگیری از درخواست گزارش‌های بسیار طولانی. به‌طور پیش‌فرض بدون محدودیت.

### `report.fastThreshold`
- **Scope:** `config`
- **Default:** `86400L` (یک روز)
- **English:** Time threshold for fast reports. Fast reports are more efficient, but less accurate and missing some information. The value is in seconds. One day by default.
- **فارسی:** آستانه زمانی برای گزارش‌های سریع. گزارش‌های سریع کارآمدتر اما با دقت کمتر و اطلاعات ناقص هستند. مقدار به ثانیه. پیش‌فرض یک روز.

### `report.trip.newLogic`
- **Scope:** `config`
- **Default:** `true`
- **English:** Enable new trips calculation logic.
- **فارسی:** فعال‌سازی منطق جدید محاسبه سفرها.

### `report.trip.minDistance`
- **Scope:** `config`, `device`
- **Default:** `200L`
- **English:** Distances above the minimum are considered trips.
- **فارسی:** مسافت‌های بالاتر از این حد به‌عنوان سفر در نظر گرفته می‌شوند.

### `report.trip.minDuration`
- **Scope:** `config`, `device`
- **Default:** `180L`
- **English:** If the device doesn't move for the minimum duration, it is considered a stop.
- **فارسی:** اگر دستگاه برای این مدت حرکت نکند، توقف محسوب می‌شود.

### `report.trip.stopGap`
- **Scope:** `config`, `device`
- **Default:** `3600L` (یک ساعت)
- **English:** Gaps of more than specified time are treated as stop/trip/stop based on average speed.
- **فارسی:** وقفه‌های بیشتر از این زمان بر اساس سرعت متوسط به‌عنوان توقف/سفر/توقف در نظر گرفته می‌شوند.

### `report.trip.minimalTripDistance`
- **Scope:** `config`, `device`
- **Default:** `500L`
- **English:** Trips less than minimal duration and minimal distance are ignored. 300 seconds and 500 meters are default.
- **فارسی:** سفرهایی با مسافت کمتر از این مقدار نادیده گرفته می‌شوند (پیش‌فرض ۵۰۰ متر).

### `report.trip.minimalTripDuration`
- **Scope:** `config`, `device`
- **Default:** `300L`
- **English:** Trips less than minimal duration and minimal distance are ignored. 300 seconds and 500 meters are default.
- **فارسی:** سفرهایی با مدت کمتر از این مقدار نادیده گرفته می‌شوند (پیش‌فرض ۳۰۰ ثانیه).

### `report.trip.minimalParkingDuration`
- **Scope:** `config`, `device`
- **Default:** `300L`
- **English:** Parking shorter than the minimum duration does not split a trip. Default is 300 seconds.
- **فارسی:** توقف‌های کوتاه‌تر از این مدت، سفر را تقسیم نمی‌کنند. پیش‌فرض ۳۰۰ ثانیه.

### `report.trip.minimalNoDataDuration`
- **Scope:** `config`, `device`
- **Default:** `3600L`
- **English:** Gaps of more than specified time are counted as stops. Default value is one hour.
- **فارسی:** وقفه‌های بیش از این زمان به‌عنوان توقف محسوب می‌شوند. پیش‌فرض یک ساعت.

### `report.trip.useIgnition`
- **Scope:** `config`, `device`
- **Default:** `false`
- **English:** Flag to enable ignition use for trips calculation.
- **فارسی:** پرچم استفاده از وضعیت سوئیچ (ignition) در محاسبه سفرها.

### `report.ignoreOdomometer`
- **Scope:** `config`, `device`
- **Default:** `false`
- **English:** Ignore odometer value reported by the device and use server-calculated total distance instead.
- **فارسی:** نادیده گرفتن کیلومترشمار دستگاه و استفاده از مسافت محاسبه‌شده توسط سرور.

---

## ۱۰. فیلترها (Filters)

### `filter.enable`
- **Scope:** `config`
- **Default:** `true`
- **English:** Boolean flag to enable or disable position filtering.
- **فارسی:** پرچم فعال/غیرفعال کردن فیلترینگ موقعیت.

### `filter.invalid`
- **Scope:** `config`, `device`
- **English:** Filter invalid (valid field is set to false) positions.
- **فارسی:** فیلتر موقعیت‌های نامعتبر (فیلد valid برابر false).

### `filter.zero`
- **Scope:** `config`, `device`
- **English:** Filter zero coordinates. Zero latitude and longitude are theoretically valid values, but in practice they usually indicate invalid GPS data.
- **فارسی:** فیلتر مختصات صفر. از نظر تئوری معتبرند اما در عمل نشان‌دهنده داده نامعتبر GPS هستند.

### `filter.duplicate`
- **Scope:** `config`, `device`
- **English:** Filter duplicate records (duplicates are detected by time value).
- **فارسی:** فیلتر رکوردهای تکراری (بر اساس زمان).

### `filter.outdated`
- **Scope:** `config`, `device`
- **English:** Filter messages that do not have GPS location. If they are not filtered, they will include the last known location.
- **فارسی:** فیلتر پیام‌های بدون موقعیت GPS. در غیر این صورت آخرین موقعیت شناخته‌شده جایگزین می‌شود.

### `filter.future`
- **Scope:** `config`, `device`
- **Default:** `86400L`
- **English:** Filter records with fix time in the future. The value is in seconds.
- **فارسی:** فیلتر رکوردهایی که زمان آنها از زمان سرور جلوتر است (تا ثانیه مشخص).

### `filter.past`
- **Scope:** `config`, `device`
- **English:** Filter records with fix time in the past. Value in seconds.
- **فارسی:** فیلتر رکوردهایی که زمان آنها بیش از حد از زمان سرور عقب‌تر است.

### `filter.accuracy`
- **Scope:** `config`, `device`
- **English:** Filter positions with accuracy less than specified value in meters.
- **فارسی:** فیلتر موقعیت‌هایی که دقت آنها کمتر از مقدار مشخص (متر) است.

### `filter.approximate`
- **Scope:** `config`, `device`
- **English:** Filter cell and wifi locations that are coming from geolocation provider.
- **فارسی:** فیلتر موقعیت‌های تقریبی (سلولی و Wi-Fi) که از سرویس مکان‌یابی می‌آیند.

### `filter.static`
- **Scope:** `config`, `device`
- **English:** Filter positions with exactly zero speed values.
- **فارسی:** فیلتر موقعیت‌هایی که سرعت دقیقاً صفر دارند.

### `filter.distance`
- **Scope:** `config`, `device`
- **English:** Filter records by distance. Value in meters. If the new position is closer than this value to the last one, it gets filtered out.
- **فارسی:** فیلتر بر اساس فاصله (متر). اگر موقعیت جدید از این مقدار به موقعیت قبلی نزدیک‌تر باشد، حذف می‌شود.

### `filter.maxSpeed`
- **Scope:** `config`, `device`
- **English:** Filter records by Maximum Speed value in knots. Can be used to filter jumps to far locations.
- **فارسی:** فیلتر بر اساس حداکثر سرعت (نات). برای حذف پرش‌های ناگهانی به موقعیت‌های دور.

### `filter.minPeriod`
- **Scope:** `config`, `device`
- **English:** Filter position if time from previous position is less than specified value in seconds.
- **فارسی:** فیلتر موقعیت اگر فاصله زمانی با موقعیت قبلی کمتر از این مقدار (ثانیه) باشد.

### `filter.dailyLimit`
- **Scope:** `config`, `device`
- **English:** Throttle positions if the daily limit is exceeded for the device.
- **فارسی:** محدود کردن تعداد موقعیت‌ها در صورت عبور از حد مجاز روزانه.

### `filter.dailyLimitInterval`
- **Scope:** `config`, `device`
- **English:** Throttling interval if the limit is exceeded. Value in seconds.
- **فارسی:** فاصله زمانی اعمال محدودیت پس از عبور از حد (ثانیه).

### `filter.skipLimit`
- **Scope:** `config`, `device`
- **English:** Time limit for filtering in seconds. If the time difference between last received position and new position is greater than this limit, the new position will not be filtered out.
- **فارسی:** محدودیت زمانی برای اعمال فیلترها (ثانیه). اگر اختلاف زمان آخرین موقعیت و موقعیت جدید بیش از این باشد، فیلتر نمی‌شود.

### `filter.skipAttributes.enable`
- **Scope:** `config`, `device`
- **English:** Enable attributes skipping.
- **فارسی:** فعال‌سازی رد شدن از فیلتر در صورت وجود ویژگی‌های خاص.

### `filter.skipAttributes`
- **Scope:** `config`, `device`
- **Default:** `""`
- **English:** Attribute skipping can be enabled in the config or device attributes. If position contains any attribute mentioned in "filter.skipAttributes" config key, position is not filtered out.
- **فارسی:** اگر موقعیت شامل یکی از ویژگی‌های لیست‌شده در این کلید باشد، فیلتر نخواهد شد.

---

## ۱۱. پردازش زمان و مختصات (Time & Coordinate Processing)

### `time.override`
- **Scope:** `config`
- **English:** Override device time. Possible values are 'deviceTime' and 'serverTime'
- **فارسی:** بازنویسی زمان دستگاه. مقادیر: `deviceTime` یا `serverTime`.

### `time.protocols`
- **Scope:** `config`
- **English:** List of protocols for overriding time. If not specified, override is applied globally.
- **فارسی:** فهرست پروتکل‌هایی که زمان آنها بازنویسی شود. در غیر این صورت برای همه اعمال می‌شود.

### `coordinates.filter`
- **Scope:** `config`
- **English:** Replaces coordinates with the last known coordinates if the change is less than `coordinates.minError` meters or more than `coordinates.maxError` meters.
- **فارسی:** جایگزینی مختصات با آخرین مختصات معتبر اگر تغییر کمتر از `minError` یا بیشتر از `maxError` باشد.

### `coordinates.minError`
- **Scope:** `config`
- **English:** Distance in meters. Distances below this value get handled as explained in `coordinates.filter`.
- **فارسی:** فاصله به متر. تغییرات کمتر از این مقدار طبق `coordinates.filter` تصحیح می‌شوند.

### `coordinates.maxError`
- **Scope:** `config`
- **English:** Distance in meters. Distances above this value get handled as explained in `coordinates.filter`.
- **فارسی:** فاصله به متر. تغییرات بیشتر از این مقدار تصحیح می‌شوند.

---

## ۱۲. پردازش موقعیت (Position Processing)

### `processing.remoteAddress.enable`
- **Scope:** `config`
- **English:** Enable saving device IP address information. Disabled by default.
- **فارسی:** فعال‌سازی ذخیره اطلاعات IP دستگاه (پیش‌فرض غیرفعال).

### `processing.useLinkedDriver`
- **Scope:** `config`
- **English:** Use linked driver id for positions if a device does not send driver id.
- **فارسی:** استفاده از شناسه راننده متصل در صورت عدم ارسال توسط دستگاه.

### `processing.copyAttributes.enable`
- **Scope:** `config`
- **English:** Enable copying of missing attributes from last position to the current one.
- **فارسی:** فعال‌سازی کپی ویژگی‌های مفقود از آخرین موقعیت به موقعیت جاری.

### `processing.copyAttributes`
- **Scope:** `config`, `device`
- **English:** List of attributes to copy. Comma-separated, e.g., alarm,ignition
- **فارسی:** فهرست ویژگی‌هایی که کپی شوند. با کاما جدا شوند.

### `processing.computedAttributes.deviceAttributes`
- **Scope:** `config`
- **English:** Include device attributes in the computed attribute context.
- **فارسی:** گنجاندن ویژگی‌های دستگاه در زمینه محاسبات.

### `processing.computedAttributes.lastAttributes`
- **Scope:** `config`
- **English:** Include last position attributes in the computed attribute context.
- **فارسی:** گنجاندن ویژگی‌های آخرین موقعیت.

### `processing.computedAttributes.localVariables`
- **Scope:** `config`
- **English:** Enable local variables declaration.
- **فارسی:** فعال‌سازی تعریف متغیرهای محلی.

### `processing.computedAttributes.loops`
- **Scope:** `config`
- **English:** Enable loop processing.
- **فارسی:** فعال‌سازی پردازش حلقه‌ها.

### `processing.computedAttributes.newInstanceCreation`
- **Scope:** `config`
- **English:** Enable new instance creation. When disabled, parsing a script/expression using 'new(...)' will throw a parsing exception.
- **فارسی:** فعال‌سازی ایجاد نمونه جدید. در صورت غیرفعال بودن، استفاده از `new(...)` خطا می‌دهد.

---

## ۱۳. ژئوکدینگ معکوس (Reverse Geocoding)

### `geocoder.enable`
- **Scope:** `config`
- **Default:** `true`
- **English:** Boolean flag to enable or disable reverse geocoder.
- **فارسی:** فعال/غیرفعال کردن ژئوکدینگ معکوس.

### `geocoder.type`
- **Scope:** `config`
- **Default:** `"locationiq"`
- **English:** Reverse geocoder type.
- **فارسی:** نوع سرویس ژئوکدینگ معکوس.

### `geocoder.url`
- **Scope:** `config`
- **English:** Geocoder server URL. Applicable only to Nominatim and Gisgraphy providers.
- **فارسی:** URL سرور ژئوکدینگ (فقط برای Nominatim و Gisgraphy).

### `geocoder.key`
- **Scope:** `config`
- **Default:** `"pk.689d849289c8c63708068b2ff1f63b2d"` (کلید عمومی LocationIQ)
- **English:** Provider API key. Most providers require API keys.
- **فارسی:** کلید API سرویس دهنده.

### `geocoder.language`
- **Scope:** `config`
- **English:** Language parameter for providers that support localization.
- **فارسی:** پارامتر زبان برای سرویس‌دهندگانی که بومی‌سازی را پشتیبانی می‌کنند.

### `geocoder.format`
- **Scope:** `config`
- **English:** Address format string. Default value is %h %r, %t, %s, %c.
- **فارسی:** رشته قالب آدرس. پیش‌فرض `%h %r, %t, %s, %c`.

### `geocoder.cacheSize`
- **Scope:** `config`
- **English:** Cache size for geocoding results.
- **فارسی:** اندازه کش برای نتایج ژئوکدینگ.

### `geocoder.ignorePositions`
- **Scope:** `config`
- **Default:** `true`
- **English:** Disable automatic reverse geocoding requests for all positions.
- **فارسی:** غیرفعال کردن درخواست خودکار ژئوکدینگ معکوس برای همه موقعیت‌ها.

### `geocoder.reuseDistance`
- **Scope:** `config`
- **English:** Minimum distance for new reverse geocoding request. If less than this (meters), last known address is reused.
- **فارسی:** حداقل فاصله (متر) برای درخواست جدید. اگر کمتر باشد، آخرین آدرس استفاده می‌شود.

### `geocoder.onRequest`
- **Scope:** `config`
- **Default:** `true`
- **English:** Perform geocoding when preparing reports and sending notifications.
- **فارسی:** انجام ژئوکدینگ هنگام تهیه گزارش و ارسال اعلان.

---

## ۱۴. تطبیق نقشه (Map Matching)

### `mapMatcher.enable`
- **Scope:** `config`
- **English:** Boolean flag to enable map matcher.
- **فارسی:** فعال‌سازی تطبیق نقشه.

### `mapMatcher.type`
- **Scope:** `config`
- **Default:** `"traccar"`
- **English:** Map matcher provider type. Currently only "traccar" is supported.
- **فارسی:** نوع سرویس تطبیق نقشه. فعلاً فقط `traccar`.

### `mapMatcher.url`
- **Scope:** `config`
- **English:** Map matcher service URL.
- **فارسی:** URL سرویس تطبیق نقشه.

### `mapMatcher.key`
- **Scope:** `config`
- **English:** Map matcher provider API key.
- **فارسی:** کلید API سرویس تطبیق نقشه.

---

## ۱۵. موقعیت‌یابی مبتنی بر شبکه (LBS/Geolocation)

### `geolocation.enable`
- **Scope:** `config`
- **English:** Boolean flag to enable LBS location resolution.
- **فارسی:** فعال‌سازی تشخیص موقعیت از طریق شبکه (دکل مخابراتی و Wi-Fi).

### `geolocation.type`
- **Scope:** `config`
- **English:** Provider to use for LBS location. Options: google, unwired, opencellid.
- **فارسی:** ارائه‌دهنده LBS. گزینه‌ها: `google`, `unwired`, `opencellid`.

### `geolocation.url`
- **Scope:** `config`
- **English:** Geolocation provider API URL address.
- **فارسی:** آدرس API ارائه‌دهنده موقعیت‌یابی.

### `geolocation.key`
- **Scope:** `config`
- **English:** Provider API key. OpenCellID service requires API key.
- **فارسی:** کلید API. برای OpenCellID ضروری است.

### `geolocation.processInvalidPositions`
- **Scope:** `config`
- **English:** Boolean flag to apply geolocation to invalid positions.
- **فارسی:** اعمال موقعیت‌یابی روی موقعیت‌های نامعتبر.

### `geolocation.reuse`
- **Scope:** `config`
- **English:** Reuse last geolocation result if network details have not changed.
- **فارسی:** استفاده مجدد از آخرین نتیجه در صورت عدم تغییر اطلاعات شبکه.

### `geolocation.requireWifi`
- **Scope:** `config`
- **English:** Process geolocation only when Wi-Fi information is available.
- **فارسی:** پردازش موقعیت‌یابی فقط در صورت وجود اطلاعات Wi-Fi (دقت بالاتر).

### `geolocation.mcc`
- **Scope:** `config`
- **English:** Default MCC value to use if device doesn't report MCC.
- **فارسی:** مقدار پیش‌فرض MCC (کد کشور موبایل) در صورت عدم گزارش دستگاه.

### `geolocation.mnc`
- **Scope:** `config`
- **English:** Default MNC value to use if device doesn't report MNC.
- **فارسی:** مقدار پیش‌فرض MNC (کد شبکه موبایل).

---

## ۱۶. محدودیت سرعت (Speed Limit API)

### `speedLimit.enable`
- **Scope:** `config`
- **English:** Boolean flag to enable speed limit API.
- **فارسی:** فعال‌سازی API محدودیت سرعت.

### `speedLimit.type`
- **Scope:** `config`
- **English:** Provider to use for speed limit. Options: overpass.
- **فارسی:** ارائه‌دهنده محدودیت سرعت (فعلاً overpass).

### `speedLimit.url`
- **Scope:** `config`
- **English:** Speed limit provider API URL address.
- **فارسی:** URL API ارائه‌دهنده.

### `speedLimit.accuracy`
- **Scope:** `config`
- **Default:** `100`
- **English:** Search radius for speed limit. Value in meters.
- **فارسی:** شعاع جستجو برای محدودیت سرعت (متر).

---

## ۱۷. نیمکره مختصات (Location Hemisphere)

### `location.latitudeHemisphere`
- **Scope:** `config`
- **English:** Override latitude sign / hemisphere. Value can be N for North or S for South.
- **فارسی:** بازنویسی نیمکره عرض جغرافیایی (N یا S).

### `location.longitudeHemisphere`
- **Scope:** `config`
- **English:** Override longitude sign / hemisphere. Value can be E for East or W for West.
- **فارسی:** بازنویسی نیمکره طول جغرافیایی (E یا W).

---

## ۱۸. گزارش درخواست‌های وب (Web Request Log)

### `web.requestLog.path`
- **Scope:** `config`
- **English:** Jetty request log path. Must include "yyyy_mm_dd" which is replaced with the actual date.
- **فارسی:** مسیر لاگ درخواست‌های Jetty. باید شامل `yyyy_mm_dd` باشد که با تاریخ واقعی جایگزین می‌شود.

### `web.requestLog.retainDays`
- **Scope:** `config`
- **English:** Set the number of days before rotated request log files are deleted.
- **فارسی:** تعداد روزهای نگهداری لاگ‌های چرخشی قبل از حذف.

### `web.disableHealthCheck`
- **Scope:** `config`
- **English:** Disable systemd health checks.
- **فارسی:** غیرفعال کردن بررسی سلامت systemd.

### `web.healthCheck.dropThreshold`
- **Scope:** `config`
- **English:** If set, Traccar will monitor drops in the number of stored messages. Threshold is a value from 0.0 to 1.0.
- **فارسی:** در صورت تنظیم، افت تعداد پیام‌های ذخیره‌شده پایش می‌شود. مقدار بین ۰.۰ تا ۱.۰.

### `web.sameSiteCookie`
- **Scope:** `config`
- **English:** Sets SameSite cookie attribute value. Options: Lax, Strict, None.
- **فارسی:** مقدار ویژگی SameSite کوکی. گزینه‌ها: `Lax`, `Strict`, `None`.

### `web.persistSession`
- **Scope:** `config`
- **English:** Enables persisting Jetty sessions to the database.
- **فارسی:** ذخیره‌سازی نشست‌های Jetty در پایگاه داده.

---

## ۱۹. لاگ‌ها (Logger)

### `logger.console`
- **Scope:** `config`
- **English:** Output logging to the standard terminal output instead of a log file.
- **فارسی:** خروجی لاگ به ترمینال به‌جای فایل.

### `logger.queries`
- **Scope:** `config`
- **English:** Log executed SQL queries.
- **فارسی:** ثبت پرس‌وجوهای SQL اجرا شده.

### `logger.file`
- **Scope:** `config`
- **Default:** `"./logs/tracker-server.log"`
- **English:** Log file name. For rotating logs, a date is added to the end of the file name for non-current logs.
- **فارسی:** نام فایل لاگ. برای لاگ‌های چرخشی، تاریخ به انتهای نام فایل اضافه می‌شود.

### `logger.level`
- **Scope:** `config`
- **Default:** `"info"`
- **English:** Logging level. Available options: off, severe, warning, info, config, fine, finer, finest, all.
- **فارسی:** سطح لاگ. گزینه‌ها: `off`, `severe`, `warning`, `info`, `config`, `fine`, `finer`, `finest`, `all`.

### `logger.fullStackTraces`
- **Scope:** `config`
- **English:** Print full exception traces. Useful for debugging.
- **فارسی:** چاپ کامل ردپای استثناها. برای اشکال‌زدایی مفید است.

### `logger.rotate`
- **Scope:** `config`
- **Default:** `true`
- **English:** Create a new log file daily.
- **فارسی:** ایجاد فایل لاگ جدید روزانه.

### `logger.decodeTextData`
- **Scope:** `config`
- **Default:** `true`
- **English:** If all bytes are printable characters, log network data as text instead of HEX.
- **فارسی:** در صورتی که همه بایت‌ها قابل چاپ باشند، داده شبکه به صورت متن ثبت شود نه HEX.

### `logger.rotate.interval`
- **Scope:** `config`
- **Default:** `"day"`
- **English:** Log file rotation interval. Options: day, hour.
- **فارسی:** فاصله چرخش لاگ. گزینه‌ها: `day`, `hour`.

### `logger.attributes`
- **Scope:** `config`
- **Default:** `"time,position,speed,course,accuracy,result"`
- **English:** A list of position attributes to log.
- **فارسی:** فهرست ویژگی‌های موقعیت که لاگ شوند.

---

## ۲۰. همگام‌سازی و Broadcast

### `broadcast.type`
- **Scope:** `config`
- **English:** Broadcast method. Options: "multicast", "redis". Default is disabled if not specified.
- **فارسی:** روش broadcast. گزینه‌ها: `multicast`, `redis`. در صورت عدم تنظیم غیرفعال است.

### `broadcast.interface`
- **Scope:** `config`
- **English:** Multicast interface. It can be either an IP address or an interface name.
- **فارسی:** رابط multicast. می‌تواند آدرس IP یا نام رابط باشد.

### `broadcast.address`
- **Scope:** `config`
- **English:** Multicast address or Redis URL for broadcasting synchronization events.
- **فارسی:** آدرس multicast یا Redis URL برای پخش رویدادهای همگام‌سازی.

### `broadcast.port`
- **Scope:** `config`
- **English:** Multicast port for broadcasting synchronization events.
- **فارسی:** پورت multicast برای پخش رویدادهای همگام‌سازی.

### `broadcast.secondary`
- **Scope:** `config`
- **English:** Flag to mark secondary servers. Some tasks, like scheduled reports, will be executed on the main server only.
- **فارسی:** پرچم نشان‌گذاری سرورهای ثانویه. برخی وظایف مانند گزارش‌های زمان‌بندی‌شده فقط روی سرور اصلی اجرا می‌شوند.

---

## ۲۱. ارسال داده (Forwarding)

### `server.forward`
- **Scope:** `config`
- **English:** Host for raw data forwarding.
- **فارسی:** میزبان برای ارسال داده خام.

### `forward.type`
- **Scope:** `config`
- **Default:** `"url"`
- **English:** Position forwarding format. Available options: "url", "json", "kafka".
- **فارسی:** فرمت ارسال موقعیت. گزینه‌ها: `url`, `json`, `kafka`.

### `forward.exchange`
- **Scope:** `config`
- **Default:** `"traccar"`
- **English:** Position forwarding AMQP exchange.
- **فارسی:** صرافی AMQP برای ارسال موقعیت.

### `forward.topic`
- **Scope:** `config`
- **Default:** `"positions"`
- **English:** Position forwarding Kafka topic or AMQP routing key.
- **فارسی:** تاپیک Kafka یا کلید مسیریابی AMQP برای موقعیت‌ها.

### `forward.url`
- **Scope:** `config`, `device`
- **English:** URL to forward positions. Data is passed through URL parameters. For example, {uniqueId} for device identifier.
- **فارسی:** آدرس ارسال موقعیت‌ها. داده‌ها از طریق پارامترهای URL ارسال می‌شوند. مثال: `{uniqueId}`.

### `forward.header`
- **Scope:** `config`
- **English:** Additional HTTP header that can be used for authorization.
- **فارسی:** هدر HTTP اضافی برای احراز هویت.

### `forward.retry.enable`
- **Scope:** `config`
- **English:** Enable position forwarding retries.
- **فارسی:** فعال‌سازی تلاش مجدد برای ارسال موقعیت.

### `forward.retry.delay`
- **Scope:** `config`
- **Default:** `100`
- **English:** Position forwarding retry first delay in milliseconds. Defaults to 100 milliseconds.
- **فارسی:** تأخیر اولیه برای تلاش مجدد (میلی‌ثانیه). پیش‌فرض ۱۰۰.

### `forward.retry.count`
- **Scope:** `config`
- **Default:** `10`
- **English:** Position forwarding retry maximum retries. Defaults to 10.
- **فارسی:** حداکثر تعداد دفعات تلاش مجدد. پیش‌فرض ۱۰.

### `forward.retry.limit`
- **Scope:** `config`
- **Default:** `100`
- **English:** Position forwarding retry pending positions limit. Defaults to 100 positions.
- **فارسی:** حداکثر موقعیت‌های در انتظار برای تلاش مجدد. پیش‌فرض ۱۰۰.

### `event.forward.type`
- **Scope:** `config`
- **Default:** `"json"`
- **English:** Events forwarding format. Options: "json", "kafka".
- **فارسی:** فرمت ارسال رویدادها. گزینه‌ها: `json`, `kafka`.

### `event.forward.exchange`
- **Scope:** `config`
- **Default:** `"traccar"`
- **English:** Events forwarding AMQP exchange.
- **فارسی:** صرافی AMQP برای رویدادها.

### `event.forward.topic`
- **Scope:** `config`
- **Default:** `"events"`
- **English:** Events forwarding Kafka topic or AMQP routing key.
- **فارسی:** تاپیک Kafka یا کلید مسیریابی AMQP برای رویدادها.

### `event.forward.url`
- **Scope:** `config`
- **English:** Events forwarding URL.
- **فارسی:** آدرس ارسال رویدادها.

### `event.forward.header`
- **Scope:** `config`
- **English:** Events forwarding headers. Example: FirstHeader: hello SecondHeader: world
- **فارسی:** هدرهای ارسال رویدادها. مثال: `FirstHeader: hello SecondHeader: world`.

### `templates.root`
- **Scope:** `config`
- **Default:** `"templates"`
- **English:** Root folder for all template files.
- **فارسی:** پوشه ریشه فایل‌های قالب.

---

## ۲۲. ایمیل و پیامک (Mail & SMS)

### `mail.debug`
- **Scope:** `config`
- **English:** Log emails instead of sending them via SMTP. Intended for testing purposes only.
- **فارسی:** به‌جای ارسال ایمیل، آن را در لاگ ثبت کن. فقط برای تست.

### `mail.smtp.systemOnly`
- **Scope:** `config`
- **English:** Restrict global SMTP configuration to system messages only.
- **فارسی:** محدود کردن تنظیمات SMTP سراسری به پیام‌های سیستمی (مثلاً بازیابی رمز).

### `mail.smtp.ignoreUserConfig`
- **Scope:** `config`
- **English:** Force SMTP settings from the config file and ignore user attributes.
- **فارسی:** اجبار تنظیمات SMTP از فایل پیکربندی و نادیده گرفتن تنظیمات کاربر.

### `mail.smtp.host`
- **Scope:** `config`, `user`
- **English:** The SMTP server to connect to.
- **فارسی:** آدرس سرور SMTP.

### `mail.smtp.port`
- **Scope:** `config`, `user`
- **Default:** `25`
- **English:** The SMTP server port to connect. Defaults to 25.
- **فارسی:** پورت سرور SMTP. پیش‌فرض ۲۵.

### `mail.transport.protocol`
- **Scope:** `config`, `user`
- **Default:** `"smtp"`
- **English:** Email transport protocol. Default value is "smtp".
- **فارسی:** پروتکل انتقال ایمیل. پیش‌فرض `smtp`.

### `mail.smtp.starttls.enable`
- **Scope:** `config`, `user`
- **English:** If true, enables the use of the STARTTLS command.
- **فارسی:** فعال‌سازی فرمان STARTTLS برای ارتقا به اتصال امن.

### `mail.smtp.starttls.required`
- **Scope:** `config`, `user`
- **English:** If true, requires the use of the STARTTLS command.
- **فارسی:** اجباری بودن STARTTLS (در صورت عدم موفقیت اتصال برقرار نمی‌شود).

### `mail.smtp.ssl.enable`
- **Scope:** `config`, `user`
- **English:** If set to true, use SSL to connect and use the SSL port by default.
- **فارسی:** استفاده از SSL برای اتصال.

### `mail.smtp.ssl.trust`
- **Scope:** `config`, `user`
- **English:** If set to "*", all hosts are trusted. If set to a whitespace separated list of hosts, those hosts are trusted.
- **فارسی:** تنظیم اعتماد به میزبان‌ها برای SSL. `*` یعنی همه معتبر.

### `mail.smtp.ssl.protocols`
- **Scope:** `config`, `user`
- **English:** Specifies the SSL protocols that will be enabled for SSL connections.
- **فارسی:** پروتکل‌های SSL فعال برای اتصال.

### `mail.smtp.username`
- **Scope:** `config`, `user`
- **English:** SMTP connection username.
- **فارسی:** نام کاربری SMTP.

### `mail.smtp.password`
- **Scope:** `config`, `user`
- **English:** SMTP connection password.
- **فارسی:** رمز عبور SMTP.

### `mail.smtp.from`
- **Scope:** `config`, `user`
- **English:** Email address to use for SMTP MAIL command.
- **فارسی:** آدرس ایمیل فرستنده.

### `mail.smtp.fromName`
- **Scope:** `config`, `user`
- **English:** The personal name for the email from address.
- **فارسی:** نام نمایشی فرستنده ایمیل.

### `sms.http.url`
- **Scope:** `config`
- **English:** SMS API service full URL. Enables SMS commands and notifications.
- **فارسی:** URL کامل سرویس SMS.

### `sms.http.authorizationHeader`
- **Scope:** `config`
- **Default:** `"Authorization"`
- **English:** SMS API authorization header name. Default value is 'Authorization'.
- **فارسی:** نام هدر احراز هویت SMS. پیش‌فرض `Authorization`.

### `sms.http.authorization`
- **Scope:** `config`
- **English:** SMS API authorization header value. This value takes precedence over user and password.
- **فارسی:** مقدار هدر احراز هویت SMS. این مقدار بر نام کاربری و رمز اولویت دارد.

### `sms.http.user`
- **Scope:** `config`
- **English:** SMS API basic authentication user.
- **فارسی:** نام کاربری برای احراز هویت پایه SMS.

### `sms.http.password`
- **Scope:** `config`
- **English:** SMS API basic authentication password.
- **فارسی:** رمز عبور احراز هویت پایه SMS.

### `sms.http.template`
- **Scope:** `config`
- **English:** SMS API body template. Placeholders {phone} and {message} can be used. If value starts with '{' or '[', server assumes JSON format.
- **فارسی:** قالب بدنه درخواست SMS. می‌توان از `{phone}` و `{message}` استفاده کرد. اگر با `{` یا `[` شروع شود JSON در نظر گرفته می‌شود.

### `sms.aws.access`
- **Scope:** `config`
- **English:** AWS Access Key with SNS permission.
- **فارسی:** کلید دسترسی AWS (با مجوز SNS).

### `sms.aws.secret`
- **Scope:** `config`
- **English:** AWS Secret Access Key with SNS permission.
- **فارسی:** کلید مخفی AWS.

### `sms.aws.region`
- **Scope:** `config`
- **English:** AWS Region for SNS service.
- **فارسی:** منطقه AWS برای سرویس SNS.

---

## ۲۳. رویدادهای خاص (Event Settings)

### `event.overspeed.thresholdMultiplier`
- **Scope:** `config`
- **Default:** `1.0`
- **English:** Speed limit threshold multiplier. Example: if speed limit is 100, set to 1.05 to trigger only above 105.
- **فارسی:** ضریب آستانه سرعت. مثلاً اگر محدودیت ۱۰۰ باشد و ضریب ۱.۰۵، رویداد در سرعت بالای ۱۰۵ رخ می‌دهد.

### `event.overspeed.minimalDuration`
- **Scope:** `config`
- **English:** Minimal overspeed duration to trigger the event. Value in seconds.
- **فارسی:** حداقل مدت زمان تخطی از سرعت برای ثبت رویداد (ثانیه).

### `event.overspeed.preferLowest`
- **Scope:** `config`
- **English:** Relevant only for geofence speed limits. Use the lowest speed limit from all geofences.
- **فارسی:** فقط برای محدودیت‌های سرعت حصار جغرافیایی. کمترین محدودیت در بین حصارها اعمال می‌شود.

### `event.behavior.accelerationThreshold`
- **Scope:** `config`
- **English:** Driver behavior acceleration threshold. Value is in meter per second squared.
- **فارسی:** آستانه شتاب (متر بر مجذور ثانیه) برای تشخیص رفتار رانندگی.

### `event.behavior.brakingThreshold`
- **Scope:** `config`
- **English:** Driver behavior braking threshold. Value is in meter per second squared.
- **فارسی:** آستانه ترمزگیری (متر بر مجذور ثانیه).

### `event.ignoreDuplicateAlerts`
- **Scope:** `config`
- **Default:** `true`
- **English:** Do not generate an alert event if the same alert was present in the last known location.
- **فارسی:** در صورت وجود هشدار یکسان در آخرین موقعیت، رویداد تکراری تولید نشود.

### `event.status.enable`
- **Scope:** `config`
- **Default:** `false`
- **English:** Generate device connection status events (deviceOnline, deviceOffline, deviceUnknown) on status changes.
- **فارسی:** تولید رویدادهای تغییر وضعیت اتصال دستگاه.

### `event.motion.speedThreshold`
- **Scope:** `config`, `device`
- **Default:** `0.01`
- **English:** If the speed is above specified value, the object is considered to be in motion. Default is 0.01 knots.
- **فارسی:** سرعت بالاتر از این مقدار (نات) یعنی دستگاه در حال حرکت است. پیش‌فرض ۰.۰۱.

### `protocols.enable`
- **Scope:** `config`
- **English:** List of protocols to enable. Comma-separated. Example: teltonika,osmand
- **فارسی:** فهرست پروتکل‌های فعال. با کاما جدا شوند.

---

## یادداشت پایانی
تمامی مقادیر پیش‌فرض ذکر شده همان مقادیری هستند که در صورت عدم تنظیم پارامتر توسط سرور استفاده می‌شوند.  
برای اطلاعات بیشتر به [مستندات رسمی Traccar](https://www.traccar.org/configuration-file/) مراجعه کنید.

*This document has been compiled from the official Traccar configuration file reference. English descriptions are preserved exactly as provided.*