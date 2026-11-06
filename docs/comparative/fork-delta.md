# تحلیل اختلاف forkها با parent

برداشت 2026-11-06 UTC. روش: مقایسه path → blob SHA در درخت شاخه main هر دو مخزن. این شمارش، تعداد مسیرهای اضافه/حذف/تغییر blob است؛ مقایسه معنایی کد یا تعداد commitهای ahead/behind نیست.

| Parent | Fork | فایل parent | فایل fork | فقط در fork | فقط در parent | blob متفاوت در مسیر مشترک |
|---|---|---:|---:|---:|---:|---:|
| Martian-Engineering/lossless-claw | thedotmack/lossless-claw | 371 | 105 | 9 | 275 | 71 |
| NousResearch/hermes-agent | thedotmack/hermes-agent | 22,595 | 2,133 | 691 | 21,153 | 1,123 |
| ParadevAI/Paradev | thedotmack/superset | 15,167 | 5,344 | 1,520 | 11,343 | 1,766 |
| Osmantic/ODS | thedotmack/ODS | 5,960 | 5,818 | 0 | 142 | 452 |

## Head snapshots
| Repo | Commit SHA | Commit date |
|---|---|---|
| Martian-Engineering/lossless-claw | 51c3c90bfe82fa649e4895dd1a2c4874b5e28f76 | 2026-11-05 |
| thedotmack/lossless-claw | 4fdb53ca9949faf1e7ffaa6abadca7f8c9921130 | 2026-03-12 |
| NousResearch/hermes-agent | ca4360692055bdb4df3a695c164646e528891da8 | 2026-11-06 |
| thedotmack/hermes-agent | 73d0b083510367adec42746e90c41ace16c0afb2 | 2026-04-19 |
| ParadevAI/Paradev | 53f1ac1885e3751b939a615f5b54d1976370539b | 2026-11-06 |
| thedotmack/superset | d4b10385dcd75f3259bb163e53575cd288fac85d | 2026-05-15 |
| Osmantic/ODS | 891f722ab7cca7715fa719cff5f8b3ac3b09c1ac | 2026-11-03 |
| thedotmack/ODS | fce1a2281343cd9de1ca0622b4bbf28f3469523e | 2026-10-24 |

## برداشت‌ها
- هر چهار fork نسبت به parent فعلی head متفاوت دارند.
- lossless-claw و hermes-agent و superset به‌طور معنادار از parent فعلی کوچک‌ترند؛ این snapshotها نباید به‌عنوان نماینده نسخه جاری parent معرفی شوند.
- ODS از نظر تعداد فایل به parent نزدیک است، اما 142 مسیر parent در fork نیست و 452 مسیر مشترک محتوای متفاوت دارند.
- این روش ثابت نمی‌کند هر فایل حذف‌شده عمداً حذف شده یا هر تغییر blob یک تغییر رفتاری است؛ ممکن است تغییرات تاریخچه، ساختار، vendor، dataset یا build artifacts دخیل باشند.
- GitHub compare endpoint cross-repo در این pass خطای 404 داد؛ بنابراین ahead/behind commit count و patch-level semantic diff ثبت نشده است.

## نمونه‌های تفاوت مسیر
- lossless-claw: fork شامل src/integrity.ts و چند تست integration/index جدید است؛ در مقابل بسیاری از workflowها، agent skills و changesetهای parent در fork نیستند.
- hermes-agent: fork release notes و workflowهای مخصوص خود دارد؛ تعداد بسیار زیادی از مسیرهای parent فعلی در fork موجود نیستند.
- superset: fork دارای .superset configuration/scripts و workflowهای اختصاصی است؛ بخش بزرگی از paths parent در fork نیست.
- ODS: fork path جدیدی نسبت به parent ندارد و بخشی از مسیرهای parent را ندارد؛ باید برای هر تغییر، commit provenance بررسی شود.

## تصمیم روش‌شناختی
هر dossier باید دو محور جدا داشته باشد:
1. تحلیل خود snapshot thedotmack (این درخواست اصلی).
2. delta و provenance نسبت به parent فعلی (برای فهمیدن fork drift).
هیچ‌کدام جایگزین دیگری نیست.
