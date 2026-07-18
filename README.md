# LAYLATY-ai-TEEM

هذا المستودع مُهيأ لتشغيل واجهة n8n محليًا عبر Docker Compose مع مثال Workflow جاهز.

الملفات المُضافة:

- `docker-compose.yml` — لتشغيل حاوية n8n مع مجلد `workflows` وبيانات المستخدم.
- `workflows/example_workflow.json` — مثال workflow بسيط (Cron → Set).

كيفية التشغيل:

1. تأكد أن Docker و Docker Compose مُثبتان على جهازك.
2. شغّل الأمر:

```bash
docker compose up -d
```

3. افتح المتصفح واذهب إلى: http://localhost:5678
	- بيانات الاعتماد الافتراضية: admin / admin (قابلة للتعديل في `docker-compose.yml`).
4. لعرض أو استيراد الـ workflow المثال: اذهب إلى واجهة n8n → Workflows → Import، واختر `workflows/example_workflow.json`.

تلميحات:
- يمكنك تعديل كلمات المرور وبيانات البيئة في `docker-compose.yml` قبل التشغيل.
- المجلد `data` سيحفظ إعدادات n8n المستمرة على جهازك.

GitHub Actions:

- يوجد ملف إعداد CI في `.github/workflows/ci-deploy.yml` يقوم بالآتي:
	- التحقق من صحة `docker-compose.yml` وملف الـ workflow المثال.
	- يوفّر مهمة نشر يدوية (`workflow_dispatch`) تنفّذ نشرًا عبر SSH إلى خادم بعيد.

للاستخدام العملي لمرحلة النشر، أضف الأسرار التالية في إعدادات المستودع (Settings → Secrets):

- `DEPLOY_HOST` — عنوان الخادم.
- `DEPLOY_USER` — اسم المستخدم للاتصال عبر SSH.
- `DEPLOY_SSH_KEY` — مفتاح SSH الخاص (private key).
- `DEPLOY_PORT` — (اختياري) منفذ SSH إن لم يكن 22.

بعد إعداد الأسرار يمكنك تشغيل النشر من تبويب Actions → CI & Deploy n8n → Run workflow.

توليد مفتاح SSH (موجز سريع):

1. على جهازك المحلي، شغّل:

```bash
ssh-keygen -t ed25519 -C "your_email@example.com"
# اضغط Enter لحفظ في المسار الافتراضي وادخل passphrase إن رغبت
```

2. انسخ المفتاح العام إلى الحافظة (لينكس/macOS):

```bash
cat ~/.ssh/id_ed25519.pub | xclip -selection clipboard  # أو استخدم pbcopy على macOS
```

3. أضف المفتاح إلى GitHub: Settings → SSH and GPG keys → New SSH key.

4. لاختبار الاتصال:

```bash
ssh -T git@github.com
```

يمكنك بعد ذلك وضع المفتاح الخاص (`id_ed25519`) في `DEPLOY_SSH_KEY` كـ secret في إعدادات المستودع.

