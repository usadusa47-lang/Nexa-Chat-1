# صرف موبائل سے APK حاصل کرنے کا طریقہ

آپ کے پاس کمپیوٹر نہیں ہے تو GitHub Actions کے ذریعے کلاؤڈ میں **ٹیسٹنگ APK** بنوانے کی کوشش کر سکتے ہیں۔ اس کے لیے GitHub اکاؤنٹ درکار ہے، اور پہلے Firebase config بھی لگانی ہوگی اگر آن لائن چیٹ ٹیسٹ کرنا چاہتے ہیں۔

1. GitHub پر نیا repository بنائیں۔
2. اس ZIP کو extract کرکے تمام فائلیں repository میں upload کریں، hidden `.github` folder سمیت۔ بہتر ہے GitHub کی ویب سائٹ پر `Add file` یا موبائل browser کے desktop mode سے فائلیں اپ لوڈ کریں۔
3. `web/firebase-config.js` میں Firebase config اپنی values سے بدلیں۔ اگر ابھی config نہیں ہے تو APK بن سکتی ہے، مگر login/chat کام نہیں کریں گے۔
4. فائلیں `main` branch میں commit کریں۔
5. repository کے `Actions` tab میں `Build Nexa Chat Android APK` کھولیں اور `Run workflow` دبائیں۔
6. کام مکمل ہونے کے بعد workflow run میں `NexaChat-debug-apk` artifact ڈاؤن لوڈ کریں۔
7. APK اپنے Android فون پر کھول کر install کریں۔ Android آپ سے browser/files کے لیے “Install unknown apps” کی اجازت مانگ سکتا ہے؛ صرف اپنی بنائی ہوئی APK پر اعتماد کریں۔

## Google Play Store کے لیے
یہ workflow صرف **debug APK** بناتا ہے، جو ذاتی ٹیسٹنگ کے لیے ہے۔ Play Store کے لیے release build اور signed Android App Bundle (AAB) درکار ہوگا۔ اس کے لیے signing key/keystore کو محفوظ بنانا، GitHub Actions secrets یا مقامی محفوظ build میں استعمال کرنا، Play Console اکاؤنٹ، privacy policy، Data Safety form، testing اور موجودہ Play requirements پوری کرنا ہوں گی۔ اپنی keystore یا passwords کسی کے ساتھ شیئر نہ کریں۔

## حقیقی آن لائن چیٹ کے لیے
`web/firebase-config.js` میں Firebase project کی درست config لازمی ہے، اور `firestore.rules` کو Firebase Console میں publish کریں۔ پہلے دو الگ اکاؤنٹس بنا کر browser میں ٹیسٹ کریں۔ موجودہ MVP میں text chat ہے؛ میڈیا، calls، push notifications اور end-to-end encryption شامل نہیں۔
