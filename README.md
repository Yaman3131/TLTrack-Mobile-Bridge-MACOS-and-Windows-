# TLTrack-Mobile-Bridge

Esat, rehberin her iki platform için de profesyonel, temiz ve GitHub'da parlayacak bir hale geldi. Windows ve macOS bölümlerini net bir hiyerarşiyle, senin istediğin gibi başlıkları belirginleştirerek ve önemli uyarıları ekleyerek tek bir metinde topladım.

Bu metni doğrudan GitHub `README.md` dosyana kopyalayabilirsin:

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

# WİNDOWS

---

---

### Adım 1: Android Cihazı Hazırlama (Telefon/Tablet)
Bu adım Mac ile aynıdır, cihazın kapısını dış dünyaya açıyoruz:
1.  **Ayarlar > Telefon Hakkında** kısmına girin.
2.  **Yapım Numarası (Build Number)** üzerine 7 kez hızlıca tıklayarak "Geliştirici Seçeneklerini" açın.
3.  **Geliştirici Seçenekleri** menüsüne girin ve **USB Hata Ayıklama (USB Debugging)** modunu aktif edin.

---

### Adım 2: Windows İçin ADB Araçlarını İndirme
Windows'ta otomatik bir yükleyici yerine Google'ın resmi araçlarını manuel indirmek en sağlıklısıdır:
1.  [Google Platform Tools](https://developer.android.com/studio/releases/platform-tools) sayfasına gidin ve **"Download SDK Platform-Tools for Windows"** seçeneğine tıklayın.
2.  İnen ZIP dosyasını klasöre çıkartın. 
3.  **Önemli:** Bu klasörü kolay bulabileceğiniz bir yere taşıyın (Örneğin: `C:\adb`).

---

### Adım 3: ADB'yi Windows Sistemine Tanıtma (Path Ayarı)
Eğer bu adımı yapmazsanız, her seferinde `C:\adb` klasörünün içine girmek zorunda kalırsınız. Bunu engellemek için:
1.  Windows arama çubuğuna **"Sistem ortam değişkenlerini düzenleyin"** yazın ve açın.
2.  **Ortam Değişkenleri (Environment Variables)** butonuna tıklayın.
3.  Alt kısmdaki "Sistem değişkenleri" listesinde **Path** yazanı bulun ve **Düzenle** deyin.
4.  **Yeni** butonuna basın ve ADB'yi koyduğunuz klasörün yolunu yapıştırın (Örn: `C:\adb`).
5.  Tamam diyerek tüm pencereleri kapatın.

---

### Adım 4: USB Sürücülerini (Driver) Kontrol Etme
Windows bazen Android cihazı "Bilinmeyen Aygıt" olarak görür.
1.  Cihazı USB ile bağlayın.
2.  **Aygıt Yöneticisi**'ni açın.
3.  Eğer cihazınızın yanında sarı bir ünlem varsa, sağ tıklayıp "Sürücüyü Güncelleştir" deyin ve Windows Update üzerinden sürücüyü bulmasını sağlayın. (Samsung, Xiaomi gibi markaların kendi "USB Driver" paketlerini sitelerinden indirmeniz gerekebilir).

---

### Adım 5: İlk Bağlantı ve Güvenlik Onayı
1.  Bir **PowerShell** veya **Komut İstemi (CMD)** penceresi açın.
2.  Şu komutu yazın:
    ```powershell
    adb devices
    ```
3.  **Hayati Adım:** Telefonunuzun/Tabletinizin ekranına bakın! *"USB Hata Ayıklamasına izin verilsin mi?"* uyarısı çıkacak.
4.  **"Bu bilgisayara her zaman izin ver"** kutucuğunu işaretleyin ve **Tamam** deyin.
5.  Ekranda cihazınızın ID'sini ve yanında `device` yazısını gördüğünüzde Windows artık tableti kontrol edebilir demektir.

---

### Windows Kullanıcıları İçin Kritik Notlar:
* **Kablo Kalitesi:**  Eğer cihaz sürekli kopuyorsa veya listede görünmüyorsa mutlaka kasanın **arka tarafındaki** ana USB portlarını kullanın.
* **PowerShell vs CMD:** Bu rehberdeki komutlar her ikisinde de çalışır, ancak modern bir deneyim için PowerShell önerilir.

---

## Windows İçin Kablosuz ADB Bağlantısı

Bu yöntem sayesinde tabletiniz şarjdayken veya kucağınızdayken, Windows bilgisayarınız üzerinden veri çekmeye devam edebilirsiniz.

### 1. Adım: Cihazın Kapısını Açın (İlk Seferlik Kablo Şart)
Bilgisayarın tablete Wi-Fi üzerinden ulaşabilmesi için bir kereliğine kabloyla şu izni vermelisiniz:
1.  Tableti USB ile bilgisayara bağlayın.
2.  **PowerShell** penceresini açın ve şu komutu yazın:
    ```powershell
    adb tcpip 5555
    ```
    *(Ekranda "restarting in TCP mode port: 5555" yazısını görmelisiniz. Artık kabloyu çekebilirsiniz.)*

### 2. Adım: Tabletin Yerel IP Adresini Öğrenin
Tabletin bilgisayarınızla aynı Wi-Fi ağına bağlı olduğundan emin olun ve IP adresini bulun:
* **Ayarlar > Hakkında > Durum** kısmına bakın.
* Veya Wi-Fi ayarlarına girip bağlı olduğunuz ağın üzerine tıklayın.
* Genellikle şuna benzer: `192.168.1.XX` (Örneğin: `192.168.1.45`)

### 3. Adım: Wi-Fi Üzerinden Bağlanın
Kabloyu çıkardıktan sonra PowerShell'e şu komutu yazın:
```powershell
adb connect 192.168.1.45:5555
```
*(IP adresini kendi tabletinizin adresiyle değiştirmeyi unutmayın).*

### 4. Adım: Bağlantıyı Onaylayın
Her şeyin yolunda olduğunu anlamak için:
```powershell
adb devices
```
Yazdığınızda listede hem cihaz ID'sini hem de IP adresini `device` olarak görüyorsanız işlem tamamdır!

---

### Windows Kullanıcıları İçin Önemli Notlar:
* **Bağlantı Koparsa:** Windows bazen güç tasarrufu için Wi-Fi uykusuna geçebilir. Eğer bağlantı koparsa sadece `adb connect IP:5555` komutunu tekrar girmeniz yeterlidir (Kabloya tekrar gerek kalmaz).
* **Statik IP Tavsiyesi:** Eğer tabletinizin IP adresi sürekli değişiyorsa (modemden dolayı), modem arayüzünden tabletinize sabit bir IP tanımlamak bu işi kalıcı kılar.
* **Hata Alırsanız:** Eğer "Connection refused" (Bağlantı reddedildi) hatası alırsanız, `adb tcpip 5555` komutunun kablo takılıyken başarıyla verildiğinden emin olun.

---

# MACOS

## Adım 1: Android Cihazda "Hata Ayıklama" Modunu Açmak
Android cihazlar varsayılan olarak dışarıdan erişime kapalıdır. Bunu açmak için:

1.  **Ayarlar > Telefon Hakkında** (veya Tablet Hakkında) kısmına girin.
2.  **Derleme Numarası (Build Number)** yazısını bulun. (Bazı cihazlarda "Yazılım Bilgileri" altındadır).
3.  Bu yazının üzerine **7 kez art arda** tıklayın. Ekranda *"Artık bir geliştiricisiniz!"* yazısı çıkana kadar devam edin.
4.  Geri gelip **Sistem > Geliştirici Seçenekleri** menüsüne girin.
5.  **USB Hata Ayıklama (USB Debugging)** seçeneğini bulun ve **Açık** konuma getirin.

---

## Adım 2: Mac Üzerinde ADB Kurulumu
Mac'in Android cihazla konuşabilmesi için "Platform Tools" paketine ihtiyacı var.

1.  **Terminal'i açın** (Command + Space tuşuna basıp "Terminal" yazın).
2.  Eğer Mac'inizde **Homebrew** yüklü değilse şu komutu yapıştırıp kurun:
    ```bash
    /bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
    ```
3.  Şimdi ADB'yi kurun:
    ```bash
    brew install android-platform-tools
    ```

---

## Adım 3: Cihazı Mac'e Tanıtmak (El Sıkışma)
Bu adımda Mac ve telefon arasında güvenlik onayı vereceğiz.

1.  Android cihazınızı orijinal bir USB kablosuyla Mac'e bağlayın.
2.  Terminal'e şu komutu yazın:
    ```bash
    adb devices
    ```
3.  **Çok Önemli:** Bu komutu yazdıktan sonra **Android cihazınızın ekranına bakın**. Şöyle bir uyarı çıkacak: *"USB Hata Ayıklamasına izin verilsin mi?"*
4.  **"Bu bilgisayara her zaman izin ver"** kutucuğunu işaretleyin ve **Tamam** deyin.
5.  Terminal'de tekrar `adb devices` yazdığınızda cihazınızın isminin yanında `device` yazısını görmelisiniz. Şöyle görünmeli:
    > `List of devices attached`
    > `ZY22GXXXXX    device`


Esat, bu çok yerinde bir ekleme olur! Özellikle tabletle oyun oynarken kabloya bağlı kalmak (hele ki MacBook Air gibi az portu olan bir cihazda) çok can sıkıcı. Android'in **Wireless ADB** (Kablosuz ADB) özelliğini kullanarak bu işi tamamen özgürleştirebiliriz.

İşte rehberine eklemen gereken **"Kablosuz Bağlantı"** bölümü:

---

## Bonus: Kablosuz Bağlantı Kurulumu (Wireless ADB)

Tableti Mac'e kabloyla bağlamadan veri çekmek istiyorsanız bu adımları izleyin. **Not:** Cihazınız ve Mac'iniz aynı Wi-Fi ağına bağlı olmalıdır.

### 1. Adım: Başlangıç (Sadece bir kez kablo lazım)
Kablosuz modu aktif etmek için cihazı sadece bir seferlik kabloyla Mac'e bağlayın ve Terminal'e şu komutu yazın:
```bash
adb tcpip 5555
```
*(Bu komut cihazın içindeki "kablosuz kapıyı" açar. "restarting in TCP mode port: 5555" yazısını görmelisiniz.)*

### 2. Adım: Cihazın IP Adresini Bulun
Tabletin ayarlarına girin:
* **Ayarlar > Tablet Hakkında > Durum** (veya Wi-Fi Ayarları) kısmından cihazın **IP Adresini** bulun (Örn: `192.168.1.45`).

### 3. Adım: Kabloyu Çıkarın ve Bağlanın
Artık kabloyu çekebilirsiniz! Şu komutla kablosuz bağlantıyı kurun:
```bash
adb connect 192.168.1.45:5555
```
*(IP adresini kendi cihazınıza göre değiştirin. "connected to..." yazısını gördüğünüzde işlem tamamdır.)*

### 4. Adım: Doğrulama
Bağlantının kurulup kurulmadığını anlamak için şu komutu yazın:
```bash
adb devices
```
Listede cihazınızı `192.168.1.45:5555  device` şeklinde görüyorsanız artık her şey kablosuz akacaktır!

---

### Önemli Notlar:
* **Bağlantı Koparsa:** Eğer tabletin Wi-Fi bağlantısı koparsa veya tablet kapanıp açılırsa, `adb connect` komutunu tekrar yazmanız gerekebilir.
* **Cihaz Yeniden Başlarsa:** Android güvenlik kuralları gereği, cihazı yeniden başlatırsanız `adb tcpip 5555` komutunu bir kez daha kabloyla vermeniz gerekebilir. (Android 11 ve üzeri cihazlarda geliştirici seçeneklerinden "Kablosuz Hata Ayıklama"yı açarak kabloya hiç gerek duymadan da yapılabilir ama bu yöntem en garantisidir).

---

## Adım 4: Bağlantıyı Test Etme (Opsiyonel)
Dosya yolunun doğru olduğundan emin olmak için şu komutu deneyin:
```bash
adb shell ls /sdcard/Android/data/com.xd.TLglobal/files/UE4Game/UE_game/UE_game/Saved/Logs/
```
Eğer ekranda `UE_game.log` dosyasını görüyorsanız, bağlantı **kusursuz** demektir.

---

### Mac Kullanıcıları İçin Küçük İpuçları:
* **Kablo Seçimi:** Eğer `adb devices` yazdığınızda hiçbir şey çıkmıyorsa, kullandığınız kablo sadece "şarj kablosu" olabilir. "Veri transferi" destekleyen bir kablo kullandığınızdan emin olun.
* **M1/M2/M3 İşlemcili Macler:** Homebrew ve ADB bu işlemcilerde tamamen sorunsuz çalışır, ekstra bir işlem yapmanıza gerek yoktur.











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

## 3. Adım: TITrack Yazılımının Kurulumu

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

## 4. Adım: İlk Kurulum (Veritabanını Hazırlama)

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

## 5. Adım: Analizi Başlatma (Tail & Serve)

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

## 6. Adım: Son Ayarlar ve Kullanım

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



















































