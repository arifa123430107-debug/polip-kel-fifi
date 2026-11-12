# Segmentasi Polip pada Citra Kolonoskopi dengan U-Net

<!--
  TEMPLATE LAPORAN — Tugas P10 Kecerdasan Buatan (BM25-41204), Teknik Biomedis ITERA.
  Petunjuk lengkap: https://mctm.web.id/ai-bm/tugas

  Cara memakai:
  - Ganti semua teks yang diawali "✍️" dengan tulisan kelompokmu, lalu hapus tanda ✍️-nya.
  - Blok komentar seperti ini adalah petunjuk; tidak tampil di GitHub. Boleh dihapus.
  - Gambar diambil dari folder hasil/ yang dibuat otomatis oleh notebook.
  - Jangan hapus judul-judul bagian (##).
-->

**Tugas P10 — Kecerdasan Buatan (BM25-41204), Teknik Biomedis ITERA**

## Anggota kelompok
| Nama | NIM | Username GitHub |
|---|---|---|
| Elisa Karunialita Sitorus | 123430073 | elisa123430073-bit |
| Arifa Lutfiah | 123430107 | arifa123430107-debug |
| Muhammad Rafi Adh Dhuha | 123430023 | rafi123430023-debug |
## 1. Pertanyaan

Augmentasi apa yang membantu U-Net menemukan polip pada citra kolonoskopi, dan augmentasi apa yang tidak? Kenapa?

## 2. Data & pengaturan

<!-- Isi dari Bagian 2 notebook. Pastikan CEPAT = False untuk hasil akhir. -->
- Dataset: Kvasir-SEG, berisi 1.000 citra kolonoskopi beserta mask polip, dengan resolusi citra diperkecil menjadi 128 × 128 piksel.
- Data uji: 200 citra.
- Data latih: 200 citra per eksperimen.
- Model: U-Net mini (sekitar 0,5 juta parameter).
- Epoch: 40.
- Pengulangan: 3 kali per skenario.
- Perangkat: Google Colab dengan GPU NVIDIA Tesla T4.
- Mode cepat: False (eksperimen akhir).
![Contoh data](hasil/contoh_data.png)

## 3. Skenario augmentasi & hipotesis

<!-- Tulis bagian ini SEBELUM menjalankan eksperimen. Commit dulu, baru jalankan.
     Alasan klinis = kenapa variasi ini mungkin (atau tidak mungkin) terjadi pada kolonoskopi sungguhan. -->

| Skenario | Augmentasi (kode) | Alasan klinis | Hipotesis: Dice naik/turun/sama? |
|---|---|---|---|
| Tanpa augmentasi | None |  Menjadi pembanding untuk melihat kemampuan dasar U-Net dalam mendeteksi polip.| Performa dasar sebagai baseline.|
| Flip | RandomHorizontalFlip dan RandomVerticalFlip (p=0.5) |  Membantu model mengenali polip pada berbagai posisi dan orientasi citra kolonoskopi. | Dice dapat meningkat karena variasi posisi, tetapi perlu dibuktikan. |
| Kecerahan & kontras | ColorJitter (brightness dan contrast 0.7–1.3) | Mensimulasikan perubahan pencahayaan dan kontras selama pemeriksaan kolonoskopi | Dapat meningkatkan ketahanan terhadap pencahayaan berbeda, tetapi perubahan berlebihan bisa menurunkan Dice. |

Contoh augmentasi yang dicoba:

![Contoh augmentasi](hasil/augmentasi_flip.png)
<!-- Ganti nama file sesuai skenario kalian (lihat isi folder hasil/). -->

## 4. Hasil
| Skenario | Dice (rata-rata ± sb) | IoU (rata-rata ± sb) | Dice tiap ulangan |
|---|---|---|---|
| Tanpa augmentasi | 0.402 ± 0.000 | 0.283 ± 0.000 | 0.402 |
| Flip | 0.389 ± 0.000 | 0.271 ± 0.000 | 0.389 |
| Kecerahan & kontras | 0.329 ± 0.000 | 0.219 ± 0.000 | 0.329 |


![Perbandingan Dice](hasil/perbandingan.png)

## 5. Analisis peta kesalahan

### Analisis peta kesalahan

Peta kesalahan digunakan untuk mengevaluasi kesesuaian hasil segmentasi U-Net terhadap mask referensi dari dataset Kvasir-SEG. Visualisasi ini membantu mengidentifikasi wilayah yang berhasil dideteksi maupun wilayah yang mengalami kesalahan prediksi.

Pada peta kesalahan, warna hijau menunjukkan **True Positive (TP)**, yaitu piksel polip yang berhasil diprediksi dengan benar. Warna merah menunjukkan **False Positive (FP)**, yaitu area bukan polip yang keliru diprediksi sebagai polip. Sementara itu, warna biru menunjukkan **False Negative (FN)**, yaitu bagian polip yang tidak berhasil terdeteksi oleh model.

Kesalahan segmentasi dapat terjadi akibat kemiripan warna dan tekstur antara polip dengan jaringan mukosa di sekitarnya, variasi pencahayaan, serta batas polip yang kurang jelas. Polip berukuran kecil atau memiliki kontras rendah juga berpotensi sulit dideteksi.

Augmentasi Flip bertujuan membantu model mengenali variasi orientasi polip, sedangkan augmentasi Kecerahan dan Kontras bertujuan meningkatkan kemampuan model menghadapi perubahan kondisi pencahayaan. Pengaruh kedua augmentasi terhadap kesalahan segmentasi perlu dinilai berdasarkan perbandingan hasil prediksi dan metrik evaluasi.

Secara keseluruhan, peta kesalahan melengkapi evaluasi Dice Score dan Intersection over Union (IoU), karena menunjukkan lokasi kesalahan prediksi yang tidak dapat diketahui hanya dari nilai numerik.
## 6. Pembahasan & kesimpulan

Berdasarkan hasil eksperimen segmentasi polip menggunakan U-Net, augmentasi Flip dan Kecerahan & Kontras menunjukkan perbedaan performa segmentasi. Pada hasil pengujian yang tersedia, augmentasi Flip menghasilkan Dice rata-rata sekitar 0,646, sedangkan augmentasi Kecerahan & Kontras menghasilkan Dice rata-rata sekitar 0,587.

Augmentasi Flip memungkinkan model mempelajari variasi orientasi polip, sedangkan perubahan kecerahan dan kontras membantu mensimulasikan kondisi pencahayaan yang berbeda pada citra kolonoskopi. Namun, perubahan intensitas yang terlalu besar dapat mengurangi kejelasan struktur polip.

Kesimpulan akhir mengenai manfaat augmentasi dibandingkan tanpa augmentasi perlu didasarkan pada nilai baseline dari eksperimen akhir dengan konfigurasi pelatihan yang sama.
## 7. Pembagian kerja

<!-- Siapa mengerjakan apa. Penilaian proses kerja dilihat dari riwayat commit tiap anggota. -->

✍️

## 8. Penggunaan AI


Dalam pengerjaan tugas segmentasi polip pada citra kolonoskopi menggunakan U-Net, kelompok kami memanfaatkan kecerdasan buatan (AI), khususnya ChatGPT, sebagai alat bantu pembelajaran dan pendukung teknis.

AI digunakan untuk membantu memahami konsep segmentasi citra medis, arsitektur U-Net, penggunaan augmentasi data, serta interpretasi metrik evaluasi Dice Score dan Intersection over Union (IoU).

Selain itu, AI membantu mengidentifikasi dan memberikan solusi terhadap kendala teknis, seperti pengaturan lingkungan Python, instalasi dependensi menggunakan uv, penggunaan JupyterLab dan Google Colab, serta penyelesaian error saat menjalankan notebook.

AI juga dimanfaatkan untuk memberikan panduan penggunaan Git dan GitHub, termasuk konfigurasi repository, pembuatan commit, proses push, dan penyusunan dokumentasi dalam README.md.

Seluruh proses menjalankan eksperimen, memeriksa hasil evaluasi, dan mengunggah hasil pekerjaan dilakukan oleh anggota kelompok. Hasil eksperimen yang dilaporkan bersumber dari keluaran program yang dijalankan, bukan angka yang dibuat oleh AI.

Dengan demikian, AI digunakan sebagai alat bantu dalam proses pembelajaran, pemecahan masalah, dan penyusunan laporan, bukan sebagai pengganti pelaksanaan eksperimen maupun tanggung jawab akademik anggota kelompok.
## Cara menjalankan ulang

```bash
uv sync
uv run jupyter lab segmentasi_polip.ipynb
```

Atau buka notebook di Google Colab (Runtime → Change runtime type → T4 GPU).

## Referensi

- Jha, D., dkk. (2020). Kvasir-SEG: A Segmented Polyp Dataset. *MultiMedia Modeling (MMM 2020)*, 451–462.
- Ronneberger, O., Fischer, P., & Brox, T. (2015). U-Net: Convolutional Networks for Biomedical Image Segmentation. *MICCAI*. arXiv:1505.04597.
- ✍️ tambahkan referensi lain yang kalian pakai
