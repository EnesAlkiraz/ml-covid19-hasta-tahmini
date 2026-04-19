# COVID-19 Hasta Şiddeti Tahmini

> Makine öğrenmesi algoritmalarıyla COVID-19 hastalarının hastalık şiddetini (Hafif / Ağır) semptom verilerine dayanarak tahmin eden bir sınıflandırma projesi.

---

## Proje Açıklaması

Bu proje, COVID-19 hastalarının semptomlarına (ateş, öksürük, nefes darlığı vb.) dayanarak hastalık şiddetini **Hafif (Mild)** veya **Ağır/Diğer** olarak sınıflandırmayı amaçlamaktadır. İki farklı makine öğrenmesi algoritması — **Lojistik Regresyon** ve **Random Forest** — eğitilmiş, performansları karşılaştırılmış ve en başarılı model ile örnek hasta tahmini gerçekleştirilmiştir.

**Projenin Hedefleri:**
- COVID-19 semptomlarının hastalık şiddeti üzerindeki etkisini analiz etmek
- İki farklı sınıflandırma algoritmasını karşılaştırmak
- En iyi modeli belirleyerek yeni hasta verisi üzerinde tahmin yapmak

---

##  Veri Seti

**Veri Seti Adı:** COVID-19 Cleaned Dataset  
**Kaynak:** [Kaggle – COVID-19 Dataset](https://www.kaggle.com/datasets/imdevskp/corona-virus-report)

### Veri Seti Hakkında

| Özellik | Detay |
|---|---|
| Format | CSV (Cleaned-Data.csv) |
| Değişken Tipi | Binary (0/1) – Kategorik |
| Hedef Değişken | `Severity_Mild` (1: Hafif, 0: Ağır/Diğer) |

### Temel Sütunlar

| Sütun | Açıklama |
|---|---|
| `Fever` | Ateş var mı? (0/1) |
| `Tiredness` | Yorgunluk var mı? (0/1) |
| `Dry-Cough` | Kuru öksürük var mı? (0/1) |
| `Difficulty-in-Breathing` | Nefes darlığı var mı? (0/1) |
| `Sore-Throat` | Boğaz ağrısı var mı? (0/1) |
| `Severity_Mild` | Hedef: Hafif mi? (0/1) |
| `Severity_Moderate` | Orta şiddetli mi? (0/1) |
| `Severity_Severe` | Ağır mı? (0/1) |
| `Severity_None` | Semptom yok mu? (0/1) |

---

##  Veri Ön İşleme Adımları

### 1. Veri Okuma
Ham veri, zip dosyasından çıkarılarak `pandas` ile CSV formatında okunmuştur.

```python
df = pd.read_csv('./extracted_data/Cleaned-Data.csv')
```

### 2. Sayısal Sütunların Seçimi
Veri setinde yer alan kategorik/string sütunlar model eğitimine dahil edilmemiş; yalnızca sayısal (binary) özellikler seçilmiştir.

```python
df = df.select_dtypes(include=['number'])
```

### 3. Özellik ve Hedef Değişken Ayrımı
Hedef değişken olarak `Severity_Mild` seçilmiş; model sızıntısını önlemek adına tüm şiddet sütunları (`Severity_Mild`, `Severity_Moderate`, `Severity_Severe`, `Severity_None`) girdi özelliklerinden çıkarılmıştır.

```python
drop_list = ['Severity_Mild', 'Severity_Moderate', 'Severity_Severe', 'Severity_None']
X = df.drop(columns=drop_list)
y = df['Severity_Mild']
```

### 4. Eğitim / Test Bölümü
Veri seti **%80 eğitim – %20 test** oranında ayrılmıştır. Tekrarlanabilirlik için `random_state=42` kullanılmıştır.

```python
X_train, X_test, y_train, y_test = train_test_split(X, y, test_size=0.2, random_state=42)
```

### 5. Aykırı Değer Analizi
Veri seti büyük ölçüde binary (0-1) yapıda olduğundan Z-score analizi uygulanmış ve anlamlı aykırı değere rastlanmamıştır. Boxplot görselleri ile bu durum teyit edilmiştir.

---

##  Kullanılan Algoritmalar

### 1. Lojistik Regresyon (Logistic Regression)

Lojistik Regresyon, ikili sınıflandırma problemleri için yaygın kullanılan bir doğrusal modeldir. Giriş özelliklerinin doğrusal kombinasyonunu **sigmoid fonksiyonu** aracılığıyla 0 ile 1 arasında bir olasılık değerine dönüştürür.


- Belirli bir eşiğin (genellikle 0.5) üzerindeki olasılıklar **Hafif (1)**, altındakiler **Ağır/Diğer (0)** olarak sınıflandırılır.
- Veri setinin binary yapısına ve doğrusal ayrılabilirliğe uygun, yorumlanması kolay bir temel modeldir.
- `max_iter=1000` ile yakınsama sorunu önlenmiştir.

### 2. Random Forest

Random Forest, çok sayıda karar ağacının birleşiminden oluşan bir **topluluk öğrenmesi (ensemble learning)** yöntemidir.

- Her ağaç, verinin rastgele örneklenmiş bir alt kümesi (**bootstrap**) üzerinde eğitilir.
- Her düğümde yalnızca rastgele seçilen bir özellik alt kümesi dikkate alınır (**feature randomness**).
- Tüm ağaçların tahminleri **çoğunluk oyu** ile birleştirilir.
- Aşırı öğrenmeye (overfitting) karşı dirençlidir ve **özellik önem skorları** üretebilir.
- `n_estimators=100` (100 karar ağacı) kullanılmıştır.

---

##  Model Performans Karşılaştırması

Her iki model de **Accuracy**, **Precision**, **Recall** ve **F1-Score** metrikleriyle değerlendirilmiştir.

| Metrik | Logistic Regression | Random Forest |
|---|---|---|
| Accuracy | Model çıktısına göre değişir | Model çıktısına göre değişir |
| Precision | `classification_report` ile elde edilir | `classification_report` ile elde edilir |
| Recall | `classification_report` ile elde edilir | `classification_report` ile elde edilir |
| F1-Score | `classification_report` ile elde edilir | `classification_report` ile elde edilir |

> **Not:** Gerçek metrik değerleri notebook çalıştırıldığında `classification_report` çıktısında görüntülenecektir.

### Görselleştirmeler
- **Confusion Matrix (Karmaşıklık Matrisi):** Her iki model için ayrı ayrı ısı haritası
- **Model Başarı Karşılaştırması:** Bar grafiği ile doğruluk oranı karşılaştırması
- **En Önemli 10 Özellik:** Random Forest özellik önem skoru grafiği

---

## Sonuç ve Yorumlar

- **Veri seti tamamen binary (0/1) yapıda** olduğundan özellik mühendisliğine gerek duyulmamış; model, semptomların varlığını/yokluğunu doğrudan işleyebilmiştir.
- **Random Forest**, doğrusal olmayan ilişkileri yakalama kapasitesi sayesinde Lojistik Regresyon'a kıyasla genellikle daha yüksek doğruluk oranı vermektedir.
- **Özellik önem analizi**, Ateş (`Fever`) ve Yorgunluk (`Tiredness`) gibi belirtilerin hastalık şiddetini belirlemede en kritik faktörler olduğunu ortaya koymuştur.
- **Model sızıntısını önlemek** adına tüm şiddet sütunları eğitim verisinden çıkarılmıştır; bu, sonuçların güvenilirliği açısından kritik bir adımdır.
- **Geliştirme Önerileri:**
  - Hiperparametre optimizasyonu (GridSearchCV) uygulanabilir
  - K-Fold çapraz doğrulama ile model güvenilirliği artırılabilir
  - XGBoost veya SVM gibi ek algoritmalar denenerek karşılaştırma genişletilebilir
  - Sınıf dengesizliği varsa SMOTE ile veri dengeleme yapılabilir

---

##  Kodların Nasıl Çalıştırılacağı

### Gereksinimler

```bash
pip install pandas numpy matplotlib seaborn scikit-learn
```

### Adım 1: Repoyu Klonla

```bash
git clone https://github.com/kullanici-adi/covid19-hasta-tahmini.git
cd covid19-hasta-tahmini
```

### Adım 2: Veri Setini İndir

[Kaggle'dan veri setini indirin](https://www.kaggle.com/datasets/imdevskp/corona-virus-report) ve zip dosyasını proje dizinine koyun:

```
covid19-hasta-tahmini/
├── archive (3).zip        ← buraya koyun
├── COVID_19_Hasta_Tahmini.ipynb
└── README.md
```

### Adım 3: Notebook'u Aç ve Çalıştır

**Jupyter Notebook ile:**
```bash
jupyter notebook COVID_19_Hasta_Tahmini.ipynb
```

**Google Colab ile:**
1. [colab.research.google.com](https://colab.research.google.com) adresine gidin
2. `Dosya > Not Defteri Yükle` ile `.ipynb` dosyasını yükleyin
3. Veri setini Colab'a yükleyin
4. `Çalışma Zamanı > Tümünü Çalıştır` ile tüm hücreleri çalıştırın

### Adım 4: Hücreleri Sırayla Çalıştır

| Hücre | İşlem |
|---|---|
| 1 | Veri setini çıkarma ve yükleme |
| 2 | Keşifsel veri analizi ve görselleştirme |
| 3 | Model eğitimi (Logistic Regression + Random Forest) |
| 4 | Confusion Matrix ve sınıflandırma raporu |
| 5 | Aykırı değer analizi |
| 6 | Model karşılaştırma tablosu |
| 7 | Örnek hasta tahmini |

---

##  Proje Yapısı

```
covid19-hasta-tahmini/
├── COVID_19_Hasta_Tahmini.ipynb   # Ana notebook
├── README.md                       # Bu dosya
└── extracted_data/
    └── Cleaned-Data.csv            # İşlenmiş veri seti (zip'ten çıkarılır)
```

---

##  Kullanılan Kütüphaneler

| Kütüphane | Versiyon | Kullanım Amacı |
|---|---|---|
| `pandas` | ≥ 1.3 | Veri okuma ve işleme |
| `numpy` | ≥ 1.21 | Sayısal işlemler |
| `matplotlib` | ≥ 3.4 | Görselleştirme |
| `seaborn` | ≥ 0.11 | İstatistiksel grafikler |
| `scikit-learn` | ≥ 0.24 | Model eğitimi ve değerlendirme |

---
###  Hazırlayan
**Enes ALKİRAZ**  Öğrenci No: **25019921033**  Bartın Üniversitesi - Yapay Zeka Operatörlüğü

*Bu proje, makine öğrenmesi dersi kapsamında eğitim amaçlıdır.*
