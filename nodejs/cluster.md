## PM2 Cluster Mode

**PM2 cluster mode** runs multiple instances of the same Node.js application as separate processes. It uses Node.js's **cluster module** to distribute incoming requests among these processes, allowing the application to:

- Utilize multiple CPU cores
- Improve request throughput
- Increase application availability and fault tolerance

Each worker process:

- Has its own **memory space**
- Has its own **Node.js event loop**
- Runs independently of other workers

If a worker process crashes, **PM2 automatically restarts it**, helping keep the application available with minimal downtime.
