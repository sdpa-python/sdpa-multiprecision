# SDPA-GMP

This repository aims to provide a maintained version of the SDPA-GMP solver. SDPA-GMP utilizes [GMP](https://gmplib.org) (the GNU Multiple Precision  Arithmetic Library) to provide a highly accurate SDP solver.

The buildsystem has been modified from the official SDPA-GMP to also build a `libsdpa_gmp.a` static library, besides the `sdpa_gmp` executable.

## Building on macOS and Linux

The installation process involves building the GMP library followed by building SDPA-GMP and linking it against `libgmp.a`.

Note: even if you have `libgmp` installed on your system, it's recommended to download and build GMP from the official website to ensure you have the latest version.

### Building GMP

Download, extract and `cd` into GMP source:

```bash
wget https://gmplib.org/download/gmp/gmp-6.2.1.tar.bz2
tar xf gmp-6.2.1.tar.bz2
cd gmp-6.2.1
```

Run the configure script. Assuming you had GMP extracted on the Desktop:

```bash
autoreconf -i
./configure --prefix=$HOME/Desktop/gmp-6.2.1 --enable-cxx
```

Build GMP. It's important to run `make check` as well as `make install`. Without `make install`, libraries and headers will not be generated.

```bash
make
make check
make install
```

### Building SDPA-GMP

Clone this repository

```bash
git clone https://github.com/sdpa-python/sdpa-gmp.git
cd sdpa-gmp
```

Run the configure script.

```bash
./configure --with-gmp-includedir=$HOME/Desktop/gmp-6.2.1/include --with-gmp-libdir=$HOME/Desktop/gmp-6.2.1/lib
```

Build the `sdpa_gmp` binary as well as `libsdpa_gmp.a` static library.

```bash
make
```

If you want to use SDPA-GMP through the SDPA for Python package, SDPA for Python can now link against `libsdpa_gmp.a`.

## Building on Windows

Owing to the `autotools` based buildsystem of SDPA-GMP, a POSIX like environment is required to build on Windows. MSYS2 is recommended. Instructions for macOS and Linux in the previous section can be used.
