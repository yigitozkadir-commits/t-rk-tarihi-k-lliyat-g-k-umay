# Gök Umay Editör — Kategori A: Belge & Dil

## Bu Kategori Ne Yapar
Eline geçen her türlü tarihi belgeyi okur, tanır, işler, Türkçeye taşır.
Yazı sistemi tespitinden el yazması okumaya, 24 Türkî dilden Arapça/Farsça/Rusçaya.

## Agent Ekibi

| Agent | Görev | Ne Zaman |
|-------|-------|---------|
| `script-decoder` | Yazı sistemi tanı | HER ZAMAN İLK BU |
| `manuscript-reader` | El yazması oku | Yazı sistemi belirlendikten sonra |
| `language-bridge` | Çevir + kültürel bağlam | TR dışı kaynak varsa |

## İş Akışı

```
Belge geldi
    ↓
/yazi-tani → script-decoder
    ↓
El yazması mı? → manuscript-reader (Kraken + eScriptorium)
    ↓
TR dışı dil mi? → /cevir → language-bridge (TurkicNLP + CAMeL + Hazm)
    ↓
Çıktı → Kategori C analize gönder
```

## Slash Komutları
- `/yazi-tani [görüntü]` → Yazı sistemi tespit
- `/belge-oku [dosya]` → Belge analizi
- `/cevir [metin] --kaynak=[dil]` → Çeviri

## Tool Ekosistemi

```
tools/htr/
  kraken.md          → HTR motoru (%97+ doğruluk)
  escriptorium.md    → HTR arayüzü (OpenITI entegre)
  akis-dataset.md    → Osmanlıca test verisi

tools/turkic-languages/
  turkicnlp.md       → 24 Türkî dil NLP omurgası
  ottominer.md       → Osmanlıca NLP (Boğaziçi)
  kaznlp.md          → Kazakça özel NLP

tools/arabic/
  camel-tools.md     → Arapça NLP (NYU Abu Dhabi)
  openiti.md         → Arapça/Farsça metin külliyatı

tools/persian/
  hazm.md            → Farsça NLP (Timurlu metinler)

tools/mongolian-russian/
  mongolian-russian.md → Moğolca + Rusça NLP
```

## Etiket Sistemi
```
[BELGELENMİŞ]     → Kaynakta açıkça var
[YORUM]            → Mantıklı çıkarım
[EKSİK]            → Bilgi yok
[OKUNMUYOR]        → El yazmasında okunamayan kısım
```

## Yeni Tool Ekosistemi (v2)

```
tools/ocr-engines/
  paddleocr.md       → 76.000+ yıldız, 111 dil, baskı metin
  tesseract.md       → 64.000+ yıldız, klasik OCR, 116 dil

tools/nlp-core/
  transformers.md    → 143.000+ yıldız, AraBERT/XLM-RoBERTa/Helsinki
  spacy.md           → 33.000+ yıldız, NER pipeline, Arapça/Rusça
  easyocr.md         → 25.000+ yıldız, 80+ dil, hızlı prototip
  stanza.md          → Stanford NLP, 70+ dil, bağımlılık ağacı

tools/document-processing/
  docling.md         → 31.000+ yıldız, PDF→Markdown, IBM Research
  mineru.md          → 30.000+ yıldız, tablo+minyatür+formül
  surya.md           → 16.000+ yıldız, RTL sayfa düzeni

tools/multilingual/
  marker.md          → 21.000+ yıldız, toplu PDF→Markdown
  cltk.md            → Klasik Arapça/Farsça etimoloji
```
