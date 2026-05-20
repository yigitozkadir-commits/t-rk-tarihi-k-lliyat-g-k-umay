# Moğolca NLP Kaynakları

## Repo
github.com/tugstugi/mongolian-nlp
Lisans: Çeşitli | Aktif: Kaynak derlemesi

## Ne Yapar
Moğolca NLP için kapsamlı kaynak derlemesi.
Sözlük, model, veri seti — hepsi bir arada.

## Gök Umay İçin Değeri
- Hiashatar (Moğol satranç) araştırması için temel
- Kiril Moğolca metin işleme
- Tohoku Üniversitesi dijital Moğolca sözlük

## Kurulum ve Kullanım
```python
# TurkicNLP Moğolca modülü (önerilen — bkz. turkicnlp.md)
import turkicnlp
turkicnlp.download('mon')
nlp = turkicnlp.Pipeline('mon', processors=['tokenize', 'pos'])

# Moğolca sözlük (Tohoku)
# URL: https://hkuri.cneas.tohoku.ac.jp/project3/
# Moğolca oyun terimleri için başvuru sözlüğü

# mongolian-bert (HuggingFace)
from transformers import pipeline
classifier = pipeline('text-classification',
    model='tugstugi/mongolian-bert-base-cased')
```

## Moğolca Oyun Terimleri
```python
MOGOLCA_OYUN_TERIMLERI = {
    'шатар':     'shatar (Moğol satranç)',
    'тоглоом':   'togloom (oyun)',
    'тоглогч':   'tologch (oyuncu)',
    'нүүлт':     'nüylt (hamle)',
    'самбар':    'sambar (tahta)',
    'хаан':      'khaan (han/kral taşı)',
    'ялагч':     'yalagch (kazanan)',
}

def mogolca_hiashatar_analiz(metin: str) -> dict:
    """Moğolca metinde Hiashatar referansı ara"""
    bulgular = {}
    for terim, anlam in MOGOLCA_OYUN_TERIMLERI.items():
        if terim in metin:
            bulgular[terim] = anlam
    return bulgular
```

---

# Natasha/Corus — Rusça Külliyat

## Repo
github.com/natasha/corus
Lisans: MIT | Dil: Python | Aktif: Evet

## Ne Yapar
Rusça külliyat yükleyici.
Sovyet dönemi metinlere programatik erişim.

## Gök Umay İçin Değeri
- Sovyet etnografya makaleleri (1950-1975) için erişim
- "Советская этнография" dergisi oyun makaleleri
- Kazak, Moğol, Özbek oyun araştırmaları Rusça yayınlandı

## Kurulum
```bash
pip install corus
```

## Sovyet Etnografya Erişimi
```python
# Doğrudan dijital erişim için:
# → cyberleninka.ru (Rusça akademik makale — ücretsiz)
# → elibrary.ru (Rusça akademik)

# Arama terimleri:
RUSCA_ARAMA = [
    'казахские игры',        # Kazak oyunları
    'монгольские игры',      # Moğol oyunları
    'народные игры',         # Halk oyunları
    'настольные игры',       # Masa oyunları
    'шашки казахские',       # Kazak dama
    'тоғызқұмалақ',         # Togyzool
    'шатар монгольский',     # Moğol satranç
]

# cyberleninka.ru üzerinden URL oluştur
BASE = "https://cyberleninka.ru/search#q="
for terim in RUSCA_ARAMA:
    url = BASE + terim.replace(' ', '+')
    print(url)
```

## pymorphy2 — Rusça Morfoloji
```bash
pip install pymorphy2

# Kullanım
import pymorphy2
morph = pymorphy2.MorphAnalyzer()

kelime = 'игры'  # "oyunlar" (çoğul)
analiz = morph.parse(kelime)[0]
print(analiz.normal_form)   # 'игра' (tekil)
print(analiz.tag.POS)       # 'NOUN'
```

## Agent Bağlantısı
→ language-bridge Moğolca + Rusça modülleri
→ corpus-builder Sovyet dönemi kaynaklara erişim
