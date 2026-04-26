# TLTrack-Mobile-Bridge

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
