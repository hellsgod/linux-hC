# linux-hC

A personal Linux kernel built for my own use, based on the CachyOS PKGBUILD and a selection of patches from various upstream and community projects.

The goal is to create a responsive and efficient kernel tailored to my hardware and preferences, rather than a general-purpose kernel intended for everyone.

This repository is primarily maintained for my personal use. I make it publicly available for anyone interested in experimenting with it.

## About

`linux-hC` and `linux-hC-rc` use a modified version of the [CachyOS PKGBUILD](https://github.com/CachyOS/linux-cachyos).

I integrate selected patches and optimizations from projects such as ZEN, CachyOS, Clear Linux, XanMod, pf-kernel, and independent kernel developers.

My [kernel source repository](https://github.com/hellsgod/linux) contains separate branches for each kernel version, where I maintain the changes included in my patch collections, such as `base.patch` and `fixes.patch`.

Each change is kept as an individual commit, preserving the original author's attribution and commit message. This allows anyone interested to browse the corresponding branches and see exactly which changes are included, rather than having to dig through large patch files.

The patch files themselves are generated using `git diff`, as I previously encountered issues with `git format-patch` during the PKGBUILD build process.

Patches from [firelzrd](https://github.com/firelzrd), including BORE, poc-selector, NAP, Cambyses, and lru_cast, are maintained by their original developer and integrated separately.

`linux-hC` is a personal project, developed primarily for my own use and tailored to my hardware, preferences, and interests.

I enjoy experimenting with different approaches to kernel development, learning from other developers, and sharing ideas and improvements along the way.

I'm happy to make my work publicly available and always appreciate constructive feedback, bug reports, and technical discussions.

However, I prefer to keep this project as a hobby rather than turn it into a community-maintained kernel. Features and patches are selected based on my own interests and requirements, and I don't intend to provide ongoing support or accommodate individual feature requests.

You're welcome to use, modify, and experiment with it to suit your own needs.

## Design Philosophy

My goal with `linux-hC` is not to achieve maximum performance through aggressive tuning or brute-force optimizations, but to make better use of the available hardware by reducing unnecessary overhead and improving efficiency.

I primarily focus on three areas:

- **Responsiveness:** Keeping the system responsive and interactive, even under heavy workloads and high CPU utilization.
- **Efficiency:** Improving how system resources are managed and utilized, rather than simply pushing the hardware harder.
- **Practical performance:** Prioritizing consistent behavior and a smooth everyday experience over synthetic benchmark results.

I prefer targeted changes to scheduling, load balancing, CPU idle behavior, and memory management that fit this approach.

Patches are selected based on their design, potential benefits, and relevance to my own usage. I'm also willing to experiment with newer approaches, even if their benefits are not immediately measurable.

I don't believe that more features necessarily make a better kernel. Instead, I focus on a carefully selected set of changes that serve a clear purpose, rather than including every available tweak or optimization simply because it exists.

Most testing is done through real-world usage, including gaming, multitasking, and compiling large projects under full CPU load. Feedback from other users and different hardware configurations is also valuable.

I do not claim that every modification provides a measurable performance improvement, nor do I aim to outperform other kernels in benchmarks.

Ultimately, `linux-hC` is about finding a balance between responsiveness, efficiency, and stability that works well for my everyday use.

## Kernel Versions

| Package | Description |
|---|---|
| `linux-hC` | Stable upstream kernel releases |
| `linux-hC-rc` | Release candidates and experimental development |

**Development and support policy:**

- Most development takes place on the RC branch.
- I generally use the latest release candidate on my own system.
- When a new kernel series reaches stable, support for the previous stable series is dropped.
- No LTS versions are maintained.
- Kernel versions are not necessarily maintained until their upstream EOL.
- Features are added or removed based on my personal requirements and testing.
- Compatibility with other hardware configurations is not guaranteed.

This is not intended to be a kernel for everyone.

## Features

### Scheduling and CPU Management

- **BORE (Burst-Oriented Response Enhancer)** — Default scheduler enhancements by Masahito Suzuki. [Repository](https://github.com/firelzrd/bore-scheduler)
- **poc-selector** — Optimized idle CPU selection. [Repository](https://github.com/firelzrd/poc-selector)
- **Cambyses** — Custom load-balancing optimizations. [Repository](https://github.com/firelzrd/cambyses)
- **TEO** — Default CPU idle governor instead of menu.
- **NAP** — Alternative CPU idle governor, compiled into the kernel but requiring explicit activation through the kernel command line. [Repository](https://github.com/firelzrd/nap)

### Memory Management

- Selected memory management tweaks from ZEN, including compaction and watermark adjustments.
- **lru_cast** — Cost-Aware Self-Tuning LRFU page replacement algorithm. [Repository](https://github.com/firelzrd/lru_cast)

### Compiler and Build Configuration

- Built using my custom **LLVM/Clang 24** toolchain. [Repository](https://github.com/hellsgod/clang)
- ThinLTO enabled by default.
- Rust support enabled.
- Custom compiler optimization flags targeting my AMD Ryzen 9 9950X.
- Additional compiler and build configuration adjustments for my personal setup.

### Additional Patches

- Selected fixes and optimizations from Clear Linux, ZEN, CachyOS, XanMod, and pf-kernel.
- Various upstream fixes and patches.

The exact patch selection may change between kernel versions.

## Building the Kernel

The PKGBUILD is configured for my personal system by default. **You should review and adjust the configuration before building it on different hardware.**

### 1. CPU Optimization

Set the following variable in the PKGBUILD:

`"${_processor_opt:=zen4}"`

Available options:

| Option | Description |
|---|---|
| `zen4` | Optimization settings for Zen 4 |
| `native` | Optimize for the CPU used during compilation |
| `generic` | More portable configuration for different CPUs |

I do not use [graysky2's kernel_compiler_patch](https://github.com/graysky2/kernel_compiler_patch), so these are the only supported options.

### 2. LLVM/Clang Configuration

By default, the PKGBUILD uses my custom LLVM/Clang installation.

To use the system LLVM/Clang toolchain instead, set:

`"${_custom_llvm=no}"`

To use a custom LLVM/Clang installation, enable it:

`"${_custom_llvm=yes}"`

Then adjust the following paths to match your installation:

```bash
"${_custom_llvm_path=/home/hellsgod/clang24/bin}"
"${_custom_libclang_path=/home/hellsgod/clang24/lib}"
```

The custom libclang path is used for Rust-related build components.

### 3. Compiler Optimization Patch

**WARNING: My `compiler-opt.patch` is intended for Zen 4/Zen 5 CPUs. Do not use it on other CPU architectures without reviewing and adjusting its contents.**

Changing `_processor_opt` alone does not necessarily make the compiler optimization patch compatible with other CPUs.

### 4. Build and Install

After reviewing the configuration, build and install the kernel using:

```bash
makepkg -si
```

You can modify the PKGBUILD to enable or disable additional features according to your requirements.

## NVIDIA Open Kernel Module

Both `linux-hC` and `linux-hC-rc` can compile the NVIDIA open kernel module as part of the kernel build process.

This is enabled by default for my personal setup.

To disable it, set:

`"${_build_nvidia_open=no}"`

The module is built for the corresponding kernel version.

## Disclaimer

This kernel is provided **as-is**, without any warranty or guarantee of stability, compatibility, or performance.

It is developed and tested primarily on my own hardware. Experimental patches, release candidates, and custom compiler optimizations may introduce regressions or unexpected behavior.

Use it at your own risk.

## Credits

Thanks to the Linux kernel developers, CachyOS, ZEN, Clear Linux, XanMod, pf-kernel, and the independent developers whose work makes this project possible.

All credit for individual patches and features belongs to their respective authors and maintainers.
