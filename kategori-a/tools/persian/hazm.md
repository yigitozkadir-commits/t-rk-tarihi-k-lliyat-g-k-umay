# Hazm — Farsça NLP

## Repo
github.com/roshan-research/hazm
Lisans: MIT | Dil: Python | Aktif: Evet (2024)

## Ne Yapar
Farsça metin işleme için kapsamlı Python kütüphanesi.
Normalizasyon, tokenizasyon, lemmatizasyon, POS, bağımlılık analizi.

## Gök Umay İçin Değeri
- Timurlu saray metinleri (Babürname, Zafarname, Şahname)
- Farsça satranç risaleleri ve oyun referansları
- Farsça → Türkçe çeviri ön işleme

## Kurulum
```bash
pip install hazm
```

## Temel Kullanım
```python
from hazm import *

normalizer = Normalizer()
tokenizer = WordTokenizer()
tagger = POSTagger(repo_id="roshan-research/hazm-postagger",
                   model_filename="pos_tagger.model")
lemmatizer = Lemmatizer()

# Farsça oyun metni işle
metin = "شاه بازی شطرنج را در قصر انجام داد"
normalized = normalizer.normalize(metin)
tokens = tokenizer.tokenize(normalized)
tagged = tagger.tag(tokens)
lemmas = [lemmatizer.lemmatize(t) for t in tokens]

for token, tag, lemma in zip(tokens, tagged, lemmas):
    print(f"{token[0]:15} {token[1]:6} {lemma}")
```

## Oyun Araştırması için Kullanım
```python
FARSCA_OYUN_TERIMLERI = {
    'شطرنج':  'shatranj (satranç)',
    'نرد':    'nard (tavla)',
    'بازی':   'bazi (oyun — kumar çağrışımı dikkat)',
    'مهره':   'mohra (taş)',
    'صفحه':   'sahfe (tahta)',
    'نوبت':   'nawbat (sıra)',
    'چوگان':  'chawgan (çevgan)',
    'برنده':  'barandeh (kazanan)',
    'بازنده':  'bazandeh (kaybeden)',
}

def farsca_metin_analiz(metin: str) -> dict:
    """Timurlu dönemi Farsça metinde oyun izini bul"""
    from hazm import Normalizer, WordTokenizer
    
    normalizer = Normalizer()
    tokenizer = WordTokenizer()
    
    normalized = normalizer.normalize(metin)
    tokens = tokenizer.tokenize(normalized)
    
    bulgular = {}
    for token, _ in tokens:
        if token in FARSCA_OYUN_TERIMLERI:
            bulgular[token] = FARSCA_OYUN_TERIMLERI[token]
    
    return bulgular
```

## Timurlu Dönemi Kaynakları için Notlar
```
Babürname:
  → Farsça ve Türkçe karışık
  → Önce dil tespiti → Farsça bölümlere Hazm, Türkçe bölümlere TurkicNLP

Zafarname (Şeref al-Din Yazdi):
  → Saf Farsça
  → Timur'un saray hayatı → oyun referansları
  
Şahname minyatürleri açıklamaları:
  → Farsça metin → görsel oyun sahnesi bağlamı
  → Claude Vision + Hazm birlikte kullan
```

## Agent Bağlantısı
→ language-bridge Farsça modülü
→ document-archaeologist Farsça kaynak analizi
