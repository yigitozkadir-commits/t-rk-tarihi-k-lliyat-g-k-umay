# pyzotero — Zotero Python İstemcisi + MCP Server

## Repo
github.com/urschrei/pyzotero
Lisans: MIT | Dil: Python | Aktif: Evet (MCP dahil)

## Ne Yapar
Zotero kütüphanesine programatik erişim.
Kaynak ekle, düzenle, BibTeX/Chicago/APA çıktısı al.
MCP server ile Claude'a doğrudan bağlanır.

## Gök Umay İçin Değeri
- Okunan her kaynağı otomatik Zotero'ya ekle
- Chicago/APA atıf formatı otomatik üret
- Claude Desktop'tan direkt Zotero kütüphanene eriş
- Semantic Scholar entegrasyonu — makale ara, otomatik ekle

## Kurulum
```bash
pip install pyzotero

# Zotero API anahtarı al:
# zotero.org → Settings → Feeds/API → Create new private key
# Library ID: zotero.org profilindeki kullanıcı ID
```

## MCP Server Kurulumu (Claude Desktop)
```json
// claude_desktop_config.json
{
  "mcpServers": {
    "zotero": {
      "command": "uvx",
      "args": ["zotero-mcp"],
      "env": {
        "ZOTERO_API_KEY": "senin_api_anahtarin",
        "ZOTERO_LIBRARY_ID": "senin_library_id",
        "ZOTERO_LIBRARY_TYPE": "user"
      }
    }
  }
}
```

## Temel Kullanım
```python
from pyzotero import zotero

# Bağlan
zot = zotero.Zotero(
    library_id='LIBRARY_ID',
    library_type='user',
    api_key='API_KEY'
)

# Tüm kaynakları listele
kaynaklar = zot.top(limit=50)
for kaynak in kaynaklar:
    print(kaynak['data']['title'])

# Yeni kaynak ekle
yeni_kaynak = zot.item_template('book')
yeni_kaynak['title'] = 'A History of Chess'
yeni_kaynak['creator'] = [{'creatorType': 'author',
                            'firstName': 'H.J.R.',
                            'lastName': 'Murray'}]
yeni_kaynak['date'] = '1913'
yeni_kaynak['publisher'] = 'Clarendon Press'
yeni_kaynak['place'] = 'Oxford'
zot.create_items([yeni_kaynak])

# BibTeX çıktısı al
bibtex = zot.collection_items('COLLECTION_ID', format='bibtex')

# Chicago atıf formatı
chicago = zot.collection_items('COLLECTION_ID', format='chicago-note-bibliography')
```

## Gök Umay Koleksiyon Yapısı
```python
def gok_umay_koleksiyon_kur(zot):
    """Gök Umay AR-GE için Zotero koleksiyon yapısı"""
    koleksiyonlar = [
        'Temel Referanslar',
        'Satranç Tarihi',
        'Orta Asya Oyunları',
        'Osmanlı Kaynakları',
        'Sovyet Etnografyası',
        'Akademik Makaleler',
        'El Yazmaları',
        'Arkeoloji Buluntuları',
    ]
    
    for ad in koleksiyonlar:
        zot.create_collections([{'name': ad}])
        print(f"Koleksiyon oluşturuldu: {ad}")

# Belge analizi sonrası otomatik kayıt
def belge_analiz_sonrasi_kaydet(
    zot, kaynak_bilgi: dict, koleksiyon: str
):
    """
    document-archaeologist analizi sonrası
    otomatik Zotero'ya kaydet
    """
    sablon = zot.item_template(kaynak_bilgi['tur'])  # 'book', 'journalArticle', vb.
    sablon.update(kaynak_bilgi)
    
    result = zot.create_items([sablon])
    item_key = result['successful']['0']['key']
    
    # Koleksiyona ekle
    zot.addto_collection(koleksiyon, {'key': item_key})
    
    return item_key
```

## Chicago Atıf Otomasyonu
```python
def chicago_atif_uret(zot, item_key: str) -> str:
    """Tek kaynak için Chicago atıf formatı"""
    item = zot.item(item_key, format='chicago-note-bibliography')
    return item

def bibliyografya_olustur(zot, koleksiyon_id: str) -> str:
    """Tüm koleksiyon için bibliyografya"""
    items = zot.collection_items(
        koleksiyon_id,
        format='chicago-note-bibliography'
    )
    return '\n\n'.join(items)
```

## Agent Bağlantısı
→ citation-manager'ın birincil aracı
→ academic-collaboration atıf listesi için
→ arge-reporter GDD atıf bölümü için
