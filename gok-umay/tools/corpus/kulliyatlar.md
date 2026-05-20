# Metin Külliyatları — Erişim Rehberi

## 1. OpenITI — Arapça/Farsça/Osmanlıca

```
github.com/OpenITI/RELEASE
openiti.org (arama motoru)

Oyunla ilgili arama terimleri:
  AR: شطرنج، النرد، اللعب
  FA: شطرنج، بازی، نرد
  OTA: شطرنج، نرد، اویون

Hemen erişilebilir metinler:
  → İbn Battuta (seyahatname, oyun referansları)
  → Biruni (Orta Asya oyunları)
  → Şahname (oyun sahneli minyatür metinleri)
```

## 2. Ottoman-NLP / OTC — Osmanlıca

```
github.com/Ottoman-NLP/ottominer-public
HuggingFace: Ottoman-NLP organizasyonu

Kapsam: 15-20. yy, Tanzimat dönemi odaklı
Format: Translitere edilmiş Latin alfabesi
Kullanım: Osmanlıca metinde oyun terimi arama
```

## 3. Historical Uyghur-Chinese Corpus

```
github.com/HKBUproject/historical-uyghur-chinese-corpus

Kapsam:
  → 1940'lar Uygurca-Çince paralel belgeler
  → Çing dönemi Uygurca
  → PRC dönemi yasal belgeler

Gök Umay İçin:
  → Uygurca oyun terminolojisi için referans
  → Karluk kolu dil araştırması
  → TurkicNLP Uygurca modülü ile birlikte kullan
```

## 4. Açık Dijital Kütüphaneler (Programatik Erişim)

```python
import requests

def archive_org_ara(sorgu: str, dil: str = 'tur') -> list:
    """
    archive.org'da oyun tarihi kitabı ara.
    Murray, Bell, Parlett gibi telif hakkı süresi dolmuş kitaplar.
    """
    url = "https://archive.org/advancedsearch.php"
    params = {
        'q': f'{sorgu} language:{dil}',
        'fl': 'identifier,title,creator,date',
        'output': 'json',
        'rows': 20
    }
    response = requests.get(url, params=params)
    return response.json()['response']['docs']

# Kullanım
kitaplar = archive_org_ara('board games history', 'eng')
for kitap in kitaplar:
    print(f"{kitap['title']} ({kitap.get('date', '?')})")
    print(f"  → archive.org/details/{kitap['identifier']}")

# Murray'ı doğrudan indir
murray_url = "https://archive.org/download/historyofchess00murruoft/historyofchess00murruoft.pdf"
```

## 5. JSTOR Erişimi

```python
# JSTOR API (akademik kurum üyeliği ile)
# Alternatif: JSTOR Data for Research (ücretsiz)

JSTOR_ARAMA = [
    '"board games" "Central Asia"',
    '"shatranj" "medieval"',
    '"Mongol" "games" "ethnography"',
    '"Togyzool" OR "Togyzkumalak"',
    '"Hiashatar" OR "Mongolian chess"',
    '"Turkic games" "history"',
]

# jstor.org/action/doBasicSearch?Query= ile başla
# Ücretsiz PDF erişimi için: sci-hub (araştırmacı sorumluluğu)
# Açık erişim: unpaywall.org eklentisi
```

## 6. Gallica (BnF) — Fransız Ulusal Kütüphanesi

```python
def gallica_ara(sorgu: str) -> list:
    """
    Gallica'da Osmanlı/Orta Asya el yazması ara.
    """
    url = "https://gallica.bnf.fr/SRU"
    params = {
        'operation': 'searchRetrieve',
        'version': '1.2',
        'query': f'dc.title all "{sorgu}"',
        'maximumRecords': 10
    }
    response = requests.get(url, params=params)
    return response.text  # XML parse gerekli

# Örnek: Osmanlı el yazması ara
gallica_ara("manuscrit turc jeu échecs")
# → Osmanlıca satranç el yazmaları
```

## 7. Yazmalar.gov.tr — Türkiye El Yazmaları

```python
# API yok ama katalog açık
# URL: https://www.yazmalar.gov.tr/

ARAMA_TERIMLERI = [
    'satranç', 'şatranç', 'nerd', 'oyun',
    'la\'b', 'eğlence', 'seyir'
]

# Manuel arama URL şablonu:
# https://www.yazmalar.gov.tr/eser/arama?q={terim}

def yazma_url_olustur(terim: str) -> str:
    return f"https://www.yazmalar.gov.tr/eser/arama?q={terim}"

for terim in ARAMA_TERIMLERI:
    print(yazma_url_olustur(terim))
```

## Agent Bağlantısı
→ corpus-builder tüm bu külliyatları yönetir
→ archive-navigator fiziksel + dijital erişim rehberi
→ document-archaeologist dijital metni analiz eder
