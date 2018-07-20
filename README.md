# Android build environment for OpenSSL

* Supports build for multiple architectures - ARM, ARMv7, X86
* Uses OpenSSL source codes
* Integrated with Android.mk build
* Output directories for libcrypto and libssl are set in Android.mk
  * arch-armeabi/lib/
  * arch-armeabi-v7a/lib/
  * arch-x86/lib/

**Advice: Do NOT use OpenSSL binaries you find laying about the Internet.**


## How to compile

```bash
cd jni/openssl
./build.sh
```

Optionally, set variables on the beginning of the `build.sh` according to your Android NDK.

In the global JNI Android.mk you can then simply include Android.mk from openssl directory so the
static or dynamic libraries are linked to the rest of your project.

```
include jni/openssl/Android.mk
```

Include paths for header files are not set, headers will be after compilation present at

```
jni/openssl/sources/include
```

## Notes

Advice: Do not use pre-built OpenSSL libs you find laying around the Internet

### macOS Using Android NDK r10e

On macOS, building OpenSSL fails when using NDK 11 or greater.

Work-around is to download and use r10e to build OpenSSL.

1) Please download Android NDK r10e and update ANDROID_NDK
  * https://developer.android.com/ndk/downloads/older_releases
  * https://dl.google.com/android/repository/android-ndk-r10e-windows-x86_64.zip
2) Set NDK to point to r10e  

### Ubuntu 18.04

I could never get OpenSSL to build with this script even after reverting to NDK r10e?

Ubuntu build error "i686-linux-android-gcc: Command not found"
````
i686-linux-android-gcc -I. -I.. -I../include  -fPIC -DOPENSSL_PIC -DOPENSSL_THREADS -D_REENTRANT -DDSO_DLFCN -DHAVE_DLFCN_H -mandroid -I/home/edward3/Android/Sdk/ndk-r10e/platforms/android-14/arch-x86/usr/include -B/home/edward3/Android/Sdk/ndk-r10e/platforms/android-14/arch-x86/usr/lib -O3 -fomit-frame-pointer -Wall -DOPENSSL_BN_ASM_PART_WORDS -DOPENSSL_IA32_SSE2 -DOPENSSL_BN_ASM_MONT -DOPENSSL_BN_ASM_GF2m -DRC4_ASM -DSHA1_ASM -DSHA256_ASM -DSHA512_ASM -DMD5_ASM -DRMD160_ASM -DAES_ASM -DVPAES_ASM -DWHIRLPOOL_ASM -DGHASH_ASM   -c -o cryptlib.o cryptlib.c
make[1]: i686-linux-android-gcc: Command not found
<builtin>: recipe for target 'cryptlib.o' failed
````
