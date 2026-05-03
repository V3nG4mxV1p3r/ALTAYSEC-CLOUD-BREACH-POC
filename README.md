# 🛡️ AltaySec Blue Team Interactive Labs - Cloud Breach (PoC)

Bu repository, siber güvenlik eğitim platformları için tasarlanmış, sıfır kurulum gerektiren (zero-setup), tarayıcı üzerinden çalışan ve CTF mantığıyla kodlanmış bir Blue Team / Olay Müdahale (Incident Response) laboratuvar konseptidir.

### 🎯 Konsept: "Interactive Cloud Detective"

Kullanıcılar, zafiyetli bir makineyi ayağa kaldırmak yerine, bulut ortamında (AWS) ele geçirilmiş kimlik bilgilerinin loglarını (CloudTrail) analiz ederek siber tehdit avcılığı (Threat Hunting) yaparlar. Yeni nesil eğitim platformlarında (TryHackMe vb.) olduğu gibi, analizciler bulgularını sistemdeki interaktif doğrulama scriptine girerek bayrağı (Flag) elde ederler.

### ✨ Öne Çıkan Özellikler:

*   **Tarayıcı Tabanlı Terminal:** `ttyd` aracı kullanılarak Docker içindeki bash terminali doğrudan web tarayıcısına yansıtılır. SSH bağlantısı gerektirmez.
*   **Anti-Cheat (Hile Koruması):** Bayrakları (Flag) veren bash scriptleri `shc` ile derlenerek okunamaz makine kodu (Binary) haline getirilmiştir. Analizci, logları gerçekten analiz etmeden bayrağa ulaşamaz.
*   **Tam İzolasyon:** Her seviye kendi `docker-compose` ağı içinde yalıtılmış olarak çalışır.

### 🚀 Lab Nasıl Çalıştırılır?

Herhangi bir Docker yüklü sistemde, çözmek istediğiniz seviyenin klasörüne (Örn: `Altay_Sec_Cloud_Easy`) girip şu komutları çalıştırmak yeterlidir:

1. Konteyneri inşa edin ve başlatın:
```bash
docker-compose up -d --build
```
* Tarayıcınızdan terminale erişin (İlgili port üzerinden, örn: http://localhost:8080).

* Analizi bitirdiğinizde terminale ./submit yazarak interaktif sınav sistemini başlatın!

---

## 🟢 Level 1: Cloud Breach (Kolay)
**Odak Noktası:** AWS CloudTrail Log Analizi, İlk Erişim (Initial Access), IAM İhlalleri.

## 📝 Senaryo:
Altay Tech yazılım ekibinden bir çalışan, AWS Access Key (Erişim Anahtarı) bilgilerini yanlışlıkla GitHub'da herkese açık bir repoya push'lamıştır. Saldırganın bu anahtarlarla sistemimize sızdığından şüpheleniyoruz.

## 🎯 Görevler:

`cloudtrail_easy.json` dosyasını analiz et.

* Saldırganın kendi yetkilerini doğrulamak için tetiklediği `AWS API` olayını (`eventName`) bul.

* Sisteme yetkisiz erişim sağlayan IP adresini (`sourceIPAddress`) bul.

* Saldırganın şirketimizdeki hangi kullanıcının (`userName`) hesabına sızdığını tespit et.

* `./submit` çalıştır ve Flag'i kap.

---

## 🟡 Level 2: Persistence & Cryptojacking (Orta)
**Odak Noktası:** İleri Seviye JSON Analizi, IAM Arka Kapı (Persistence) Tespiti, EC2 İstismarı (Kripto Madencilik).

## 📝 Senaryo:
Level 1'deki saldırganın `'dev_ahmet'` hesabını kullandığını tespit ettik. Ancak bu hesap kapatılsa bile saldırganın sistemde kalabilmek için kendine gizli bir "Arka Kapı" (`Persistence`) hesabı oluşturduğunu ve kripto madencilik yapmak için büyük bir sanal sunucu başlattığını düşünüyoruz.

## 🎯 Görevler:

`cloudtrail_medium.json` dosyasını analiz et.

* Saldırganın arka kapı olarak oluşturduğu yeni gizli kullanıcının adını (`userName`) bul.

* Bu yeni kullanıcıya erişim anahtarı üretmek için kullanılan API eylemini (`eventName`) tespit et.

* Saldırganın madencilik için başlattığı sunucunun tipini (`instanceType`) bul.

* `./submit` çalıştır ve Flag'i kap.

---

### 🔴 Level 3: Privilege Escalation & Exfiltration (Zor)

**Odak Noktası:** İleri Seviye JSON Analizi, IAM Rol İstismarı (AssumeRole), S3 Veri Sızıntısı (Data Exfiltration).

## 📝 Senaryo:
Arka kapıyı tespit ettiniz ancak saldırganın içeride geçirdiği sürede sadece madencilik yapmadığını fark ettik. Saldırgan, ele geçirdiği "aws_svc_backup_admin" hesabı üzerinden daha yüksek yetkilere sahip kritik bir role bürünmüş (AssumeRole) ve şirketimizin en mahrem S3 bucket'ından müşteri verilerini sızdırmıştır. Loglar artık çok daha derin ve karmaşık.

## 🎯 Görevler:
*   `cloudtrail_hard.json` dosyasını analiz et.
  
*   Saldırganın hak yükseltmek (`Privilege Escalation`) için büründüğü IAM Rolünün tam ARN'sini (`roleArn`) bul.
  
*   Saldırganın sızdırdığı verileri çekerken hangi veri transfer aracını (`userAgent`) kullandığını tespit et.

*   'company-secure-vault' adlı S3 bucket'ından çalınan hassas dosyanın tam adını (`key`) bul.
  
*   `./submit` çalıştır ve Final Flag'i kap.

*Developed by Emir - Information Security Specialist / Blue Team Lab Researcher*
