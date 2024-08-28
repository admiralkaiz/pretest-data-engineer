# soal untuk pretest-data-engineer
Penjawab: Kaisar Kertarajasa

## Knowledge Base
1. Apa yang Anda ketahui tentang Data Engineer ?
2. Ceritakan pengalaman Anda dalam mengolah data?
3. Apa yang kamu ketahui tentang AI
4. Mengapa Data menjadi sesuatu yang sangat penting sekarang, dan apa dampak dari kebocoran data
5. Apa yg anda ketahui mengenai model generative AI ? da apa saja penerapannya

Jawaban:
1. Data Engineer merupakan pekerjaan yang bertugas melakukan pengelolaan terhadap data dari data mentah ke dalam format data yang lebih bersih, lebih rapi, dan lebih mudah dianalisa. Data Engineer melakukan inspeksi terhadap derau (noise) yang terdapat pada data, seperti missing values, duplicated values, dan pencilan (outlier). Derau tersebut kemudian dibersihkan dari dataset. Selain itu, data engineer juga dapat melakukan transformasi data dengan mengaplikasikan fungsi tertentu dan mengubah data ke dalam format yang lebih mudah dibaca.
    
2. Pengalaman saya dalam mengolah data dapat ditarik mundur hingga ke masa SD saya, yang mana di masa sekolah tersebut saya diajarkan untuk mencari nilai rerata, median, dan modus dari data numerik yang disajikan. Tidak hanya itu, saya juga diajarkan cara akuisisi data (kala itu menggunakan metode penjajakan) dan visualisasi data dalam berbagai diagram. Berlanjut ke masa SMP saya bertemu dengan teknik pengolahan data yang lebih beragam lagi, seperti mengetahui persebaran data dengan jarak antarkuartil. Di masa SMA, lebih banyak lagi yang saya pelajari terkait pengolahan data, termasuk mencari pola pemusatan data (mean, median, dan modus) dan pola persebaran data (variansi, standar deviasi, kuantil, persentil) serta distribusi data  (distribusi normal). Karena saya saat SMA mengikuti OSN Astronomi hingga ke tahap nasional, di situ saya juga belajar mengelola data-data terkait astronomi, yang melibatkan aplikasi komputer dan perhitungan yang lebih kompleks. Saat beralih dari masa SMA ke kuliah, saya mengikuti kursus untuk sains data dengan diperkenalkannya bahasa R, Python, dan SQL yang banyak digunakan di bidang data. Ilmu yang saya dapatkan sebelumnya menjadi bekal saya dalam kuliah di bidang informatika ini. Di masa kuliah inilah ketertarikan saya dalam bidang data semakin meningkat, dengan mempelajari cara mengomunikasikan data secara benar, hingga merancang berbagai model kecerdasan buatan. Pada saat mempelajari pengenalan kecerdasan buatan, di sinilah saya mempelajari lebih mendalam library Python yang lebih canggih dalam pengolahan data seperti NumPy dan Pandas, lalu menggunakan library tersebut dalam melakukan prapemrosesan data, pembersihan data, normalisasi data, hingga merancang model seperti k-NN dan Naive Bayes. Selain data berupa tabel, saya juga pernah merancang model dalam mengolah data tak terstruktur seperti data teks dan data gambar. Dalam hal pengolahan data gambar, saya pernah membuat model pembelajaran mesin yang melibatkan visi komputer dalam klasifikasi varietas tumbuhan, dengan memperoleh nilai akurasi yang cukup tinggi menggunakan convolutional neural network dengan augmentasi data dan regularisasi.

3. AI atau kecerdasan buatan adalah program komputer yang memungkinkan komputer untuk dapat berpikir dan bernalar sebagaimana manusia berpikir dalam rangka memecahkan masalah, pengambilan keputusan, dan menemukan pola dari data.

4. Data merupakan aset krusial bagi seseorang maupun perusahaan. Data sensitif tidak boleh jatuh ke tangan yang salah agar menghindari penyalahgunaan data. Maka dari itu, data harus dilindungi dari kebocoran data. Kebocoran data dapat mengakibatkan kerusakan dan/atau kehilangan data, penyalahgunaan data (termasuk aksi penipuan), menurunnya reputasi perusahaan dan tingkat kepercayaan pelanggan, serta kehilangan privasi bagi individu.

5. Generative AI adalah model kecerdasan buatan yang mampu melakukan prediksi dari proses pembelajaran (training),yang menerima input tertentu untuk menghasilkan keluaran, seperti dalam bentuk teks, audio, gambar, bahkan video. Salah satu penerapan generative AI yang banyak digunakan adalah chatbot AI seperti ChatGPT, atau chatbot yang jauh lebih lawas lagi seperti Eliza. Selain itu, penerapan generative AI juga digunakan dalam menghasilkan musik. Generative AI juga digunakan dalam menghasilkan lukisan dan gambar dari teks, seperti pada StableDiffusion  dan DALL-E. OpenAI juga telah menghasilkan model AI yang dapat menghasilkan video dari teks, yang bernama Sora. Teknologi Generative AI menuai kontroversi dari bebagai pihak, seperti para pekerja seni. Oleh karena itu, teknologi ini harus digunakan secara bijak dan ethical.

## Soal Coding
studi kasus = 
seorang data engineer diharuskan oleh klien untuk membuat
blueprint data orchestration di sebuah tools yaitu Kestra.
klien ingin memproses data review lazada (review-lazada.csv).

kami sudah menyediakan akses kestra di
[Kestra](https://kestra-magang.t-dev.site/) - https://kestra-magang.t-dev.site/ 
silahkan pelajari kestra lihat dokumentasinya 
dan silahkan lakukan proses dibawah ini:

1. satu flows untuk proses data cleansing dimana bersihkan data reviewnya kemudian export ke csv
2. satu flows untuk proses menghitung sentiment analysis berdasarkan kolom rating

Keterangan :
flow kestra dan script python sertakan dalam git kalian ketika pelaporan

Jawaban :
1. Flow:
   ```yaml
    id: data_cleaning
    namespace: data_wrangling
   
    tasks:
      - id: pandas_cleanse
        type: io.kestra.plugin.scripts.python.Script
        beforeCommands:
          - pip install pandas
        outputFiles: 
          - review-lazada-cleaned.csv
        script: |
          import pandas as pd
          url = "https://raw.githubusercontent.com/admiralkaiz/pretest-data-engineer/main/review-lazada.csv"
          df = pd.read_csv(url)
          df.dropna(axis=0, how='all', inplace=True)
          df.dropna(axis=1, how='all', inplace=True)
          df['reviewTitle'].fillna('-', inplace=True)
          df['reviewContent'].fillna('-', inplace=True)
          df['boughtDate'].fillna('-', inplace=True)
          df.to_csv("review-lazada-cleaned.csv")
   ```

2. Flow:
   ```yaml
   id: rating_analysis
   namespace: data_wrangling
   
   tasks:
     - id: pandas_rate
       type: io.kestra.plugin.scripts.python.Script
       beforeCommands:
         - pip install pandas
       outputFiles: 
         - rating-count.csv
       script: |
         import pandas as pd
         url = "https://raw.githubusercontent.com/admiralkaiz/pretest-data-engineer/main/review-lazada.csv"
         df = pd.read_csv(url)
         df.dropna(axis=0, how='all', inplace=True)
         df.dropna(axis=1, how='all', inplace=True)
         df['reviewTitle'].fillna('-', inplace=True)
         df['reviewContent'].fillna('-', inplace=True)
         df['boughtDate'].fillna('-', inplace=True)
         pd.DataFrame(df['rating'].value_counts()).sort_values(by='rating').to_csv("rating-count.csv")
   ```
