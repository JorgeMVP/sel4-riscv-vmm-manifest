The code is experimental and not maintained as the same degree as our other projects.

Update:

## 28/09/21
Single- and Multi-core version support (Experimental).

RISC-V compiler: https://github.com/sifive/freedom-tools/releases

```console
git clone git@github.com:kvm-riscv/qemu.git
cd qemu
mkdir build; cd build
../configure
make -j16
```

Download using repo tool
```console
repo init -u https://github.com/SEL4PROJ/sel4-riscv-vmm-manifest.git -m master.xml
repo sync
```

and build it
```console
export PATH=<PATH>/riscv64-unknown-elf-toolchain-10.2.0-2020.12.8-x86_64-linux-ubuntu14/bin:$PATH
mkdir bld
cd bld
../init-build.sh -DPLATFORM=spike -DRISCV64=1 -DCROSS_COMPILER_PREFIX=riscv64-unknown-elf-
ninja
```

run single-core version on qemu (H-extensions enabled 0.6.1)
```console
qemu-system-riscv64 -bios none -machine virt -cpu rv64,x-h=true -nographic -serial mon:stdio -m size=4095M -kernel images/sel4-riscv-vmm-image-riscv-spike
```

Configure multicore:
```console
ccmake .
# toggle `SMP` config to `ON`, and set `KernelMaxNumNodes` to greater than 1
ninja
qemu-system-riscv64 -machine virt -cpu rv64,x-h=true -smp 4 -bios none -nographic -serial mon:stdio -m size=4095M -kernel images/sel4-riscv-vmm-image-riscv-spike
```


## 19/08/21
Now the 4-core SMP VM should work.

## 07/08/21

Update the master.xml to track RISCV HE v0.6. The QEMU code can be downloaded from

<https://github.com/kvm-riscv/qemu/tree/mainline/anup%2Friscv-hyp-ext-v0.6.1>.

Execute
```console
repo init -u https://github.com/SEL4PROJ/sel4-riscv-vmm-manifest.git -m master.xml
repo sync
```
to use the master.xml.


To build and run the image with QEMU:
```console
mkdir bld
cd bld
../init-build.sh -DPLATFORM=spike -DRISCV64=1
ninja
cp ../projects/sel4_riscv_vmm/run.sh ./
./run.sh
```

The v0.3 and v0.4 supports are removed.



## 31/10/19

The QEMU RISCV HE v0.4 and v0.3 working with the VMM can be downloaded from

<https://github.com/yyshen/qemu-alistair23/tree/mainline/alistair/riscv-hyp-ext-v0.4.next>

<https://github.com/yyshen/qemu-alistair23/tree/mainline/alistair/riscv-hyp-ext-v0.3.next>

They are snapshots of the original repo <https://github.com/alistair23/qemu>, which removed the
v0.3 branch.


30/10/19

master.xml is added to track the current development.
* The kernel now supports RISCV HE v0.4.
* Experimental SMP VM support is added to the VMM.

Execute
```console
repo init -u https://github.com/SEL4PROJ/sel4-riscv-vmm-manifest.git -m master.xml
repo sync
```
to use the master.xml.

To select the RISCV HE v0.4 with SMP support
```console
mkdir bld
cd bld
../init-build.sh -DPLATFORM=spike -DRISCV64=1
ccmake .
```
In the config menu:
* set `KernelRiscvHVersion` to `4` to enable V0.4 support.
* set `SMP` to `ON` to enable SMP support.
* set `KernelMaxNumNodes` to `2`, `3`, or `4` to specify the number of cores supported.
This option is also used to specify the number of VCPUs of an SMP VM.

Having chosen the values, press key `c` then `g` to generate save the new configuration.
Then, execute the following commands to build and run the VM.

```console
ninja
cp ../projects/sel4_riscv_vmm/run_smp.sh ./
./run_smp.sh
```

The QEMU RISCV HE v0.4 is from:

[https://github.com/alistair23/qemu/tree/mainline/alistair/riscv-hyp-ext-v0.4.next]


The QEMU with RISCV hypervisor extenstion v0.3:

[https://github.com/alistair23/qemu/tree/mainline/alistair/riscv-hyp-ext-v0.3.next]

The RISCV GCC toolchain:

[https://github.com/riscv/riscv-gnu-toolchain]

You can also use the seL4 docker image [https://docs.sel4.systems/Docker.html] for the building environment.

Run the following commands after repo sync:

```console
mkdir bld
cd bld
../init-build.sh -DPLATFORM=spike -DRISCV64=1
ninja
cp ../projects/sel4_riscv_vmm/run.sh ./
./run.sh
```

The login is `root/root`.
