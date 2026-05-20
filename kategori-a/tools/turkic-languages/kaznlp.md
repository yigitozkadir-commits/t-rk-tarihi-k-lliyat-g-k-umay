# kaznlp — Kazakça NLP Araç Seti

## Repo
github.com/nlacslab/kaznlp
Lisans: CC-SA-BY | Dil: Python | Kurum: Nazarbayev Üniversitesi

## Ne Yapar
Kazakça için özel NLP araç seti.
Morfolojik analiz, tokenizasyon, dil tespiti.

## Gök Umay İçin Değeri
- Togyzool kaynaklarını işlemek için birincil araç
- Kazakça etnografya metinleri (Sovyet + modern)
- Kiril Kazakça metinlerde oyun terimi analizi

## Kurulum
```bash
pip install kaznlp
```

## Temel Kullanım
```python
from kaznlp.morphology import Analyser

analyser = Analyser()

# Togyzool terimi analizi
kelime = "тоғызқұмалақ"
analiz = analyser.analyse(kelime)
print(analiz)
# Kök + morfolojik özellikler

# Dil tespiti
from kaznlp.lid import LID
detector = LID()
metin = "Мен ойын ойнадым"
dil = detector.detect(metin)
print(dil)  # 'kaz'
```

## Oyun Araştırması için Kullanım
```python
KAZAKCA_OYUN_TERIMLERI = {
    'тоғызқұмалақ': 'Togyzool (9 taş)',
    'асық': 'Aşık/kemik oyunu',
    'дойбы': 'Dama benzeri',
    'ойын': 'Oyun (genel)',
    'жүріс': 'Hamle',
    'ұтыс': 'Kazanma',
    'ұтылу': 'Kaybetme'
}

def kazakca_oyun_analiz(metin: str) -> dict:
    from kaznlp.morphology import Analyser
    analyser = Analyser()
    bulgular = {}
    for terim, anlam in KAZAKCA_OYUN_TERIMLERI.items():
        if terim in metin:
            analiz = analyser.analyse(terim)
            bulgular[terim] = {
                'anlam': anlam,
                'morfoloji': analiz
            }
    return bulgular
```

## TurkicNLP ile Fark
```
kaznlp:     Kazakça özel, daha derin morfoloji
TurkicNLP:  24 dil kapsıyor, Kazakça dahil ama daha genel
Öneri:      İkisini birlikte kullan
            TurkicNLP → genel pipeline
            kaznlp → Kazakça özel morfoloji
```

## Agent Bağlantısı
→ language-bridge Kazakça modülü
→ TurkicNLP ile birlikte kullanılır (bkz. turkicnlp.md)
