# CMake 프로그램의 최소 버전
cmake_minimum_required(VERSION 3.11.0)

# 프로젝트 정보
project(
  claujson_pool_2
  VERSION 0.1.0 #
  DESCRIPTION "claujson with memory_pool_2"
  LANGUAGES CXX)

set(CMAKE_POLICY_DEFAULT_CMP0077 NEW)
set(CMAKE_POLICY_DEFAULT_CMP0079 NEW)
set(CMAKE_POLICY_DEFAULT_CMP0048 NEW)

#set(CMAKE_CXX_STANDARD 17) # C++17
set(CMAKE_BUILD_TYPE "Release")

set(LIB_NAME claujson_pool_2) # static lib
set(LIB_NAME14 claujson_pool14_2) # static lib with c++ 14

set(ROOT .)

set(LIB_DIR ${ROOT})

set(SOURCE_DIR ${ROOT})

set(INCLUDE_DIR ${ROOT}) 

# build setting.
add_library(${LIB_NAME} STATIC ${SOURCE_DIR}/claujson.cpp ${SOURCE_DIR}/_simdjson.cpp ${SOURCE_DIR}/claujson_array.cpp ${SOURCE_DIR}/claujson_object.cpp ${SOURCE_DIR}/claujson_partialjson.cpp ${SOURCE_DIR}/claujson_value.cpp) 
add_library(${LIB_NAME14} STATIC ${SOURCE_DIR}/claujson.cpp ${SOURCE_DIR}/_simdjson.cpp ${SOURCE_DIR}/claujson_array.cpp ${SOURCE_DIR}/claujson_object.cpp ${SOURCE_DIR}/claujson_partialjson.cpp ${SOURCE_DIR}/claujson_value.cpp) 
 
include(FetchContent)

FetchContent_Declare(
  fmt
  GIT_REPOSITORY https://github.com/fmtlib/fmt
  GIT_TAG        e69e5f977d458f2650bb346dadf2ad30c5320281) # 10.2.1
FetchContent_MakeAvailable(fmt)

# header..
target_include_directories(${LIB_NAME}  PUBLIC ${INCLUDE_DIR} PRIVATE ${fmt_SOURCE_DIR}/include)
target_include_directories(${LIB_NAME14} PUBLIC ${INCLUDE_DIR} PRIVATE ${fmt_SOURCE_DIR}/include)


if (UNIX) 
        target_compile_options(${LIB_NAME} PRIVATE -m64 -std=c++2a -march=native -pthread -Wno-narrowing -Wextra -O2)
        target_compile_options(${LIB_NAME14} PRIVATE -m64 -std=c++14 -march=native -pthread -Wno-narrowing -Wextra -O2)
      
elseif (MSVC)

target_compile_definitions(${LIB_NAME} PRIVATE
        "$<$<CONFIG:Release>:"
            "NDEBUG"
        ">"
        "UNICODE;"
        "_UNICODE"
    )
	
	if("${CMAKE_VS_PLATFORM_NAME}" STREQUAL "x64")
        target_compile_options(${LIB_NAME} PRIVATE
            $<$<CONFIG:Release>:
                /std:c++20;
                /Oi;
                /Gy
            >
            /permissive-;
            /sdl;
            /W3;
	    /Zc:__cplusplus; 
            ${DEFAULT_CXX_DEBUG_INFORMATION_FORMAT};
            ${DEFAULT_CXX_EXCEPTION_HANDLING}
        )
    endif()
    
    target_compile_definitions(${LIB_NAME14} PRIVATE
        "$<$<CONFIG:Release>:"
            "NDEBUG"
        ">"
        "UNICODE;"
        "_UNICODE"
    )
	
     if("${CMAKE_VS_PLATFORM_NAME}" STREQUAL "x64")
        target_compile_options(${LIB_NAME14} PRIVATE
            $<$<CONFIG:Release>:
                /std:c++14;
                /Oi;
                /Gy
            >
            /permissive-;
            /sdl;
            /W3;
	    /Zc:__cplusplus; 
            ${DEFAULT_CXX_DEBUG_INFORMATION_FORMAT};
            ${DEFAULT_CXX_EXCEPTION_HANDLING}
        )
    endif()
    

endif()


if (UNIX)
	target_link_libraries(${LIB_NAME} PRIVATE fmt::fmt pthread)
	target_link_libraries(${LIB_NAME14} PRIVATE fmt::fmt pthread)
	
elseif (MSVC)
	target_link_libraries(${LIB_NAME} PRIVATE fmt::fmt  )
	target_link_libraries(${LIB_NAME14} PRIVATE fmt::fmt  )
  
	
endif()
