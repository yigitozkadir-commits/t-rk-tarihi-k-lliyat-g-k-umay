# Gök Umay Editör — Kategori B: Arşiv & Külliyat

## Bu Kategori Ne Yapar
Dünya arşivlerine erişir, kaynakları indirir, külliyatı yönetir,
atıfları kayıt altına alır, akademik makaleleri tarar.

## Agent Ekibi

| Agent | Görev | Ne Zaman |
|-------|-------|---------|
| `archive-navigator` | Arşiv tara, erişim rehberi | Yeni kaynak aranırken |
| `corpus-builder` | Külliyat yönet, indir, kaydet | Kaynak ekleme/güncelleme |
| `citation-manager` | Zotero, Chicago/APA atıf | Her kaynak eklendiğinde |
| `research-analyst` | Makale tara, sentez | Akademik araştırma |

## İş Akışı

```
Yeni konu araştırılacak
    ↓
/arsiv-tara → archive-navigator (OAI-PMH + IIIF)
    ↓
/kaynak-indir → corpus-builder (archive.org)
    ↓
citation-manager → Zotero'ya kaydet
    ↓
/makale-ara → research-analyst (JBGS, Semantic Scholar)
    ↓
Kategori A'ya gönder (belge-oku için hazır)
```

## Slash Komutları
- `/arsiv-tara [konu]` → OAI-PMH arşiv taraması
- `/kaynak-indir [ad]` → archive.org'dan indir
- `/makale-ara [konu]` → Akademik makale tara
- `/bibliyografya` → Zotero koleksiyonundan bibliyografya

## Tool Ekosistemi

```
tools/archive-access/
  metha.md           → OAI-PMH harvester (1700+ kütüphane)
  iiif.md            → Görüntü erişimi (Bodleian, BL, Gallica)

tools/citation/
  pyzotero.md        → Zotero Python + MCP server

tools/academic-search/
  paper-search.md    → paper-search-mcp + Academix + API'lar

tools/digital-collections/
  internetarchive.md → archive.org resmi Python
```

## Kategori A ile Bağlantı
```
Bu kategori kaynak bulur ve indirir.
Kategori A o kaynağı okur ve analiz eder.

B → A köprüsü:
  corpus-builder indirdi → document-archaeologist analiz eder
  archive-navigator IIIF buldu → manuscript-reader okur
```

## Yeni Tool Ekosistemi (v2)

```
tools/web-harvesting/
  requests-lib.md    → 52.000+ yıldız, HTTP omurgası, OAI-PMH/API erişimi
  scrapy.md          → 54.000+ yıldız, toplu crawl, cyberleninka/arşiv tarama
  playwright.md      → 12.000+ yıldız, JS/giriş korumalı siteler
  httpx.md           → 14.000+ yıldız, async paralel API çağrıları

tools/data-storage/
  duckdb.md          → 32.000+ yıldız, külliyat veritabanı ve boşluk analizi

tools/search-index/
  elasticsearch.md   → 72.000+ yıldız, çok dilli tam metin arama (AR+RU+TR)
  pandas.md          → 44.000+ yıldız, metadata analiz ve boşluk haritası
  whoosh-tantivy.md  → 10.000+ yıldız, yerel/çevrimdışı külliyat arama

tools/workflow/
  airflow.md         → 38.000+ yıldız, haftalık otomatik arşiv tarama
  prefect.md         → 16.000+ yıldız, Python-native pipeline orkestratörü
```
