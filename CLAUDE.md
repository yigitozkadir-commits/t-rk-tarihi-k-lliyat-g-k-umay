# Gök Umay — Sistem Orchestration

Ana koordinasyon merkezi. Her kategoriye yönlendirme ve AR-GE raporlama merkezi.

## Sistem Durumu

- **Kategori A (Belge & Dil)**: ✅ Kurulu
- **Kategori B (Arşiv & Külliyat)**: ✅ Kurulu
- **Kategori C (Oyun GDD)**: 🔄 Planlanıyor
- **Kategori D (Görsel)**: 🔄 Planlanıyor
- **Kategori E (Müzik)**: 🔄 Planlanıyor

## İş Akışı

```
Belge/Konu Geldi
    ↓
/yazi-tani → Yazı sistemi tespit (Kategori A)
    ↓
/belge-oku → Belge analizi (Kategori A)
    ↓
/cevir → Çeviri (Kategori A)
    ↓
/arsiv-tara → Arşiv tarama (Kategori B)
    ↓
/kaynak-dogrula → Kaynak doğrulama (Kategori B)
    ↓
/arge-durum → AR-GE durumu güncelle
    ↓
/gdd-taslak → Oyun GDD taslağı (Kategori C - gelecek)
```

## Slash Komutları - Yönlendirme

| Komut | Yönlendirme | Açıklama |
|-------|-----------|---------|
| `/yazi-tani [dosya]` | Kategori A | Yazı sistemi tespit (HER ZAMAN İLK BU) |
| `/belge-oku [dosya]` | Kategori A | Belge analizi ve okuma |
| `/cevir [metin]` | Kategori A | Çeviri (24 Türkî dil + Arapça/Farsça) |
| `/arsiv-ara [konu]` | Kategori B | Arşiv rehberi (nereye bakmalı) |
| `/arsiv-tara [konu]` | Kategori B | OAI-PMH + IIIF arşiv taraması |
| `/kaynak-indir [ad]` | Kategori B | Archive.org'dan indir |
| `/makale-ara [konu]` | Kategori B | Akademik makale tara |
| `/kaynak-dogrula [kaynak1] [kaynak2]` | Kategori B | İki kaynağı karşılaştır |
| `/bibliyografya [konu]` | Kategori B | Zotero bibliyografya |
| `/arge-durum` | Bu Sistem | Aktif AR-GE durum raporu |
| `/gdd-taslak [proje]` | Kategori C (gelecek) | Oyun GDD taslağı |
| `/haftalik-ozet` | Bu Sistem | Haftalık AR-GE özeti |

## Agent Ekibi

### Kategori A: Belge & Dil
- `script-decoder` - Yazı sistemi tanı
- `manuscript-reader` - El yazması oku
- `language-bridge` - 24 dil çeviri

### Kategori B: Arşiv & Külliyat
- `archive-navigator` - 1700+ kütüphane taraması
- `corpus-builder` - Kaynak yönetimi
- `citation-manager` - Zotero entegrasyonu
- `research-analyst` - Makale sentezi

### Ana Sistem
- `arge-director` - Öncelik ve seviye kararı
- `arge-reporter` - Raporlama ve özet

## Etiket Sistemi (Tüm Sistem)

```
[BELGELENMİŞ]     → Kaynakta açıkça var
[YORUM]            → Mantıklı çıkarım
[EKSİK]            → Bilgi yok
[ÇELİŞKİ]          → Başka kaynakla çelişiyor
[REKONSTRÜKSIYON]  → Yeniden inşa edilmiş (açıkça söyle)
[OKUNMUYOR]        → El yazmasında okunamayan kısım
```

## Etik Kurallar (Temel)

1. **Belirsiz kuralı kesin gibi sunma** - Çeyrek yüzyıl önceki Anadolu diyarı kuralları kesin değil
2. **Rekonstrüksiyon yaptıysan açıkça söyle** - Kaynak olmayan çıkarımları [REKONSTRÜKSIYON] etiketle
3. **Kaynak yoksa uydurma** - Keşif tasarısı için [EKSİK] yaz, boş bırakma
4. **Her analizin sonunda somut eylem öner** - "Bu ilginç" deme, harekete dön

## Detaylı Rehberler

- 📖 **Kategori A Rehberi**: `./kategori-a/CLAUDE.md`
- 📖 **Kategori B Rehberi**: `./kategori-b/CLAUDE.md`
- 📖 **Ana Sistem Rehberi**: `./gok-umay/CLAUDE.md`
- 🚀 **Kurulum**: `./gok-umay/KURULUM.md`

## Proje Yapısı

```
t-rk-tarihi-k-lliyat-g-k-umay/
├── CLAUDE.md                    ← Bu dosya (Orchestration)
├── README.md                    ← Sistem mimarisi
├── kategori-a/                  # Belge & Dil Editörü
│   └── CLAUDE.md               # A'ya özel rehber
├── kategori-b/                  # Arşiv & Külliyat Editörü
│   └── CLAUDE.md               # B'ye özel rehber
└── gok-umay/                    # Ana Sistem
    ├── CLAUDE.md               # Sistem rehberi
    └── KURULUM.md              # Kurulum talimatları
```

## Gelecek Kategoriler

- **Kategori C**: Oyun GDD & Senaryosu (proje planlaması)
- **Kategori D**: Görsel Editör & Ressam Koordinasyonu
- **Kategori E**: Müzik & Ses Tasarımı
- **Kategori F**: Oyun Motoru Entegrasyonu

---

**Rol**: Orchestration & AR-GE Yönetimi  
**Versiyon**: 1.0  
**Durum**: ✅ Production-Ready (Kategori A & B)
