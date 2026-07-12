# Node.js Child Processes

- A **Child Process** in Node.js creates a new **operating system process**.
- Each child process runs in a **completely isolated environment** with its own:
  - Process ID (PID)
  - Memory space
  - V8 JavaScript engine
  - Event loop
- Child processes are useful for:
  - Running CPU-intensive tasks without blocking the parent process.
  - Executing programs written in other languages (e.g., Python, Java, or C++).
  - Running shell commands.
  - Building multi-process applications, such as with the `cluster` module.
- A child process inherits some properties from its parent, such as environment variables, the current working directory, and standard input/output streams (depending on configuration). However, it **does not share the parent's JavaScript memory or state**.
- Communication between the parent and child process is typically done using **Inter-Process Communication (IPC)**.
