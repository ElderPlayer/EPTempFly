# EPTempFly - Gelişmiş Süreli Uçuş Eklentisi (Sürüm 1.2.1)

EPTempFly, oyuncularınıza belirli bir süreliğine uçma yeteneği (TempFly) vermenizi sağlayan, tamamen modern, kapsamlı ve performanslı bir eklentidir. Oyuncularınızın uçuş deneyimini güvenli ve adil hale getirmek için tasarlanmıştır.

## 🌟 Neden EPTempFly?

* **Zaman İsrafı Yok:** Oyuncular yere indiklerinde veya yürüdüklerinde uçuş süresi otomatik olarak duraklatılır (`general.pause-time-when-on-ground`). Sadece gerçekten havadayken süre eksilir.
* **Düşme Koruması:** Oyuncunun uçuş süresi havadayken biterse, eklenti oyuncunun yere güvenli bir şekilde inmesini sağlar ve düşme hasarını engeller.
* **Görsel Şölen (Partiküller):** Oyuncular, havada uçarken arkalarında bırakabilecekleri 68 farklı partikül efektinden (1.2.1 ile 33 yeni efekt eklendi) birini seçebilirler. Maliyeti "0" olan partiküller oyunculara ücretsiz sunulur.
* **Süre Paylaşımı:** Oyuncular kendi uçuş sürelerini istedikleri zaman başka oyunculara hediye edebilir veya aktarabilirler. Bu işlem proxy (BungeeCord/Velocity) üzerinden farklı sunuculardaki oyunculara da yapılabilir.
* **Giriş Ödülleri:** Sunucunuza ilk defa veya her katıldıklarında oyunculara otomatik uçuş süresi hediye edebilirsiniz.

## 🔗 Tam Desteklenen Eklentiler (Hooks)

EPTempFly, sunucunuzdaki diğer sistemleri bozmamak için birçok eklenti ile entegre çalışır. Bu eklentiler zorunlu değildir, yüklü iseler otomatik olarak algılanır.

**Arazi, Bölge ve Ada Eklentileri:**
Aşağıdaki eklentilerde sadece izin verilen bölgelerde uçuşa müsaade edilir:
* **uxmClaims:** Bölge rolleri ile tam uyum.
* **SuperiorSkyblock2:** Ada uçuş yetkisi kontrolü.
* **WorldGuard:** Belirli bölgelerde uçuşu açıp kapatmak için `FLY` bayrağı (flag) desteği.
* **FactionsUUID:** Klan arazilerinde uçuş. (İsteğe bağlı olarak vahşi doğa ve savaş alanlarında kapatılabilir).
* **Diğer Desteklenenler:** GriefPrevention, Residence, PlotSquared, Lands, HuskClaims, ExcellentClaims, BentoBox, IridiumSkyblock, FabledSkyblock, Towny, GriefDefender.

**Savaş ve PvP Eklentileri:**
* **PvPManager:** (v4 API Uyumlu) Oyuncu savaşa girdiğinde uçuş anında kapatılır.
* **CombatLogX:** Çatışma sırasında uçuş engellenir.

**Ekonomi Eklentileri (Market İçin):**
* **Vault:** Oyun içi para ile süre satışı.
* **PlayerPoints:** Kredi/Puan ile süre satışı.

## 📊 PlaceholderAPI Değişkenleri

Menülerinizde veya bilgi tablolarınızda (scoreboard) kullanabileceğiniz değişkenler:
* `%eptempfly_time%` / `%eptempfly_remaining%` - Kalan uçuş süresini gösterir (Örn: 1h 30m).
* `%eptempfly_time_seconds%` - Kalan süreyi saniye cinsinden verir.
* `%eptempfly_flying%` - Oyuncunun o an uçup uçmadığını gösterir (True/False).
* `%eptempfly_locked%` - Oyuncunun uçuşunun kilitli olup olmadığını gösterir.
* `%eptempfly_unlimited%` - Sınırsız uçuş yetkisi olup olmadığını gösterir.
* `%eptempfly_inclaim%` - Oyuncunun uçuş izni olan bir bölgede olup olmadığını gösterir.
* `%eptempfly_canfly%` - Oyuncunun genel olarak uçabilme durumunu gösterir.

## 💾 Veritabanı ve Sunucu Ağı (Proxy)

* **Bağımsız Sunucular:** Varsayılan olarak ekstra kurulum gerektirmeyen hızlı SQLite (`data.db`) kullanır. İstenirse MySQL bağlanabilir.
* **BungeeCord / Velocity Desteği:** Eğer birden fazla sunucudan oluşan bir ağınız varsa, `EPTempFly.jar` dosyasını proxy sunucunuzun `plugins` klasörüne atmanız yeterlidir. Ortak MySQL tablosu kurmanıza gerek kalmadan `eptempfly:sync` kanalı üzerinden oyuncuların süreleri tüm sunucularda otomatik senkronize olur.

## 🌍 Dil Desteği

Eklenti 8 farklı dili destekler ve `config.yml` üzerinden tek tıkla değiştirilebilir:
* Türkçe (`tr_TR`), İngilizce (`en_EN`), Almanca (`de_DE`), Rusça (`ru_RU`), Arapça (`ar_SA`), Çince (`zh_CN`), Portekizce (`pt_BR`), Arnavutça (`sq_AL`).

## 💻 Geliştirici API'si (Developer API)

Kendi eklentilerinizi entegre etmek için kolay API desteği:
```java
EPTempFly api = Bukkit.getServicesManager().load(EPTempFly.class);
// Veya alternatif olarak: EPTempFlyAPI.get()
```
* **Etkinlikler (Events):** `TempFlyToggleEvent` (Uçuş açıldığında veya kapandığında tetiklenir).

## ⌨️ Komutlar ve Yetkiler

**Temel Komutlar:**
* `/tempfly` - Uçuşu açar/kapatır (`eptempfly.use`).
* `/tempfly time [oyuncu]` - Kalan süreyi kontrol eder.
* `/tempfly give <oyuncu> <süre>` - Başkasına kendi süresinden gönderir (`eptempfly.give`).
* `/tempfly shop` - Süre marketini açar (`eptempfly.shop`).
* `/tempfly particles` - Efekt menüsünü açar (`eptempfly.particle`).

**Yönetici Komutları (`eptempfly.admin`):**
* `/tempfly add <oyuncu> <süre>` - Süre ekler.
* `/tempfly set <oyuncu> <süre>` - Süreyi değiştirir.
* `/tempfly remove <oyuncu> <süre>` - Süre siler.
* `/tempfly lock <oyuncu> [true/false]` - Uçuşu kilitler.
* `/tempfly reload` - Config ve dil dosyalarını yeniler.

**Ekstra Yetkiler:**
* `eptempfly.unlimited` - Sınırsız uçuş hakkı.
* `eptempfly.bypass.combat` - Çatışma kısıtlamalarını görmezden gelir.
* `eptempfly.bypass.world` - Kapatılmış dünyalarda uçabilme izni.
