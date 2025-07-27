# 📝 Laporan Tugas Akhir

**Mata Kuliah**: Sistem Operasi  
**Semester**: Genap / Tahun Ajaran 2024–2025  
**Nama**: `Fendy Agustian`  
**NIM**: `240202898`  
**Modul yang Dikerjakan**: Modul 5 – Audit dan Keamanan Sistem (xv6-public)

---

## 📌 Deskripsi Singkat Tugas

Modul ini berfokus pada penambahan mekanisme audit log di kernel xv6. Audit ini mencatat setiap system call yang dilakukan oleh proses. Selain itu, disediakan sebuah system call `get_audit_log()` yang hanya bisa dipanggil oleh proses dengan PID 1 (init) untuk membaca isi log.

---

## 🛠️ Rincian Implementasi

* Menambahkan struktur `audit_entry` di `syscall.c` untuk menyimpan log system call
* Menambahkan logika pencatatan di fungsi `syscall()` untuk setiap system call yang valid
* Menambahkan system call baru `get_audit_log()`:
  * Mendeklarasikan syscall di `syscall.h`, `user.h`, dan `usys.S`
  * Menambahkan handler syscall di `syscall.c` dan `sysproc.c`
  * Membatasi akses hanya untuk proses PID 1
* Menambahkan program uji `audit.c` untuk membaca dan menampilkan log
* Memodifikasi `Makefile` untuk menyertakan program uji

---

## ✅ Uji Fungsionalitas

### Program uji:
* `audit`: Menampilkan isi audit log, hanya dapat diakses oleh PID 1

### Langkah uji:
1. Jalankan `make clean && make qemu-nox`
2. Edit `init.c`, ganti shell default dengan:

   ```c
   exec("audit", argv);
Build ulang dan jalankan kembali

Program audit akan dijalankan sebagai PID 1 dan menampilkan audit log

📷 Hasil Uji
<img width="854" height="580" alt="modul5" src="https://github.com/user-attachments/assets/b3018d66-9126-4d40-a3f2-c786fd62f81f" />


##**📍 Output audit :**

```
=== Audit Log ===
[0] PID=1 SYSCALL=7 TICK=2
[1] PID=1 SYSCALL=15 TICK=4
[2] PID=1 SYSCALL=17 TICK=4
...
```


##**⚠️ Kendala yang Dihadapi**

*Audit log terbatas maksimal 128 entri (dibatasi MAX_AUDIT)

*Audit tidak mencatat syscall yang gagal jika nomor tidak valid

*Perlu mengubah init untuk bisa menjalankan audit sebagai PID 1 (tidak bisa diuji langsung dari shell)

**📚 Referensi**
*Buku xv6 MIT: https://pdos.csail.mit.edu/6.828/2018/xv6/book-rev11.pdf

*Repositori xv6-public: https://github.com/mit-pdos/xv6-public

*Diskusi kelas dan praktikum

*Stack Overflow dan forum terkait kernel xv6
