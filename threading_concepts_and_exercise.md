# Konsep Single Thread dan Multithread

## Single Thread
Konsep single thread mengacu pada eksekusi program yang hanya memiliki satu jalur kontrol, artinya hanya satu tugas yang dikerjakan dalam satu waktu. Dalam model ini, CPU menjalankan satu instruksi dari satu thread hingga selesai sebelum melanjutkan ke instruksi berikutnya. Hal ini membuat pengelolaan dan debugging aplikasi menjadi lebih sederhana, namun bisa menjadi tidak efisien jika tugas-tugas yang berat harus diselesaikan secara berurutan.

![Single Thread](https://upload.wikimedia.org/wikipedia/commons/5/5a/Single_threaded_process.png)

## Multithread
Multithreading memungkinkan program menjalankan beberapa thread secara bersamaan dalam satu proses. Setiap thread dapat menjalankan tugasnya masing-masing, yang membuat program lebih efisien terutama pada sistem multiprosesor. Dengan multithreading, aplikasi dapat tetap responsif meskipun menjalankan proses berat di latar belakang, seperti pengunduhan file atau komputasi matematis.

![Multithread](https://upload.wikimedia.org/wikipedia/commons/0/01/Multithreaded_process.png)

# Programming Exercise

## a. Penerapan Thread pada SumTask.java
File `SumTask.java` merupakan contoh penerapan threading di Java yang terdapat dalam repositori [osc10e/ch4](https://github.com/ferryastika/osc10e/tree/master/ch4). Dalam file ini, digunakan konsep *Fork/Join Framework*, yaitu mekanisme untuk membagi tugas besar menjadi beberapa sub-tugas yang lebih kecil dan menjalankannya secara paralel menggunakan thread.

Kelas `SumTask` meng-extend `RecursiveTask<Long>`, di mana metode `compute()` akan memeriksa apakah ukuran data cukup kecil untuk dihitung secara langsung. Jika tidak, data akan dibagi menjadi dua bagian, dan setiap bagian akan dijalankan sebagai tugas baru secara paralel menggunakan `fork()` dan kemudian hasilnya digabungkan menggunakan `join()`. Konsep ini sangat cocok digunakan untuk pekerjaan seperti penjumlahan elemen array besar karena dapat mempercepat waktu eksekusi secara signifikan. Penggunaan multithreading melalui Fork/Join memungkinkan sistem untuk memanfaatkan banyak inti prosesor secara efisien.

### Penjelasan Singkat Kode SumTask.java:
```java
class SumTask extends RecursiveTask<Long> {
    private int[] array;
    private int low;
    private int high;
    
    public SumTask(int[] array, int low, int high) {
        this.array = array;
        this.low = low;
        this.high = high;
    }

    protected Long compute() {
        if (high - low <= 1000) {
            long sum = 0;
            for (int i = low; i < high; i++) sum += array[i];
            return sum;
        } else {
            int mid = low + (high - low) / 2;
            SumTask left = new SumTask(array, low, mid);
            SumTask right = new SumTask(array, mid, high);
            left.fork();
            long rightResult = right.compute();
            long leftResult = left.join();
            return leftResult + rightResult;
        }
    }
}
```

## b. Penerapan Thread di Linux (thrd-posix.c) dan Microsoft Windows (thrd-win32.c)

### Penerapan di Linux: `thrd-posix.c`
Pada sistem operasi Linux, implementasi threading dilakukan menggunakan POSIX Threads (pthreads). File `thrd-posix.c` dalam repositori menunjukkan bagaimana membuat dan mengelola thread menggunakan pustaka ini. Thread dibuat menggunakan `pthread_create()`, dan kita memberikan fungsi callback sebagai argumen yang akan dijalankan oleh thread tersebut.

Contoh alur dari file ini:
1. Inisialisasi struktur data thread.
2. Fungsi `pthread_create()` digunakan untuk memulai eksekusi thread.
3. Setelah thread dijalankan, `pthread_join()` digunakan untuk menunggu semua thread selesai.

Pthreads juga memungkinkan sinkronisasi antar thread menggunakan `pthread_mutex_t` untuk mencegah kondisi balapan (*race condition*). Hal ini penting terutama saat beberapa thread mengakses atau memodifikasi data bersama secara bersamaan.

### Penerapan di Windows: `thrd-win32.c`
Sementara itu, di Microsoft Windows, pembuatan thread dilakukan menggunakan API Windows. File `thrd-win32.c` menunjukkan cara penggunaan `CreateThread()` untuk memulai thread baru. Fungsi `WaitForSingleObject()` digunakan untuk menunggu thread menyelesaikan tugasnya, mirip seperti `pthread_join()` di POSIX.

Langkah-langkah:
1. Mendefinisikan fungsi yang akan dijalankan oleh thread.
2. Menggunakan `CreateThread()` untuk memulai eksekusi fungsi dalam thread baru.
3. Menggunakan `WaitForSingleObject()` untuk menunggu penyelesaian thread.

Perbedaan utama antara POSIX dan Windows adalah dalam API dan sintaks yang digunakan. Meskipun kedua pendekatan memiliki tujuan yang sama, yaitu melakukan multitasking, masing-masing memiliki konvensi dan dukungan sistem operasi yang berbeda. Namun, keduanya sangat berguna dalam aplikasi yang membutuhkan performa tinggi melalui eksekusi paralel.

---

Referensi kode: [GitHub Repository - OSC10E Chapter 4](https://github.com/ferryastika/osc10e/tree/master/ch4)

