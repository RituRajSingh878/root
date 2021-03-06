############################################################################
# CMakeLists.txt file for building TMVA/DNN/RNN tests.
# @author Saurav Shekhar
############################################################################

project(tmva-tests)
find_package(ROOT REQUIRED)

set(Libraries Core MathCore Matrix TMVA)

if (CUDA_FOUND AND  tmva-gpu)
include_directories(${ROOT_INCLUDE_DIRS} ${CUDA_INCLUDE_DIRS})
else()
include_directories(${ROOT_INCLUDE_DIRS})
endif()


# RNN - BackPropagation Reference
ROOT_EXECUTABLE(testRecurrentBackpropagation TestRecurrentBackpropagation.cxx LIBRARIES ${Libraries})
ROOT_ADD_TEST(TMVA-DNN-RNN-Backpropagation COMMAND testRecurrentBackpropagation)
#
## RNN - Initialization Reference
#ROOT_EXECUTABLE(testRecurrentNetInit TestRecurrentNetInitialization.cxx LIBRARIES ${Libraries})
#ROOT_ADD_TEST(TMVA-DNN-RNN-Init COMMAND testRecurrentNetInit)

# RNN - Forward Reference
ROOT_EXECUTABLE(testRecurrentForwardPass TestRecurrentForwardPass.cxx LIBRARIES ${Libraries})
ROOT_ADD_TEST(TMVA-DNN-RNN-Forward COMMAND testRecurrentForwardPass)

# RNN - Full Test Reference
ROOT_EXECUTABLE(testFullRNN TestFullRNN.cxx LIBRARIES ${Libraries})
ROOT_ADD_TEST(TMVA-DNN-RNN-FullRNN COMMAND testFullRNN)

# RNN - Loss Reference
#ROOT_EXECUTABLE(testRecurrentNetLoss TestRecurrentNetLoss.cxx LIBRARIES ${Libraries})
#ROOT_ADD_TEST(TMVA-DNN-RNN-Loss COMMAND testRecurrentNetLoss)
#
## RNN - Prediction Reference
#ROOT_EXECUTABLE(testRecurrentNetPred TestRecurrentNetPrediction.cxx LIBRARIES ${Libraries})
#ROOT_ADD_TEST(TMVA-DNN-RNN-Pred COMMAND testRecurrentNetPred)

#--- CUDA tests. ---------------------------
if (CUDA_FOUND AND tmva-gpu)

  SET(DNN_CUDA_LIBRARIES dnn_cuda ${CUDA_CUBLAS_LIBRARIES})

endif()

#--- CPU tests. ----------------------------
if ((BLAS_FOUND OR mathmore) AND imt AND tmva-cpu)
  include_directories(${TBB_INCLUDE_DIRS})

  # DNN - Forward CPU
  ROOT_EXECUTABLE(testRecurrentForwardPassCpu TestRecurrentForwardPassCpu.cxx LIBRARIES ${Libraries})
  ROOT_ADD_TEST(TMVA-DNN-RNN-Forward-Cpu COMMAND testRecurrentForwardPassCpu)

 ROOT_EXECUTABLE(testRecurrentBackpropagationCpu TestRecurrentBackpropagationCpu.cxx LIBRARIES ${Libraries})
 ROOT_ADD_TEST(TMVA-DNN-RNN-Backpropagation-Cpu COMMAND testRecurrentBackpropagationCpu)

# RNN - Full Test Reference
ROOT_EXECUTABLE(testFullRNNCpu TestFullRNNCpu.cxx LIBRARIES ${Libraries})
ROOT_ADD_TEST(TMVA-DNN-RNN-FullRNN-Cpu COMMAND testFullRNNCpu)


endif ()
