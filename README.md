# SssssssskiLLL — Repository Intelligence Archive

آرشیو مستندات، نقشه‌ساختار و شواهد مهندسی معکوس پروژه‌های نرم‌افزاری.

## دامنه فعلی
فاز یک: هشت مخزن عمومی thedotmack: claude-mem، lossless-claw، membench، mcp-client-cli، hermes-agent، superset، ODS و humanish.

## ناوبری
- [فهرست دانش](docs/INDEX.md)
- [روش‌شناسی](docs/METHODOLOGY.md)
- [فهرست مخازن و snapshot](docs/REPOSITORY-INVENTORY.md)
- [مجوز و منشأ کد](docs/comparative/license-and-provenance.md)
- [دفتر تست‌ها](docs/reference/test-register.md)
- [فهرست سورس‌های upstream](sources/README.md)
- [نقشه فایل‌های کلیدی سورس](sources/FILE-MAP.md)
- [تحلیل اختلاف forkها](docs/comparative/fork-delta.md)
- [گزارش‌های مستقل](docs/repositories/)
- [تحلیل تطبیقی](docs/comparative/)

## قواعد
- مخازن منبع فقط خوانده می‌شوند؛ هیچ تغییر، commit، issue یا PR در آن‌ها ایجاد نمی‌شود.
- سورس ثالث بدون بررسی مجوز و ضرورت کپی نمی‌شود؛ مسیر و لینک upstream ثبت می‌شود.
- شمارش فایل تست به معنی اجرای موفق تست نیست.
- هر ادعا باید به فایل، مسیر، SHA یا snapshot مشخص متصل شود.
- وضعیت‌های PASS / FAIL / NOT RUN / BLOCKED فقط با شواهد ثبت می‌شوند.

## وضعیت فعلی
- [x] شناسایی مخزن مقصد و تأیید خالی بودن آن
- [x] برداشت درخت اولیه ۸ مخزن
- [ ] تحلیل عمیق کد و جریان‌ها
- [ ] اجرای تست‌ها در محیط ایزوله
- [ ] گزارش نهایی تطبیقی
