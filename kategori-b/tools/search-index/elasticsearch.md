# Elasticsearch — Tam Metin Arama Motoru

## Repo
github.com/elastic/elasticsearch
Lisans: SSPL/Elastic | Dil: Java (Python istemci mevcut) | Yıldız: 72.000+ | Aktif: Evet

## Ne Yapar
Dağıtık tam metin arama ve analiz motoru.
Büyük metin külliyatlarında milisaniyeler içinde arama.
Arapça, Rusça, Türkçe analizörleri dahil.

## Gök Umay İçin Değeri
- İndirilen tüm kaynak metinlerini (Murray, Bell, Parlett...) indeksle
- Arapça analizör: satranç risalelerinde "شطرنج" ve türevleri
- Rusça analizör: Sovyet etnografyasında oyun terimleri
- Çok dilli eş zamanlı arama: TR+AR+FA+RU tek sorguda
- OpenITI külliyatında hızlı terim arama (binlerce metin)

## Kurulum
```bash
# Docker ile (önerilen)
docker run -d --name es \
  -e discovery.type=single-node \
  -p 9200:9200 \
  elasticsearch:8.13.0

pip install elasticsearch
```

## Külliyat İndeksleme
```python
from elasticsearch import Elasticsearch

es = Elasticsearch("http://localhost:9200")

INDEKS = "gokumay_kulliyat"

# Çok dilli indeks oluştur
es.indices.create(index=INDEKS, body={
    "settings": {
        "analysis": {
            "analyzer": {
                "arabic_analyzer": {
                    "type": "arabic"
                },
                "russian_analyzer": {
                    "type": "russian"
                }
            }
        }
    },
    "mappings": {
        "properties": {
            "baslik":  {"type": "text", "analyzer": "standard"},
            "metin_ar": {"type": "text", "analyzer": "arabic_analyzer"},
            "metin_ru": {"type": "text", "analyzer": "russian_analyzer"},
            "metin_tr": {"type": "text", "analyzer": "turkish"},
            "yazar":   {"type": "keyword"},
            "yil":     {"type": "integer"},
            "oyun_etiketleri": {"type": "keyword"},
            "kaynak_tur": {"type": "keyword"},
        }
    }
})

def kaynak_indeksle(kaynak_id: str, kaynak: dict):
    """Bir kaynağı tüm dil alanlarıyla indeksle."""
    es.index(index=INDEKS, id=kaynak_id, document=kaynak)

def cok_dilli_ara(sorgu: str, oyun_filtresi: str = None) -> list:
    """
    Aynı terimi tüm dil alanlarında aynı anda ara.
    'satranç' → TR+AR+FA+RU alanlarında eş zamanlı.
    """
    filtreler = []
    if oyun_filtresi:
        filtreler.append({"term": {"oyun_etiketleri": oyun_filtresi}})

    return es.search(index=INDEKS, body={
        "query": {
            "bool": {
                "should": [
                    {"match": {"metin_ar": sorgu}},
                    {"match": {"metin_ru": sorgu}},
                    {"match": {"metin_tr": sorgu}},
                    {"match": {"baslik": sorgu}},
                ],
                "filter": filtreler,
                "minimum_should_match": 1,
            }
        },
        "highlight": {
            "fields": {
                "metin_ar": {}, "metin_ru": {}, "metin_tr": {}
            }
        }
    })['hits']['hits']
```

## Agent Bağlantısı
→ corpus-builder külliyat tam metin arama altyapısı
→ research-analyst çok dilli kaynak keşfi
→ archive-navigator indekslenmiş arşiv tarama
