# TLTrack-Mobile-Bridge

---

# 💻 CİHAZ BAĞLANTI REHBERİ (ADB SETUP)

Bu bölüm, Android cihazınızdaki oyun loglarını bilgisayarınıza aktarabilmeniz için gerekli olan bağlantı altyapısını kurmanızı sağlar.

---

# 🪟 WINDOWS

### Adım 1: Android Cihazı Hazırlama
1.  **Ayarlar > Telefon Hakkında** kısmına girin.
2.  **Yapım Numarası (Build Number)** üzerine 7 kez hızlıca tıklayarak "Geliştirici Seçeneklerini" açın.
3.  **Geliştirici Seçenekleri** menüsüne girin ve **USB Hata Ayıklama (USB Debugging)** modunu aktif edin.

### Adım 2: ADB Araçlarını İndirme
1.  [Google Platform Tools](https://developer.android.com/studio/releases/platform-tools) sayfasından Windows sürümünü indirin.
2.  İnen ZIP dosyasını klasöre çıkartın (Örneğin: `C:\adb`).

### Adım 3: Sistem Yolu (Path) Ayarı
1.  Arama çubuğuna **"Sistem ortam değişkenlerini düzenleyin"** yazın.
2.  **Ortam Değişkenleri > Path > Düzenle > Yeni** yolunu izleyerek `C:\adb` klasörünü ekleyin.

### Adım 4: Bağlantı ve Güvenlik Onayı
1.  **PowerShell**'i açın ve şu komutu yazın: `adb devices`
2.  **Önemli:** Cihaz ekranında çıkan "USB Hata Ayıklamasına izin verilsin mi?" uyarısına **"Her zaman izin ver"** diyerek onay verin.

> [!TIP]
> **Windows Kablosuz Bağlantı:**
> Cihazı bir kez kabloyla bağlayıp `adb tcpip 5555` yazın. Ardından kabloyu çekip `adb connect TABLET_IP_ADRESI:5555` komutuyla tamamen kablosuz çalışabilirsiniz.

---

# 🍎 MACOS

### Adım 1: Android Cihazda Hata Ayıklamayı Açma
1.  **Ayarlar > Telefon Hakkında** yolunu izleyin.
2.  **Derleme Numarası (Build Number)** üzerine 7 kez tıklayarak geliştirici modunu açın.
3.  **Sistem > Geliştirici Seçenekleri** altından **USB Hata Ayıklama**'yı aktif edin.

### Adım 2: ADB Kurulumu (Homebrew)
Terminal'i açın ve şu komutları sırayla çalıştırın:
```bash
# Homebrew yüklü değilse kurun:
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"

# ADB'yi kurun:
brew install android-platform-tools
```

### Adım 3: Cihazı Tanıtma ve Onay
1.  Cihazı Mac'e bağlayın ve Terminal'e şunu yazın: `adb devices`
2.  Cihaz ekranındaki **"Bu bilgisayara her zaman izin ver"** uyarısını onaylayın.
3.  Listede cihaz isminin yanında `device` yazısını gördüğünüzde bağlantı hazırdır.

### Adım 4: Kablosuz Bağlantı Kurulumu (Bonus)
Kabloya bağlı kalmadan veri çekmek için:
1.  Kablo takılıyken: `adb tcpip 5555`
2.  Tabletin IP adresini bulun (Ayarlar > Durum).
3.  Kabloyu çekin ve bağlanın: `adb connect 192.168.1.XX:5555`

> [!NOTE]
> **Bağlantı Testi:** Her şeyin doğru olduğunu teyit etmek için şu komutu kullanın:
> `adb shell ls /sdcard/Android/data/com.xd.TLglobal/files/UE4Game/UE_game/UE_game/Saved/Logs/`
> Eğer `UE_game.log` dosyasını görüyorsanız sistem kusursuz çalışıyor demektir.

---

### ⚠️ Kritik Hatırlatmalar
* **Kablo:** Bağlantı sürekli kopuyorsa mutlaka veri transferi destekleyen orijinal bir kablo kullanın.
* **M1/M2/M3 Mac'ler:** Apple Silicon işlemcili cihazlarda kurulum tamamen aynıdır ve sorunsuz çalışır.
* **Hata:** Eğer "Connection Refused" alıyorsanız, cihazın ve bilgisayarın aynı Wi-Fi ağında olduğundan emin olun.

---

Esat, bu düzenleme hem görsel olarak GitHub'ın yeni uyarı kutularını (`[!TIP]`, `[!IMPORTANT]`) kullanıyor hem de senin istediğin gibi başlıkları büyük ve belirgin hale getiriyor. Bu bölümden sonra "Veri Çekme" ve "TITrack Kurulumu" bölümlerini ekleyerek rehberi tamamlayabilirsin. Elimize sağlık!





## 5. Adım: Veri Köprüsünü Kurma (Otomatik Klasör ve Veri Akışı)

Bu aşamada, bilgisayarınızda hiçbir manuel hazırlık yapmanıza gerek yok. Aşağıdaki "Akıllı Komutlar" sizin yerinize Masaüstünüzde gerekli klasörü oluşturacak ve tabletinizdeki oyun verilerini her 5 saniyede bir bilgisayarınıza güvenli bir şekilde çekecektir.

### Neden Bu Komutları Kullanıyoruz?
Doğrudan kopyalamak yerine "Temp" (Geçici dosya) mantığını kullanıyoruz. Bu sayede veri transferi sırasında oluşabilecek takılmaların önüne geçiyoruz ve loot fiyatlarının **0 FE** görünmesini engelliyoruz.

---

### **B. Windows Kullanıcıları İçin**
PowerShell'i açın ve aşağıdaki komutu olduğu gibi yapıştırıp Enter'a basın:

```powershell
if (!(Test-Path "$HOME\Desktop\TLI_Data")) { New-Item -ItemType Directory -Path "$HOME\Desktop\TLI_Data" }; while($true) { adb pull "/sdcard/Android/data/com.xd.TLglobal/files/UE4Game/UE_game/UE_game/Saved/Logs/UE_game.log" "$HOME\Desktop\TLI_Data\UE_game.log.tmp"; Move-Item -Force "$HOME\Desktop\TLI_Data\UE_game.log.tmp" "$HOME\Desktop\TLI_Data\UE_game.log"; Write-Host -NoNewline "."; Start-Sleep -Seconds 5 }
```
### **A. macOS Kullanıcıları İçin**
Terminal'i açın ve aşağıdaki komutu olduğu gibi yapıştırıp Enter'a basın:

```bash
mkdir -p ~/Desktop/TLI_Data && while true; do adb pull "/sdcard/Android/data/com.xd.TLglobal/files/UE4Game/UE_game/UE_game/Saved/Logs/UE_game.log" ~/Desktop/TLI_Data/UE_game.log.tmp > /dev/null 2>&1; mv ~/Desktop/TLI_Data/UE_game.log.tmp ~/Desktop/TLI_Data/UE_game.log; echo -n "."; sleep 5; done
```
---

### **Çalıştığını Nasıl Anlarım?**
1.  Komutu çalıştırdıktan sonra Terminal/PowerShell ekranında her 5 saniyede bir **nokta (`.`)** işareti belirecektir. Her nokta, verinin başarıyla güncellendiği anlamına gelir.
2.  Masaüstünüzde otomatik olarak `TLI_Data` isimli bir klasör oluştuğunu ve içinde `UE_game.log` dosyasının geldiğini göreceksiniz.

### **Dikkat Edilmesi Gerekenler:**
* **Bağlantı Hatası:** Eğer noktalar ilerlemiyorsa, tabletinizin kablosunu kontrol edin veya "Kablosuz Bağlantı" adımını tekrarlayın.
* **Dosya Yolu:** Oyunun farklı sürümlerinde (Global/Asya) dosya yolları değişebilir. Eğer hata alırsanız cihazınızdaki dosya yolunun `/sdcard/Android/data/com.xd.TLglobal/...` olduğundan emin olun.
* **Durdurma:** İşlemi durdurmak istediğinizde Terminal penceresindeyken `Ctrl + C` tuşlarına basmanız yeterlidir.

---

## 6. Adım: TITrack Yazılımının Kurulumu

Verilerimiz masaüstündeki `TLI_Data` klasörüne gelmeye başladığına göre, şimdi bu verileri okuyup web sayfasına dökecek olan yazılımı kuralım.

### 1. Projeyi Bilgisayarınıza İndirin
Önce yazılımın dosyalarını GitHub'dan bilgisayarınıza çekmeniz gerekiyor.

* **Mac Kullanıcıları (Terminal):**
    ```bash
    cd ~/Desktop && git clone https://github.com/astockman99/TITrack.git
    ```
* **Windows Kullanıcıları (PowerShell):**
    ```powershell
    cd "$HOME\Desktop"; git clone https://github.com/astockman99/TITrack.git
    ```

### 2. Gerekli Kütüphaneleri Yükleyin
Yazılımın "fiyat çekme" ve "veritabanı" özelliklerinin çalışması için şu ek paketleri yüklemeliyiz:

* **Mac Kullanıcıları:**
    ```bash
    python3 -m pip install -r ~/Desktop/TITrack/requirements.txt
    python3 -m pip install supabase postgrest --break-system-packages
    ```
* **Windows Kullanıcıları:**
    ```powershell
    pip install -r "$HOME\Desktop\TITrack\requirements.txt"
    pip install supabase postgrest
    ```

---

## 7. Adım: İlk Kurulum (Veritabanını Hazırlama)

Yazılımı ilk kez çalıştırdığımızda, oyun içindeki eşyaların isimlerini ve temel bilgilerini tanıması için bir "hazırlık" (init) yapmamız gerekiyor. **Bunu sadece bir kez yapacaksınız.**

* **Mac Kullanıcıları:**
    ```bash
    cd ~/Desktop/TITrack && export PYTHONPATH=$PYTHONPATH:$(pwd)/src && python3 -m titrack init
    ```
* **Windows Kullanıcıları:**
    ```powershell
    cd "$HOME\Desktop\TITrack"; $env:PYTHONPATH = ".\src"; python -m titrack init
    ```

---

## 8. Adım: Analizi Başlatma (Tail & Serve)

Şimdi her şeyi birleştiriyoruz. İki ayrı terminal penceresi açın.

### **Pencere A: Veri İşleyici (Tail)**
Bu işlem, masaüstüne akan logları anlık olarak okur.
* **Mac:**
    ```bash
    cd ~/Desktop/TITrack && export PYTHONPATH=$PYTHONPATH:$(pwd)/src && python3 -m titrack tail ~/Desktop/TLI_Data/UE_game.log
    ```
* **Windows:**
    ```powershell
    cd "$HOME\Desktop\TITrack"; $env:PYTHONPATH = ".\src"; python -m titrack tail "$HOME\Desktop\TLI_Data\UE_game.log"
    ```

### **Pencere B: Web Sayfası (Serve)**
Bu işlem, lootları göreceğiniz sayfayı yayınlar.
* **Mac:**
    ```bash
    cd ~/Desktop/TITrack && export PYTHONPATH=$PYTHONPATH:$(pwd)/src && python3 -m titrack serve --no-window
    ```
* **Windows:**
    ```powershell
    cd "$HOME\Desktop\TITrack"; $env:PYTHONPATH = ".\src"; python -m titrack serve --no-window
    ```

---

## 9. Adım: Son Ayarlar ve Kullanım

Her şey çalışıyorsa, artık ödülünüzü alma zamanı:

1.  Tarayıcınızı (Chrome/Edge) açın ve adres çubuğuna şunu yazın: `http://localhost:8000`
2.  Sol menüden **Settings** (Ayarlar) sekmesine gidin.
3.  **Game Directory** yazan yere masaüstündeki veri klasörünüzü girin:
    * **Mac:** `/Users/KullaniciAdiniz/Desktop/TLI_Data`
    * **Windows:** `C:\Users\KullaniciAdiniz\Desktop\TLI_Data`
4.  **Cloud Sync** kutucuğunu işaretlemeyi unutmayın!

---

### **Rehber İçin Not:**
Esat, kullanıcılara şu uyarıyı mutlaka yapmalısın: *"Eğer web sayfasında her şey '0' görünüyorsa, 'Cloud Sync' kutucuğunun işaretli olduğundan ve internetinizin aktif olduğundan emin olun."*

Esat, harika bir noktaya değindin. Senin yaşadığın o "Cloud Sync butonuna basamama" sorunu, aslında rehberin en kritik çözümü olacak. Eğer kullanıcı sadece orijinal `requirements.txt` dosyasındaki kütüphaneleri kurarsa, **Cloud Sync butonu donuk (gri) kalır ve tıklanamaz.** Bunun sebebi, uygulamanın bulut özelliklerini çalıştırmak için `supabase` kütüphanesine ihtiyaç duyması ama bu kütüphanenin orijinal kurulum listesinde olmamasıdır.

Rehberine bu durumu şu şekilde **"Çözüm Odaklı"** ekleyelim:

---

### **⚠️ ÖNEMLİ: Cloud Sync Butonu Tıklanmıyor mu? (Çözümü)**

Eğer web arayüzünde "Cloud Sync" kutucuğuna tıklayamıyorsanız veya fiyatlar `0` görünüyorsa, bilgisayarınızda bulut bağlantısını sağlayan yan paketler eksik demektir. Bu sorunu çözmek için şu adımı uygulayın:

**1. Tüm terminalleri durdurun (Ctrl + C).**

**2. Şu komutu çalıştırarak eksik kütüphaneleri yükleyin:**
* **Mac Kullanıcıları:**
  ```bash
  python3 -m pip install supabase postgrest --break-system-packages
  ```
* **Windows Kullanıcıları:**
  ```powershell
  pip install supabase postgrest
  ```

**3. Uygulamayı tekrar başlatın.** Artık Cloud Sync kutucuğunun aktif olduğunu ve fiyatların internetten akmaya başladığını göreceksiniz.

---



















































