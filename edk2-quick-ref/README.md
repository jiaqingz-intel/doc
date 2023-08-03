# Unofficial EDK2 Quick Reference

A quick reference for building edk2 on linux

## Setup environment

### Install dependencies
 
```bash
sudo apt install build-essential git uuid-dev nasm acpica-tools python
```

### Clone git repo

```bash
git clone https://github.com/tianocore/edk2.git
cd ./edk2
git submodule update --init
```

You may optionally checkout the latest stable version, prefix is `edk2-stable`.

### Build toolchain

```bash
. edksetup.sh
make -C ./BaseTools -j $(nproc)
```


## Build component

Before running commands below, make sure you have already run `. edksetup.sh`.

### Build UEFI shell

```bash
build -a X64 -t GCC5 -b RELEASE -p OvmfPkg/OvmfPkgX64.dsc -m ShellPkg/Application/Shell/Shell.inf
```

The shell binary is at `Build/OvmfX64/RELEASE_GCC5/X64/Shell.efi`.

To enter the UEFI shell, get a FAT32-formatted USB drive, put the binary as `EFI/BOOT/BOOTX64.EFI` on the USB drive, and then boot that USB drive.


### Buid DXE driver

Take PciSioSerialDxe as example.

```bash
build -a X64 -t GCC5 -b RELEASE -p OvmfPkg/OvmfPkgX64.dsc -m MdeModulePkg/Bus/Pci/PciSioSerialDxe/PciSioSerialDxe.inf
```

Driver .efi file is at `Build/OvmfX64/RELEASE_GCC5/X64/PciSioSerialDxe.efi`, this DXE driver can be loaded by `load` command in UEFI shell.

FFS file is at `Build/OvmfX64/RELEASE_GCC5/FV/Ffs/E2775B47-D453-4EE3-ADA7-391A1B05AC17PciSioSerialDxe/E2775B47-D453-4EE3-ADA7-391A1B05AC17.ffs`. The filename is the module's GUID.

This file is used when modding the firmware with [UEFITool](https://github.com/LongSoft/UEFITool) or AMI MMTool.
