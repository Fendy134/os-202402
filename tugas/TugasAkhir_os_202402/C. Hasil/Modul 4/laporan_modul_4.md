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
  
## Bagian A – System Call chmod()

### Penambahan ke `inode`:

```c
short mode; // 0 = read-write, 1 = read-only
```

### Implementasi syscall:

```c
int sys_chmod(void) {
  char *path;
  int mode;
  struct inode *ip;

  if(argstr(0, &path) < 0 || argint(1, &mode) < 0)
    return -1;

  begin_op();
  if((ip = namei(path)) == 0){
    end_op();
    return -1;
  }

  ilock(ip);
  ip->mode = mode;
  iupdate(ip);  // opsional
  iunlock(ip);
  end_op();

  return 0;
}
```

### Validasi penulisan file:

```c
if(f->ip && f->ip->mode == 1)  // mode == 1 berarti read-only
  return -1;
```

---

## 📄 Program Uji `chmodtest.c`

```c
int main() {
  int fd = open("myfile.txt", O_CREATE | O_RDWR);
  write(fd, "hello", 5);
  close(fd);

  chmod("myfile.txt", 1);  // ubah jadi read-only

  fd = open("myfile.txt", O_RDWR);
  if(write(fd, "world", 5) < 0)
    printf(1, "Write blocked as expected\n");
  else
    printf(1, "Write allowed unexpectedly\n");

  close(fd);
  exit();
}
```
## ✅ Output Diharapkan

```bash
$ chmodtest
Write blocked as expected

```

### 🌀 Pseudo-device `/dev/random`

- Menambahkan file baru `random.c`:
  - Fungsi `randomread()` menggunakan algoritma Linear Congruential Generator (LCG) sederhana.
- Register driver di `file.c` pada array `devsw[]` dengan major number `3`.
- Menambahkan entri `/dev/random` di `init.c` dengan `mknod("/dev/random", 1, 3)`.
- Program uji `randomtest.c` untuk membaca dan mencetak byte acak dari `/dev/random`.

### Struktur `randomread`:

```c
static uint seed = 123456;

int randomread(struct inode *ip, char *dst, int n) {
  for(int i = 0; i < n; i++) {
    seed = seed * 1664525 + 1013904223;
    dst[i] = seed & 0xFF;
  }
  return n;
}
```

### Registrasi di `file.c`:

```c
[3] = { 0, randomread }, // device major = 3
```

### Tambahkan node device:

```c
mknod("/random", 3, 0);
```

---

## 📄 Program Uji `randomtest.c`

```c
int main() {
  char buf[8];
  int fd = open("/random", 0);
  if(fd < 0){
    printf(1, "cannot open /dev/random\n");
    exit();
  }

  read(fd, buf, 8);
  for(int i = 0; i < 8; i++)
    printf(1, "%d ", (unsigned char)buf[i]);
  printf(1, "\n");

  close(fd);
  exit();
}
```

---

## ✅ Output Diharapkan

```
$ randomtest
201 45 132 88 2 79 234 11
```




## 📷 Hasil Uji
<img width="977" height="552" alt="modul 4" src="https://github.com/user-attachments/assets/5644c74a-3516-4f6a-aa23-ee019f7b9587" />

### 📍 Output `chmodtest`:
```
Write blocked as expected
```

### 📍 Output `randomtest` (acak setiap kali):
```
159 114 41 116 67 198 109 232
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
