This folder contains files to run JerryScript in LinkIt RTOS SDK, support List:

* MT7687/2523/7697

### Cross-compiler
For cross-compilation the GCC 4.8.4(2014Q3) is suggested to be used. [Download Link](https://launchpad.net/gcc-arm-embedded/4.8/4.8-2014-q3-update)

##### Machine Env:

```
ubuntu
```

##### How to change ubuntu GCC to this version:

* remove your origin gcc

```
sudo apt-get remove gcc-arm-none-eabi

```

* Download GCC, and unzip it:

```
tar -xjvf ./gcc-arm-none-eabi-4_8-2014q3-20140805-linux.tar.bz2
```

```
sudo mv gcc-arm-none-eabi-4_8-2014q3 /opt
exportline="export PATH=/opt/gcc-arm-none-eabi-4_8-2014q3/bin:$PATH"
```

* If there have other GCC in /usr/bin, Please remove it:
```
sudo rm -r /usr/bin/arm-none-eabi-gcc
sudo rm -r /usr/bin/arm-none-eabi-g++
```

* copy gcc to /usr/bin
```
sudo ln -s /opt/gcc-arm-none-eabi-4_8-2014q3/bin/arm-none-eabi-g++ /usr/bin
sudo ln -s /opt/gcc-arm-none-eabi-4_8-2014q3/bin/arm-none-eabi-gcc /usr/bin

```

* If your machine is 64bit , you should install this one:
```
sudo apt-get install lsb-core
```

* check your gcc:

```
arm-none-eabi-gcc -v
```

* It will print it:

```
Using built-in specs.
COLLECT_GCC=arm-none-eabi-gcc
COLLECT_LTO_WRAPPER=/home/ubuntu/gcc-arm-none-eabi-4_8-2014q3/bin/../lib/gcc/arm-none-eabi/4.8.4/lto-wrapper
Target: arm-none-eabi
Configured with: /home/build/work/GCC-4-8-build/src/gcc/configure --target=arm-none-eabi --prefix=/home/build/work/GCC-4-8-build/install-native --libexecdir=/home/build/work/GCC-4-8-build/install-native/lib --infodir=/home/build/work/GCC-4-8-build/install-native/share/doc/gcc-arm-none-eabi/info --mandir=/home/build/work/GCC-4-8-build/install-native/share/doc/gcc-arm-none-eabi/man --htmldir=/home/build/work/GCC-4-8-build/install-native/share/doc/gcc-arm-none-eabi/html --pdfdir=/home/build/work/GCC-4-8-build/install-native/share/doc/gcc-arm-none-eabi/pdf --enable-languages=c,c++ --enable-plugins --disable-decimal-float --disable-libffi --disable-libgomp --disable-libmudflap --disable-libquadmath --disable-libssp --disable-libstdcxx-pch --disable-nls --disable-shared --disable-threads --disable-tls --with-gnu-as --with-gnu-ld --with-newlib --with-headers=yes --with-python-dir=share/gcc-arm-none-eabi --with-sysroot=/home/build/work/GCC-4-8-build/install-native/arm-none-eabi --build=i686-linux-gnu --host=i686-linux-gnu --with-gmp=/home/build/work/GCC-4-8-build/build-native/host-libs/usr --with-mpfr=/home/build/work/GCC-4-8-build/build-native/host-libs/usr --with-mpc=/home/build/work/GCC-4-8-build/build-native/host-libs/usr --with-isl=/home/build/work/GCC-4-8-build/build-native/host-libs/usr --with-cloog=/home/build/work/GCC-4-8-build/build-native/host-libs/usr --with-libelf=/home/build/work/GCC-4-8-build/build-native/host-libs/usr --with-host-libstdcxx='-static-libgcc -Wl,-Bstatic,-lstdc++,-Bdynamic -lm' --with-pkgversion='GNU Tools for ARM Embedded Processors' --with-multilib-list=armv6-m,armv7-m,armv7e-m,armv7-r
Thread model: single
gcc version 4.8.4 20140725 (release) [ARM/embedded-4_8-branch revision 213147] (GNU Tools for ARM Embedded Processors) 
```

### How to build a target
Navigate to your JerryScript root folder (after you cloned this repository into the targets folder) and use the following command:

```
make -f targets/RTOS/Makefile.mbed board=stm32f4 jerry
```

1. The next rule is the `jerry` rule. This rule builds the JerryScript and copy the output files into the target libjerry folder. Two files will be generated at `targets/mbed/libjerry`:
  * libjerrycore.a
  * libfdlibm.a

2. The next rule is the `js2c`. This rule calls a `js2c.py` python script from the `jerryscript/targets/tools` and creates the JavaScript builtin file into the `targets/mbed/source/` folder. This file is the `jerry_targetjs.h`. You can run this rule with the follwoing command:

  - `make -f targets/RTOS/Makefile.mbed board=stm32f4 js2c`

3. Optional rule: `clean`. It removes the build folder from the mbed and jerry. You can run this rule with this command:

  - `make -f targets/RTOS/Makefile.mbed board=stm32f4 clean`

4. copy ./jerry-core into LinkIt RTOS SDK./middleware
###Note
If you use an STM32F4 board your build will stop with missing header errors. To fix this error please visit to [this page](http://browser.sed.hu/blog/20160407/how-run-javascripts-jerryscript-mbed) and read about the fix in the `New target for STM32F4` block.
