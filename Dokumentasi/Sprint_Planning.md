# Sprint 1 (28/04/2023 s/d 29/04/2023)

### Sprint Goal
- Memberi pengguna kemampuan untuk mendaftarkan akun dan masuk ke aplikasi melalui akun tersebut
- Memberi pengguna kemampuan untuk membuat survey/poll baru

### Sprint Backlog

1. Sebagai user, saya ingin dapat membuat akun dan log in melalui akun saya agar bisa membuat survey/poll
- Membuat tampilan halaman pendaftaran di Figma [3]
    - Tampilan form login
    - Tampilan form register
- Pengguna bisa switch antara halaman login/daftar dengan click link dalam form [1]
    - Tampilan link
    - Prototype Figma untuk menghubungkan kedua state dari halaman pendaftaran
- Mendeskripsikan endpoint API untuk mendaftarkan akun dan login, serta otentikasi [1]
    - Endpoint login
    - Endpoint daftar
    - Endpoint otentikasi
- Membuat diagram untuk tabel akun di Visual Paradigm [1]

2. Sebagai mahasiswa sedang melakukan riset, atau tim marketing melakukan riset pasar, saya ingin memiliki fitur membuat survey untuk mendapat informasi dari kalangan tertentu
- Membuat tombol untuk membuat survey baru di halaman utama, yang dihubungkan ke halaman Create New/Edit Survey [1]
- Membuat tampilan halaman Create New/Edit Survey di Figma [3]
- Membuat komponen-komponen Figma untuk jenis pertanyaan yang di-support [3]
    - Komponen multiple choice
    - Komponen checklist
    - Komponen input teks pendek
    - Komponen input teks panjang
- Mendeskripsikan endpoint API untuk membuat survey baru atau mengedit survey [1]
- Membuat diagram untuk tabel yang berhubungan survey di Visual Paradigm [1]
    - Tabel survey
    - Tabel pertanyaan dalam survey

3. Sebagai dosen, saya ingin membuat poll live dimana mahasiswa dapat berpartisipasi menjawab pertanyaan untuk mendapat informasi tentang jangkauan pengetahuan mereka secara real-time.
- Membuat tombol untuk membuat poll baru di halaman utama, yang dihubungkan ke halaman Create New Poll [1]
- Membuat tampilan halaman Create Poll di Figma [3]
- Mendeskripsikan endpoint API untuk membuat poll baru [1]
    - Endpoint untuk membuat poll baru
    - Endpoint untuk menutup sesi poll
- Membuat diagram untuk tabel yang berhubungan dengan poll di Visual Paradigm [1]
    - Tabel poll
    - Tabel pertanyaan dalam poll

4. Dan sebagainya
- Menentukan tema warna dari aplikasi [1]
- Membuat komponen untuk navbar dari aplikasi [1]
    - Tampilan navbar
    - Prototype untuk menghubungkan navbar ke halaman yang dihubungkan oleh linknya
- Membuat tampilan halaman utama (kosong) di Figma [1]

**Total Story Point**: 24

# Sprint 2 (30/04/2023 s/d 01/05/2023)

### Sprint Goal
- Memberi kemampuan untuk orang mengisi survey dan berpartisipasi dalam poll
- Memberi kemampuan pengguna untuk melihat hasil dari survey dan poll mereka

### Sprint Backlog

1. Sebagai orang yang di-survey, saya ingin bisa mengisi survey yang tersedia seperti survey public ataupun survey private jika saya di-invite.
- Membuat tampilan halaman *Answer Survey* di Figma [3]
- Membuat komponen-komponen untuk jawaban pertanyaan di Figma [3]
- Endpoint untuk mengakses survey tertentu berdasarkan ID [2]
- Endpoint untuk menerima dan menyimpan respon dari pertanyaan [2]
- Diagram tabel yang berhubungan dengan respon [2]

2. Sebagai pembuat poll, saya ingin dapat mengajukan pertanyaan secara real-time untuk semua partisipan agar dapat menerima tanggapan secara real-time.
- Membuat tampilan halaman *Poll Manager* di Figma [3]
- Endpoint untuk menambahkan pertanyaan baru [2]
- Endpoint untuk menghentikan *live-polling* [1]
- Deskripsi komunikasi *real-time* untuk memfasilitasi *live-polling* [2]

3. Sebagai partisipan poll, saya ingin dapat melihat dan menjawab pertanyaan yang diajukan secara real-time.
- Membuat tampilan halaman *Answer Poll* di Figma [3]
- Membuat endpoint untuk menjawab pertanyaan poll. [1]
- Membuat endpoint untuk mendapatkan daftar pertanyaan poll yang tersedia [1]
- Membuat endpoint untuk mendapatkan detail pertanyaan poll berdasarkan ID [1]

4. Sebagai pembuat survey, saya ingin bisa melihat hasil dari survey yang saya buat.
- Membuat tampilan halaman *Survey Result* di Figma [3]
- Membuat endpoint untuk mendapatkan informasi ringkasan survey berdasarkan ID [2]
- Membuat endpoint untuk mendapatkan hasil respon individu dari setiap responden berdasarkan ID [2]

5. Sebagai pembuat poll dan partisipan, saya ingin melihat jawaban poll orang lain secara real-time (jika diperbolehkan oleh pembuat poll).
- Tampilan jawaban orang lain di halaman *Answer Poll* di Figma [2]
- Endpoint untuk mengambil semua jawaban untuk pertanyaan sekarang poll [1]