# تحويل موقع Strofy إلى تطبيق iOS

## المتطلبات قبل البدء
- جهاز **Mac** (Xcode ما يشتغل إلا على macOS)
- تثبيت [Xcode](https://apps.apple.com/app/xcode/id497799835) من App Store
- تثبيت [Node.js](https://nodejs.org)
- حساب Apple Developer (مجاني للتجربة على جهازك، 99$/سنة للنشر بـ App Store)
- موقعك مرفوع فعليًا على رابط (مثلاً GitHub Pages) — لازم يكون شغال قبل هالخطوة

---

## الخطوات بالترتيب

### 1. افتح Terminal داخل هذا المجلد وثبّت الحزم
```bash
npm install
```

### 2. عدّل ملف `capacitor.config.json`
افتحه وغيّر:
- `appId`: بصيغة عكسية مثل `com.strofy.app` (لازم يكون فريد)
- `server.url`: حط رابط موقعك الحقيقي بعد ما ترفعه (مثال: `https://username.github.io/strofy`)

### 3. أضف منصة iOS
```bash
npx cap add ios
npx cap sync
```
هذا الأمر ينشئ مجلد `ios/` فيه مشروع Xcode كامل.

### 4. أضف الشعار وصورة splash screen
1. افتح `ios/App/App/Assets.xcassets` من Finder أو Xcode
2. اسحب صورة الـ App Icon داخل `AppIcon.appiconset`
3. اسحب صورة splash داخل `Splash.imageset` (احتاج 3 أحجام: 1x, 2x, 3x)
   - أسهل طريقة: استخدم موقع [appicon.co](https://appicon.co) لتوليد كل الأحجام تلقائيًا من صورة وحدة

### 5. زامن التغييرات
```bash
npx cap sync
```
(كرر هذا الأمر كل مرة تغيّر بملف capacitor.config.json أو تضيف صور)

### 6. افتح المشروع بـ Xcode
```bash
npx cap open ios
```

### 7. شغّل التطبيق
- من أعلى Xcode اختر جهاز أو محاكي (Simulator)
- اضغط ▶️ Run
- أول مرة تشغّل، Xcode بيطلب توقيع (Signing) — روح لـ:
  `App target → Signing & Capabilities → اختر Team (حسابك Apple Developer)`

### 8. تصدير ملف .ipa (للتوزيع)
من Xcode:
```
Product → Archive
```
بعد ما يخلص الأرشفة:
```
Distribute App → اختار طريقة التوزيع (Ad Hoc / TestFlight / App Store Connect) → Export
```
بيطلع لك ملف `.ipa` جاهز.

---

## كيف تحدّث محتوى التطبيق بدون رفع نسخة جديدة؟
بما إن `server.url` بملف الإعدادات يشاور على موقعك المرفوع (GitHub Pages مثلاً):
- أي تعديل تسويه على ملفات الموقع وترفعه (`git push`) ينعكس **تلقائيًا** داخل التطبيق
- ما تحتاج تعيد الأرشفة أو ترفع نسخة جديدة لـ App Store لتحديثات المحتوى (الأقسام، التطبيقات المعروضة، الصور...)
- تحتاج نسخة جديدة فقط لو غيّرت كود Swift/Capacitor نفسه أو أضفت plugin جديد

## مجلد www/
فيه نسخة احتياطية من الموقع (offline fallback) — تنعرض فقط لو مافي إنترنت أو الرابط ما اشتغل. المحتوى الأساسي دايمًا من `server.url`.
