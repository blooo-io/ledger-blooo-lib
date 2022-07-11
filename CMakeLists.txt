#*******************************************************************************
#*   (c) 2018 Zondax GmbH
#*
#*  Licensed under the Apache License, Version 2.0 (the "License");
#*  you may not use this file except in compliance with the License.
#*  You may obtain a copy of the License at
#*
#*      http://www.apache.org/licenses/LICENSE-2.0
#*
#*  Unless required by applicable law or agreed to in writing, software
#*  distributed under the License is distributed on an "AS IS" BASIS,
#*  WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
#*  See the License for the specific language governing permissions and
#*  limitations under the License.
#********************************************************************************
cmake_minimum_required(VERSION 3.0)
project(ledger-blooo-lib)

set(CMAKE_CXX_STANDARD 11)

add_subdirectory(cmake/gtest)

include_directories(
        ${CMAKE_CURRENT_SOURCE_DIR}/include
)

###############

file(GLOB_RECURSE BLOOO_LIB_SRC
        ${CMAKE_CURRENT_SOURCE_DIR}/src/*.c
        )

file(GLOB_RECURSE TESTS_SRC
        ${CMAKE_CURRENT_SOURCE_DIR}/tests/*.cpp
        )

###############
set(BUILD_TESTS OFF CACHE BOOL "Enables tests")

add_library(blooo_lib STATIC ${BLOOO_LIB_SRC})
target_include_directories(blooo_lib PRIVATE ${CMAKE_CURRENT_SOURCE_DIR}/include)

enable_testing()

add_executable(blooo_lib_tests
        ${TESTS_SRC}
        )

target_include_directories(blooo_lib_tests PRIVATE
        ${CMAKE_CURRENT_SOURCE_DIR}/include
        ${gtest_SOURCE_DIR}/include
        ${gmock_SOURCE_DIR}/include
        )

target_link_libraries(blooo_lib_tests gtest_main blooo_lib)

add_test(BLOOO_LIB_TESTS blooo_lib_tests)
