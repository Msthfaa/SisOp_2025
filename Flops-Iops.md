# Benchmarking CPU dengan FLOPS-IOPS pada Ryzen 7 4000 Series

## Langkah-langkah

### 1. Buka terminal Linux dan lakukan clone repository flops-iops:
```sh
git clone https://github.com/ferryastika/flops-iops.git
```

### 2. Masuk ke direktori flops-iops:
```sh
cd flops-iops
```

### 3. Build Program Benchmark:
```sh
make
```

### 4. Jalankan pengujian FLOPS dan IOPS:
```sh
./iops32 $(nproc)
./iops64 $(nproc)
./flops32 $(nproc)
./flops64 $(nproc)
```

## Hasil
### CPU: AMD Ryzen 7 7435HS

#### 32 Bit Integer Operations Per Second
```sh
./iops32 $(nproc)
```
```
Benchmarking for 32 Bit Integer operations per second
1 | Tr 1: 15477088351 Tr 2: 15391333267 Tr 3: 15485343585 IOPS = 46353765203
Maximum CPU Throughput: 46.353764 Gigaiops.
Maximum Single Core Throughput: 15.485844 Gigaiops.
```

#### 64 Bit Integer Operations Per Second
```sh
./iops64 $(nproc)
```
```
Benchmarking for 64 Bit Integer operations per second
1| Tr 1: 43473578720 Tr 2: 43305846454 Tr 3: 43555439304 IOPS = 130334864478
Maximum CPU Throughput: 130.334869 Gigaiops.
Maximum Single Core Throughput: 43.555439 Gigaiops.
```

#### 32 Bit Floating Point Operations Per Second
```sh
./flops32 $(nproc)
```
```
Benchmarking for 32 Bit Floating point operations per second
1| Tr 1: 5627075432 Tr 2: 5524270671 Tr 3: 5498892562 FLOPS = 16650238665
Maximum CPU Throughput: 16.650238 Gigaflops.
Maximum Single Core Throughput: 5.627076 Gigaflops.
```

#### 64 Bit Floating Point Operations Per Second
```sh
./flops64 $(nproc)
```
```
Benchmarking for 64 Bit Floating point operations per second
1| Tr 1: 11557878748 Tr 2: 11205021022 Tr 3: 10784198178 FLOPS = 33547097948
Maximum CPU Throughput: 33.547096 Gigaflops.
Maximum Single Core Throughput: 11.557878 Gigaflops.
```

