# Case-Based Reasoning — Pemalsuan Surat (Pasal 263 & 266 KUHP)

Sistem CBR untuk retrieval dan klasifikasi putusan pidana pemalsuan surat
dari Pengadilan Negeri Jakarta Utara menggunakan TF-IDF, IndoBERT, dan SVM.

## Struktur Direktori

    CBR_Pemalsuan/
    ├── data/
    │   ├── raw/              # Full text putusan
    │   ├── amar/             # Teks amar putusan
    │   ├── processed/
    │   │   └── cases.csv     # Dataset final
    │   ├── eval/             # train_ids.csv, test_ids.csv, queries.json
    │   └── results/          # predictions.csv, metrics
    ├── models/               # Model tersimpan (dibuat saat eksekusi)
    └── notebooks/
        ├── 01_case_base.ipynb
        ├── 02_case_representation.ipynb
        ├── 03_case_retrieval.ipynb
        ├── 04_solution_reuse.ipynb
        └── 05_evaluation.ipynb

## Persyaratan

- Python 3.13.7
- Jupyter Notebook

## Instalasi

```bash
git clone https://github.com/LeiXuanz1/Case-Based-Reasoning.git
cd Case-Based-Reasoning
pip install -r requirements.txt
```

## Konfigurasi Path

Setiap notebook memiliki variabel `BASE_DIR` di bagian atas. Sesuaikan dengan
lokasi folder `CBR_Pemalsuan` di komputer kamu:

```python
BASE_DIR = r'C:\Users\NamaUser\path\ke\CBR_Pemalsuan'
```

## Eksekusi

Jalankan notebook secara berurutan:

| Tahap | Notebook | Deskripsi |
|-------|----------|-----------|
| 1 | `01_case_base.ipynb` | Load dan parsing dokumen putusan |
| 2 | `02_case_representation.ipynb` | Preprocessing dan ekstraksi fitur |
| 3 | `03_case_retrieval.ipynb` | Training TF-IDF + SVM, embedding IndoBERT |
| 4 | `04_solution_reuse.ipynb` | Prediksi outcome kasus baru |
| 5 | `05_evaluation.ipynb` | Evaluasi retrieval dan klasifikasi |

> **Penting:** Notebook 03 harus selesai dijalankan sebelum 04 dan 05,
> karena model yang dihasilkan dibutuhkan oleh tahap selanjutnya.

## Model yang Dihasilkan

Setelah notebook 03 selesai, folder `models/` akan berisi:

    models/
    ├── svm_pipeline.pkl
    ├── tfidf_retriever.pkl
    ├── train_tfidf_matrix.pkl
    ├── train_embeddings.npy
    ├── train_df.pkl
    ├── test_df.pkl
    └── indobert/

## Label Klasifikasi

| Label | Deskripsi |
|-------|-----------|
| `pasal 263 ayat (1)` | Membuat atau memalsukan surat |
| `pasal 263 ayat (2)` | Memakai surat palsu |
| `pasal 266` | Pemalsuan akta otentik (ayat 1 & 2 digabung) |
| `bebas` | Terdakwa dibebaskan |

## Dataset

- 36 putusan pidana, Pengadilan Negeri Jakarta Utara
- Split: 28 train / 8 test (80/20, stratified)
- Pasal dakwaan: 263 dan 266 KUHP

## Keterbatasan

- Ukuran dataset kecil (n=36) — metrik evaluasi bersifat indikatif
- `pasal_terbukti` adalah inferensi dari frasa amar, bukan ekstraksi pasal eksplisit
- IndoBERT tidak di-fine-tune pada domain hukum Indonesia
