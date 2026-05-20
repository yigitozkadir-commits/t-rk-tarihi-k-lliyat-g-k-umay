# Ottoman-NLP / ottominer — Osmanlıca Metin İşleme

## Repo
github.com/Ottoman-NLP/ottominer-public
Lisans: Açık | Kurum: Boğaziçi Üniversitesi | Aktif: Evet (2024)

## Ne Yapar
Osmanlıca metin işleme araç seti + Ottoman Text Corpus (OTC).
15-20. yüzyıl arası kapsamlı translitere edilmiş dijital metin külliyatı.
İlk Osmanlıca NER dataset (HisTR) ve bağımlılık ağacı (OTA-BOUN).

## Gök Umay İçin Değeri
- Osmanlıca oyun metinleri için NER → "satranç", "nerd" gibi terimleri bul
- 15-20. yy arası Osmanlıca metinlerde oyun referansı arama
- PDF çıkarma + kodlama hatası düzeltme pipeline'ı

## Kurulum
```bash
git clone https://github.com/Ottoman-NLP/ottominer-public.git
cd ottominer-public
pip install -e .
```

## Temel Kullanım
```python
from ottominer import TextProcessor, NERModel

# Osmanlıca metin işle
processor = TextProcessor()
metin = "Şatranç oyunu sarayda oynanırdı"

# Normalize et (ligature ve kodlama sorunlarını düzelt)
normalize = processor.normalize(metin)

# NER — oyun adlarını bul
ner = NERModel()
entities = ner.predict(normalize)
for ent in entities:
    print(f"{ent.text}: {ent.label}")
# → "Şatranç": OYUN_ADI
```

## Oyun Terimi Arama
```python
def osmanlica_oyun_ara(metin_dizini: str) -> list:
    """
    Osmanlıca metinlerde oyun referanslarını bul.
    OTC külliyatında tarama için.
    """
    OYUN_TERIMLERI = [
        'şatranç', 'şıtrınc', 'satranç',
        'nerd', 'nard', 'tavla',
        'çevgan', 'cürid',
        'güreş', 'ok', 'tir-endaz',
        'oyun', 'la\'ib', 'bazi'
    ]

    from ottominer import Corpus
    corpus = Corpus(metin_dizini)
    bulgular = []

    for terim in OYUN_TERIMLERI:
        sonuclar = corpus.search(terim, context=3)  # 3 cümle bağlam
        for sonuc in sonuclar:
            bulgular.append({
                'terim': terim,
                'bagolam': sonuc.context,
                'kaynak': sonuc.document,
                'sayfa': sonuc.page
            })

    return bulgular
```

## Ottoman Text Corpus (OTC) Hakkında
```
Kapsam:    15-20. yüzyıl Osmanlıca metinler
Odak:      Tanzimat dönemi (1839-1922)
Format:    Translitere edilmiş (Latin alfabesi)
Boyut:     Büyüyen külliyat
Erişim:    GitHub repo + HuggingFace
Lisans:    Açık araştırma

Oyun için faydalı bölümler:
  → Seyahatname ve gözlem metinleri
  → Saray yaşamına dair belgeler
  → Eğlence ve spor ile ilgili dönem metinleri
```

## Agent Bağlantısı
→ manuscript-reader Osmanlıca NLP için
→ corpus-builder OTC külliyatını kaynak olarak kullanır
→ language-bridge Osmanlıca morfoloji modülü
