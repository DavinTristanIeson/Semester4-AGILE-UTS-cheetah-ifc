# Legenda

Cookie: Isi dari *Cookie* yang harus disertakan dengan request

Set Cookie: Isi dari *Cookie* apa yang diberikan *server* ke *client*

Request: Tubuh atau *payload* yang datang beserta request

Request Files: *Files* yang di-*upload* bersama dengan *request*

Response: Jawaban yang dikembalikan *server* ke *client*

<*Field*>(<*type*>?): *Field* yang optional

<*Field*>(<*type*>[]): *Field* dengan tipe *array*

# Endpoints

### POST /api/accounts/register
Endpoint ini digunakan untuk membuat akun baru dalam sistem.

Request (multipart/form-data):
- email (string)
- password (string)
- name (string)
- description (string)?

Request Files:
- pfp?: File gambar profil pengguna (Profil Picture).

Response:
- 201 Created: Menunjukkan bahwa akun baru berhasil dibuat dan data pengguna telah dimasukkan ke dalam database.
    - id (number)
    - email (string)
    - name (string)
    - description (string)
    - pfp (string): Link ke *Profile Picture* dari pengguna
- 400 Bad Request: Menunjukkan bahwa format data yang dikirim dalam permintaan tidak sesuai dengan yang diharapkan. Pesan kesalahan harus menjelaskan masalah yang spesifik, seperti format yang salah atau kolom yang hilang.
    - message: "Expected fields: email, password, name, description"
- 409 Conflict: Menunjukkan bahwa alamat email yang digunakan untuk membuat akun baru sudah terdaftar dalam sistem. Pengguna harus menggunakan email yang unik.
    - message: "Email already in use"

Set Cookie (JIKA 201 Created):
- anonymous_id (number): ID yg bersifat untuk mengidentifikasi pengguna selama cookie tidak dihapus, digunakan untuk menyimpan respon survey dan poll pengguna. Jika pengguna sudah ada cookie dengan anonymous_id, maka tidak di-override.
- user_id (number): ID dari pengguna

### POST /api/accounts/login
Endpoint ini digunakan untuk melakukan login dan melakukan otentikasi dengan menggunakan cookie.

Cookie:
- user_id

Request:
- email (string)
- password (string)

Response:
- 200 OK: Menunjukkan bahwa otentikasi berhasil dan mengatur cookie sesi untuk pengguna.
- 401 Unauthorized: Menunjukkan bahwa otentikasi gagal karena kredensial pengguna tidak valid atau tidak ditemukan.
    - message: "Email or password is wrong"
- 400 Bad Request: Menunjukkan bahwa format data yang dikirim dalam permintaan tidak sesuai dengan yang diharapkan. Pesan kesalahan harus     menjelaskan masalah yang spesifik, seperti format yang salah atau kolom yang hilang.
    - message: "Expected fields: email, password"

Set Cookie (JIKA 200 OK):
- anonymous_id (number): ID yg bersifat untuk mengidentifikasi pengguna selama cookie tidak dihapus, digunakan untuk menyimpan respon survey dan poll pengguna. Jika pengguna sudah ada cookie dengan anonymous_id, maka tidak di-override.
- user_id (number): ID dari pengguna

### GET /api/accounts/me
Endpoint ini digunakan mengecek apakah pengguna terotentikasi.

Cookie:
- user_id (number)

Response:
- 200 OK: Pengguna terotentikasi
- 401 Unauthorized: Pengguna tidak terotentikasi

### GET /api/accounts/:id
Endpoint ini digunakan untuk mengambil informasi akun pengguna berdasarkan ID.

Response:
- 200 OK: Menunjukkan bahwa informasi akun pengguna berhasil ditemukan dan dikembalikan.
    - id (number)
    - email (string)
    - name (string)
    - description (string)
    - pfp (string): Link ke *Profile Picture* dari pengguna
- 404 Not Found: Menunjukkan bahwa tidak ada akun pengguna yang ditemukan berdasarkan ID yang diberikan.
    - message: "No accounts found with that ID"

### POST /api/polls
Menambahkan poll baru ke database

Cookie:
- user_id

Request (multipart/form-data):
- title (string)
- description (string)
- is_public (boolean)

Request Files:
- thumbnail?: File gambar untuk poll

Response:
- 200 Success:
    - id (number)
    - creator: Pembuat poll
        - id (number)
        - email (string)
        - name (string)
        - description (string)
        - pfp (string)
    - title (string)
    - description (string)
    - link (string): Invite link
    - start_time (string)
    - end_time (string|null): Kapan poll dihentikan. Jika poll masih berjalan maka nilai ini adalah null.
    - thumbnail (string): URL ke gambar dari poll
- 400 Bad Request: Jika format tidak sesuai
    - message: "Expected fields: title, description, is_public, thumbnail"
- 401 Unauthorized: Jika tidak ada cookie dengan nilai user_id yang valid

### POST /api/surveys
Menambahkan survey baru ke database

Cookie:
- user_id

Request (multipart-formdata):
- title (string)
- description (string)
- close_time (string): Kapan survey secara otomatis ditutup dalam format waktu ISO
- is_public (boolean)
- questions[]
    - type: string
    - question: string
    - required: boolean
    - image_name (string?)

Request Files:
- thumbnail
- question_images

Response:
- 201 Created: Sukses membuat survey baru
    - id (number)
    - creator
        - id (number)
        - email (string)
        - name (string)
        - description (string)
        - pfp (string)
    - title (string)
    - description (string)
    - last_modified (string)
    - is_public (boolean)
    - link (string)
    - close_time (string): Kapan survey terakhir diubah dalam format waktu ISO
    - thumbnail (string)
    - questions[]
        - id (number)
        - type (string)
        - question (string)
        - required (boolean)
        - image? (string)
        - options[]?
            - id (number)
            - label (string)
- 400 Bad Request: Jika format tidak sesuai
    - message: "Expected fields: title, description, close_time, thumbnail, is_public, questions"
- 400 Bad Request: Jika close_time bukan format date ISO
    - message: "close_time must be in ISO Date format"
- 400 Bad Request: Jika objek dalam questions tidak sesuai format
    - message: "Expected fields in an individual question: type, question, required, image_name"
- 400 Bad Request: Jumlah pertanyaan yang disediakan image_name tidak sama dengan jumlah question_images yang disediakan
    - message: "Number of questions is different from number of provided question images"
- 401 Unauthorized: Jika tidak ada cookie dengan nilai user_id yang valid

### PUT /api/polls/:id
Mengedit properti dari poll dengan id yang ditentukan sesuai dengan field yang disediakan.

Cookie:
- user_id

Request (multipart/form-data):
- title (string?)
- description (string?)

Request Files:
- thumbnail?

Response:
- 200 Success: Sukses mengedit poll
- 304 Not Modified: Tidak ada perubahan yang terjadi
- 401 Unauthorized: Jika tidak ada cookie dengan nilai user_id yang valid
- 401 Unauthorized: Jika user_id dalam cookie bukan owner dari poll tersebut

### PUT /api/surveys
Mengedit properti dari survey dengan id yang ditentukan sesuai dengan field yang disediakan.

Cookie:
- user_id

Request (multipart/form-data):
- title (string?)
- description (string?)
- close_time (string?): Kapan survey otomatis ditutup dalam format waktu ISO
- questions (string?)

Request Files:
- thumbnail?
- question_images?

Response:
- 200 Success: Sukses mengedit survey
    - last_modified (string): Kapan survey terakhir diubah dalam format date ISO
- 304 Not Modified: Tidak ada perubahan yang terjadi
- 400 Bad Request: Jika close_time bukan format date ISO
    - message: "close_time must be in ISO Date format"
- 400 Bad Request: Jika objek dalam questions tidak sesuai format
    - message: "Expected fields in an individual question: type, question, required"
- 401 Unauthorized: Jika tidak ada cookie dengan nilai user_id yang valid
