# CLTK — Klasik Diller Arastirma Kutuphanesi

## Repo
github.com/cltk/cltk
Lisans: MIT | Kurum: Akademik topluluk | Yildiz: 4.300+ | Aktif: Evet

## Ne Yapar
Antik ve ortacag dilleri icin NLP araclari.
Arapca (klasik), Farsca, Latince, Yunan ca, Sanscritce — tarihsel diller.

## Gok Umay Icin Degeri
NOT: CLTK 10.000 yildizin altinda (4.300+) — ama akademik deger nedeniyle oneriliyor.
- Klasik Arapca: satranc risalelerinin dilinin (8-12. yy) analizi
- Sanscritce: "caturnaga" → "satranc" etimoloji arastirmasi
- Farsca ortacag: Timurlu donem metinleri icin

## Kurulum
```bash
pip install cltk
python -c "from cltk.data.fetch import FetchCorpus; FetchCorpus('arabic').import_corpus('arabic_text_perseus')"
```

## Temel Kullanim
```python
from cltk.tokenizers.word import WordTokenizer
from cltk.stop.arabic.stops import STOPS_LIST

# Klasik Arapca tokenizasyon
tokenizer = WordTokenizer(language="arabic")
metin = "كتاب الشطرنج لأبي بكر السولي"
tokenlar = tokenizer.tokenize(metin)

# Stop word temizleme
temiz = [t for t in tokenlar if t not in STOPS_LIST]
print(temiz)
```

## CAMeL Tools ile Karsilastirma
```
CLTK:       Ortacag/klasik odak, akademik arastirma
CAMeL:      Modern + klasik, aktif gelistirme, daha kapsamli
Oneri:      Etimoloji ve tarihi baglam -> CLTK; NLP pipeline -> CAMeL
```

## Agent Baglantisi
-> language-bridge klasik Arapca etimoloji notlari
-> document-archaeologist tarihsel terim analizi
