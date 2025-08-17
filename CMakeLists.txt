cmake_minimum_required(VERSION 3.16)

# Projeto "superbuild" para compilar todos os módulos e agregar documentação
project(EmbarcaTech_Sensores_e_Atuadores_do_KIT_BitDogLab C CXX)

# Opções para habilitar/desabilitar módulos
option(BUILD_BH1750 "Build BH1750 module" ON)
option(BUILD_HDMI "Build HDMI module" ON)
option(BUILD_I2C_PROXY "Build I2C proxy module" ON)
option(BUILD_MPU6050 "Build MPU6050 module" ON)
option(BUILD_OLED_SSD1306 "Build OLED SSD1306 module" ON)
option(BUILD_VL53L0X "Build VL53L0X module" ON)
option(BUILD_RP_PICO_MOCK "Build RP-Pico-Mock (stubs)" ON)

# Habilita a execução de testes unitários de forma unificada
option(ENABLE_UNIT_TESTS "Enable unit tests across modules" ON)
if(ENABLE_UNIT_TESTS)
  enable_testing()
  # Alvo conveniente para executar os testes: `cmake --build . --target check`
  add_custom_target(check
    COMMAND ${CMAKE_CTEST_COMMAND} --output-on-failure
    COMMENT "Executando testes unitários (ctest)"
  )
endif()

# Adiciona subdiretórios (cada um possui seu próprio project()/SDK)
if(NOT ENABLE_UNIT_TESTS)
  if(BUILD_BH1750)
    add_subdirectory(BH1750-RP2040-RP3050)
  endif()

  if(BUILD_HDMI)
    add_subdirectory(HDMI-RP2040-RP2350)
  endif()

  if(BUILD_I2C_PROXY)
    # Apenas adicione se existir CMakeLists.txt (este módulo pode não ter build separado)
    if(EXISTS "${CMAKE_SOURCE_DIR}/I2C-proxy-RP2040-RP2350/CMakeLists.txt")
      add_subdirectory(I2C-proxy-RP2040-RP2350)
    endif()
  endif()

  if(BUILD_MPU6050)
    add_subdirectory(MPU6050-RP2040-RP2350)
  endif()

  if(BUILD_OLED_SSD1306)
    add_subdirectory(OLED_SSD1306-RP2040-RP2350)
  endif()
endif()

if(BUILD_VL53L0X)
  # Quando estiver rodando testes unitários, força o módulo VL53L0X a compilar no modo HOST (BUILD_FOR_PICO=OFF),
  # o que habilita o executável de testes baseado em Unity/CTEST definido pelo próprio módulo.
  if(ENABLE_UNIT_TESTS)
    set(BUILD_FOR_PICO OFF CACHE BOOL "Build firmware for Pico" FORCE)
  endif()
  add_subdirectory(VL53L0X-RP2040-RP2350)
endif()

if(BUILD_RP_PICO_MOCK)
  if(ENABLE_UNIT_TESTS)
    message(STATUS "Skipping top-level RP-Pico-Mock; modules' test CMake will include it as needed")
  else()
    add_subdirectory(RP-Pico-Mock)
  endif()
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

# ---------- Placeholders de testes para módulos sem testes implementados ----------
if(ENABLE_UNIT_TESTS)
  # Para cada módulo sem suíte de testes ainda, adiciona um teste que emite um aviso e passa.
  # Isso permite uma execução unificada de `ctest` sem falhas artificiais.

  # BH1750
  if(BUILD_BH1750)
    add_test(NAME bh1750_missing_tests
             COMMAND ${CMAKE_COMMAND} -E echo "[WARNING] BH1750: testes unitários ainda não implementados.")
    set_tests_properties(bh1750_missing_tests PROPERTIES LABELS "missing-tests")
  endif()

  # HDMI
  if(BUILD_HDMI)
    add_test(NAME hdmi_missing_tests
             COMMAND ${CMAKE_COMMAND} -E echo "[WARNING] HDMI: testes unitários ainda não implementados.")
    set_tests_properties(hdmi_missing_tests PROPERTIES LABELS "missing-tests")
  endif()

  # I2C proxy
  if(BUILD_I2C_PROXY)
    add_test(NAME i2c_proxy_missing_tests
             COMMAND ${CMAKE_COMMAND} -E echo "[WARNING] I2C-proxy: testes unitários ainda não implementados.")
    set_tests_properties(i2c_proxy_missing_tests PROPERTIES LABELS "missing-tests")
  endif()

  # MPU6050
  if(BUILD_MPU6050)
    add_test(NAME mpu6050_missing_tests
             COMMAND ${CMAKE_COMMAND} -E echo "[WARNING] MPU6050: testes unitários ainda não implementados.")
    set_tests_properties(mpu6050_missing_tests PROPERTIES LABELS "missing-tests")
  endif()

  # OLED SSD1306
  if(BUILD_OLED_SSD1306)
    add_test(NAME oled_ssd1306_missing_tests
             COMMAND ${CMAKE_COMMAND} -E echo "[WARNING] OLED_SSD1306: testes unitários ainda não implementados.")
    set_tests_properties(oled_ssd1306_missing_tests PROPERTIES LABELS "missing-tests")
  endif()
endif()
