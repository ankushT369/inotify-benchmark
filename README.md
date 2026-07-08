# inotify-benchmark

`inotify` is a Linux file event notification system used to monitor filesystem events. It allows applications to receive notifications whenever a monitored file or directory is modified.

In this benchmark, I measured the performance of `inotify` by generating write events on multiple files simultaneously.

## Benchmarking Setup

### Test Environment

* **OS:** Ubuntu 22.04 LTS (64-bit)
* **Architecture:** x86_64
* **CPU:** Intel Core i3-1125G4 (11th Gen)
* **Cores / Threads:** 4 Cores / 8 Threads
* **Base Frequency:** 2.00 GHz
* **Turbo Frequency:** Up to 3.70 GHz
* **Memory:** 8 GB RAM (7.4 GiB usable)
* **Swap:** 27 GiB
* **CPU Cache:** L1 (192 KiB Data + 128 KiB Instruction), L2 (5 MiB), L3 (8 MiB)
* **Virtualization:** Intel VT-x

## Benchmark 1

In this benchmark, I used **128 writer threads (fixed)** that simultaneously wrote to **128, 256, 512, and 1024 files**, respectively. All files were monitored by an `inotify` program for two events:

* `IN_MODIFY` – Generated whenever a monitored file is modified.
* `IN_Q_OVERFLOW` – Generated when the kernel's `inotify` event queue overflows because events are produced faster than they can be processed.

In the graphs below:

* The **x-axis** represents the maximum event buffer size used by the program (in **KiB**).
* The **left y-axis** represents the **throughput**.
* The **right y-axis** represents the **total number of events processed**.

> **Note:** The buffer sizes shown in the graphs are in **KiB**.

![image1](https://raw.githubusercontent.com/ankushT369/inotify-benchmark/refs/heads/main/images/inotify_benchmark_128_files.png)

![image2](https://github.com/ankushT369/inotify-benchmark/blob/main/images/inotify_benchmark_256_files.png?raw=true)

![image3](https://github.com/ankushT369/inotify-benchmark/blob/main/images/inotify_benchmark_512_files.png?raw=true)

![image4](https://github.com/ankushT369/inotify-benchmark/blob/main/images/inotify_benchmark_1024_files.png?raw=true)

As shown in the graphs, the throughput increases as the maximum event buffer size increases. This is because a larger buffer allows more events to be read in a single `read()` system call, reducing system call overhead.

## Benchmark 2

In this benchmark, I increased the event buffer size to **8 MiB**, **16 MiB**, and **32 MiB**.

The results are interesting. Unlike the first benchmark, there is very little difference in throughput between these buffer sizes. This suggests that the application has reached a saturation point where increasing the read buffer no longer improves performance. At this stage, the bottleneck is likely another component of the system, such as the kernel's internal event queue, CPU scheduling, or the application's event processing rate.

![image5](https://raw.githubusercontent.com/ankushT369/inotify-benchmark/refs/heads/main/images/throughput_bar_chart.png)

## Conclusion

The benchmarks show that increasing the event read buffer significantly improves the throughput of an `inotify`-based application when the buffer size is relatively small. However, the performance gain gradually diminishes as the buffer becomes larger.

In the second benchmark, increasing the buffer size beyond **8 MiB** produced almost no improvement in throughput, indicating that the application had reached a saturation point. This suggests that the read buffer is no longer the primary bottleneck, and other factors—such as the kernel's event queue, CPU scheduling, or event processing overhead—are likely limiting performance.

Overall, these results indicate that using a sufficiently large event buffer is important for achieving good performance, but allocating excessively large buffers offers little additional benefit.

