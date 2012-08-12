# Kotleta

A research project started by two drunk monkeys.

## Step 1. Get source code

* Linux 3.5.1
* Busybox 1.20.2

## Step 2. Build toolchain

You will need the package named `arm-linux-gnueabi-gcc` on Arch Linux.

## Step 3. Build the kernel

```bash
export ARCH=arm
export TARGET=arm-linux-gnueabi
export CROSS_COMPILE=arm-linux-gnueabi-

make versatile_defconfig
make menuconfig # or 'make gconfig'
make zImage modules
```

Kernel will be placed in `arch/arm/boot/zImage`.

**OH SHIT, IT DOESN'T WORK**