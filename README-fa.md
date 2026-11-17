# ساخت خروجی اندروید و آیفون (Capacitor)

## پیش‌نیازها
- Node.js (نسخهٔ LTS)
- اندروید: Android Studio
- آیفون: مک + Xcode + حساب اپل دولوپر

## مراحل ساخت
```bash
cd hadith-kisa-app
npm install @capacitor/core @capacitor/cli @capacitor/android @capacitor/ios @capacitor-community/keep-awake
npx cap add android
npx cap add ios
npx cap sync
```

## آیکون برنامه (مهم)
آیکون پیش‌فرض Capacitor تا وقتی آیکون خودتان را جایگزین نکنید باقی می‌ماند. ساده‌ترین راه، کپی مستقیم فایل‌های آمادهٔ پوشهٔ `native-icons` است.

**اندروید:** محتوای `native-icons/android/res/` را داخل `android/app/src/main/res/` کپی کنید و اگر پرسید، جایگزین (Replace) کنید. بعد:
```bash
npx cap sync android
```
در Android Studio از Build ← Clean Project و بعد دوباره Build بگیرید. اگر نسخهٔ قبلی روی گوشی نصب است، پیش از نصب نسخهٔ جدید آن را حذف کنید، چون لانچر آیکون قدیمی را نگه می‌دارد.

**آیفون:** فایل `native-icons/ios/AppIcon-512@2x.png` را جایگزین فایل هم‌نام در مسیر `ios/App/App/Assets.xcassets/AppIcon.appiconset/` کنید.

**روش جایگزین (خودکار):**
```bash
npm install -D @capacitor/assets
npx capacitor-assets generate --android --ios --iconBackgroundColor '#214f66' --iconBackgroundColorDark '#214f66'
```
این دستور فایل‌های پوشهٔ `assets` را می‌خواند و باید بعد از `npx cap add` اجرا شود.

بعد از هر تغییر در `www/index.html` فقط `npx cap sync` کافی است.

## نکته‌ها
- **شناسهٔ اپ:** در `capacitor.config.json` مقدار `appId` (فعلاً com.abrar.hadithkisa) را قبل از انتشار به شناسهٔ دلخواه خودتان تغییر دهید.
- **فونت‌ها:** فونت‌های امیری و وزیرمتن از اینترنت (Google Fonts) بارگذاری می‌شوند. برای کار کامل بدون اینترنت، فایل فونت‌ها را داخل `www` بگذارید و با `@font-face` معرفی کنید. در غیر این صورت فونت پیش‌فرض گوشی نمایش داده می‌شود.
- **پخش صوت با قفل صفحه (iOS):** در Xcode، قسمت Signing & Capabilities، Background Modes را اضافه و Audio, AirPlay and Picture in Picture را فعال کنید.
- **کیفیت آیکون:** تصویر منبع کوچک بود (حدود ۵۰۰ پیکسل) و برای ۱۰۲۴ بزرگ‌نمایی شده است. برای کیفیت بهتر در فروشگاه‌ها، نسخهٔ ۱۰۲۴ اصلی را جایگزین کنید. آیکون آیفون باید بدون شفافیت باشد و این فایل همین‌طور است.
- **اعلان‌ها:** در این نسخه نیست و با پلاگین `@capacitor/local-notifications` اضافه می‌شود.
- **روشن ماندن صفحه:** اپ هنگام پخش صوت (و اسکرول خودکار) صفحه را روشن نگه می‌دارد تا پایان دعا. پلاگین `@capacitor-community/keep-awake` برای این کار در دستور نصب بالا آمده است. بعد از نصب پلاگین حتماً `npx cap sync` بزنید و دوباره Build بگیرید. اگر پلاگین نصب نباشد، اپ از روش‌های مرورگر استفاده می‌کند که در بعضی گوشی‌ها قابل اطمینان نیست. هنگام توقف موقت، صفحه تا دو دقیقه روشن می‌ماند و پس از آن می‌تواند خاموش شود.
