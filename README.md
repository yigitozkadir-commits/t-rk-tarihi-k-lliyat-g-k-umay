# Gök Umay — Türk Tarihi Külliyat ve Belge Editörü Sistemi

Tarihi belgeleri okur, analiz eder, arşivler arasında navigasyon yaparak AR-GE kararlarına dönüştüren entegre sistem.

## Sistem Mimarisi

```
Gök Umay (Ana Sistem)
│
├── Kategori A: Belge & Dil (kategori-a/)
│   ├── Yazı sistemi tanıma
│   ├── El yazması okuma
│   ├── 24 Türkî dil + Arapça/Farsça/Rusça çeviri
│   └── Tools: Tesseract, PaddleOCR, Kraken, eScriptorium, TurkicNLP, CAMeL
│
├── Kategori B: Arşiv & Külliyat (kategori-b/)
│   ├── 1700+ kütüphane arşiv taraması (OAI-PMH)
│   ├── Görüntü erişimi (IIIF: Bodleian, BL, Gallica)
│   ├── Kaynak indirme ve yönetme
│   └── Tools: Metha, DuckDB, Elasticsearch, Airflow
│
└── Gök Umay: Orchestration (gok-umay/)
    ├── Agent ekibi yönetimi
    ├── İş akışı kontrol
    └── AR-GE raporlama
```

## Başlangıç

### Kategori A: Belge Okuma
```
/yazi-tani [görüntü]       → Yazı sistemi tespit
/belge-oku [dosya]         → Belge analizi
/cevir [metin]             → Çeviri
```

### Kategori B: Arşiv Erişimi
```
/arsiv-tara [konu]         → Arşiv taraması
/kaynak-indir [ad]         → Archive.org'dan indir
/makale-ara [konu]         → Akademik makale tara
```

### Ana Sistem
```
/yazi-tani                 → HER ZAMAN İLK BU
/belge-oku                 → Analiz et
/cevir                     → Çevir
/arsiv-ara                 → Arşiv rehberi
/arge-durum                → Proje durumu
/gdd-taslak                → Oyun GDD taslağı
```

## Etiket Sistemi

```
[BELGELENMİŞ]     → Kaynakta açıkça var
[YORUM]            → Mantıklı çıkarım
[EKSİK]            → Bilgi yok
[ÇELİŞKİ]          → Başka kaynakla çelişiyor
[REKONSTRÜKSIYON]  → Yeniden inşa edilmiş
[OKUNMUYOR]        → El yazmasında okunamayan
```

## Etik Kurallar

1. Belirsiz kuralı kesin gibi sunma
2. Rekonstrüksiyon yaptıysan açıkça söyle
3. Kaynak yoksa uydurma — `[EKSİK]` yaz
4. Her analizin sonunda somut eylem öner

## Klasör Yapısı

```
.
├── kategori-a/              # Belge & Dil Editörü
│   ├── CLAUDE.md            # A kategorisine özel sistem rehberi
│   ├── tools/               # NLP, OCR, HTR araçları
│   │   ├── htr/             # El yazması okuma
│   │   ├── turkic-languages/ # 24 Türkî dil NLP
│   │   ├── ocr-engines/     # Tesseract, PaddleOCR
│   │   ├── nlp-core/        # Transformers, spaCy, Stanza
│   │   ├── document-processing/ # PDF→Markdown
│   │   └── multilingual/    # CLTK, Marker
│   └── .claude/
│
├── kategori-b/              # Arşiv & Külliyat Editörü
│   ├── CLAUDE.md            # B kategorisine özel sistem rehberi
│   ├── tools/               # Arşiv erişimi, kaynak yönetimi
│   │   ├── archive-access/  # OAI-PMH, IIIF
│   │   ├── citation/        # Zotero entegrasyonu
│   │   ├── academic-search/ # Makale taraması
│   │   ├── web-harvesting/  # Scrapy, Playwright
│   │   ├── data-storage/    # DuckDB külliyat
│   │   ├── search-index/    # Elasticsearch
│   │   └── workflow/        # Airflow otomasyonu
│   └── .claude/
│
├── gok-umay/                # Ana Sistem & Orchestration
│   ├── CLAUDE.md            # Sistem rehberi
│   ├── KURULUM.md           # Kurulum talimatları
│   ├── scripts/             # Otomasyonlar
│   ├── tools/               # Araç belgelemeleri
│   └── .claude/
│
└── README.md                # Bu dosya
```

## Kaynaklar

- **Kategori A CLAUDE.md**: `./kategori-a/CLAUDE.md`
- **Kategori B CLAUDE.md**: `./kategori-b/CLAUDE.md`
- **Ana Sistem CLAUDE.md**: `./gok-umay/CLAUDE.md`
- **Kurulum**: `./gok-umay/KURULUM.md`

## Gelecek Geliştirmeler

Sistem modüler olarak tasarlanmıştır. İlerleyen zamanlarda aşağıdaki eklenecek:

- [ ] Kategori C: Oyun GDD & Senaryosu
- [ ] Kategori D: Görsel Editör & Ressam Koordinasyonu
- [ ] Kategori E: Müzik & Ses Tasarımı
- [ ] Kategori F: Oyun Motoru Entegrasyonu
- [ ] Webhook & Event sistemi
- [ ] Veritabanı şeması
- [ ] REST API
- [ ] Web Arayüzü

---

**Versiyon**: 1.0  
**Son Güncelleme**: 2026-05-20  
**Durum**: Kategori A & B kurulu, production-ready