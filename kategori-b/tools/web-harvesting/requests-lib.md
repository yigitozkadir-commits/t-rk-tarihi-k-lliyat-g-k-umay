# Requests — Python HTTP Kütüphanesi

## Repo
github.com/psf/requests
Lisans: Apache 2.0 | Dil: Python | Yıldız: 52.000+ | Aktif: Evet (v2.32, 2025)

## Ne Yapar
Python'un en çok indirilen HTTP kütüphanesi (aylık 30M+).
Arşiv API'larına, OAI-PMH uç noktalarına, Europeana/OpenAlex'e
güvenli, session-tabanlı HTTP erişimi.

## Gök Umay İçin Değeri
- Europeana, OpenAlex, Semantic Scholar, CrossRef API çağrıları
- IIIF manifest indirme (iiif.md'deki kodun omurgası)
- OAI-PMH ham XML çekimi (metha'nın Python alternatifi)
- archive.org API (internetarchive kütüphanesinin alt katmanı)
- Bodleian, BnF, British Library REST API erişimi

## Kurulum
```bash
pip install requests
```

## Arşiv API Erişimi
```python
import requests

SESSION = requests.Session()
SESSION.headers.update({
    'User-Agent': 'GokUmay-Research/1.0 (mailto:arastirma@gokumay.com)'
})

def europeana_ara(sorgu: str, api_key: str) -> list:
    """Europeana'da oyun tarihi kaynağı ara."""
    url = "https://api.europeana.eu/record/v2/search.json"
    params = {
        'wskey':  api_key,
        'query':  sorgu,
        'qf':     'TYPE:IMAGE',
        'rows':   20,
        'profile':'rich'
    }
    r = SESSION.get(url, params=params, timeout=30)
    r.raise_for_status()
    return r.json().get('items', [])

def openalex_sayfalama(sorgu: str) -> list:
    """
    OpenAlex cursor-based sayfalama ile tüm sonuçları çek.
    Büyük makale taramaları için.
    """
    url = "https://api.openalex.org/works"
    cursor = "*"
    sonuclar = []

    while cursor:
        r = SESSION.get(url, params={
            'search': sorgu,
            'per_page': 200,
            'cursor': cursor,
            'select': 'id,title,publication_year,doi,open_access'
        }, timeout=30)
        data = r.json()
        sonuclar.extend(data.get('results', []))
        cursor = data.get('meta', {}).get('next_cursor')

    return sonuclar

def oai_pmh_cek(endpoint: str, metadata_prefix: str = 'oai_dc') -> list:
    """
    OAI-PMH ListRecords protokolü ile metadata çek.
    metha alternatifi — Python içinden doğrudan kullanım için.
    """
    kayitlar = []
    token = None

    while True:
        params = {
            'verb': 'ListRecords' if not token else 'ListRecords',
            'metadataPrefix': metadata_prefix,
        }
        if token:
            params = {'verb': 'ListRecords', 'resumptionToken': token}

        r = SESSION.get(endpoint, params=params, timeout=60)
        r.raise_for_status()

        # XML parse et
        import xml.etree.ElementTree as ET
        root = ET.fromstring(r.content)
        ns = {'oai': 'http://www.openarchives.org/OAI/2.0/'}

        for record in root.findall('.//oai:record', ns):
            kayitlar.append(ET.tostring(record, encoding='unicode'))

        token_el = root.find('.//oai:resumptionToken', ns)
        token = token_el.text if token_el is not None and token_el.text else None
        if not token:
            break

    return kayitlar
```

## Agent Bağlantısı
→ archive-navigator tüm API çağrılarının temeli
→ corpus-builder veri çekme katmanı
→ research-analyst OpenAlex/Semantic Scholar erişimi
