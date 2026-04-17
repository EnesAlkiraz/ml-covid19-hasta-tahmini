🐍 Python 3.8+
⚙️ scikit-learn
📓 Jupyter Notebook
✅ Tamamlandı
🦠 COVID-19 Hasta Şiddet Tahmini
Bu proje, COVID-19 semptomlarına dayanarak hastaların hastalık şiddetini makine öğrenmesi ile tahmin etmeyi amaçlamaktadır. Belirtilerin ikili (binary: 0/1) giriş değerleri olarak kullanıldığı bu sistemde, hastanın hastalık seyrinin Hafif (Mild) mi yoksa Ağır/Diğer mi olacağı sınıflandırılmaktadır.

Proje kapsamında iki farklı makine öğrenmesi algoritması karşılaştırılmış; model başarıları değerlendirilmiş ve en iyi performansı gösteren model final tahmincisi olarak seçilmiştir.

🎯 Hedefler
COVID-19 semptomlarından hastalık şiddetini tahmin etmek
Lojistik Regresyon ve Random Forest algoritmalarını karşılaştırmak
Hastalık tahminine en çok katkı sağlayan belirtileri belirlemek
Yeni bir hasta için gerçek zamanlı tahmin üretmek
📂 Kullanılan Veri Seti
Özellik	Detay
Veri Seti Adı	COVID-19 Symptoms and Presence Dataset
Kaynak	Kaggle
Bağlantı	🔗 Veri Setine Git
Dosya	Cleaned-Data.csv
Veri Tipi	Binary (İkili: 0 / 1)
Veri seti; COVID-19 tanısı almış ve almamış bireylerden toplanan semptom bilgilerini içermektedir. Her satır bir hastayı, her sütun ise o hastada gözlemlenen semptomu temsil etmektedir.

📋 Temel Sütunlar
Sütun Adı	Açıklama
Fever	Ateş (0: Yok, 1: Var)
Tiredness	Yorgunluk
Dry-Cough	Kuru Öksürük
Difficulty-in-Breathing	Nefes Darlığı
Sore-Throat	Boğaz Ağrısı
Severity_Mild	🎯 Hedef Değişken — Hafif şiddet (1: Hafif, 0: Ağır/Diğer)
Severity_Moderate	Orta şiddet etiketi
Severity_Severe	Ağır şiddet etiketi
Severity_None	Belirti yok etiketi
🔧 Veri Ön İşleme Adımları
1
Veri Yükleme
Ham veri, .zip arşivinden çıkarılarak ./extracted_data/Cleaned-Data.csv yolundan pandas ile okunmuştur.

import pandas as pd
df = pd.read_csv('./extracted_data/Cleaned-Data.csv')
2
Sayısal Sütun Seçimi
String tipindeki sütunlar modele doğrudan verilemeyeceğinden yalnızca sayısal sütunlar alınmıştır.

df = df.select_dtypes(include=['number'])
3
Özellik ve Hedef Ayrımı
Hedef değişkeni Severity_Mild'dır. Modelin diğer şiddet etiketlerinden kopya çekmesini önlemek için tüm Severity_* sütunları özellik matrisinden çıkarılmıştır.

drop_list = ['Severity_Mild', 'Severity_Moderate', 'Severity_Severe', 'Severity_None']
X = df.drop(columns=drop_list)
y = df['Severity_Mild']
4
Eğitim / Test Bölümlemesi
Veri %80 eğitim ve %20 test olarak ayrılmıştır. Tekrar üretilebilirlik için random_state=42 kullanılmıştır.

from sklearn.model_selection import train_test_split

X_train, X_test, y_train, y_test = train_test_split(
    X, y, test_size=0.2, random_state=42
)
5
Aykırı Değer Analizi
Veri seti binary (0-1) yapıda olduğundan Z-score analizi uygulanmış; uç değer tespit edilmemiştir. Bu durum kutu grafikleriyle (boxplot) görsel olarak da doğrulanmıştır.

🤖 Kullanılan Algoritmaların Mantığı
📈 1 — Lojistik Regresyon
Lojistik Regresyon, ikili sınıflandırma problemleri için klasik bir istatistiksel modeldir. Girdi özelliklerinin ağırlıklı toplamını sigmoid fonksiyonu ile 0-1 aralığına dönüştürerek bir sınıfa ait olma olasılığını hesaplar.

P(y=1 | X) = 1 / ( 1 + e−(β₀ + β₁x₁ + ... + βₙxₙ) )
Avantajları
Yorumlanabilirliği yüksektir
Az veriyle de iyi çalışır
Eğitim süresi hızlıdır
from sklearn.linear_model import LogisticRegression

lr_model = LogisticRegression(max_iter=1000)
lr_model.fit(X_train, y_train)  # max_iter=1000 → yakınsama sorununu önler
🌲 2 — Random Forest (Rastgele Orman)
Random Forest, birden fazla karar ağacının bir araya getirildiği topluluk öğrenmesi (ensemble learning) yöntemidir. Her ağaç verinin rastgele bir alt kümesinde eğitilir; sonuçlar çoğunluk oylamasıyla birleştirilir.

Temel Prensipler
Bagging: Her ağaç, eğitim verisinin farklı bir bootstrap örneği üzerinde oluşturulur.
Rastgele Özellik Seçimi: Her düğümde özelliklerin rastgele alt kümesi değerlendirilir — ağaçlar arası korelasyonu azaltır.
Çoğunluk Oylaması: Tüm ağaçların tahminleri birleştirilerek nihai sınıf belirlenir.
from sklearn.ensemble import RandomForestClassifier

rf_model = RandomForestClassifier(n_estimators=100)
rf_model.fit(X_train, y_train)  # 100 karar ağacından oluşan orman
📊 Model Performans Karşılaştırması
Her iki model de Confusion Matrix, Classification Report (Precision, Recall, F1-Score) ve Accuracy Score metrikleriyle değerlendirilmiştir.

📈 Logistic Regression
Accuracy
✔ Hesaplandı
Precision
✔ Raporlandı
Recall
✔ Raporlandı
F1-Score
✔ Raporlandı
Confusion Matrix
✔ Görselleştirildi
🌲 Random Forest
Accuracy
✔ Hesaplandı
Precision
✔ Raporlandı
Recall
✔ Raporlandı
F1-Score
✔ Raporlandı
Confusion Matrix
✔ Görselleştirildi
🏆 Final Model Seçimi
best_model = models.loc[models['Score'].idxmax(), 'Model']
print(f"En yüksek başarı: {best_model}")
Doğruluk skoru karşılaştırması sonucunda en yüksek başarıyı sergileyen model otomatik olarak tespit edilmiş ve final tahminci olarak seçilmiştir.

🔍 Özellik Önem Analizi
Random Forest modeli aracılığıyla hastalık şiddetini tahmin etmede en belirleyici ilk 10 semptom belirlenmiştir. Bu analiz, klinik açıdan hangi belirtilerin daha kritik olduğunu ortaya koymaktadır.

💡 Sonuç ve Yorumlar
Veri Kalitesi: Veri seti temiz ve binary formatlı olduğundan kapsamlı bir temizleme adımı gerekmemiştir.
Model Seçimi: Random Forest, ensemble yapısı sayesinde genellikle Lojistik Regresyon'a kıyasla daha yüksek doğruluk sergilemiştir. Lojistik Regresyon ise yorumlanabilirliği ve hızı açısından avantajlıdır.
Özellik Önemi: Ateş, kuru öksürük ve nefes darlığı hastalık şiddetini tahmin etmede en belirleyici semptomlar arasında yer almaktadır.
Gerçek Zamanlı Tahmin: Model, yeni hasta verisi için predict() ve predict_proba() fonksiyonları aracılığıyla hem sınıf tahmini hem de olasılık skoru üretmektedir.
Sınırlılıklar: Yaş, cinsiyet gibi demografik değişkenler modele dahil edilmemiştir. Daha büyük ve dengeli veri setleriyle model performansı artırılabilir.
🚀 Kodların Nasıl Çalıştırılacağı
✅ Gereksinimler
pip install pandas numpy matplotlib seaborn scikit-learn jupyter
📁 Proje Yapısı
📦 COVID-19-Hasta-Tahmini/
├── 📓 COVID_19_Hasta_Tahmini.ipynb   # Ana Jupyter Notebook
├── 📁 extracted_data/
│   └── Cleaned-Data.csv              # İşlenmiş veri seti
├── archive.zip                       # Ham veri arşivi (Kaggle'dan indirilir)
└── 📄 README.md
▶️ Adım Adım Çalıştırma
1
Depoyu klonlayın
git clone https://github.com/kullanici-adi/covid19-hasta-tahmini.git
cd covid19-hasta-tahmini
2
Veri setini indirin
Kaggle Veri Seti Sayfası'ndan archive.zip dosyasını indirip proje klasörüne kopyalayın.

3
Jupyter Notebook'u başlatın
jupyter notebook
4
Notebook'u çalıştırın
COVID_19_Hasta_Tahmini.ipynb dosyasını açın → Kernel > Restart & Run All

⚠️ Not: Google Colab'da çalıştırıyorsanız zip dosya yolunu /content/archive (3).zip olarak bırakabilirsiniz. Yerel ortamda dosya yolunu güncellemeniz gerekir.
☁️ Google Colab'da Çalıştırma
Google Colab'a gidin
Dosya > Not Defteri Yükle ile .ipynb dosyasını yükleyin
Kaggle veri setini Colab ortamına aktarın
Çalışma Zamanı > Tümünü Çalıştır seçeneğiyle başlatın
📚 Kullanılan Kütüphaneler
Kütüphane	Versiyon	Kullanım Amacı
pandas	≥ 1.3	Veri okuma ve işleme
numpy	≥ 1.21	Sayısal hesaplamalar
matplotlib	≥ 3.4	Görselleştirme
seaborn	≥ 0.11	İstatistiksel görselleştirme
scikit-learn	≥ 0.24	Makine öğrenmesi modelleri ve metrikler
Bu proje eğitim amaçlı hazırlanmıştır. Klinik tanı aracı olarak kullanılmamalıdır.
