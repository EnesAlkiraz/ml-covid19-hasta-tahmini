<!DOCTYPE html>
<html lang="tr">
<head>
<meta charset="UTF-8" />
<meta name="viewport" content="width=device-width, initial-scale=1.0"/>
<title>COVID-19 Hasta Tahmini — README</title>
<style>
  * { box-sizing: border-box; margin: 0; padding: 0; }
  body {
    font-family: -apple-system, BlinkMacSystemFont, "Segoe UI", Roboto, sans-serif;
    background: #0d1117;
    color: #c9d1d9;
    line-height: 1.7;
    padding: 40px 20px;
  }
  .container {
    max-width: 860px;
    margin: 0 auto;
    background: #161b22;
    border: 1px solid #30363d;
    border-radius: 12px;
    padding: 48px 52px;
  }

  /* BADGES */
  .badges {
    display: flex;
    flex-wrap: wrap;
    gap: 8px;
    margin-bottom: 36px;
  }
  .badge {
    display: inline-flex;
    align-items: center;
    gap: 6px;
    padding: 5px 12px;
    border-radius: 20px;
    font-size: 12px;
    font-weight: 600;
    letter-spacing: 0.3px;
  }
  .badge-blue   { background: #1f4a8a; color: #79c0ff; border: 1px solid #388bfd; }
  .badge-orange { background: #5a2d00; color: #ffa657; border: 1px solid #d1882c; }
  .badge-orange2{ background: #3d2000; color: #e3b341; border: 1px solid #9e6a03; }
  .badge-green  { background: #0f3d25; color: #56d364; border: 1px solid #238636; }

  /* MAIN TITLE */
  h1 {
    font-size: 32px;
    font-weight: 800;
    color: #f0f6fc;
    border-bottom: 3px solid #238636;
    padding-bottom: 14px;
    margin-bottom: 28px;
  }

  /* SECTION TITLES h2 */
  h2 {
    font-size: 20px;
    font-weight: 700;
    color: #f0f6fc;
    background: linear-gradient(90deg, #21262d, transparent);
    border-left: 4px solid #238636;
    padding: 10px 16px;
    margin-top: 44px;
    margin-bottom: 18px;
    border-radius: 0 6px 6px 0;
  }

  /* SUB TITLES h3 */
  h3 {
    font-size: 15px;
    font-weight: 700;
    color: #79c0ff;
    text-transform: uppercase;
    letter-spacing: 0.8px;
    margin-top: 26px;
    margin-bottom: 10px;
    padding-bottom: 4px;
    border-bottom: 1px dashed #30363d;
  }

  p { margin-bottom: 12px; color: #c9d1d9; font-size: 14.5px; }

  /* HORIZONTAL RULE */
  hr { border: none; border-top: 1px solid #30363d; margin: 36px 0; }

  /* TABLES */
  table {
    width: 100%;
    border-collapse: collapse;
    font-size: 13.5px;
    margin-bottom: 20px;
    border-radius: 8px;
    overflow: hidden;
  }
  thead tr { background: #21262d; }
  th {
    text-align: left;
    padding: 10px 14px;
    color: #79c0ff;
    font-weight: 700;
    font-size: 12px;
    text-transform: uppercase;
    letter-spacing: 0.5px;
    border-bottom: 2px solid #30363d;
  }
  td { padding: 9px 14px; border-bottom: 1px solid #21262d; }
  tr:last-child td { border-bottom: none; }
  tr:hover td { background: #1c2128; }
  td code, th code {
    background: #2d333b;
    padding: 1px 6px;
    border-radius: 4px;
    font-size: 12px;
    color: #e3b341;
    font-family: "SFMono-Regular", Consolas, monospace;
  }

  /* CODE BLOCKS */
  pre {
    background: #0d1117;
    border: 1px solid #30363d;
    border-radius: 8px;
    padding: 18px 20px;
    overflow-x: auto;
    margin: 14px 0 20px;
    font-size: 13px;
    line-height: 1.6;
  }
  code {
    font-family: "SFMono-Regular", Consolas, "Liberation Mono", monospace;
    color: #e3b341;
  }
  pre code { color: #c9d1d9; }
  .kw  { color: #ff7b72; }
  .fn  { color: #d2a8ff; }
  .st  { color: #a5d6ff; }
  .cm  { color: #8b949e; font-style: italic; }
  .nm  { color: #79c0ff; }

  /* LISTS */
  ul, ol { padding-left: 22px; margin-bottom: 14px; }
  li { margin-bottom: 6px; font-size: 14.5px; color: #c9d1d9; }

  /* INLINE CODE */
  p code, li code {
    background: #2d333b;
    padding: 1px 6px;
    border-radius: 4px;
    font-size: 12.5px;
    color: #e3b341;
    font-family: monospace;
  }

  /* HIGHLIGHT BOX */
  .note {
    background: #0f3d25;
    border: 1px solid #238636;
    border-radius: 8px;
    padding: 12px 16px;
    font-size: 13.5px;
    margin: 14px 0;
    color: #aff5b4;
  }
  .warn {
    background: #3d2000;
    border: 1px solid #9e6a03;
    border-left: 4px solid #e3b341;
    border-radius: 8px;
    padding: 12px 16px;
    font-size: 13.5px;
    margin: 14px 0;
    color: #e3b341;
  }

  /* STEP BLOCKS */
  .step {
    display: flex;
    gap: 14px;
    margin-bottom: 18px;
    align-items: flex-start;
  }
  .step-num {
    min-width: 30px;
    height: 30px;
    border-radius: 50%;
    background: #238636;
    color: #fff;
    font-weight: 800;
    font-size: 13px;
    display: flex;
    align-items: center;
    justify-content: center;
    margin-top: 2px;
  }
  .step-content h4 {
    font-size: 14px;
    font-weight: 700;
    color: #f0f6fc;
    margin-bottom: 4px;
  }
  .step-content p { margin: 0; font-size: 13.5px; }

  /* FORMULA BOX */
  .formula {
    background: #161b22;
    border: 1px solid #388bfd;
    border-radius: 8px;
    padding: 14px 18px;
    text-align: center;
    font-size: 15px;
    color: #79c0ff;
    margin: 14px 0;
    font-family: "Georgia", serif;
  }

  /* FOOTER */
  .footer {
    text-align: center;
    margin-top: 50px;
    padding-top: 20px;
    border-top: 1px solid #30363d;
    font-size: 12.5px;
    color: #6e7681;
  }

  strong { color: #f0f6fc; }
  a { color: #58a6ff; text-decoration: none; }
  a:hover { text-decoration: underline; }

  .tag {
    display: inline-block;
    background: #21262d;
    border: 1px solid #30363d;
    border-radius: 4px;
    padding: 2px 8px;
    font-size: 12px;
    color: #8b949e;
    margin: 2px;
  }

  .perf-grid {
    display: grid;
    grid-template-columns: 1fr 1fr;
    gap: 14px;
    margin: 16px 0;
  }
  .perf-card {
    background: #21262d;
    border: 1px solid #30363d;
    border-radius: 8px;
    padding: 16px;
  }
  .perf-card h4 {
    font-size: 13px;
    font-weight: 700;
    color: #79c0ff;
    margin-bottom: 10px;
    text-transform: uppercase;
    letter-spacing: 0.5px;
  }
  .perf-card .metric { display: flex; justify-content: space-between; font-size: 13px; margin-bottom: 6px; }
  .perf-card .metric span:last-child { color: #56d364; font-weight: 700; }
</style>
</head>
<body>
<div class="container">

  <!-- BADGES -->
  <div class="badges">
    <span class="badge badge-blue">🐍 Python 3.8+</span>
    <span class="badge badge-orange">⚙️ scikit-learn</span>
    <span class="badge badge-orange2">📓 Jupyter Notebook</span>
    <span class="badge badge-green">✅ Tamamlandı</span>
  </div>

  <!-- MAIN TITLE -->
  <h1>🦠 COVID-19 Hasta Şiddet Tahmini</h1>

  <p>
    Bu proje, COVID-19 semptomlarına dayanarak hastaların hastalık şiddetini makine öğrenmesi ile tahmin etmeyi amaçlamaktadır.
    Belirtilerin ikili (binary: 0/1) giriş değerleri olarak kullanıldığı bu sistemde, hastanın hastalık seyrinin
    <strong>Hafif (Mild)</strong> mi yoksa <strong>Ağır/Diğer</strong> mi olacağı sınıflandırılmaktadır.
  </p>
  <p>
    Proje kapsamında iki farklı makine öğrenmesi algoritması karşılaştırılmış; model başarıları değerlendirilmiş
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

  <!-- DATASET -->
  <h2>📂 Kullanılan Veri Seti</h2>

  <table>
    <thead><tr><th>Özellik</th><th>Detay</th></tr></thead>
    <tbody>
      <tr><td><strong>Veri Seti Adı</strong></td><td>COVID-19 Symptoms and Presence Dataset</td></tr>
      <tr><td><strong>Kaynak</strong></td><td>Kaggle</td></tr>
      <tr><td><strong>Bağlantı</strong></td><td><a href="https://www.kaggle.com/datasets/hemanthhari/symptoms-and-covid-presence" target="_blank">🔗 Veri Setine Git</a></td></tr>
      <tr><td><strong>Dosya</strong></td><td><code>Cleaned-Data.csv</code></td></tr>
      <tr><td><strong>Veri Tipi</strong></td><td>Binary (İkili: 0 / 1)</td></tr>
    </tbody>
  </table>

  <p>Veri seti; COVID-19 tanısı almış ve almamış bireylerden toplanan semptom bilgilerini içermektedir.
  Her satır bir hastayı, her sütun ise o hastada gözlemlenen semptomu temsil etmektedir.</p>

  <h3>📋 Temel Sütunlar</h3>
  <table>
    <thead><tr><th>Sütun Adı</th><th>Açıklama</th></tr></thead>
    <tbody>
      <tr><td><code>Fever</code></td><td>Ateş (0: Yok, 1: Var)</td></tr>
      <tr><td><code>Tiredness</code></td><td>Yorgunluk</td></tr>
      <tr><td><code>Dry-Cough</code></td><td>Kuru Öksürük</td></tr>
      <tr><td><code>Difficulty-in-Breathing</code></td><td>Nefes Darlığı</td></tr>
      <tr><td><code>Sore-Throat</code></td><td>Boğaz Ağrısı</td></tr>
      <tr><td><code>Severity_Mild</code></td><td>🎯 <strong>Hedef Değişken</strong> — Hafif şiddet (1: Hafif, 0: Ağır/Diğer)</td></tr>
      <tr><td><code>Severity_Moderate</code></td><td>Orta şiddet etiketi</td></tr>
      <tr><td><code>Severity_Severe</code></td><td>Ağır şiddet etiketi</td></tr>
      <tr><td><code>Severity_None</code></td><td>Belirti yok etiketi</td></tr>
    </tbody>
  </table>

  <hr/>

  <!-- PREPROCESSING -->
  <h2>🔧 Veri Ön İşleme Adımları</h2>

  <div class="step">
    <div class="step-num">1</div>
    <div class="step-content">
      <h4>Veri Yükleme</h4>
      <p>Ham veri, <code>.zip</code> arşivinden çıkarılarak <code>./extracted_data/Cleaned-Data.csv</code> yolundan <code>pandas</code> ile okunmuştur.</p>
      <pre><code><span class="kw">import</span> pandas <span class="kw">as</span> pd
df = pd.<span class="fn">read_csv</span>(<span class="st">'./extracted_data/Cleaned-Data.csv'</span>)</code></pre>
    </div>
  </div>

  <div class="step">
    <div class="step-num">2</div>
    <div class="step-content">
      <h4>Sayısal Sütun Seçimi</h4>
      <p>String tipindeki sütunlar modele doğrudan verilemeyeceğinden yalnızca sayısal sütunlar alınmıştır.</p>
      <pre><code>df = df.<span class="fn">select_dtypes</span>(include=[<span class="st">'number'</span>])</code></pre>
    </div>
  </div>

  <div class="step">
    <div class="step-num">3</div>
    <div class="step-content">
      <h4>Özellik ve Hedef Ayrımı</h4>
      <p>Hedef değişkeni <code>Severity_Mild</code>'dır. Modelin diğer şiddet etiketlerinden kopya çekmesini önlemek için tüm <code>Severity_*</code> sütunları özellik matrisinden çıkarılmıştır.</p>
      <pre><code>drop_list = [<span class="st">'Severity_Mild'</span>, <span class="st">'Severity_Moderate'</span>, <span class="st">'Severity_Severe'</span>, <span class="st">'Severity_None'</span>]
X = df.<span class="fn">drop</span>(columns=drop_list)
y = df[<span class="st">'Severity_Mild'</span>]</code></pre>
    </div>
  </div>

  <div class="step">
    <div class="step-num">4</div>
    <div class="step-content">
      <h4>Eğitim / Test Bölümlemesi</h4>
      <p>Veri <strong>%80 eğitim</strong> ve <strong>%20 test</strong> olarak ayrılmıştır. Tekrar üretilebilirlik için <code>random_state=42</code> kullanılmıştır.</p>
      <pre><code><span class="kw">from</span> sklearn.model_selection <span class="kw">import</span> train_test_split

X_train, X_test, y_train, y_test = <span class="fn">train_test_split</span>(
    X, y, test_size=<span class="nm">0.2</span>, random_state=<span class="nm">42</span>
)</code></pre>
    </div>
  </div>

  <div class="step">
    <div class="step-num">5</div>
    <div class="step-content">
      <h4>Aykırı Değer Analizi</h4>
      <p>Veri seti binary (0-1) yapıda olduğundan Z-score analizi uygulanmış; uç değer tespit edilmemiştir. Bu durum kutu grafikleriyle (boxplot) görsel olarak da doğrulanmıştır.</p>
    </div>
  </div>

  <hr/>

  <!-- ALGORITHMS -->
  <h2>🤖 Kullanılan Algoritmaların Mantığı</h2>

  <h3>📈 1 — Lojistik Regresyon</h3>
  <p>
    Lojistik Regresyon, ikili sınıflandırma problemleri için klasik bir istatistiksel modeldir.
    Girdi özelliklerinin ağırlıklı toplamını <strong>sigmoid fonksiyonu</strong> ile 0-1 aralığına dönüştürerek
    bir sınıfa ait olma olasılığını hesaplar.
  </p>

  <div class="formula">
    P(y=1 | X) = 1 / ( 1 + e<sup>−(β₀ + β₁x₁ + ... + βₙxₙ)</sup> )
  </div>

  <h3>Avantajları</h3>
  <ul>
    <li>Yorumlanabilirliği yüksektir</li>
    <li>Az veriyle de iyi çalışır</li>
    <li>Eğitim süresi hızlıdır</li>
  </ul>

  <pre><code><span class="kw">from</span> sklearn.linear_model <span class="kw">import</span> LogisticRegression

lr_model = <span class="fn">LogisticRegression</span>(max_iter=<span class="nm">1000</span>)
lr_model.<span class="fn">fit</span>(X_train, y_train)  <span class="cm"># max_iter=1000 → yakınsama sorununu önler</span></code></pre>

  <h3>🌲 2 — Random Forest (Rastgele Orman)</h3>
  <p>
    Random Forest, birden fazla karar ağacının bir araya getirildiği <strong>topluluk öğrenmesi (ensemble learning)</strong> yöntemidir.
    Her ağaç verinin rastgele bir alt kümesinde eğitilir; sonuçlar çoğunluk oylamasıyla birleştirilir.
  </p>

  <h3>Temel Prensipler</h3>
  <ul>
    <li><strong>Bagging:</strong> Her ağaç, eğitim verisinin farklı bir bootstrap örneği üzerinde oluşturulur.</li>
    <li><strong>Rastgele Özellik Seçimi:</strong> Her düğümde özelliklerin rastgele alt kümesi değerlendirilir — ağaçlar arası korelasyonu azaltır.</li>
    <li><strong>Çoğunluk Oylaması:</strong> Tüm ağaçların tahminleri birleştirilerek nihai sınıf belirlenir.</li>
  </ul>

  <pre><code><span class="kw">from</span> sklearn.ensemble <span class="kw">import</span> RandomForestClassifier

rf_model = <span class="fn">RandomForestClassifier</span>(n_estimators=<span class="nm">100</span>)
rf_model.<span class="fn">fit</span>(X_train, y_train)  <span class="cm"># 100 karar ağacından oluşan orman</span></code></pre>

  <hr/>

  <!-- PERFORMANCE -->
  <h2>📊 Model Performans Karşılaştırması</h2>

  <p>Her iki model de <strong>Confusion Matrix</strong>, <strong>Classification Report</strong> (Precision, Recall, F1-Score) ve <strong>Accuracy Score</strong> metrikleriyle değerlendirilmiştir.</p>

  <div class="perf-grid">
    <div class="perf-card">
      <h4>📈 Logistic Regression</h4>
      <div class="metric"><span>Accuracy</span><span>✔ Hesaplandı</span></div>
      <div class="metric"><span>Precision</span><span>✔ Raporlandı</span></div>
      <div class="metric"><span>Recall</span><span>✔ Raporlandı</span></div>
      <div class="metric"><span>F1-Score</span><span>✔ Raporlandı</span></div>
      <div class="metric"><span>Confusion Matrix</span><span>✔ Görselleştirildi</span></div>
    </div>
    <div class="perf-card">
      <h4>🌲 Random Forest</h4>
      <div class="metric"><span>Accuracy</span><span>✔ Hesaplandı</span></div>
      <div class="metric"><span>Precision</span><span>✔ Raporlandı</span></div>
      <div class="metric"><span>Recall</span><span>✔ Raporlandı</span></div>
      <div class="metric"><span>F1-Score</span><span>✔ Raporlandı</span></div>
      <div class="metric"><span>Confusion Matrix</span><span>✔ Görselleştirildi</span></div>
    </div>
  </div>

  <h3>🏆 Final Model Seçimi</h3>
  <pre><code>best_model = models.<span class="fn">loc</span>[models[<span class="st">'Score'</span>].<span class="fn">idxmax</span>(), <span class="st">'Model'</span>]
<span class="fn">print</span>(<span class="st">f"En yüksek başarı: {best_model}"</span>)</code></pre>
  <p>Doğruluk skoru karşılaştırması sonucunda en yüksek başarıyı sergileyen model otomatik olarak tespit edilmiş ve <strong>final tahminci</strong> olarak seçilmiştir.</p>

  <h3>🔍 Özellik Önem Analizi</h3>
  <p>Random Forest modeli aracılığıyla hastalık şiddetini tahmin etmede en belirleyici <strong>ilk 10 semptom</strong> belirlenmiştir. Bu analiz, klinik açıdan hangi belirtilerin daha kritik olduğunu ortaya koymaktadır.</p>

  <hr/>

  <!-- RESULTS -->
  <h2>💡 Sonuç ve Yorumlar</h2>

  <ol>
    <li><strong>Veri Kalitesi:</strong> Veri seti temiz ve binary formatlı olduğundan kapsamlı bir temizleme adımı gerekmemiştir.</li>
    <li><strong>Model Seçimi:</strong> Random Forest, ensemble yapısı sayesinde genellikle Lojistik Regresyon'a kıyasla daha yüksek doğruluk sergilemiştir. Lojistik Regresyon ise yorumlanabilirliği ve hızı açısından avantajlıdır.</li>
    <li><strong>Özellik Önemi:</strong> Ateş, kuru öksürük ve nefes darlığı hastalık şiddetini tahmin etmede en belirleyici semptomlar arasında yer almaktadır.</li>
    <li><strong>Gerçek Zamanlı Tahmin:</strong> Model, yeni hasta verisi için <code>predict()</code> ve <code>predict_proba()</code> fonksiyonları aracılığıyla hem sınıf tahmini hem de olasılık skoru üretmektedir.</li>
    <li><strong>Sınırlılıklar:</strong> Yaş, cinsiyet gibi demografik değişkenler modele dahil edilmemiştir. Daha büyük ve dengeli veri setleriyle model performansı artırılabilir.</li>
  </ol>

  <hr/>

  <!-- HOW TO RUN -->
  <h2>🚀 Kodların Nasıl Çalıştırılacağı</h2>

  <h3>✅ Gereksinimler</h3>
  <pre><code>pip install pandas numpy matplotlib seaborn scikit-learn jupyter</code></pre>

  <h3>📁 Proje Yapısı</h3>
  <pre><code>📦 COVID-19-Hasta-Tahmini/
├── 📓 COVID_19_Hasta_Tahmini.ipynb   <span class="cm"># Ana Jupyter Notebook</span>
├── 📁 extracted_data/
│   └── Cleaned-Data.csv              <span class="cm"># İşlenmiş veri seti</span>
├── archive.zip                       <span class="cm"># Ham veri arşivi (Kaggle'dan indirilir)</span>
└── 📄 README.md</code></pre>

  <h3>▶️ Adım Adım Çalıştırma</h3>

  <div class="step">
    <div class="step-num">1</div>
    <div class="step-content">
      <h4>Depoyu klonlayın</h4>
      <pre><code>git clone https://github.com/kullanici-adi/covid19-hasta-tahmini.git
cd covid19-hasta-tahmini</code></pre>
    </div>
  </div>

  <div class="step">
    <div class="step-num">2</div>
    <div class="step-content">
      <h4>Veri setini indirin</h4>
      <p><a href="https://www.kaggle.com/datasets/hemanthhari/symptoms-and-covid-presence" target="_blank">Kaggle Veri Seti Sayfası</a>'ndan <code>archive.zip</code> dosyasını indirip proje klasörüne kopyalayın.</p>
    </div>
  </div>

  <div class="step">
    <div class="step-num">3</div>
    <div class="step-content">
      <h4>Jupyter Notebook'u başlatın</h4>
      <pre><code>jupyter notebook</code></pre>
    </div>
  </div>

  <div class="step">
    <div class="step-num">4</div>
    <div class="step-content">
      <h4>Notebook'u çalıştırın</h4>
      <p><code>COVID_19_Hasta_Tahmini.ipynb</code> dosyasını açın → <strong>Kernel &gt; Restart &amp; Run All</strong></p>
    </div>
  </div>

  <div class="warn">
    ⚠️ <strong>Not:</strong> Google Colab'da çalıştırıyorsanız zip dosya yolunu <code>/content/archive (3).zip</code> olarak bırakabilirsiniz. Yerel ortamda dosya yolunu güncellemeniz gerekir.
  </div>

  <h3>☁️ Google Colab'da Çalıştırma</h3>
  <ol>
    <li><a href="https://colab.research.google.com/" target="_blank">Google Colab</a>'a gidin</li>
    <li><strong>Dosya &gt; Not Defteri Yükle</strong> ile <code>.ipynb</code> dosyasını yükleyin</li>
    <li>Kaggle veri setini Colab ortamına aktarın</li>
    <li><strong>Çalışma Zamanı &gt; Tümünü Çalıştır</strong> seçeneğiyle başlatın</li>
  </ol>

  <hr/>

  <!-- LIBRARIES -->
  <h2>📚 Kullanılan Kütüphaneler</h2>
  <table>
    <thead><tr><th>Kütüphane</th><th>Versiyon</th><th>Kullanım Amacı</th></tr></thead>
    <tbody>
      <tr><td><code>pandas</code></td><td>≥ 1.3</td><td>Veri okuma ve işleme</td></tr>
      <tr><td><code>numpy</code></td><td>≥ 1.21</td><td>Sayısal hesaplamalar</td></tr>
      <tr><td><code>matplotlib</code></td><td>≥ 3.4</td><td>Görselleştirme</td></tr>
      <tr><td><code>seaborn</code></td><td>≥ 0.11</td><td>İstatistiksel görselleştirme</td></tr>
      <tr><td><code>scikit-learn</code></td><td>≥ 0.24</td><td>Makine öğrenmesi modelleri ve metrikler</td></tr>
    </tbody>
  </table>

  <div class="footer">
    Bu proje eğitim amaçlı hazırlanmıştır. Klinik tanı aracı olarak kullanılmamalıdır.
  </div>

</div>
</body>
</html>
