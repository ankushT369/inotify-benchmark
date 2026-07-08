# inotify-benchmark
`inotify` is a linux file event notification system it is used for getting any event if is occuring on the monitored filesystem.
In this benchmark I tried to measure its performance by writing on many files simultaneously.

## Test Environment
- **OS:** Ubuntu 22.04 LTS (64-bit)
- **Architecture:** x86_64
- **CPU:** Intel Core i3-1125G4 (11th Gen)
- **Cores / Threads:** 4 Cores / 8 Threads
- **Base Frequency:** 2.00 GHz
- **Turbo Frequency:** Up to 3.70 GHz
- **Memory:** 8 GB RAM (7.4 GiB usable)
- **Swap:** 27 GiB
- **CPU Cache:** L1 (192 KiB Data + 128 KiB Instruction), L2 (5 MiB), L3 (8 MiB)
- **Virtualization:** Intel VT-x
