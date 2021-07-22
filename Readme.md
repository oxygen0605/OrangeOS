
## 

## Build

- BootLoader Build
> \$ cd ~/edk2  
> \$ source edksetup.sh  
> \$ build

- Kernel Build
> cd ~/work/orangeos/kenerl  
> $ make

## QEMUの実行
  > $ run_qemu_mikan Build/OrangeLoaderX64/DEBUG_CLANG38/X64/Loader.efi ~/work/orangeos/kernel/kernel.elf


## USBストレージへのイメージ焼きこみ
  > $ ./write_orange_disk.sh 
  