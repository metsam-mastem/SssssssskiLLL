# ماتریس امکان‌سنجی و اولویت مطالعه

امتیازها در این نسخه کیفی‌اند و بر اساس اندازه/ساختار قابل مشاهده و نوع مسئله‌اند؛ ارزیابی کیفیت محصول یا امنیت نیستند.

| مخزن | پیچیدگی مطالعه | امکان بررسی کامل ایستا | امکان اجرای آزمایش کنترل‌شده | ریسک مجوز/استفاده مجدد | اولویت تحلیل |
|---|---|---|---|---|---|
| humanish | پایین از نظر اندازه؛ بالا از نظر امنیت | بالا؛ فقط 5 blob | متوسط، با mock services | متوسط؛ Apache metadata، dependency review لازم | بسیار بالا برای threat model |
| mcp-client-cli | پایین | بالا؛ 15 blob | بالا با MCP mock server | پایین تا متوسط؛ MIT | بالا برای فهم protocol bridge |
| lossless-claw | متوسط | بالا | بالا با OpenClaw test harness | متوسط؛ MIT، fork delta لازم | بالا برای الگوریتم context |
| membench | متوسط | بالا | متوسط تا پایین؛ وابسته به harness و judge | بالا از نظر privacy/data handling | بسیار بالا برای معیار ارزیابی |
| claude-mem | بالا | متوسط؛ 3,030 blob | متوسط؛ native/runtime/provider integration | متوسط؛ Apache + vendored review | بالا |
| hermes-agent | بسیار بالا | متوسط | متوسط؛ چند provider/runtime backend | متوسط؛ fork و plugins/skills | بالا |
| superset | بسیار بالا | متوسط | متوسط؛ desktop/DB/PTY/Git dependencies | بسیار بالا؛ Elastic License 2.0 | مشروط به بررسی مجوز |
| ODS | بسیار بالا | متوسط | پایین تا متوسط؛ سخت‌افزار، Docker، GPU/CPU matrix | بالا؛ mixed-license | مشروط به مجوز/محیط اجرا |

## ترتیب پیشنهادی تحلیل عمیق
1. humanish — تمام فایل‌ها و threat model؛ نسبت اندازه به ارزش بررسی بالاست.
2. mcp-client-cli — فهم دقیق اتصال، schema conversion و process lifecycle.
3. lossless-claw — الگوریتم داده/summary DAG و invariantها.
4. membench — اعتبارسنجی تجربی و کنترل‌های آماری.
5. claude-mem — pipeline حافظه و ادغام‌های متعدد.
6. hermes-agent — agent runtime و tool/skill execution.
7. superset — پس از تعیین محدوده مجوز ELv2 و بررسی parent delta.
8. ODS — بعد از license component map و تحلیل install/security contracts.

## شروط توقف
- هرگونه درخواست credential واقعی یا داده خصوصی در آزمایش.
- مجوز نامشخص برای کپی/توزیع جزء.
- نیاز به دسترسی privileged یا تغییر سیستم اصلی.
- تستی که بدون محیط ایزوله ممکن است به حساب، دستگاه یا سرویس واقعی اثر بگذارد.

## تصمیم
این پروژه فعلاً تحلیل و مستندسازی است؛ نه fork، نه integration، نه deployment و نه reuse کد در محصول دیگر.
