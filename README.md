# zenoh-bench

## Overview

simple benchmark to investigate low latency mode.

## Install & run

1) clone repo 
2) cd zenoh-bench
3) run sub first:  ```cargo run --bin sub```
4) run pub next:  ```cargo run --bin pub```
5) When pub finishes, est. throughput will be printed out

Files:
* Configuration: [src/lib.rs](src/lib.rs)
* Publisher [src/bin/pub](src/bin/pub/main.rs)
* Subscriber: [src/bin/sub](src/bin/sub/main.rs)

Both, pub and sub share the exact same configuration.

## Observations

General:
* Default config gives good performance
* Release build is nearly twice as fast as debug build due to aggressive optimization. See cargo.toml 

Low latency mode:
* Best throughput when enabled
* With publisher in low latency mode, throughput drops in half
* With pub & sub in low latency mode, throughput drops fivefold (from 500k to ~100k).

## Sample measurement

System:
* Apple Macbook, M1
* Mac OS Sonoma 14.1
* rustc 1.74.0 (79e9716c9 2023-11-13)
* zenoh v0.10.0-rc

Release mode with low latency DISABLED:
```
Max Messages: 1000000 (1 Million)
Elapsed time: 2.338041208s
Throughput: 500000.00 msg/s
```

Release mode with low latency PUB & SUB ENABLED:

```
Max Messages: 1000000 (1 Million)
Elapsed time: 9.321751834s
Throughput: 111111.11 msg/s
```

## Comparison

|            | Low Lat ON | Low Lat OFF | Change  | Percent |
|------------|------------|-------------|---------|---------|
| Time (Sec) | 9.32       | 2.33        | -6.9    | -300%   |
| msg/s      | 111 111    | 500 000     | -388889 | -77.8%  |
