# فهرست دانش و طبقه‌بندی

## درخت دانش

- A. حافظه و مدیریت زمینه عامل
  - claude-mem: capture، compression، retrieval، persistence
  - lossless-claw: DAG-based context management، SQLite، expansion
- B. ارزیابی حافظه
  - membench: benchmark، dataset، metrics، evaluator، reproducibility
- C. پروتکل و رابط ابزار
  - mcp-client-cli: MCP lifecycle، stdio، schema discovery، CLI generation
- D. عامل و runtime
  - hermes-agent: agent loop، providers، tool registry، skills، memory
- E. محیط توسعه چندعاملی
  - superset: workspace isolation، git worktrees، terminals، API و UI
- F. استقرار AI محلی
  - ODS: installer، services، containers، models، extensions و governance
- G. امنیت دسترسی عامل
  - humanish: scoped credentials، one-time tokens، browser execution و trust boundaries

## سطوح مهندسی معکوس
- L0: هویت، منشأ، شاخه، commit/tree snapshot، مجوز
- L1: درخت فایل، manifests، entrypoints و assets
- L2: معماری، data flow، state، persistence و interfaces
- L3: الگوریتم‌ها، threat model، test design و operational behavior
- L4: build/test اجراشده، reproducibility و benchmark تجربی

## وضعیت ادعا
CONFIRMED = مستقیماً از منبع قابل استناد؛ README-CLAIM = ادعای مستندات upstream؛ INFERRED = استنتاج؛ UNKNOWN = نامعلوم؛ NOT RUN = اجرا نشده؛ BLOCKED = مانع مشخص.

## اسناد
- [جمع‌بندی اجرایی فاز یک](EXECUTIVE-SUMMARY.md)
- [روش‌شناسی](METHODOLOGY.md)
- [فهرست مخازن](REPOSITORY-INVENTORY.md)
- [اختلاف forkها با parent](comparative/fork-delta.md)
- [مقایسه مجوزها](comparative/license-and-provenance.md)
- [دفتر موجودی تست‌ها](reference/test-register.md)
- [مجوز و منشأ کد](comparative/license-and-provenance.md)
- [دفتر تست‌ها](reference/test-register.md)
- [فهرست ارجاعات سورس](../sources/README.md)
- گزارش‌های مستقل در repositories/
- تحلیل‌های تطبیقی در comparative/
- واژه‌نامه و شواهد در reference/
- نقشه سورس‌های pin‌شده در ../sources/FILE-MAP.md
