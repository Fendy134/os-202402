# 📝 Laporan Tugas Akhir

**Mata Kuliah**: Sistem Operasi  
**Semester**: Genap / Tahun Ajaran 2024–2025  
**Nama**: `Fendy Agustian`  
**NIM**: `240202898`  
**Modul yang Dikerjakan**: Modul 3 – Manajemen Memori Tingkat Lanjut (xv6-public x86)

---

## 📌 Deskripsi Singkat Tugas

Modul ini mengimplementasikan dua fitur memori tingkat lanjut pada xv6:

1. **Copy-on-Write (CoW) Fork**  
   Fork menjadi lebih efisien dengan berbagi halaman memori read-only antar proses. Salinan halaman hanya dibuat saat ada proses yang melakukan penulisan.

2. **Shared Memory ala System V**  
   Implementasi mekanisme berbagi halaman memori antar proses berdasarkan *key*. Halaman akan dibebaskan saat tidak ada proses yang mereferensikannya lagi.

---

## 🛠️ Rincian Implementasi

### A. Copy-on-Write Fork (CoW)

- Menambahkan array `ref_count[]` untuk setiap halaman fisik di `vm.c`
- Membuat fungsi `incref()` dan `decref()` untuk manajemen referensi halaman
- Menambahkan flag `PTE_COW` di `mmu.h`
- Mengganti `copyuvm()` menjadi `cowuvm()` di `fork()` (`proc.c`)
- Mengatur ulang page fault handler (`trap.c`) untuk menangani penulisan ke halaman COW
- Memastikan `incref()` dan `decref()` dipanggil dengan konsisten di `kalloc()` dan `kfree()`

### B. Shared Memory ala System V

- Menambahkan struktur `shmtab[]` di `vm.c` untuk menyimpan daftar shared memory
- Implementasi syscall `shmget(int key)` untuk mengambil atau membuat shared memory
- Implementasi syscall `shmrelease(int key)` untuk melepas shared memory
- Melakukan mapping shared memory ke alamat `USERTOP - (i+1)*PGSIZE`
- Registrasi syscall di `user.h`, `usys.S`, `syscall.h`, dan `syscall.c`

---

## ✅ Uji Fungsionalitas

Berikut adalah program uji yang digunakan:

- `cowtest.c`: Menguji apakah child menyalin halaman saat menulis dan parent tetap melihat nilai awal.
- `shmtest.c`: Menguji apakah dua proses dapat berbagi dan saling mengakses halaman shared memory.
- `fork` → memastikan tidak melakukan duplikasi memori saat fork.
  
---

## 📷 Hasil Uji

<img width="616" height="722" alt="modul 3" src="https://github.com/user-attachments/assets/e7cad518-9a18-47d4-987e-755d09c25b24" />
### 📍 Output dari `cowtest`:
```
Child sees: Y
Parent sees: X
```
shell
Copy
Edit

### 📍 Output dari `shmtest`:
```
Child reads: A
Parent reads: B
```
shell
Copy
Edit

### 📍 (Opsional) Screenshot:



yaml
Copy
Edit

---

## ⚠️ Kendala yang Dihadapi

- Awalnya page fault tidak berhasil karena salah cek flag `PTE_COW`
- Salah perhitungan alamat virtual saat mapping shared memory ke `USERTOP`
- Lupa menambah `incref()` di `kalloc()` menyebabkan free memory terlalu awal (bug)

---

## 📚 Referensi

- [Buku xv6 (MIT)](https://pdos.csail.mit.edu/6.828/2018/xv6/book-rev11.pdf)
- [xv6-public GitHub](https://github.com/mit-pdos/xv6-public)
- Diskusi praktikum dan forum GitHub Issues
- Stack Overflow (keyword: `xv6 copy-on-write` dan `xv6 shared memory`)

---
⚠️ Catatan:

Ganti <Nama Lengkap> dan <Nomor Induk Mahasiswa> dengan identitasmu.

Jika kamu punya screenshot, taruh file-nya di folder screenshots/ dan sesuaikan nama file-nya di bagian 📷 Hasil Uji.

Markdown ini bisa langsung kamu salin ke file laporan_modul3.md atau Google Docs, tergantung format pengumpulan.
