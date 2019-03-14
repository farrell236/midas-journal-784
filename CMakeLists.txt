## ==============================================================================
##
##  CMake Project Settings
##
## ==============================================================================

cmake_minimum_required(VERSION 3.4)

project(ImageRegistration2D3D)

find_package(ITK REQUIRED)

ENABLE_TESTING()
INCLUDE(Dart)


## ==============================================================================
##
##  ImageRegistration2D3D
##
## ==============================================================================

include(${ITK_USE_FILE})

include_directories(${ITK_INCLUDE_DIRS})
link_directories(${ITK_LIBRARIES_DIR})

SET(CurrentExe "DicomSeriesReadNiftiImageWrite")
ADD_EXECUTABLE(${CurrentExe} DicomSeriesReadNiftiImageWrite.cxx) 
TARGET_LINK_LIBRARIES(${CurrentExe} ${ITK_LIBRARIES})

SET(CurrentExe "ReadResampleWriteNifti")
ADD_EXECUTABLE(${CurrentExe} ReadResampleWriteNifti.cxx) 
TARGET_LINK_LIBRARIES(${CurrentExe} ${ITK_LIBRARIES})

SET(CurrentExe "getDRRSiddonJacobsRayTracing")
ADD_EXECUTABLE(${CurrentExe} getDRRSiddonJacobsRayTracing.cxx) 
TARGET_LINK_LIBRARIES(${CurrentExe} ${ITK_LIBRARIES})

SET(CurrentExe "2D3DTwoProjRegistration")
ADD_EXECUTABLE(${CurrentExe} 2D3DTwoProjRegistration.cxx) 
TARGET_LINK_LIBRARIES(${CurrentExe} ${ITK_LIBRARIES})

SET(CurrentExe "ImageCompare")
ADD_EXECUTABLE(${CurrentExe} ImageCompare.cxx) 
TARGET_LINK_LIBRARIES(${CurrentExe} ${ITK_LIBRARIES})


## ==============================================================================
##
##  Testing
##
## ==============================================================================

#the following line is an example of how to add a test to your project.
#Testname is the title for this particular test.  ExecutableToRun is the
#program which will be running this test.  It can either be a part of this
#project or an external executable.  After that list any args that are needed
#for this test.  Include as many tests as you like.  If your project doesn't have
#any tests you can comment out or delete the following line.
ADD_TEST(CreateProjectionImage getDRRSiddonJacobsRayTracing -rp 0 -iso 255 259 130  -rx -3 -ry 4 -rz 2 -t 5 5 5 -res 0.5 0.5 -size 512 512 -o ${CMAKE_SOURCE_DIR}/images/BoxheadDRR_Disp1_G0.tif ${CMAKE_SOURCE_DIR}/images/BoxheadCTFull.img)
ADD_TEST(CreateProjectionImage getDRRSiddonJacobsRayTracing -rp 90 -iso 255 259 130  -rx -3 -ry 4 -rz 2 -t 5 5 5 -res 0.5 0.5 -size 512 512 -o ${CMAKE_SOURCE_DIR}/images/BoxheadDRR_Disp1_G90.tif ${CMAKE_SOURCE_DIR}/images/BoxheadCTFull.img)
ADD_TEST(2D3DTwoProjRegistration 2D3DTwoProjRegistration -res 0.5 0.5 0.5 0.5 -iso 255 259 130 -o ${CMAKE_SOURCE_DIR}/images/BoxheadDRR_Disp1_G0_Reg.tif ${CMAKE_SOURCE_DIR}/images/BoxheadDRR_Disp1_G90_Reg.tif ${CMAKE_SOURCE_DIR}/images/BoxheadDRR_Disp1_G0.tif 0 ${CMAKE_SOURCE_DIR}/images/BoxheadDRR_Disp1_G90.tif 90 ${CMAKE_SOURCE_DIR}/images/BoxheadCTFull.img)
ADD_TEST(CompareImage ImageCompare ${CMAKE_SOURCE_DIR}/images/BoxheadDRR_Disp1_G0_Reg_Author.tif ${CMAKE_SOURCE_DIR}/images/BoxheadDRR_Disp1_G0_Reg.tif)
ADD_TEST(CompareImage ImageCompare ${CMAKE_SOURCE_DIR}/images/BoxheadDRR_Disp1_G90_Reg_Author.tif ${CMAKE_SOURCE_DIR}/images/BoxheadDRR_Disp1_G90_Reg.tif)


## ==============================================================================
##
##  Configuration summary
##
## ==============================================================================

message (STATUS "------------------------------------------------------------"  )
message (STATUS "[ImageRegistration2D3D] Configuration summary."                )
message (STATUS "------------------------------------------------------------"  )
message (STATUS " System configuration:"                                        )
message (STATUS " .. Processor type ............. = ${CMAKE_SYSTEM_PROCESSOR}"  )
message (STATUS " .. CMake executable ........... = ${CMAKE_COMMAND}"           )
message (STATUS " .. CMake version .............. = ${CMAKE_VERSION}"           )
message (STATUS " .. System name ................ = ${CMAKE_SYSTEM}"            )
message (STATUS " .. C++ compiler ............... = ${CMAKE_CXX_COMPILER}"      )
message (STATUS " .. C compiler ................. = ${CMAKE_C_COMPILER}"        )
message (STATUS " .. size(void*) ................ = ${CMAKE_SIZEOF_VOID_P}"     )
message (STATUS "------------------------------------------------------------"  )

