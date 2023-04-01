# SDPA Multiprecision

SDPA Multiprecision is a fork of SDPA-GMP [1]. SDPA-GMP is a part of the SDPA [2] [3] [4] family and utilizes the [GNU Multiple Precision](https://gmplib.org) library to provide a highly accurate SDP solver.

Since SDPA-GMP is not officially maintained, this fork aims to provide the additional functionality available in the main SDPA package, the most important of which is the ability to use the solver as a callable library in other programs. To this end, the buildsystem has been modified to also generate a `libsdpa_gmp.a` static library, besides the `sdpa_gmp` executable.

## Building from source

The installation process involves building the GMP library followed by building SDPA-GMP, linking it against `libgmp.a` and `libgmpxx.a`.

Even if you have `libgmp` installed on your system, it's recommended to download and build GMP from the official website to ensure you have the latest version.

On Windows, owing to the `autotools` based buildsystem of SDPA-GMP, a POSIX like environment is required to build from source. [MSYS2](https://www.msys2.org) is recommended.

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

### Building SDPA Multiprecision

Clone this repository

```bash
git clone https://github.com/sdpa-python/sdpa-multiprecision.git
cd sdpa-gmp
```

#### Building on Linux and macOS

You will need to provide the location of GMP libraries and headers to the configure script

```bash
autoreconf -i
./configure --with-gmp-includedir=$HOME/Desktop/gmp-6.2.1/include --with-gmp-libdir=$HOME/Desktop/gmp-6.2.1/lib
make
```

By default, `autotools` builds with debugging symbols. If you would like to exclude debugging symbols, please add `CFLAGS="-DNDEBUG" CXXFLAGS="-DNDEBUG"` to the `configure` command.

#### Building on Windows

If you are using MSYS2, `make install` will have installed the GMP libraries and headers alongside other MinGW libraries and headers and they will be autodetected.

SDPA Multiprecision also requires the SPOOLES library. SPOOLES is also contained in this repository, and the buildsystem is configured to automatically build it, however, you may encounter problems building SPOOLES yourself on MSYS2. As a workaround, you can install it from the [MSYS2 repo](https://packages.msys2.org/base/mingw-w64-spooles) instead. While the library will be autodetected, you will have to manually specify the location of the SPOOLES headers.

Assuming you had MSYS2 installed in the default location:

```bash
autoreconf -i
./configure --with-system-spooles --with-spooles-includedir=/c/msys64/mingw64/include/spooles
make
```

This will generate the `sdpa_gmp` executable as well as the `libsdpa_gmp.a` static library. If you want to use SDPA Multiprecision through the [SDPA for Python](https://github.com/sdpa-python/sdpa-python) package, SDPA for Python can now link against `libsdpa_gmp.a`.

You may use the executable to verify the build was successful. To this end, while still in the build directory, run

```bash
./sdpa_gmp example1.dat example1.out
```

If you see `pdOPT` as the status, build was successful. On Windows, you will have to manually ensure that `example1.dat` line endings are `CRLF` instead of `LF`. You can do this conversion using most code editors.

## References

[1] Nakata, M. (2010). A numerical evaluation of highly accurate multiple-precision arithmetic version of semidefinite programming solver: SDPA-GMP, -QD and -DD. 2010 *IEEE International Symposium on Computer-Aided Control System Design*, 29–34. doi: [10.1109/CACSD.2010.5612693](https://doi.org/10.1109/CACSD.2010.5612693)

[2] Makoto Yamashita, Katsuki Fujisawa and Masakazu Kojima, "Implementation and evaluation of SDPA 6.0 (Semidefinite Programming Algorithm 6.0)," *Optimization Methods and Software, vol. 18, no. 4, pp. 491–505*, 2003, doi: [10.1080/1055678031000118482](https://doi.org/10.1080/1055678031000118482).

[3] Makoto Yamashita, Katsuki Fujisawa, Kazuhide Nakata, Maho Nakata, Mituhiro Fukuda, Kazuhiro Kobayashi, and Kazushige Goto, "A high-performance software package for semidefinite programs: SDPA 7," *[Research Report B-460](http://www.optimization-online.org/DB_HTML/2010/01/2531.html) Dept. of Mathematical and Computing Science, Tokyo Institute of Technology, Tokyo, Japan, September*, 2010.

[4] Makoto Yamashita, Katsuki Fujisawa, Mituhiro Fukuda, Kazuhiro Kobayashi, Kazuhide Nakata and Maho Nakata, “Latest Developments in the SDPA Family for Solving Large-Scale SDPs,” in *Handbook on Semidefinite, Conic and Polynomial Optimization, M. F. Anjos and J. B. Lasserre, Eds. Boston, MA: Springer US*, 2012, pp. 687–713. doi: [10.1007/978-1-4614-0769-0_24](https://doi.org/10.1007/978-1-4614-0769-0_24).