# Benchmarks

## SSDs

Benchmarked using [Black Magic Disk Speed Test](https://apps.apple.com/gb/app/blackmagic-disk-speed-test/id425264550?mt=12) on macOS and Windows.

| Drive | Write (MB/s) | Read (MB/s) |
| --- | --- | --- |
| 2TB Kingston SSD (from macOS) | 465 | 527 |
| 2TB Kingston SSD (from Windows) | 477| 527 |
| 1TB WD BLACK (macOS) | 2848 | 2922 |
| 1TB WD BLACK with heatsink (Windows) | 2799 | 2908 |

## CPU

Undervolted with an offset of -100mV. Benchmarked using [Cinebench](https://www.techspot.com/downloads/6709-cinebench.html) and [Geekbench](https://www.geekbench.com/index.html).

| Benchmark | Single Core | Multi Core |
| --- | --- | --- |
| Cinebench R23 (macOS) | 1225 | 12859 |
| Cinebench R23 (Windows) | 1208 | 12243 |
| Geekbench 5 (macOS) | 1271 | 9091 |
| Geekbench 5 (Windows) | 1281 | 8932 |

## GPU

Undervolted using AMD Radeon Software's auto undervolt feature. Benchmarked using [Geekbench](https://www.geekbench.com/index.html) (OpenCL).  Might need to use the kext that improves 5700 XT performance on macOS?

| Benchmark | Score |
| --- | --- |
| Geekbench compute (macOS) | 65687 |
| Geekbench compute (Windows) | 78243 |
