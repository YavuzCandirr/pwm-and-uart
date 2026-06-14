# STM32 PWM LED Animasyonları ve UART Haberleşme Altyapısı

Bu proje, STM32 mikrodenetleyicilerinde donanımsal **PWM (Darbe Genişlik Modülasyonu)** ve **Durum Makinesi (State Machine)** mimarisi kullanılarak geliştirilmiş çoklu aydınlatma modlarına sahip bir gömülü sistem uygulamasıdır. 

Sistem sadece donanımsal animasyonlar sunmakla kalmaz, aynı zamanda sistemin dış dünya ile haberleşmesi ve hata ayıklama (debugging) süreçleri için **UART (Seri Haberleşme)** altyapısını da hazır barındırır.

## 🌟 Öne Çıkan Mühendislik Yaklaşımları

* **Donanımsal PWM Kontrolü:** LED'lerin sadece dijital (1/0) olarak açılıp kapanması yerine, TIM1 (Timer 1) kullanılarak parlaklık seviyeleri hassas bir şekilde kontrol edilmiştir.
* **Durum Makinesi (State Machine):** Mum alevi animasyonu üç farklı faza (Fade-in, Flicker, Fade-out) bölünmüş ve sistemin her fazda donanımı farklı şekilde sürmesi sağlanmıştır.
* **Rastgelelik (Randomization) ile Organik Efekt:** Standart C kütüphanesindeki `rand()` fonksiyonu PWM sinyaline entegre edilerek mekanik olmayan, organik ve öngörülemez bir titreme (flicker) efekti elde edilmiştir.
* **UART ile Hata Ayıklama (Debug) Altyapısı:** Projeye `UART4` donanımı (9600 Baud, 8N1 standartlarında) entegre edilmiştir. Bu altyapı, ilerleyen aşamalarda anlık parlaklık değerlerini bilgisayara yazdırmak veya bilgisayardan komut alarak mod değiştirmek için tamamen hazırdır.

## ⚙️ Animasyon Modları ve Çalışma Mantığı

### Mod 0: Mum Alevi (3 Fazlı Animasyon)
* **Faz 0 (Fade In):** TIM1_CH1'e bağlı LED yavaş yavaş maksimum belirlenen parlaklığa (500) ulaşır.
* **Faz 1 (Flicker/Titreme):** LED'in parlaklığı 500 ile 1000 duty-cycle değerleri arasında rastgele değişerek, belirli bir süre boyunca rüzgardaki bir mum alevi gibi titrer.
* **Faz 2 (Fade Out):** LED'in parlaklığı kademeli olarak düşürülür. Tamamen söndüğünde döngü başa (Faz 0) sarar.

### Mod 1: Sabit Flaşör
* İkinci bir kanal olan TIM1_CH2 devreye girer. Sayaç mantığına bağlı olarak belirli bir süre yüksek parlaklıkta (900) yanar, ardından tamamen sönerek basit bir flaşör/çakar görevi görür.

## 📌 Donanım ve Pin Konfigürasyonu

* **Geliştirme Kartı:** STM32F3 Discovery (veya benzer STM32 donanımları)
* **PWM Çıkışı 1 (Mum Efekti):** `TIM1_CH1`
* **PWM Çıkışı 2 (Flaşör Efekti):** `TIM1_CH2`
* **Kullanıcı Butonu (Mod Değiştirici):** `PA0` (GPIO_Input)
* **Seri Haberleşme:** `UART4` (9600 bps, 8 Data Bits, No Parity, 1 Stop Bit)

