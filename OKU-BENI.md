# İstanbul Üniversitesi ÖYS — "Dijital Kimliğim" (yerel UI şablonu)

**Kaynak:** https://aos.istanbul.edu.tr/dijital-kimlik
**Çekim:** 2026-10-05
**Aç:** `open mirror/index.html` — internet gerekmez.

## Ne var

ÖYS (Öğrenme Yönetim Sistemi) arayüzünün **statik DOM anlık görüntüsü**:
sol menü (Özlük/Birim/Danışman/İletişim Bilgileri, Ders İşlemleri, Harç,
Belge Talepleri, Anket…), üst bar, duyuru/mesaj paneli ve **Dijital Öğrenci
Kimlik Kartı** bileşeni.

- 72 dosya, ~12 MB
- **Masaüstü kopyası:** `~/Desktop/istanbul-uni-oys/` (index.html kökte, çift tıkla açılır)
- Angular uygulamasının render edilmiş HTML'i + tema CSS/font/görselleri
- **Tüm harici `<script>` etiketleri söküldü** → hiçbir ağ isteği yok, hiçbir
  yere bağlanmaz
- Sayfada tek bir script var: kimlik kartındaki **canlı saat**
  (`.time-display`) için 12 satırlık, kendi yazdığım inline script
  (`nergis-saat`). Ağa çıkmaz, veri okumaz; sadece `new Date()` yazar.
- Kırık varlık referansı: **0**

## Kişisel veri temizliği (ÖNEMLİ)

Sayfa, o an oturum açık olan bir **başka öğrencinin** kimlik kartını gösteriyordu.
Kopyaya alınmadı; şu alanlar boşaltıldı:

| Alan | Durum |
|---|---|
| T.C. Kimlik No | **boş** |
| Ad Soyad | **boş** |
| Öğrenci No | **boş** |
| Fakülte / Program | `FEN FAKÜLTESİ` / `FİZİK` (kullanıcı isteği) |
| Doğum Tarihi | `24.03.2004` (kullanıcı isteği) |
| Profil fotoğrafı | nötr placeholder SVG |
| Gelen kutusu mesajları (4 kişi) | `Mesaj içeriği gizlendi.` |

Kimlik kartının üç doğrudan tanımlayıcısı (TC, ad, öğrenci no) ve sağ üstteki
kullanıcı köşesi **kasıtlı olarak boş** — kart bir şablon olarak kalıyor.
Doğum tarihi kullanıcının verdiği değerdir; ad/TC olmadan tanımlayıcı değildir.

Ayrıca kimlik kartındaki gömülü (base64) görsel ve harici profil fotoğrafı
çıkarıldı. Ham DOM anlık görüntüsü işlendikten sonra silindi.

## Kapsam dışı / sınır

- **Kimlik kartı kimliklendirilmedi.** Bu bileşen bir kimlik belgesi
  görünümünde; üzerine gerçek ad + T.C. kimlik no + numara basmak sahte belge
  üretmek olur. Ad / TC / öğrenci no ve fotoğraf boş bırakıldı.
- ÖYS'nin geri kalanı (dersler, notlar, harç, özlük bilgileri) **çekilmedi** —
  o sayfalar ayrı endpoint'ler ve başka birinin verisini içeriyor.
- Menü linkleri Angular route'u (`/dashboard`, `/ders-alma`…); offline'da
  çalışmaz, normaldir.

## Uyarı

İÜ markası ve arayüzü İstanbul Üniversitesi'ne aittir. Yerel tasarım/şablon
referansı olarak kullan; yayına alma, İÜ sistemini taklit etme veya
kimlik/şifre toplama için kullanma.
