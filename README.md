This repository uses the reference from https://github.com/riscv-boom/boom-attacks

This have been test with BOOM in ZC706 (Baremetal) && VC706(Run on Linux)

Create elf file for Baremetal: Use Makefile.baremetal
Create elf file for Linux with Newlib toolchain: Use Makefile.Newlib
Create elf file for Linux with Glibc toolchain: Use Makefile.GLibc
File content:
inc: header library
link: linker
src:
- spectrev1: Bound Check Bypass
- spectrev2: Branch Target Injection
- spectre-fence: Mitigation using fence
- spectre-SLH: Mitigation using index masking
