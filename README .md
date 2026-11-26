# Heart Disease Classification Models

Klasifikasi biner untuk memprediksi ada atau tidaknya penyakit jantung dari 13 atribut klinis pasien. Proyek ini membandingkan tiga model, yaitu Logistic Regression, Decision Tree, dan Random Forest, pada data 294 pasien.

Seluruh kode ada di satu notebook: `heart-disease-classification-models.ipynb`.

## Dataset

Dataset Heart Disease dalam format UCI ([UCI Machine Learning Repository](https://archive.ics.uci.edu/dataset/45/heart+disease)): 294 pasien, 13 fitur, dan 1 kolom target `num` (0 = tanpa penyakit jantung, 1 = ada penyakit jantung). File data tidak disertakan di repo ini. Notebook membaca file `data.csv`.

Fitur: usia (`age`), jenis kelamin (`sex`), jenis nyeri dada (`cp`), tekanan darah istirahat (`trestbps`), kolesterol (`chol`), gula darah puasa (`fbs`), hasil EKG istirahat (`restecg`), detak jantung maksimum (`thalach`), angina akibat olahraga (`exang`), ST depression (`oldpeak`), kemiringan segmen ST (`slope`), jumlah pembuluh darah utama (`ca`), dan `thal`.

Nilai hilang ditandai `?` di data mentah. Jumlahnya per kolom:

| Kolom | Nilai hilang | Persentase |
|---|---|---|
| `ca` | 291 | 99,0% |
| `thal` | 266 | 90,5% |
| `slope` | 190 | 64,6% |
| `chol` | 23 | 7,8% |
| `fbs` | 8 | 2,7% |
| `trestbps`, `restecg`, `thalach`, `exang` | 1 per kolom | 0,3% |

## Metode

1. **Pembersihan:** tanda `?` diganti `NaN`, lalu semua kolom dikonversi ke numerik.
2. **Imputasi:** nilai kosong diisi dengan median tiap kolom (`SimpleImputer`).
3. **Split:** 80% train (235 pasien) dan 20% test (59 pasien), stratifikasi berdasarkan target, `random_state=42`.
4. **Model:**
   - Logistic Regression dengan `StandardScaler` (`max_iter=1000`)
   - Decision Tree (parameter bawaan)
   - Random Forest (100 pohon)
5. **Evaluasi:** accuracy, classification report, confusion matrix, dan F1-score kelas 1 (ada penyakit jantung).

## Hasil

| Model | Accuracy | Precision kelas 1 | Recall kelas 1 | F1 kelas 1 |
|---|---|---|---|---|
| Random Forest | 0,8644 | 0,81 | 0,81 | 0,8095 |
| Logistic Regression | 0,8305 | 0,79 | 0,71 | 0,7500 |
| Decision Tree | 0,7458 | 0,62 | 0,71 | 0,6667 |

Data uji berisi 38 pasien kelas 0 dan 21 pasien kelas 1.

Temuan utama:

- Random Forest memberi hasil terbaik di semua metrik. Model ini mengenali 17 dari 21 pasien dengan penyakit jantung, sedangkan Logistic Regression dan Decision Tree masing-masing 15 dari 21.
- Selisih Random Forest dan Logistic Regression hanya 2 pasien dari 59 (51 vs 49 prediksi benar).
- Decision Tree tunggal paling rendah di accuracy, precision, dan F1.

## Keterbatasan

- Data kecil. Satu pasien di data uji setara 1,7 poin akurasi, jadi selisih antar model mudah berubah pada split yang berbeda.
- Evaluasi memakai satu kali split, tanpa cross-validation dan tanpa tuning hyperparameter.
- Imputasi median dijalankan sebelum split, sehingga nilai median ikut dihitung dari data uji (data leakage kecil).
- Kolom `ca`, `thal`, dan `slope` sebagian besar kosong. Setelah imputasi median, `ca` dan `thal` praktis menjadi konstanta dan hampir tidak membawa informasi.
- Metrik berfokus pada accuracy dan F1 kelas 1. Notebook belum menghitung ROC-AUC.
- Proyek ini latihan pemodelan, bukan alat diagnosis medis.

## Cara menjalankan

1. Unduh data dan simpan sebagai `data.csv`.
2. Buka notebook di Google Colab atau Jupyter, lalu ubah path `/content/data.csv` sesuai lokasi file.
3. Jalankan semua sel berurutan. Library yang dipakai: pandas, NumPy, Matplotlib, Seaborn, scikit-learn.

## Struktur repo

```
heart-disease-classification-models/
├── heart-disease-classification-models.ipynb
└── README.md
```
