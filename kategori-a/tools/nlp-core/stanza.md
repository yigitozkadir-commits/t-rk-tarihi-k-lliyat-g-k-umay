# Stanza — Stanford NLP Pipeline

## Repo
github.com/stanfordnlp/stanza
Lisans: Apache 2.0 | Kurum: Stanford NLP | Yildiz: 7.500+ | Aktif: Evet

## Ne Yapar
Stanford'un sinirsal NLP kutuphanesi. 70+ dil icin tam NLP pipeline.
Tokenizasyon, morfoloji, NER, bagimlilık agaci.

## Gok Umay Icin Degeri
- Arapca: klasik ve modern morfoloji analizi
- Farsca: Hazm'a gore daha derin bağımlılık agaci
- Rusca: Moğol/Kazak araştirmalarındaki Rusca kaynaklar
- TurkicNLP ile tamamlayıcı: Stanza eksik kaldığı Turkce dillerde TurkicNLP

## Kurulum
```bash
pip install stanza
python -c "import stanza; stanza.download('ar'); stanza.download('fa'); stanza.download('ru')"
```

## Temel Kullanim
```python
import stanza

# Arapca pipeline
nlp_ar = stanza.Pipeline('ar', processors='tokenize,pos,lemma,ner')
doc = nlp_ar("لعب الملك الشطرنج في القصر العظيم")

for sent in doc.sentences:
    for word in sent.words:
        print(f"{word.text:20} pos={word.pos:6} lemma={word.lemma}")

# Farsca - Timurlu metinleri icin
nlp_fa = stanza.Pipeline('fa', processors='tokenize,pos,lemma')
doc_fa = nlp_fa("شاه شطرنج بازی کرد")
for sent in doc_fa.sentences:
    for word in sent.words:
        print(f"{word.text}: {word.lemma}")
```

## Hazm ile Karsilastirma
```
Stanza:   Bagimlilik agaci, NER, 70+ dil kapsamı
Hazm:     Farsca ozel, daha derin normalizasyon
Oneri:    Hazm -> on isleme + Stanza -> tam analiz
```

## Agent Baglantisi
-> language-bridge Arapca ve Farsca detaylı analiz
-> manuscript-reader metin dogrulama ve duzeltme
