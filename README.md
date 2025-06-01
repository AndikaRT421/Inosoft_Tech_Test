# Amazon Forecast Dataset

## Deskripsi Dataset
Dataset ini berisi data penjualan produk yang diambil dari Amazon. Dataset ini berisi 7 fitur yang berbeda, yaitu:
- `Date`
- `Open`
- `High`
- `Low`
- `Close`
- `Adj Close`
- `Volume`

Adapun insight dari dataset ini adalah:
- **Kondisi Data**:
1. Tidak ada missing value.
2. Terdapat outlier karena lonjakan penjualan di tahun tertentu.
3. Data hanya dicatat pada hari kerja, tidak termasuk akhir pekan.

- **Tren Penjualan**:
1. Penjualan Stagnan hingga 2010, lalu meningkat bertahap hingga 2018 karena perkembangan internet, mobile, dan media sosial.
2. Terjadi penurunan singkat terjadi di 2018–2019, tapi naik lagi di akhir 2019. Lonjakan besar pada 2020 akibat pandemi COVID-19.
3. Penurunan relatif di 2022 karena perubahan perilaku konsumen dan persaingan e-commerce yang meningkat.

## Prerequisite

### A. Membuat Environment pada Anaconda
1. Install Anaconda dari [sini](https://www.anaconda.com/products/distribution).
2. Buka Anaconda Navigator.
3. Buat environment baru (misalnya: `Inosoft_Test`):
   - Klik **Environments** di sidebar kiri.
   - Klik **Create** pada ikon `+` di bawah lalu beri nama environment-nya `Inosoft_Test`.
   - Pilih versi Python (misalnya: `3.12`)
   - Klik **Create** pada pop-up tersebut.

![create_env](/assets/create_env.png)

4. Pastikan Path Anaconda sudah ditambahkan ke dalam `Path` environment variable pada sistem operasi Anda. Jika belum, Anda dapat menambahkannya secara manual:
   - **Windows**: Cari `Edit the system Environment Variables` pada windows search, lalu tambahkan path Anaconda ke dalam `Path`.

   ![edit_path](/assets/edit_path.png)

    - **Linux/MacOS**: Buka terminal dan tambahkan baris berikut ke dalam file `~/.bashrc` atau `~/.bash_profile`:

      ```bash
      export PATH="/path/to/anaconda3/bin:$PATH"
      ```

      Gantilah `/path/to/anaconda3/bin` dengan path sebenarnya dari instalasi Anaconda Anda. Setelah menambahkan path, jalankan perintah berikut untuk memperbarui environment:

      ```bash
      source ~/.bashrc
      ```

### B. Clone Repository dan Install Library
1. Lakukan clone repository pada link [berikut](https://github.com/AndikaRT421/Inosoft_Tech_Test.git)
2. Buka terminal pada directory repository tadi, lalu ganti ke environment `Inosoft_Test` yang telah dibuat:
     
    ```bash
    conda activate Inosoft_Test
    ```

3. Install library yang dibutuhkan:
   - Jalankan perintah berikut:
    ```bash
    pip install -r requirements.txt
    ```

4. Pastikan semua library terinstall dengan baik. Anda dapat memeriksa dengan menjalankan perintah berikut:
    ```bash
    pip list
    ```

5. Pada repository ini menggunakan PyTorch versi 2.4.0 dengan dukungan CUDA 12.4. Untuk dapat menginstall PyTorch, Anda dapat mengunjungi [situs resmi PyTorch](https://pytorch.org/get-started/locally/) untuk mendapatkan instruksi instalasi yang sesuai dengan sistem operasi dan versi CUDA yang Anda gunakan. Selain itu, ada alternatif untuk melakukan instalasi PyTorch dengan menggunakan CPU saja. Berikut ini adalah cara install Pytorch dengan CPU:

    ```bash
    pip3 install torch torchvision torchaudio
    ```
    
    Untuk dapat menjalankan kode ini dengan bantuan GPU NVIDIA (CUDA), Anda dapat menginstall CUDA Toolkit dan cuDNN sesuai dengan versi PyTorch yang Anda gunakan. Pastikan juga driver NVIDIA Anda sudah terinstall dengan benar.

## Menjalankan Kode
1. Buka Code Editor (misalnya: VSCode, PyCharm, atau Jupyter Notebook).
2. Buka file [main.ipynb](main.ipynb) pada Code Editor.
3. Pastikan kernel yang digunakan adalah `Inosoft_Test` yang telah dibuat sebelumnya.
4. Jalankan setiap sel pada notebook tersebut secara berurutan.

> Analisis dan visualisasi data ada pada Jupyter Notebook `main.ipynb`.

## Split Data
- Data training dan validation akan dibagi pada 3 periode waktu, dengan tiap periode berisi 7 tahun. Hal ini dilakukan untuk memastikan model dapat memahami pattern data yang berbeda-beda pada tiap periode.
- Data testing akan berisi data 2-3 tahun terakhir (2021-2023). Ini bagus karena dapat memprediksi tren penjualan pasca pandemi COVID-19.
- Adapun rolling window yang digunakan adalah 30 hari (1 bulan)

## Hasil Evaluasi
Eksperimen ini dilakukan untuk memprediksi nilai `Close` dari data time-series menggunakan pendekatan autoregressive. Beberapa model deep learning digunakan dan dilakukan perbandingan performanya, yaitu:

- LSTM (Long Short-Term Memory)
- Bi-LSTM (Bidirectional Long Short-Term Memory)
- GRU (Gated Recurrent Unit)
- Bi-GRU (Bidirectional Gated Recurrent Unit)

| Model   | MSE         | RMSE       | R² Score   |
| ------- | ----------- | ---------- | ---------- |
| **GRU** | **11.0588** | **3.3255** | **0.9867** |
| Bi-GRU  | 12.6645     | 3.5587     | 0.9848     |
| Bi-LSTM | 16.5685     | 4.0704     | 0.9801     |
| LSTM    | 18.1916     | 4.2652     | 0.9781     |

Berdasarkan hasil evaluasi di atas, model **GRU** memiliki performa terbaik dengan nilai MSE, RMSE, dan R² Score paling optimal.

## Catatan: 

Hasil evaluasi ini dapat berubah-ubah tiap kali Anda menjalankan kode. Hal ini dikarenakan adanya inisialisasi bobot yang acak pada model deep learning.

Namun secara umum, keempat model tersebut dapat memberikan prediksi yang cukup baik pada kisaran:

- **MSE**: 10 - 20
- **RMSE**: 3 - 4
- **R² Score**: 0.97 - 0.98

> Andika Rahman Teja