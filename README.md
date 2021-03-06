# RaptorQ #


[![build status](https://www.fenrirproject.org/Luker/libRaptorQ/badges/master/build.svg)](https://www.fenrirproject.org/Luker/libRaptorQ/commits/master)  
[Homepage](https://www.fenrirproject.org/Luker/libRaptorQ/wikis/home)

stable  release: **v0.1.9**  
current release: **v1.0.0-prealpha**  
**CURRENTLY BREAKING APIs, stick with the stable release**  
UPDATE: **almost finished the brakage, things should be stable enough**

RaptorQ is a **Forward Error Correction** algorithm designed to deliver your data
efficiently and without retransmissions for lost packets.

After sending K packets of your data as-is, RaptorQ generates as many repair
symbols as you need. Once the receiver has at least K symbols, be it the source
symbols, repair symbols or any combination of the two, it can reconstruct the
whole input it was meant to receive.

This is called a Fountain code, and RaptorQ is the latest and most efficient
code in this category.


**libRaptorQ** implements **RFC6330**, which specifies the RaptorQ algorithm.  

libRaptorQ is entirely written in *C++11*, and uses eigen to handle matrix
manipulation.


Currently it's only been tested under Linux, but should work well under
*BSD and MacOSX, too.

Windows _should_ work too, provided you use a complier that understands C++11  
(hint: you might have some problems with VisualC++, but that's not a C++
compiler anyway)


## Developers ##

See the [CONTRIBUTING](CONTRIBUTING.md) file.

## The Source Code ##

Although things seems to work, no stable release has been released yet.

This means you can only check this out with git.

to check out the repository:

``$ git clone https://github.com/LucaFulchir/libRaptorQ.git``

you can also get it from our main server:

``$ git clone https://www.fenrirproject.org/Luker/libRaptorQ.git``


### GPG source check ###

Once you have cloned it, it's always a good thing to check the repository gpg
signatures, so you can import my key with:

2016 key:  
``$ gpg --keyserver pgp.mit.edu --recv-key F61F6137``  
2015 key:  
``$ gpg --keyserver pgp.mit.edu --recv-key D42DDF0A``  

please check the full fingerprint, it should be like this:

```bash
 $ gpg2 --fingerprint F61F6137
 pub   rsa2048/F61F6137 2016-01-11 [expires: 2017-01-25]
       Key fingerprint = 95EB 6FA4 03D7 29A7 13C2  FC9F F85A DA1C F61F 6137
 uid         [ultimate] Luca Fulchir (2016 key) <luker@fenrirproject.org>
 full 2015 fingerprint:
 Key fingerprint = AB35 E45F 5CA5 E35B 8B55  818F 0157 D133 D42D DF0A
```

Now you have the source, and the key, it's enough to check the signature of the
last commit:

``$ git log -n 1 --show-signature``

The important part is that you get something like this:

```bash
 gpg: Signature made Mon 11 Jan 2016 14:19:21 CET using RSA key ID F61F6137
 gpg: Good signature from "Luca Fulchir (2016 key) <luker@fenrirproject.org>"
 [unknown]
 gpg: WARNING: This key is not certified with a trusted signature!
 gpg:          There is no indication that the signature belongs to the owner.
 Primary key fingerprint: 95EB 6FA4 03D7 29A7 13C2  FC9F F85A DA1C F61F 6137
 Author: Luca Fulchir <luker@fenrirproject.org>
```

And as long as you got the right key, and you find the "gpg: Goog signature",
you can be sure you have the right code.

TDB: The repository is using the maintainer private key for now,
it will start using a dedicated key in the (near) future.


## Install ##

#### Dependencies ####

libRaptorQ depends from **Eigen3** and **LZ4**  
In case your system does not have Eigen3, Eigenv3.2.8 is included
in the source files, so you do not need it.  
LZ4 is included as a git submodule, so if you do not have it, run:  
```git submodule init```  
```git submodule update```  
To get the library, which will be statically linked and **NOT INSTALLED**
on your system.

#### Building ####

The build system uses CMake. So enter the source directory and we'll create a
directory and build everything there:

```bash
$ mkdir build
$ cd build

$ cmake -DCMAKE_BUILD_TYPE=Release ../

$ make -j 4
```

Optional targets are available:  
``$ make tests examples docs``  
where:  
* tests: benchmarks, rfc tests.
* examples: C/C++ examples
* docs: LATEX documentation.

or simply:  
``$ make -j 4 everything``

finally, install everything  
``$ make install``

libRaptorQ uses **deterministic (reproducible) builds**,
so if you compile it twice, or on two different computers
(but with the same compiler), the hash of the resulting
libraries will be the same.

_There are combinations of compiler and LTO/Profiling that
break deterministic builds, so check the cmake warnings._

You can customize the CMake build with the following variables:

```
PROFILING   ON/OFF: Default:ON. Activate or deactivate profiling.
                Profiling compiles everything, then runs a test to see
                which code paths are more used. Then it recompiles everything
                but optimizing for for those code paths.
                Only for gcc/clang.
LTO         ON/OFF: Default:ON. Activate or deactivate Link time Optimization
                Makes the library smaller and better optimized.
                Only for gcc/clang.
CLANG_STDLIB ON/OFF: Default:OFF. use clang's libc++
                Note: only clang can use its standard library
USE_LZ4     ON/OFF: use the lz4 compression for caching precomputations.
                Default: ON
CLI         ON/OFF Build Command Line Interface tools.
CMAKE_C_COMPILER    gcc, clang...
CMAKE_CXX_COMPILER  choose between g++ or clang++.
RQ_LINKER   gold/ld/bsd Choose your linker. Default:autodetect.
CMAKE_BUILD_TYPE    Debug,MinSizeRel,Release,RelWithDebInfo
CMAKE_INSTALL_PREFIX Default: /usr/local
```

## Using libRaptorQ ##

C and C++11 interfaces are available.

You can compile a PDF of the documentation by doing:  
``$ make docs``

Or you can simply visit the [wiki](https://www.fenrirproject.org/Luker/libRaptorQ/wikis/libRaptorQ.pdf)
for the full up-to-date documentation.



