cmake_minimum_required(VERSION 3.16)

# Projeto "superbuild" para compilar todos os módulos e agregar documentação
project(EmbarcaTechAll C CXX)

# Opções para habilitar/desabilitar módulos
option(BUILD_BH1750 "Build BH1750 module" ON)
option(BUILD_HDMI "Build HDMI module" ON)
option(BUILD_I2C_PROXY "Build I2C proxy module" ON)
option(BUILD_MPU6050 "Build MPU6050 module" ON)
option(BUILD_OLED_SSD1306 "Build OLED SSD1306 module" ON)
option(BUILD_VL53L0X "Build VL53L0X module" ON)
option(BUILD_RP_PICO_MOCK "Build RP-Pico-Mock (stubs)" ON)

# Adiciona subdiretórios (cada um possui seu próprio project()/SDK)
if(BUILD_BH1750)
  add_subdirectory(BH1750-RP2040-RP3050)
endif()

if(BUILD_HDMI)
  add_subdirectory(HDMI-RP2040-RP2350)
endif()

if(BUILD_I2C_PROXY)
  add_subdirectory(I2C-proxy-RP2040-RP2350)
endif()

if(BUILD_MPU6050)
  add_subdirectory(MPU6050-RP2040-RP2350)
endif()

if(BUILD_OLED_SSD1306)
  add_subdirectory(OLED_SSD1306-RP2040-RP2350)
endif()

if(BUILD_VL53L0X)
  add_subdirectory(VL53L0X-RP2040-RP2350)
endif()

if(BUILD_RP_PICO_MOCK)
  add_subdirectory(RP-Pico-Mock)
endif()

# ---------- Alvos de documentação por módulo (via Doxygen) ----------
# Requer doxygen instalado no sistema
find_program(DOXYGEN_EXECUTABLE doxygen)

function(add_doxygen_target target_name work_dir doxyfile)
  if(DOXYGEN_EXECUTABLE AND EXISTS "${work_dir}/${doxyfile}")
    add_custom_target(${target_name}
      COMMAND "${DOXYGEN_EXECUTABLE}" "${doxyfile}"
      WORKING_DIRECTORY "${work_dir}"
      COMMENT "Gerando documentação Doxygen em ${work_dir}"
      VERBATIM)
  endif()
endfunction()

# Criar alvos por módulo
add_doxygen_target(doxygen_root         "${CMAKE_SOURCE_DIR}"                          "Doxyfile")
add_doxygen_target(doxygen_bh1750       "${CMAKE_SOURCE_DIR}/BH1750-RP2040-RP3050"     "Doxyfile")
add_doxygen_target(doxygen_hdmi         "${CMAKE_SOURCE_DIR}/HDMI-RP2040-RP2350"       "Doxyfile")
add_doxygen_target(doxygen_i2c_proxy    "${CMAKE_SOURCE_DIR}/I2C-proxy-RP2040-RP2350"  "Doxyfile")
add_doxygen_target(doxygen_mpu6050      "${CMAKE_SOURCE_DIR}/MPU6050-RP2040-RP2350"    "Doxyfile")
add_doxygen_target(doxygen_oled_ssd1306 "${CMAKE_SOURCE_DIR}/OLED_SSD1306-RP2040-RP2350" "Doxyfile")
add_doxygen_target(doxygen_vl53l0x      "${CMAKE_SOURCE_DIR}/VL53L0X-RP2040-RP2350"    "Doxyfile")

# Alvo agregado que depende de todos os alvos de documentação existentes
add_custom_target(docs-all DEPENDS
  doxygen_root
  doxygen_bh1750
  doxygen_hdmi
  doxygen_i2c_proxy
  doxygen_mpu6050
  doxygen_oled_ssd1306
  doxygen_vl53l0x
)
