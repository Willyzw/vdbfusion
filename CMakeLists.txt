cmake_minimum_required(VERSION 3.10)
project(VDBFusion VERSION 0.1.5 LANGUAGES CXX)

# Setup build options
option(USE_CCACHE "Build using Ccache if found on the path" ON)
option(USE_SYSTEM_EIGEN3 "Use system pre-installed eigen3" ON)
option(USE_SYSTEM_OPENVDB "Use system pre-installed OpenVDB" ON)
option(USE_SYSTEM_PYBIND11 "Use system pre-installed pybind11" ON)
option(PYOPENVDB_SUPPORT "Add suuport for pyopenvdb, this depends on boost::python" OFF)
option(SILENCE_WARNINGS "To build manylinux packages only, disable on the global scope" OFF)
option(BUILD_CXX_EXAMPLE "Build C++ examples" OFF)

mark_as_advanced(USE_CCACHE USE_SYSTEM_OPENVDB PYOPENVDB_SUPPORT SILENCE_WARNINGS)

if(SILENCE_WARNINGS)
  message(WARNING "Disable all warnings for all targets in the build system, only for the CI/CD")
  add_compile_options(-w)
endif()

# ccache setup
if(USE_CCACHE)
  find_program(CCACHE_PATH ccache)
  if(CCACHE_PATH)
    set_property(GLOBAL PROPERTY RULE_LAUNCH_COMPILE ccache)
    set_property(GLOBAL PROPERTY RULE_LAUNCH_LINK ccache)
    message(STATUS "Using ccache: ${CCACHE_PATH}")
  endif()
endif()

set(CMAKE_BUILD_TYPE Release)
set(CMAKE_EXPORT_COMPILE_COMMANDS ON)
set(CMAKE_POSITION_INDEPENDENT_CODE ON)
set(CMAKE_CXX_STANDARD_REQUIRED ON)
set(CMAKE_CXX_EXTENSIONS OFF)

# Find Dependencies from the 3rdparty directory.
include(3rdparty/find_dependencies.cmake)

if(BUILD_CXX_EXAMPLE)
  add_subdirectory(examples)
endif()
add_subdirectory(src)
