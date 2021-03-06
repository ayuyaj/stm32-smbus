PROJECT(stm32-smbus)

CMAKE_MINIMUM_REQUIRED(VERSION 2.8)
ENABLE_LANGUAGE(ASM)
set(CMAKE_VERBOSE_MAKEFILE ON)
set(CMAKE_BUILD_TYPE DEBUG)

FIND_PACKAGE(CMSIS REQUIRED)
FIND_PACKAGE(StdPeriphLib REQUIRED)

SET(CMAKE_C_FLAGS "${CMAKE_C_FLAGS} -mfloat-abi=hard -Wall")

INCLUDE_DIRECTORIES(
    ${CMAKE_CURRENT_SOURCE_DIR}
    ${CMSIS_INCLUDE_DIR}
    ${StdPeriphLib_INCLUDE_DIR}
	)

SET(PROJECT_SOURCES
  system_stm32f4xx.c
  main.c
  rcc.c
  usart.c
  utils.c
  smbus.c
  mlx90614.c
)

SET(STM32_LINKER_SCRIPT ${CMSIS_LINKER_SCRIPT})

#ADD_DEFINITIONS("--allow-multiple-definition")
#SET( CMAKE_EXE_LINKER_FLAGS  "${CMAKE_EXE_LINKER_FLAGS} --allow-multiple-definition" )
ADD_EXECUTABLE(${CMAKE_PROJECT_NAME} ${PROJECT_SOURCES} ${CMSIS_STARTUP_SOURCE})
TARGET_LINK_LIBRARIES(${CMAKE_PROJECT_NAME} ${CMSIS_LIBRARIES} ${StdPeriphLib_LIBRARIES}  "gcc" "c" "-Wl,--allow-multiple-definition") #"-lc -lrdimon -u _printf_float")
STM32_SET_TARGET_PROPERTIES(${CMAKE_PROJECT_NAME})
STM32_ADD_HEX_BIN_TARGETS(${CMAKE_PROJECT_NAME})
