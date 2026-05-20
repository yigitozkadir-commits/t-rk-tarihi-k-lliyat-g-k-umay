# Gök Umay Editör Sistemi — Kurulum

## Bu Sistem Nedir
Tarihi belgeleri okuyan, analiz eden, AR-GE kararlarına dönüştüren
bağımsız Claude agent ekosistemi.

Ana sisteminizden bağımsız çalışır. İstediğinizde entegre edebilirsiniz.

---

## Kurulum

```bash
# Bu klasörü Claude proje dizinine koy
cp -r gokumay-editor/ ~/your-claude-project/

# Claude'u bu dizinde başlat
cd gokumay-editor/
claude
```

---

## Agent Ekibi

| Agent | Dosya | Görev |
|-------|-------|-------|
| document-archaeologist | `.claude/agents/document-archaeologist.md` | Belge okur, etiketler |
| arge-director | `.claude/agents/arge-director.md` | Önceliklendirir, yönlendirir |
| source-validator | `.claude/agents/source-validator.md` | Çelişen kaynakta hakemlik |
| arge-reporter | `.claude/agents/arge-reporter.md` | Rapor ve GDD üretir |

---

## Slash Komutları

| Komut | Ne Yapar |
|-------|---------|
| `/belge-oku [dosya]` | Belgeyi analiz et |
| `/arge-durum` | Tüm aktif oyunların durumu |
| `/gdd-taslak [oyun]` | Oyun tasarım dokümanı üret |
| `/haftalik-ozet` | Bu haftanın AR-GE özeti |
| `/kaynak-dogrula [A] vs [B]` | İki kaynağı karşılaştır |

---

## Temel Kullanım Akışı

```
1. Eline belge geçti
   → /belge-oku ile yükle

2. document-archaeologist okur
   → Keşif Raporu üretir
   → [BELGELENMİŞ/YORUM/EKSİK] etiketler

3. arge-director devreye girer
   → Öncelik belirler (1-9 matris)
   → Seviye günceller (SEV.1-6)
   → Sonraki adımı söyler

4. Şüpheli varsa → /kaynak-dogrula
   Rapor lazımsa → /haftalik-ozet
   Oyun taslağı → /gdd-taslak [oyun adı]
```

---

## Etiket Referansı

```
[BELGELENMİŞ]     → Kaynakta açıkça var
[YORUM]            → Mantıklı çıkarım
[EKSİK]            → Bilgi yok
[ÇELİŞKİ]          → Başka kaynakla çelişiyor
[REKONSTRÜKSIYON]  → Yeniden inşa edilmiş
```

---

## İleride Entegrasyon

Bu sistem kendi başına çalışır. Ana Gök Umay sistemine
entegre etmek istediğinizde şu köprüler kurulur:

- RAG → document-archaeologist bulguları Supabase'e yazan pipeline
- n8n → Haftalık özet otomatik üretimi
- Katman 3 → memory_client ile GDD arşivleme
