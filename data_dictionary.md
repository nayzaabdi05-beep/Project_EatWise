# 📖 Data Dictionary — EatWise Final Dataset

**File:** `eatwise_final_dataset.csv`  
**Jumlah Baris:** 993 makanan  
**Jumlah Kolom:** 14  
**Missing Values:** Tidak ada (0 pada semua kolom)

---

## 📋 Daftar Kolom

| No | Nama Kolom | Tipe Data | Deskripsi |
|----|------------|-----------|-----------|
| 1 | `id` | int64 | ID unik setiap makanan (1 – 1346) |
| 2 | `name` | str | Nama makanan dalam Bahasa Indonesia |
| 3 | `calories` | float64 | Kandungan kalori per 100g (kkal), rentang 0 – 940 |
| 4 | `proteins` | float64 | Kandungan protein per 100g (gram) |
| 5 | `fat` | float64 | Kandungan lemak per 100g (gram) |
| 6 | `carbohydrate` | float64 | Kandungan karbohidrat per 100g (gram) |
| 7 | `image` | str | URL gambar makanan (sumber: web publik) |
| 8 | `Cluster` | int64 | Label klaster hasil K-Means (0, 1, atau 2) |
| 9 | `label_nutrisi` | str | Nama kategori klaster dalam bahasa natural |
| 10 | `penjelasan_realistis` | str | Narasi otomatis kondisi nutrisi makanan |
| 11 | `status_makanan` | str | Status konsumsi makanan: `Aman`, `Perhatian`, atau `Waspada` |
| 12 | `status` | str | Tingkat intensitas risiko: `Ringan`, `Sedang`, atau `Berat` |
| 13 | `level_risiko` | str | Level risiko kesehatan: `Aman`, `Perhatian`, atau `Waspada` |
| 14 | `health_score` | int64 | Skor kesehatan makanan (0 – 100, semakin tinggi semakin sehat) |

---

## 🔍 Detail Kolom Kategorikal

### `Cluster`
Hasil segmentasi K-Means berdasarkan kandungan kalori, lemak, dan karbohidrat.

| Nilai | Representasi |
|-------|-------------|
| `0` | Kelompok dominan — makanan dengan profil nutrisi lebih terkontrol |
| `1` | Kelompok lemak dan kalori sedang-tinggi |
| `2` | Kelompok nutrisi terkontrol / rendah kalori |

### `label_nutrisi`
Label deskriptif dari hasil klaster. Nilai yang muncul dalam dataset:

| Nilai | Keterangan |
|-------|------------|
| `Nutrisi Terkontrol` | Kalori, lemak, dan karbo dalam batas wajar |
| `Lemak Tinggi` | Kandungan lemak dominan, kalori moderat |
| `Kalori Tinggi & Lemak Tinggi` | Kalori dan lemak sama-sama tinggi |
| `Karbo Tinggi` | Karbohidrat dominan |
| `Kalori Tinggi & Lemak Tinggi & Karbo Tinggi` | Semua makronutrien tinggi |

### `status_makanan`
Rekomendasi konsumsi berdasarkan profil nutrisi makanan.

| Nilai | Keterangan |
|-------|------------|
| `Aman` | Aman dikonsumsi dalam porsi normal |
| `Perhatian` | Perlu diperhatikan frekuensi dan porsinya |
| `Waspada` | Sebaiknya dibatasi konsumsinya |

### `status`
Tingkat intensitas risiko, merupakan turunan dari profil kalori dan lemak.

| Nilai | Keterangan |
|-------|------------|
| `Ringan` | Risiko rendah, nutrisi masih terkontrol |
| `Sedang` | Risiko menengah, satu atau dua nutrisi melebihi batas wajar |
| `Berat` | Risiko tinggi, kalori dan/atau lemak sangat tinggi |

### `level_risiko`
Level risiko kesehatan jangka panjang jika dikonsumsi berlebihan.

| Nilai | Keterangan |
|-------|------------|
| `Aman` | Tidak berisiko jika dalam batas porsi normal |
| `Perhatian` | Perlu pemantauan asupan |
| `Waspada` | Berisiko melebihi kebutuhan harian jika tidak dikontrol |

---

## 📊 Ringkasan Statistik Kolom Numerik

| Kolom | Min | Rata-rata | Median | Maks |
|-------|-----|-----------|--------|------|
| `calories` | 0 | 202.2 kkal | 146 kkal | 940 kkal |
| `proteins` | 0 | — | — | — |
| `fat` | 0 | — | — | — |
| `carbohydrate` | 0 | — | — | — |
| `health_score` | 0 | 72.2 | 82 | 100 |

---

## 🛠️ Kolom Hasil Feature Engineering

Kolom-kolom berikut **tidak ada di dataset mentah (`nutrition.csv`)** dan dihasilkan melalui proses feature engineering di notebook:

| Kolom | Proses |
|-------|--------|
| `Cluster` | K-Means Clustering berdasarkan `calories`, `fat`, `carbohydrate` |
| `label_nutrisi` | Pemetaan nilai cluster ke label deskriptif |
| `health_score` | Scoring berbasis invers kalori dan lemak, dinormalisasi 0–100 |
| `status_makanan` | Rule-based dari nilai `health_score` |
| `status` | Rule-based dari kandungan kalori dan lemak |
| `level_risiko` | Turunan dari `status_makanan` |
| `penjelasan_realistis` | Teks naratif otomatis berdasarkan kombinasi `label_nutrisi` |
