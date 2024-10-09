This repo contains Xapian 1.4.25 Python Bindings that can be built with cmake / MSVC.

Xapian bindings 1.4.25 for various languages was used:
https://oligarchy.co.uk/xapian/1.4.25/xapian-bindings-1.4.25.tar.xz
Copied:
'xapian_wrap.cc'
'xapian_wrap.h'
'xapian__init__.py' as xapian/__init__.py

- To build:
mkdir build
cd build
cmake -S .. -B .
cmake --build .
copy Debug\_xapian.exp ..\xapian
copy Debug\_xapian.lib ..\xapian
copy Debug\_xapian.pyd ..\xapian

- To use in a python script:
import xapian






