COVID-19 Hasta Tahmin Projesi
📌 Proje Açıklaması

Bu projede, COVID-19 veri seti kullanılarak bireylerin hastalık durumunun (özellikle hastalık şiddetinin) makine öğrenmesi algoritmaları ile tahmin edilmesi amaçlanmıştır. Proje sürecinde veri ön işleme, analiz, modelleme ve performans değerlendirme adımları uygulanmıştır.

📂 Veri Seti Tanıtımı

Projede kullanılan veri seti, COVID-19 semptomları ve hastalık şiddeti bilgilerini içermektedir.

Veri seti kaynağı: Kaggle
Link: https://www.kaggle.com/

Veri setinde:

Ateş, öksürük, yorgunluk gibi semptomlar
Hastalık şiddeti (hafif / ağır)
⚙️ Veri Ön İşleme Adımları
Veri seti pandas ile okunmuştur
Sadece sayısal sütunlar seçilmiştir
Hedef değişken ayrılmıştır
Veri %80 eğitim, %20 test olarak bölünmüştür
🤖 Kullanılan Algoritmaların Mantığı
Logistic Regression
Lineer sınıflandırma yapar
Hızlıdır
Basit veri yapılarında etkilidir
Random Forest
Birden fazla karar ağacından oluşur
Daha yüksek doğruluk sağlar
Overfitting riskini azaltır
📊 Model Performans Karşılaştırması
Logistic Regression → Orta seviye doğruluk
Random Forest → Daha yüksek doğruluk

Kullanılan metrikler:

Accuracy
Precision
Recall
F1-Score
📈 Sonuç ve Yorumlar
Random Forest modeli daha başarılı sonuç vermiştir
Semptomlar ile hastalık şiddeti arasında ilişki bulunmuştur
Veri görselleştirme analiz sürecini kolaylaştırmıştır
▶️ Kodların Nasıl Çalıştırılacağı
Kütüphaneleri yükle:
pip install pandas matplotlib seaborn scikit-learn

Notebook'u aç:
COVID_19_Hasta_Tahmini.ipynb

Tüm hücreleri sırayla çalıştır
