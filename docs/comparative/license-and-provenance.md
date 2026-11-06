# مجوز و منشأ کد

## ماتریس اولیه
| مخزن | metadata GitHub | فایل/وضعیت مشاهده‌شده | نتیجه عملی |
|---|---|---|---|
| claude-mem | Apache-2.0 | LICENSE و NOTICE؛ package.json نیز Apache-2.0 | قابل بررسی تحت Apache؛ third-party assets و vendored components جداگانه |
| lossless-claw | MIT | LICENSE و package.json MIT؛ fork از Martian-Engineering | بررسی تغییرات fork و noticeها |
| membench | MIT | LICENSE و NOTICE؛ SECURITY.md محدودیت privacy دارد | MIT مجوز کد است؛ محرمانگی داده‌های آزمایش جداست |
| mcp-client-cli | MIT | LICENSE و package.json MIT | MIT؛ README/SUMMARY شامل ادعاهای نمونه تاریخی‌اند |
| hermes-agent | MIT | LICENSE؛ fork از NousResearch | بررسی parent delta و plugin/skill licenses |
| superset | NOASSERTION | LICENSE.md صریحاً Elastic License 2.0 (ELv2) | **محدودکننده؛ hosted/managed service ممنوع طبق متن مجوز**؛ استفاده مجدد نیازمند بررسی حقوقی |
| ODS | Apache-2.0 | ods/LICENSING.md تصریح می‌کند mixed-license؛ Pixel دارای Pixel License for ODS؛ third-partyها مستقل | Apache برای original ODS code فقط در محدوده اعلام‌شده؛ مجوز اجزا باید جداگانه ثبت شود |
| humanish | Apache-2.0 | LICENSE موجود؛ repository snapshot فاقد implementation source | مجوز فایل‌های موجود را ثبت کن؛ dependency/licenseهای طرح هنوز باید استخراج شوند |

## منشأ / fork status
- مستقل: claude-mem، membench، mcp-client-cli، humanish.
- fork: lossless-claw ← Martian-Engineering/lossless-claw.
- fork: hermes-agent ← NousResearch/hermes-agent.
- fork: superset ← ParadevAI/Paradev.
- fork: ODS ← Osmantic/ODS.

## محدودیت بازنشر
این آرشیو فعلاً کد منبع کامل را mirror نمی‌کند. برای هر قطعه‌ای که بعداً لازم شود:
1. مسیر و blob SHA upstream را ثبت کن.
2. مجوز همان فایل/پوشه و مجوز dependency را بررسی کن.
3. notice و copyright را حفظ کن.
4. کد تغییر‌یافته را با منشأ و patch جدا ثبت کن.
5. برای ELv2 و Pixel License، قبل از هر توزیع یا hosted use بررسی حقوقی مستقل لازم است.

## نکته مهم
وجود یک مجوز در metadata ریشه، به معنی مجوز واحد برای کل monorepo، مدل‌ها، container images، فونت‌ها، artwork یا vendored dependencies نیست.
