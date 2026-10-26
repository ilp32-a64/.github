# ILP32-A64

**Bringing AArch64 ILP32 back into focus.**

## Why ILP32?
Embedded systems run on tight memory budgets, often 128–512 MB, and DRAM keeps getting more
expensive as AI data centers absorb more and more of the supply. On these devices, every megabyte counts.

The classic answer was 32-bit Arm (armhf). It is going away: recent Arm cores
(Cortex-A715/A720, Cortex-A520, Cortex-X2 and later) no longer run AArch32 code.

AArch64 ILP32 runs 64-bit Arm code with 32-bit `int`, `long`, and pointers:
- Keeps the full AArch64 instruction set and all 31 general-purpose registers
- Smaller pointers and data structures: less memory, better cache use
- Runs on AArch64-only cores, where armhf is no longer an option

32-bit pointers on 64-bit CPUs are not an Arm-only idea: Linux has had the x32 ABI for x86-64
since 2012, Apple has shipped `arm64_32` on Apple Watch since 2018, and the RISC-V psABI has a draft
`RV64ILP32` ABI for RV64 cores.

On a Cortex-A35 board, CPython built for ILP32 uses up to 30% less memory than the same build for
LP64, and runs a few percent slower
([details](https://github.com/ilp32-a64/llvm-ilp32/blob/main/docs/benchmarks.md)).
On OpenWrt One hardware, with a 150,000-domain adblock list, LuCI and Python, the ILP32 user space
uses 25% less memory, leaving 8.6 MB more free
([details](https://github.com/ilp32-a64/openwrt-ilp32#readme)).

## The Challenge
ILP32 needs a complete ecosystem, and upstream support has stayed limited: the Linux kernel's
ILP32 ABI was never merged into mainline, glibc never merged its port, GCC 15 deprecated
`-mabi=ilp32`, and clang never got front-end support. We keep these pieces working and tested
together, with a 64-bit `time_t` throughout.

## What's Here
| Repository | Content |
|---|---|
| [linux-ilp32](https://github.com/ilp32-a64/linux-ilp32) | Linux with the ILP32 ABI: the openEuler ILP32 series on 6.12 stable (`ilp32-6.12.y`), with fixes for the vDSO, kernels without AArch32, pointer authentication, BTI and SVE |
| [glibc-ilp32](https://github.com/ilp32-a64/glibc-ilp32) | glibc 2.41 ILP32 port with a 64-bit `time_t` (`ilp32-2.41`) |
| [musl-ilp32](https://github.com/ilp32-a64/musl-ilp32) | musl 1.2.5 with the AArch64 ILP32 port (`aarch64_ilp32`) |
| [llvm-ilp32](https://github.com/ilp32-a64/llvm-ilp32) | Patches that make clang 22 a working compiler for `aarch64-linux-gnu_ilp32` and `aarch64-linux-musl_ilp32`, interoperable with GCC |
| [openwrt-ilp32](https://github.com/ilp32-a64/openwrt-ilp32) | OpenWrt 25.12 with an AArch64 ILP32 (musl) user land and ILP32 kernel |
| [debian-ilp32](https://github.com/ilp32-a64/debian-ilp32) | ILP32 kernel and runtime packages for *Debian* 13 from a signed apt repository (unofficial, early preview) |
| [test-ilp32](https://github.com/ilp32-a64/test-ilp32) | ILP32 cross toolchains (GCC 14.3 with glibc 2.41 or musl 1.2.5), test programs and a QEMU test harness |

## Where to Start
| You want to | Start with |
|---|---|
| Try ILP32: build the toolchain and a kernel, run the tests in QEMU (glibc or musl) | [test-ilp32](https://github.com/ilp32-a64/test-ilp32) |
| Build ILP32 user space with OpenWrt (musl) | [openwrt-ilp32](https://github.com/ilp32-a64/openwrt-ilp32) |
| Compile with clang | [llvm-ilp32](https://github.com/ilp32-a64/llvm-ilp32) |
| Run ILP32 programs on Debian 13 | [debian-ilp32](https://github.com/ilp32-a64/debian-ilp32) |
| Use only the kernel or only a C library | [linux-ilp32](https://github.com/ilp32-a64/linux-ilp32), [glibc-ilp32](https://github.com/ilp32-a64/glibc-ilp32), [musl-ilp32](https://github.com/ilp32-a64/musl-ilp32) |

## Status
- Tested in QEMU (Cortex-A35/A53 and `-cpu max` with PAC, BTI and SVE) with glibc 2.41, musl 1.2.5
  and OpenWrt 25.12; ILP32 user space also runs on a Cortex-A35 board and on the OpenWrt One.
- GCC has no return address signing (`pac-ret`) for ILP32; clang 22 with our patches does.
- Not built or tested for ILP32 yet: the LLVM runtimes (compiler-rt, libunwind, libc++, sanitizers)
  and language runtimes other than C, C++ and CPython (for example Java and Node.js).

## Our Goals
- Bring ILP32 to more real hardware people already use, such as the Raspberry Pi, not only QEMU
  and single boards
- Publish open benchmarks and methodology quantifying ILP32's memory savings
- Maintain ILP32 support across the kernel, C libraries and toolchains
- Provide reproducible build environments and examples

## Upstreaming
Anyone is welcome to take these patches upstream, whole or in part, under their own name;
please keep the original `Signed-off-by`. Maintainers are welcome too.

## Get Involved
Issues, pull requests, and benchmark results from your own embedded platforms are all welcome.
