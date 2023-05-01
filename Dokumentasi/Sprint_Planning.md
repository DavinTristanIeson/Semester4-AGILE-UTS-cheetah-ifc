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
- Diagram tabel yang berhubungan dengan respon [1]

2. Sebagai pembuat poll, saya ingin dapat mengajukan pertanyaan secara real-time untuk semua partisipan agar dapat menerima tanggapan secara real-time.
- Membuat tampilan halaman *Poll Editor* di Figma [3]
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

**Total Story Point**: 35

# Sprint 3 (01/05/2023 s/d 02/05/2023)

### Sprint Goal
- Menghubungkan interaksi antara penjawab dan pembuat survey/poll
- Membuat fitur-fitur yang berhubungan dengan pemilik akun
- Menyempurnakan *mockup*

### Sprint Backlog
1. Sebagai pembuat survey, saya ingin bisa set survey menjadi private/public agar mengontrol siapa yang dapat mengakses survei
- *Popup* yang berisi fungsionalitas untuk mengatur akses ke *survey* [2]
    - Tombol *Share* untuk membuka *popup*
    - Tampilan link untuk di-*copy* atau di-*randomize* jika mau link lain

2. Sebagai user, saya ingin bisa share survey/poll ke orang lain melalui link agar mereka bisa mengisi survey atau berpartisipasi dalam poll
- Endpoint untuk *randomize* link dari survey/poll tertentu [1]
    - Randomize link survey/poll
    - Randomize link hasil survey/poll
- Endpoint untuk mencari dan mengembalikan survey yang diasosiasikan dengan link yang diberikan [1]
- Endpoint untuk menambahkan partisipan ke poll yang diasosiasikan dengan link yang diberikan [2]
- Tabel *poll_participants* untuk menyimpan partisipan dari suatu poll [1]

3. Sebagai user, saya ingin dapat mencari survey publik untuk diisi
- Membuat endpoint untuk mendapatkan daftar survey publik [2]
- Membuat halaman pencarian survey yang memungkinkan pengguna untuk melakukan pencarian berdasarkan kata kunci [3]
- Menampilkan hasil pencarian pada daftar yang bisa diklik untuk mengisi survey. [2]

4. Sebagai user dengan akun, saya ingin melihat semua survey/poll yg pernah saya buat agar saya dapat melakukan pengeditan pada survey, atau melihat hasil dari survey/poll.
- Membuat endpoint untuk mendapatkan daftar survey/poll yang pernah dibuat oleh pengguna [2]
- Membuat halaman untuk menampilkan survey/poll yang pernah dibuat pengguna. [2]
    - Jika poll belum di-end maka akan ditampilkan statusnya sebagai Ongoing
    - Jika poll sudah end maka statusnya Ended
- Menampilkan daftar survey yang berisi informasi penting seperti judul, tanggal pembuatan, dan status [1]

5. Sebagai pembuat survey/poll, saya ingin memperbolehkan orang lain untuk melihat hasil survey/poll dan membaginya melalui link, misalnya untuk presentasi hasil survey kepada pemangku kepentingan.
- Memberikan akses kepada pembuat survey/poll untuk share link untuk membagikan hasil [2]
    - Tombol share untuk membuka popup
    - Pengaturan dalam popup seperti mengatur apakah partisipan poll/survey boleh melihat hasil, dan link untuk membagikan hasil
- Update backend untuk menyimpan link hasil [2]
    - Update skema database untuk menyimpan link hasil dari survey/poll
    - Update endpoint agar mengakomodasikan link hasil

6. Sebagai user, saya ingin dapat mengedit informasi akun saya dan menghapus akun saya
- Membuat halaman profil [3]
    - Popup konfirmasi hapus akun
- Endpoint untuk modifikasi akun [3]
    - Edit informasi akun
    - Hapus akun

7. dan sebagainya
    - Endpoint untuk mengambil tampilan/*frontend* dari aplikasi [1]
    - Kontrol akses informasi dari endpoint, contohnya ketika mengambil info survey, pemilik akan mendapat semua info tapi penjawab tidak akan mendapat link dan link hasil karena bukan untuk dilihat mereka [2]

**Total Story Point**: 32