# /kaynak-listesi

Belirtilen oyun için okuma öncelik listesi çıkar.

## Kullanım
```
/kaynak-listesi [oyun adı]
/kaynak-listesi [oyun adı] --durum  (okundu/bekliyor/eksik)
/kaynak-listesi --tum  (tüm külliyat durumu)
```

## Adımlar
1. corpus-builder'ı çağır
2. O oyunla ilgili tüm bilinen kaynakları derle
3. Öncelik sırası çıkar
4. Erişim bilgisi ekle (dijital/fiziksel)

## Çıktı
→ Öncelikli okuma listesi
→ Dijital erişim linkleri
→ Boşluk haritası
→ Tahmini okuma süresi
