# VanitySearch

VanitySearch is a bitcoin address prefix finder. It uses fixed size arithmethic in order to get best performances. 
Secure hash algorithms (SHA256 and RIPEMD160) are performed using SSE on the CPU. The GPU kernel has been written using
CUDA in order to take advantage of inline PTX assembly. VanitySearch may not compute a good grid size for your hardware, so try different values using -g option. If you want to use GPU and CPU together, you may have best performance by keeping one CPU core for handling GPU/CPU exchanges (use -t option to set the number of CPU threads).

# Usage

You can downlad latest release from https://github.com/JeanLucPons/VanitySearch/releases

VanitySeacrh [-check] [-v] [-u] [-gpu] [-stop] [-o outputfile] [-gpuId gpuId] [-g gridSize] [-s seed] [-t threadNumber] prefix
  prefix: prefix to search\
  -v: Print version\
  -check: Check GPU kernel vs CPU\
  -u: Search uncompressed addresses\
  -o outputfile: Output results to the specified file\
  -gpu: Enable gpu calculation\
  -gpu gpuId: Use gpu gpuId, default is 0\
  -g gridSize: Specify GPU kernel gridsize, default is 16*(MP number)\
  -s seed: Specify a seed for the base key, default is random\
  -t threadNumber: Specify number of CPU thread, default is number of core\
  -stop: Stop when prefix is found
  
  Exemple (Windows, Intel Core i7-4770 3.4GHz 8 multithreaded cores, GeForce GTX 645):
  ```
  C:\C++\VanitySearch\x64\Release>VanitySearch.exe -stop -gpu 1tryme
  Start Mon Feb 25 08:10:36 2019
  Search: 1tryme
  Difficulty: 15318045009
  Base Key:45D090A251EB279D63E3632DA1FDB01A0052A3209ED841A17CD4F2E7C9583D0A
  Number of CPU thread: 7
  GPU: GPU #0 GeForce GTX 645 (3x192 cores) Grid(48x64)
  31.385 MK/s (GPU 24.378 MK/s) (2^35.04) [P 90.00%][99.00% in 00:18:43]
  Pub Addr: 1trymeffDV5eAJMANdFs3vVAq6oEjdzK6
  Prv Addr: 5JM2uQyqRAzc1Jusep4gfKFRurRA49EEKpzByi5ESpp4KT9PBVZ
  Prv Key : 0x45D090A251EB279D63E3632DA1FDB01A0052A3209ED841A77CD4F2E80C2F92D4
  Check   : 16rXQ3Rvzb1oxdTn1JSPbYXBniPU1uzHBe
  Check   : 1trymeffDV5eAJMANdFs3vVAq6oEjdzK6 (comp)
  ```

# Compilation

## Windows

Intall CUDA SDK and open VanitySearch.sln in Visual C++ 2017.\
You may need to reset your *Windows SDK version* in project properties.\
In Build->Configuration Manager, select the *Release* configuration.\
Build and enjoy.\
\
Note: The current relase has been compiled with CUDA SDK 10.0, if you have a different release of the CUDA SDK, you may need to update CUDA SDK paths in VanitySearch.vcxproj using a text editor.

## Linux

Intall CUDA SDK.\
Depenging on the CUDA SDK version and on your Linux distribution you may need to install an older gcc (just for the CUDA SDK).\
Add a link to the good gcc in /usr/local/cuda, nvcc will use this path first.

```
lrwxrwxrwx 1 root root      16 mars   1 10:54 /usr/local/cuda/bin/g++ -> /usr/bin/g++-4.8*
lrwxrwxrwx 1 root root      16 mars   1 10:53 /usr/local/cuda/bin/gcc -> /usr/bin/gcc-4.8*
```

Edit the makefile and set up the good compute capabilites for your hardware and CUDA SDK path. You can enter a list of architectrure (refer to nvcc documentation). Here it is set up for compute capability 2.0 (Fermi) which is deprecated for recent CUDA SDK.
```
-gencode=arch=compute_20,code=sm_20
```

VanitySearch need to be compiled and linked with a recent gcc. The current release has been compiled with gcc 7.3.0.\
Go to the VanitySearch directory.

```
$ g++ -v
gcc version 7.3.0 (Ubuntu 7.3.0-27ubuntu1~18.04)
$ make all (for build without CUDA support)
or
$ make gpu=1 all
```
Runnig VanitySearch.
```
$export LD_LIBRARY_PATH=/usr/local/cuda-8.0/lib64
$./VanitySearch -stop -gpu 1Happy
Start Sat Mar  2 14:50:56 2019
Search: 1Happy
Difficulty: 264104224
Base Key:E5157A2C7E69B82E807A63582B694CA6537687F55FD0240D80FA1309D8DC4BDA
Number of CPU thread: 1
GPU: GPU #0 Quadro 600 (2x48 cores) Grid(32x64)
5.406 MK/s (GPU 4.718 MK/s) (2^25.11) [P 12.85%][50.00% in 00:00:27]
Pub Addr: 1HappyEUfSip2dS1wi7SLiHnX7daXsYahf
Prv Addr: 5KZBDqaxrXYLCBQctrBegLzY2tkTKkDtvDgPmhknC8xpXSjj4oe
Prv Key : 0xE5157A2C7E69B82E807A63582B694CA653768B005FD0240D80FA1309D8DC9939
Check   : 1Fz3rj7Pf3nTbtrLRjPcCV8pnBA7jwf4Y2
Check   : 1HappyEUfSip2dS1wi7SLiHnX7daXsYahf (comp)
```

# License

VanitySearch is licensed under GPLv3.

