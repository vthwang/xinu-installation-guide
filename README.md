# Xinu Installation Guide
Welcome to the Xinu Installation Guide. This guide will walk you through the steps to build and run Xinu.
## Build Xinu
### 1. Setup Environment
```shell
sudo apt install -y git vim build-essential texinfo bison flex
cd ~
mkdir xinu
cd xinu
git clone https://github.com/xinu-os/xinu
```
### 2. Build binutils
```shell
cd ~/xinu
wget https://ftp.gnu.org/gnu/binutils/binutils-2.41.tar.gz
tar xvf binutils-2.41.tar.gz
mkdir binutils-2.41-build && cd binutils-2.41-build
../binutils-2.41/configure --prefix=/opt/mipsel-dev --target=mipsel --disable-nls
make
sudo make install
```
### 3. Link include directory
```shell
sudo mkdir -p /opt/mipsel-dev/mipsel/usr
sudo ln -s /usr/include /opt/mipsel-dev/mipsel/usr/include
```
### 4. Build GNU C Compiler
Install prerequisites for building GCC
```shell
cd ~/xinu
wget https://ftp.gnu.org/gnu/gcc/gcc-13.2.0/gcc-13.2.0.tar.gz
tar xvf gcc-13.2.0.tar.gz
cd gcc-13.2.0
./contrib/download_prerequisites
```
Build GCC
```shell
cd ~/xinu
mkdir gcc-13.2.0-build && cd gcc-13.2.0-build
../gcc-13.2.0/configure --prefix=/opt/mipsel-dev --target=mipsel --enable-languages=c,c++ --without-headers --disable-nls
make -j4 all-gcc all-target-libgcc
sudo make install-gcc install-target-libgcc
```
### 5. Setup Path
```shell
echo 'export PATH="$PATH:/opt/mipsel-dev/bin"' >> ~/.bashrc
source ~/.bashrc
echo $PATH
```
### 6. Build Xinu image
```shell
cd ~/xinu/xinu/complie
make PLATFORM=mipsel-qemu COMPILER_ROOT=/opt/mipsel-dev/bin/mipsel-
```
## Run Xinu
### 1. Install QEMU
```shell
sudo apt install -y qemu-system-mips
```
### 2. Run Xinu

```shell
qemu-system-mipsel -M mips -m 16M -kernel xinu.boot -nographic
```
Warning: If you got the error message `command not found: qemu-system-mipsel`, please try to type the command manually instead of copy and paste.
Note: If you want to exit QEMU, press `Ctrl + A` and then press `X`.