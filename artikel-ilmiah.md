# **Implementasi Arsitektur ECAPA-TDNN untuk Verifikasi Pembicara**

### Penulis
    Amrina Rossada*, Ardhi Iwantara Saputra*, Abellia Putri Dwi Masita*, Wiranto*, Alex Anandha Romadhona*, Reno Fahrezi Purnomo*

## **Pendahuluan**
Dalam beberapa tahun terakhir, teknologi verifikasi pembicara telah mengalami pertumbuhan pesat dan menemukan berbagai aplikasi luas, seiring dengan meningkatnya kebutuhan akan sistem autentikasi suara yang aman dan efisien [(Yao dkk, 2023)][1]. Menurut laporan [DataIntelo][2], pasar verifikasi pembicara global diperkirakan akan meningkat dari USD 1,8 miliar pada tahun 2023 menjadi USD 4,2 miliar pada tahun 2032, dengan tingkat pertumbuhan tahunan gabungan (CAGR) sebesar 10,5% selama periode tersebut. Pertumbuhan ini menunjukkan pentingnya teknologi verifikasi pembicara dalam berbagai sektor, mulai dari keamanan digital hingga layanan pelanggan.

Meskipun demikian, teknologi verifikasi pembicara masih menghadapi berbagai tantangan teknis yang signifikan, terutama terkait akurasi, keamanan, dan skalabilitas sistem. Salah satu tantangan utama adalah ketahanan terhadap variasi akustik, khususnya dalam kondisi _far-field_ atau lingkungan dengan tingkat kebisingan tinggi. Studi pada dataset FFSVC2022 menunjukkan bahwa performa sistem verifikasi menurun secara signifikan akibat gangguan seperti suara latar, gema (_reverberation_), dan penurunan energi suara karena jarak [(Zheng dkk, 2022)][3]. Selain itu, _Spoofing Attacks_ juga terus menjadi ancaman, di mana rekaman sintetis atau replayed voice dapat menipu sistem. Studi ASV spoof 2021 menekankan pentingnya deteksi deepfake yang lebih _robust_, terutama ketika data pelatihan tidak mencakup variasi serangan yang diantisipasi [(Yamagishi dkk, 2021)][4].

Secara alami, setiap individu memiliki karakteristik suara unik yang dapat diamati pada spektrum suara. Namun, mengidentifikasi seseorang hanya melalui suara mereka merupakan suatu tantangan yang besar, terutama ketika terdapat gangguan seperti kebisingan, gema, atau variasi emosi saat berbicara. Di samping itu, teknik pengenalan pembicara yang konvensional seperti Model Campuran _Gaussian_ cenderung tidak tepat saat data pelatihan terbatas atau ketika kualitas audio kurang baik. Penelitian menunjukkan bahwa sistem verifikasi tradisional GMM-UBM (_Gaussian Mixture Model - Universal Background Model_) rentan terhadap efek gema. Meskipun pelatihan i-vector dapat membantu, namun performanya tetap menurun dalam kondisi akustik yang bervariasi [(Avila dkk, 2020)][5]. Hal ini menjadikan pengembangan sistem verifikasi pembicara yang tangguh dan adaptif sebagai suatu kebutuhan mendesak.

Untuk mengatasi permasalahan ini, pendekatan yang berfokus pada deep learning seperti ECAPA-TDNN (Jaringan Syaraf Waktu Tunda dengan Perhatian Saluran yang Ditekankan, Propagasi, dan Agregasi) menjadi pilihan yang cerah. Model ECAPA-TDNN, yang dikembangkan oleh [(Desplanques dkk)][6], tahun 2020, dirancang untuk meningkatkan ekstraksi fitur suara dengan menggunakan modul Res2Net 1-dimensi dan _Squeeze-and-Excitation_ (SE) untuk memodelkan ketergantungan antar kanal. Dengan menggabungkan fitur hierarkis melalui agregasi dan propagasi, serta meningkatkan statistics pooling dengan channel-dependent frame attention. Dalam penelitian tersebut, ECAPA-TDNN telah terbukti menghasilkan representasi suara yang kuat dan tahan terhadap variasi akustik. Pendekatan ini terbukti unggul dalam identifikasi dan verifikasi pembicara di berbagai kondisi akustik.

Penerapan model ECAPA-TDNN telah banyak diteliti dan digunakan dalam berbagai studi. [Furui (1981)][7] menjadi orang pertama yang memperkenalkan penggunaan fitur dinamis untuk meningkatkan performa pengenalan pembicara. Selanjutnya, penelitian berbasis [i-vector][8] dan [x-vector][9] pernah menjadi standar utama, namun kinerjanya melemah pada segmen audio yang pendek. ECAPA-TDNN muncul sebagai perbaikan dari pendekatan x-vector yang memiliki kemampuan untuk menangkap konteks temporal yang lebih luas serta fitur frekuensi yang lebih rinci. Penelitian oleh [Loweimi dkk, tahun 2024][10] tersebut menunjukkan bahwa ECAPA-TDNN mampu meraih tingkat akurasi yang tinggi dalam berbagai tugas pengenalan pembicara dan kini telah menjadi salah satu arsitektur yang diakui di bidang ini.

Penelitian ini bertujuan untuk mengimplementasikan model speaker recognition berbasis ECAPA-TDNN yang mampu mengenali identitas pembicara secara akurat, bahkan dalam kondisi nyata suara yang tidak ideal. Model yang diimplementasikan akan diuji pada dataset seperti _VoxCeleb_ 1 guna memverifikasi akurasi dan ketahanannya.


[1]: http://www.xiaolei-zhang.net/papers/Yao%20et%20al.%20-%202023%20-%20Branch-ECAPA-TDNN%20A%20Parallel%20Branch%20Architecture%20to%20Capture%20Local%20and%20Global%20Features%20for%20Speaker%20Verification%20(2).pdf

[2]: https://dataintelo.com/report/global-speaker-verification-market

[3]: https://www.isca-archive.org/ffsvc_2022/zheng22_ffsvc.pdf

[4]: https://arxiv.org/pdf/2109.00537

[5]: https://musaelab.ca/pdfs/C73.pdf

[6]: https://www.isca-archive.org/interspeech_2020/desplanques20_interspeech.pdf  

[7]: https://ieeexplore.ieee.org/document/1163530

[8]: https://www.researchgate.net/publication/230643046_i-vector_Based_Speaker_Recognition_on_Short_Utterances

[9]: https://www.isca-archive.org/interspeech_2019/kanagasundaram19_interspeech.pdf 

[10]: https://www.isca-archive.org/interspeech_2024/loweimi24_interspeech.pdf 


## **Metode**
Arsitektur ECAPA-TDNN yang diimplementasikan, diadopsi dari penelitian oleh [(Desplanques dkk, 2020)][6]. Arsitektur memberi perhatian pada karakteristik pembicara yang tidak selalu aktif pada waktu yang sama. Dalam hal ini _Self-Attention_ digunakan untuk memperhatikan saluran yang relevan dan mengabaikan yang tidak relevan.

<div style="display: flex; flex-direction: column; align-items: center; justify-content: center; text-align: center;">
  <img src="https://i.imgur.com/B0D9Vu3.png" alt="Rumus Self Attention" style="max-width: 100%; height: auto;" />
  <p>Gambar 1. Rumus Mekanisme <em>Self Attention</em></p>
</div>

Dimana:

- \( h_t \) adalah aktivasi pada frame ke-\( t \),
- \( W \) dan \( b \) adalah parameter yang digunakan untuk memproyeksikan informasi ke ruang berdimensi lebih kecil,
- \( v_c \) adalah vektor yang digunakan untuk menghitung perhatian pada saluran ke-\( c \),
- \( k_c \) adalah bias untuk saluran ke-\( c \).

Kemudian, skor perhatian
e_{t,c} dinormalisasi dengan softmax untuk mendapatkan pentingnya setiap frame pada saluran tersebut:

<div style="display: flex; flex-direction: column; align-items: center; justify-content: center; text-align: center;">
  <img src="https://i.imgur.com/jPpCM1c.png" alt="Fungsi Softmax " style="max-width: 100%; height: auto;" />
  <p>Gambar 2. Rumus Fungsi <em>Softmax</em></p>
</div>

Dimana 
𝛼_{𝑡,𝑐} adalah skor perhatian yang menunjukkan pentingnya setiap frame untuk saluran ke-c.

Selanjutnya, statistik berbobot dihitung dengan menggunakan skor perhatian 𝛼_{𝑡,𝑐} untuk mendapatkan rata-rata tertimbang dan deviasi standar tertimbang untuk setiap saluran:

Rata-rata tertimbang saluran ke-c :

<div style="display: flex; flex-direction: column; align-items: center; justify-content: center; text-align: center;">
  <img src="https://i.imgur.com/iMV6BeO.png" alt="Rumus Rata-Rata Tertimbang " style="max-width: 100%; height: auto;" />
  <p>Gambar 3. Rumus Rata-Rata Tertimbang</p>
</div>

Deviasi standar tertimbang saluran ke-c :

<div style="display: flex; flex-direction: column; align-items: center; justify-content: center; text-align: center;">
  <img src="https://i.imgur.com/1IaAUC6.png" alt="Rumus Standar Deviasi " style="max-width: 100%; height: auto;" />
  <p>Gambar 4. Rumus Standar Deviasi Tertimbang</p>
</div>

Output dari pooling layer adalah gabungan dari rata-rata tertimbang dan deviasi standar untuk semua saluran.

Penggunaan Squeeze-Excitation (SE) Blocks untuk merescaling fitur frame-level berdasarkan konteks global dari rekaman. Langkah pertama dalam SE-block adalah operasi squeeze, yang menghitung deskriptor untuk setiap saluran dengan mengambil rata-rata fitur sepanjang waktu:

<div style="display: flex; flex-direction: column; align-items: center; justify-content: center; text-align: center;">
  <img src="https://i.imgur.com/4fvldMD.png" alt="Rumus Rata-Rata Fitur " style="max-width: 100%; height: auto;" />
  <p>Gambar 5. Rumus Rata-Rata Fitur</p>
</div>

Deskriptor 𝑧 ini kemudian digunakan untuk operasi excitation, yang menghitung bobot untuk setiap saluran:

<div style="display: flex; flex-direction: column; align-items: center; justify-content: center; text-align: center;">
  <img src="https://i.imgur.com/y8JvFMG.png" alt="Rumus Menghitung Bobot Saluran " style="max-width: 100%; height: auto;" />
  <p>Gambar 6. Rumus Menghitung Bobot Saluran</p>
</div>

Dimana:
- \( W_1 \) dan \( W_2 \) adalah matriks berat untuk lapisan bottleneck,
- \( f \) adalah fungsi non-linearitas (biasanya ReLU),
- \( \sigma \) adalah fungsi sigmoid yang menghasilkan vektor bobot \( s \) antara 0 dan 1.

Bobot 𝑠 yang dihasilkan dari proses Squeeze-Excitation (SE) diterapkan ke fitur saluran h_c melalui perkalian saluran-wise:
<!-- 
$$
\tilde{h}_c = s_c h_c
$$ (7) -->

<div style="display: flex; flex-direction: column; align-items: center; justify-content: center; text-align: center;">
  <img src="https://i.imgur.com/QIDqfBc.png" alt="Rumus Perkalian Saluran-Wise" style="max-width: 100%; height: auto;" />
  <p>Gambar 7. Rumus  perkalian Saluran-Wise</p>
</div>


Output dari seluruh SE-Res2Block dikombinasikan melalui proses Multi-layer Feature Aggregation (MFA), yaitu dengan menggabungkan (concatenate) semua peta fitur dari tiap SE-Res2Block Gambaran umum keseluruhan arsitektur ditunjukkan pada gambar 1 dibawah : 

<div style="display: flex; flex-direction: column; align-items: center; justify-content: center; text-align: center;">
  <img src="https://i.imgur.com/OZG2n6U.jpeg" alt="Arsitektur ECAPA-TDNN" style="max-width: 100%; height: auto;" />
  <p>Gambar 8. Arsitektur ECAPA-TDNN (Sumber: Desplanques dkk, 2020) </p>
</div>

Fitur input yang digunakan dalam model ini memiliki dimensi 80 kolom dan T baris, di mana T merujuk pada panjang waktu atau urutan data yang dapat bervariasi. Langkah pertama adalah penerapan lapisan konvolusi 1D dengan ukuran kernel 5 dan langkah (stride) 1 pada input. Setelah proses konvolusi, fungsi aktivasi ReLU digunakan untuk memberikan non-linearitas pada data, diikuti dengan penerapan Batch Normalization (BN) untuk menstabilkan data dan mempercepat proses pelatihan.

Model ini selanjutnya menggunakan dua lapisan **SE-Res2Block** dengan kernel ukuran 3 dan langkah yang berbeda (d=2 dan d=3). Blok residual ini menggunakan teknik **Squeeze-and-Excitation (SE)** untuk meningkatkan pemahaman terhadap fitur penting dalam data. Proses konvolusi dengan ukuran kernel 3 dan langkah yang berbeda ini secara bertahap mengurangi dimensi data sepanjang perjalanan model.

Setelah itu, lapisan **Conv1D + ReLU (k=1, d=1)** diterapkan, di mana konvolusi 1D dengan ukuran kernel 1 dan langkah 1 digunakan untuk mereduksi dimensi fitur lebih lanjut, memungkinkan model untuk mempelajari fitur-fitur penting dengan lebih mendalam.

Kemudian, **Attentive Stat Pooling** diterapkan untuk menggabungkan fitur berdasarkan perhatian, memberikan bobot lebih pada fitur yang lebih relevan. Di sini juga digunakan Batch Normalization untuk memastikan distribusi data tetap stabil selama pelatihan.

Model ini dilanjutkan dengan lapisan **Fully Connected (FC)** yang menghasilkan output dari fitur yang telah diproses, diikuti dengan Batch Normalization untuk meningkatkan stabilitas dan kualitas pelatihan. Setelah itu, digunakan teknik **AAM-Softmax** untuk klasifikasi, yang memodelkan distribusi kelas dalam ruang fitur untuk menghasilkan prediksi kelas yang lebih akurat.

Output akhir model berupa vektor berukuran **S × 1**, yang berisi hasil prediksi dari model, baik untuk klasifikasi maupun regresi. Model ini menggabungkan berbagai teknik untuk memproses data urutan dan menghasilkan output yang tepat sesuai dengan tugas yang diinginkan.


[6]: https://www.isca-archive.org/interspeech_2020/desplanques20_interspeech.pdf 

## **Metodologi**
xxx

## **Daftar Pustaka**
Avila, Anderson R., et al. “Improving the Performance of Far-Field Speaker Verification Using Multi-Condition Training: The Case of GMM-UBM and i-vector Systems.” https://musaelab.ca/pdfs/C73.pdf. Accessed 14 Mei 2025.

Desplanques, Brecht, et al. “ECAPA-TDNN: Emphasized Channel Attention, Propagation and Aggregation in TDNN Based Speaker Verification.” INTERSPEECH 2020, 2020, pp. 3830-3834, https://www.isca-archive.org/interspeech_2020/desplanques20_interspeech.pdf. Accessed 14 Mei 2025.

Kanagasundaram, Ahlian, et al. “i-vector Based Speaker Recognition on Short Utterances.” INTERSPEECH 2011, 2011, pp. 2341-2344, researchgate.net/publication/230643046_i-vector_Based_Speaker_Recognition_on_Short_Utterances. Accessed 14 Mei 2025.

Kanagasurandaram, A., et al. “A Study of X-vector Based Speaker Recognition on Short Utterances.” INTERSPEECH 2019, 2019, pp. 1943-1947, https://www.isca-archive.org/interspeech_2019/kanagasundaram19_interspeech.pdf. Accessed 14 Mei 2025.

Loweimi, Erfan, et al. “On the Usefulness of Speaker Embeddings for Speaker Retrieval in the Wild: A Comparative Study of x-vector and ECAPA-TDNN Models.” INTERSPEECH 2024, 2024, pp. 3774-3778, https://www.isca-archive.org/interspeech_2024/loweimi24_interspeech.pdf. Accessed 14 Mei 2025.

S. Furui. “Cepstral analysis technique for automatic speaker verification.” IEEE Transactions on Acoustics, Speech, and Signal Processing, vol. 29, no. 2, 1981, pp. 254-272, https://ieeexplore.ieee.org/document/1163530. Accessed 14 Mei 2025.

Sharma, Raksha, et al. “Speaker Verification Market.” DATAINELO, https://dataintelo.com/, https://dataintelo.com/report/global-speaker-verification-market. Accessed 14 Mei 2025.

Yamagishi, Junichi, et al. “ASVspoof 2021: accelerating progress in spoofed and deepfake speech detection.” arxiv.org/pdf/2109.00537. Accessed 14 Mei 2025.

Yao, Jiadi, et al. “Branch-ECAPA-TDNN: A Parallel Branch Architecture to Capture Local and Global Features for Speaker Verification.” INTERSPEECH, 2023, pp. 1943-1947, https://www.isca-archive.org/interspeech_2023/yao23_interspeech.pdf. Accessed 14 Mei 2025.

Zheng, Yu, et al. “The SpeakIn Speaker Verification System for Far-Field Speaker Verification Challenge 2022.” The 2022 Far-field Speaker Verification Challenge (FFSVC2022), 2022, pp. 15-19, https://www.isca-archive.org/ffsvc_2022/zheng22_ffsvc.pdf. Accessed 14 Mei 2025.
