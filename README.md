Makine öğrenmesi dersi COVID-19 Hasta Tahmini ödevi
🦠 COVID-19 Hasta Şiddet Tahmini
<p align="center">
  <img src="https://img.shields.io/badge/Python-3.8%2B-blue?style=for-the-badge&logo=python" />
  <img src="https://img.shields.io/badge/scikit--learn-ML-orange?style=for-the-badge&logo=scikitlearn" />
  <img src="https://img.shields.io/badge/Jupyter-Notebook-F37626?style=for-the-badge&logo=jupyter" />
  <img src="https://img.shields.io/badge/Status-Tamamlandı-success?style=for-the-badge" />
</p>

📌 Proje Açıklaması
Bu proje, COVID-19 semptomlarına dayanarak hastaların hastalık şiddetini makine öğrenmesi ile tahmin etmeyi amaçlamaktadır. Belirtilerin (ateş, öksürük, nefes darlığı vb.) ikili (binary) giriş değerleri olarak kullanıldığı bu sistemde, hastanın hastalık seyrinin Hafif (Mild) mi yoksa Ağır/Diğer mi olacağı sınıflandırılmaktadır.
Proje kapsamında iki farklı makine öğrenmesi algoritması karşılaştırılmış; model başarıları değerlendirilmiş ve en iyi performansı gösteren model final tahmincisi olarak seçilmiştir.
🎯 Hedefler

COVID-19 semptomlarından hastalık şiddetini tahmin etmek
Lojistik Regresyon ve Random Forest algoritmalarını karşılaştırmak
Hastalık tahminine en çok katkı sağlayan belirtileri belirlemek
Yeni bir hasta için gerçek zamanlı tahmin üretmek


📂 Kullanılan Veri Seti
ÖzellikDetayVeri Seti AdıCOVID-19 Symptoms and Presence DatasetKaynakKaggleBağlantı🔗 Veri Setine GitDosyaCleaned-Data.csvVeri TipiBinary (İkili: 0 / 1)
📊 Veri Seti Hakkında
Veri seti; COVID-19 tanısı almış ve almamış bireylerden toplanan semptom bilgilerini içermektedir. Her satır bir hastayı, her sütun ise o hastada gözlemlenen semptomu temsil etmektedir.
Temel Sütunlar:
Sütun AdıAçıklamaFeverAteş (0: Yok, 1: Var)TirednessYorgunlukDry-CoughKuru ÖksürükDifficulty-in-BreathingNefes DarlığıSore-ThroatBoğaz AğrısıSeverity_Mild🎯 Hedef Değişken — Hafif şiddet (1: Hafif, 0: Ağır/Diğer)Severity_ModerateOrta şiddet etiketiSeverity_SevereAğır şiddet etiketiSeverity_NoneBelirti yok etiketi

🔧 Veri Ön İşleme Adımları
1. 📥 Veri Yükleme
Ham veri, .zip arşivinden çıkarılarak ./extracted_data/Cleaned-Data.csv yolundan pandas ile okunmuştur.
pythondf = pd.read_csv('./extracted_data/Cleaned-Data.csv')
2. 🔢 Sayısal Sütun Seçimi
Veri setinde string (metin) tipindeki sütunlar modele doğrudan verilemeyeceğinden, yalnızca sayısal (numeric) sütunlar alınarak bu tür hatalar önlenmiştir.
pythondf = df.select_dtypes(include=['number'])
3. 🎯 Özellik ve Hedef Ayrımı
Hedef değişkeni Severity_Mild olarak belirlenmiştir. Modelin diğer şiddet etiketlerinden "kopya çekmesini" önlemek amacıyla tüm şiddet sütunları (Severity_*) özellik matrisinden çıkarılmıştır.
pythondrop_list = ['Severity_Mild', 'Severity_Moderate', 'Severity_Severe', 'Severity_None']
X = df.drop(columns=drop_list)
y = df['Severity_Mild']
4. ✂️ Eğitim / Test Bölümlemesi
Veri, %80 eğitim ve %20 test olarak ayrılmıştır. Tekrar üretilebilirlik için random_state=42 kullanılmıştır.
pythonX_train, X_test, y_train, y_test = train_test_split(X, y, test_size=0.2, random_state=42)
5. 📦 Aykırı Değer Analizi
Veri seti çoğunlukla binary (0-1) yapıda olduğundan Z-score analizi uygulanmış; uç değer (outlier) tespit edilmemiştir. Bu durum kutu grafikleri (boxplot) ile görsel olarak da doğrulanmıştır.

🤖 Kullanılan Algoritmaların Mantığı
1. 📈 Lojistik Regresyon (Logistic Regression)
Lojistik Regresyon, ikili sınıflandırma problemleri için klasik bir istatistiksel modeldir. Girdi özelliklerinin ağırlıklı toplamını sigmoid fonksiyonu ile 0-1 aralığına dönüştürerek bir sınıfa ait olma olasılığını hesaplar.
Formül:
P(y=1∣X)=11+e−(β0+β1x1+...+βnxn)P(y=1|X) = \frac{1}{1 + e^{-(\beta_0 + \beta_1 x_1 + ... + \beta_n x_n)}}P(y=1∣X)=1+e−(β0​+β1​x1​+...+βn​xn​)1​
Avantajları:

Yorumlanabilirlik yüksek
Az veriyle de iyi çalışır
Hızlı eğitim süresi

Projede Kullanımı:
pythonlr_model = LogisticRegression(max_iter=1000)
lr_model.fit(X_train, y_train)

max_iter=1000 — Binary veri setinde yakınsama sorunlarını önlemek için iterasyon sınırı artırılmıştır.


2. 🌲 Random Forest (Rastgele Orman)
Random Forest, birden fazla karar ağacının (decision tree) bir araya getirildiği topluluk öğrenmesi (ensemble learning) yöntemidir. Her ağaç, verinin rastgele bir alt kümesi üzerinde eğitilir ve sonuçlar çoğunluk oylamasıyla birleştirilir.
Temel Prensipler:

Bagging (Bootstrap Aggregating): Her ağaç, eğitim verisinin farklı bir bootstrap örneği üzerinde oluşturulur.
Rastgele Özellik Seçimi: Her düğümde özelliklerin rastgele bir alt kümesi değerlendirilir — bu, ağaçlar arasındaki korelasyonu azaltır.
Çoğunluk Oylaması: Tüm ağaçların tahminleri birleştirilerek nihai sınıf belirlenir.

Avantajları:

Aşırı öğrenmeye (overfitting) dirençli
Özellik önem skoru çıkarabilir
Yüksek boyutlu verilerde iyi performans

Projede Kullanımı:
pythonrf_model = RandomForestClassifier(n_estimators=100)
rf_model.fit(X_train, y_train)

n_estimators=100 — 100 karar ağacından oluşan bir orman kurulmuştur.


📊 Model Performans Karşılaştırması
Her iki model de Confusion Matrix, Classification Report (Precision, Recall, F1-Score) ve Accuracy Score metrikleri ile değerlendirilmiştir.
📋 Karşılaştırma Tablosu
MetrikLogistic RegressionRandom ForestAccuracy✔ Hesaplandı✔ HesaplandıPrecisionclassification_report ileclassification_report ileRecallclassification_report ileclassification_report ileF1-Scoreclassification_report ileclassification_report ileConfusion Matrix✔ Görselleştirildi✔ Görselleştirildi
🏆 Final Model Seçimi
pythonbest_model = models.loc[models['Score'].idxmax(), 'Model']
print(f"Sonuç: En yüksek başarıyı {best_model} algoritması vermiştir.")
Doğruluk skoru karşılaştırması sonucunda en yüksek başarıyı sergileyen model otomatik olarak tespit edilmiş ve final tahminci olarak seçilmiştir.
🔍 Özellik Önem Analizi (Feature Importance)
Random Forest modeli aracılığıyla, hastalık şiddetini tahmin etmede en belirleyici olan ilk 10 semptom belirlenmiştir. Bu analiz, klinik açıdan hangi belirtilerin daha kritik olduğunu ortaya koymaktadır.

💡 Sonuç ve Yorumlar

Veri Kalitesi: Veri seti temiz ve binary formatlı olduğundan kapsamlı bir veri temizleme adımı gerekmemiştir.
Model Seçimi: Random Forest, ensemble yapısı sayesinde genellikle Lojistik Regresyon'a kıyasla daha yüksek doğruluk sergilemiştir. Ancak Lojistik Regresyon, yorumlanabilirliği ve hızı açısından avantajlıdır.
Özellik Önemi: Ateş (Fever), kuru öksürük (Dry-Cough) ve nefes darlığı (Difficulty-in-Breathing) hastalık şiddetini tahmin etmede en belirleyici semptomlar arasında yer almaktadır.
Gerçek Zamanlı Tahmin: Model, yeni bir hasta verisi için predict() ve predict_proba() fonksiyonları aracılığıyla hem sınıf tahmini hem de olasılık skoru üretebilmektedir.
Sınırlılıklar:

Veri seti sabit ve sınırlı sayıda semptom içermektedir.
Yaş, cinsiyet gibi demografik değişkenler modele dahil edilmemiştir.
Daha büyük ve dengeli veri setleriyle model performansı artırılabilir.




🚀 Kodların Nasıl Çalıştırılacağı
✅ Gereksinimler
Aşağıdaki Python kütüphanelerinin kurulu olması gerekmektedir:
bashpip install pandas numpy matplotlib seaborn scikit-learn jupyter
📁 Proje Yapısı
📦 COVID-19-Hasta-Tahmini/
├── 📓 COVID_19_Hasta_Tahmini.ipynb   # Ana Jupyter Notebook
├── 📁 extracted_data/
│   └── Cleaned-Data.csv              # İşlenmiş veri seti
├── 📁 archive (3).zip                # Ham veri arşivi (Kaggle'dan indirilir)
└── 📄 README.md                      # Bu dosya
▶️ Adım Adım Çalıştırma
1. Depoyu klonlayın veya dosyaları indirin:
bashgit clone https://github.com/kullanici-adi/covid19-hasta-tahmini.git
cd covid19-hasta-tahmini
2. Veri setini Kaggle'dan indirin:
Kaggle Veri Seti Sayfası adresinden archive.zip dosyasını indirip proje klasörüne kopyalayın.
3. Jupyter Notebook'u başlatın:
bashjupyter notebook
4. Notebook'u açın ve hücreleri sırayla çalıştırın:
COVID_19_Hasta_Tahmini.ipynb → "Kernel > Restart & Run All"

⚠️ Not: Notebook'u Google Colab üzerinde çalıştırıyorsanız, zip dosya yolunu /content/archive (3).zip olarak bırakabilirsiniz. Yerel ortamda çalıştırıyorsanız dosya yolunu güncellemeniz gerekmektedir.

☁️ Google Colab'da Çalıştırma

Google Colab adresine gidin
Dosya > Not Defteri Yükle seçeneğiyle .ipynb dosyasını yükleyin
Kaggle veri setini Colab ortamına yükleyin
Tüm hücreleri sırayla çalıştırın (Çalışma Zamanı > Tümünü Çalıştır)


📚 Kullanılan Kütüphaneler
KütüphaneVersiyonKullanım Amacıpandas≥1.3Veri okuma ve işlemenumpy≥1.21Sayısal hesaplamalarmatplotlib≥3.4Görselleştirmeseaborn≥0.11İstatistiksel görselleştirmescikit-learn≥0.24Makine öğrenmesi modelleri ve metrikler

<p align="center">
  <i>Bu proje eğitim amaçlı hazırlanmıştır. Klinik tanı aracı olarak kullanılmamalıdır.</i>
</p>
