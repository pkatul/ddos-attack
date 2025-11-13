## Test system load ( from my mac )

wrk --latency -t24 -c5000 -d200s \
"https://api.bapsoj.org/api/judge/contests/?offset=0&limit=200"

📌 Meaning of flags:

--latency → show detailed latency distribution

-t24 → 24 threads (more CPU usage on your machine)

-c5000 → 5000 open connections

-d200s → run for 200 seconds

💡 What it actually does

This command sends requests:

👉 as fast as your machine + network + Cloudflare + server can go

There is no throttle, no RPS limit, no pacing.

The output depends entirely on what the server can handle.

🎯 Purpose

“Full throttle” stress test

Maximum throughput

Used to see breaking point

Useful for peak load behavior

---

Note: We might get error like below

```
unable to create thread 13: Too many open files
```

This error comes from macOS, not wrk:

It means your system hit the file descriptor limit → every thread & every TCP connection consumes file descriptors.

To run wrk with high concurrency (2000–5000 connections) you MUST raise your OS limits.

Here is the exact fix that works on macOS.

✅ Step 1 — Set higher file descriptor limit in this terminal

Run:

```
ulimit -n 65536
```

## Test with more request

wrk --latency -t200 -c50000 -d200s \
"https://api.bapsoj.org/api/judge/contests/?offset=0&limit=200"

-> This will have 200 threads and 50000 connections and run for 200 seconds
