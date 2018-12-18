libgmsd ![Travis CI](https://travis-ci.org/clunietp/libgmsd.svg?branch=master)
===================
https://github.com/clunietp/libgmsd

Introduction
------------
libgmsd is a portable C library and command-line tool for full-reference image quality assessment.

libgmsd is an implementation of the Gradient Magnitude Similarity Deviation (GMSD) algorithm by Xue, Zhang, Mou, and Bovik.
http://www4.comp.polyu.edu.hk/~cslzhang/IQA/GMSD/GMSD.htm

Features
------------
- Cross-platform; tested MSVC17, GCC 4.6, Clang 5
- Efficient: ~12ms MSVC 15.8, ~6.5ms gcc -o3 (cygwin); 512x384 grayscale input on Core i5-4210U @ 1.7GHz
- No external dependencies
- Unit tested, valgrind clean
- Permissive license (MIT)

Usage
-----------------------
- Library:			see `include/libgmsd.h`
- Command line:		`gmsd.exe "path/to/file1.bmp" "path/to/file2.bmp"`
  * Input:  24BPP BMP; Output:  GMSD value					
  * Win32 command line binary:  https://github.com/clunietp/libgmsd/raw/master/gmsd_win32.zip
- Tests:			`./gmsd_tests`

Tested compilers:
------------
- MSVC 2017
- gcc 4.6
- clang 5

Tests ![Travis CI](https://travis-ci.org/clunietp/libgmsd.svg?branch=master)
-------------
- Tests/benchmark included in `/test/`

Acknowledgements
---------
http://www4.comp.polyu.edu.hk/~cslzhang/IQA/GMSD/GMSD.htm , Gradient Magnitude Similarity Deviation: A Highly Efficient Perceptual Image Quality Index by Xue, Zhang, Mou, Bovik

https://github.com/chazomaticus/libbmpread , tiny, fast bitmap (.bmp) image file loader by Charles Lindsay.  Libbmpread is used in the test and command line apps for bmp input.
