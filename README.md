# Prediksi Klasifikasi pH Tanah Berdasarkan Indikator Kimiawi Tanah
## Latar belakang
Tanah memiliki peran penting dalam ekosistem, di mana pH tanah bertindak sebagai variabel yang mengendalikan ketersediaan unsur hara dan produktivitas tanaman. Namun, metode analisis tanah konvensional seringkali terkendala oleh waktu, biaya, dan skalabilitas yang menghambat penerapan pertanian presisi. Mengingat kompleksitas interaksi antara sifat fisik dan kimia tanah—seperti kandungan bahan organik, kapasitas tukar kation (CEC), dan rasio unsur hara—analisa berbasis data diperlukan untuk memahami pola tersebut. Project ini menggunakan machine learning untuk memprediksi klasifikasi pH tanah secara efisien, sehingga dapat mendukung pengambilan keputusan agronomis yang lebih cepat dan berkelanjutan.
## Pernyataan Masalah
Masalah yang ingin kita selesaikan adalah bagaimana cara kita mengatasi inefisiensi pengukuran manual pH tanah dengan mengembangkan model klasifikasi machine learning yang mampu memprediksi kondisi pH secara akurat berdasarkan indikator kimiawi di dalam tanah.
## Alur Kerja (Workflow)
### 1. Data Preprocessing & Feature Engineering
Beberapa fitur baru yang diciptakan:
- base_cations: Total jumlah ion-ion yang bersifat Basa (alkali) yang ada di dalam tanah.
- base_saturations: Persen kapasitas penyimpanan (CEC) tanah yang diisi oleh ion basa.
- ca_sat_ratio: Persentase kapasitas tukar kation (CEC) yang ditempati oleh Kalsium.
- mg_sat_ratio: Persentase kapasitas tukar kation (CEC) yang ditempati oleh Magnesium.
- k_sat_ratio: Persentase kapasitas tukar kation (CEC) yang ditempati oleh Kalium.
- ca_mg_ratio: Indikator untuk menilai keseimbangan fisik tanah.
- mg_k_ratio: Mengidentifikasi persaingan nutrisi antara Magnesium dan Kalium.
- om_clay_ratio: Menilai kemampuan tanah dalam menahan nutrisi dan air lebih banyak bersumber dari bahan organik (humus) atau dari mineral liat.
- sand_clay_ratio: Menggambarkan drainase tanah dengan membandingkan jumlah pasir yang meloloskan air dengan liat yang menahan air.
- silt_clay_ratio: Melengkapi profil tekstur tanah dengan melihat perbandingan partikel debu (silt) terhadap liat untuk klasifikasi jenis tanah yang lebih detail.
- n_p_ratio: Melihat keseimbangan hara Nitrogen dan Fosfor untuk mendeteksi potensi pengasaman tanah yang sering terjadi akibat proses penguraian Nitrogen yang tinggi.
- p_k_ratio: Mengukur keseimbangan antara nutrisi untuk akar (Fosfor) dan kualitas/ketahanan tanaman (Kalium) guna menilai kesuburan menyeluruh.
### 2. Penanganan Imbalance Data
Dataset menunjukkan adanya ketidakseimbangan kelas pada distribusi target yang terdiri dari 5 kategori pH tanah. teknik yang digunakan:
- StratifiedKfold: Membagi data latih & validasi dengan mengunci rasio setiap kelas agar tetap proporsional.
### 3. Hyperparameter Tuning
teknik yang digunakan:
- GridSearchCV (Grid Search Cross Validation): Menemukan konfigurasi pengaturan terbaik untuk memaksimalkan akurasi model.
### 4. Model Machine Learning
Beberapa algoritma yang digunakan agar mendapatkan hasil yang terbaik:
- Random Forest: Mengandalkan voting mayoritas dari seluruh pohon untuk menentukan hasil, sehingga model sangat stabil dan tahan terhadap overfitting.
- XGBoost: Menggunakan teknik Gradient Boosting yang dioptimalkan dengan regularisasi tambahan.
- LightGBM: untuk efisiensi memori dan kecepatan pelatihan yang jauh lebih tinggi dibandingkan algoritma lain tanpa mengorbankan akurasi.
- CatBoost: Memberikan hasil prediksi yang sangat akurat dan stabil, serta mampu menangani fitur kategorikal secara otomatis.
## Hasil dan Insight
### Fitur paling berpengaruh (Feature Importance)
Berdasarkan algoritma CatBoost, fitur-fitur yang cukup mempengaruhi nilai pH adalah:
1. ca_sat_ratio: Jika rasio tinggi, tempat untuk Hidrogen menjadi sempit sehingga tanah pasti bersifat Netral atau Basa.
2. ca: Semakin banyak jumlah atom Kalsium yang terdeteksi, semakin kuat kemampuan tanah melawan perubahan pH menuju asam
3. p: jika nilai p tinggi, dapat dikatakan bahwa pH tanah berada di level Netral.
4. ca_mg_ratio: Rasio ini mendeteksi tingkat pelapukan dan masalah drainase tanah, di mana nilai yang rendah menjadi sinyal kuat bahwa tanah tersebut sudah tua atau sering tergenang air yang memicu keasaman.
5. base_saturation: mengukur persentase kapasitas tanah yang diisi oleh ion basa, di mana nilainya berbanding lurus secara langsung dengan tinggi-rendahnya pH tanah.
### Performa Model
Algoritma CatBoost mendapatkan Akurasi tertinggi mencapai 74%. model dievaluasi menggunakan Classification Report.

