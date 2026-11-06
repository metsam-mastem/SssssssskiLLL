# امنیت، حریم خصوصی و مرز اعتماد

## اولویت ریسک به تفکیک مخزن
| پروژه | مرز اعتماد حساس | بررسی اجباری |
|---|---|---|
| claude-mem | transcript/observation → model/provider → memory → future prompt | redaction، private tags، telemetry، cloud sync، retention، prompt injection |
| lossless-claw | OpenClaw session → DB/summarizer → context returned to agent | tool output trust، expansion authorization، FTS query sanitation، DB access |
| membench | experiment data → observer/executor/judge → report | synthetic-only policy، secret scans، artifact provenance، blinded evaluation |
| mcp-client-cli | local config → child process/server → dynamic schema/tool output | shell injection، env secrets، argument coercion، untrusted server responses |
| hermes-agent | model output → tool execution/gateway/plugins/skills | command execution, SSRF, provider secrets, plugin trust, session isolation |
| superset | desktop UI/API → worktree/process/terminal/IPC | cross-workspace access، path traversal، PTY privilege، local DB, process escape |
| ODS | installer → containers/services → host/network/models | supply chain، privileged containers، network exposure، secret generation، update trust |
| humanish | human browser login → encrypted storageState → one-time bearer → action runner | token replay، scope bypass، AES-GCM nonce/key handling، SSRF، browser cleanup |

## یافته‌های قطعی مهم
- membench/SECURITY.md: مخزن باید private بماند و درج credential، داده واقعی کاربر، transcript یا artifact تولیدی در commit/issue/fixture/log ممنوع است. این محدودیت باید در هر اجرای آزمایشی رعایت شود.
- superset/LICENSE.md: مجوز ریشه Elastic License 2.0 است، نه مجوز OSI-approved مانند MIT/Apache. متن محدودیت ارائه نرم‌افزار به‌صورت hosted/managed service و محدودیت تغییر/دور زدن license-key را دارد؛ قبل از reuse یا ارائه سرویس تجاری باید حقوقی بررسی شود.
- ODS/ods/LICENSING.md: مخزن mixed-license است؛ کد اصلی ODS تحت Apache-2.0، Pixel تحت Pixel License for ODS، و third-party services/assets/models تحت شرایط جداگانه‌اند.
- humanish/README.md: معماری توصیف‌شده شامل Cloudflare Worker و Durable Object برای slug single-use، و Fly.io container با neko و Playwright action runner است. این ادعاها هنوز با اجرای امنیتی تأیید نشده‌اند.
- ODS/SECURITY_AUDIT.md: سند خودش صریحاً تاریخی است (audit مارس 2026 و status review مه 2026) و نمی‌توان آن را گواه امنیت snapshot جاری یا نصب فعال دانست.

## کنترل‌های لازم قبل از هر اجرا
1. فقط sandbox بدون credential واقعی و داده مصنوعی.
2. network egress محدود؛ ثبت مقصدهای outbound.
3. secrets از محیط جدا و کوتاه‌عمر؛ عدم چاپ در log.
4. process/container non-root و حداقل capability.
5. فایل‌سیستم read-only تا حد امکان؛ مسیرهای mount محدود.
6. kill/timeout و cleanup برای child process، browser و container.
7. سنجش cross-tenant/workspace/session access.
8. بازبینی third-party licenses و SBOM پیش از توزیع.

## حکم فعلی
هیچ‌یک از مخازن در این پروژه هنوز اجرای adversarial یا penetration test دریافت نکرده‌اند. وضعیت امنیت اجرایی همه: NOT RUN.
