# Apa itu Forking?

Forking adalah proses dalam sistem operasi di mana sebuah proses induk (parent process) membuat salinan dirinya sendiri. Proses hasil duplikasi ini disebut sebagai child process. Forking biasanya dilakukan menggunakan sistem call `fork()` yang tersedia dalam bahasa pemrograman C dan digunakan dalam sistem operasi berbasis Unix/Linux.

## Ketika `fork()` Dipanggil:

- Proses baru (anak) akan dibuat sebagai salinan identik dari proses induk.
- Eksekusi akan dilanjutkan oleh kedua proses secara paralel, dimulai dari titik setelah `fork()` dipanggil.
- Proses anak dan induk memiliki ruang memori yang terpisah, meskipun pada awalnya identik.

## Contoh Forking dalam C:

```c
#include <stdio.h>
#include <unistd.h>

int main() {
    pid_t pid = fork();

    if (pid < 0) {
        perror("Fork gagal");
        return 1;
    } else if (pid == 0) {
        printf("Ini adalah proses anak dengan PID %d\n", getpid());
    } else {
        printf("Ini adalah proses induk dengan PID %d dan anak dengan PID %d\n", getpid(), pid);
    }

    return 0;
}
```

### Penjelasan Kode:

- `fork()` digunakan untuk membuat proses baru.
- Jika `pid < 0`, berarti terjadi kesalahan saat mencoba membuat proses anak.
- Jika `pid == 0`, berarti proses saat ini adalah proses anak.
- Jika `pid > 0`, berarti proses saat ini adalah proses induk, dan `pid` adalah ID dari proses anak.

---

# Apakah `print123thread.c` Menggunakan Forking?

Tidak. Kode `print123thread.c` tidak menggunakan mekanisme forking, melainkan menggunakan threading dengan pustaka POSIX thread (pthread). Ini adalah pendekatan berbeda untuk menjalankan beberapa alur eksekusi secara bersamaan.

> Forking menciptakan proses baru yang terpisah, sedangkan threading menciptakan beberapa jalur eksekusi (thread) di dalam proses yang sama, berbagi ruang memori yang sama.

---

# Analisis Kode `print123thread.c`

Kode ini dirancang untuk menjalankan tiga thread secara sinkron yang masing-masing mencetak angka 1, 2, dan 3 secara bergiliran tanpa henti.

## Struktur Umum Program:

### 1. Inisialisasi:
- Mengimpor pustaka pthread dan pustaka standar.
- Mendeklarasikan variabel global untuk sinkronisasi antar thread.

### 2. Variabel Global:
- `pthread_mutex_t lock`: digunakan untuk menghindari race condition.
- `pthread_cond_t cond1, cond2, cond3`: variabel kondisi untuk mengatur giliran thread.
- `int done = 1`: menunjukkan giliran thread aktif (1 = thread 1, 2 = thread 2, dst).

### 3. Fungsi Thread (`foo`):
- Menerima argumen identitas thread (1, 2, atau 3).
- Dalam loop tak hingga:
  - Mengunci mutex.
  - Memeriksa apakah saat ini gilirannya.
  - Jika tidak, menunggu pada variabel kondisi.
  - Jika ya:
    - Mencetak angka.
    - Mengubah nilai `done`.
    - Memberi sinyal ke thread berikutnya.
  - Membuka mutex.

### 4. Fungsi Main:
- Membuat 3 thread dengan `pthread_create`.
- Menunggu thread dengan `pthread_join` (meskipun tidak akan selesai karena loop).

## Ilustrasi Sederhana Sinkronisasi:

```
Thread 1: Cetak 1 -> Sinyal ke Thread 2  
Thread 2: Cetak 2 -> Sinyal ke Thread 3  
Thread 3: Cetak 3 -> Sinyal ke Thread 1  
(Ulang dari awal)
```

---

# Kesimpulan

- Threading lebih ringan daripada forking karena semua thread berbagi ruang memori dalam satu proses.
- Kode `print123thread.c` adalah contoh yang bagus untuk memahami sinkronisasi antar thread menggunakan mutex dan conditional variable.
- Tidak ada proses baru yang dibuat; hanya thread yang berjalan secara paralel dalam satu proses.
