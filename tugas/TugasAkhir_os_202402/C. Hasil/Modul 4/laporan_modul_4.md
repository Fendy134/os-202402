# 📝 Laporan Tugas Akhir

**Mata Kuliah**: Sistem Operasi  
**Semester**: Genap / Tahun Ajaran 2024–2025  
**Nama**: `Fendy Agustian`  
**NIM**: `240202898`  
**Modul yang Dikerjakan**: Modul 4 – Subsistem Kernel Alternatif (xv6-public)

---

## 📌 Deskripsi Singkat Tugas

Modul ini mencakup dua fitur utama dalam sistem operasi xv6:

- Implementasi system call `chmod(path, mode)` untuk mengatur file menjadi `read-only` atau `read-write`.
- Penambahan driver pseudo-device `/dev/random` yang menghasilkan byte acak saat dibaca.

---

## 🛠️ Rincian Implementasi

### 🔧 System Call `chmod(path, mode)`

- Menambahkan field `short mode` pada `struct inode` di `fs.h`.
- Menambahkan syscall baru `chmod`:
  - `syscall.h` → menambahkan `#define SYS_chmod 27`
  - `syscall.c` → menambahkan entry `SYS_chmod`
  - `user.h`, `usys.S` → deklarasi dan entri syscall
  - `sysfile.c` → implementasi fungsi `sys_chmod()`
- Menambahkan pengecekan pada `file.c::filewrite()` agar menolak penulisan pada file ber-mode read-only.
- Program uji `chmodtest.c` untuk memverifikasi file tidak dapat ditulis setelah diset `read-only`.

### 🌀 Pseudo-device `/dev/random`

- Menambahkan file baru `random.c`:
  - Fungsi `randomread()` menggunakan algoritma Linear Congruential Generator (LCG) sederhana.
- Register driver di `file.c` pada array `devsw[]` dengan major number `3`.
- Menambahkan entri `/dev/random` di `init.c` dengan `mknod("/dev/random", 1, 3)`.
- Program uji `randomtest.c` untuk membaca dan mencetak byte acak dari `/dev/random`.

### 📄 Makefile

- Menambahkan `_chmodtest\` dan `_randomtest\` pada variabel `UPROGS` agar dibangun secara otomatis.

---

## ✅ Uji Fungsionalitas

Program uji yang digunakan:

- `chmodtest`: Menguji apakah file tidak bisa ditulis setelah di-set sebagai read-only.
- `randomtest`: Menguji apakah `/dev/random` menghasilkan byte acak.

---

## 📷 Hasil Uji
<img width="977" height="552" alt="modul 4" src="https://github.com/user-attachments/assets/5644c74a-3516-4f6a-aa23-ee019f7b9587" />

### 📍 Output `chmodtest`:
```
Write blocked as expected
```

### 📍 Output `randomtest` (acak setiap kali):
```
241 6 82 99 12 201 44 73
```

---

## ⚠️ Kendala yang Dihadapi

- Menambahkan field `mode` ke `struct inode` sempat menyebabkan error ketika mencoba menyimpan ke disk, sehingga akhirnya diputuskan disimpan hanya di memori.
- Awalnya lupa menambahkan `iupdate(ip)` pada `sys_chmod`, menyebabkan perubahan `mode` tidak terlihat.
- Kesalahan dalam register major number menyebabkan `/dev/random` gagal dibuka, diperbaiki dengan memastikan major=3 di `mknod()` dan `devsw[]`.

---

## 📚 Referensi

- Buku xv6 MIT: [https://pdos.csail.mit.edu/6.828/2018/xv6/book-rev11.pdf](https://pdos.csail.mit.edu/6.828/2018/xv6/book-rev11.pdf)
- Repositori xv6-public: [https://github.com/mit-pdos/xv6-public](https://github.com/mit-pdos/xv6-public)
- Diskusi kelompok praktikum dan dokumentasi Linux syscall
