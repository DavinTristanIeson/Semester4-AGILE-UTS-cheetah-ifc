### POST /api/surveys/randomize-link/:id

Endpoint yang dapat digunakan untuk mengacak link dari suatu survey/poll tertentu.

Request:

- Params:
  - :id (path parameter): ID survey/poll yang ingin diacak link-nya

Response:

- Status: 200 OK
- Body: { "newLink": "https://example.com/surveys/abcd1234" }

Mengembalikan objek JSON yang berisi link baru yang telah diacak. Link tersebut dapat digunakan untuk mengakses survey/poll yang terkait.

- Status: 404 Not Found
- Body: { "message": "Survey/poll not found" }

Memberikan pesan error yang mengindikasikan bahwa survey/poll dengan ID yang diberikan tidak ditemukan.

### GET /api/surveys/:link

Endpoint yang dapat digunakan untuk mencari dan mengembalikan survey yang diasosiasikan dengan link yang diberikan.

Request:

- Params:
  - link (query parameter): Link survey/poll yang ingin dicari

Response:

- 200 OK: Survey ditemukan
  - title (string)
  - description (string)
  - close_time (string): Kapan survey secara otomatis ditutup dalam format waktu ISO
  - is_public (boolean)
  - are_results_open (boolean): Apakah orang lain dapat mengakses hasil survey setelah mengisi survey tanpa diberikan link?
  - questions[]
    - type: string
    - question: string
    - required: boolean
    - image_name (string?)
- 404 Not Found: Survey tidak ditemukan
  - message: "No surveys found with that ID"
- 401 Unauthorized: Survey tidak diberikan akses ke publik dan user_id tidak sesuai dengan id pemilik
  - message: "You don't have access to this survey."

### GET /api/polls/:id/join

Endpoint ini digunakan untuk menghubungkan partisipan ke halaman poll berdasarkan ID poll yang diberikan.

Request:

- Header:
  - user_id (cookie): ID user yang menggunakan halaman
- params:
  - :id(Path Parameter): ID dari poll yang ingin dimasuki

Response:

- Status 200 OK: Berhasil Masuk
- Status 404 Not Found: Poll tidak ditemukan

### GET /api/surveys/public

Endpoint ini digunakan untuk mendapatkan daftar survey publik yang tersedia untuk diisi oleh pengguna.

Response:

- Status: 200 OK
  - Memberikan daftar survey publik yang tersedia.
- Status: 404 Not Found
  - Memberikan pesan error tidak ada survey publik yang tersedia.

### GET /api/surveys/created-surveys

Request:

- Headers>Cookie:
  - user_id (cookie): ID user yang menggunakan halaman

Response:

- Status: 200 OK
  - Body:{
    "surveys":
    [
    {
    "id": 1,
    "title": "Survey 1",
    "description": "Deskripsi Survey 1",
    "created_at": "2022-12-31T12:00:00Z",
    "updated_at": "2023-01-01T09:00:00Z"
    },
    {
    "id": 2,
    "title": "Survey 2",
    "description": "Deskripsi Survey 2",
    "created_at": "2023-01-02T15:00:00Z",
    "updated_at": "2023-01-02T16:30:00Z"
    },
    ]}
- Status: 401 Unauthorized
  - Body: {"message": "Invalid authentication. Please login or provide proper authorization."}
- Status: 404 Not Found
  - Body:{"message": "You haven't created anything."}
- Status: 400 Bad Request
  - Body:{"message": "An error occurred in the request. Please ensure that the provided data is in the expected format."}

### DELETE /api/surveys/:id

Request:

- Headers>Cookie:
  - user_id (cookie): ID user yang menggunakan halaman

Response:

- Status: 200 OK(Survey berhasil dihapus)
- Status: 401 Unauthorized(pesan: Invalid authentication. Please login or provide proper authorization.)
- Status: 404 Not Found(pesan: Survey/poll not found.)

### PUT /api/accounts/:id

Endpoint ini digunakan untuk memodifikasi informasi akun pengguna yang terdaftar.

Request:

- Headers>Cookie:
  - user_id (query parameter): ID akun pengguna yang akan diubah
- Body (multipart/form-data):
  - email (string, optional): Alamat email baru pengguna.
  - password (string, optional): Kata sandi baru pengguna.
  - name (string, optional): Nama baru pengguna.
  - description (string, optional): Deskripsi baru pengguna.
  - pfp (file, optional): File gambar profil baru pengguna (Profil Picture).

Response:

- Status: 200 OK

  - Body:{
    "id": 123,
    "email": "newemail@example.com",
    "name": "New Name",
    "description": "New Description",
    "pfp": "https://example.com/profile_picture.jpg"
    }

  Memberikan respons dengan data akun pengguna yang telah dimodifikasi.

- Status: 404 Not Found

  - Body:{"message": "User account not found."}

  Memberikan pesan error akun pengguna dengan ID yang diberikan tidak ditemukan.

- Status: 400 Bad Request

  - Body:{"message": "An error occurred in the request. Make sure the provided data is in the expected format."}

  Memberikan pesan error terjadi kesalahan dalam permintaan.

- Status: 401 Unauthorized

  - Body:{"message": "Invalid authentication. Please login or provide proper authorization."}

  Memberikan pesan error pengguna tidak memiliki akses yang diperlukan atau otentikasi tidak valid.
