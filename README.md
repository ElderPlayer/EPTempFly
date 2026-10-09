# EPTempFly - Gelişmiş Süreli Uçuş Eklentisi

EPTempFly, oyuncularınıza belirli bir süreliğine uçma yeteneği (TempFly) vermenizi sağlayan, tamamen modern ve kapsamlı bir eklentidir. Oyuncularınızın uçuş deneyimini güvenli ve eğlenceli hale getirmek için tasarlandı.

## 🌟 Neden EPTempFly?

Bu eklenti sadece süre vermekle kalmaz, oyuncu deneyimini bozan sorunları çözer:

* **Zaman İsrafı Yok:** Oyuncular yere indiklerinde veya yürüdüklerinde uçuş süresi otomatik olarak duraklatılır. Sadece gerçekten havadayken süre eksilir.
* **Düşme Koruması:** Oyuncunun uçuş süresi havadayken biterse, eklenti oyuncunun yere güvenli bir şekilde inmesini sağlar ve düşme hasarını engeller.
* **Görsel Şölen (Partiküller):** Oyuncular, uçarken arkalarında bırakabilecekleri 33'ten fazla farklı partikül efektinden (örn. ateş, su damlası, duman) birini seçebilirler.
* **Oyuncular Arası Paylaşım:** Oyuncular kendi uçuş sürelerini istedikleri zaman başka oyunculara hediye edebilir veya aktarabilirler.

## ⚙️ Temel Sistemler ve Entegrasyonlar

* **Oyun İçi Market:** Vault veya PlayerPoints kullanarak, oyuncularınıza doğrudan menü (GUI) üzerinden uçuş süresi satabilirsiniz.
* **Bölge ve Ada Koruması:** SuperiorSkyblock2, uxmClaims, WorldGuard, GriefPrevention, FactionsUUID gibi birçok popüler arazi ve ada eklentisiyle tam uyumlu çalışır. Yetkisiz bölgelerde uçuşu engeller.
* **Çatışma (PvP) Kontrolü:** PvPManager veya CombatLogX ile entegre çalışır. Oyuncu savaşa girdiğinde uçuş otomatik olarak kapatılır.
* **Giriş Ödülleri:** Sunucunuza ilk defa veya her katıldıklarında oyunculara otomatik uçuş süresi hediye edebilirsiniz.
* **Ağ (Proxy) Desteği:** BungeeCord ve Velocity desteği sayesinde oyuncuların uçuş süresi tüm sunucularınız arasında sorunsuzca senkronize olur. Sadece .jar dosyasını Proxy sunucunuza da yüklemeniz yeterlidir.

## 💾 Veri Depolama ve Dil Desteği

* **Veritabanı:** Varsayılan olarak kurulum gerektirmeyen SQLite kullanır (`data.db`). Büyük sunucular için gelişmiş MySQL desteği de mevcuttur.
* **Çoklu Dil Desteği:** Türkçe (`tr_TR`), İngilizce (`en_EN`), Almanca, Rusça, Arapça gibi 8 farklı dili destekler. Tüm mesajlar özelleştirilebilir.

## 📊 PlaceholderAPI Desteklenen Değişkenler

Menülerinizde veya bilgi tablolarınızda (scoreboard) uçuş verilerini göstermek için PlaceholderAPI kullanabilirsiniz:
* `%eptempfly_time%` - Kalan uçuş süresini gösterir.
* `%eptempfly_flying%` - Oyuncunun o an uçup uçmadığını gösterir (True/False).
* `%eptempfly_locked%` - Oyuncunun uçuşunun kilitli olup olmadığını gösterir.
* `%eptempfly_unlimited%` - Sınırsız uçuş yetkisi olup olmadığını gösterir.

## 🔧 Sistem Gereksinimleri

* **Java:** Java 21 veya daha üstü bir sürüm gereklidir.
* **Sunucu Sürümü:** 1.17 ve sonrasındaki tüm Spigot, Paper ve Purpur sürümlerini destekler.
* Hiçbir zorunlu ek eklenti gerektirmez, tamamen bağımsız çalışabilir.

## ⌨️ Komutlar

**Oyuncu Komutları:**
* `/tempfly` - Uçuşu açar veya kapatır.
* `/tempfly time [oyuncu]` - Kendi kalan sürenizi veya başkasının süresini kontrol eder.
* `/tempfly shop` - Süre satın alma marketini açar.
* `/tempfly particles` - Uçuş efekti (partikül) seçme ekranını açar.
* `/tempfly give <oyuncu> <süre>` - Başkasına süre gönderir (Örn: 10m, 1h).

**Yönetici Komutları:**
* `/tempfly add <oyuncu> <süre>` - Oyuncuya belirtilen miktarda süre ekler.
* `/tempfly set <oyuncu> <süre>` - Oyuncunun süresini net olarak belirler.
* `/tempfly remove <oyuncu> <süre>` - Oyuncudan süre siler.
* `/tempfly lock <oyuncu> [true/false]` - Bir oyuncunun uçmasını zorla kilitler.
* `/tempfly reload` - Config ve dil dosyalarını yeniler.

## 🛡️ Yetkiler (Permissions)

* `eptempfly.use` - Temel uçuş komutunu kullanma yetkisi (Varsayılan: Açık).
* `eptempfly.give` - Başkasına süre gönderme yetkisi (Varsayılan: Açık).
* `eptempfly.shop` - Marketi açma yetkisi (Varsayılan: Açık).
* `eptempfly.particle` - Efekt menüsünü açma yetkisi (Varsayılan: Açık).
* `eptempfly.unlimited` - Sınırsız uçuş hakkı verir.
* `eptempfly.admin` - Admin komutlarını kullanma yetkisi (Varsayılan: Sadece OP).
* `eptempfly.bypass.combat` - Çatışma (PvP) sırasında uçuşun kapanmasını engeller.
