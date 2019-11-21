cmake_minimum_required(VERSION 3.5)
project(AMDVLK)

# LLVM
# LLVM will be switched to llvm monorepo. Check if the new llvm exist first and fallback to
# the old llvm if it doesn't during the transition. The old path should be removed once the transition gets completed.
if(EXISTS ${PROJECT_SOURCE_DIR}/llvm-project/llvm)
    set(XGL_LLVM_SRC_PATH ${PROJECT_SOURCE_DIR}/llvm-project/llvm CACHE PATH "Specify the path to the LLVM.")
else()
    set(XGL_LLVM_SRC_PATH ${PROJECT_SOURCE_DIR}/llvm CACHE PATH "Specify the path to the LLVM.")
endif()
set(LLVM_TARGETS_TO_BUILD AMDGPU CACHE STRING Force)
add_subdirectory(${XGL_LLVM_SRC_PATH} ${PROJECT_BINARY_DIR}/llvm)
set(XGL_LLVM_BUILD_PATH ${CMAKE_BINARY_DIR}/llvm)
add_subdirectory(xgl)

install(FILES json/Redhat/amd_icd64.json DESTINATION /etc/vulkan/icd.d)
