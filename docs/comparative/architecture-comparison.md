# مقایسه معماری

## الگوهای غالب
1. Event-to-memory pipeline: در claude-mem، داده تعامل/مشاهده به حافظه قابل بازیابی تبدیل می‌شود.
2. Lossless history + lossy abstraction: lossless-claw پیام خام را نگه می‌دارد، اما context فعال را با summary DAG کوچک می‌کند.
3. Controlled experiment: membench treatment (observer memory) را از outcome evaluator جدا می‌کند و چهار arm دارد: candidate، none، shuffled، reference.
4. Protocol reflection: mcp-client-cli از MCP tools/list schema به CLI command/option تبدیل می‌کند.
5. Long-lived agent runtime: hermes-agent شامل provider، tools، skills، gateways و کانال‌های گفتگو است.
6. Multi-workspace execution: superset کد و محیط‌های agent را در چند workspace سازمان‌دهی می‌کند؛ جزئیات isolation باید از process/workspace implementation تأیید شود.
7. Local service orchestration: ODS لایه نصب/compose/service registry را حول inference، UI، RAG، voice، agents و observability سازمان می‌دهد.
8. Capability brokerage: humanish بین مرورگر ورود انسانی و اجرای یک action با credential کوتاه‌عمر واسطه می‌شود.

## داده و state
| پروژه | داده پایدار/کلیدی | مورد نیاز برای اثبات |
|---|---|---|
| claude-mem | مشاهدات، خلاصه‌ها، session/project memory | schema، retention، index، deletion، sync |
| lossless-claw | conversations، messages، message_parts، summaries، context_items | migration، DAG invariants، transaction boundaries |
| membench | corpus/events، experiment spec، schedule، observer memory، attempts، audits | hash commitments، determinism، missingness |
| mcp-client-cli | server config و dynamic tool schemas | validation، process lifecycle، timeout، secret handling |
| hermes-agent | sessions، tool state، memory/skills، schedules | state ownership، concurrency، sandbox boundary |
| superset | projects/workspaces، worktrees، process/terminal state، DB | workspace isolation، IPC، DB schema، process cleanup |
| ODS | compose/service registry، configuration، secrets، model artifacts | install idempotency، service health، secret lifecycle |
| humanish | browser storageState، profiles، slug claims، short-lived bearer | crypto/key storage، TTL، atomic redemption، audit |

## رابط‌ها و قراردادها
- MCP در mcp-client-cli یک پروتکل مرزی روشن است؛ در hermes و ODS نیز نقاط اتصال MCP باید با schema و transport واقعی استخراج شوند.
- OpenClaw context engine در lossless-claw با plugin contract مشخص وابسته است.
- Agent skills در hermes و claude-mem صرفاً متن prompt نیستند؛ باید مشخص شود چه loader، permission و runtime آن‌ها را اجرا می‌کند.
- humanish رابط human-in-the-loop دارد: کاربر credential را در browser provision می‌کند و skill markdown را خودش به chat منتقل می‌کند.

## موارد نامعلوم
- سازگاری مستقیم بین نسخه‌های agent/tool/memory
- مدل مشترک هویت و مجوز بین این محصولات
- APIهای پایدار و نسخه‌بندی‌شده
- semantics حذف/فراموشی داده و export
- حدود واقعی sandbox در process و browser
