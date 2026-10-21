# ILP32-A64

**Bringing AArch64 ILP32 back into focus.**

## Why ILP32?
Embedded systems run on tight memory budgets, often 128–512 MB, and DRAM keeps getting more
expensive as AI data centers absorb more and more of the supply. On these devices, every megabyte counts.

The classic answer was 32-bit Arm (armhf). It is going away: recent Arm cores
(Cortex-A715/A720, Cortex-A520, Cortex-X2 and later) no longer run AArch32 code.

AArch64 ILP32 runs 64-bit ARM code with 32-bit `int`, `long`, and pointers:
- Keeps the full AArch64 instruction set and all 31 general-purpose registers
- Smaller pointers and data structures: less memory, better cache use
- Runs on AArch64-only cores, where armhf is no longer an option

## The Challenge
ILP32 needs a complete ecosystem, and upstream support has stayed limited: the Linux kernel's
ILP32 ABI was never merged into mainline, and the glibc port never left the sourceware `arm/ilp32`
branch. We keep these pieces working and usable, and work toward upstream acceptance.

## What's Here
| Repository | Content |
|---|---|
| [linux-ilp32](https://github.com/ilp32-a64/linux-ilp32) | Linux with the ILP32 ABI: the openEuler ILP32 series on vanilla 5.10, 6.6 and 6.12, with our fixes (vDSO, kernels without AArch32) |
| [glibc-ilp32](https://github.com/ilp32-a64/glibc-ilp32) | glibc 2.30 ILP32 port (sourceware `arm/ilp32`) with fixes; a port to glibc 2.41 is in progress |
| [musl-ilp32](https://github.com/ilp32-a64/musl-ilp32) | musl 1.2.5 with the AArch64 ILP32 port (`aarch64_ilp32`) |
| [openwrt-ilp32](https://github.com/ilp32-a64/openwrt-ilp32) | OpenWrt 25.12 with an AArch64 ILP32 (musl) user-land toolchain option; kernel integration in progress |
| [test-ilp32](https://github.com/ilp32-a64/test-ilp32) | ILP32 cross toolchain, test programs and a QEMU test harness |

LLVM/Clang support is in progress.

## Where to Start
| You want to | Start with |
|---|---|
| Try ILP32: build the toolchain and a kernel, run the tests in QEMU (glibc) | [test-ilp32](https://github.com/ilp32-a64/test-ilp32) |
| Build ILP32 user space with OpenWrt (musl) | [openwrt-ilp32](https://github.com/ilp32-a64/openwrt-ilp32) |
| Use only the kernel or only a C library | [linux-ilp32](https://github.com/ilp32-a64/linux-ilp32), [glibc-ilp32](https://github.com/ilp32-a64/glibc-ilp32), [musl-ilp32](https://github.com/ilp32-a64/musl-ilp32) |

## Known Limitations
- **Year 2038**: the glibc 2.30 port has a 32-bit `time_t`; the musl port and the glibc 2.41 port in progress use a 64-bit `time_t`.
- **Language runtimes**: Java, Node.js, Python and other runtimes are not fully supported yet.

## Our Goals
- Publish open benchmarks and methodology quantifying ILP32's memory savings
- Maintain ILP32 support across the kernel, C libraries and toolchains, and work toward upstream acceptance
- Provide reproducible build environments and examples

## Get Involved
Issues, pull requests, and benchmark results from your own embedded platforms are all welcome.
