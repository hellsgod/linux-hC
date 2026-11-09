## linux-hC

This repository is primarly for my personal use.

`linux-hC` and `linux-hC-rc` uses the PKGBUILD provided by [CachyOS](https://github.com/CachyOS/linux-cachyos)

I modified it slightly for my personal use.

I use patches from ZEN/Cachy/Clear/Xanmod/pf-kernel to build a kernel for my liking.

You find the repositories for these patches in my linux kernel [REPO](https://github.com/hellsgod/linux)

### INFORMATION
- I don't support LTS. I don't support kernel versions until EOL.
- After each cycle (RC gets stable) I will drop support for the previous stable.
- Most of the time I'm on the RC variant and most development will be done there.
- I don't add features I don't need.
- It's not my intention to make a kernel for everyone.

### linux-hC and linux-hC-rc
- `linux-hC` is for stable releases.
- `linux-hC-rc` is for RC releases - bleeding edge.

### Features:
- Default built with BORE (Burst-Oriented Response Enhancer) by Masahito Suzuki: [Repository](https://github.com/firelzrd/bore-scheduler)
- Default built with Clang 24, which you can find here: [Repository](https://github.com/hellsgod/clang)
- Default built with ThinLTO and enabled rust support.
- Default built with a custom set of compiler optimization flags for my 9950X.
- Memory management tweaks from zen-kernel (compaction, watermark).
- Cherry-picked fixes and tweaks from Clear-linux, ZEN, CachyOS and pf-kernel.
- poc-selector for faster idle CPU discovery : [Repository](https://github.com/firelzrd/poc-selector)
- cambyses - A tuned CFS load balancer: [Repository](https://github.com/firelzrd/cambyses)
- cpuidle - TEO default instead of menu.
- cpuidle - NAP built in, but has to be set in cmdline: [Repository](https://github.com/firelzrd/nap)
- Cost-Aware Self-Tuning LRFU (lru_cast): [Repository](https://github.com/firelzrd/lru_cast)
- Various fixes and patches from upstream.

### If you want to build this kernel, you have to do following steps:

#### Step 1:
- Set your cpu optimization in the PKGBUILD `${_processor_opt:=}`
- I have set it to zen4 for myself. 
- Set it to native to optimize it for your cpu.
- Or generic to use the kernel on different devices.
#### NOTE:
- I dont use graysky2's kernel_compiler_patch [Repository](https://github.com/graysky2/kernel_compiler_patch)
- So the only options you can set are: zen4, native, or generic.

#### Step 2:
- Set `${_custom_llvm=yes}` to `no` or `false` if you don't use an external llvm/clang.
- If you use an external clang compiler:
- Set `${_custom_llvm_path=/home/hellsgod/clang24/bin}`
- And `${_custom_libclang_path=/home/hellsgod/clang24/lib}`
- To your own path.

#### Step 3:
#### CAUTION: DO NOT USE MY compiler-opt.patch if you don't use a zen4/5 CPU!!
- Run `makepkg -si` to install dependencies and build the kernel.
- You can also modify the PKGBUILD to change various options to your liking.

### Nvidia module:
- `linux-hC` and `linux-hC-rc` will compile the `nvidia-open-module`. 
- You can disable its compilation by setting `${_build_nvidia_open:=yes}` to `no`.
