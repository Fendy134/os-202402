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

## ✅ Uji Fungsionalitas<img width="725" height="622" alt="modul 1" src="https://github.com/user-attachments/assets/1a58182e-7959-44ea-a5c7-ff006b188907" />


Program uji yang saya jalankan untuk memastikan fungsi bekerja dengan benar adalah:

- `ptest` – Menampilkan daftar proses aktif (PID, ukuran memori, nama).
- `rtest` – Menampilkan jumlah pemanggilan `read()` sebelum dan sesudah membaca input stdin.

---

## 📷 Hasil Uji

### 📍 Output `ptest`

