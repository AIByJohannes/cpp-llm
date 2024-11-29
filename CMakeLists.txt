# CMakeList.txt : CMake project for cpp-llm, include source and define
# project specific logic here.
#
cmake_minimum_required (VERSION 3.8)

set(ONNXRUNTIME_ROOTDIR ${CMAKE_SOURCE_DIR}/externals/onnxruntime)

# Enable Hot Reload for MSVC compilers if supported.
if (POLICY CMP0141)
  cmake_policy(SET CMP0141 NEW)
  set(CMAKE_MSVC_DEBUG_INFORMATION_FORMAT "$<IF:$<AND:$<C_COMPILER_ID:MSVC>,$<CXX_COMPILER_ID:MSVC>>,$<$<CONFIG:Debug,RelWithDebInfo>:EditAndContinue>,$<$<CONFIG:Debug,RelWithDebInfo>:ProgramDatabase>>")
endif()

project ("cpp-llm")

# Add source to this project's executable.
add_executable (cpp-llm "cpp-llm.cpp" "cpp-llm.h")

add_library(onnxruntime SHARED IMPORTED GLOBAL)
set_target_properties(onnxruntime PROPERTIES
    IMPORTED_IMPLIB "${ONNXRUNTIME_ROOTDIR}/lib/onnxruntime.lib"
    IMPORTED_LOCATION "${ONNXRUNTIME_ROOTDIR}/lib/onnxruntime.dll"
    INTERFACE_INCLUDE_DIRECTORIES "${ONNXRUNTIME_ROOTDIR}/include"
)

target_link_libraries(cpp-llm PRIVATE onnxruntime)

if (CMAKE_VERSION VERSION_GREATER 3.12)
  set_property(TARGET cpp-llm PROPERTY CXX_STANDARD 20)
endif()

# TODO: Add tests and install targets if needed.
