
cmake_minimum_required(VERSION 3.0)
enable_language(ASM)

set(src 
  STM32F4xx_DSP_StdPeriph_Lib_V1.8.0/Libraries/CMSIS/Device/ST/STM32F4xx/Source/Templates/gcc_ride7/startup_stm32f40_41xxx.s
  STM32F4xx_DSP_StdPeriph_Lib_V1.8.0/Libraries/CMSIS/Device/ST/STM32F4xx/Source/Templates/system_stm32f4xx.c
  src/main.c)

add_executable(example.elf ${src})
target_include_directories( example.elf PRIVATE src/ )

add_subdirectory(STM32F4xx_DSP_StdPeriph_Lib_V1.8.0/)
target_link_libraries( example.elf stm32f4 )

file(COPY 
  ${CMAKE_CURRENT_SOURCE_DIR}/STM32F4xx_DSP_StdPeriph_Lib_V1.8.0/Project/STM32F4xx_StdPeriph_Templates/TrueSTUDIO/STM32F40_41xxx/STM32F417IG_FLASH.ld
  DESTINATION
  ${CMAKE_BINARY_DIR})

set_target_properties(
  example.elf 
  PROPERTIES 
  LINK_FLAGS 
  "-TSTM32F417IG_FLASH.ld \
   -mthumb \
   -mcpu=cortex-m4 \
   -mfloat-abi=hard \
   -mfpu=fpv4-sp-d16 \
   -Wl,--gc-sections")
