# Nexa Chat — عالمی آن لائن چیٹ MVP

یہ ایک **ابتدائی، قابلِ ترتیب پروجیکٹ** ہے۔ اس میں responsive ویب ایپ/PWA، Firebase Authentication، Firestore کی realtime chat، بنیادی سکیورٹی rules، اور Capacitor کے ذریعے Android پیکج بنانے کی بنیاد شامل ہے۔

## کیا چلتا ہے؟
- موبائل اور کمپیوٹر کے browser میں responsive UI
- Email/password کے ذریعے account sign-up اور login (Firebase configure ہونے کے بعد)
- username کے ذریعے کسی registered user کو تلاش کرنا
- دو صارفین کے درمیان realtime text chat (Firestore configure ہونے کے بعد)
- PWA manifest اور service worker
- Android wrapper بنانے کے لیے Capacitor config

## ابھی کیا شامل نہیں؟
- یہ WhatsApp کا مکمل متبادل یا production-certified app نہیں۔
- فی الحال تصویر/ویڈیو اپ لوڈ، voice/video calls، push notifications، group chats، end-to-end encryption، spam reporting/moderation اور account recovery مکمل نہیں ہیں۔
- ایپ کو Google Play پر شائع نہیں کیا گیا؛ Play Console اکاؤنٹ، signed Android App Bundle، privacy policy، data safety declaration، testing اور Google کی موجودہ شرائط پوری کرنا آپ کی ذمہ داری ہوگی۔
- Firebase configuration ڈالے بغیر login اور cloud chat کام نہیں کریں گے۔

---

## حصہ 1 — Firebase کو مفت/اپنے منصوبے کے مطابق configure کریں

1. https://console.firebase.google.com/ کھولیں اور Google account سے sign in کریں۔
2. `Create a project` منتخب کریں، مثلاً `nexa-chat-global`۔
3. Project میں Web App شامل کریں اور Firebase config values نقل کریں۔
4. `Authentication` → `Sign-in method` میں **Email/Password** فعال کریں۔
5. `Firestore Database` بنائیں۔ ابتدائی طور پر اپنے مناسب region کا انتخاب سوچ سمجھ کر کریں کیونکہ بعد میں region بدلنا آسان نہیں۔
6. `web/firebase-config.js` میں Firebase config values درج کریں۔
7. `firestore.rules` کے متن کو Firebase Console → Firestore → Rules میں paste اور Publish کریں۔
8. Firebase Console میں Authentication کے Email/Password provider کو فعال رکھیں۔

**اہم:** Firebase web config عموماً browser میں ظاہر ہوتا ہے، مگر Firestore Security Rules لازمی ہیں۔ کبھی بھی service-account private key یا server secret کو ویب فائل میں نہ ڈالیں۔

## حصہ 2 — موبائل/کمپیوٹر پر ویب ایپ چلائیں

ES modules کی وجہ سے اسے صرف `file://` سے کھولنے کے بجائے local server یا HTTPS hosting سے چلائیں۔

Python موجود ہو تو اس فولڈر میں terminal کھولیں:
```bash
cd web
python -m http.server 8080
```
پھر اسی کمپیوٹر پر `http://localhost:8080` کھولیں۔ موبائل پر ٹیسٹ کرنے کے لیے اسی Wi‑Fi پر کمپیوٹر کا local IP استعمال کیا جا سکتا ہے؛ Firebase Auth کے authorized domains میں domain بھی شامل کرنا پڑ سکتا ہے۔

آسان hosting:
- Firebase Hosting: https://firebase.google.com/docs/hosting
- Firebase Hosting پر deploy کرنے سے پہلے Firebase CLI انسٹال اور login کرنا ہوگا۔

## حصہ 3 — دو صارفین سے ٹیسٹ

1. ایک browser میں user A کا account بنائیں۔
2. دوسرے browser/incognito یا دوسرے موبائل پر user B بنائیں۔
3. user A میں user B کا username تلاش کریں اور chat شروع کریں۔
4. دونوں طرف پیغام بھیج کر realtime delivery چیک کریں۔
5. Firestore console میں database records اور rules errors چیک کریں۔

## حصہ 4 — Android app بنانا

ضروری: Node.js، Android Studio اور Android SDK والا کمپیوٹر۔

اس project میں `capacitor.config.json` اور `package.json` شامل ہیں۔ عام workflow:
```bash
npm install
npm install @capacitor/core @capacitor/android
npm install --save-dev @capacitor/cli
npx cap add android
npx cap sync android
npx cap open android
```
Android Studio میں Gradle sync کریں، emulator/حقیقی فون پر test کریں، پھر release کے لیے signed Android App Bundle (AAB) بنائیں۔ اس project میں پہلے سے compiled APK/AAB شامل نہیں۔

## اشاعت سے پہلے لازمی کام
- حقیقی فونز پر testing اور crash fixes
- Privacy Policy URL اور درست Data Safety form
- account deletion/retention workflow
- abuse reporting, blocking, rate limiting, anti-spam
- image/video storage کے لیے Firebase Storage rules اور upload limits
- end-to-end encryption اگر واقعی درکار ہو؛ صرف HTTPS یا Firestore استعمال کرنا E2E encryption نہیں
- Google Play کی موجودہ target SDK، testing اور verification requirements کی جانچ

## سکیورٹی نوٹ
`firestore.rules` میں user profiles کو authenticated صارفین کے لیے محدود رکھا گیا ہے اور chats کے لیے participant membership چیک کیا جاتا ہے۔ یہ MVP rules ہیں؛ public launch سے پہلے expert security review اور abuse testing کروائیں۔


## موبائل سے ٹیسٹنگ APK
`MOBILE_BUILD_GUIDE_UR.md` دیکھیں۔ GitHub Actions workflow `.github/workflows/android-apk.yml` کلاؤڈ میں debug APK بنانے کی بنیاد فراہم کرتا ہے؛ اسے ابھی خودکار طور پر چلایا یا APK کے طور پر تصدیق نہیں کیا گیا۔
