cmake_minimum_required(VERSION 2.6)
project(LAPACK Fortran)
if (UNIX)
   if ( "${CMAKE_Fortran_COMPILER}" MATCHES "ifort" )
  set( CMAKE_Fortran_FLAGS "${CMAKE_Fortran_FLAGS} -fltconsistency -fp_port" )
   endif ()
endif ()
enable_testing()
include(CTest)
enable_testing()
set(CMAKE_MODULE_PATH ${LAPACK_SOURCE_DIR} ${CMAKE_MODULE_PATH})
include(CheckTimeFunction)
set(TIME_FUNC NONE ${TIME_FUNC})
CHECK_TIME_FUNCTION(NONE TIME_FUNC)
CHECK_TIME_FUNCTION(INT_CPU_TIME TIME_FUNC)
CHECK_TIME_FUNCTION(EXT_ETIME TIME_FUNC)
CHECK_TIME_FUNCTION(EXT_ETIME_ TIME_FUNC)
CHECK_TIME_FUNCTION(INT_ETIME TIME_FUNC)
message(STATUS "--> Will use second_${TIME_FUNC}.f and dsecnd_${TIME_FUNC}.f as timing function.")

set(SECOND_SRC  ${LAPACK_SOURCE_DIR}/INSTALL/second_${TIME_FUNC}.f)
set(DSECOND_SRC  ${LAPACK_SOURCE_DIR}/INSTALL/dsecnd_${TIME_FUNC}.f)

add_subdirectory(BLAS)
add_subdirectory(SRC)
add_subdirectory(TESTING)

set(LAPACK_VERSION 3.2.1)
set(CPACK_PACKAGE_VERSION_MAJOR 3)
set(CPACK_PACKAGE_VERSION_MINOR 2)
set(CPACK_PACKAGE_VERSION_PATCH 1)
include(CPack)
export(TARGETS blas FILE lapack-targets.cmake)
configure_file(${LAPACK_SOURCE_DIR}/lapack-config-version.cmake.in
  ${LAPACK_BINARY_DIR}/lapack-config-version.cmake @ONLY)
configure_file(${LAPACK_SOURCE_DIR}/lapack-config.cmake.in
  ${LAPACK_BINARY_DIR}/lapack-config.cmake @ONLY)