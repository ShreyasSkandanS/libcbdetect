cmake_minimum_required(VERSION 2.8)
project(libcbdetect)

#set(EXECUTABLE_OUTPUT_PATH ${PROJECT_SOURCE_DIR}/bin/${CMAKE_BUILD_TYPE})
#set(LIBRARY_OUTPUT_PATH ${PROJECT_SOURCE_DIR}/lib/${CMAKE_BUILD_TYPE})
#set(CMAKE_LIBRARY_PATH ${PROJECT_SOURCE_DIR}/lib/${CMAKE_BUILD_TYPE})

find_package(catkin REQUIRED COMPONENTS)

set(CMAKE_CXX_COMPILER g++)
set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -std=c++14 -fopenmp")
add_definitions(-DUNIX)

catkin_package(INCLUDE_DIRS include
              LIBRARIES libcbdetect)

option(SHARED_LIB "Request build of shared libraries." ON)
if (SHARED_LIB)
  set(LIB_TYPE SHARED)
else ()
  set(LIB_TYPE STATIC)
endif ()

find_package(OpenCV REQUIRED)

set(THIRD_PARTY_INCLUDE_DIRS ${OpenCV_INCLUDE_DIRS})
set(THIRD_PARTY_LIBS ${OpenCV_LIBS})
include_directories(${THIRD_PARTY_INCLUDE_DIRS}
                    ${PROJECT_SOURCE_DIR}/include)

#add_subdirectory(${PROJECT_SOURCE_DIR}/src)
#add_subdirectory(${PROJECT_SOURCE_DIR}/src/libcbdetect)

include_directories(${PROJECT_SOURCE_DIR}/include/libcbdetect)
aux_source_directory(${PROJECT_SOURCE_DIR}/src/libcbdetect SOURCE_LIST)
add_library(libcbdetect ${LIB_TYPE} ${SOURCE_LIST})
target_link_libraries(libcbdetect ${OpenCV_LIBS})


install(DIRECTORY ${PROJECT_NAME}
        DESTINATION ${CATKIN_PACKAGE_INCLUDE_DESTINATION})	
