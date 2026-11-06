# فهرست مخازن و snapshot اولیه

برداشت: 2026-11-06 UTC. همه شاخه‌های پیش‌فرض بررسی‌شده main هستند.

| مخزن | فایل | بایت blob | تست‌یاب heuristic | مجوز metadata | منشأ |
|---|---:|---:|---:|---|---|
| [claude-mem](https://github.com/thedotmack/claude-mem) | 3,030 | 59,485,747 | 1,543 | Apache-2.0 | مستقل |
| [lossless-claw](https://github.com/thedotmack/lossless-claw) | 105 | 1,761,693 | 20 | MIT | fork از Martian-Engineering/lossless-claw |
| [membench](https://github.com/thedotmack/membench) | 83 | 847,584 | 25 | MIT | مستقل |
| [mcp-client-cli](https://github.com/thedotmack/mcp-client-cli) | 15 | 41,429 | 0 | MIT | مستقل |
| [hermes-agent](https://github.com/thedotmack/hermes-agent) | 2,133 | 37,027,305 | 665 | MIT | fork از NousResearch/hermes-agent |
| [superset](https://github.com/thedotmack/superset) | 5,344 | 83,927,768 | 333 | NOASSERTION | fork از ParadevAI/Paradev |
| [ODS](https://github.com/thedotmack/ODS) | 5,818 | 78,071,722 | 2,214 | Apache-2.0 | fork از Osmantic/ODS |
| [humanish](https://github.com/thedotmack/humanish) | 5 | 79,894 | 0 | Apache-2.0 | مستقل |
| **جمع** | **16,533** | **261,243,142** | **4,800** | — | — |

## Tree SHA
| مخزن | Tree SHA |
|---|---|
| claude-mem | 8954effe93546e9cd55b793a878f4bf855c69b22 |
| lossless-claw | 4fdb53ca9949faf1e7ffaa6abadca7f8c9921130 |
| membench | fe61cf0a484c3e7932888e4f366b5aaf5aad938c |
| mcp-client-cli | a9e7795a1dd14a21040cc260557cc20a91927edc |
| hermes-agent | 73d0b083510367adec42746e90c41ace16c0afb2 |
| superset | d4b10385dcd75f3259bb163e53575cd288fac85d |
| ODS | fce1a2281343cd9de1ca0622b4bbf28f3469523e |
| humanish | 84ece3e1c4290e000d4685492182ccab1387c84a |

## تفسیر
261,243,142 بایت مجموع اندازه blobهای درخت Git است، نه حجم clone یا دانلود ZIP. شمارش 4,800 مسیر تست heuristic است. superset دارای NOASSERTION در metadata است؛ فایل LICENSE.md و مجوز اجزا باید جداگانه بررسی شوند. چهار مخزن fork هستند و باید با parent مقایسه شوند.
