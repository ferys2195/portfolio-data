# Agent Instructions & Workflow Guide (`portfolio-data`)

Panduan instruksi dan standar operasional untuk AI Assistant saat mengelola repository data portfolio (`portfolio-data`).

---

## 🎯 Tujuan Repository

Repository ini berfungsi sebagai **Single Source of Truth (SSOT)** untuk data website portfolio personal milik **Fery Irawan** (`@ferys2195`). Repository ini menyimpan daftar project, skills, dan kontak dalam format JSON bilingual (Bahasa Indonesia & Bahasa Inggris).

---

## 📁 Struktur File & Aturan Sinkronisasi

1. **`lang/id/projects.json`**: Data project lengkap dalam **Bahasa Indonesia**.
2. **`lang/en/projects.json`**: Data project lengkap dalam **Bahasa Inggris**.
3. **`projects.json`** (Root): Selalu disinkronkan 1:1 identik dengan `lang/en/projects.json`.
4. **`skills.json`** & **`lang/*/skills.json`**: Data daftar keahlian / tech stack.
5. **`contact.json`** & **`lang/*/contact.json`**: Data kontak & profil media sosial.

> ⚠️ **PENTING**: Setiap ada penambahan atau pengeditan data di `projects.json`, **KETIGA FILE** (`lang/id/projects.json`, `lang/en/projects.json`, dan `projects.json` di root) **WAJIB** diperbarui dan divalidasi format JSON-nya.

---

## 🏗️ Skema Struktur Objek Project

Setiap item di dalam array `"projects"` harus mengikuti struktur berikut:

```json
{
  "id": 17,
  "name": "Nama Project",
  "projectType": "personal",
  "image": "https://ik.imagekit.io/mafhi/portfolio/<slug>.png",
  "category": "Kategori",
  "organization": "",
  "period": "2026",
  "description": "Deskripsi ringkas, profesional, dan menjelaskan nilai dari aplikasi.",
  "highlights": [
    "Poin keunggulan / fitur teknis utama 1",
    "Poin keunggulan / fitur teknis utama 2",
    "Poin keunggulan / fitur teknis utama 3"
  ],
  "tech": [
    "Next.js 16",
    "React 19",
    "TypeScript",
    "Tailwind CSS v4",
    "shadcn/ui"
  ],
  "links": {
    "demo": "https://...",
    "source": "https://github.com/ferys2195/..."
  },
  "is_pin": true,
  "sort_order": 3
}
```

### 📋 Aturan Detail Field:
* **`id`**: Integer unik, selalu gunakan `max(id) + 1` dari project yang sudah ada.
* **`name`**: Nama project yang bersih dan profesional.
* **`projectType`**: `"personal"` (proyek mandiri/SaaS/eksperimen) atau `"client"` (pekerjaan dinas/klien).
* **`image`**: Standar URL ImageKit: `https://ik.imagekit.io/mafhi/portfolio/<slug>.png`.
* **`category`**:
  * **ID**: `Pemerintahan`, `GIS & Pemetaan`, `Keuangan`, `Pendidikan`, `Website Korporat`, `Pembelajaran Bahasa`, `Karir`, `Permainan`, `Portfolio`.
  * **EN**: `Government`, `GIS & Mapping`, `Finance`, `Education`, `Corporate Website`, `Language Learning`, `Career`, `Game`, `Portfolio`.
* **`organization`**: Nama institusi/klien jika `projectType` adalah `"client"`, kosongkan `""` jika `"personal"`.
* **`period`**: Format tahun (contoh: `"2026"`) atau rentang bulan-tahun (contoh: `"Sep 2025 - Nov 2025"`).
* **`description`**: 1–2 kalimat profesional yang padat dan jelas.
* **`highlights`**: 3–4 poin teknis atau dampak fungsional sistem.
* **`tech`**: Array string framework, bahasa, dan library utama dengan versi terbaru yang akurat.
* **`links`**:
  * Jika repo bersifat publik: sertakan `"source"`.
  * Jika repo bersifat privat: **jangan sertakan** `"source"`, hanya sediakan `"demo"` jika tersedia.
* **`is_pin`**: `true` untuk project unggulan, `false` jika biasa.
* **`sort_order`**: Urutan prioritas tampilan.

---

## ⚡ Alur Otomatisasi Saat Pengguna Memberi Instruksi

Ketika user meminta untuk menambahkan/memperbarui project dari GitHub (contoh: *"masukan repo XYZ ke portfolio"*), jalankan tahapan berikut secara mandiri tanpa banyak tanya:

### Langkah 1: Investigasi Repository via GitHub CLI (`gh`)
1. Jalankan `gh repo view ferys2195/<nama-repo>`.
2. Periksa file konfigurasi dan dokumentasi repo:
   - `package.json` / `composer.json` / `requirements.txt` (untuk mendeteksi tech stack akurat).
   - `README.md`, `ARCHITECTURE.md`, `metadata.json`, atau folder fitur `features/` / `app/`.
   - Riwayat commit terbaru untuk memahami fitur teranyar.

### Langkah 2: Penyusunan Data Bilingual
1. Ekstrak data dan buat deskripsi & highlights dalam **Bahasa Indonesia** dan **Bahasa Inggris**.
2. Tentukan ID berikutnya (`max(id) + 1`), kategori yang sesuai, dan status pin/urutan.
3. Cek apakah repository publik atau privat (jika privat, jangan pasang link source GitHub).

### Langkah 3: Modifikasi File Data
1. Tambahkan item project ke [lang/id/projects.json](file:///e:/Personal/Website/portfolio-data/lang/id/projects.json).
2. Tambahkan item project ke [lang/en/projects.json](file:///e:/Personal/Website/portfolio-data/lang/en/projects.json).
3. Sinkronkan [projects.json](file:///e:/Personal/Website/portfolio-data/projects.json) dengan `lang/en/projects.json`.

### Langkah 4: Validasi & Quality Control
1. Jalankan script validasi Python/Node.js untuk memastikan ketiga file valid JSON dan tidak ada syntax error.
2. Tampilkan ringkasan data yang baru dimasukkan kepada pengguna.

### Langkah 5: Git Commit & Push (Jika Diinstruksikan)
1. Gunakan pesan commit konvensional:
   - Contoh: `feat: add <Project Name> project entry with demo link in projects.json`
2. Push ke branch `main`.

---

## 🛠️ Perintah Berguna

```bash
# Cek daftar repo pengguna di GitHub
gh repo list ferys2195 --limit 15

# Cek detail repo
gh repo view ferys2195/<nama-repo>

# Validasi JSON
python -c "import json; [json.load(open(f, encoding='utf-8')) for f in ['projects.json', 'lang/en/projects.json', 'lang/id/projects.json']]; print('All valid!')"
```
