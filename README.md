# GCC-9 Installation from Git

This guide details the process for cloning a specific GCC release branch and performing an out-of-tree build.

## 1. Environment Setup
Before starting, ensure your system has the necessary build tools:
- `build-essential`
- `flex`
- `bison`
- `libgmp-dev`, `libmpfr-dev`, `libmpc-dev`

## 2. Cloning the Specific Branch
To get version 9, we clone the `releases/gcc-9` branch. Using `--depth 1` is recommended to avoid downloading the entire history of the GCC project.

```bash
git clone --branch releases/gcc-9 --single-branch --depth 1 [https://github.com/gcc-mirror/gcc.git](https://github.com/gcc-mirror/gcc.git)
```


## 3. Handling Dependencies
GCC provides a script to download and link the required math libraries (GMP, MPFR, MPC) directly into the source tree.
```bash
cd gcc
./contrib/download_prerequisites
cd ..
```

## 4. The Out-of-Tree Build
GCC requires that the build be performed in a directory separate from the source code. This prevents the compilation objects from cluttering the repository.

```bash
mkdir gcc-build
cd gcc-build
../gcc/configure \
    --prefix=$HOME/gcc-9 \
    --enable-languages=c,c++ \
    --disable-multilib
```

## 5. Compilation
Use nproc to utilize all available CPU cores. This process is resource-intensive.

```bash
make -j$(nproc)
```

## 6. Installation
Install the binaries to the path defined during the configuration step.
```bash
make install
```

## 7. Post-Install Configuration
Add the new binaries to your system path to begin using GCC-9.
```bash
export PATH=$HOME/gcc-9/bin:$PATH
export LD_LIBRARY_PATH=$HOME/gcc-9/lib64:$LD_LIBRARY_PATH
```
