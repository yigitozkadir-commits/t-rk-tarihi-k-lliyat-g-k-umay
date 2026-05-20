# TurkicNLP — 24 Türkî Dil NLP Kütüphanesi

## Repo
github.com/turkic-nlp/turkicnlp
Lisans: Apache 2.0 | Dil: Python | Yayın: Şubat 2026 | Aktif: Evet

## Ne Yapar
24 Türkî dil için tek tutarlı NLP pipeline.
Latin, Kiril, Arap-Farsça ve Eski Türkçe Runik — dört yazı ailesi.

## Gök Umay İçin Değeri
Bu tek repo şunları çözüyor:
- script-decoder: yazı sistemi tespiti + alfabe dönüşümü
- language-bridge: 24 dil tokenizasyon, morfoloji, çeviri
- Osmanlıca ve Eski Türkçe runik dahil

## Desteklenen Diller (Seçili)
```
Oğuz kolu:    Türkçe, Azerbaycanca, Türkmence, Gagavuzca
Kıpçak kolu:  Kazakça, Kırgızca, Başkırtça, Tatarca, Kırım Tatarcası
Karluk kolu:  Özbekçe, Uygurca
Sibirya kolu: Sahaca (Yakutça), Hakasça, Tuvaca
Tarihi:       Osmanlı Türkçesi, Eski Türkçe Runik (Orhun)
Sıfır-atış:   Karakalpakça, Kumukça, Nogayca, Karaçay-Balkarca
```

## Kurulum
```bash
pip install turkicnlp

# İlk kullanımda dil modellerini indir
python -c "import turkicnlp; turkicnlp.download('tur')"  # Türkçe
python -c "import turkicnlp; turkicnlp.download('kaz')"  # Kazakça
python -c "import turkicnlp; turkicnlp.download('uzb')"  # Özbekçe
python -c "import turkicnlp; turkicnlp.download('ota')"  # Osmanlıca
```

## Temel Kullanım
```python
import turkicnlp

# Kazakça pipeline
nlp_kaz = turkicnlp.Pipeline("kaz",
    processors=["tokenize", "pos", "lemma", "depparse"])
doc = nlp_kaz("Мен мектепке бардым")
for word in doc.words:
    print(word.text, word.pos, word.lemma)

# Özbekçe
nlp_uzb = turkicnlp.Pipeline("uzb",
    processors=["tokenize", "morph", "pos"])
doc = nlp_uzb("Men maktabga bordim")

# Osmanlıca
nlp_ota = turkicnlp.Pipeline("ota",
    processors=["tokenize", "morph"])
doc = nlp_ota("شطرنج oyunu hakkında")
```

## Alfabe Dönüşümü — Gök Umay İçin Kritik
```python
from turkicnlp import Transliterator

# Kiril Kazakça → Latin
tr = Transliterator("kaz", source="Cyrl", target="Latn")
print(tr.transliterate("Мен ойын ойнадым"))
# → "Men oyın oynadım"

# Kiril Özbekçe → Latin
tr = Transliterator("uzb", source="Cyrl", target="Latn")
print(tr.transliterate("Мен ўйнадим"))
# → "Men o'ynadim"

# Uygurca Arap → Latin
tr = Transliterator("uig", source="Arab", target="Latn")

# Eski Türkçe Runik → Latin (tek yön)
tr = Transliterator("otk", source="Runr", target="Latn")
print(tr.transliterate("𐰭𐰀𐰢𐰶𐰆𐱃"))
# → "tengrikut" (tahmin)
```

## Çok Dilli Karşılaştırma — Oyun Terimi Araştırması
```python
import turkicnlp

OYUN_TERIMI = {
    'tur': 'oyun',
    'kaz': 'ойын',
    'uzb': "o'yin",
    'kir': 'оюн',
    'ota': 'بازی',  # Osmanlıca'da Farsça etkili
}

for dil_kodu, terim in OYUN_TERIMI.items():
    nlp = turkicnlp.Pipeline(dil_kodu, processors=["morph"])
    doc = nlp(terim)
    print(f"{dil_kodu}: {terim} → {doc.words[0].feats}")
```

## Makine Çevirisi
```python
from turkicnlp import Translator

# Kazakça → Türkçe
tr = Translator(source="kaz", target="tur")
print(tr.translate("Тоғызқұмалақ ойыны"))
# → "Togyzool oyunu"

# Özbekçe → Türkçe
tr = Translator(source="uzb", target="tur")
print(tr.translate("Shaxmat o'yini"))
# → "Satranç oyunu"
```

## Gök Umay Özel Kullanım Senaryoları
```
SENARYO 1 — Kazakça Togyzool Kaynağı:
  Kiril Kazakça metin → Latin'e çevir → Türkçeye çevir
  → language-bridge Kazakça modülü

SENARYO 2 — Osmanlıca Terim Analizi:
  Osmanlıca satranç risalesi → morfoloji analizi
  → "şatranc" kökünü bul → dönem tespiti

SENARYO 3 — Yazı Sistemi Tespiti:
  Bilinmeyen metin → otomatik yazı sistemi tanı
  → script-decoder'ın otomatik yönlendirme motoru

SENARYO 4 — Terminoloji Karşılaştırma:
  "oyun" kelimesi 10 Türkî dilde → morfolojik farkları bul
  → terminology-keeper için temel veri
```

## Sınırlar
- Eski Türkçe Runik: tek yönlü transliterasyon
- Osmanlıca: morfoloji kısmi, el yazması için değil
- Düşük kaynaklı diller (Tuva, Hakas): sıfır-atış, doğruluk düşük
- Büyük model dosyaları: ilk indirme yavaş

## İlgili Tool
→ tools/turkic-languages/kaznlp.md (Kazakça özel)
→ tools/htr/kraken.md (el yazması okuma ile birlikte)

## Agent Bağlantısı
→ script-decoder: yazı sistemi tespiti + alfabe dönüşümü
→ language-bridge: tokenizasyon, morfoloji, çeviri omurgası
