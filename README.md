# Customer Segmentation using Hybrid Metaheuristic Clustering

Proyek ini mengimplementasikan analisis segmentasi pelanggan dengan membandingkan algoritma clustering standar dan metode optimasi metaheuristik hybrid. Metode yang dibandingkan meliputi **K-Means standar**, **Harmony Search (HS)**, **Ant Colony Optimization (ACO)**, serta pendekatan hybrid **HS + K-Means** dan **ACO + K-Means** untuk mengelompokkan pelanggan berdasarkan perilaku transaksi dan profil demografis.

---

## 📌 Daftar Isi

1. [Deskripsi Proyek](#-deskripsi-proyek)
2. [Dataset & Rekayasa Fitur](#-dataset--rekayasa-fitur)
3. [Metode Clustering](#-metode-clustering)
4. [Hasil Eksperimen](#-hasil-eksperimen-k2)
5. [Analisis Uji Statistik](#-analisis-uji-statistik)
6. [Kesimpulan](#-kesimpulan)

---

## 📖 Deskripsi Proyek

Segmentasi pelanggan (*Customer Segmentation*) merupakan proses penting untuk memahami karakteristik pelanggan dan menentukan strategi pemasaran yang lebih terarah.

K-Means merupakan salah satu algoritma clustering yang banyak digunakan, tetapi hasil pengelompokannya dapat dipengaruhi oleh pemilihan centroid awal. Untuk mengatasi keterbatasan tersebut, proyek ini menerapkan algoritma metaheuristik **Harmony Search (HS)** dan **Ant Colony Optimization (ACO)** untuk mengoptimalkan centroid awal K-Means.

Penelitian ini bertujuan untuk membandingkan kualitas hasil clustering berdasarkan metrik evaluasi internal dan menguji perbedaan performa antarmetode secara statistik.

---

## 📊 Dataset & Rekayasa Fitur

Dataset yang digunakan adalah `customer_segmentation.csv`, yang berisi informasi profil dan perilaku pembelian pelanggan retail.

### Tahapan Preprocessing

1. **Data Cleaning**
   - Menangani data tahun lahir yang tidak wajar (`Year_Birth < 1920`).
   - Menyatukan kategori status pernikahan (`Marital_Status`).
   - Menghapus baris dengan nilai `Income` yang kosong.

2. **Feature Engineering**

   Menghasilkan enam fitur representatif untuk proses clustering:

   - `Age`: Umur pelanggan.
   - `Tenure_Days`: Lama waktu menjadi pelanggan dalam hari.
   - `Income`: Pendapatan pelanggan.
   - `Total_Spend`: Total pengeluaran untuk seluruh kategori produk.
   - `Total_Purchases`: Total jumlah pembelian.
   - `Total_Children`: Jumlah anak dan remaja di rumah.

3. **Feature Scaling**

   Melakukan standarisasi fitur menggunakan **Z-score Normalization** dengan `StandardScaler` agar perbedaan skala antarfitur tidak mendominasi perhitungan jarak.

---

## 🛠️ Metode Clustering

Eksperimen dilakukan sebanyak **30 kali pengulangan independen** untuk setiap metode dan jumlah klaster yang diuji, yaitu \(k = 2\) hingga \(k = 5\).

Metode yang dibandingkan meliputi:

- **K-Means**: Algoritma clustering dengan inisialisasi centroid secara acak.
- **Harmony Search Clustering**: Penerapan algoritma Harmony Search untuk mencari solusi clustering.
- **Ant Colony Optimization Clustering**: Penerapan algoritma ACO untuk mencari solusi clustering.
- **Hybrid HS + K-Means**: Harmony Search digunakan untuk mengoptimalkan centroid awal sebelum proses clustering K-Means.
- **Hybrid ACO + K-Means**: Ant Colony Optimization digunakan untuk mengoptimalkan centroid awal sebelum proses clustering K-Means.

### Metrik Evaluasi

- **Sum of Squared Errors (SSE)**: Mengukur jumlah kuadrat jarak data terhadap centroid klasternya. Nilai yang lebih rendah menunjukkan klaster yang lebih rapat.
- **Silhouette Coefficient (SC)**: Mengukur kualitas pemisahan dan kekompakan klaster. Nilai yang lebih tinggi menunjukkan hasil clustering yang lebih baik berdasarkan metrik ini.

---

## 📈 Hasil Eksperimen (k = 2)

Berdasarkan hasil evaluasi rata-rata dari 30 pengulangan pada jumlah klaster \(k = 2\), diperoleh hasil berikut.

| Metrik Evaluasi | K-Means | Hybrid HS + K-Means | Hybrid ACO + K-Means |
|:---|---:|---:|---:|
| **Rata-rata SSE** | 8.680,84 | 12.403,66 | 11.712,75 |
| **Rata-rata Silhouette Coefficient (SC)** | 0,3422 | **0,8338** | 0,7441 |

### Interpretasi Hasil

- **Silhouette Coefficient tertinggi** diperoleh oleh Hybrid HS + K-Means sebesar **0,8338**, diikuti Hybrid ACO + K-Means sebesar 0,7441.
- K-Means menghasilkan rata-rata SSE terendah, yaitu 8.680,84, sedangkan Hybrid ACO + K-Means menghasilkan SSE sebesar 11.712,75 dan Hybrid HS + K-Means sebesar 12.403,66.
- Hasil tersebut menunjukkan bahwa Hybrid HS + K-Means menghasilkan pemisahan klaster terbaik berdasarkan rata-rata Silhouette Coefficient, tetapi tidak menghasilkan SSE terendah.

---

## 🧪 Analisis Uji Statistik

Pengujian statistik dilakukan untuk mengevaluasi apakah terdapat perbedaan performa yang signifikan di antara metode clustering. Pengujian menggunakan **Friedman Test** sebagai uji keseluruhan, dilanjutkan dengan **Wilcoxon Signed-Rank Test** untuk perbandingan berpasangan.

Taraf signifikansi yang digunakan adalah **5% (\(\alpha = 0,05\))**.

### 1. Uji Signifikansi Sum of Squared Error (SSE)

**Friedman Test**
- P-value: `0.000000`
- Keputusan: **Terdapat perbedaan yang signifikan** di antara metode yang diuji.

**Wilcoxon Post-hoc**

| Perbandingan Metode | Hasil |
|:---|:---|
| K-Means vs. HS + K-Means | Signifikan |
| K-Means vs. ACO + K-Means | Signifikan |
| HS + K-Means vs. ACO + K-Means | Tidak signifikan |

### 2. Uji Signifikansi Silhouette Coefficient (SC)

**Friedman Test**
- P-value: `0.000000`
- Keputusan: **Terdapat perbedaan yang signifikan** di antara metode yang diuji.

**Wilcoxon Post-hoc**

| Perbandingan Metode | Hasil |
|:---|:---|
| K-Means vs. HS + K-Means | Signifikan |
| K-Means vs. ACO + K-Means | Signifikan |
| HS + K-Means vs. ACO + K-Means | Tidak signifikan |

**Catatan:** Hasil uji statistik perlu diinterpretasikan berdasarkan metrik yang diuji. Perbedaan signifikan pada Friedman Test menunjukkan bahwa setidaknya terdapat satu perbedaan antarmetode, sedangkan hasil Wilcoxon menunjukkan pasangan metode yang berbeda secara signifikan.

---

## 📝 Kesimpulan

1. Penerapan metode metaheuristik melalui pendekatan hybrid HS + K-Means dan ACO + K-Means menghasilkan rata-rata Silhouette Coefficient yang lebih tinggi dibandingkan K-Means standar pada eksperimen dengan \(k = 2\).

2. Hybrid HS + K-Means memperoleh rata-rata Silhouette Coefficient tertinggi sebesar **0,8338**, sedangkan Hybrid ACO + K-Means memperoleh 0,7441 dan K-Means standar memperoleh 0,3422.

3. Berdasarkan rata-rata SSE, K-Means standar menghasilkan nilai terendah sebesar 8.680,84. Dengan demikian, kualitas clustering tidak dapat dinilai hanya berdasarkan satu metrik, karena SSE dan Silhouette Coefficient mengukur aspek yang berbeda.

4. Hasil uji statistik menunjukkan adanya perbedaan performa secara keseluruhan antarmetode. Berdasarkan hasil post-hoc yang dilaporkan, perbedaan antara HS + K-Means dan ACO + K-Means tidak signifikan pada SSE maupun Silhouette Coefficient.

---

## 💻 Teknologi yang Digunakan

- Python
- Pandas
- NumPy
- Scikit-learn
- SciPy
- Google Colab / Jupyter Notebook

