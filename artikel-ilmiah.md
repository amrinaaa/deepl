# **Implementasi Arsitektur ECAPA-TDNN untuk Verifikasi Pembicara**

### Penulis
    Amrina Rossada*, Ardhi Iwantara Saputra*, Abellia Putri Dwi Masita*, Wiranto*, Alex Anandha Romadhona*, Reno Fahrezi Purnomo*

## **Pendahuluan**
Dalam beberapa tahun terakhir, teknologi verifikasi pembicara telah mengalami pertumbuhan pesat dan menemukan berbagai aplikasi luas, seiring dengan meningkatnya kebutuhan akan sistem autentikasi suara yang aman dan efisien [(Yao dkk, 2023)][1]. Menurut laporan [DataIntelo][2], pasar verifikasi pembicara global diperkirakan akan meningkat dari USD 1,8 miliar pada tahun 2023 menjadi USD 4,2 miliar pada tahun 2032, dengan tingkat pertumbuhan tahunan gabungan (CAGR) sebesar 10,5% selama periode tersebut. Pertumbuhan ini menunjukkan pentingnya teknologi verifikasi pembicara dalam berbagai sektor, mulai dari keamanan digital hingga layanan pelanggan.

Meskipun demikian, teknologi verifikasi pembicara masih menghadapi berbagai tantangan teknis yang signifikan, terutama terkait akurasi, keamanan, dan skalabilitas sistem. Salah satu tantangan utama adalah ketahanan terhadap variasi akustik, khususnya dalam kondisi _far-field_ atau lingkungan dengan tingkat kebisingan tinggi. Studi pada dataset FFSVC2022 menunjukkan bahwa performa sistem verifikasi menurun secara signifikan akibat gangguan seperti suara latar, gema (_reverberation_), dan penurunan energi suara karena jarak [(Zheng dkk, 2022)][3]. Selain itu, _Spoofing Attacks_ juga terus menjadi ancaman, di mana rekaman sintetis atau replayed voice dapat menipu sistem. Studi ASV spoof 2021 menekankan pentingnya deteksi deepfake yang lebih _robust_, terutama ketika data pelatihan tidak mencakup variasi serangan yang diantisipasi [(Yamagishi, 2021)][4].

Secara alami, setiap individu memiliki karakteristik suara unik yang dapat diamati pada spektrum suara. Namun, mengidentifikasi seseorang hanya melalui suara mereka merupakan suatu tantangan yang besar, terutama ketika terdapat gangguan seperti kebisingan, gema, atau variasi emosi saat berbicara. Di samping itu, teknik pengenalan pembicara yang konvensional seperti Model Campuran _Gaussian_ cenderung tidak tepat saat data pelatihan terbatas atau ketika kualitas audio kurang baik. Penelitian menunjukkan bahwa sistem verifikasi tradisional GMM-UBM (_Gaussian Mixture Model - Universal Background Model_) rentan terhadap efek gema. Meskipun pelatihan i-vector dapat membantu, namun performanya tetap menurun dalam kondisi akustik yang bervariasi [(Avila, 2020)][5]. Hal ini menjadikan pengembangan sistem verifikasi pembicara yang tangguh dan adaptif sebagai suatu kebutuhan mendesak.

Untuk mengatasi permasalahan ini, pendekatan yang berfokus pada deep learning seperti ECAPA-TDNN (Jaringan Syaraf Waktu Tunda dengan Perhatian Saluran yang Ditekankan, Propagasi, dan Agregasi) menjadi pilihan yang cerah. Model ECAPA-TDNN, yang dikembangkan oleh [(Desplanques dkk)][6], tahun 2020, dirancang untuk meningkatkan ekstraksi fitur suara dengan menggunakan modul Res2Net 1-dimensi dan _Squeeze-and-Excitation_ (SE) untuk memodelkan ketergantungan antar kanal. Dengan menggabungkan fitur hierarkis melalui agregasi dan propagasi, serta meningkatkan statistics pooling dengan channel-dependent frame attention. Dalam penelitian tersebut, ECAPA-TDNN telah terbukti menghasilkan representasi suara yang kuat dan tahan terhadap variasi akustik. Pendekatan ini terbukti unggul dalam identifikasi dan verifikasi pembicara di berbagai kondisi akustik.

Penerapan model ECAPA-TDNN telah banyak diteliti dan digunakan dalam berbagai studi. [Furui (1981)][7] menjadi orang pertama yang memperkenalkan penggunaan fitur dinamis untuk meningkatkan performa pengenalan pembicara. Selanjutnya, penelitian berbasis [i-vector][8] dan [x-vector][9] pernah menjadi standar utama, namun kinerjanya melemah pada segmen audio yang pendek. ECAPA-TDNN muncul sebagai perbaikan dari pendekatan x-vector yang memiliki kemampuan untuk menangkap konteks temporal yang lebih luas serta fitur frekuensi yang lebih rinci. Penelitian oleh [Loweimi tahun 2024][10] tersebut menunjukkan bahwa ECAPA-TDNN mampu meraih tingkat akurasi yang tinggi dalam berbagai tugas pengenalan pembicara dan kini telah menjadi salah satu arsitektur yang diakui di bidang ini.

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
xxx

## **Metodologi**
xxx

## **Daftar Pustaka**
Avila, Anderson R., et al. “Improving the Performance of Far-Field Speaker Verification Using Multi-Condition Training: The Case of GMM-UBM and i-vector Systems.” https://musaelab.ca/pdfs/C73.pdf. Accessed 14 Mei 2025.

Displanques, Brecht, et al. “ECAPA-TDNN: Emphasized Channel Attention, Propagation and Aggregation in TDNN Based Speaker Verification.” INTERSPEECH 2020, 2020, pp. 3830-3834, https://www.isca-archive.org/interspeech_2020/desplanques20_interspeech.pdf. Accessed 14 Mei 2025.

Kanagasundaram, Ahlian, et al. “i-vector Based Speaker Recognition on Short Utterances.” INTERSPEECH 2011, 2011, pp. 2341-2344, researchgate.net/publication/230643046_i-vector_Based_Speaker_Recognition_on_Short_Utterances. Accessed 14 Mei 2025.

Kanagasurandaram, A., et al. “A Study of X-vector Based Speaker Recognition on Short Utterances.” INTERSPEECH 2019, 2019, pp. 1943-1947, https://www.isca-archive.org/interspeech_2019/kanagasundaram19_interspeech.pdf. Accessed 14 Mei 2025.

Loweimi, Erfan, et al. “On the Usefulness of Speaker Embeddings for Speaker Retrieval in the Wild: A Comparative Study of x-vector and ECAPA-TDNN Models.” INTERSPEECH 2024, 2024, pp. 3774-3778, https://www.isca-archive.org/interspeech_2024/loweimi24_interspeech.pdf. Accessed 14 Mei 2025.

S. Furui. “Cepstral analysis technique for automatic speaker verification.” IEEE Transactions on Acoustics, Speech, and Signal Processing, vol. 29, no. 2, 1981, pp. 254-272, https://ieeexplore.ieee.org/document/1163530. Accessed 14 Mei 2025.

Sharma, Raksha, et al. “Speaker Verification Market.” DATAINELO, https://dataintelo.com/, https://dataintelo.com/report/global-speaker-verification-market. Accessed 14 Mei 2025.

Yamagishi, Junichi, et al. “ASVspoof 2021: accelerating progress in spoofed and deepfake speech detection.” arxiv.org/pdf/2109.00537. Accessed 14 Mei 2025.

Yao, Jiadi, et al. “Branch-ECAPA-TDNN: A Parallel Branch Architecture to Capture Local and Global Features for Speaker Verification.” INTERSPEECH, 2023, pp. 1943-1947, https://www.isca-archive.org/interspeech_2023/yao23_interspeech.pdf. Accessed 14 Mei 2025.

Zheng, Yu, et al. “The SpeakIn Speaker Verification System for Far-Field Speaker Verification Challenge 2022.” The 2022 Far-field Speaker Verification Challenge (FFSVC2022), 2022, pp. 15-19, https://www.isca-archive.org/ffsvc_2022/zheng22_ffsvc.pdf. Accessed 14 Mei 2025.
