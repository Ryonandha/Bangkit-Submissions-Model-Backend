# Bangkit Submission - Model Backend (Skin Cancer Classification)

Aplikasi backend ini dibangun menggunakan framework **Hapi** untuk menyediakan API prediksi klasifikasi kanker (Cancer/Non-cancer) menggunakan model **TensorFlow.js**. Data riwayat prediksi disimpan secara persisten menggunakan **Google Cloud Firestore**.

Proyek ini dirancang untuk memenuhi submisi "Belajar Penerapan Machine Learning dengan Google Cloud".

## 📱 Tampilan Aplikasi

![image](https://github.com/indrayyana/Belajar-Penerapan-ML-dengan-Google-Cloud-Submission/assets/93801579/7c9a92ad-0888-48fc-b122-0a1a2c183af6)

![image](https://github.com/indrayyana/Belajar-Penerapan-ML-dengan-Google-Cloud-Submission/assets/93801579/a0c84fab-3c28-41d7-99eb-864ba0048bfc)

![image](https://github.com/indrayyana/Belajar-Penerapan-ML-dengan-Google-Cloud-Submission/assets/93801579/8081795d-9ec8-4699-85c4-7676cec80f6b)

![image](https://github.com/indrayyana/Belajar-Penerapan-ML-dengan-Google-Cloud-Submission/assets/93801579/8c7e9b1b-a5b0-45d6-9802-333a956154e2)

## 🚀 Fitur

-   **Prediksi Klasifikasi**: Mengklasifikasikan gambar yang diunggah menjadi "Cancer" atau "Non-cancer".
-   **Riwayat Prediksi**: Menyimpan dan mengambil riwayat prediksi dari database Firestore.
-   **Penanganan Error**: Validasi ukuran file (maks 1MB) dan format input.

## 🛠️ Teknologi yang Digunakan

-   **Node.js** (Runtime Environment)
-   **@hapi/hapi** (Web Framework)
-   **@tensorflow/tfjs-node** (Machine Learning Model Loader & Inference)
-   **@google-cloud/firestore** (NoSQL Database)
-   **Docker** (Containerization)

## 📂 Struktur Proyek

```text
.
├── src
│   ├── exceptions      # Custom Error classes (ClientError, InputError)
│   ├── server          # Konfigurasi Server, Routes, dan Handler
│   └── services        # Logika bisnis (Inference, Firestore, Load Model)
├── Dockerfile          # Konfigurasi Docker image
├── package.json        # Dependensi proyek
└── ...

```

## ⚙️ Prasyarat

Sebelum menjalankan aplikasi, pastikan Anda telah menginstal:

1. **Node.js** (Disarankan v20 sesuai Dockerfile)
2. **Google Cloud Service Account Key** (File JSON untuk akses Firestore)

## 🔧 Instalasi & Konfigurasi

1. **Clone repository ini:**
```bash
git clone <repository-url>
cd <folder-repository>

```


2. **Install dependencies:**
```bash
npm install

```


3. **Konfigurasi Environment Variable:**
Aplikasi ini membutuhkan beberapa variabel lingkungan. Anda dapat mengaturnya di sistem operasi Anda atau membuat file `.env`.
Variabel yang digunakan:
* `PORT`: Port server (default: `3000`)
* `MODEL_URL`: URL publik ke file `model.json` (default: `https://storage.googleapis.com/machinelearning-ryo/model.json`).
* `GOOGLE_APPLICATION_CREDENTIALS`: Path absolut ke file JSON Service Account Google Cloud Anda (wajib untuk akses Firestore).


> **Penting:** Jangan lupa menambahkan file `serviceaccount.json` atau kunci rahasia ke dalam `.gitignore` agar tidak terunggah ke repositori publik.



## ▶️ Menjalankan Aplikasi

### Secara Lokal

Untuk menjalankan di mode production:

```bash
npm run start

```

Untuk menjalankan di mode development (dengan nodemon):

```bash
npm run start:dev

```

Server akan berjalan di `http://localhost:3000`.

### Menggunakan Docker

1. **Build Image:**
```bash
docker build -t submission-backend .

```


2. **Jalankan Container:**
```bash
docker run -p 3000:3000 submission-backend

```



## 📡 Dokumentasi API

### 1. POST `/predict`

Melakukan prediksi pada gambar yang diunggah.

* **Method:** `POST`
* **Header:** `Content-Type: multipart/form-data`
* **Body:**
* `image`: File gambar (Maksimal 1MB)



**Contoh Response Sukses (201 Created):**

```json
{
    "status": "success",
    "message": "Model is predicted successfully",
    "data": {
        "id": "77e30dcd-...",
        "result": "Cancer",
        "suggestion": "Segera periksa ke dokter!",
        "createdAt": "2024-05-01T10:00:00.000Z"
    }
}

```

**Kriteria Prediksi:**

* Confidence Score > 50% = **Cancer**
* Confidence Score <= 50% = **Non-cancer**

### 2. GET `/predict/histories`

Mengambil seluruh riwayat prediksi yang tersimpan di Firestore.

* **Method:** `GET`

**Contoh Response Sukses (200 OK):**

```json
{
    "status": "success",
    "data": [
        {
            "id": "77e30dcd-...",
            "history": {
                "result": "Cancer",
                "createdAt": "2024-05-01T10:00:00.000Z",
                "suggestion": "Segera periksa ke dokter!",
                "id": "77e30dcd-..."
            }
        },
        ...
    ]
}

```

## ⚠️ Penanganan Error

Aplikasi menangani beberapa skenario error umum:

* **Payload Too Large (413):** Jika ukuran gambar melebihi 1MB.
* **Input Error (400):** Jika terjadi kesalahan dalam proses prediksi (Bad Request).

## 📝 Lisensi

ISC

```

```
