# MLGC Submission

Submission proyek MLGC (Machine Learning Google Cloud) berupa backend API untuk prediksi kanker kulit dari gambar. API menerima unggahan gambar, menjalankannya ke model TensorFlow.js yang dihosting di Google Cloud Storage, dan menyimpan hasil prediksi ke Cloud Firestore.

## Fitur

- Prediksi klasifikasi gambar (Cancer / Non-cancer) dengan skor kepercayaan dan saran
- Endpoint riwayat prediksi dari Firestore
- Batas payload gambar 1 MB dengan respons error khusus (HTTP 413)
- Penanganan error terpusat melalui extension `onPreResponse`
- Konfigurasi melalui variabel lingkungan, berkas `.env` tidak ikut ter-commit (terdaftar di `.gitignore`)
- Siap dideploy dengan Docker (Node.js 18, port 8080)

## Teknologi

- Node.js 18
- Hapi (`@hapi/hapi`)
- TensorFlow.js Node (`@tensorflow/tfjs-node` 4.x)
- Cloud Firestore (`@google-cloud/firestore`)
- `dotenv`

## Struktur Proyek

```
mlgc-submission/
├── src/
│   ├── exceptions/    # InputError dan error khusus lain
│   ├── server/        # server.js, routes.js, handler.js
│   └── services/      # inferenceService.js, loadModel.js, storeData.js
├── Dockerfile
└── package.json
```

## Endpoint API

| Method | Path | Keterangan |
| ------ | ---- | ---------- |
| POST | `/predict` | Mengunggah gambar (multipart/form-data, field `image`) dan mendapatkan hasil prediksi |
| GET | `/predict/histories` | Mengambil riwayat prediksi dari Firestore |

## Cara Menjalankan

Prasyarat: Node.js dan proyek Google Cloud dengan Firestore.

1. Instal dependensi:

   ```bash
   npm install
   ```

2. Buat berkas `.env` (tidak tersedia di repositori) berisi antara lain `APP_PORT`, `APP_HOST`, `MODEL_URL` (URL model.json di Cloud Storage), dan `PROJECT_ID`.

3. Jalankan server:

   ```bash
   npm start          # produksi
   npm run start:dev  # pengembangan (nodemon)
   ```

   Server berjalan di port 8080 secara bawaan.

### Docker

```bash
docker build -t mlgc-submission .
docker run -p 8080:8080 mlgc-submission
```

Nilai bawaan `MODEL_URL` dan `PROJECT_ID` untuk deployment contoh tertulis pada `Dockerfile`.

## Repositori Terkait

- `backend-mlgc`, backend MLGC versi lain dengan struktur layanan yang mirip.
