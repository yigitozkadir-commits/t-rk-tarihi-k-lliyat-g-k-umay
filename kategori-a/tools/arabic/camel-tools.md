# CAMeL Tools — Arapça NLP

## Repo
github.com/CAMeL-Lab/camel_tools
Lisans: MIT | Kurum: NYU Abu Dhabi | Aktif: Evet

## Ne Yapar
Klasik ve modern Arapça için kapsamlı NLP araç seti.
Morfoloji, NER, lehçe tespiti, sentiment analizi.

## Gök Umay İçin Değeri
- Arapça satranç risaleleri (al-Adli, al-Suli, al-Lajlaj)
- Klasik Arapça oyun terminolojisi analizi
- "şatranç", "nerd", "vezir" gibi terimlerin morfolojik kökü

## Kurulum
```bash
pip install camel-tools
camel_data -i all  # veri setlerini indir
```

## Temel Kullanım
```python
from camel_tools.morphology.database import MorphologyDB
from camel_tools.morphology.analyzer import Analyzer
from camel_tools.tokenizers.word import simple_word_tokenize

# Morfoloji veritabanı
db = MorphologyDB.builtin_db()
analyzer = Analyzer(db)

# Arapça satranç terimi analizi
kelimeler = ["الشطرنج", "الوزير", "الفيل", "الحركة"]
for kelime in kelimeler:
    analizler = analyzer.analyze(kelime)
    for analiz in analizler[:1]:  # İlk analiz
        print(f"{kelime}: kök={analiz['root']}, lemma={analiz['lemma']}")

# Tokenizasyon
metin = "لعب الملك الشطرنج في القصر"
tokenlar = simple_word_tokenize(metin)
print(tokenlar)
# ['لعب', 'الملك', 'الشطرنج', 'في', 'القصر']
```

## Oyun Araştırması için Kullanım
```python
ARAPCA_OYUN_TERIMLERI = {
    'الشطرنج':  'al-shatranj (satranç)',
    'النرد':    'al-nard (tavla)',
    'اللعبة':   'al-lu\'ba (oyun)',
    'الوزير':   'al-wazir (vezir)',
    'الفيل':    'al-fil (fil taşı)',
    'الحركة':   'al-haraka (hamle)',
    'الدور':    'al-dawr (sıra/tur)',
    'الفائز':   'al-fa\'iz (kazanan)',
    'القاعدة':  'al-qa\'ida (kural)',
}

def risale_analiz(arapca_metin: str) -> list:
    """
    Arapça satranç risalesinde oyun terimlerini bul.
    al-Adli, al-Suli gibi klasik kaynaklar için.
    """
    from camel_tools.tokenizers.word import simple_word_tokenize
    from camel_tools.morphology.analyzer import Analyzer
    from camel_tools.morphology.database import MorphologyDB

    db = MorphologyDB.builtin_db()
    analyzer = Analyzer(db)
    tokenlar = simple_word_tokenize(arapca_metin)

    bulgular = []
    for token in tokenlar:
        if token in ARAPCA_OYUN_TERIMLERI:
            bulgular.append({
                'terim': token,
                'anlam': ARAPCA_OYUN_TERIMLERI[token],
                'morfoloji': analyzer.analyze(token)[:1]
            })

    return bulgular
```

## Klasik vs Modern Arapça
```
CAMeL Tools klasik Arapça için de çalışır ama:
- Harekesiz metin → belirsizlik artar
- 8-12. yy Arapça → bazı arkaik formlar tanınmayabilir
- Öneri: harekeli (matbu/dijital) kaynaklarda daha iyi sonuç

al-Adli (9. yy) risalesi için:
  → Önce dijital versiyonu OpenITI'den al
  → CAMeL Tools ile analiz et
  → Belirsiz terimler için manuel kontrol
```

## Agent Bağlantısı
→ language-bridge Arapça modülü
→ document-archaeologist Arapça kaynak analizi
→ corpus-builder OpenITI ile birlikte (bkz. arabic/openiti.md)
