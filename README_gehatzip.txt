This repo was created by me after unsuccessfully trying to build python3 bindings from the official xapian bindings github repo: https://github.com/easysoftware/xapian-bindings
What I did is:
- Copied sources from:
  'xapian-bindings-1.4.25/python3' ('xapian-bindings-1.4.25' was cloned from github xapian bindings repo)
  NOTE: './configure' needs to be run in order that swig is executed and 'xapian_wrap.cc', 'xapian_wrap.h', 'xapian__init__.py' are generated again.
  Other obscure changes also take place like renamed 'python3\xapian.py' to '__init__.py'
- Used ideas from: 
  'xapian-core-1.4.25-cmake/CMakeLists.txt'

Change Python and Xapian bin and include directories in CMakeLists.txt to your own.
Copy __init__.py to the build directory of the xapian python module and you are ready!