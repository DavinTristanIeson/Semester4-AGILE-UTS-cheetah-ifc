# Legenda

Cookie: Isi dari _Cookie_ yang harus disertakan dengan request

Set Cookie: Isi dari _Cookie_ apa yang diberikan _server_ ke _client_

Request: Tubuh atau _payload_ yang datang beserta request

Request Files: _Files_ yang di-_upload_ bersama dengan _request_

Response: Jawaban yang dikembalikan _server_ ke _client_

<_Field_>(<_type_>?): _Field_ yang optional

<_Field_>(<_type_>[]): _Field_ dengan tipe _array_

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
  - pfp (string): Link ke _Profile Picture_ dari pengguna
- 400 Bad Request: Menunjukkan bahwa format data yang dikirim dalam permintaan tidak sesuai dengan yang diharapkan. Pesan kesalahan harus menjelaskan masalah yang spesifik, seperti format yang salah atau kolom yang hilang.
  - message: "Expected fields: email, password, name, description"
- 409 Conflict: Menunjukkan bahwa alamat email yang digunakan untuk membuat akun baru sudah terdaftar dalam sistem. Pengguna harus menggunakan email yang unik.
  - message: "Email already in use"

Set Cookie (JIKA 201 Created):

- anonymous_id (string): ID yg bersifat untuk mengidentifikasi pengguna selama cookie tidak dihapus, digunakan untuk menyimpan respon survey dan poll pengguna. Jika pengguna sudah ada cookie dengan anonymous_id, maka tidak di-override.
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
- 400 Bad Request: Menunjukkan bahwa format data yang dikirim dalam permintaan tidak sesuai dengan yang diharapkan. Pesan kesalahan harus menjelaskan masalah yang spesifik, seperti format yang salah atau kolom yang hilang.
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
  - pfp (string): Link ke _Profile Picture_ dari pengguna
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
  - link (string): Link untuk masuk poll
  - result_link (string): Link untuk melihat hasil poll
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
- are_results_open (boolean): Apakah orang lain dapat mengakses hasil survey setelah mengisi survey tanpa diberikan link?
- questions[]
  - type (string)
  - question (string)
  - required (boolean)
  - image_name (string?)
  - options (string[]?)

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
  - link (string): Link mengakses survey
  - result_link (string): Link melihat hasil survey
  - close_time (string): Kapan survey terakhir diubah dalam format waktu ISO
  - thumbnail (string)
  - are_results_open (boolean): Apakah orang lain dapat mengakses hasil survey setelah mengisi survey tanpa diberikan link?
  - questions[]
    - id (number)
    - type (string)
    - question (string)
    - required (boolean)
    - image? (string)
    - options (string[]?)
- 400 Bad Request: Jika format tidak sesuai
  - message: "Expected fields: title, description, close_time, thumbnail, is_public, questions"
- 400 Bad Request: Jika close_time bukan format date ISO
  - message: "close_time must be in ISO Date format"
- 400 Bad Request: Jika objek dalam questions tidak sesuai format
  - message: "Expected fields in an individual question: type, question, required, image_name"
- 400 Bad Request: Jumlah pertanyaan yang disediakan image_name tidak sama dengan jumlah question_images yang disediakan
  - message: "Number of questions is different from number of provided question images"
- 401 Unauthorized: Jika tidak ada cookie dengan nilai user_id yang valid

### GET /api/polls/:id

Mengambil informasi poll tertentu jika sudah berpartisipasi dalam poll atau merupakan pemilik poll

Cookie:
- anonymous_id
- user_id

Response:
- 200 OK
  - id (number)
  - creator
    - id (number)
    - email (string)
    - name (string)
    - description (string)
    - pfp (string)
  - title (string)
  - description (string)
  - link (string?): Hanya ada jika user_id merupakan id creator
  - result_link (string?): Hanya ada jika user_id merupakan id creator
  - start_time (string)
  - end_time (string)
  - thumbnail (string)
  - are_results_open (boolean)
- 404 Not Found: Tidak ada poll dengan ID tersebut
  - message: "No polls found with that ID"
- 401 Unauthorized: Penjawab anonim tidak masuk ke dalam poll atau hasil poll tidak dibuka untuk umum
  - message: "You didn't join the poll." or message: "The results of this poll can only be viewed by its creator."

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

### PUT /api/surveys/:id

Mengedit properti dari survey dengan id yang ditentukan sesuai dengan field yang disediakan.

Cookie:

- user_id

Request (multipart/form-data):

- title (string?)
- description (string?)
- close_time (string?): Kapan survey otomatis ditutup dalam format waktu ISO
- questions (string?)
- is_public (boolean?)
- are_results_open (boolean?): Apakah orang lain dapat mengakses hasil survey setelah mengisi survey tanpa diberikan link?

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

### GET /api/surveys/:id

Mengambil survey dengan ID yang dispesifikasikan di URL

Cookie:

- user_id

Response:

- 200 OK: Survey ditemukan
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
  - link (string?): Hanya ada jika user_id merupakan id creator
  - result_link (string?): Hanya ada jika user_id merupakan id creator
  - close_time (string): Kapan survey terakhir diubah dalam format waktu ISO
  - thumbnail (string)
  - are_results_open (boolean): Apakah orang lain dapat mengakses hasil survey setelah mengisi survey tanpa diberikan link?
  - questions[]
    - id (number)
    - type (string)
    - question (string)
    - required (boolean)
    - image? (string)
    - options (string[]?)
- 404 Not Found: Survey tidak ditemukan
  - message: "No surveys found with that ID"
- 401 Unauthorized: Survey tidak diberikan akses ke publik dan user_id tidak sesuai dengan id pemilik
  - message: "You don't have access to this survey."

### POST /api/surveys/:id/responses

Menambahkan respon ke survey. Jika sudah ada respon sebelumnya dari penjawab anonim tersebut maka akan digantikan dengan respons baru

Cookie:

- anonymous_id? (ID untuk mengidentifikasi penjawab anonim)

Request:

- respondee_alias (string): Nama palsu dari penjawab anonim
- answers[]:
  - type (string)
  - question_id (number)
  - answer (string|string[])

Response:

- 201 Created: Berhasil menyimpan respons
  - are_results_open (boolean): Apakah penjawab dapat melihat hasil atau tidak
  - link (string?): Link untuk melihat hasil jika are_results_open true
- 404 Not Found: Jika tidak ada survey dengan id tersebut
  - message: "No surveys found with that ID"
- 400 Bad Request: Format tidak sesuai
  - message: "Expected fields: respondee_alias, answers"
- 400 Bad Request: Format answers tidak sesuai
  - message: "Expected fields in answers: type, question_id, answer"
- 401 Unauthorized: Penjawab anonim belum masuk ke dalam poll
  - message: "You haven't joined into the poll yet"

Set Cookie (JIKA tidak ada anonymous_id):

- anonymous_id (string)

### POST /api/polls/:id/questions

Menambahkan pertanyaan baru ke poll sesuai ID di URL

Cookie:

- user_id

Request:

- type (string)
- question (string)
- options (string[]?)


Request Files:

- image?

Response:

- 201 Created: Sukses menambah pertanyaan
    - id (number)
    - type (string)
    - question (string)
    - image? (string)
    - options (string[]?)
- 400 Bad Request: Format tidak sesuai
  - message: "Expected fields: type, question, and options if the type is 'checklist' or 'choice'"
- 404 Not Found: Tidak ada poll dengan ID tersebut
  - message: "No surveys found with that ID"
- 401 Unauthorized: Jika user_id tidak sesuai dengan ID pemilik poll

Real-Time Server => Client (Event: "newQuestion")

- poll_id (number)
- question:
    - id (number)
    - type (string)
    - question (string)
    - image? (string)
    - options (string[]?)

### DELETE /api/polls/:id

Menutup poll sesuai dengan ID yang dispesifikasikan URL

Cookie:

- user_id

Response:

- 200 OK: Poll berhasil ditutup
- 404 Not Found: Tidak ada poll dengan ID tersebut
  - message: "No surveys found with that ID"
- 401 Unauthorized: Jika user_id tidak sesuai dengan ID pemilik poll

Real-Time Server => Client (Event: "closePoll")

- poll_id (number)
- are_results_open (boolean)

### POST /api/polls/:id/responses

Menambahkan respon untuk pertanyaan poll berdasarkan ID

Cookie:

- anonymous_id

Request:

- respondee_alias (string)
- type (string)
- question_id (number)
- answer (string|string[])

Response:

- 201 Created: Berhasil menambahkan respon
  - []: Respon dari partisipan lain
    - respondee_alias (string)
    - type (string)
    - answer (string|string[])
- 404 Not Found: Tidak ada poll dengan ID tersebut
  - message: "No polls found with that ID"
- 404 Not Found: Tidak ada pertanyaan dengan ID tersebut
  - message: "No questions found on the poll with that ID"
- 403 Forbidden: Poll sudah ditutup
  - message: "The poll has already ended."
- 400 Bad Request: Format tidak sesuai
  - message: "Expected fields: respondee_alias, type, answer"
- 401 Unauthorized: Penjawab anonim belum masuk ke dalam poll
  - message: "You haven't joined into the poll yet"

Real-Time Server => Client (Event: "newResponse")

- poll_id (number)
- question_id (number)
- respondee_alias (string)
- type (string)
- answer (string|string[])

### GET /api/polls/:id/responses/current

Mengambil semua respons untuk pertanyaan sekarang

Cookie:

- anonymous_id

Response:
- 200 OK
  - id (number)
  - type (string)
  - question (string)
  - respondee_alias (string)
  - answer (string|string[])

- 403 Forbidden: Poll sudah ditutup
  - message: "The poll has already ended."
- 404 Not Found: Tidak ada poll dengan ID tersebut
  - message: "No polls found with that ID"
- 404 Not Found: Belum ada pertanyaan untuk poll
  - message: "This poll doesn't have any questions yet"
- 401 Unauthorized: Penjawab anonim belum masuk ke dalam poll
  - message: "You haven't joined into the poll yet"

### GET /api/surveys/:id/responses

Mengambil ringkasan dari jawaban-jawaban untuk survey, serta rangkaian id dari penjawab. Respondee hanya boleh melihat hasil survey jika are_results_open = true untuk pengaturan survey.

Cookie:

- anonymous_id
- user_id

Response:

- 200 OK
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
  - are_results_open (boolean): Apakah orang lain dapat mengakses hasil survey setelah mengisi survey tanpa diberikan link?
  - responses_count (number)
  - summary[]
    - id (number)
    - type (string)
    - question (string)
    - required (boolean)
    - responses (string[] | { [key:string]: number })?: Jika type adalah text/long maka string[], jika checklist/choice maka yang lain
- 404 Not Found: Tidak ada survey dengan ID tersebut
  - message: "No surveys found with that ID"
- 401 Unauthorized: Penjawab anonim belum menjawab survey
  - message: "You haven't responded to this survey yet."

### GET /api/surveys/:id/responses/:respondee_id

Mengambil respon dari respondee tertentu dari poll dengan ID yang dispesifikasikan. Partisipan hanya boleh melihat hasil jika sudah are_results_open di-set ke true

Cookie:

- anonymous_id
- user_id

Response:

- 200 OK
  - respondee_alias (string)
  - questions[]
    - type (string)
    - question (string)
    - required (boolean)
    - answer (string|string[])
- 404 Not Found: Tidak ada survey dengan ID tersebut
  - message: "No surveys found with that ID"
- 404 Not Found: Tidak ada respons dengan ID respondee tersebut
  - message: "No respondees found with that ID"
- 401 Unauthorized: Penjawab anonim belum menjawab survey
  - message: "You haven't responded to this survey yet."

### GET /api/polls/:id/responses

Mengambil semua respons untuk semua pertanyaan yang ditanya ketika poll. Partisipan hanya boleh melihat hasil jika sudah are_results_open di-set ke true

Cookie:

- anonymous_id
- user_id

Response:

- 200 OK: Otorisasi valid
  - title (string)
  - description (string)
  - creator
    - id (number)
    - email (string)
    - name (string)
    - description (string)
    - pfp (string)
  - start_time (string)
  - end_time (string)
  - thumbnail (string)
  - questions[]
    - id (number)
    - type (string)
    - question (string)
    - respondee_alias (string)
    - answer (string|string[])
- 404 Not Found: Tidak ada poll dengan ID tersebut
  - message: "No polls found with that ID"
- 404 Not Found: Belum ada pertanyaan untuk poll
  - message: "This poll doesn't have any questions yet"
- 401 Unauthorized: Penjawab anonim tidak masuk ke dalam poll atau hasil poll tidak dibuka untuk umum
  - message: "You didn't join the poll." or message: "The results of this poll can only be viewed by its creator."
