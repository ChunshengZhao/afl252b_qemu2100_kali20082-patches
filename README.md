# Build afl-fuzz(-Q mode) on Kali(2018.2)  

## Version:  

kali-linux-2018.2-amd64  
https://www.kali.org/  
afl-2.52b  
http://lcamtuf.coredump.cx/afl/  


## No patches will throw an error like this:  

```util/memfd.c:40:12: error: static declaration of ‘memfd_create’ follows non-static declaration
 static int memfd_create(const char *name, unsigned int flags)
            ^~~~~~~~~~~~
In file included from /usr/include/x86_64-linux-gnu/bits/mman-linux.h:115:0,
                 from /usr/include/x86_64-linux-gnu/bits/mman.h:45,
                 from /usr/include/x86_64-linux-gnu/sys/mman.h:41,
                 from /root/Downloads/afl-2.52b/qemu_mode/qemu-2.10.0/include/sysemu/os-posix.h:29,
                 from /root/Downloads/afl-2.52b/qemu_mode/qemu-2.10.0/include/qemu/osdep.h:104,
                 from util/memfd.c:28:
/usr/include/x86_64-linux-gnu/bits/mman-shared.h:46:5: note: previous declaration of ‘memfd_create’ was here
 int memfd_create (const char *__name, unsigned int __flags) __THROW;
     ^~~~~~~~~~~~
/root/Downloads/afl-2.52b/qemu_mode/qemu-2.10.0/rules.mak:66: recipe for target 'util/memfd.o' failed
make: *** [util/memfd.o] Error 1
```
![Error](https://raw.githubusercontent.com/ChunshengZhao/afl252b_qemu2100_kali20082-patches/master/pic/kali-2018-05-31-23-33-38.jpg)

## Build Tips:  

root@kali:\~/Downloads# tar xzf afl-latest.tgz  
root@kali:\~/Downloads# cd afl-2.52b/  
root@kali:\~/Downloads/afl-2.52b# make && make install  
  
root@kali:\~/Downloads/afl-2.52b# cd qemu_mode/  
root@kali:\~/Downloads/afl-2.52b/qemu_mode# patch -p0 < build_qemu_support.sh.diff  
root@kali:\~/Downloads/afl-2.52b/qemu_mode# ./build_qemu_support.sh  
![OK](https://raw.githubusercontent.com/ChunshengZhao/afl252b_qemu2100_kali20082-patches/master/pic/kali-2018-05-31-22-37-53.jpg)

## Where is Patches?  

Copy "configure.diff" and "memfd.c.diff" to "afl-2.52b/qemu_mode/patches"  
Copy "build_qemu_support.sh.diff" to "afl-2.52b/qemu_mode"  


## Test:  

Set the "afl-qemu-trace" variable:  
root@kali:\~/Downloads/afl-2.52b/testcases/archives/common/rar# export AFL_PATH=/root/Downloads/afl-2.52b  

root@kali:\~/Downloads/afl-2.52b/testcases/archives/common/rar# mkdir out  
root@kali:\~/Downloads/afl-2.52b/testcases/archives/common/rar# afl-fuzz -i ./ -o ./out/ -Q rar t
![GO](https://raw.githubusercontent.com/ChunshengZhao/afl252b_qemu2100_kali20082-patches/master/pic/kali-2018-05-31-22-40-38.jpg)

## Thanks:  

Paolo Bonzini <pbonzini@redhat.com>  
>https://git.qemu.org/?p=qemu.git;a=commitdiff;h=75e5b70e6b5dcc4f2219992d7cffa462aa406af0
