## linux-hC

This repository is primarly for my personal use. linux-hC and linux-hC-rc uses the PKGBUILD provided by [CachyOS](https://github.com/CachyOS/linux-cachyos)

I modified it slightly for my personal use and I use patches from ZEN/Cachy/Clear/Xanmod/pf-kernel to build a kernel for my liking. You find the repositories for these patches in my linux kernel [REPO](https://github.com/hellsgod/linux)

### INFORMATION
- I don't support LTS. I don't support kernel versions until EOL. After each cycle (RC gets stable) I will drop support for the previous stable.
- Most of the time I'm on the RC variant and most development will be done there.
- I don't add features I don't need.
- It's not my intention to make a kernel for everyone.

### linux-hC and linux-hC-rc
- linux-hC is for stable releases
- linux-hC-rc is for RC releases - bleeding edge

### Features:
- Default built with BORE (Burst-Oriented Response Enhancer) by Masahito Suzuki: [Repository](https://github.com/firelzrd/bore-scheduler)
- Default built with Clang 22 and thinLTO
- Default built with a custom set of Compiler Optimization Flags - Optimized for efficiency, latency and performance - Optimized for my 9950X
- AMD P-State Preferred Core / amd-pstate Enhancements and Fixes
- Memory management tweaks from zen-kernel (compaction, watermark)
- Cherry-picked fixes and tweaks from Clear-linux, ZEN, CachyOS and pf-kernel
- le9uo working set protection: [Repository](https://github.com/firelzrd/le9uo)
- cpuidle TEO instead of menu
- Various fixes and patches from upstream

### If you want to build this kernel, you have to do following steps:

#### Step 1:
Set your cpu optimization in the PKGBUILD `${_processor_opt:=}` - I have set it to zen4 for myself. Set it to native to optimize it for your cpu, or generic to use the kernel on different devices.
#### NOTE:
I dont use graysky2's kernel_compiler_patch [Repository](https://github.com/graysky2/kernel_compiler_patch) - So the only options you can set are: zen4, native, or generic.

#### Step 2:
Remove my custom compiler path under `BUILD_FLAGS:`
```
CC=/home/hellsgod/clang22/bin/clang
LD=/home/hellsgod/clang22/bin/ld.lld
AR=/home/hellsgod/clang22/bin/llvm-ar
NM=/home/hellsgod/clang22/bin/llvm-nm
STRIP=/home/hellsgod/clang22/bin/llvm-strip
OBJCOPY=/home/hellsgod/clang22/bin/llvm-objcopy
OBJDUMP=/home/hellsgod/clang22/bin/llvm-objdump
RANLIB=/home/hellsgod/clang22/bin/llvm-ranlib
READELF=/home/hellsgod/clang22/bin/llvm-readelf
AS=/home/hellsgod/clang22/bin/llvm-as
```
#### Step 3:
I recommend to use this custom built Full LTO, PGO and BOLT optimized Clang 22, which you can find here: [CLANG](https://github.com/Mandi-Sa/clang/releases)

#### Step 4:
#### CAUTION: DO NOT USE MY compiler-opt.patch if you don't use a zen4/5 CPU!!
Run `makepkg -si` to install dependencies and build the kernel. You can also modify the PKGBUILD to change various options to your liking.

### Nvidia module:
linux-hC will compile the nvidia closed module, since my 3090 runs better with it. You can choose the open module if you want, or disable it in PKGBUILD, if you don't use a nvidia card.
linux-hC-rc will also compile the nvidia closed module, since it works for now. You can disable it in PKGBUILD.

### DKMS Modules:
If you need some dkms modules, they probably won't work. They fall back to the system wide compiler, which is clang 20. So you either use my linked clang 22 system wide, or use a hack to tell dkms to compile the modules with clang 22.

