# روش‌شناسی مهندسی معکوس

1. هر مخزن با owner/name، default branch، تاریخ برداشت، commit SHA و tree SHA ثبت می‌شود.
2. تحلیل روی snapshot مشخص انجام می‌شود؛ اشاره به main بدون SHA شناسه نسخه کامل نیست.
3. README، LICENSE، manifests و اسناد معماری/امنیت با URL و blob SHA ثبت می‌شوند.
4. درخت فایل به تفکیک حوزه‌های اجرایی، تست، مستندات، asset و vendor دسته‌بندی می‌شود.
5. کد ثالث فقط در صورت نیاز روشن و مجوز معتبر بازنشر می‌شود؛ پیش‌فرض ثبت لینک و مسیر upstream است.

## انواع شواهد
- Metadata: اطلاعات GitHub API مانند fork، parent، language و license metadata.
- Tree: مسیر، نوع، اندازه blob و SHA.
- Source: محتوای واقعی فایل و blob SHA.
- Documentation claim: ادعای README یا docs، نه اثبات runtime.
- Execution: خروجی build/test/benchmark قابل بازتولید.

## شمارش تست
شمارش اولیه با الگوهای نام مسیر و فایل test/tests و suffixهای .test. و .spec. انجام شده است. این شمارش اثبات وجود تست اجرایی، پوشش، CI سبز یا کیفیت تست نیست.

## وضعیت اجرا
INVENTORIED = مسیر تست پیدا شده؛ STATIC-REVIEWED = فایل تست بررسی شده؛ NOT RUN = در محیط ما اجرا نشده؛ PASS/FAIL = فقط پس از اجرای ثبت‌شده با دستور، نسخه runtime، محیط و log.

## محدودیت
fork بودن به معنی برابری با parent نیست. GitHub license metadata جایگزین بررسی فایل LICENSE و مجوز اجزای vendored نیست. نبود فایل تست با الگوی نام‌گذاری، نبود همه تست‌ها را ثابت نمی‌کند.
