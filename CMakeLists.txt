# Copyright 2012 Philip Puryear
#
# Licensed under the Apache License, Version 2.0 (the "License");
# you may not use this file except in compliance with the License.
# You may obtain a copy of the License at
#
#    http://www.apache.org/licenses/LICENSE-2.0
#
# Unless required by applicable law or agreed to in writing, software
# distributed under the License is distributed on an "AS IS" BASIS,
# WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
# See the License for the specific language governing permissions and
# limitations under the License.

cmake_minimum_required(VERSION 2.6)
project(memrev CXX)

option (MEMREV_BUILD_TESTS
        "Build the test executable(s)." OFF)

set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -std=c++11")
if (APPLE)
    set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -stdlib=libc++")
endif()

if (MEMREV_BUILD_TESTS)
    if (APPLE)
        add_definitions(-DGTEST_USE_OWN_TR1_TUPLE=1)
    endif()
    add_subdirectory(gtest)
    include_directories(gtest/include)
endif()

add_subdirectory(libmemrev)

include_directories(libmemrev/include)
add_executable(rev rev.cc)
target_link_libraries(rev memrev)

install(TARGETS rev DESTINATION bin)
