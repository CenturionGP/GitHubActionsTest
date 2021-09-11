cmake_minimum_required(VERSION 3.1)
# CMake setup for main library and sample executable
project(CMake_test)
if (MSVC)
    string(REPLACE "/MD" "/MT" CMAKE_C_FLAGS_DEBUG            ${CMAKE_C_FLAGS_DEBUG})
    string(REPLACE "/MD" "/MT" CMAKE_C_FLAGS_MINSIZEREL       ${CMAKE_C_FLAGS_MINSIZEREL})
    string(REPLACE "/MD" "/MT" CMAKE_C_FLAGS_RELEASE          ${CMAKE_C_FLAGS_RELEASE})
    string(REPLACE "/MD" "/MT" CMAKE_C_FLAGS_RELWITHDEBINFO   ${CMAKE_C_FLAGS_RELWITHDEBINFO})
    string(REPLACE "/MD" "/MT" CMAKE_CXX_FLAGS_DEBUG          ${CMAKE_CXX_FLAGS_DEBUG})
    string(REPLACE "/MD" "/MT" CMAKE_CXX_FLAGS_MINSIZEREL     ${CMAKE_CXX_FLAGS_MINSIZEREL})
    string(REPLACE "/MD" "/MT" CMAKE_CXX_FLAGS_RELEASE        ${CMAKE_CXX_FLAGS_RELEASE})
    string(REPLACE "/MD" "/MT" CMAKE_CXX_FLAGS_RELWITHDEBINFO ${CMAKE_CXX_FLAGS_RELWITHDEBINFO})
endif (MSVC)
# option(CMAKE_USE_WIN32_THREADS_INIT "using WIN32 threads" ON)
# option(gtest_disable_pthreads "Disable uses of pthreads in gtest." ON)
add_subdirectory(third_party/googletest/googletest)
add_library(libSampleApi STATIC
    src/libSampleApi/libSampleApi.cpp
    src/libSampleCoreLib/libSampleCoreLib.cpp
)
add_executable(main_app sample/main.cpp)
target_include_directories(libSampleApi PRIVATE
    src/include
    ./include
)
target_include_directories(main_app PRIVATE
    ./include
)
add_dependencies(main_app libSampleApi)
target_link_libraries(main_app 
    libSampleApi
)
target_link_directories(main_app PRIVATE build/${CMAKE_BUILD_TYPE})

# CMake setup for test module powerd by gtest
add_executable(test
    test/test.cpp
)
target_link_libraries(test
    libSampleApi
    gtest_main
)
target_include_directories(test PUBLIC
    ./include
    third_party/googletest/googletest/include
)
set_property( DIRECTORY PROPERTY VS_STARTUP_PROJECT "main_app" )
