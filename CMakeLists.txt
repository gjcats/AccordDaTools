cmake_minimum_required(VERSION 3.5)

project(HarmonieDaTools)

enable_language(Fortran)
find_package (Python3 COMPONENTS Interpreter)

enable_testing()

if(CMAKE_Fortran_COMPILER_ID MATCHES "GNU")
    set(dialect "-ffree-form -fimplicit-none -fconvert=big-endian")
    set(bounds "-fbounds-check")
endif()

if(CMAKE_Fortran_COMPILER_ID MATCHES "Intel")
    set(dialect "-free -implicitnone -convert big_endian")
    set(bounds "-check bounds")
endif()

set(CMAKE_Fortran_FLAGS_DEBUG "${CMAKE_Fortran_FLAGS_DEBUG} ${bounds}")
set(CMAKE_Fortran_FLAGS "${CMAKE_Fortran_FLAGS} ${dialect}")

#
# Compile.
#

file(GLOB_RECURSE sources  src/*.F90 src/*.h)
add_executable(jbdiagnose.x ${sources})

# install executables and scripts
install (TARGETS jbdiagnose.x
         PERMISSIONS OWNER_EXECUTE OWNER_WRITE OWNER_READ GROUP_EXECUTE GROUP_READ
         DESTINATION libexec)

add_subdirectory (scripts)
add_subdirectory (share)
add_subdirectory (tests)
