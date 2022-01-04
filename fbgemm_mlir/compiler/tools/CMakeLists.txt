add_executable(fbgemm-mlir-opt fbgemm-mlir-opt.cpp)
get_property(dialect_libs GLOBAL PROPERTY MLIR_DIALECT_LIBS)
get_property(conversion_libs GLOBAL PROPERTY MLIR_CONVERSION_LIBS)
target_link_directories(fbgemm-mlir-opt PRIVATE ${TORCH_MLIR_LIB})
target_link_libraries(fbgemm-mlir-opt PRIVATE
  MLIROptLib
  TorchMLIRInitAll
  TorchMLIRRefBackend
  TorchMLIRConversionPasses
  TorchMLIRTorchConversionDialect
  TorchMLIRTorchConversionPasses
  TorchMLIRTorchDialect
  TorchMLIRTorchPasses
  TorchMLIRTorchToLinalg
  TorchMLIRTorchToSCF
  TorchMLIRTorchToStd
  TorchMLIRTorchToTosa
  TorchMLIRTorchUtils
  ${dialect_libs}
  ${conversion_libs}
)
