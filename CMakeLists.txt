# CMakeList.txt : Top-level CMake project file, do global configuration
# and include sub-projects here.
#
cmake_minimum_required (VERSION 3.8)

project ("PsychPitchShifterCMake")

#unset(CMAKE_DEBUG_POSTFIX CACHE)

find_package(fftw3)

# Include sub-projects.
add_subdirectory ("rtaudio-master")
add_subdirectory("stk-master")
add_subdirectory ("rubberband-cmake")

set(rtaudio_INCLUDE_DIR "${CMAKE_CURRENT_SOURCE_DIR}/rtaudio-master")
set(rubberband_INCLUDE_DIR "${CMAKE_CURRENT_SOURCE_DIR}/rubberband-cmake/rubberband")
add_subdirectory ("PsychPitchShifter")


install(DIRECTORY ${CMAKE_CURRENT_SOURCE_DIR}/stk-master/rawwaves DESTINATION bin)