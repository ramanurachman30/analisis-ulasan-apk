# Analisis Sentimen Ulasan Aplikasi Gojek

Proyek ini bertujuan untuk melakukan analisis sentimen (Positif & Negatif) terhadap ulasan pengguna aplikasi **Gojek** di Google Play Store menggunakan pendekatan *Machine Learning* dan *Natural Language Processing* (NLP) Bahasa Indonesia.

---

## Struktur Proyek

```text
analisis-ulasan-apk/
├── data/
│   ├── ulasan_gojek.csv                # Dataset mentah hasil scraping ulasan Gojek
│   ├── data_step1_no_rating3.csv       # Dataset setelah menghapus rating 3 (netral)
│   ├── data_step2_clean.csv            # Dataset bersih (tanpa NaN & tanpa duplikat)
│   ├── data_step3_final_labeled.csv    # Dataset bersih dengan label sentimen (positif/negatif)
│   └── data_step4_preprocessed.csv     # Dataset hasil 6 tahapan text preprocessing
├── source/
│   ├── cleaning_data.ipynb             # Notebook tahap 1: Pembersihan data awal & duplikat
│   ├── labeling_data.ipynb             # Notebook tahap 2: Pelabelan & distribusi sentimen
│   └── text_processing.ipynb           # Notebook tahap 3: 6 tahapan Text Preprocessing
├── requirements.txt                    # Daftar dependensi library Python
└── README.md                           # Dokumentasi proyek
```

---

## Alur Pemrosesan Data (Pipeline)

### 1. Data Cleaning (`source/cleaning_data.ipynb`)
Tahap pembersihan dataset awal (`ulasan_gojek.csv`):
- **Menghapus Rating 3 (Netral/Ambigu):** Memfokuskan analisis pada klasifikasi biner (Positif vs Negatif).
- **Menghapus Baris Kosong (`NaN`):** Membuang ulasan yang tidak memiliki teks pada kolom `content`.
- **Menghapus Duplikat Identik:** Menyisakan 1 data jika terdapat teks ulasan dan rating yang sama persis.
- **Menghapus Duplikat Ambigu:** Menghapus seluruh baris jika terdapat teks ulasan yang sama persis tetapi memiliki rating yang berbeda.

### 2. Data Labeling (`source/labeling_data.ipynb`)
Tahap pelabelan sentimen berdasarkan skor bintang (`score`) dari `data_step2_clean.csv`:
- **Positif:** Ulasan dengan rating **4 dan 5**.
- **Negatif:** Ulasan dengan rating **1 dan 2**.
- Menghitung jumlah serta persentase distribusi kelas positif dan negatif, lalu menyimpan hasilnya ke `data_step3_final_labeled.csv`.

### 3. Text Preprocessing (`source/text_processing.ipynb`)
Tahap pemrosesan teks ulasan Bahasa Indonesia yang mencakup **6 tahapan**:
1. **Cleaning:** Menghapus URL/tautan, *mention* (`@`), *hashtag* (`#`), angka, tanda baca, emoji/karakter khusus, dan spasi berlebih.
2. **Case Folding:** Mengubah seluruh huruf menjadi huruf kecil (*lowercase*).
3. **Tokenization:** Memecah kalimat menjadi potongan kata tunggal (*token*).
4. **Normalization:** Mengubah kata tidak baku, singkatan, atau bahasa gaul (*slang*) menjadi kata baku Bahasa Indonesia menggunakan kamus normalisasi.
5. **Stopword Removal:** Menghapus kata umum/kata hubung menggunakan daftar *stopword* `Sastrawi` (dengan tetap mempertahankan kata negasi seperti *"tidak"*, *"bukan"*, *"jangan"*, *"belum"*, dan *"kurang"* agar makna sentimen tidak terbalik).
6. **Stemming:** Mengubah kata berimbuhan menjadi kata dasar (*root word*) menggunakan algoritma *Nazief & Adriani* dari library `PySastrawi`.

---

## Cara Instalasi & Penggunaan

### 1. Persiapan Virtual Environment
Aktifkan *virtual environment* yang sudah ada atau buat baru:
```powershell
# Membuat virtual environment (jika belum ada)
python -m venv analisis-venv

# Mengaktifkan virtual environment (Windows PowerShell)
.\analisis-venv\Scripts\Activate.ps1
```

### 2. Instalasi Library
Instal seluruh kebutuhan library dari file `requirements.txt`:
```powershell
pip install -r requirements.txt
```

### 3. Menjalankan Notebook
Buka proyek di VS Code atau Jupyter Notebook, lalu jalankan file di dalam folder `source/` secara berurutan:
1. `source/cleaning_data.ipynb`
2. `source/labeling_data.ipynb`
3. `source/text_processing.ipynb`
