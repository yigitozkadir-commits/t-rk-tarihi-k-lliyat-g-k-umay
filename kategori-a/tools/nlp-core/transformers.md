# HuggingFace Transformers — Dil Modeli Kutuphanesi

## Repo
github.com/huggingface/transformers
Lisans: Apache 2.0 | Kurum: HuggingFace | Yildiz: 143.000+ | Aktif: Evet

## Ne Yapar
BERT, GPT, T5 gibi onceden egitilmis dil modellerini calistirmak icin
standart kutüphane. Ceviri, NER, siniflandirma, metin uretimi.

## Gok Umay Icin Degeri
- CAMeLBERT: Arapca BERT — klasik Arapca metinler icin ozel
- AraT5: Arapca T5 — satranc risalesi ozeti ve cevirisi
- XLM-RoBERTa: cok dilli (Turkce, Arapca, Farsca, Rusca)
- AraBERT: Arapca NLP (klasik + modern)
- TURNA: Turkce BERT (Sabanci Univ.)

## Kurulum
```bash
pip install transformers torch
```

## Gok Umay Icin Ozel Modeller
```python
from transformers import pipeline, AutoTokenizer, AutoModelForTokenClassification

# AraBERT ile Arapca NER
ner_pipeline = pipeline(
    "ner",
    model="aubmindlab/bert-large-arabertv02",
    aggregation_strategy="simple"
)
metin_ar = "لعب الملك شطرنج في قرطبة مع الوزير"
sonuclar = ner_pipeline(metin_ar)
for e in sonuclar:
    print(f"{e['word']}: {e['entity_group']} ({e['score']:.2f})")

# XLM-RoBERTa cok dilli NER
ner_multi = pipeline("ner", model="Babelscape/wikineural-multilingual-ner")
# Rusca, Farsca, Arapca metinlerde NER
```

## Ceviri Pipeline
```python
from transformers import pipeline

# Arapca -> Ingilizce (Helsinki-NLP)
ceviri = pipeline("translation", model="Helsinki-NLP/opus-mt-ar-en")
cikti = ceviri("الشطرنج لعبة الملوك")
print(cikti[0]['translation_text'])

# Rusca -> Turkce
ceviri_ru_tr = pipeline("translation", model="Helsinki-NLP/opus-mt-ru-tr")
```

## Osmanica icin Kullanim
```python
# Osmanicadan modern Turkceye yakinlastirma
# TURNA modeli ile finetune edilen model kullan
from transformers import AutoTokenizer, AutoModelForMaskedLM

tokenizer = AutoTokenizer.from_pretrained("dbmdz/bert-base-turkish-cased")
# Ottoman metin -> Latin transkripsiyon -> TURNA -> anlam
```

## Agent Baglantisi
-> language-bridge ceviri ve NER modulleri
-> corpus-builder metin siniflandirma
