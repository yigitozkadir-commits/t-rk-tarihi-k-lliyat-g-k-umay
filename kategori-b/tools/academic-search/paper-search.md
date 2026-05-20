# Akademik Makale Arama Araçları

## 1. paper-search-mcp

### Repo
github.com/openags/paper-search-mcp
Lisans: MIT | MCP Server | Aktif: Evet (2025)

### Ne Yapar
20+ akademik kaynak tek arayüzden.
MCP server olarak Claude'a doğrudan bağlanır.

### Desteklenen Kaynaklar
```
arXiv, PubMed, Semantic Scholar, CrossRef,
OpenAlex, JSTOR, Zenodo, IEEE, ACM,
bioRxiv, medRxiv, SSRN, EconPapers
```

### MCP Kurulumu
```json
// claude_desktop_config.json
{
  "mcpServers": {
    "paper-search": {
      "command": "uvx",
      "args": ["paper-search-mcp"],
      "env": {
        "SEMANTIC_SCHOLAR_API_KEY": "isteğe_bağlı"
      }
    }
  }
}
```

### Gök Umay Arama Stratejisi
```
Journal of Board Game Studies:
  → "board games" "Central Asia"
  → "shatranj" "medieval"
  → "Mongolian chess" "Hiashatar"
  → "Togyzool" OR "Togyzkumalak"
  → "mancala" "Turkic"

Etnografya:
  → "Kazakh games" "ethnography"
  → "nomadic games" "steppe"
  → "folk games" "Central Asia"

Arkeoloji:
  → "game pieces" "archaeological" "Central Asia"
  → "chess pieces" "medieval" "Islamic"
  → "board game" "excavation" "Silk Road"

Müzik ve Kültür:
  → "Turkic music" "historical"
  → "Central Asian instruments" "ethnomusicology"
  → "Islamic music" "medieval"
```

---

## 2. Academix

### Repo
github.com/xingyulu23/Academix
Lisans: MIT | MCP Server | Aktif: Evet (2025)

### Ne Yapar
OpenAlex, DBLP, Semantic Scholar, arXiv, CrossRef birleşik arama.
BibTeX çıktısı, atıf ağı görselleştirmesi, AI öneri.

### MCP Kurulumu
```json
{
  "mcpServers": {
    "academix": {
      "command": "uvx",
      "args": ["academix-mcp"]
    }
  }
}
```

### Atıf Ağı Analizi
```python
# Academix API ile atıf ağı
# Murray (1913) → kim bu kitaba atıf yapmış?
# → Parlett, Bell, Pritchard...
# → Onlar kime atıf yapmış?
# → Tarihi satranç literatürünün tam haritası

def atif_agi_ciz(baslangic_doi: str, derinlik: int = 2) -> dict:
    """
    Akademix ile atıf ağı analizi.
    Bir kaynaktan başla, atıf zincirini izle.
    """
    # Academix MCP üzerinden Claude'a sor:
    # "Bu DOI'nin atıf ağını derinlik 2 ile çiz"
    pass
```

---

## 3. Doğrudan API Erişimi (Ücretsiz)

### Semantic Scholar API
```python
import requests

def semantic_scholar_ara(sorgu: str, limit: int = 10) -> list:
    """Semantic Scholar'da oyun tarihi makale ara"""
    url = "https://api.semanticscholar.org/graph/v1/paper/search"
    params = {
        'query': sorgu,
        'limit': limit,
        'fields': 'title,authors,year,abstract,openAccessPdf,citationCount'
    }
    response = requests.get(url, params=params)
    return response.json().get('data', [])

# Kullanım
makaleler = semantic_scholar_ara("board games Central Asia history")
for makale in makaleler:
    pdf = makale.get('openAccessPdf', {})
    print(f"{makale['title']} ({makale['year']})")
    print(f"  Atıf: {makale['citationCount']}")
    if pdf:
        print(f"  PDF: {pdf.get('url', 'Yok')}")
```

### OpenAlex API (Tamamen Ücretsiz)
```python
def openalex_ara(sorgu: str, dergi: str = None) -> list:
    """
    OpenAlex'te makale ara.
    Journal of Board Game Studies için:
    """
    url = "https://api.openalex.org/works"
    params = {
        'search': sorgu,
        'filter': f'primary_location.source.display_name:{dergi}' if dergi else None,
        'per_page': 20,
        'select': 'title,authorships,publication_year,open_access,doi'
    }
    params = {k: v for k, v in params.items() if v}
    
    response = requests.get(url, params=params,
                           headers={'mailto': 'arastirma@gokumay.com'})
    return response.json().get('results', [])

# Journal of Board Game Studies'ın tüm makaleleri
jbgs = openalex_ara(
    "board games history",
    dergi="Journal of Board Game Studies"
)
```

### CrossRef API (DOI Çözümleme)
```python
def doi_coz(doi: str) -> dict:
    """DOI'den tam atıf bilgisi al"""
    url = f"https://api.crossref.org/works/{doi}"
    response = requests.get(url)
    data = response.json()['message']
    
    return {
        'baslik': data['title'][0] if data.get('title') else '',
        'yazarlar': [f"{a.get('given','')} {a.get('family','')}"
                    for a in data.get('author', [])],
        'yil': data.get('published', {}).get('date-parts', [['']])[0][0],
        'dergi': data.get('container-title', [''])[0],
        'doi': doi,
    }
```

## Agent Bağlantısı
→ research-analyst makale tarama için
→ citation-manager bulunan makaleleri Zotero'ya ekler
→ corpus-builder akademik kaynak tabanı
