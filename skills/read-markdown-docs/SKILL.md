---
name: read-markdown-docs
description: >-
  Use this skill whenever the user asks to read, scan, or understand a folder
  of markdown files (.md) in a project. Strictly reads and analyzes the documentation
  to build full project context without modifying any files or writing any code.
---

# Read Markdown Docs

Skill ini digunakan untuk membaca, memindai, dan memahami seluruh file Markdown (`.md`) di dalam folder tertentu secara menyeluruh untuk membangun pemahaman proyek.

## Batasan Ketat (Strict Rules)

- **READ-ONLY**: Jangan membuat, mengedit, atau menghapus file apa pun.
- **NO CODING**: Jangan menulis kode, menginstal dependensi, atau menjalankan perintah yang mengubah status sistem/proyek.
- **FOKUS**: Tugas selesai setelah seluruh dokumen dibaca, dipahami, dan disajikan ringkasannya kepada user.

---

## Prosedur Kerja

### 1. Identifikasi Folder Target
1. Periksa folder yang di-mention oleh user (misalnya: `docs/`, `spec/`, `documentation/`, `.docs/`, dsb.).
2. Jika user tidak menyebutkan folder secara spesifik, periksa kandidat umum di direktori proyek:
   - `docs/`
   - `.docs/`
   - `spec/` atau `specs/`
   - `documentation/`
3. Jika folder tidak ditemukan, minta konfirmasi nama folder yang benar kepada user.

### 2. Inventarisasi File Markdown
1. Pindai seluruh file berekstensi `.md` atau `.markdown` di dalam folder target (termasuk subfolder jika ada).
2. Urutkan prioritas membaca:
   - **Index / Overview**: `README.md`, `index.md`, `overview.md`, `architecture.md`
   - **Aturan & Standar**: `conventions.md`, `guidelines.md`, `rules.md`, `coding-standard.md`
   - **Spesifikasi & API**: `spec.md`, `api.md`, `database.md`, dsb.
   - **Dokumen pendukung lainnya**: File modul spesifik atau catatan lainnya.

### 3. Membaca Dokumen Secara Mendalam
1. Gunakan tool pembaca file (`read_file`) untuk membaca isi file satu per satu.
2. Serap dan pahami poin-poin krusial:
   - **Gambaran & Tujuan Sistem**: Apa tujuan proyek/modul ini?
   - **Arsitektur & Pola Desain**: Bagaimana struktur komponen dan aliran datanya?
   - **Standar & Konvensi**: Bahasa, framework, aturan penamaan, atau format kode apa yang wajib diikuti?
   - **Batasan & Larangan**: Apa yang dilarang atau hal-hal penting yang harus diperhatikan?

### 4. Penyajian Hasil & Konfirmasi Pemahaman
Setelah selesai membaca seluruh dokumen, tampilkan laporan ringkas dan terstruktur kepada user:

1. **Daftar File yang Dibaca**: Sebutkan file apa saja yang telah dipindai.
2. **Ringkasan Inti Konteks**:
   - Arsitektur & Tech Stack
   - Konvensi & Aturan Kunci
   - Hal-hal penting / batasan yang perlu diperhatikan
3. **Pernyataan Kesiapan**:
   Tegaskan bahwa seluruh dokumentasi telah dipahami, tidak ada perubahan kode/file yang dilakukan, dan tanyakan kepada user apa langkah berikutnya yang ingin dikerjakan.
