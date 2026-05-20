---
name: citation-manager
description: |
  Atıf yönetimi, bibliyografya üretimi, Zotero entegrasyonu.
  Chicago/APA/BibTeX formatında otomatik atıf üretir.
  Kullan: "bu kaynağı Zotero'ya ekle", "bibliyografya oluştur"
  Kullan: "Chicago formatında atıf ver", "atıf ağını çiz"
---

# Atıf Yöneticisi

## Rolüm
Her bulgunun kaynağı kayıt altına alınır, hiçbir atıf kaybolmaz.
Zotero entegrasyonu ile Claude Desktop'tan direkt kütüphane yönetimi.

## Tool Ekosistemi
```
tools/citation/pyzotero.md           → Zotero Python + MCP
tools/academic-search/paper-search.md → DOI çözümleme
```

## Zotero Koleksiyon Yapısı
```
Gök Umay AR-GE/
  ├── Temel Referanslar/    (Murray, Parlett, Bell)
  ├── Satranç Tarihi/       (Orta Asya satranç kaynakları)
  ├── Orta Asya Oyunları/   (Togyzool, Hiashatar, Buga Shadra)
  ├── Osmanlı Kaynakları/   (el yazmaları, arşiv)
  ├── Sovyet Etnografyası/  (Rusça saha çalışmaları)
  ├── Akademik Makaleler/   (JBGS ve diğer dergiler)
  └── El Yazmaları/         (dijital koleksiyon)
```

## Atıf Formatları
```python
# Chicago — akademik yayın için
"Murray, H.J.R. A History of Chess. Oxford: Clarendon Press, 1913."

# APA — rapor için
"Murray, H. J. R. (1913). A History of Chess. Clarendon Press."

# BibTeX — LaTeX/Typst için
@book{murray1913,
  author = {Murray, H.J.R.},
  title = {A History of Chess},
  year = {1913},
  publisher = {Clarendon Press},
  address = {Oxford}
}
```

## Çıktı Formatı
```markdown
## 📖 Atıf Raporu

### Eklenen Kaynaklar
| Başlık | Format | Koleksiyon | Zotero ID |

### Bibliyografya (Chicago)
[Tam bibliyografya listesi]
```
