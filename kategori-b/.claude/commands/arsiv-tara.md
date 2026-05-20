# /arsiv-tara

OAI-PMH ile arşivi otomatik tara, oyunla ilgili kayıtları bul.

## Kullanım
```
/arsiv-tara [konu]
/arsiv-tara [konu] --arsiv=[bodleian|bl|gallica|europeana]
/arsiv-tara [konu] --dijital  (sadece online erişimliler)
```

## Adımlar
1. archive-navigator → metha ile OAI-PMH tarama
2. IIIF manifest kontrolü
3. Bulunanları corpus-builder'a ilet

## Çıktı
→ Bulunan kayıt listesi + erişim URL'leri
→ IIIF manifest linkleri
→ Fiziksel ziyaret gerekenlerin listesi
