COVID-19 Hasta Tahmin Projesi
📌 Proje Açıklaması

Bu projede, COVID-19 veri seti kullanılarak bireylerin hastalık durumunun (özellikle hastalık şiddetinin) makine öğrenmesi algoritmaları ile tahmin edilmesi amaçlanmıştır. Proje sürecinde veri ön işleme, analiz, modelleme ve performans değerlendirme adımları uygulanmıştır.

📂 Veri Seti Tanıtımı

Projede kullanılan veri seti, COVID-19 semptomları ve hastalık şiddeti bilgilerini içermektedir.

Veri seti kaynağı: Kaggle
🔗 https://www.kaggle.com/

Veri setinde:

Ateş, öksürük, yorgunluk gibi semptomlar
Hastalık şiddeti (hafif / ağır)
gibi bilgiler bulunmaktadır.
⚙️ Veri Ön İşleme Adımları

Modelin doğru çalışabilmesi için aşağıdaki işlemler uygulanmıştır:

Veri seti pandas ile okunmuştur
Eksik ve hatalı verilerin önüne geçmek için sadece sayısal sütunlar seçilmiştir
Hedef değişken (hastalık durumu) ayrılmıştır
Eğitim ve test verisi olarak ikiye bölünmüştür (%80 - %20)
🤖 Kullanılan Algoritmaların Mantığı
1. Logistic Regression
Lineer bir sınıflandırma algoritmasıdır
Veriler arasındaki ilişkiyi doğrusal olarak öğrenir
Hızlı çalışır ancak karmaşık verilerde sınırlı kalabilir
2. Random Forest
Birden fazla karar ağacından oluşur
Her ağaç farklı veri parçalarıyla eğitilir
Daha yüksek doğruluk sağlar ve overfitting’i azaltır
📊 Model Performans Karşılaştırması

Projede iki model karşılaştırılmıştır:

Logistic Regression → Daha hızlı ama daha düşük doğruluk
Random Forest → Daha yavaş ama daha yüksek doğruluk

Değerlendirme metrikleri:

Accuracy (Doğruluk)
Precision
Recall
F1-Score

Sonuç olarak Random Forest modeli daha başarılı bulunmuştur.

📈 Sonuç ve Yorumlar
COVID-19 semptomları ile hastalık şiddeti arasında anlamlı ilişkiler tespit edilmiştir
Random Forest modeli daha güvenilir tahminler üretmiştir
Veri görselleştirme, verinin anlaşılmasını kolaylaştırmıştır

Genel olarak proje, makine öğrenmesi sürecinin temel adımlarını başarılı şekilde göstermektedir.

▶️ Kodların Nasıl Çalıştırılacağı

Projeyi çalıştırmak için aşağıdaki adımları izleyin:

Gerekli kütüphaneleri yükleyin:
pip install pandas matplotlib seaborn scikit-learn

Jupyter Notebook veya Google Colab ortamını açın
Veri setini projeye ekleyin (zip ise çıkartın)
Notebook dosyasını çalıştırın:
COVID_19_Hasta_Tahmini.ipynb

Tüm hücreleri sırayla çalıştırarak:
Veri analizi
Model eğitimi
Sonuçları görüntüleyebilirsiniz
