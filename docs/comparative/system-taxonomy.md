# طبقه‌بندی سیستم‌ها

## محور 1 — نوع محصول
| پروژه | کلاس اصلی | مسئله‌ای که حل می‌کند | خروجی/مرز اصلی |
|---|---|---|---|
| claude-mem | حافظه پایدار عامل | حفظ مشاهده و زمینه بین نشست‌ها | observer/worker، memory store، retrieval/injection |
| lossless-claw | context engine | جلوگیری از حذف تاریخچه هنگام فشرده‌سازی context | SQLite + summary DAG + context assembler |
| membench | harness ارزیابی | سنجش اثر علّی حافظه روی موفقیت عامل | experiment batch + outcome/audit report |
| mcp-client-cli | ابزار توسعه/پروتکل | قابل استفاده کردن MCP برای انسان و shell | dynamic CLI از MCP tool schemas |
| hermes-agent | agent runtime | عامل چندکاناله، چندمدلی و قابل گسترش | agent loop + tools + skills + gateways |
| superset | محیط توسعه عامل‌محور | اجرای موازی agent coding در workspaceها | desktop + API + workspace/process services |
| ODS | پلتفرم self-hosted AI | نصب و اتصال سرویس‌های محلی AI | installer + compose/service catalog + dashboard |
| humanish | credential broker | واگذاری دسترسی محدود و کوتاه‌عمر به عامل | one-time link/bearer + isolated browser/action runner |

## محور 2 — نقش در زنجیره عملیاتی
- ساخت زمینه: claude-mem، lossless-claw
- سنجش کیفیت زمینه: membench
- کشف و اجرای ابزار: mcp-client-cli
- تصمیم‌گیری/اجرای agent: hermes-agent
- محیط کار و ایزوله‌سازی توسعه: superset
- میزبانی مدل‌ها و سرویس‌ها: ODS
- کنترل دسترسی به حساب‌های وب: humanish

## نکته
این دسته‌بندی، نقش‌های مفهومی را جدا می‌کند. هم‌پوشانی احتمالی میان مخازن به معنی قابلیت اتصال آماده یا سازگاری API نیست؛ آن موارد باید با قراردادها و نسخه‌های واقعی بررسی شوند.
