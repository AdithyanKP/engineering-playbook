# Node.js Worker Threads

- Node.js executes JavaScript on a **single main JavaScript thread** by default.
- If a **long-running CPU-bound task** is executed on the main thread, it **blocks the event loop**, preventing other JavaScript code from running.
- To avoid blocking the main thread, you can **explicitly create a Worker Thread** and run the CPU-intensive task there.
- Worker threads run in **separate JavaScript runtimes** (each has its own V8 instance, event loop, and call stack).
- Worker threads can communicate with the main thread using **message passing**.
- By default, workers do **not** share JavaScript memory. However, memory can be shared using **`SharedArrayBuffer`** and synchronization primitives like **`Atomics`**.
- Worker threads share the same **process** and **operating system resources**, making them more lightweight than creating separate processes.
