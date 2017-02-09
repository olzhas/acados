cmake_minimum_required(VERSION 2.8.11)

find_package(SWIG REQUIRED)
set(CMAKE_MODULE_PATH ${CMAKE_CURRENT_SOURCE_DIR}/../cmake)
# SWIG wrapper file introduces some warnings
if (CMAKE_CXX_COMPILER_ID MATCHES "Clang")
    set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -Wno-missing-field-initializers -Wno-deprecated-register")
elseif (CMAKE_CXX_COMPILER_ID MATCHES "GNU")
    set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -Wno-missing-field-initializers")
endif ()

include(${SWIG_USE_FILE})

# Python
find_package(PythonInterp 3 REQUIRED)
find_package(PythonLibs 3 REQUIRED)
include_directories(${PYTHON_INCLUDE_DIRS})
find_package(NumPy REQUIRED)
include_directories(${PYTHON_NUMPY_INCLUDE_DIR})
make_directory(${PROJECT_BINARY_DIR}/python/acados)
set_source_files_properties(acados.i  PROPERTIES  CPLUSPLUS ON)
# SWIG typemaps debug flag
# set(CMAKE_SWIG_FLAGS "-debug-tmused")
swig_add_module(acados python acados.i)
swig_link_libraries(acados ${PYTHON_LIBRARIES} acados blasfeo)

file(COPY test_swig.py DESTINATION ${PROJECT_BINARY_DIR}/swig)
file(COPY list_of_arrays.py DESTINATION ${PROJECT_BINARY_DIR}/swig)