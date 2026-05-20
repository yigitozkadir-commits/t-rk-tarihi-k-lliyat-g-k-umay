# HTTPX — Async HTTP İstemcisi

## Repo
github.com/encode/httpx
Lisans: BSD | Dil: Python | Yıldız: 14.000+ | Aktif: Evet

## Ne Yapar
Requests'in async/await destekli modern halefi.
HTTP/1.1 + HTTP/2, eşzamanlı bağlantı havuzu.
Playwright ile birlikte: tarayıcı + async API çağrısı aynı anda.

## Gök Umay İçin Değeri
- Paralel IIIF manifest indirme: 10 arşiv aynı anda
- OpenAlex cursor sayfalama: async ile 10× daha hızlı
- Semantic Scholar + CrossRef + Europeana aynı anda sorgulama
- Büyük archive.org indirmelerinde timeout yönetimi
- HTTP/2 desteği: modern API'larda bant genişliği tasarrufu

## Kurulum
```bash
pip install httpx[http2]
```

## Paralel Arşiv Sorgulama
```python
import httpx
import asyncio

async def cok_arsiv_paralel(sorgu: str, api_key_europeana: str) -> dict:
    """
    Birden fazla akademik API'yı aynı anda sorgula.
    Requests ile sıralı yaparsan: ~5s | httpx async ile: ~0.8s
    """
    async with httpx.AsyncClient(timeout=30) as client:
        gorevler = {
            'openalex': client.get(
                "https://api.openalex.org/works",
                params={'search': sorgu, 'per_page': 5}
            ),
            'semantic_scholar': client.get(
                "https://api.semanticscholar.org/graph/v1/paper/search",
                params={'query': sorgu, 'limit': 5,
                        'fields': 'title,year,citationCount'}
            ),
            'europeana': client.get(
                "https://api.europeana.eu/record/v2/search.json",
                params={'wskey': api_key_europeana, 'query': sorgu, 'rows': 5}
            ),
        }

        yanıtlar = await asyncio.gather(*gorevler.values(),
                                        return_exceptions=True)

        return {
            kaynak: (r.json() if not isinstance(r, Exception) else {'hata': str(r)})
            for kaynak, r in zip(gorevler.keys(), yanıtlar)
        }

async def iiif_toplu_indir(manifest_url_listesi: list, cikti_dir: str):
    """
    Birden fazla IIIF manifestini paralel olarak indir.
    10 arşiv belgesi aynı anda — Requests ile sıralı değil.
    """
    import os, json
    os.makedirs(cikti_dir, exist_ok=True)

    async with httpx.AsyncClient(timeout=60, http2=True) as client:
        async def tek_manifest(url):
            r = await client.get(url)
            return r.json()

        manifestler = await asyncio.gather(
            *[tek_manifest(u) for u in manifest_url_listesi],
            return_exceptions=True
        )

    for i, manifest in enumerate(manifestler):
        if isinstance(manifest, dict):
            with open(f"{cikti_dir}/manifest_{i+1:03d}.json", 'w') as f:
                json.dump(manifest, f, ensure_ascii=False, indent=2)
```

## Agent Bağlantısı
→ archive-navigator paralel arşiv erişimi
→ corpus-builder hızlı toplu veri çekme
