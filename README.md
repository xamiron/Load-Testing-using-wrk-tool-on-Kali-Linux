# Load-Testing-using-wrk-tool-on-Kali-Linux

This document provides a complete guide for setting up and performing **HTTP Load Testing** using the `wrk` tool on **Kali Linux** or **Ubuntu**.
It includes installation steps, testing strategy, commands, and performance analysis methods for web applications.

---

## Table of Contents

1. [Overview](#overview)
2. [Requirements](#requirements)
3. [Installation Steps](#installation-steps)

   * [Ubuntu / Debian](#for-ubuntu--debian)
   * [Kali Linux](#for-kali-linux)
4. [Test Setup](#test-setup)
5. [Test Types and Purpose](#test-types-and-purpose)
6. [Example Commands](#example-commands)
7. [Concurrency and Duration Scenarios](#concurrency-and-duration-scenarios)
8. [Sample Test Output](#sample-test-output)
9. [How to Analyze Results](#how-to-analyze-results)
10. [Conclusion](#conclusion)

---

## Overview

**wrk** is a modern HTTP benchmarking tool that measures how many requests per second a web server can handle.
It supports multithreading and concurrent connections, making it suitable for:

* Stress Testing
* Load Testing
* Stability and Performance Validation

---

## Requirements

Before installing or running tests, ensure you have:

* **Operating System:** Kali Linux / Ubuntu (Debian-based)
* **Tools Needed:**

  * `git`
  * `build-essential`
  * `libssl-dev`
* **Internet Access:** For cloning and dependency installation
* **Sudo Privileges:** To build and install binaries
* **Target URL:** Application endpoint for testing
  Example: `https://sabuj-modak-qa.vercel.app/`

---

## Installation Steps

### For Ubuntu / Debian

```bash
sudo apt update
sudo apt install build-essential libssl-dev git -y
git clone https://github.com/wg/wrk.git
cd wrk
sudo make
sudo cp wrk /usr/local/bin
wrk --version
```

### For Kali Linux

```bash
sudo apt update
sudo apt install build-essential libssl-dev git -y
git clone https://github.com/wg/wrk.git
cd wrk
make
sudo cp wrk /usr/local/bin
wrk --version
```

**Check Installation:**

```bash
wrk --version
```

If you see an output like `wrk 4.2.0`, the setup is successful.

---

## Test Setup

Target URL for this example:

```
https://sabuj-modak-qa.vercel.app/
```

Purpose:

* Evaluate website performance
* Measure latency and throughput
* Validate stability under concurrent user load

---

## Test Types and Purpose

| Test Type                      | Duration      | When to Use                  | Purpose                                                 |
| ------------------------------ | ------------- | ---------------------------- | ------------------------------------------------------- |
| **Smoke Test**                 | 1–2 minutes   | Quick check after deployment | Verify if the server responds under light load          |
| **Stress Test**                | 5–10 minutes  | To find system limits        | Identify where the app starts to fail (timeouts/errors) |
| **Stability / Endurance Test** | 15–60 minutes | For long-run scenarios       | Check if the app remains stable over time               |
| **Production Readiness Test**  | 10–15 minutes | Before live launch           | Validate real-world steady-state load                   |

---

## Example Commands

### Basic Load Test

```bash
wrk -t4 -c25 -d60s https://sabuj-modak-qa.vercel.app/
```

**Explanation:**

* `-t4` → Use 4 threads
* `-c25` → 25 concurrent connections
* `-d60s` → Run test for 60 seconds

---

## Concurrency and Duration Scenarios

| Concurrency | Duration   | Command                                                    |
| ----------- | ---------- | ---------------------------------------------------------- |
| 100 Users   | 2 minutes  | `wrk -t4 -c100 -d120s https://sabuj-modak-qa.vercel.app/`  |
| 250 Users   | 5 minutes  | `wrk -t4 -c250 -d300s https://sabuj-modak-qa.vercel.app/`  |
| 500 Users   | 8 minutes  | `wrk -t4 -c500 -d500s https://sabuj-modak-qa.vercel.app/`  |
| 1000 Users  | 10 minutes | `wrk -t4 -c1000 -d600s https://sabuj-modak-qa.vercel.app/` |

---

## Sample Test Output

Example output (25 concurrent users, 60 seconds):

```
Running 1m test @ https://sabuj-modak-qa.vercel.app/
  4 threads and 25 connections
  Thread Stats   Avg      Stdev     Max   +/- Stdev
    Latency   729.91ms  308.65ms   1.98s    77.08%
    Req/Sec     8.12      6.02    40.00     78.82%
  1474 requests in 1.00m, 48.56MB read
  Socket errors: connect 0, read 0, write 0, timeout 152
  Non-2xx or 3xx responses: 1388
Requests/sec:     24.53
Transfer/sec:    827.42KB
```

---

## How to Analyze Results

| Metric            | Meaning                             | Expected Behavior                |
| ----------------- | ----------------------------------- | -------------------------------- |
| **Requests/sec**  | Total requests processed per second | Higher = Better performance      |
| **Latency**       | Average response time               | Lower = Faster responses         |
| **Socket Errors** | Failed connections or timeouts      | Should be minimal                |
| **Transfer/sec**  | Data transferred per second         | Depends on page size and content |

---

## Conclusion

The **wrk** tool is ideal for:

* Measuring web application performance
* Identifying bottlenecks under high load
* Ensuring stability before production release

For best results:

* Gradually increase concurrency (e.g., 25 → 1000)
* Monitor latency, error rates, and throughput
* Use results to optimize backend performance and server configuration
