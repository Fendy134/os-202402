# 📝 Laporan Tugas Akhir

**Mata Kuliah**: Sistem Operasi  
**Semester**: Genap / Tahun Ajaran 2024–2025  
**Nama**: `Fendy Agustian`  
**NIM**: `240202898`  
**Modul yang Dikerjakan**:  
**Modul 1 – System Call dan Instrumentasi Kernel**

---

## 📌 Deskripsi Singkat Tugas

Pada modul ini, saya mengimplementasikan dua buah **system call** baru ke dalam kernel **xv6-public**. Tujuannya adalah:

- `getpinfo()` – untuk memperoleh informasi proses-proses aktif di sistem.
- `getreadcount()` – untuk mengetahui berapa kali fungsi `read()` telah dipanggil sejak sistem boot.

---

## 🛠️ Rincian Implementasi

Langkah-langkah yang saya lakukan dalam implementasi modul ini adalah:

- Menambahkan `struct pinfo` pada `proc.h` untuk menyimpan informasi proses.
- Menambahkan variabel global `readcount` di `sysproc.c`.
- Mengedit `syscall.h` untuk menambahkan nomor syscall baru.
- Menambahkan fungsi `sys_getpinfo()` dan `sys_getreadcount()` di `sysproc.c`.
- Memodifikasi `syscall.c` untuk meregistrasi syscall baru.
- Menambahkan deklarasi syscall di `user.h` dan `usys.S`.
- Menambahkan logika penambahan counter `readcount` di awal fungsi `sys_read()` pada `sysfile.c`.
- Menulis program uji `ptest.c` untuk `getpinfo()` dan `rtest.c` untuk `getreadcount()`.
- Menambahkan kedua program tersebut ke `Makefile` agar bisa dijalankan di shell xv6.

---

## ✅ Uji Fungsionalitas

Program uji yang saya jalankan untuk memastikan fungsi bekerja dengan benar adalah:

- `ptest` – Menampilkan daftar proses aktif (PID, ukuran memori, nama).
- `rtest` – Menampilkan jumlah pemanggilan `read()` sebelum dan sesudah membaca input stdin.

---

## 📷 Hasil Uji
<img width="725" height="622" alt="modul 1" src="https://github.com/user-attachments/assets/5cd07e4f-284b-44b1-8ddf-8c564df4e508" />

### 📍 Output `ptest`

 ## Daftar Proses

| PID |   MEM   |  NAME  |
|-----|---------|--------|
|  1  | 12288   | init   |
|  2  | 16384   | sh     |
|  3  | 12288   | ptest  |

### 📍 Output `rtest`

Read Count Sebelum: 12
hello
Read Count Setelah: 13



---

## ⚠️ Kendala yang Dihadapi

Beberapa kendala yang sempat saya hadapi selama implementasi antara lain:

- Kesalahan saat menggunakan `argptr()` menyebabkan kernel panic karena pointer dari user-space tidak tervalidasi dengan benar.
- Salah menggunakan `ptable_lock` padahal tidak tersedia di versi xv6-public, sehingga harus menggunakan `ptable.lock` dari struktur `ptable`.
- Kesalahan indentasi di `Makefile` (menggunakan spasi alih-alih tab) menyebabkan program uji tidak dikenali.

---

## 📚 Referensi

- MIT xv6 Book: [https://pdos.csail.mit.edu/6.828/2018/xv6/book-rev11.pdf](https://pdos.csail.mit.edu/6.828/2018/xv6/book-rev11.pdf)
- xv6-public GitHub Repo: [https://github.com/mit-pdos/xv6-public](https://github.com/mit-pdos/xv6-public)
- Diskusi dan dokumentasi praktikum
- Stack Overflow dan GitHub Issues terkait system call di xv6

---
Jika kamu ingin saya bantu ekspor ke PDF atau Word (.docx), saya bisa bantu hasilkan markdown-nya ke format tersebut melalui alat konversi eksternal (misalnya Pandoc) atau kamu bisa upload file ini di editor Markdown online seperti:

dillinger.io

markdowntopdf.com
