# References Indonesian Thread, Task Number 3-4

## 2.11.1. Proses

### 3. Jelaskan tindakan yang diambil oleh sebuah kernel ketika alih konteks antar proses
Alih konteks (context switching) adalah proses ketika kernel menyimpan status proses yang sedang berjalan agar dapat dilanjutkan di lain waktu, lalu memuat status proses lain untuk dijalankan. Tindakan yang dilakukan kernel meliputi:

- Menyimpan konteks proses aktif saat ini (register CPU, program counter).
- Memperbarui Process Control Block (PCB) proses yang sedang dihentikan.
- Memuat data konteks dari proses yang akan dijalankan.
- Mengatur ulang memori (jika diperlukan).
- Mengatur ulang register untuk eksekusi proses baru.

### 4. Informasi apa saja yang disimpan pada tabel proses saat alih konteks
Informasi yang disimpan pada Process Control Block (PCB) saat alih konteks antara lain:

- Register CPU (general dan special purpose)
- Program Counter
- Stack Pointer
- Status proses (ready, running, waiting, dll)
- Informasi memori (page table, base & limit register)
- Informasi akun pengguna (UID, GID, dll)
- Daftar I/O dan file yang digunakan

---

## 2.11.2. Thread

### 3. Sebutkan dua perbedaan antara user level thread dan kernel thread

| Aspek                    | User-Level Thread                         | Kernel-Level Thread                      |
|--------------------------|-------------------------------------------|------------------------------------------|
| Manajemen Thread         | Dikelola oleh pustaka pengguna            | Dikelola langsung oleh kernel            |
| Kecepatan Switching      | Cepat, tidak perlu interupsi kernel       | Lebih lambat karena butuh interupsi      |

**Kapan lebih baik:**
- *User-level thread*: Saat membutuhkan banyak thread ringan dan tidak sering melakukan blocking I/O.
- *Kernel-level thread*: Saat program perlu menjalankan thread secara paralel pada banyak inti CPU (multi-core).

### 4. Jelaskan tindakan kernel saat alih konteks antar kernel-level thread

Kernel akan:

- Menyimpan isi register dan program counter dari thread lama.
- Memuat isi register dan program counter dari thread baru.
- Memperbarui scheduler kernel.
- Menyesuaikan cache dan TLB jika diperlukan.

---

## 2.11.3. Penjadwalan CPU

### 3. Keuntungan penggunaan time quantum berbeda dalam sistem multilevel queue

Beberapa manfaat dari penggunaan time quantum yang berbeda pada tiap level antrian adalah:

- Menyesuaikan prioritas: Proses interaktif (respon cepat) diberi quantum kecil, batch process diberi quantum besar.
- Menghindari starvation: Proses prioritas rendah tetap mendapat giliran.
- Efisiensi: Mengurangi frekuensi context switch pada proses berat.
- Fleksibilitas: Sistem dapat disesuaikan dengan jenis proses.

### 4. Ilustrasi Eksekusi Proses (Gantt Chart)

**Diketahui:**

| Proses | Burst Time | Prioritas |
|--------|------------|-----------|
| P1     | 10         | 3         |
| P2     | 1          | 1         |
| P3     | 2          | 4         |
| P4     | 1          | 4         |
| P5     | 5          | 2         |

Semua proses datang di waktu t = 0

#### a) FCFS (First Come First Serve)
Gantt Chart:
```
P1 | P2 | P3 | P4 | P5
0   10  11  13  14  19
```

#### b) SJF (Shortest Job First)
Urutan eksekusi berdasarkan burst time: P2, P4, P3, P5, P1
```
P2 | P4 | P3 | P5 | P1
0   1   2   4   9   19
```

#### c) Prioritas Non-Preemptive
Urutan berdasarkan prioritas (1 = tertinggi): P2, P5, P1, P3, P4
```
P2 | P5 | P1 | P3 | P4
0   1   6   16  18  19
```

#### d) Round Robin (Quantum = 2)
Eksekusi dengan quantum 2:
```
P1 | P2 | P3 | P4 | P5 | P1 | P5 | P1 | P5 | P1 | P1
0   2   3   5   6   8   10  12  13  15  17  19
```
