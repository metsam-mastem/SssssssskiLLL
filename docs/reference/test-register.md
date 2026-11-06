# دفتر موجودی تست‌ها

## شمارش اولیه و وضعیت اجرا
| مخزن | مسیرهای تست‌یاب | دسته‌های نمونه | اجرا |
|---|---:|---|---|
| claude-mem | 1,543 | SQLite، worker، search، context، infrastructure، server، eval، E2E | NOT RUN |
| lossless-claw | 20 | config، engine، expansion، FTS، indexing، migration، integration | NOT RUN |
| membench | 25 | command، corpus، metrics، orchestrator، observer، sandbox، privacy، release gate | NOT RUN |
| mcp-client-cli | 0 | package.json test script uses node --test; no test-named paths detected | NOT RUN |
| hermes-agent | 665 | agent/provider، ACP، MCP، gateway، tools، sessions | NOT RUN |
| superset | 333 | API routes، Git operations، worktrees، terminal، presets، settings | NOT RUN |
| ODS | 2,214 | Python/JS tests، service configs، governance، installers، fixtures | NOT RUN |
| humanish | 0 | no test-named paths; repository has no implementation source in snapshot | NOT RUN |

## تفسیر
- این شمارش از tree و heuristic نام‌گذاری آمده است؛ تعداد تست‌های قابل اجرا نیست.
- ODS و claude-mem دارای تعداد بالای مسیرهای تست هستند؛ باید به framework و package تفکیک شوند.
- mcp-client-cli در package.json فرمان test دارد، اما هیچ مسیر تستی با heuristic دیده نشد؛ Node test runner ممکن است فایل‌های دیگری را discover کند، باید عملاً اجرا شود.
- humanish در snapshot فعلی تنها 5 فایل blob دارد: .gitignore، LICENSE، README.md، plan و report؛ فایل اجرایی یا پوشه src وجود ندارد.
- PASS/FAIL فقط پس از اجرای کنترل‌شده و ثبت command، runtime، OS، dependency lock و log ثبت خواهد شد.

## پروتکل اجرای بعدی
1. دریافت snapshot در sandbox مستقل و read-only.
2. نصب وابستگی‌ها از lockfile؛ عدم اجرای install script ناشناخته قبل از بررسی.
3. اجرای static checks و test discovery.
4. اجرای unit tests.
5. اجرای integration tests فقط با سرویس‌های mock/container ایزوله.
6. ثبت exit code، تعداد passed/failed/skipped، زمان، نسخه‌ها و log.
7. عدم استفاده از credential یا داده واقعی.
