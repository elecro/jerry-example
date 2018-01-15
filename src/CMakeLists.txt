# Copyright 2018 elecro
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

cmake_minimum_required(VERSION 3.0)
project(app-jerry)
include(ExternalProject)

set(CMAKE_BUILD_TYPE Debug)

# Set Jerry as an external project to compile
set(EXTERNAL_INSTALL_LOCATION ${CMAKE_BINARY_DIR}/external)
set(JERRY_LIBS jerry-core jerry-libm jerry-port-default)
ExternalProject_Add(libjerry
  PREFIX ${CMAKE_CURRENT_SOURCE_DIR}/../external
  BUILD_IN_SOURCE 0
  UPDATE_COMMAND ""
  GIT_REPOSITORY https://github.com/jerryscript-project/jerryscript.git
  GIT_TAG ded0d5a846679699d5767d1122b0c320a0b3d637
  CMAKE_ARGS
    -DCMAKE_INSTALL_PREFIX=${EXTERNAL_INSTALL_LOCATION}
    -DCMAKE_BUILD_TYPE=${CMAKE_BUILD_TYPE}
    -DCMAKE_C_FLAGS=${CMAKE_C_FLAGS}
    -DENABLE_ALL_IN_ONE=ON
    -DJERRY_CMDLINE=OFF
    -DJERRY_LIBC=OFF
    -DFEATURE_SNAPSHOT_EXEC=ON
    -DFEATURE_SNAPSHOT_SAVE=OFF
    -DFEATURE_PROFILE=es5.1
    -DFEATURE_VM_EXEC_STOP=ON
    -DENABLE_LTO=OFF
    -DFEATURE_ERROR_MESSAGES=ON
)

include_directories(${EXTERNAL_INSTALL_LOCATION}/include)
link_directories(${EXTERNAL_INSTALL_LOCATION}/lib)

# Add example application
add_executable(app-jerry main.c)
add_dependencies(app-jerry libjerry)
target_link_libraries(app-jerry ${JERRY_LIBS})
