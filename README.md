## linux-hC

This repository is primarly for my personal use. linux-hC and linux-hC-rc uses the PKGBUILD provided by [CachyOS](https://github.com/CachyOS/linux-cachyos)

I modified it slightly for my personal use and I use patches from ZEN/Cachy/Clear/Xanmod/pf-kernel to build a kernel for my liking. I do this patches by myself, you find the repositories for these patches in my linux kernel [REPO](https://github.com/hellsgod/linux)

### INFORMATION
- I don't support LTS. I don't support kernel versions until EOL. 
- I probably will support 6.14 until 6.15 is officially out, then it will be dropped. Most of the time I'm on the RC variant and most development will be done there.
- I don't add features I don't need.
- It's not my intention to make a kernel for everyone.

### linux-hC and linux-hC-rc
- linux-hC is for stable releases
- linux-hC-rc is for RC releases - bleeding edge

### Features:
- Default built with BORE (Burst-Oriented Response Enhancer) by Masahito Suzuki: [Repository](https://github.com/firelzrd/bore-scheduler)
- Default built with Clang 21 and thinLTO
- Default built with a custom set of Compiler Optimization Flags
- AMD P-State Preferred Core / amd-pstate Enhancements and Fixes
- Patches to improve performance of CRC32 and AES128 crypto
- Latest & improved ZSTD 1.5.7 patch-set
- Memory management tweaks from zen-kernel (compaction, watermark)
- Cherry-picked fixes and patches from Clear-linux, ZEN, CachyOS and pf-kernel
- le9uo working set protection: [Repository](https://github.com/firelzrd/le9uo)
- cpuidle TEO instead of menu
- Various patches from upstream

### If you want to build this kernel, you have to do following steps:

#### Step 1:
Set your cpu optimization in the PKGBUILD `${_processor_opt:=}` - I have set it to zen4 for myself, or set `${_use_auto_optimization:=yes}` and let it optimize for your cpu automatically.

#### Step 2:
Remove my custom compiler path under `BUILD_FLAGS:`
```
CC=/home/hellsgod/clang21/bin/clang
LD=/home/hellsgod/clang21/bin/ld.lld
AR=/home/hellsgod/clang21/bin/llvm-ar
NM=/home/hellsgod/clang21/bin/llvm-nm
STRIP=/home/hellsgod/clang21/bin/llvm-strip
OBJCOPY=/home/hellsgod/clang21/bin/llvm-objcopy
OBJDUMP=/home/hellsgod/clang21/bin/llvm-objdump
RANLIB=/home/hellsgod/clang21/bin/llvm-ranlib
READELF=/home/hellsgod/clang21/bin/llvm-readelf
AS=/home/hellsgod/clang21/bin/llvm-as
```
#### Step 3:
Install clang 20 or clang 21, because some of my compiler flags only work with clang 20 and up. If you use a clang compiler in your home directory, modify Step 2 to your compiler path. If you don't want to install clang 20/21 and you want to use your system wide clang, which is 19.1.7 on Arch Linux, head over to Step 4.

#### Step 4 (instead of step 3):
Remove following compiler flags from my compiler-opt.patch:
```
-mllvm -codegen-data-thinlto-two-rounds
-mllvm -enable-gvn-memoryssa
-mllvm -enable-dse-initializes-attr-improvement
-mllvm -enable-early-exit-vectorization
```
And you should be able to compile the kernel with clang 19.1.7, which is the system wide version of Arch Linux.

#### Step 5:
Run `makepkg -si` to install dependencies and build the kernel. You can also modify the PKGBUILD to change tickrate, etc to your liking.

### Clang 20:
I use a custom built Full LTO, PGO and BOLT optimized Clang 21, which you can find here: [CLANG](https://github.com/Mandi-Sa/clang/releases)

### Nvidia module for clang 20 and up:
Be aware the nvidia module won't compile for you, if you compile your kernel with clang 20 and up. But there's a workaround for that. If you don't use clang 20 and up as your system wide compiler, the nvidia module will try to compile with the system wide compiler, which is clang 19.1.7 on Arch an will therefore fail.

I'm using the nvidia-dkms closed module, since the open module produces stutters in UI animations for me and I can only give you a workaround for that one. You have to find a way for your used driver by yourself.

I'm refering to the latest nvidia driver 570.86.16: 
Navigate to: `/usr/src/nvidia-570.86.16/Makefile`

In the Makefile you will find the following: 
```
CC ?= cc
LD ?= ld
OBJDUMP ?= objdump
```
It refers to the compiler it reads from the kernel, GCC or Clang. But even if it reads clang from the kernel, it will use your system wide compiler, which you don't want. So, replace it with:
```
ifeq ($(shell echo $(KERNEL_UNAME) | grep -o hC),hC)
    CC := /home/hellsgod/clang21/bin/clang
    LD := /home/hellsgod/clang21/bin/ld.lld
    OBJDUMP=/home/hellsgod/clang21/bin/llvm-objdump
else
  CC ?= cc
  LD ?= ld
  OBJDUMP ?= objdump
endif
```
Instead of just reading the kernel config for the compiler, it'll read the name of the kernel itself and if "hC" is in the name, it will use your custom compiler path. You have to modify it for your own compiler path of course.

### Other modules:
If you use other modules, they probably need a modification, too. You are on your own for them. Maybe you can use a similar way like I do for the nvidia module. Another way could be to ignore cc mismatch, but I don't recommend that.

