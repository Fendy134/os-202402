# 📄 Laporan Tugas Akhir  
**Mata Kuliah:** Sistem Operasi  
**Semester:** Genap / Tahun Ajaran 2024–2025  
**Nama:** `Fendy Agustian`  
**NIM:** `240202898`  
**Modul yang Dikerjakan:** Modul 2 – Penjadwalan CPU Lanjutan (Priority Scheduling Non-Preemptive)

---

## 📌 Deskripsi Singkat Tugas  

Modul ini bertujuan untuk memodifikasi kernel `xv6-public` agar menggunakan algoritma **Non-Preemptive Priority Scheduling** dalam pemilihan proses. Scheduler akan menjalankan proses RUNNABLE dengan nilai `priority` terendah terlebih dahulu (0 = tertinggi).  

Fitur baru yang ditambahkan:

- Field `priority` untuk setiap proses.
- Syscall baru `set_priority(int)` untuk mengatur prioritas proses dari user space.
- Modifikasi pada `scheduler()` agar menggunakan sistem penjadwalan berbasis prioritas.

---

## 🛠️ Rincian Implementasi

- Menambahkan field `int priority` ke `struct proc` di `proc.h`
- Inisialisasi nilai default `priority = 60` pada `allocproc()` di `proc.c`
- Membuat syscall `set_priority(int)`:
  - Nomor syscall baru di `syscall.h`
  - Deklarasi di `user.h`
  - Entri syscall di `usys.S`
  - Registrasi handler di `syscall.c`
  - Implementasi di `sysproc.c`
- Modifikasi fungsi `scheduler()` di `proc.c`:
  - Scheduler memilih proses RUNNABLE dengan `priority` terkecil
  - Non-preemptive: hanya berpindah proses setelah selesai
- Menambahkan program uji `ptest.c`
- Menambahkan `_ptest` ke `UPROGS` di `Makefile`

---

## ✅ Uji Fungsionalitas

### Program uji: `ptest.c`

- Membuat dua child process dengan prioritas berbeda
- Proses dengan prioritas lebih tinggi (`priority = 10`) harus selesai lebih dulu

**Urutan Output yang Diharapkan:**
Child 2 selesai
Child 1 selesai
Parent selesai

yaml
Copy
Edit

---

## 📷 Hasil Uji
<img width="532" height="545" alt="modul 2" src="https://github.com/user-attachments/assets/56babd12-e424-4f32-ab44-8a2bbfa4edd1" />

**📍 Output `ptest` di shell `xv6`:**
```
Child 2 selesai
Child 1 selesai
Parent selesai
```



## ⚠️ Kendala yang Dihadapi

- Awalnya syscall tidak teregister karena lupa menambahkan entri di `usys.S`
- Scheduler sempat tetap menggunakan Round Robin karena lupa mengganti `proc = p` dengan `proc = highest`
- Nilai prioritas tidak berubah saat runtime karena syscall `set_priority()` salah memanggil proses (harus `myproc()`)

---

## 📚 Referensi

- [MIT xv6 Book (x86)](https://pdos.csail.mit.edu/6.828/2018/xv6/book-rev11.pdf)
- [xv6-public GitHub](https://github.com/mit-pdos/xv6-public)
- Diskusi Lab / Forum Praktikum Sistem Operasi
