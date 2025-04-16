
## Practice Exercises 4.1 – 4.5  
---

### 4.1  
**Question:**  
Provide two programming examples in which multithreading provides better performance than a single-threaded solution.

**Answer:**  
1. **Web Server**: A multithreaded web server can handle multiple client requests simultaneously. While one thread handles a request, others can process additional incoming connections, resulting in better throughput.

2. **Multimedia Application**: An application like a video player can have one thread decoding video, another for audio, and another for rendering. This separation allows smoother performance compared to a single-threaded approach where tasks would block each other.

---

### 4.2  
**Question:**  
What are two differences between user-level threads and kernel-level threads?

**Answer:**  
1. **Management**:  
   - *User-level threads* are managed by user-space libraries without kernel involvement.  
   - *Kernel-level threads* are managed directly by the operating system.

2. **Context Switching**:  
   - *User-level threads* have faster context switching since it does not require mode switching to the kernel.  
   - *Kernel-level threads* require a system call to switch, making the operation slower.

---

### 4.3  
**Question:**  
Provide two programming examples where multithreading does not provide better performance than a single-threaded solution.

**Answer:**  
1. **Simple Calculator Program**: For basic arithmetic computations without concurrency, the overhead of thread creation may outweigh any performance benefit.

2. **Sequential File Compression**: When compressing a small file, using multiple threads can introduce unnecessary complexity and overhead, making it less efficient than a single-threaded version.

---

### 4.4  
**Question:**  
Describe the actions taken by a thread library to context switch between user-level threads.

**Answer:**  
The thread library:
1. Saves the context (registers, program counter, stack pointer) of the currently running thread.
2. Selects the next thread to run using a scheduling algorithm.
3. Restores the saved context of the selected thread.
This process is done entirely in user space, without kernel intervention.

---

### 4.5  
**Question:**  
Suppose that threads A and B belong to the same process. Can they communicate with one another? If so, how?

**Answer:**  
Yes, threads A and B can communicate directly because they share the same address space. They can use shared variables in memory. However, proper synchronization mechanisms like mutexes or semaphores are needed to avoid race conditions.

---
