# README:

# This file was created by gehatzip and changes were made to xapian_wrap.cc but unfortuanately the project doesn't link so it was abandoned.

cmake_minimum_required(VERSION 3.10)
project(xapian VERSION 1.0.0)

set(CMAKE_CXX_STANDARD 11)
set(CMAKE_CXX_STANDARD_REQUIRED ON)

SET(CMAKE_CONFIGURATION_TYPES "Debug;Release" CACHE STRING "" FORCE)

############################################################################

IF(MSVC)

SET(MY_CMAKE_CXX_FLAGS "-W4 -Zi -DWIN32_LEAN_AND_MEAN -DNOMINMAX -D_CRT_SECURE_NO_WARNINGS -D_CRT_NONSTDC_NO_DEPRECATE -D_WINSOCK_DEPRECATED_NO_WARNINGS -std:c++17 ")
SET(MY_CMAKE_CXX_FLAGS_DEBUG "${MY_CMAKE_CXX_FLAGS} -MTd -Od")
SET(MY_CMAKE_CXX_FLAGS_RELEASE "${MY_CMAKE_CXX_FLAGS} -MT -Ox -DNDEBUG")

ENDIF()

SET(CMAKE_CXX_FLAGS_DEBUG ${MY_CMAKE_CXX_FLAGS_DEBUG} CACHE STRING "" FORCE)
SET(CMAKE_CXX_FLAGS_RELEASE ${MY_CMAKE_CXX_FLAGS_RELEASE} CACHE STRING "" FORCE)

############################################################################

set(PYTHON_ROOT_DIR "D:/development/cpython")
set(PYTHON_BIN_DIR "${PYTHON_ROOT_DIR}/PCbuild/amd64_release")

set(PYTHON_INCLUDE_DIR "${PYTHON_ROOT_DIR}/PC" "${PYTHON_ROOT_DIR}/Include")

set(XAPIAN_CMAKE_ROOT_DIR "D:/development/xapian-core-1.4.25-cmake")
set(XAPIAN_INCLUDE_DIR "${XAPIAN_CMAKE_ROOT_DIR}/include")
set(XAPIAN_BUILD_DIR "${XAPIAN_CMAKE_ROOT_DIR}/build")

#############################################################################

add_library (xapian SHARED
  xapian_wrap.cc
)

#############################################################################
set(VCPKG_BASE_DIR D:/development/vcpkg/installed/x64-windows-static)
set(INC_ZLIB ${VCPKG_BASE_DIR}/include )
set(DBG_ZLIB ${VCPKG_BASE_DIR}/debug/lib/zlibd.lib )
set(REL_ZLIB ${VCPKG_BASE_DIR}/lib/zlib.lib)

set(DBG_LIBXAPIAN ${XAPIAN_CMAKE_ROOT_DIR}/build/Debug/libxapian.lib)
set(REL_LIBXAPIAN ${XAPIAN_CMAKE_ROOT_DIR}/build/Release/libxapian.lib)
set(DBG_LIBGETOPT ${XAPIAN_CMAKE_ROOT_DIR}/build/Debug/libgetopt.lib)
set(REL_LIBGETOPT ${XAPIAN_CMAKE_ROOT_DIR}/build/Release/libgetopt.lib)

if(WIN32)
    # https://stackoverflow.com/questions/2209929/linking-different-libraries-for-debug-and-release-builds-in-cmake-on-windows
    target_link_libraries(xapian 
                            debug ${DBG_ZLIB} 
                            debug ${DBG_LIBXAPIAN} 
                            debug ${DBG_LIBGETOPT} 
                            ws2_32 rpcrt4.lib 
                            optimized ${REL_ZLIB} 
                            optimized ${REL_LIBXAPIAN} 
                            optimized ${REL_LIBGETOPT})
endif()
#############################################################################

target_include_directories(xapian PUBLIC ${CMAKE_CURRENT_SOURCE_DIR} ${PYTHON_INCLUDE_DIR} ${XAPIAN_INCLUDE_DIR} ${XAPIAN_BUILD_DIR} ${XAPIAN_BUILD_DIR}/include ${INC_ZLIB})

target_compile_definitions(xapian PRIVATE THREAD_LOCAL)

set_target_properties (xapian PROPERTIES PREFIX "")
set_target_properties (xapian PROPERTIES OUTPUT_NAME "_xapian")

if (WIN32)
  set_target_properties (xapian PROPERTIES SUFFIX ".pyd")
else()
  set_target_properties (xapian PROPERTIES SUFFIX ".so")
endif()

target_link_directories(xapian PUBLIC
    ${PYTHON_BIN_DIR}
)