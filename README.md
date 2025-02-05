## linux-hC

This repository is primarly for my personal use. linux-hC and linux-hC-rc uses the PKGBUILD provided by [CachyOS](https://github.com/CachyOS/linux-cachyos)

I modified it slightly for my personal use and I use patches from ZEN/Cachy/Clear/Xanmod/pf-kernel to build a kernel for my liking. I do this patches by myself, you find the repositories for these patches in my linux kernel [REPO](https://github.com/hellsgod/linux)

### If you want to build this kernel, you have to do following steps:

#### Step 1:
Set your cpu optimization in the PKGBUILD `${_processor_opt:=}` - I have set it to zen4 for myself, or set `${_use_auto_optimization:=yes}` and let it optimize for your cpu automatically.

#### Step 2:
Remove my custom compiler path under `BUILD_FLAGS:`
```
CC=/home/hellsgod/clang20/bin/clang
LD=/home/hellsgod/clang20/bin/ld.lld
AR=/home/hellsgod/clang20/bin/llvm-ar
NM=/home/hellsgod/clang20/bin/llvm-nm
STRIP=/home/hellsgod/clang20/bin/llvm-strip
OBJCOPY=/home/hellsgod/clang20/bin/llvm-objcopy
OBJDUMP=/home/hellsgod/clang20/bin/llvm-objdump
RANLIB=/home/hellsgod/clang20/bin/llvm-ranlib
READELF=/home/hellsgod/clang20/bin/llvm-readelf
AS=/home/hellsgod/clang20/bin/llvm-as
```
#### Step 3:
Install clang 20, because some of my compiler flags only work with clang 20. If you use a clang compiler in your home directory, modify Step 2 to your compiler path. If you don't want to install clang 20 and you want to use your system wide clang, which is 19.1.7 on Arch Linux, head over to Step 4.

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

### Clang 20
I use a custom built Full LTO, PGO and BOLT optimized Clang 20, which you can find here: [CLANG](https://github.com/Mandi-Sa/clang/releases)


