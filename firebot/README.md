# 🔥 Firebot - Robot Pemadam Kebakaran Cerdas

**Firebot** adalah robot cerdas yang mampu mendeteksi **api**, **asap**, dan **rintangan** secara otomatis. Proyek ini menggunakan model AI seperti **YOLOv8** dan **Vision Transformer (DPT)** untuk navigasi dan pemahaman lingkungan secara real-time.

> 📌 **Konseptor:** Proyek ini dikonsep dan diarahkan oleh saya, dengan fokus pada **desain sistem**, **arsitektur navigasi**, dan **integrasi AI** secara keseluruhan.

---

## 🎯 Tujuan Proyek

Robot ini dirancang untuk:

- 🔥 Mendeteksi **api** dan **asap**
- 🚧 Menghindari **rintangan**
- 🧭 Menentukan arah gerak secara otomatis berdasarkan **prioritas misi**
- 🌐 Mengirim perintah ke **Firebase** untuk dikontrol oleh **mikrokontroler**

---

## 🧠 Cara Kerja Singkat

### 1. 🔍 Persepsi
- 📦 Menggunakan **dua model YOLOv8** secara paralel:
  - 🔥 **Deteksi Api/Asap** → model custom YOLOv8
  - 🚧 **Deteksi Rintangan** → model YOLOv8 standar
- 🌄 Estimasi kedalaman dilakukan menggunakan **Vision Transformer (DPT)**: `dpt-swinv2`, untuk memetakan jarak antar objek secara akurat.

### 2. 🤖 Pengambilan Keputusan
Berdasarkan hasil deteksi dan peta kedalaman, robot menentukan aksi berdasarkan **prioritas** berikut:

| Prioritas | Kondisi                                                                 | Aksi Robot         |
|-----------|--------------------------------------------------------------------------|--------------------|
| 1️⃣        | Rintangan terlalu dekat (`< safe_distance`)                             | Belok kiri/kanan   |
| 2️⃣        | Api terdeteksi, tidak ada ancaman rintangan                             | Maju ke arah api   |
| 3️⃣        | Tidak ada api, jalur aman                                                | Eksplorasi maju    |
| 4️⃣        | Api atau rintangan terlalu dekat hingga membahayakan                    | Berhenti total     |

### 3. 🧭 Aksi Robot
- 📡 Perintah (forward, left, right, stop) dikirim ke **Firebase Realtime Database**
- ⚙️ Robot fisik membaca perintah dan menggerakkan motor sesuai arah

---

## 🧱 Teknologi yang Digunakan

| Teknologi                  | Fungsi                                                              |
|---------------------------|---------------------------------------------------------------------|
| **YOLOv8 (Ultralytics)**  | Deteksi objek (api, asap, dan rintangan umum)                       |
| **DPT (Hugging Face)**    | Estimasi kedalaman berbasis Vision Transformer                      |
| **Firebase Realtime DB**  | Komunikasi cloud antara Python dan mikrokontroler                   |
| **Python**                | Logika pemrosesan visual dan kontrol navigasi robot                 |

---

## 🚀 Cara Menjalankan Proyek

### 1. 📥 Clone Repositori
```bash
git clone https://github.com/Delapuspita77/robotics.git
cd robotics/firebot
```

### 2. 🐍 Siapkan Virtual Environment
```bash
# Masuk ke folder python/
cd python

# Buat dan aktifkan virtual environment
python -m venv venv

# Aktifkan environment:
venv\Scripts\activate    # ← Windows
# atau
source venv/bin/activate  # ← Linux/macOS

# Install dependencies
pip install -r requirements.txt
```

### 3. 💾 Siapkan Model dan Firebase
- Letakkan file model berikut ke dalam folder checkpoints/:
  - yolov8n.pt → untuk deteksi rintangan
  - yolov8n-200e-v0.2.pt → untuk deteksi api dan asap
- Simpan file serviceAccountKey.json dari Firebase ke folder utama proyek (firebot/)

### 4. ▶️ Jalankan Program
```bash
python navigate2depth.py
```
