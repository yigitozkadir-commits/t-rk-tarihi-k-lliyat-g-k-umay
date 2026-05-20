# Playwright — Tarayıcı Otomasyonu

## Repo
github.com/microsoft/playwright-python
Lisans: Apache 2.0 | Kurum: Microsoft | Yıldız: 12.000+ | Aktif: Evet

## Ne Yapar
Headless Chrome/Firefox/WebKit ile tam tarayıcı otomasyonu.
JavaScript gerektiren, giriş korumalı, dinamik içerikli sitelere erişim.
Scrapy/Requests'in ulaşamadığı yerlerde çalışır.

## Gök Umay İçin Değeri
- JSTOR tam metin: JavaScript ile yüklenen makale sayfaları
- Transkribus web arayüzü otomasyonu
- Süleymaniye, Topkapı dijital arşiv: dinamik arama formları
- Academia.edu: giriş gerektiren makaleler
- Europeana gelişmiş arama: filtre kombinasyonları

## Kurulum
```bash
pip install playwright
playwright install chromium
```

## Arşiv Otomasyon Örnekleri
```python
from playwright.sync_api import sync_playwright

def jstor_makale_indir(doi: str, cikti_yolu: str):
    """JSTOR'dan JavaScript yüklü makale sayfasını çek."""
    with sync_playwright() as p:
        browser = p.chromium.launch(headless=True)
        page = browser.new_page()

        page.goto(f"https://www.jstor.org/stable/{doi}")
        page.wait_for_selector("article.content", timeout=15000)

        metin = page.inner_text("article.content")
        with open(cikti_yolu, 'w') as f:
            f.write(metin)

        browser.close()

def dinamik_arsiv_ara(arsiv_url: str, sorgu: str) -> list:
    """
    JavaScript ile yüklenen arşiv arama sonuçlarını çek.
    Statik HTML döndürmeyen kataloglar için.
    """
    with sync_playwright() as p:
        browser = p.chromium.launch(headless=True)
        page = browser.new_page()

        page.goto(arsiv_url)
        page.fill('input[name="q"]', sorgu)
        page.press('input[name="q"]', 'Enter')
        page.wait_for_selector('.search-result', timeout=10000)

        sonuclar = []
        for el in page.query_selector_all('.search-result'):
            sonuclar.append({
                'baslik': el.query_selector('h2')
                            .inner_text() if el.query_selector('h2') else '',
                'url': el.query_selector('a')
                         .get_attribute('href') if el.query_selector('a') else '',
            })

        browser.close()
        return sonuclar

async def toplu_sayfa_cek(url_listesi: list) -> list:
    """Async Playwright ile paralel sayfa çekme."""
    from playwright.async_api import async_playwright
    import asyncio

    async with async_playwright() as p:
        browser = await p.chromium.launch(headless=True)

        async def tek_sayfa(url):
            page = await browser.new_page()
            await page.goto(url)
            icerik = await page.content()
            await page.close()
            return {'url': url, 'html': icerik}

        sonuclar = await asyncio.gather(*[tek_sayfa(u) for u in url_listesi])
        await browser.close()
        return sonuclar
```

## Agent Bağlantısı
→ archive-navigator giriş korumalı ve JS-ağırlıklı arşivler
→ corpus-builder özel erişim gerektiren kaynaklar
