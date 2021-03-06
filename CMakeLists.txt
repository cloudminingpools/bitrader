cmake_minimum_required(VERSION 3.1.0)

project(bitrader)

set(PROJECT_VERSION_MAJOR 0)
set(PROJECT_VERSION_MINOR 1)
set(PROJECT_VERSION_PATCH 0)

set(CMAKE_CXX_STANDARD 11)

include(FindOpenMP)
if(OPENMP_FOUND)
  set(CMAKE_C_FLAGS "${CMAKE_C_FLAGS} ${OpenMP_C_FLAGS}")
  set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} ${OpenMP_CXX_FLAGS}")
  set(CMAKE_EXE_LINKER_FLAGS "${CMAKE_EXE_LINKER_FLAGS} ${OpenMP_EXE_LINKER_FLAGS}")
endif()

# Manual build type selection (for debugging purposes)
#set(CMAKE_BUILD_TYPE Debug)
set(CMAKE_BUILD_TYPE Release)

add_subdirectory(binance-cxx-api)
add_subdirectory(tgbot-cpp)

find_package(PkgConfig REQUIRED)
pkg_check_modules(GTK3 REQUIRED gtk+-3.0)

include_directories(${CMAKE_CURRENT_SOURCE_DIR})
include_directories(${CMAKE_CURRENT_SOURCE_DIR}/binance-cxx-api/include)
include_directories(${CMAKE_CURRENT_SOURCE_DIR}/tgbot-cpp/include)
include_directories(${GTK3_INCLUDE_DIRS})

link_directories(${CMAKE_CURRENT_BINARY_DIR}/binance-cxx-api)
link_directories(${CMAKE_CURRENT_BINARY_DIR}/tgbot-cpp)
link_directories(${GTK3_LIBRARY_DIRS})

add_executable(bitrader bitrader.cpp telegram.h telegram.cpp telegram_bot.cpp)
target_link_libraries(bitrader binance-cxx-api tgbot-cpp)

add_executable(bihistorian bihistorian.cpp)
target_link_libraries(bihistorian binance-cxx-api)

add_executable(biviewer biviewer.cpp)
target_link_libraries(biviewer ${GTK3_LIBRARIES} archive)

