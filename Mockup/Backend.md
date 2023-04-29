### POST /api/polls
Menambahkan poll baru ke database

Cookie:
- user_id

Request (multipart/form-data):
- title
- description
- is_public

Request Files:
- thumbnail

Response:
- 200 Success:
    - id
    - creator
        - id
        - email
        - name
        - description
        - pfp
    - title
    - description
    - link
    - start_time
    - end_time
    - thumbnail
- 400 Bad Request: Jika format tidak sesuai
    - message: "Expected fields: title, description, is_public, thumbnail"
- 401 Unauthorized: Jika tidak ada cookie dengan nilai user_id yang valid

### POST /api/surveys
Menambahkan survey baru ke database

Cookie:
- user_id

Request (multipart-formdata):
- title
- description
- close_time
- is_public
- questions[]
    - type
    - question
    - required
    - image_name?

Request Files:
- thumbnail
- question_images

Response:
- 201 Created: Sukses membuat survey baru
    - id
    - creator
        - id
        - email
        - name
        - description
        - pfp
    - title
    - description
    - last_modified
    - is_public
    - link
    - close_time
    - thumbnail
    - questions[]
        - id
        - type
        - question
        - required
        - image?
        - options[]?
            - id
            - label
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
- title?
- description?

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
- title?
- description?
- close_time?
- questions?

Request Files:
- thumbnail?
- question_images?

Response:
- 200 Success: Sukses mengedit survey
    - last_modified
- 304 Not Modified: Tidak ada perubahan yang terjadi
- 400 Bad Request: Jika close_time bukan format date ISO
    - message: "close_time must be in ISO Date format"
- 400 Bad Request: Jika objek dalam questions tidak sesuai format
    - message: "Expected fields in an individual question: type, question, required"
- 401 Unauthorized: Jika tidak ada cookie dengan nilai user_id yang valid
