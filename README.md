# TLTrack-Mobile-Bridge
---

## 🌍 Language / Dil
* [English](#english-version)
* [Türkçe](#türkçe-versiyon)

---

<a name="english-version"></a>
# 🛡️ TLI-Mobile-Mac-Bridge (English) (TITrack Mobile Integration)

This project provides a seamless bridge to sync **Torchlight: Infinite** game logs from an Android device (Phone/Tablet) to a PC or Mac, enabling real-time loot analysis using **TITrack**.

---

## 💻 DEVICE CONNECTION GUIDE (ADB SETUP)

This section covers setting up the bridge to transfer game logs from your Android device to your computer.

---

# 🪟 WINDOWS SETUP

### Step 1: Prepare Your Android Device
1.  Go to **Settings > About Phone**.
2.  Tap **Build Number** 7 times rapidly until "Developer Options" is enabled.
3.  Enter **Developer Options** and enable **USB Debugging**.

### Step 2: Download ADB Tools
1.  Download the Windows version of [Google Platform Tools](https://developer.android.com/studio/releases/platform-tools).
2.  Extract the ZIP file to a permanent location (e.g., `C:\adb`).

### Step 3: Set System Environment Path
1.  Search for **"Edit the system environment variables"** in the Windows search bar.
2.  Navigate to **Environment Variables > Path > Edit > New** and add the path to your ADB folder (e.g., `C:\adb`).

### Step 4: Connection & Security Authorization
1.  Open **PowerShell** and type: `adb devices`.
2.  **Crucial:** Check your device screen. When the "Allow USB Debugging?" prompt appears, check **"Always allow from this computer"** and tap **OK**.

> [!TIP]
> **Windows Wireless Connection:**
> Connect via cable once and run `adb tcpip 5555`. Unplug the cable and run `adb connect YOUR_TABLET_IP:5555` to work completely wirelessly.

---

# 🍎 MACOS SETUP

### Step 1: Enable Debugging on Android
1.  Go to **Settings > About Device**.
2.  Tap **Build Number** 7 times to enable developer mode.
3.  Under **System > Developer Options**, enable **USB Debugging**.

### Step 2: Install ADB (via Homebrew)
Open **Terminal** and run these commands:
```bash
# Install Homebrew if you haven't already:
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"

# Install ADB:
brew install android-platform-tools
```

### Step 3: Device Authorization
1.  Connect your device to your Mac and type: `adb devices`.
2.  Authorize the connection on your device screen by selecting **"Always allow"**.
3.  Connection is successful if you see your device ID followed by `device`.

### Step 4: Wireless Setup (Bonus)
To sync data without cables:
1.  While plugged in: `adb tcpip 5555`
2.  Find your tablet's IP address (Settings > Status).
3.  Unplug and connect: `adb connect 192.168.1.XX:5555`

> [!NOTE]
> **Connection Test:** To verify the path is correct, run:
> `adb shell ls /sdcard/Android/data/com.xd.TLglobal/files/UE4Game/UE_game/UE_game/Saved/Logs/`
> If you see `UE_game.log`, your setup is perfect.

---

## Step 5: Setting Up the Data Bridge (Auto-Sync)

No manual folder creation is required. The following commands will automatically create a folder on your **Desktop** and pull game data every 5 seconds.

### Why "Temp" Files?
We use a "Temp" file logic during the `adb pull` process. Bu prevents data corruption and ensures loot prices do not appear as **0 FE** while the file is being written.

---

### **A. For Windows Users**
Open **PowerShell** and paste:
```powershell
if (!(Test-Path "$HOME\Desktop\TLI_Data")) { New-Item -ItemType Directory -Path "$HOME\Desktop\TLI_Data" }; while($true) { adb pull "/sdcard/Android/data/com.xd.TLglobal/files/UE4Game/UE_game/UE_game/Saved/Logs/UE_game.log" "$HOME\Desktop\TLI_Data\UE_game.log.tmp"; Move-Item -Force "$HOME\Desktop\TLI_Data\UE_game.log.tmp" "$HOME\Desktop\TLI_Data\UE_game.log"; Write-Host -NoNewline "."; Start-Sleep -Seconds 5 }
```

### **B. For macOS Users**
Open **Terminal** and paste:
```bash
mkdir -p ~/Desktop/TLI_Data && while true; do adb pull "/sdcard/Android/data/com.xd.TLglobal/files/UE4Game/UE_game/UE_game/Saved/Logs/UE_game.log" ~/Desktop/TLI_Data/UE_game.log.tmp > /dev/null 2>&1; mv ~/Desktop/TLI_Data/UE_game.log.tmp ~/Desktop/TLI_Data/UE_game.log; echo -n "."; sleep 5; done
```

---

## Step 6: Installing TITrack Software

### 1. Clone the Project
* **Mac:** `cd ~/Desktop && git clone https://github.com/astockman99/TITrack.git`
* **Windows:** `cd "$HOME\Desktop"; git clone https://github.com/astockman99/TITrack.git`

### 2. Install Required Libraries
* **Mac:**
    ```bash
    python3 -m pip install -r ~/Desktop/TITrack/requirements.txt
    python3 -m pip install supabase postgrest --break-system-packages
    ```
* **Windows:**
    ```powershell
    pip install -r "$HOME\Desktop\TITrack\requirements.txt"
    pip install supabase postgrest
    ```

---

## Step 7: Initial Setup (Database Init)
*Required only once.*
* **Mac:** `cd ~/Desktop/TITrack && export PYTHONPATH=$PYTHONPATH:$(pwd)/src && python3 -m titrack init`
* **Windows:** `cd "$HOME\Desktop\TITrack"; $env:PYTHONPATH = ".\src"; python -m titrack init`

---

## Step 8: Start Analysis (Tail & Serve)
Open **two separate** terminal windows.

### **Window A: Log Processor (Tail)**
* **Mac:** `cd ~/Desktop/TITrack && export PYTHONPATH=$PYTHONPATH:$(pwd)/src && python3 -m titrack tail ~/Desktop/TLI_Data/UE_game.log`
* **Windows:** `cd "$HOME\Desktop\TITrack"; $env:PYTHONPATH = ".\src"; python -m titrack tail "$HOME\Desktop\TLI_Data\UE_game.log"`

### **Window B: Web Interface (Serve)**
* **Mac:** `cd ~/Desktop/TITrack && export PYTHONPATH=$PYTHONPATH:$(pwd)/src && python3 -m titrack serve --no-window`
* **Windows:** `cd "$HOME\Desktop\TITrack"; $env:PYTHONPATH = ".\src"; python -m titrack serve --no-window`

---

## Step 9: Final Configuration
1.  Open your browser and go to: `http://localhost:8000`
2.  Go to **Settings**.
3.  Set **Game Directory** to your desktop data folder:
    * **Mac:** `/Users/YourUsername/Desktop/TLI_Data`
    * **Windows:** `C:\Users\YourUsername\Desktop\TLI_Data`
4.  Enable **Cloud Sync** to fetch live market prices.

---

### **⚠️ TROUBLESHOOTING: Cloud Sync Button Not Working?**
If the "Cloud Sync" checkbox is greyed out or prices show as `0`, you are missing the cloud bridge libraries. Run this:
* **Mac:** `python3 -m pip install supabase postgrest --break-system-packages`
* **Windows:** `pip install supabase postgrest`

---

## 🔄 DAILY USE: RESTARTING THE SYSTEM
If you restart your computer, follow these 3 steps:

1.  **Reconnect ADB:** `adb connect TABLET_IP:5555`
2.  **Run Data Bridge:** Paste the `adb pull` loop command (from Step 5).
3.  **Start TITrack:** Run the **Tail** and **Serve** commands in separate windows.

> [!CAUTION]
> If you restart your tablet, you may need to reconnect via cable once and run `adb tcpip 5555` to re-enable wireless debugging.

---

<a name="türkçe-versiyon"></a>
# 🛡️ TLI-Mobil-Bilgisayar Köprüsü (Türkçe)


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

## 🔄 GÜNLÜK KULLANIM: SİSTEMİ YENİDEN BAŞLATMA

Bilgisayarınızı kapattıktan sonra sistemi tekrar ayağa kaldırmak için şu 3 adımı uygulamanız yeterlidir. (Kurulumları zaten yaptığınızı varsayıyoruz).

### 1. ADB Bağlantısını Yenileyin
Cihazınız kablosuz bağlıysa Terminal/PowerShell açıp şu komutu yazın:
```bash
adb connect TABLET_IP_ADRESINIZ:5555
```

### 2. Veri Köprüsünü (Data Bridge) Çalıştırın
Bu komut Masaüstündeki klasörü kontrol eder ve veri çekmeye başlar.

**macOS:**
```bash
mkdir -p ~/Desktop/TLI_Data && while true; do adb pull "/sdcard/Android/data/com.xd.TLglobal/files/UE4Game/UE_game/UE_game/Saved/Logs/UE_game.log" ~/Desktop/TLI_Data/UE_game.log.tmp > /dev/null 2>&1; mv ~/Desktop/TLI_Data/UE_game.log.tmp ~/Desktop/TLI_Data/UE_game.log; echo -n "."; sleep 5; done
```

**Windows (PowerShell):**
```powershell
if (!(Test-Path "$HOME\Desktop\TLI_Data")) { New-Item -ItemType Directory -Path "$HOME\Desktop\TLI_Data" }; while($true) { adb pull "/sdcard/Android/data/com.xd.TLglobal/files/UE4Game/UE_game/UE_game/Saved/Logs/UE_game.log" "$HOME\Desktop\TLI_Data\UE_game.log.tmp"; Move-Item -Force "$HOME\Desktop\TLI_Data\UE_game.log.tmp" "$HOME\Desktop\TLI_Data\UE_game.log"; Write-Host -NoNewline "."; Start-Sleep -Seconds 5 }
```

### 3. TITrack Analiz ve Web Sunucusunu Başlatın
İki ayrı pencerede şu komutları çalıştırın (Yolların doğruluğundan emin olun):

**Pencere A (Analiz - Tail):**
```bash
# Mac için:
cd ~/Desktop/TITrack && export PYTHONPATH=$PYTHONPATH:$(pwd)/src && python3 -m titrack tail ~/Desktop/TLI_Data/UE_game.log

# Windows için:
cd "$HOME\Desktop\TITrack"; $env:PYTHONPATH = ".\src"; python -m titrack tail "$HOME\Desktop\TLI_Data\UE_game.log"
```

**Pencere B (Web Sunucu - Serve):**
```bash
# Mac için:
cd ~/Desktop/TITrack && export PYTHONPATH=$PYTHONPATH:$(pwd)/src && python3 -m titrack serve --no-window

# Windows için:
cd "$HOME\Desktop\TITrack"; $env:PYTHONPATH = ".\src"; python -m titrack serve --no-window
```

> [!CAUTION]
> Tabletinizi tamamen kapatıp açtıysanız, kablosuz bağlantının tekrar aktif olması için bir kez kabloyla bağlayıp `adb tcpip 5555` komutunu vermeniz gerekebilir.

---
























# TLTrack-Mobile-Bridge
---

# 🛡️ TLI-Mobile-Mac-Bridge (TITrack Mobile Integration)

This project provides a seamless bridge to sync **Torchlight: Infinite** game logs from an Android device (Phone/Tablet) to a PC or Mac, enabling real-time loot analysis using **TITrack**.

---

## 💻 DEVICE CONNECTION GUIDE (ADB SETUP)

This section covers setting up the bridge to transfer game logs from your Android device to your computer.

---

# 🪟 WINDOWS SETUP

### Step 1: Prepare Your Android Device
1.  Go to **Settings > About Phone**.
2.  Tap **Build Number** 7 times rapidly until "Developer Options" is enabled.
3.  Enter **Developer Options** and enable **USB Debugging**.

### Step 2: Download ADB Tools
1.  Download the Windows version of [Google Platform Tools](https://developer.android.com/studio/releases/platform-tools).
2.  Extract the ZIP file to a permanent location (e.g., `C:\adb`).

### Step 3: Set System Environment Path
1.  Search for **"Edit the system environment variables"** in the Windows search bar.
2.  Navigate to **Environment Variables > Path > Edit > New** and add the path to your ADB folder (e.g., `C:\adb`).

### Step 4: Connection & Security Authorization
1.  Open **PowerShell** and type: `adb devices`.
2.  **Crucial:** Check your device screen. When the "Allow USB Debugging?" prompt appears, check **"Always allow from this computer"** and tap **OK**.

> [!TIP]
> **Windows Wireless Connection:**
> Connect via cable once and run `adb tcpip 5555`. Unplug the cable and run `adb connect YOUR_TABLET_IP:5555` to work completely wirelessly.

---

# 🍎 MACOS SETUP

### Step 1: Enable Debugging on Android
1.  Go to **Settings > About Device**.
2.  Tap **Build Number** 7 times to enable developer mode.
3.  Under **System > Developer Options**, enable **USB Debugging**.

### Step 2: Install ADB (via Homebrew)
Open **Terminal** and run these commands:
```bash
# Install Homebrew if you haven't already:
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"

# Install ADB:
brew install android-platform-tools
```

### Step 3: Device Authorization
1.  Connect your device to your Mac and type: `adb devices`.
2.  Authorize the connection on your device screen by selecting **"Always allow"**.
3.  Connection is successful if you see your device ID followed by `device`.

### Step 4: Wireless Setup (Bonus)
To sync data without cables:
1.  While plugged in: `adb tcpip 5555`
2.  Find your tablet's IP address (Settings > Status).
3.  Unplug and connect: `adb connect 192.168.1.XX:5555`

> [!NOTE]
> **Connection Test:** To verify the path is correct, run:
> `adb shell ls /sdcard/Android/data/com.xd.TLglobal/files/UE4Game/UE_game/UE_game/Saved/Logs/`
> If you see `UE_game.log`, your setup is perfect.

---

## Step 5: Setting Up the Data Bridge (Auto-Sync)

No manual folder creation is required. The following commands will automatically create a folder on your **Desktop** and pull game data every 5 seconds.

### Why "Temp" Files?
We use a "Temp" file logic during the `adb pull` process. Bu prevents data corruption and ensures loot prices do not appear as **0 FE** while the file is being written.

---

### **A. For Windows Users**
Open **PowerShell** and paste:
```powershell
if (!(Test-Path "$HOME\Desktop\TLI_Data")) { New-Item -ItemType Directory -Path "$HOME\Desktop\TLI_Data" }; while($true) { adb pull "/sdcard/Android/data/com.xd.TLglobal/files/UE4Game/UE_game/UE_game/Saved/Logs/UE_game.log" "$HOME\Desktop\TLI_Data\UE_game.log.tmp"; Move-Item -Force "$HOME\Desktop\TLI_Data\UE_game.log.tmp" "$HOME\Desktop\TLI_Data\UE_game.log"; Write-Host -NoNewline "."; Start-Sleep -Seconds 5 }
```

### **B. For macOS Users**
Open **Terminal** and paste:
```bash
mkdir -p ~/Desktop/TLI_Data && while true; do adb pull "/sdcard/Android/data/com.xd.TLglobal/files/UE4Game/UE_game/UE_game/Saved/Logs/UE_game.log" ~/Desktop/TLI_Data/UE_game.log.tmp > /dev/null 2>&1; mv ~/Desktop/TLI_Data/UE_game.log.tmp ~/Desktop/TLI_Data/UE_game.log; echo -n "."; sleep 5; done
```

---

## Step 6: Installing TITrack Software

### 1. Clone the Project
* **Mac:** `cd ~/Desktop && git clone https://github.com/astockman99/TITrack.git`
* **Windows:** `cd "$HOME\Desktop"; git clone https://github.com/astockman99/TITrack.git`

### 2. Install Required Libraries
* **Mac:**
    ```bash
    python3 -m pip install -r ~/Desktop/TITrack/requirements.txt
    python3 -m pip install supabase postgrest --break-system-packages
    ```
* **Windows:**
    ```powershell
    pip install -r "$HOME\Desktop\TITrack\requirements.txt"
    pip install supabase postgrest
    ```

---

## Step 7: Initial Setup (Database Init)
*Required only once.*
* **Mac:** `cd ~/Desktop/TITrack && export PYTHONPATH=$PYTHONPATH:$(pwd)/src && python3 -m titrack init`
* **Windows:** `cd "$HOME\Desktop\TITrack"; $env:PYTHONPATH = ".\src"; python -m titrack init`

---

## Step 8: Start Analysis (Tail & Serve)
Open **two separate** terminal windows.

### **Window A: Log Processor (Tail)**
* **Mac:** `cd ~/Desktop/TITrack && export PYTHONPATH=$PYTHONPATH:$(pwd)/src && python3 -m titrack tail ~/Desktop/TLI_Data/UE_game.log`
* **Windows:** `cd "$HOME\Desktop\TITrack"; $env:PYTHONPATH = ".\src"; python -m titrack tail "$HOME\Desktop\TLI_Data\UE_game.log"`

### **Window B: Web Interface (Serve)**
* **Mac:** `cd ~/Desktop/TITrack && export PYTHONPATH=$PYTHONPATH:$(pwd)/src && python3 -m titrack serve --no-window`
* **Windows:** `cd "$HOME\Desktop\TITrack"; $env:PYTHONPATH = ".\src"; python -m titrack serve --no-window`

---

## Step 9: Final Configuration
1.  Open your browser and go to: `http://localhost:8000`
2.  Go to **Settings**.
3.  Set **Game Directory** to your desktop data folder:
    * **Mac:** `/Users/YourUsername/Desktop/TLI_Data`
    * **Windows:** `C:\Users\YourUsername\Desktop\TLI_Data`
4.  Enable **Cloud Sync** to fetch live market prices.

---

### **⚠️ TROUBLESHOOTING: Cloud Sync Button Not Working?**
If the "Cloud Sync" checkbox is greyed out or prices show as `0`, you are missing the cloud bridge libraries. Run this:
* **Mac:** `python3 -m pip install supabase postgrest --break-system-packages`
* **Windows:** `pip install supabase postgrest`

---

## 🔄 DAILY USE: RESTARTING THE SYSTEM
If you restart your computer, follow these 3 steps:

1.  **Reconnect ADB:** `adb connect TABLET_IP:5555`
2.  **Run Data Bridge:** Paste the `adb pull` loop command (from Step 5).
3.  **Start TITrack:** Run the **Tail** and **Serve** commands in separate windows.

> [!CAUTION]
> If you restart your tablet, you may need to reconnect via cable once and run `adb tcpip 5555` to re-enable wireless debugging.

---

# FOR TURKİSH

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

## 🔄 GÜNLÜK KULLANIM: SİSTEMİ YENİDEN BAŞLATMA

Bilgisayarınızı kapattıktan sonra sistemi tekrar ayağa kaldırmak için şu 3 adımı uygulamanız yeterlidir. (Kurulumları zaten yaptığınızı varsayıyoruz).

### 1. ADB Bağlantısını Yenileyin
Cihazınız kablosuz bağlıysa Terminal/PowerShell açıp şu komutu yazın:
```bash
adb connect TABLET_IP_ADRESINIZ:5555
```

### 2. Veri Köprüsünü (Data Bridge) Çalıştırın
Bu komut Masaüstündeki klasörü kontrol eder ve veri çekmeye başlar.

**macOS:**
```bash
mkdir -p ~/Desktop/TLI_Data && while true; do adb pull "/sdcard/Android/data/com.xd.TLglobal/files/UE4Game/UE_game/UE_game/Saved/Logs/UE_game.log" ~/Desktop/TLI_Data/UE_game.log.tmp > /dev/null 2>&1; mv ~/Desktop/TLI_Data/UE_game.log.tmp ~/Desktop/TLI_Data/UE_game.log; echo -n "."; sleep 5; done
```

**Windows (PowerShell):**
```powershell
if (!(Test-Path "$HOME\Desktop\TLI_Data")) { New-Item -ItemType Directory -Path "$HOME\Desktop\TLI_Data" }; while($true) { adb pull "/sdcard/Android/data/com.xd.TLglobal/files/UE4Game/UE_game/UE_game/Saved/Logs/UE_game.log" "$HOME\Desktop\TLI_Data\UE_game.log.tmp"; Move-Item -Force "$HOME\Desktop\TLI_Data\UE_game.log.tmp" "$HOME\Desktop\TLI_Data\UE_game.log"; Write-Host -NoNewline "."; Start-Sleep -Seconds 5 }
```

### 3. TITrack Analiz ve Web Sunucusunu Başlatın
İki ayrı pencerede şu komutları çalıştırın (Yolların doğruluğundan emin olun):

**Pencere A (Analiz - Tail):**
```bash
# Mac için:
cd ~/Desktop/TITrack && export PYTHONPATH=$PYTHONPATH:$(pwd)/src && python3 -m titrack tail ~/Desktop/TLI_Data/UE_game.log

# Windows için:
cd "$HOME\Desktop\TITrack"; $env:PYTHONPATH = ".\src"; python -m titrack tail "$HOME\Desktop\TLI_Data\UE_game.log"
```

**Pencere B (Web Sunucu - Serve):**
```bash
# Mac için:
cd ~/Desktop/TITrack && export PYTHONPATH=$PYTHONPATH:$(pwd)/src && python3 -m titrack serve --no-window

# Windows için:
cd "$HOME\Desktop\TITrack"; $env:PYTHONPATH = ".\src"; python -m titrack serve --no-window
```

> [!CAUTION]
> Tabletinizi tamamen kapatıp açtıysanız, kablosuz bağlantının tekrar aktif olması için bir kez kabloyla bağlayıp `adb tcpip 5555` komutunu vermeniz gerekebilir.


















































