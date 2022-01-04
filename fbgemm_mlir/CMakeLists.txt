cmake_minimum_required(VERSION 3.18)
project(fbgemm_mlir LANGUAGES C CXX)
include(ExternalProject)

set(TP_DIR ${CMAKE_CURRENT_LIST_DIR}/../third_party/)
ExternalProject_Add(torch-mlir
  PREFIX ${CMAKE_BINARY_DIR}/torch-mlir
  SOURCE_DIR ${TP_DIR}/torch-mlir/external/llvm-project/llvm
  BINARY_DIR ${CMAKE_BINARY_DIR}/torch-mlir
  INSTALL_COMMAND ${CMAKE_COMMAND} -E echo "Skipping install step."
  CMAKE_ARGS
    -DCMAKE_C_COMPILER=${CMAKE_C_COMPILER}
    -DCMAKE_CXX_COMPILER=${CMAKE_CXX_COMPILER}
    -DPython3_FIND_VIRTUALENV=ONLY
    -DLLVM_ENABLE_PROJECTS=mlir
    -DLLVM_EXTERNAL_PROJECTS=torch-mlir
    -DLLVM_EXTERNAL_TORCH_MLIR_SOURCE_DIR=${TP_DIR}/torch-mlir
    -DMLIR_ENABLE_BINDINGS_PYTHON=ON
    -DLLVM_TARGETS_TO_BUILD=host
)

set(TORCH_MLIR_LIB ${CMAKE_BINARY_DIR}/torch-mlir/lib)
set(TORCH_MLIR_INC ${TP_DIR}/torch-mlir/include)
set(TORCH_MLIR_GEN_INC ${CMAKE_BINARY_DIR}/torch-mlir/tools/torch-mlir/include)
set(MLIR_DIR ${CMAKE_BINARY_DIR}/torch-mlir/lib/cmake/mlir)
ExternalProject_Add(compiler
  DEPENDS torch-mlir
  PREFIX ${CMAKE_BINARY_DIR}/compiler
  SOURCE_DIR ${CMAKE_CURRENT_LIST_DIR}/compiler
  BINARY_DIR ${CMAKE_BINARY_DIR}/compiler
  INSTALL_COMMAND ${CMAKE_COMMAND} -E echo "Skipping install step."
  CMAKE_ARGS
    -DCMAKE_C_COMPILER=${CMAKE_C_COMPILER}
    -DCMAKE_CXX_COMPILER=${CMAKE_CXX_COMPILER}
    -DTORCH_MLIR_LIB=${TORCH_MLIR_LIB}
    -DTORCH_MLIR_INC=${TORCH_MLIR_INC}
    -DTORCH_MLIR_GEN_INC=${TORCH_MLIR_GEN_INC}
    -DMLIR_DIR=${MLIR_DIR}
)
