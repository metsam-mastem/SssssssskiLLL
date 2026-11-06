# دفتر شواهد

## Snapshotهای upstream
همه موارد از GitHub API و شاخه پیش‌فرض main در برداشت 2026-11-06 ثبت شده‌اند.

| Repo | Commit/Tree SHA | منبع درخت |
|---|---|---|
| claude-mem | 8954effe93546e9cd55b793a878f4bf855c69b22 | https://github.com/thedotmack/claude-mem/tree/main |
| lossless-claw | 4fdb53ca9949faf1e7ffaa6abadca7f8c9921130 | https://github.com/thedotmack/lossless-claw/tree/main |
| membench | fe61cf0a484c3e7932888e4f366b5aaf5aad938c | https://github.com/thedotmack/membench/tree/main |
| mcp-client-cli | a9e7795a1dd14a21040cc260557cc20a91927edc | https://github.com/thedotmack/mcp-client-cli/tree/main |
| hermes-agent | 73d0b083510367adec42746e90c41ace16c0afb2 | https://github.com/thedotmack/hermes-agent/tree/main |
| superset | d4b10385dcd75f3259bb163e53575cd288fac85d | https://github.com/thedotmack/superset/tree/main |
| ODS | fce1a2281343cd9de1ca0622b4bbf28f3469523e | https://github.com/thedotmack/ODS/tree/main |
| humanish | 84ece3e1c4290e000d4685492182ccab1387c84a | https://github.com/thedotmack/humanish/tree/main |

## فایل‌های با محتوای واقعی دریافت‌شده
- claude-mem: README.md، package.json، LICENSE در فهرست فایل‌های کلیدی؛ SHAها در فراخوانی‌های برداشت ثبت شده‌اند.
- lossless-claw: README.md، docs/architecture.md، package.json، LICENSE.
- membench: README.md، docs/methodology.md، package.json، LICENSE، SECURITY.md.
- mcp-client-cli: README.md، package.json، LICENSE، SUMMARY.md.
- hermes-agent: README.md، pyproject.toml، LICENSE، AGENTS.md، SECURITY.md (برخی فایل‌ها نیازمند استخراج دوباره و ثبت SHA دقیق‌اند).
- superset: README.md، package.json، LICENSE.md، AGENTS.md.
- ODS: README.md، ARCHITECTURE.md، SECURITY_AUDIT.md، LICENSE، ods/LICENSING.md.
- humanish: README.md، LICENSE، plans/2026-05-14-humanish.md، reports/2026-05-14-humanish-server.md.

## موارد نیازمند تکمیل
- ثبت blob SHA دقیق برای تمام اسناد کلیدی.
- commit-to-parent diff برای هر چهار fork.
- فهرست کامل paths و اندازه‌ها به JSON/CSV machine-readable.
- پیوند ادعاها به line ranges.
- اجرای build/test و ثبت logs؛ فعلاً NOT RUN.
