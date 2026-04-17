<h1>🦠 COVID-19 Hasta Şiddeti Tahmini</h1>

<p>
  Bu proje, COVID-19 semptomlarına dayanarak hastaların hastalık şiddetini makine öğrenmesi ile tahmin etmeyi amaçlamaktadır.
  Belirtilerin ikili (binary: 0/1) giriş değerleri olarak kullanıldığı bu sistemde, hastanın hastalık seyri
  <strong>Hafif (Mild)</strong> mi yoksa <strong>Ağır/Diğer</strong> mi olacağı sınıflandırılmaktadır.
</p>

<p>
  Proje kapsamında iki farklı makine öğrenmesi algoritması karşılaştırılmış, model başarıları değerlendirilmiş
  ve en iyi performansı gösteren model final tahmincisi olarak seçilmiştir.
</p>

<h3>🎯 Hedefler</h3>
<ul>
  <li>COVID-19 semptomlarından hastalık şiddetini tahmin etmek</li>
  <li>Lojistik Regresyon ve Random Forest algoritmalarını karşılaştırmak</li>
  <li>Hastalık tahminine en çok katkı sağlayan belirtileri belirlemek</li>
  <li>Yeni bir hasta için gerçek zamanlı tahmin üretmek</li>
</ul>

<hr/>

<h2>📂 Kullanılan Veri Seti</h2>

<table>
  <thead><tr><th>Özellik</th><th>Detay</th></tr></thead>
  <tbody>
    <tr><td><strong>Veri Seti Adı</strong></td><td>COVID-19 Symptoms and Presence Dataset</td></tr>
    <tr><td><strong>Kaynak</strong></td><td>Kaggle</td></tr>
    <tr><td><strong>Bağlantı</strong></td><td><a href="https://www.kaggle.com/datasets/imdevskp/corona-virus-report" target="_blank">🔗 Veri Setine Git</a></td></tr>
    <tr><td><strong>Dosya</strong></td><td><code>Cleaned-Data.csv</code></td></tr>
    <tr><td><strong>Veri Tipi</strong></td><td>Binary (İkili: 0 / 1)</td></tr>
  </tbody>
</table>

<p>
  Veri seti; COVID-19 tanısı almış ve almamış bireylerden toplanan semptom bilgilerini içermektedir.
  Her satır bir hastayı, her sütun ise o hastada gözlemlenen semptomu temsil etmektedir.
</p>

<h3>📋 Temel Sütunlar</h3>
<table>
  <thead><tr><th>Sütun Adı</th><th>Açıklama</th></tr></thead>
  <tbody>
    <tr><td><code>Fever</code></td><td>Ateş (0: Yok, 1: Var)</td></tr>
    <tr><td><code>Tiredness</code></td><td>Yorgunluk</td></tr>
    <tr><td><code>Dry-Cough</code></td><td>Kuru öksürük</td></tr>
    <tr><td><code>Difficulty-in-Breathing</code></td><td>Nefes darlığı</td></tr>
    <tr><td><code>Sore-Throat</code></td><td>Boğaz ağrısı</td></tr>
    <tr><td><code>Severity_Mild</code></td><td>🎯 <strong>Hedef değişken</strong> — Hafif şiddet (1: Hafif, 0: Ağır/Diğer)</td></tr>
    <tr><td><code>Severity_Moderate</code></td><td>Orta şiddet etiketi</td></tr>
    <tr><td><code>Severity_Severe</code></td><td>Ağır şiddet etiketi</td></tr>
    <tr><td><code>Severity_None</code></td><td>Belirti yok etiketi</td></tr>
  </tbody>
</table>

<hr/>

<h2>🔧 Veri Ön İşleme Adımları</h2>

<div class="step">
  <div class="step-num">1</div>
  <div class="step-content">
    <h4>Veri Yükleme</h4>
    <p>Ham veri, <code>.zip</code> arşivinden çıkarılarak <code>./extracted_data/Cleaned-Data.csv</code> yolundan <code>pandas</code> ile okunmuştur.</p>
    <pre><code>import pandas as pd
df = pd.read_csv('./extracted_data/Cleaned-Data.csv')</code></pre>
  </div>
</div>

<div class="step">
  <div class="step-num">2</div>
  <div class="step-content">
    <h4>Sayısal Sütun Seçimi</h4>
    <p>String tipindeki sütunlar modele doğrudan verilemediğinden yalnızca sayısal sütunlar alınmıştır.</p>
    <pre><code>df = df.select_dtypes(include=['number'])</code></pre>
  </div>
</div>

<div class="step">
  <div class="step-num">3</div>
  <div class="step-content">
    <h4>Özellik ve Hedef Ayrımı</h4>
    <p>Hedef değişken <code>Severity_Mild</code>'dir. Modelin diğer şiddet etiketlerinden veri sızıntısı yapmasını önlemek için tüm <code>Severity_*</code> sütunları özelliklerden çıkarılmıştır.</p>
    <pre><code>
drop_list = ['Severity_Mild', 'Severity_Moderate', 'Severity_Severe', 'Severity_None']
X = df.drop(columns=drop_list)
y = df['Severity_Mild']
    </code></pre>
  </div>
</div>

<div class="step">
  <div class="step-num">4</div>
  <div class="step-content">
    <h4>Eğitim / Test Bölünmesi</h4>
    <p>Veri %80 eğitim ve %20 test olarak ayrılmıştır. Tekrarlanabilirlik için <code>random_state=42</code> kullanılmıştır.</p>
    <pre><code>
from sklearn.model_selection import train_test_split

X_train, X_test, y_train, y_test = train_test_split(
    X, y, test_size=0.2, random_state=42
)
    </code></pre>
  </div>
</div>

<div class="step">
  <div class="step-num">5</div>
  <div class="step-content">
    <h4>Aykırı Değer Analizi</h4>
    <p>Veri seti binary (0–1) yapıda olduğundan Z-score analizi uygulanmış, uç değer tespit edilmemiştir. Bu durum kutu grafikleriyle de doğrulanmıştır.</p>
  </div>
</div>

<hr/>

<h2>🤖 Kullanılan Algoritmaların Mantığı</h2>

<h3>📈 1 — Lojistik Regresyon</h3>
<p>
  Lojistik regresyon, ikili sınıflandırma problemleri için kullanılan istatistiksel bir modeldir.
  Girdi özelliklerinin ağırlıklı toplamını sigmoid fonksiyonu ile 0–1 aralığına dönüştürerek bir sınıfa ait olma olasılığını hesaplar.
</p>

<h3>🌲 2 — Random Forest</h3>
<p>
  Random Forest, birden fazla karar ağacının bir araya getirildiği bir topluluk öğrenmesi (ensemble learning) yöntemidir.
  Her ağaç verinin rastgele alt kümeleri üzerinde eğitilir ve sonuçlar çoğunluk oylaması ile birleştirilir.
</p>

<hr/>

<h2>💡 Sonuç ve Yorumlar</h2>

<ol>
  <li><strong>Veri Kalitesi:</strong> Veri seti temiz ve binary formatta olduğundan kapsamlı bir veri temizleme adımı gerektirmemiştir.</li>
  <li><strong>Model Seçimi:</strong> Random Forest, ensemble yapısı sayesinde genellikle Lojistik Regresyon’a kıyasla daha yüksek doğruluk sergilemiştir. Lojistik regresyon ise yorumlanabilirliği ve hızı açısından avantajlıdır.</li>
  <li><strong>Özellik Önemi:</strong> Ateş, kuru öksürük ve nefes darlığı hastalık şiddetini tahmin etmede en önemli semptomlar arasındadır.</li>
  <li><strong>Gerçek Zamanlı Tahmin:</strong> Model, yeni hasta verisi için <code>predict()</code> ve <code>predict_proba()</code> fonksiyonlarıyla hem sınıf tahmini hem de olasılık üretmektedir.</li>
  <li><strong>Sınırlılıklar:</strong> Yaş ve cinsiyet gibi demografik değişkenler modele dahil edilmemiştir. Daha büyük veri setleri ile performans artırılabilir.</li>
</ol>

<hr/>

<h2>🚀 Kodların Nasıl Çalıştırılacağı</h2>

<h3>📚 Gereksinimler</h3>
<pre><code>pip install pandas numpy matplotlib seaborn scikit-learn jupyter</code></pre>
