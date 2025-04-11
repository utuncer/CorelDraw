Anladım! Aşağıda, düzenlenmiş promptunu DeepSearch ve Think Mode için iki ayrı metne böleceğim. Her biri, SuperGrok’un ilgili modunda kullanılmak üzere optimize edildi. DeepSearch için kaynak tarama ve görsel bulma odaklı, Think Mode için analiz, tablo, metin ve şema tarifine odaklı talimatlar hazırladım. Orijinal metnin içeriğini korudum, sadece talimatları modlara göre ayırdım. İşte sonuç:

---

### **DeepSearch için Metin**

**TEKNOFEST Savaşan İHA Yarışması KTR Promptu (DeepSearch)**

TEKNOFEST 2025 Savaşan İHA Yarışması’na katılıyoruz ve şu anda Kritik Tasarım Raporu (KTR) yazım sürecindeyiz. Aşağıdaki bilgiler ışığında, KTR’nin “4. Otonom Görevler” başlığı altında “4.1 Otonom Kilitlenme” ve “4.2 Kamikaze Görevi” bölümlerini hazırlamak için destek istiyoruz.

**Malzeme Listemiz**:
- **Pil**: 6S 16000mAh Li-Po 40C
- **Motor**: T-Motor Antigravity MN5008 KV400
- **ESC**: T-Motor Alpha 40A / 60A FOC ESC
- **Görev Bilgisayarı**: NVIDIA Jetson Orin NX 16GB (görüntü işleme ve otonom görevler için)
- **Uçuş Bilgisayarı**: Pixhawk 6X Pro (denge ve motor kontrolü için, Jetson’dan komut alır)
- **Kamera**: Raspberry Pi HQ Kamera (Sony IMX477R, 12.3 MP, 720p@30FPS veya 60FPS, f/1.4 lens, düşük ışıkta yüksek performans, ileride lens değişebilir)
- **PM02D Power Module**, **UBEC 12A (3-14S)**, **PDB Board**
- **Pitot Tüpü**: Pixhawk PX4 Diferansiyel Pitot Tüpü
- **Telemetri**: RFD900x
- **GPS**: Here 3 GPS + RTK
- **Yer Kontrol İstasyonu**: Ubiquiti Rocket M5, Ubiquiti AirFiber 5XHD anten
- **Kumanda**: Radiolink AT10 II
- **Yer Kontrol Arayüzü**: Mission Planner

**Donanım Kullanımı**:
- Jetson Orin NX, tüm görüntü işleme (kilitlenme, QR okuma) ve otonom görevleri (yol planlama, kontrol) yönetir. Linux ortamında kodlarımız çalışır.
- Pixhawk 6X Pro, yalnızca İHA’nın dengesini sağlamak ve motorlara güç vermek için kullanılır. Jetson’dan MAVLink protokolüyle komut alır (örn. motor güç ayarı, duruş kontrolü).
- Kamera, 720p@30FPS’te çalışacak (hızlı hareket için 60FPS’e çıkabilir), f/1.4 lens ile geniş görüş alanı ve düşük ışık performansı sağlar.

**Şartname Özeti** (Savaşan İHA ve Kamikaze Görevleri):
- **Savaşan İHA**:
  - İHA’lar birbirini görüntü üzerinden kilitlemeye çalışır. Kamera, İHA burnunda sabit, monoküler (stereo/360° yasaktır). Ek sensör (lidar, radar) kullanılamaz.
  - Kilitlenme: Kamera görüntüsünün merkezinde rakip İHA, 4 saniye kesintisiz tutulmalı (%5 ekran kaplama, yatay/dikey eksenlerin %5’ini kapsamalı). 1 saniye tolerans ve %5 frame kaybı (200ms) kabul edilir.
  - Otonom kilitlenme, kullanıcı müdahalesi olmadan yapılır. Manuel kilitlenme için kullanıcı İHA’yı kontrol edebilir, ancak tespit otomatik olmalı.
  - Kilitlenme dörtgeni kırmızı (#FF0000), hedefin %90’ını kapsamalı, merkezler arası mesafe sınırlı. Video, 640x480 minimum, OpenCV 4.5 ve FFPLAY ile oynatılabilir olmalı.
  - Sunucuya kilitlenme paketi (JSON formatı) 2 saniye içinde gönderilir. Yanlış kilitlenme eksi puan getirir.
- **Kamikaze Görevi**:
  - Sabit yer hedefine (2.5m x 2.5m QR kodu) otonom intikal, dalış ve pas geçme. Fiziksel çarpma yok.
  - QR kodu okunur, metin sunucuya 2 saniye içinde gönderilir. Kamera, kilitlenme kamerasıyla aynı olmalı.
  - Dalış irtifası limit altına inerse alan dışı sayılır. Kaza kırımda puanlama otonom kilit için geçerli, ancak iniş tamamlanmazsa kamikaze puanı alınmaz.
- **Haberleşme**:
  - Yer istasyonu, yarışma sunucusuyla ethernet üzerinden JSON API ile iletişim kurar (1-5 Hz telemetri, kilitlenme/kamikaze paketleri).
  - Telemetri: GPS, irtifa, duruş, batarya, otonom/kilitlenme durumu. Sunucu saatiyle senkronize.
  - Hava savunma sistemleri (HSS): Sanal kırmızı daireler, koordinatlar sunucudan alınır, kaçınma algoritması gerekir.
- **Video Gereksinimleri**:
  - Canlı aktarım (isteğe bağlı): 640x480, 15 FPS, sabit frame rate.
  - Değerlendirme videosu: 640x480, 15 FPS, sunucu saati sağ üstte, kilitlenme dörtgeni kırmızı, H264/MP4 formatı, Türkçe/özel karakter olmadan isimlendirme.

**KTR Gereksinimleri**:
KTR’de “4. Otonom Görevler” başlığı en yüksek puanı sağlar, bu yüzden detaylı, teknik ve profesyonel olmalı. Aşağıdaki alt başlıklar için destek istiyoruz:

**4.1 Otonom Kilitlenme**:
- Sunucudan gelen rakip İHA verilerinin (konum, irtifa) nasıl değerlendirileceği, hangi İHA’nın takip edileceğine nasıl karar verileceği.
- Yaklaşma stratejisi: Hedefe nasıl yaklaşılacak, hangi yol izlenecek (örn. agresif manevralar, engel kaçınma).
- Nesne tespit ve takip algoritması: YOLOv11n (Nano) kullanılacak. Neden seçildi, nasıl geliştirildi (5000 görüntülük veri setiyle eğitim). Alternatif algoritmalarla (SSD, DeepSORT) karşılaştırma, avantaj/dezavantajlar, resmi kaynaklara dayalı tablo.
- Kontrol algoritması: PID kullanılacak. Görüntüdeki hataya göre duruş (yatış, dikilme, yönelme) ve hız kontrolü nasıl yapılacak. Alternatiflerle (MPC, Nonlinear) karşılaştırma, avantaj/dezavantajlar.
- Görsel destek: Sade akış diyagramları (YOLOv11n tespiti, PID kontrolü), algoritma anlatımını güçlendirecek.

**4.2 Kamikaze Görevi**:
- İHA’nın intikal, dalış ve pas geçme fazları: Dalış açısı (örn. 30-45°), hız (20-25 m/s), rüzgar etkisi, irtifa (10-50m), yapısal dayanım (5G limiti).
- QR kodu tespiti: ZBar kullanılacak. Tespit algoritması, görüntü bozulmalarına karşı önlemler, 10-15m irtifadan okuma performansı.
- Navigasyon: RRT ile HSS’lerden kaçınma ve QR hedefine intikal. Alternatiflerle (A*) karşılaştırma.
- Kontrol: PID ile dalış açısı ve hız kontrolü. Alternatiflerle (Nonlinear) karşılaştırma.
- Görsel destek: Sade akış diyagramları (ZBar QR okuma, RRT navigasyonu), algoritma anlatımını güçlendirecek.

**Algoritma Detayları**:
- **YOLOv11n (Nano)**: Hız odaklı, Jetson Orin NX’te düşük gecikme. 5000 görüntülük veri setiyle eğitilecek.
- **PID**: Jetson’da hesaplanıp Pixhawk’a MAVLink ile aktarılacak. Basit, düşük gecikmeli.
- **ZBar**: QR kodu için hafif ve robust. OpenCV ile entegre.
- **RRT**: HSS kaçınma ve QR hedefine navigasyon için esnek.
- Alternatifler (SSD, DeepSORT, MPC, Nonlinear, A*) neden tercih edilmedi, tabloyla karşılaştırma.

**Tablo Formatı**:
- Kriterler: Doğruluk, FPS (Jetson’da), hesaplama maliyeti, Jetson uyumluluğu, şartname gereksinimlerine uygunluk.
- Resmi kaynaklardan (IEEE, Ultralytics, PX4, GitHub) veriyle desteklenmeli.

**DeepSearch Talimatları**:
- En detaylı modda çalışarak aşağıdaki görevleri yerine getir:
  - **Kaynak Bulma**: YOLOv11n (Nano), ZBar, RRT ve PID için resmi kaynaklar tara: IEEE Xplore, Ultralytics (https://docs.ultralytics.com/), OpenCV (https://docs.opencv.org/), PX4 dokümantasyonları (https://docs.px4.io/), GitHub projeleri, arXiv veya diğer akademik makaleler.
  - **Görsel Örnekler**: Şartnamedeki gereksinimlere uygun sade akış diyagramı örnekleri bul (YOLOv11n için nesne tespiti, PID için kontrol şeması, ZBar için QR okuma, RRT için navigasyon). Draw.io, Lucidchart veya benzer araçlarda çizilebilir örnekler öner.
  - **Donanım Uyumluluğu**: Raspberry Pi HQ Kamera (f/1.4 lens, 720p@30FPS) ve Jetson Orin NX ile uyumlu algoritma implementasyonları için kaynaklar bul (örn. YOLOv11n’nin Jetson optimizasyonu, ZBar’ın OpenCV entegrasyonu).

---

### **Think Mode için Metin**

**TEKNOFEST Savaşan İHA Yarışması KTR Promptu (Think Mode)**

TEKNOFEST 2025 Savaşan İHA Yarışması’na katılıyoruz ve şu anda Kritik Tasarım Raporu (KTR) yazım sürecindeyiz. Aşağıdaki bilgiler ışığında, KTR’nin “4. Otonom Görevler” başlığı altında “4.1 Otonom Kilitlenme” ve “4.2 Kamikaze Görevi” bölümlerini hazırlamak için destek istiyoruz.

**Malzeme Listemiz**:
- **Pil**: 6S 16000mAh Li-Po 40C
- **Motor**: T-Motor Antigravity MN5008 KV400
- **ESC**: T-Motor Alpha 40A / 60A FOC ESC
- **Görev Bilgisayarı**: NVIDIA Jetson Orin NX 16GB (görüntü işleme ve otonom görevler için)
- **Uçuş Bilgisayarı**: Pixhawk 6X Pro (denge ve motor kontrolü için, Jetson’dan komut alır)
- **Kamera**: Raspberry Pi HQ Kamera (Sony IMX477R, 12.3 MP, 720p@30FPS veya 60FPS, f/1.4 lens, düşük ışıkta yüksek performans, ileride lens değişebilir)
- **PM02D Power Module**, **UBEC 12A (3-14S)**, **PDB Board**
- **Pitot Tüpü**: Pixhawk PX4 Diferansiyel Pitot Tüpü
- **Telemetri**: RFD900x
- **GPS**: Here 3 GPS + RTK
- **Yer Kontrol İstasyonu**: Ubiquiti Rocket M5, Ubiquiti AirFiber 5XHD anten
- **Kumanda**: Radiolink AT10 II
- **Yer Kontrol Arayüzü**: Mission Planner

**Donanım Kullanımı**:
- Jetson Orin NX, tüm görüntü işleme (kilitlenme, QR okuma) ve otonom görevleri (yol planlama, kontrol) yönetir. Linux ortamında kodlarımız çalışır.
- Pixhawk 6X Pro, yalnızca İHA’nın dengesini sağlamak ve motorlara güç vermek için kullanılır. Jetson’dan MAVLink protokolüyle komut alır (örn. motor güç ayarı, duruş kontrolü).
- Kamera, 720p@30FPS’te çalışacak (hızlı hareket için 60FPS’e çıkabilir), f/1.4 lens ile geniş görüş alanı ve düşük ışık performansı sağlar.

**Şartname Özeti** (Savaşan İHA ve Kamikaze Görevleri):
- **Savaşan İHA**:
  - İHA’lar birbirini görüntü üzerinden kilitlemeye çalışır. Kamera, İHA burnunda sabit, monoküler (stereo/360° yasaktır). Ek sensör (lidar, radar) kullanılamaz.
  - Kilitlenme: Kamera görüntüsünün merkezinde rakip İHA, 4 saniye kesintisiz tutulmalı (%5 ekran kaplama, yatay/dikey eksenlerin %5’ini kapsamalı). 1 saniye tolerans ve %5 frame kaybı (200ms) kabul edilir.
  - Otonom kilitlenme, kullanıcı müdahalesi olmadan yapılır. Manuel kilitlenme için kullanıcı İHA’yı kontrol edebilir, ancak tespit otomatik olmalı.
  - Kilitlenme dörtgeni kırmızı (#FF0000), hedefin %90’ını kapsamalı, merkezler arası mesafe sınırlı. Video, 640x480 minimum, OpenCV 4.5 ve FFPLAY ile oynatılabilir olmalı.
  - Sunucuya kilitlenme paketi (JSON formatı) 2 saniye içinde gönderilir. Yanlış kilitlenme eksi puan getirir.
- **Kamikaze Görevi**:
  - Sabit yer hedefine (2.5m x 2.5m QR kodu) otonom intikal, dalış ve pas geçme. Fiziksel çarpma yok.
  - QR kodu okunur, metin sunucuya 2 saniye içinde gönderilir. Kamera, kilitlenme kamerasıyla aynı olmalı.
  - Dalış irtifası limit altına inerse alan dışı sayılır. Kaza kırımda puanlama otonom kilit için geçerli, ancak iniş tamamlanmazsa kamikaze puanı alınmaz.
- **Haberleşme**:
  - Yer istasyonu, yarışma sunucusuyla ethernet üzerinden JSON API ile iletişim kurar (1-5 Hz telemetri, kilitlenme/kamikaze paketleri).
  - Telemetri: GPS, irtifa, duruş, batarya, otonom/kilitlenme durumu. Sunucu saatiyle senkronize.
  - Hava savunma sistemleri (HSS): Sanal kırmızı daireler, koordinatlar sunucudan alınır, kaçınma algoritması gerekir.
- **Video Gereksinimleri**:
  - Canlı aktarım (isteğe bağlı): 640x480, 15 FPS, sabit frame rate.
  - Değerlendirme videosu: 640x480, 15 FPS, sunucu saati sağ üstte, kilitlenme dörtgeni kırmızı, H264/MP4 formatı, Türkçe/özel karakter olmadan isimlendirme.

**KTR Gereksinimleri**:
KTR’de “4. Otonom Görevler” başlığı en yüksek puanı sağlar, bu yüzden detaylı, teknik ve profesyonel olmalı. Aşağıdaki alt başlıklar için destek istiyoruz:

**4.1 Otonom Kilitlenme**:
- Sunucudan gelen rakip İHA verilerinin (konum, irtifa) nasıl değerlendirileceği, hangi İHA’nın takip edileceğine nasıl karar verileceği.
- Yaklaşma stratejisi: Hedefe nasıl yaklaşılacak, hangi yol izlenecek (örn. agresif manevralar, engel kaçınma).
- Nesne tespit ve takip algoritması: YOLOv11n (Nano) kullanılacak. Neden seçildi, nasıl geliştirildi (5000 görüntülük veri setiyle eğitim). Alternatif algoritmalarla (SSD, DeepSORT) karşılaştırma, avantaj/dezavantajlar, resmi kaynaklara dayalı tablo.
- Kontrol algoritması: PID kullanılacak. Görüntüdeki hataya göre duruş (yatış, dikilme, yönelme) ve hız kontrolü nasıl yapılacak. Alternatiflerle (MPC, Nonlinear) karşılaştırma, avantaj/dezavantajlar.
- Görsel destek: Sade akış diyagramları (YOLOv11n tespiti, PID kontrolü), algoritma anlatımını güçlendirecek.

**4.2 Kamikaze Görevi**:
- İHA’nın intikal, dalış ve pas geçme fazları: Dalış açısı (örn. 30-45°), hız (20-25 m/s), rüzgar etkisi, irtifa (10-50m), yapısal dayanım (5G limiti).
- QR kodu tespiti: ZBar kullanılacak. Tespit algoritması, görüntü bozulmalarına karşı önlemler, 10-15m irtifadan okuma performansı.
- Navigasyon: RRT ile HSS’lerden kaçınma ve QR hedefine intikal. Alternatiflerle (A*) karşılaştırma.
- Kontrol: PID ile dalış açısı ve hız kontrolü. Alternatiflerle (Nonlinear) karşılaştırma.
- Görsel destek: Sade akış diyagramları (ZBar QR okuma, RRT navigasyonu), algoritma anlatımını güçlendirecek.

**Algoritma Detayları**:
- **YOLOv11n (Nano)**: Hız odaklı, Jetson Orin NX’te düşük gecikme. 5000 görüntülük veri setiyle eğitilecek.
- **PID**: Jetson’da hesaplanıp Pixhawk’a MAVLink ile aktarılacak. Basit, düşük gecikmeli.
- **ZBar**: QR kodu için hafif ve robust. OpenCV ile entegre.
- **RRT**: HSS kaçınma ve QR hedefine navigasyon için esnek.
- Alternatifler (SSD, DeepSORT, MPC, Nonlinear, A*) neden tercih edilmedi, tabloyla karşılaştırma.

**Tablo Formatı**:
- Kriterler: Doğruluk, FPS (Jetson’da), hesaplama maliyeti, Jetson uyumluluğu, şartname gereksinimlerine uygunluk.
- Resmi kaynaklardan (IEEE, Ultralytics, PX4, GitHub) veriyle desteklenmeli.

**Rapor Metni**:
- Her başlık (4.1 ve 4.2) için 300-400 kelime, detaylı, teknik, profesyonel.
- YOLOv11n, PID, ZBar, RRT’nin seçilme nedenleri, alternatiflerle karşılaştırma, şartname uyumu.
- Görsel: Sade akış diyagramları (örn. YOLOv11n için tespit, PID için kontrol, ZBar için QR okuma, RRT için navigasyon).

**Think Mode Talimatları**:
- Aşağıdaki görevleri yerine getir:
  - **Algoritma Analizi ve Karşılaştırma**: 
    - YOLOv11n (Nano)’yi SSD ve DeepSORT ile karşılaştır. Kriterler: doğruluk, FPS (Jetson Orin NX’te, 720p@30FPS), hesaplama maliyeti, şartname uyumu (4 saniye kilit, %5 ekran kaplama).
    - PID’yi MPC ve Nonlinear kontrol ile karşılaştır. Kriterler: doğruluk, tepki süresi, Jetson-Pixhawk entegrasyonu, şartname uyumu (otonom mod).
    - ZBar’ı ArUco ile karşılaştır. Kriterler: doğruluk, FPS, görüntü bozulmalarına robustluk, 10-15m irtifadan QR okuma.
    - RRT’yi A* ile karşılaştır. Kriterler: esneklik, hesaplama maliyeti, HSS kaçınma performansı.
    - Karşılaştırmaları resmi kaynaklara (IEEE, Ultralytics, PX4, GitHub) dayandırarak tablo üret.
  - **Rapor Metni**: 
    - 4.1 (Otonom Kilitlenme) için 300-400 kelimelik detaylı, teknik, TEKNOFEST KTR formatına uygun metin yaz. YOLOv11n ve PID’nin seçim nedenlerini, şartname uyumunu (4 saniye kilit, %5 tolerans, otonom mod), alternatiflerin dezavantajlarını vurgula.
    - 4.2 (Kamikaze Görevi) için 300-400 kelimelik detaylı, teknik metin yaz. ZBar ve RRT’nin seçim nedenlerini, PID ile dalış kontrolünü, şartname uyumunu (QR okuma, HSS kaçınma, 5G limiti) ve alternatiflerin dezavantajlarını açıkla.
  - **Akış Diyagramları**: 
    - YOLOv11n için sade akış diyagramı tarif et (kamera görüntüsü → tespit → kilitlenme dörtgeni).
    - PID için sade kontrol şeması tarif et (hedef hatası → Jetson PID → Pixhawk komutları).
    - ZBar için sade akış diyagramı tarif et (kamera görüntüsü → QR tespiti → sunucuya gönderim).
    - RRT için sade navigasyon şeması tarif et (İHA konumu → HSS kaçınma → QR hedefi).
    - Diyagramlar, Draw.io veya Lucidchart’ta çizilebilir şekilde basit, şartnameye uygun (örn. kırmızı kilitlenme dörtgeni) olsun.

---

### **Açıklamalar**

1. **DeepSearch Metni**:
   - **Odak**: Kaynak tarama ve görsel bulma. YOLOv11n, ZBar, RRT, PID için akademik ve resmi kaynaklar (makale, dokümantasyon, GitHub) ile akış diyagramı örnekleri toplamak için optimize edildi.
   - **Neden En Detaylı?**: Teknik bir yarışma için derin akademik tarama gerekiyor. IEEE, Ultralytics, PX4 gibi kaynaklar geniş bir veri tabanından çekilmeli.
   - **Kullanım**: UI’de DeepSearch’ü seç, “en detaylı” modunu aktif et, bu metni kopyalayıp yapıştır.

2. **Think Mode Metni**:
   - **Odak**: Algoritma analizi, karşılaştırma tablosu, rapor metni ve şema tarifleri. YOLOv11n, PID, ZBar, RRT’nin şartnameye uyumu ve alternatiflerle karşılaştırması için analitik düşünme gerekiyor.
   - **Neden Think Mode?**: Yaratıcı ve teknik çıktılar (metin, tablo, şema) için ideal. Şartname detaylarını derinlemesine değerlendirir.
   - **Kullanım**: UI’de “Think” butonunu aktif et, bu metni kopyalayıp yapıştır.

3. **Farklar**:
   - DeepSearch metni, dış kaynaklara odaklanıyor (web, akademik veri tabanları, GitHub). Think Mode metni, bu kaynakları analiz edip KTR’ye uygun içerik üretiyor.
   - Metinler, aynı bağlamı (malzeme, şartname, algoritmalar) içeriyor, ama talimatlar modlara göre ayrıldı.

4. **Pratik İpucu**:
   - Her metni ayrı ayrı kopyalayıp SuperGrok UI’sine yapıştır. DeepSearch’ten gelen kaynakları Think Mode’da kullanmak istersen, DeepSearch sonuçlarını not al ve Think Mode promptuna ekle (örn. “Şu kaynakları analiz et: [liste]”).
   - Metinler uzun, ama SuperGrok’un bağlam limiti içinde kalır. Eğer UI’de karakter sınırı sorunu çıkarsa, talimatları kısaltabiliriz.

---

### **Sonraki Adımlar**

- **Onay**: Bu metinleri onaylıyorsan, başka bir sohbette orijinal taleplerine (algoritma önerileri, kaynaklar, metin, şema) yanıt vermeye başlayabilirim. Onay için “Tamam, bunları kullanacağım” gibi bir işaret verirsen, diğer sohbete geçeriz.
- **Düzenleme**: Eğer metinlerde ekleme/çıkarma istiyorsan (örn. daha kısa talimat, ek kriter), söyleyebilirsin. Mesela, “DeepSearch’te sadece IEEE ve Ultralytics tara” gibi.
- **Başka Sohbet**: Orijinal uzun metni başka bir sohbette kullanmak için, yukarıdaki metinlerden birini (veya ikisini birleştirip) temel alabilirsin. Talimatları oraya kopyalayıp projeye devam edebiliriz.

Ne dersin, metinler böyle uygun mu? Başka bir şey eklemek ister misin?
