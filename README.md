# Лаборатори №6: Category-Partition ба PICT

**Оюутан:** С.Асилан
**Оюутны код:** B232270115
**Хичээл:** F.CSA313 — Программ хангамжийн чанарын баталгаа ба тест

## 1. Зорилго

Энэ лабораторийн ажлаар банкны шилжүүлгийн тест кейсүүдийг зохиож сурлаа. Эхлээд Category-Partition аргаар параметрүүд болон тэдгээрийн утгуудыг сонгосон. Дараа нь PICT програм ашиглан тестийн хувилбарууд үүсгэж, гар аргаар хийсэн тесттэй харьцуулсан.

## 2. A1 — Сонголт ба утгууд

| Параметр | Сонголт | Утга |
|---|---|---|
| From | Данс хүчинтэй | Valid |
| From | Данс царцаагдсан | Frozen |
| From | Данс байхгүй | Missing |
| To | Өөр хүчинтэй данс | Valid |
| To | Данс байхгүй | Missing |
| To | From-той ижил данс | SameAsFrom |
| Amount | Дүн тэг буюу сөрөг | Zero |
| Amount | Зөв дүн | Normal |
| Amount | Үлдэгдлээс их | OverBalance |
| Amount | Өдрийн лимитээс их | OverLimit |
| Currency | Дансны валюттай ижил | Same |
| Currency | Дансны валютаас өөр | Different |

Мөн дансны үлдэгдэл, данс царцаагдсан эсэх, өдрийн лимитийн үлдэгдэл, валютын ханш зэрэг орчны нөхцөл нөлөөлнө.

## 3. A2 — Хязгаарлалт

| Утга | Тэмдэглэгээ | Тайлбар |
|---|---|---|
| From = Missing | [ERROR] | Эхлэх данс байхгүй тул алдаа гарна. |
| From = Frozen | [ERROR] | Эхлэх данс царцаагдсан тул шилжүүлэг хийхгүй. |
| To = Missing | [ERROR] | Хүлээн авагчийн данс байхгүй. |
| To = SameAsFrom | [ERROR] | Өөрийнхөө данс руу шилжүүлж болохгүй. |
| Amount = Zero | [ERROR] | Шилжүүлэх дүн 0-ээс их байх ёстой. |
| Amount = OverBalance | [ERROR] | Дансны үлдэгдлээс их дүн шилжүүлж болохгүй. |
| Amount = OverLimit | [ERROR] | Өдрийн лимитээс хэтэрч болохгүй. |
| Amount = Normal | [SINGLE] | Бусад нөхцөл зөв үед амжилттай шилжүүлгийг шалгана. |
| From = Missing үед Amount = Normal | [IF/property] | Данс байхгүй үед дүнгийн өөр алдаа давхар оруулахгүй. |
| To = Missing үед Amount = Normal | [IF/property] | Хүлээн авагч байхгүй үед дүнгийн өөр алдаа давхар оруулахгүй. |
| Currency = Different | [IF/property] | Валют өөр бол дүнг хөрвүүлсний дараа шалгана. |

### Хослолын тооцоо

Хязгаарлалтгүй үед:

3 × 3 × 4 × 2 = **72 хослол**

Хязгаарлалт хэрэглэсний дараа:

- From болон To хоёулаа Missing биш: 2 × 2 × 4 = 16
- From эсвэл To-ийн аль нэг Missing: 5 × 1 = 5
- Currency-ийн 2 утгыг тооцвол: (16 + 5) × 2 = 42

Иймээс хослолын тоо **72 → 42** болж буурсан.

## 4. A3 — Тест кейсүүд

Олон алдаа зэрэг гарвал дараах дарааллаар шалгана гэж сонгосон:

**ERROR_NO_FROM → ERROR_FROZEN → ERROR_NO_TO → ERROR_SAME_ACCOUNT → ERROR_BAD_AMOUNT → ERROR_INSUFFICIENT → ERROR_LIMIT**

| № | From | To | Amount | Currency | Хүлээгдэх үр дүн |
|---:|---|---|---|---|---|
| 1 | Valid | Valid | Normal | Same | HTTP 201, `{"result":"OK"}` |
| 2 | Missing | Valid | Normal | Same | HTTP 200, `{"result":"ERROR_NO_FROM"}` |
| 3 | Frozen | Valid | Normal | Same | HTTP 200, `{"result":"ERROR_FROZEN"}` |
| 4 | Valid | Missing | Normal | Same | HTTP 200, `{"result":"ERROR_NO_TO"}` |
| 5 | Valid | SameAsFrom | Normal | Same | HTTP 200, `{"result":"ERROR_SAME_ACCOUNT"}` |
| 6 | Valid | Valid | Zero | Same | HTTP 200, `{"result":"ERROR_BAD_AMOUNT"}` |
| 7 | Valid | Valid | OverBalance | Same | HTTP 200, `{"result":"ERROR_INSUFFICIENT"}` |
| 8 | Valid | Valid | OverLimit | Same | HTTP 200, `{"result":"ERROR_LIMIT"}` |
| 9 | Valid | Valid | Normal | Different | Хөрвүүлсний дараа нөхцөл зөв бол HTTP 201, `{"result":"OK"}` |
| 10 | Missing | Missing | Zero | Same | HTTP 200, `{"result":"ERROR_NO_FROM"}` |
| 11 | Frozen | Valid | Zero | Same | HTTP 200, `{"result":"ERROR_FROZEN"}` |
| 12 | Valid | Missing | OverLimit | Different | HTTP 200, `{"result":"ERROR_NO_TO"}` |

Эдгээр тестээр амжилттай шилжүүлэг болон бүх алдааны төрлийг шалгаж болно. Давхар алдаатай тестүүдэд өөрийн сонгосон дарааллын дагуу хүлээгдэх үр дүнг бичсэн.

## 5. B1 — Browser тест

Browser загварт 6 параметр ашигласан. Бүх боломжит хослолын тоо:

3 × 3 × 2 × 2 × 2 × 2 = **144**.

| Тестийн төрөл | Мөрийн тоо |
|---|---:|
| PICT 2-way | 10 |
| PICT 3-way | 20 |
| Бүрэн хослол | 144 |

2-way тест 10 мөр гарсан. Лекц дээр гараар 9 мөр гаргасан байсан ч PICT нэг мөрөөр олон гаргасан. Учир нь PICT greedy алгоритм ашигладаг бөгөөд хамгийн цөөн мөрийг заавал гаргахгүй. Харин шаардлагатай хослолуудыг хамруулахыг зорьдог.

## 6. B2 — Transfer тест

Гурван өөр загвар ашигласан:

- `transfer-nc.model` — хязгаарлалтгүй загвар.
- `transfer.model` — IF нөхцөлтэй загвар.
- `transfer-neg.model` — сөрөг утгуудыг `~` тэмдэглэгээгээр ялгасан загвар.

### Үр дүн

| Файл | Тестийн мөр |
|---|---:|
| `results/transfer-nc.txt` | 13 |
| `results/transfer.txt` | 12 |
| `results/transfer-neg.txt` | 16 |

Хязгаарлалтгүй загварт зарим мөрөнд хэд хэдэн алдаатай утга зэрэг орж болно. Харин `~` тэмдэглэгээтэй загварт нэг мөрөнд хамгийн ихдээ нэг сөрөг утга орно.

`transfer-nc` болон `transfer-neg` гаралтад хүчинтэй `Valid, Valid, Normal` утгуудтай мөр байгаа. Харин `transfer` загварт бүх параметр зөв, Amount = Normal байх мөр гараагүй.

## 7. Бодит тест кейсүүд

### Тест 1 — Амжилттай шилжүүлэг

- **Setup:** A дансанд 100,000₮ байна. Данс царцаагдаагүй. B нь өөр, хүчинтэй данс. Өдрийн лимит хангалттай.
- **Input:**

```json
{"from":"A","to":"B","amount":10000,"currency":"MNT"}
```

- **Oracle:** HTTP 201, `{"result":"OK"}`.

### Тест 2 — Эхлэх данс байхгүй

- **Setup:** A данс байхгүй, B данс хүчинтэй.
- **Input:**

```json
{"from":"UNKNOWN","to":"B","amount":10000,"currency":"MNT"}
```

- **Oracle:** HTTP 200, `{"result":"ERROR_NO_FROM"}`.

### Тест 3 — Дүн тэг

- **Setup:** A, B данс хүчинтэй. A дансны үлдэгдэл болон өдрийн лимит хангалттай.
- **Input:**

```json
{"from":"A","to":"B","amount":0,"currency":"MNT"}
```

- **Oracle:** HTTP 200, `{"result":"ERROR_BAD_AMOUNT"}`.

Эдгээр нь тестийн жишээ бөгөөд бодит API дээр ажиллуулж баталгаажуулсан үр дүн биш.

## 8. Дүгнэлт

Энэ лабораторийн ажлаар Category-Partition болон PICT ашиглан тест зохиох аргыг сурсан. Category-Partition аргаар параметрүүдийг ангилж, боломжит утгуудыг сонгосон. Мөн ямар нөхцөлд алдаа гарахыг тодорхойлсон. PICT ашиглан pairwise тестүүдийг автоматаар үүсгэж үзсэн. Хязгаарлалтгүй загварт зарим утга бодит нөхцөлд тохирохгүй байж болохыг анзаарсан. IF нөхцөл ашигласнаар зарим шаардлагагүй хослолыг хасаж болно. `~` тэмдэглэгээ нь сөрөг утгуудыг тусад нь авч үзэхэд хэрэгтэй байлаа. Гараар хийсэн тест нь алдааны шалтгаан болон хүлээгдэх үр дүнг тодорхой бичихэд илүү тохиромжтой. PICT нь олон хослолыг системтэй хамруулахад илүү тохиромжтой. Хамгийн их бодох шаардлагатай хэсэг нь давхар алдааны дараалал болон хязгаарлалтыг сонгох байв.

## 9. Файлын бүтэц

```text
lab06-category-partition/
├── README.md
├── browser.model
├── transfer-nc.model
├── transfer.model
├── transfer-neg.model
├── .gitignore
└── results/
    ├── pict-build.txt
    ├── browser-2way.txt
    ├── browser-3way.txt
    ├── transfer-nc.txt
    ├── transfer.txt
    └── transfer-neg.txt
```
