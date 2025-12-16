🦷 CNN Tabanlı Diş Fırçası ve Macunu Sınıflandırma Projesi
Model Karşılaştırması: Transfer Learning (VGG16) ve Özel CNN Mimarileri
Bu projede, tamamen özgün olarak oluşturulmuş bir görüntü veri seti üzerinde Convolutional Neural Network (CNN) tabanlı sınıflandırma modelleri geliştirilmiş ve performansları karşılaştırılmıştır.

Projenin temel amacı; hazır State-of-the-Art (VGG16) mimarisi ile sıfırdan (from scratch) eğitilen modeller arasındaki performans farklarını gözlemlemek, veri artırımı (data augmentation) ve hiperparametre optimizasyonunun model başarısına etkisini analiz etmektir.

---

📁 Veri Seti
Kaynak: Veri seti tamamen proje kapsamında özgün olarak oluşturulmuştur. İnternetten hazır veri seti kullanılmamıştır.

Çekim: Görüntüler farklı açılardan ve zeminlerden çekilerek çeşitlilik sağlanmıştır.

Ön İşleme: Tüm görüntüler ham halinden 128x128 piksel boyutuna yeniden boyutlandırılmış ve dataset_128 klasöründe toplanmıştır. Daha sonra eğitim, doğrulama ve test setlerine ayrılmıştır.

Sınıflar
Diş Fırçası (dis_fircasi)

Diş Macunu (dis_macunu)

dataset_split/
├── train/
│   ├── dis_fircasi/
│   └── dis_macunu/
├── val/
│   ├── dis_fircasi/
│   └── dis_macunu/
└── test/
    ├── dis_fircasi/
    └── dis_macunu/

---

🧪 Model 1 – Transfer Learning (State-of-the-Art)
Bu aşamada, ImageNet yarışmasında kendini kanıtlamış VGG16 mimarisi kullanılmıştır.

Mimari: VGG16 (ImageNet ağırlıkları ile).

Strateji: include_top=False yapılarak sınıflandırıcı katmanları atılmış, konvolüsyon katmanları dondurulmuştur (Freeze).

Eklenen Katmanlar: Flatten + Dense(256) + Dropout(0.5) + Output(Softmax).

Amaç: Çok az veriyle bile, önceden öğrenilmiş özelliklerin (kenar, doku bilgisi) kullanılarak yüksek başarı elde edilmesi.

Notebook: Model1.ipynb

---

🧪 Model 2 – Basit CNN (Baseline Model)
Sıfırdan tasarlanan (from scratch), veri artırımı uygulanmamış temel modeldir.

Mimari: 2 Bloklu Konvolüsyon Yapısı (32 ve 64 filtre).

Veri İşleme: Sadece Rescale (1./255). Veri artırımı yoktur.

Eksiklik: Veri artırımı olmadığı için model ezberlemeye (overfitting) daha meyillidir ve genelleme yeteneği kısıtlıdır.

Notebook: Model2.ipynb

---

🧪 Model 3 – Geliştirilmiş CNN (Optimize Edilmiş)
Model 2'nin eksikliklerini gidermek ve performansı Transfer Learning seviyesine yaklaştırmak için tasarlanmıştır.

Geliştirmeler:

Derinlik Artışı: Katman sayısı artırılarak 3. Blok (128 Filtre) eklenmiştir.

Online Veri Artırımı (Data Augmentation): Rotation, Width Shift, Height Shift, Horizontal Flip teknikleri ile veri seti eğitim sırasında sanal olarak çoğaltılmıştır.

Kademeli Dropout: Modelin kararlılığını artırmak için katman aralarına 0.1, 0.2 ve 0.3 oranlarında dropout eklenmiştir.

Amaç: Veri setini zenginleştirerek ve mimariyi derinleştirerek modelin "ezberlemesini" değil "öğrenmesini" sağlamak.

Notebook: Model3.ipynb

---

<img width="754" height="400" alt="image" src="https://github.com/user-attachments/assets/16f8880c-65c3-4b99-95fb-72fc72eb95e3" />

---

Çıkarımlar
Transfer Learning'in Gücü: Küçük veri setlerinde VGG16 gibi güçlü modeller, sıfırdan eğitime göre çok daha hızlı ve yüksek sonuç vermektedir.

Veri Artırımının Önemi: Model 3'te uygulanan veri artırımı (Augmentation), modelin başarısını Model 2'ye göre belirgin şekilde artırmıştır.

Mimari Derinliği: Filtre sayısını 128'e çıkarmak ve katman eklemek, modelin nesne detaylarını daha iyi öğrenmesini sağlamıştır.

---

🛠 Kullanılan Teknolojiler
Dil: Python

Kütüphaneler: TensorFlow, Keras, NumPy, Matplotlib, PIL

Ortam: Google Colab (GPU Hızlandırma ile)

Görselleştirme: Matplotlib ile Accuracy/Loss grafikleri ve Confusion Matrix analizleri.

---

🎯 Proje Özeti
Bu çalışma; görüntü işleme projelerinde Transfer Learning kullanımının avantajlarını ve kendi tasarladığımız modellerde 
hiperparametre optimizasyonunun (özellikle veri artırımı ve dropout) ne kadar kritik olduğunu deneysel verilerle ortaya koymuştur.
