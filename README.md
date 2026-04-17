
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
      <tr><td><strong>Bağlantı</strong></td><td><a href="https://www.kaggle.com/datasets/imdevskp/corona-virus-report" target="_blank">🔗 Veri Setine Git</a></td></tr>
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

  <h3>Avantajları</h3>
  <ul>
    <li>Yorumlanabilirliği yüksektir</li>
    <li>Az veriyle de iyi çalışır</li>
    <li>Eğitim süresi hızlıdır</li>
  </ul>

  <h3>🌲 2 — Random Forest (Rastgele Orman)</h3>
  <p>
    Random Forest, birden fazla karar ağacının bir araya getirildiği <strong>topluluk öğrenmesi (ensemble learning)</strong> yöntemidir.
    Her ağaç verinin rastgele bir alt kümesinde eğitilir; sonuçlar çoğunluk oylamasıyla birleştirilir.
  </p>

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
