## step1
project(Tutorial)

add_executable(Tutorial tutorial.cxx)  
![alt text](images/step1.png)

## step2
cmake_minimum_required(VERSION 3.3)  
project(Tutorial)  

set(CMAKE_CXX_STANDARD 11)  
set(CMAKE_CXX_STANDARD_REQUIRED True)  

/# the version number.  
set(Tutorial_VERSION_MAJOR 1)  
set(Tutorial_VERSION_MINOR 0)  

/# configure a header file to pass some of the CMake settings  
/# to the source code  
configure_file(  
  "${PROJECT_SOURCE_DIR}/TutorialConfig.h.in"  
  "${PROJECT_BINARY_DIR}/TutorialConfig.h"  
  )  

/# add the executable  
add_executable(Tutorial tutorial.cxx)  

/# add the binary tree to the search path for include files  
/# so that we will find TutorialConfig.h  
target_include_directories(Tutorial PUBLIC  
                           "${PROJECT_BINARY_DIR}"  
                           )  
![alt text](images/step2.png)
## step3
cmake_minimum_required(VERSION 3.3)  
project(Tutorial)  

set(CMAKE_CXX_STANDARD 11)  
set(CMAKE_CXX_STANDARD_REQUIRED True)  

/# should we use our own math functions  
option(USE_MYMATH "Use tutorial provided math implementation" ON)  

/# the version number.  
set(Tutorial_VERSION_MAJOR 1)  
set(Tutorial_VERSION_MINOR 0)  

/# configure a header file to pass some of the CMake settings  
/# to the source code  
configure_file(  
  "${PROJECT_SOURCE_DIR}/TutorialConfig.h.in"  
  "${PROJECT_BINARY_DIR}/TutorialConfig.h"  
  )  

/# add the MathFunctions library?  
if(USE_MYMATH)  
  add_subdirectory(MathFunctions)  
  list(APPEND EXTRA_LIBS MathFunctions)  
  list(APPEND EXTRA_INCLUDES "${PROJECT_SOURCE_DIR}/MathFunctions")  
endif(USE_MYMATH)  

/# add the executable  
add_executable(Tutorial tutorial.cxx)  

target_link_libraries(Tutorial ${EXTRA_LIBS})  

/# add the binary tree to the search path for include files  
/# so that we will find TutorialConfig.h  
target_include_directories(Tutorial PUBLIC  
                           "${PROJECT_BINARY_DIR}"  
                           ${EXTRA_INCLUDES}  
                           )  
![alt text](images/step3.png)
## step4  
cmake_minimum_required(VERSION 3.3)  
project(Tutorial)  
  
set(CMAKE_CXX_STANDARD 11)  
set(CMAKE_CXX_STANDARD_REQUIRED True)  

/# should we use our own math functions  
option(USE_MYMATH "Use tutorial provided math implementation" ON)  

/# the version number.  
set(Tutorial_VERSION_MAJOR 1)  
set(Tutorial_VERSION_MINOR 0)  

/# configure a header file to pass some of the CMake settings  
/# to the source code  
configure_file(  
  "${PROJECT_SOURCE_DIR}/TutorialConfig.h.in"  
  "${PROJECT_BINARY_DIR}/TutorialConfig.h"  
  )  

/# add the MathFunctions library?  
if(USE_MYMATH)  
  add_subdirectory(MathFunctions)  
  list(APPEND EXTRA_LIBS MathFunctions)  
endif(USE_MYMATH)  

/# add the executable  
add_executable(Tutorial tutorial.cxx)  

target_link_libraries(Tutorial PUBLIC ${EXTRA_LIBS})  

/# add the binary tree to the search path for include files  
/# so that we will find TutorialConfig.h  
target_include_directories(Tutorial PUBLIC  
                           "${PROJECT_BINARY_DIR}"  
                           )  
![alt text](images/step4.png)

## step5
cmake_minimum_required(VERSION 3.3)  
project(Tutorial)  

set(CMAKE_CXX_STANDARD 11)  
set(CMAKE_CXX_STANDARD_REQUIRED True)  

/# should we use our own math functions  
option(USE_MYMATH "Use tutorial provided math implementation" ON)  

/# the version number.  
set(Tutorial_VERSION_MAJOR 1)  
set(Tutorial_VERSION_MINOR 0)  

/# configure a header file to pass some of the CMake settings  
/# to the source code  
configure_file(  
  "${PROJECT_SOURCE_DIR}/TutorialConfig.h.in"  
  "${PROJECT_BINARY_DIR}/TutorialConfig.h"  
  )  

/# add the MathFunctions library?  
if(USE_MYMATH)  
  add_subdirectory(MathFunctions)  
  list(APPEND EXTRA_LIBS MathFunctions)  
endif()  

/# add the executable  
add_executable(Tutorial tutorial.cxx)  
target_link_libraries(Tutorial PUBLIC ${EXTRA_LIBS})  

/# add the binary tree to the search path for include files  
/# so that we will find TutorialConfig.h  
target_include_directories(Tutorial PUBLIC  
                           "${PROJECT_BINARY_DIR}"  
                           )  

/# add the install targets  
install(TARGETS Tutorial DESTINATION bin)  
install(FILES "${PROJECT_BINARY_DIR}/TutorialConfig.h"  
  DESTINATION include  
  )  

/# enable testing  
enable_testing()  

/# does the application run  
add_test(NAME Runs COMMAND Tutorial 25)  

/# does the usage message work?  
add_test(NAME Usage COMMAND Tutorial)  
set_tests_properties(Usage  
  PROPERTIES PASS_REGULAR_EXPRESSION "Usage:.*number"  
  )  

/# define a function to simplify adding tests  
function(do_test target arg result)  
  add_test(NAME Comp${arg} COMMAND ${target} ${arg})  
  set_tests_properties(Comp${arg}  
    PROPERTIES PASS_REGULAR_EXPRESSION ${result}  
    )  
endfunction(do_test)  

/# do a bunch of result based tests  
do_test(Tutorial 4 "4 is 2")  
do_test(Tutorial 9 "9 is 3")  
do_test(Tutorial 5 "5 is 2.236")  
do_test(Tutorial 7 "7 is 2.645")  
do_test(Tutorial 25 "25 is 5")  
do_test(Tutorial -25 "-25 is [-nan|nan|0]")  
do_test(Tutorial 0.0001 "0.0001 is 0.01")  
![alt text](images/step5.png)