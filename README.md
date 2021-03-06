The conda package is build on
[centos 5.11](https://github.com/dfroger/conda-build-env) (tag 0.4.0).

The following steps are executed on Ubuntu 14.04 .

Install the `secondary_dep` package:

    $ conda create -c dfroger -n sec secondary_dep
    $ source activate sec

We will need `cmake`:

    $ conda install cmake

## Using gcc from the system

We use `gcc` from the system:

    $ which gcc
    /usr/bin/gcc

Link with the library foo (provided by `secondary_dep`) (**success**)

    $ cd example
    $ mkdir build; cd build
    $ cmake ..
    $ VERBOSE=1 make
    ...
    /usr/bin/cc     CMakeFiles/main.dir/main.c.o  -o main -rdynamic /media/david/data/miniconda/envs/sec/lib/libfoo.so -Wl,-rpath,/media/david/data/miniconda/envs/sec/lib 
    ...

## Using gcc from conda

Install gcc from conda:

    $ conda install -c asmeurer gcc

Link with the library foo (provided by `secondary_dep` (**error**):

    $ cd example
    $ rm -rf build; mkdir build; cd build
    $ cmake ..
    $ VERBOSE=1 make
    ...
    /media/david/data/miniconda/envs/sec/bin/cc     CMakeFiles/main.dir/main.c.o -o main -rdynamic -lfoo 
    /usr/bin/ld: warning: libbar.so, needed by /media/david/data/miniconda/envs/sec/lib/gcc/x86_64-unknown-linux-gnu/4.8.5/../../../libfoo.so, not found (try using -rpath or -rpath-link)
    /media/david/data/miniconda/envs/sec/lib/gcc/x86_64-unknown-linux-gnu/4.8.5/../../../libfoo.so: undefined reference to `bar'
    collect2: error: ld returned 1 exit status
    ...

## Using gcc from conda in another environment

Remove gcc from the `sec` (current) environment:

    $ conda remove gcc

Install `gcc` in another environment:

    $ conda create -n gcc -c asmeurer gcc
    $ GCC_ENV_DIR=$(conda info -e | grep gcc | tr -s ' ' | cut -d' ' -f2)
    $ export PATH=$GCC_ENV_DIR/bin:$PATH

This works as with `/usr/bin/gcc`
