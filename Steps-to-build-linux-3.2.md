0. All Together:

sudo apt-get install gcc-arm-linux-gnueabi
wget http://www.kernel.org/pub/linux/kernel/v3.0/linux-3.2.tar.bz2
tar xjf linux-3.2.tar.bz2
export ARCH=arm
export CROSS_COMPILE=arm-linux-gnueabi-
cd linux-3.2
make vexpress_defconfig
make all

Note: If you get error during "make all" follow from step-5.

-- Description on each command:

1. Get Cross compiling Toolchain on Ubuntu-16.04:

$ sudo apt-get install gcc-arm-linux-gnueabi

2. Get linux-3.2 source code, untar:

$ wget http://www.kernel.org/pub/linux/kernel/v3.0/linux-3.2.tar.bz2
$ tar xjf linux-3.2.tar.bz2

3. Setup variables to run make:

$ export ARCH=arm
$ export CROSS_COMPILE=arm-linux-gnueabi-
$ cd linux-3.2

4. Configure to build for ARM versatile express and run make:

$ make vexpress_defconfig
$ make all

5. During "make all" you may get an error:
 
 fatal error: linux/compiler-gcc5.h: No such file or directory
 This is due to missing header linux/compiler-gcc5.h

 Get this file at:  https://github.com/torvalds/linux/blob/v4.0/include/linux/compiler-gcc5.h
 Add file manually to the downloaded Linux-3.2 source code in the folder 
 >> include/linux/
 
 Run make all 
 
 6. If you get error "error: redefinition of ‘return_address’void *return_address(unsigned int level)"
 
 Make following changes to Linux-3.2 source:
 https://github.com/torvalds/linux/commit/aeea3592a13bf12861943e44fc48f1f270941f8d

 In file: arch/arm/include/asm/ftrace.h 

 change this line : 
 - extern inline void *return_address(unsigned int level)
 
  To this: 
 + static inline void *return_address(unsigned int level)
   {
  	return NULL;
   }


 In file:  arch/arm/kernel/return_address.c 

  Comment out these lines:
 - void *return_address(unsigned int level)
 - {
 -	 return NULL;
 - }


Now Make all will run :)
