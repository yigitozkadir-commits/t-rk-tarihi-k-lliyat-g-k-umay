# IIIF — Uluslararası Görüntü Birlikte Çalışabilirlik Çerçevesi

## Repo
github.com/IIIF/awesome-iiif
Lisans: CC0 | Aktif: Evet (uluslararası standart)

## Ne Yapar
Dünya müze ve kütüphanelerinin el yazması görüntülerine
standart API ile yüksek çözünürlüklü erişim sağlar.
Bodleian, British Library, Gallica, Europeana, Vatican — hepsi IIIF uyumlu.

## Gök Umay İçin Değeri
- El yazması sayfalarını yüksek çözünürlükte çek
- Birden fazla arşivdeki sayfaları tek koleksiyonda birleştir
- Kraken/eScriptorium ile doğrudan entegre — görüntü → HTR

## IIIF Nasıl Çalışır
```
Arşiv IIIF Manifest URL'si yayınlar
    ↓
Manifest = Belgenin tüm sayfaları + metadata (JSON-LD)
    ↓
Her sayfa için Image API URL'si
    ↓
İstenen çözünürlükte görüntü indir
```

## Python ile Kullanım
```python
import requests
from PIL import Image
from io import BytesIO

def iiif_manifest_oku(manifest_url: str) -> dict:
    """IIIF manifest'i oku, sayfa listesini al"""
    response = requests.get(manifest_url)
    manifest = response.json()
    
    sayfalar = []
    for canvas in manifest.get('sequences', [{}])[0].get('canvases', []):
        for image in canvas.get('images', []):
            sayfalar.append({
                'label': canvas.get('label', ''),
                'image_url': image['resource']['@id'],
                'width': canvas.get('width'),
                'height': canvas.get('height'),
            })
    
    return {
        'baslik': manifest.get('label', ''),
        'toplam_sayfa': len(sayfalar),
        'sayfalar': sayfalar
    }

def iiif_goruntu_indir(image_url: str, cozunurluk: int = 1000) -> Image:
    """IIIF görüntüsünü belirtilen çözünürlükte indir"""
    # IIIF Image API formatı: /full/!{w},{h}/0/default.jpg
    base = image_url.rsplit('/', 4)[0]
    url = f"{base}/full/!{cozunurluk},{cozunurluk}/0/default.jpg"
    
    response = requests.get(url)
    return Image.open(BytesIO(response.content))

def belge_indir(manifest_url: str, cikti_dizini: str):
    """Tüm el yazması sayfalarını indir"""
    import os
    os.makedirs(cikti_dizini, exist_ok=True)
    
    manifest = iiif_manifest_oku(manifest_url)
    print(f"İndiriliyor: {manifest['baslik']} ({manifest['toplam_sayfa']} sayfa)")
    
    for i, sayfa in enumerate(manifest['sayfalar']):
        goruntu = iiif_goruntu_indir(sayfa['image_url'])
        goruntu.save(f"{cikti_dizini}/sayfa_{i+1:03d}.jpg")
        print(f"  Sayfa {i+1}/{manifest['toplam_sayfa']}")
```

## Gök Umay Hedef Arşivler — IIIF Manifest Örnekleri

```python
IIIF_ARSIVLER = {
    'Bodleian': {
        'arama': 'https://digital.bodleian.ox.ac.uk/search/?q={sorgu}&type=item',
        'manifest_ornek': 'https://iiif.bodleian.ox.ac.uk/iiif/manifest/{id}.json',
        'oyun_arama': 'chess OR shatranj OR Ottoman game',
    },
    'British Library': {
        'arama': 'https://digitisedmanuscripts.bl.uk/search?q={sorgu}',
        'manifest_ornek': 'https://api.bl.uk/metadata/iiif/{id}/manifest.json',
        'oyun_arama': 'chess shatranj Ottoman Turkish',
    },
    'Gallica': {
        'arama': 'https://gallica.bnf.fr/services/engine/search/sru?q={sorgu}',
        'manifest_ornek': 'https://gallica.bnf.fr/iiif/{ark}/manifest.json',
        'oyun_arama': 'échecs turc persan jeu',
    },
    'Vatican': {
        'arama': 'https://digi.vatlib.it/search?q={sorgu}',
        'manifest_ornek': 'https://digi.vatlib.it/iiif/{id}/manifest.json',
        'oyun_arama': 'turc échecs chess oriental',
    },
    'Europeana': {
        'api': 'https://api.europeana.eu/record/v2/search.json',
        'manifest': 'IIIF uyumlu — manifest URL metadata içinde',
        'oyun_arama': 'chess board game Ottoman manuscript',
    },
}

def arsiv_ara(arsiv_adi: str, sorgu: str) -> list:
    """Belirtilen IIIF arşivinde oyun metni ara"""
    arsiv = IIIF_ARSIVLER[arsiv_adi]
    url = arsiv['arama'].format(sorgu=sorgu.replace(' ', '+'))
    
    response = requests.get(url)
    # Her arşivin yanıt formatı farklı — parse et
    return response.json()
```

## Kraken/eScriptorium ile Entegrasyon
```python
def iiif_htR_pipeline(manifest_url: str, dil: str = 'arabic'):
    """
    IIIF → görüntü indir → Kraken ile oku
    Tam otomatik el yazması → metin pipeline'ı
    """
    from kraken import blla, rpred
    from kraken.lib import models
    
    manifest = iiif_manifest_oku(manifest_url)
    model = models.load_any(f'models/{dil}_model.mlmodel')
    
    tam_metin = []
    for sayfa in manifest['sayfalar']:
        goruntu = iiif_goruntu_indir(sayfa['image_url'], cozunurluk=2000)
        seg = blla.segment(goruntu)
        satirlar = list(rpred.rpred(model, goruntu, seg))
        tam_metin.append('\n'.join(s.prediction for s in satirlar))
    
    return '\n\n--- YENİ SAYFA ---\n\n'.join(tam_metin)
```

## Agent Bağlantısı
→ archive-navigator görüntü erişimi için
→ manuscript-reader IIIF → Kraken pipeline'ı
→ corpus-builder görüntü arşivleme
