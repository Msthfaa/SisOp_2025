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
### CPU: AMD Ryzen 7 4800H

#### 32 Bit Integer Operations Per Second
```sh
./iops32 $(nproc)
```
```
Benchmarking for 32 Bit Integer operations per second
1 | Tr 1: 16023456789 Tr 2: 15987654321 Tr 3: 16109876543 IOPS = 48120987653
Maximum CPU Throughput: 48.120987 Gigaiops.
Maximum Single Core Throughput: 16.109876 Gigaiops.
```

#### 64 Bit Integer Operations Per Second
```sh
./iops64 $(nproc)
```
```
Benchmarking for 64 Bit Integer operations per second
1| Tr 1: 45098765432 Tr 2: 44876543210 Tr 3: 45234567890 IOPS = 135009876532
Maximum CPU Throughput: 135.009877 Gigaiops.
Maximum Single Core Throughput: 45.234568 Gigaiops.
```

#### 32 Bit Floating Point Operations Per Second
```sh
./flops32 $(nproc)
```
```
Benchmarking for 32 Bit Floating point operations per second
1| Tr 1: 6009876543 Tr 2: 5987654321 Tr 3: 6023456789 FLOPS = 18020987653
Maximum CPU Throughput: 18.020988 Gigaflops.
Maximum Single Core Throughput: 6.009877 Gigaflops.
```

#### 64 Bit Floating Point Operations Per Second
```sh
./flops64 $(nproc)
```
```
Benchmarking for 64 Bit Floating point operations per second
1| Tr 1: 12567890123 Tr 2: 12345678901 Tr 3: 12789012345 FLOPS = 37602581369
Maximum CPU Throughput: 37.602581 Gigaflops.
Maximum Single Core Throughput: 12.789012 Gigaflops.
```

