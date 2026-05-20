# metha — OAI-PMH Harvester

## Repo
github.com/miku/metha
Lisans: GPL | Dil: Go | Aktif: Evet

## Ne Yapar
OAI-PMH protokolüyle dijital kütüphanelerden metadata çeker.
Dünya genelinde 1.700+ kütüphane bu protokolü konuşuyor.
Yerleşik önbellekle aynı veriyi iki kez çekmez.

## Gök Umay İçin Değeri
Bodleian, British Library, Europeana, Süleymaniye —
hepsine tek protokolle bağlanıp el yazması katalog verisi çekilebilir.
Fiziksel ziyaret öncesi "bu arşivde ne var?" sorusuna otomatik cevap.

## Kurulum
```bash
# Binary indir (Linux/Mac)
wget https://github.com/miku/metha/releases/latest/download/metha-linux-amd64
chmod +x metha-linux-amd64
mv metha-linux-amd64 /usr/local/bin/metha

# Go ile derle
go install github.com/miku/metha/cmd/metha@latest
```

## Temel Kullanım
```bash
# Bir arşivi tara
metha-sync http://bodleian.ox.ac.uk/oai/provider

# Belirli set (koleksiyon)
metha-sync -set "manuscripts:turkish" http://bodleian.ox.ac.uk/oai/provider

# Tarih aralığı
metha-sync -from 2020-01-01 -until 2025-01-01 http://bodleian.ox.ac.uk/oai/provider

# Çekilen veriyi listele
metha-ls http://bodleian.ox.ac.uk/oai/provider

# XML çıktısını JSON'a çevir
metha-cat http://bodleian.ox.ac.uk/oai/provider | python3 -c "
import sys, json
import xmltodict
for line in sys.stdin:
    data = xmltodict.parse(line)
    print(json.dumps(data, ensure_ascii=False))
"
```

## Gök Umay Hedef Arşivler ve OAI-PMH Uç Noktaları

```python
GOK_UMAY_ARSIVLER = {
    'Bodleian': {
        'oai': 'https://digital.bodleian.ox.ac.uk/oai/',
        'set': 'manuscripts',
        'arama': ['Turkish', 'Persian', 'Arabic', 'Mongolian'],
    },
    'British Library': {
        'oai': 'https://api.bl.uk/metadata/oai',
        'set': 'manuscripts',
        'arama': ['Ottoman', 'Turkish', 'Persian', 'Central Asia'],
    },
    'Europeana': {
        'oai': 'https://oai.europeana.eu/oai/record',
        'set': 'manuscript',
        'arama': ['ottoman', 'turc', 'persisch'],
    },
    'Gallica (BnF)': {
        'oai': 'https://gallica.bnf.fr/OAI/OAI2.php',
        'set': 'manuscripts',
        'arama': ['turc', 'persan', 'ottoman'],
    },
    'Yazmalar.gov.tr': {
        'oai': 'https://www.yazmalar.gov.tr/oai',  # kontrol et
        'set': None,
        'arama': ['satranç', 'nerd', 'oyun'],
    },
}
```

## Oyun İzini Süren Otomatik Tarama
```python
import subprocess
import json
import xmltodict

OYUN_ANAHTAR_KELIMELERI = [
    'chess', 'shatranj', 'satranç', 'game', 'play',
    'board game', 'nard', 'tavla', 'backgammon',
    'mongolian', 'turkic', 'central asia'
]

def arsiv_tara(oai_url: str, arama_terimleri: list) -> list:
    """OAI-PMH ile arşivi tara, oyunla ilgili kayıtları bul"""
    cmd = ['metha-cat', oai_url]
    result = subprocess.run(cmd, capture_output=True, text=True)
    
    bulgular = []
    for satir in result.stdout.splitlines():
        try:
            data = xmltodict.parse(satir)
            metadata = data.get('record', {}).get('metadata', {})
            metadata_str = json.dumps(metadata, ensure_ascii=False).lower()
            
            eslesenler = [k for k in arama_terimleri if k.lower() in metadata_str]
            if eslesenler:
                bulgular.append({
                    'kaynak': oai_url,
                    'metadata': metadata,
                    'eslesen_terimler': eslesenler
                })
        except:
            continue
    
    return bulgular

# Kullanım
bodleian_bulgular = arsiv_tara(
    'https://digital.bodleian.ox.ac.uk/oai/',
    OYUN_ANAHTAR_KELIMELERI
)
```

## Sınırlar
- Bazı arşivler OAI-PMH erişimini kısıtlıyor
- Metadata kalitesi arşivden arşive çok farklı
- Görüntü erişimi için IIIF gerekli (bkz. iiif.md)

## Agent Bağlantısı
→ archive-navigator birincil tarama aracı
→ corpus-builder bulunan kaynakları külliyata ekler
