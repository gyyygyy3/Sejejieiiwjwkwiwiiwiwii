# آسيا فون (Asia Phone)

تطبيق Android لخدمات آسياسيل. تم الإبقاء على الشاشات والوظائف الحالية، مع تحديث إعدادات البناء والتوافق مع متطلبات Android الحديثة.

## معلومات التطبيق

- الاسم الظاهر: آسيا فون
- Android Application ID: `com.asiaphone.app`
- Kotlin namespace: `com.asiaphone.app`
- الإصدار الحالي: `1.0.0` (`versionCode = 1`)
- الحد الأدنى: Android 8.0 (API 26)
- `compileSdk`: 36
- `targetSdk`: 36 (Android 16)
- JDK: 17
- Android Gradle Plugin: 8.10.1
- Gradle: 8.11.1

## البناء لرفع Google Play

1. استخدم Android Studio Meerkat أو أحدث، مع JDK 17.
2. ثبّت Android SDK Platform 36 من SDK Manager.
3. شغّل `tools/setup-gradle-wrapper.bat` على Windows أو `sh tools/setup-gradle-wrapper.sh` على macOS/Linux مرة واحدة لإنشاء Gradle Wrapper الرسمي (يتطلب اتصال إنترنت لتنزيل Gradle 8.11.1 ومكونات البناء).
4. افتح المشروع في Android Studio وانتظر اكتمال Gradle Sync.
5. من Android Studio اختر **Build > Generate Signed Bundle / APK > Android App Bundle**، ثم اختر مفتاح الرفع الخاص بك. لا تضع ملف keystore أو كلمات مروره داخل المشروع أو ترسلها لأحد.
6. ملف الرفع الناتج يكون عادةً في `app/build/outputs/bundle/release/app-release.aab`.

يمكن بعد إنشاء Gradle Wrapper البناء من سطر الأوامر باستخدام:

```bash
./gradlew bundleRelease
```

وعلى Windows:

```bat
gradlew.bat bundleRelease
```

> لا يوجد ملف AAB موقّع مرفق. يجب توقيع إصدار النشر بمفتاح الرفع الذي تملكه، ثم تفعيل Play App Signing في Play Console.

## مسار الرصيد وكود الخدمة

1. يطلب التطبيق رسالة الرصيد من شريحة آسياسيل.
2. يرسل التطبيق نص رسالة الرصيد إلى وظيفة Netlify عبر HTTPS.
3. يستقبل الخادم نص الرصيد ويعيد كود خدمة USSD المحدد في إعداد `ASIA_PHONE_USSD_CODE`.
4. يستلم التطبيق الكود وينفذه على شريحة آسياسيل باستخدام `TelephonyManager.sendUssdRequest`.

الخادم لا يحوّل الرصيد إلى مبلغ، ولا يحدد مستلماً، ولا ينشئ كود تحويل. مثال كود خدمة يمكن ضبطه في Netlify: `*350#`.

## إعداد Netlify

أضف متغيري البيئة التاليين في إعدادات الموقع:

- `ASIA_PHONE_API_KEY`: مفتاح المصادقة نفسه الذي يستخدمه التطبيق.
- `ASIA_PHONE_USSD_CODE`: كود خدمة USSD المراد إرجاعه، مثل `*350#`.

يجب أن يكون الكود بصيغة USSD صحيحة ويبدأ بـ `*` وينتهي بـ `#`. إذا لم يُضبط المتغير أو كان غير صالح، لا يعيد الخادم كوداً.

## الاختبارات

يتطلب Node.js 18 أو أحدث. من جذر المشروع شغّل:

```bash
npm test
```

## ملاحظة أمنية

مفتاح API المضمّن في تطبيق العميل قابل للاستخراج من APK/AAB، لذلك لا يُعد سراً قوياً عند توزيع التطبيق على نطاق عام. نفّذ قيود استخدام ومراقبة على وظيفة Netlify.

## أصول المتجر

الملف `play-assets/app-icon-512.png` هو أيقونة المتجر بمقاس 512×512 ومطابقة لعلامة التطبيق الموجودة. لا تعتبر صور `preview/` لقطات شاشة فعلية ملتقطة من إصدار Android النهائي.

## سياسة الخصوصية

- صفحة السياسة: `public/privacy-policy.html`
- ملاحظات النشر والاستكمال: `PRIVACY_POLICY_PUBLISH_NOTES_AR.md`
