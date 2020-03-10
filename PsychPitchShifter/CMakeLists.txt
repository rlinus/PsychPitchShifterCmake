# CMakeList.txt : CMake project for PsychPitchShifter, include source and define
# project specific logic here.
#
cmake_minimum_required (VERSION 3.8)


find_package(Matlab)
#find_package(unofficial-libstk CONFIG REQUIRED)


# Add source to this project's executable.
#matlab_add_mex(PsychPitchShifter "PsychPitchShifter.cpp" "PsychPitchShifter.h")
#target_link_libraries(PsychPitchShifter PRIVATE unofficial::libstk::libstk rubberband)
# TODO: Add tests and install targets if needed.


## PrintDeviceList
matlab_add_mex(NAME PrintDeviceList SRC "PrintDeviceList.cpp")
target_include_directories(PrintDeviceList PRIVATE ${rtaudio_INCLUDE_DIR})
target_link_libraries(PrintDeviceList rtaudio)

# bugfix for cmake 3.14 (because it set(_link_flags "${_link_flags} /EXPORT:mexCreateMexFunction /EXPORT:mexDestroyMexFunction /EXPORT:mexFunctionAdapter"))
get_property(LFLAGS TARGET PrintDeviceList PROPERTY LINK_FLAGS)
message(STATUS "LFLAGS = ${LFLAGS}")
set_property(TARGET PrintDeviceList PROPERTY LINK_FLAGS "/EXPORT:mexFunction /EXPORT:mexfilerequiredapiversion")

install(TARGETS PrintDeviceList)

## PitchLoop
matlab_add_mex(NAME PitchLoop SRC "PitchLoop.cpp" "cpvPitchShift.cpp" "smbPitchShift.cpp")
target_include_directories(PitchLoop PRIVATE ${rtaudio_INCLUDE_DIR} ${rubberband_INCLUDE_DIR} ${stk_INCLUDE_DIR})
#target_link_libraries(PitchLoop unofficial::libstk::libstk)
target_link_libraries(PitchLoop libstk)
target_link_libraries(PitchLoop rtaudio)
target_link_libraries(PitchLoop FFTW3::fftw3)
target_link_libraries(PitchLoop rubberband)

# bugfix for cmake 3.14 (because it set(_link_flags "${_link_flags} /EXPORT:mexCreateMexFunction /EXPORT:mexDestroyMexFunction /EXPORT:mexFunctionAdapter"))
get_property(LFLAGS TARGET PitchLoop PROPERTY LINK_FLAGS)
message(STATUS "LFLAGS = ${LFLAGS}")
set_property(TARGET PitchLoop PROPERTY LINK_FLAGS "/EXPORT:mexFunction /EXPORT:mexfilerequiredapiversion")

install(TARGETS PitchLoop)

## PsychPitchShifter
matlab_add_mex(NAME PsychPitchShifter SRC "PsychPitchShifter.cpp" "cpvPitchShift.cpp" "smbPitchShift.cpp" "dywapitchtrack.c")
target_include_directories(PsychPitchShifter PRIVATE ${rtaudio_INCLUDE_DIR} ${rubberband_INCLUDE_DIR} ${stk_INCLUDE_DIR})
#target_link_libraries(PsychPitchShifter unofficial::libstk::libstk)
target_link_libraries(PsychPitchShifter libstk)
target_link_libraries(PsychPitchShifter rtaudio)
target_link_libraries(PsychPitchShifter FFTW3::fftw3)
target_link_libraries(PsychPitchShifter rubberband)

# bugfix for cmake 3.14 (because it set(_link_flags "${_link_flags} /EXPORT:mexCreateMexFunction /EXPORT:mexDestroyMexFunction /EXPORT:mexFunctionAdapter"))
get_property(LFLAGS TARGET PsychPitchShifter PROPERTY LINK_FLAGS)
message(STATUS "LFLAGS = ${LFLAGS}")
set_property(TARGET PsychPitchShifter PROPERTY LINK_FLAGS "/EXPORT:mexFunction /EXPORT:mexfilerequiredapiversion")

install(TARGETS PsychPitchShifter)
install(FILES ${CMAKE_CURRENT_BINARY_DIR}/fftw3.dll DESTINATION bin)

install(FILES ${CMAKE_CURRENT_SOURCE_DIR}/PsychPitchShifter.m DESTINATION bin)
install(FILES ${CMAKE_CURRENT_SOURCE_DIR}/PitchLoop.m DESTINATION bin)
install(FILES ${CMAKE_CURRENT_SOURCE_DIR}/PrintDeviceList.m DESTINATION bin)