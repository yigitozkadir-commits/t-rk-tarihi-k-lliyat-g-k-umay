# spaCy — Endustriyel NLP Kutuphanesi

## Repo
github.com/explosion/spaCy
Lisans: MIT | Kurum: Explosion AI | Yildiz: 33.000+ | Aktif: Evet

## Ne Yapar
Uretim kaliteli NLP pipeline. NER, POS, bagimlılık analizi, metin siniflandirma.
Arapca, Farsca, Rusca dahil 60+ dil icin hazir model.

## Gok Umay Icin Degeri
- Arapca NER: satranc risalelerinde yazar/yer/tarim adlari
- Rusca: Sovyet etnografya makalelerinde oyun referansi tespiti
- Farsca pipeline + CAMeL Tools ile birlikte kullanimda
- Ozel NER modeli egitimi: "OYUN_ADI", "TAHTA", "HAMLE" etiketleri

## Kurulum
```bash
pip install spacy
python -m spacy download ar_core_news_sm   # Arapca
python -m spacy download ru_core_news_sm   # Rusca
```

## Temel Kullanim
```python
import spacy

# Arapca oyun terimi tespiti
nlp_ar = spacy.load('ar_core_news_sm')
metin = "لعب الملك شطرنج في القصر مع الوزير"
doc = nlp_ar(metin)

for ent in doc.ents:
    print(f"{ent.text}: {ent.label_}")

for token in doc:
    print(f"{token.text}: {token.pos_} | {token.dep_}")
```

## Oyun Arastirmasi icin Ozel NER
```python
import spacy
from spacy.tokens import DocBin

# Ozel etiketler icin egitim verisi olustur
EGITIM_VERISI = [
    ("Satranc oyunu sarayda oynandi", {"entities": [(0, 7, "OYUN_ADI")]}),
    ("Togyzool kazan kazdi", {"entities": [(0, 8, "OYUN_ADI")]}),
]

# Modeli egit ve kaydet
# -> language-bridge NER ciktisini kullanir
```

## Agent Baglantisi
-> language-bridge Arapca/Rusca modul icerisinde
-> corpus-builder metin siniflandirma ve filtreleme
