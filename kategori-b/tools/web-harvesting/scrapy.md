# Scrapy — Web Crawling Çerçevesi

## Repo
github.com/scrapy/scrapy
Lisans: BSD | Dil: Python | Yıldız: 54.000+ | Aktif: Evet (v2.13, 2025)

## Ne Yapar
Üretim kaliteli web tarama ve veri çekme çerçevesi.
Otomatik gecikme, önbellek, pipeline, proxy rotasyonu.
Requests'ten farklı olarak eşzamanlı, yapılandırılmış crawl.

## Gök Umay İçin Değeri
- Çoklu arşiv aynı anda tarama: Bodleian + BL + Gallica paralel
- cyberleninka.ru Rusça makale taraması (Sovyet etnografya)
- jstor.org, academia.edu: erişilebilir oyun tarihi makaleleri
- Büyük OAI-PMH kataloglarını sayfalayarak toplama
- Tüm tarama geçmişini otomatik kaydetme, tekrar çalıştırma

## Kurulum
```bash
pip install scrapy
```

## Arşiv Tarama Spider'ı
```python
import scrapy

class CyberLeninkaSpider(scrapy.Spider):
    """
    Sovyet etnografya makalelerini tarar.
    Kazak/Moğol oyun araştırmaları için.
    """
    name = "cyberleninka_etnografya"
    ARAMA_TERIMLERI = [
        "казахские игры",
        "монгольские игры",
        "настольные игры Средняя Азия",
        "тоғызқұмалақ",
    ]

    def start_requests(self):
        base = "https://cyberleninka.ru/search#q="
        for terim in self.ARAMA_TERIMLERI:
            yield scrapy.Request(
                url=base + terim.replace(" ", "+"),
                callback=self.parse_liste,
                meta={'terim': terim}
            )

    def parse_liste(self, response):
        for makale in response.css('article.search-result'):
            yield {
                'baslik': makale.css('h2 a::text').get('').strip(),
                'url': response.urljoin(makale.css('h2 a::attr(href)').get('')),
                'dergi': makale.css('.journal::text').get('').strip(),
                'yil': makale.css('.year::text').get('').strip(),
                'arama_terimi': response.meta['terim'],
            }

        # Sonraki sayfa
        sonraki = response.css('a.next::attr(href)').get()
        if sonraki:
            yield response.follow(sonraki, self.parse_liste,
                                  meta=response.meta)

class OAIPMHSpider(scrapy.Spider):
    """
    OAI-PMH destekli arşivleri toplu tara.
    metha'nın Scrapy eşdeğeri.
    """
    name = "oai_pmh"
    custom_settings = {
        'DOWNLOAD_DELAY': 2,
        'AUTOTHROTTLE_ENABLED': True,
    }

    def start_requests(self):
        ARSIVLER = [
            "https://digital.bodleian.ox.ac.uk/oai/",
            "https://gallica.bnf.fr/OAI/OAI2.php",
        ]
        for arsiv in ARSIVLER:
            yield scrapy.Request(
                url=arsiv + "?verb=ListRecords&metadataPrefix=oai_dc",
                callback=self.parse_oai
            )

    def parse_oai(self, response):
        # OAI-PMH XML parse
        response.selector.remove_namespaces()
        for record in response.xpath('//record'):
            yield {
                'id': record.xpath('.//identifier/text()').get(),
                'baslik': record.xpath('.//title/text()').get(''),
                'tarih': record.xpath('.//date/text()').get(''),
                'dil': record.xpath('.//language/text()').get(''),
                'kaynak_url': response.url,
            }
```

## Agent Bağlantısı
→ archive-navigator çoklu arşiv toplu tarama
→ corpus-builder akademik kaynak toplama pipeline'ı
