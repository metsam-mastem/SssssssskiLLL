# جمع‌بندی اجرایی فاز یک — thedotmack repositories

برداشت snapshot: 2026-11-06 UTC. این سند جمع‌بندی یافته‌های مستندات و کدهایی است که در فاز اول خوانده شدند؛ هنوز معادل ممیزی کامل همه فایل‌ها یا اجرای runtime نیست.

## 1. دامنه و موجودی
- 8 مخزن، 16,533 فایل ردیابی‌شده
- 261,243,142 بایت مجموع اندازه blobهای tracked (حدود 249.14 MiB؛ نه حجم clone)
- 4,800 مسیر منطبق با heuristic نام‌گذاری تست
- 23 فایل مستنداتی/نقشه در مخزن آرشیو تا زمان آخرین verification

## 2. کشف‌های اصلی
1. این مجموعه یک سیستم واحد نیست؛ زنجیره‌ای از حافظه، ارزیابی، پروتکل ابزار، runtime عامل، محیط توسعه، استقرار محلی و credential brokerage است.
2. lossless-claw و claude-mem هر دو context/memory را مدیریت می‌کنند، اما مدل آن‌ها متفاوت است: claude-mem از hook/worker/observation و memory search استفاده می‌کند؛ lossless-claw پیام خام را حفظ و context فعال را با summary DAG فشرده می‌کند.
3. membench از نظر روش ارزیابی یکی از دقیق‌ترین اجزای مجموعه است: چهار arm، calibration مستقل، hash-bound artifacts، paired item-level effects، bootstrap و Bonferroni، unknown handling و independent reassessment.
4. hermes-agent یک agent runtime با context engine قابل تعویض و MemoryManager است؛ MemoryManager حداکثر یک external memory provider می‌پذیرد.
5. mcp-client-cli یک bridge کوچک MCP-to-CLI است که schema ابزار را از tools/list کشف و به Commander options تبدیل می‌کند.
6. superset برای workspace/process/PTY/Git orchestration است، اما ELv2 ریسک مهم reuse/hosted service دارد.
7. ODS یک stack استقرار محلی چندسرویسی با compose overlays، installers، dashboard API و APE policy gateway است؛ V3 در مستندات خود pre-release معرفی شده و mixed-license است.
8. humanish در snapshot فعلی فقط طراحی است؛ هیچ implementation source وجود ندارد.

## 3. ریسک‌های برجسته
- مجوز: superset تحت Elastic License 2.0؛ ODS mixed-license؛ مجوز اجزای ثالث باید component-level بررسی شود.
- منشأ: 4 مخزن fork هستند و snapshot fork با parent فعلی تفاوت قابل توجه دارد.
- امنیت: credential broker، agent tool execution، process/PTY، installer و service exposure مرزهای حساس‌اند.
- کیفیت شواهد: README claims نباید با runtime verification اشتباه شوند.
- تست: 4,800 مسیر heuristic، اجرای تست نیست؛ تمام test suites فعلاً NOT RUN هستند.

## 4. وضعیت forkها
- lossless-claw: parent 371 files، fork 105؛ 9 only-in-fork، 275 only-in-parent، 71 changed paths.
- hermes-agent: parent 22,595، fork 2,133؛ 691 only-in-fork، 21,153 only-in-parent، 1,123 changed paths.
- superset: parent 15,167، fork 5,344؛ 1,520 only-in-fork، 11,343 only-in-parent، 1,766 changed paths.
- ODS: parent 5,960، fork 5,818؛ 0 only-in-fork، 142 only-in-parent، 452 changed paths.
این اعداد path/blob comparison هستند، نه semantic diff یا commit ahead/behind.

## 5. وضعیت شواهد
| سطح | وضعیت |
|---|---|
| L0 هویت و snapshot | PASS برای 8 مخزن |
| L1 درخت و inventory | PASS اولیه؛ تحلیل عمیق مسیرها در جریان |
| L2 معماری و interfaces | PARTIAL؛ برای چند مخزن source-level بررسی شد |
| L3 الگوریتم و threat model | PARTIAL؛ نیازمند trace عمیق‌تر |
| L4 build/test/runtime | NOT RUN |

## 6. اولویت ادامه
1. humanish: یافتن implementation واقعی یا توقف تحلیل اجرایی؛ تکمیل threat model طرح.
2. lossless-claw: trace تراکنش‌ها، migration، compaction، expansion authorization و DAG invariants.
3. membench: بررسی کامل protocol، statistical inference و release gate؛ اجرای synthetic offline demo.
4. claude-mem: data schema، retention, sync, redaction, deletion و provider boundary.
5. mcp-client-cli: MCP framing، timeouts، child process lifecycle و hostile server tests.
6. hermes-agent: agent loop، tool registry، gateway auth، plugin/skill sandbox و fork-specific delta.
7. superset: license boundary، workspace isolation، PTY daemon protocol و process cleanup.
8. ODS: installer contract، compose/service graph، APE policy, secrets lifecycle، extension license map و platform test matrix.

## 7. تصمیمات
- هیچ‌کدام از مخازن upstream تغییر نکرده‌اند.
- هیچ کدی از این مخازن به این آرشیو mirror نشده؛ فقط مستندات تحلیلی و pinned source links ثبت شده‌اند.
- هیچ ادغام یا استفاده مجدد در پروژه‌های دیگر انجام نشده است.
- اجرای تست و deployment در محیط ایزوله هنوز NOT RUN است.
