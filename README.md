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

make versatile_defconfig # create default .config file
make menuconfig # or 'make gconfig'
make zImage modules
```

Kernel will be placed in `arch/arm/boot/zImage`.

**OH SHIT, IT DOESN'T SEE TEH FILESYSTEM**

## Step 4. Build the Busybox

```bash
export ARCH=arm
export TARGET=arm-linux-gnueabi
export CROSS_COMPILE=arm-linux-gnueabi-

make defconfig
make menuconfig # or 'make gconfig'
make
make install
```

Files will be placed in the `_install` folder.

## Step 5. Build the disk image

```bash
qemu-img create -f raw kotleta.img 1G
losetup -f kotleta.img # from root
fdisk /dev/loop0 # from root
# now create one partition filling the whole disk, write it and exit fdisk
kpartx -va /dev/loop0 # from root
mount /dev/mapper/loop0p1 /mnt # from root
cp -r path/to/busybox/_install/* /mnt
sync
umount /mnt # from root
kpartx -vd /dev/loop0 # from root
losetup -d /dev/loop0 # from root
```

## Step 6. Run

```bash
qemu-system-arm -monitor stdio -M versatilepb -kernel path/to/zImage -drive file=path/to/kotleta.img # -append 'root=/dev/something'
```