<<<<<<< HEAD
# Sensores e Atuadores do KIT

Repositório raiz (monorepo) para projetos do KIT BitDogLab/EmbarcaTech, organizado com submódulos Git.

## Estrutura

- `I2C/` — Biblioteca I2C para RP2040 (submódulo)
- `VL53L0X-RP2040-RP2350/` — Firmware do sensor VL53L0X para RP2040 (submódulo)

## Clonagem

Para clonar com submódulos (recomendado):

```bash
# Clone
git clone https://github.com/RapportTecnologia/Sensores_e_Atuadores_do_Kit_BitDogLab.git Sensores_e_Atuadores_do_KIT
cd Sensores_e_Atuadores_do_KIT

# Inicialize submódulos
git submodule update --init --recursive
```

Caso já tenha clonado sem submódulos:

```bash
git submodule update --init --recursive
```

## Remotos e protocolos

- Arquivo `.gitmodules` usa URLs HTTPS para compatibilidade geral
- Remotos internos (em cada submódulo) podem ser configurados com SSH (git@) para push:

```bash
cd I2C
git remote set-url origin git@github.com:carlosdelfino/I2C_proxy_RP2040_RP2350.git
cd ../VL53L0X-RP2040-RP2350
git remote set-url origin git@github.com:RapportTecnologia/VL53L0X-RP2040-RP2350.git
cd ..
```

## Build do projeto VL53L0X

Pré-requisitos:
- Pico SDK 2.1.x
- CMake 3.13+
- GCC ARM

Passos:
```bash
cd VL53L0X-RP2040-RP2350
mkdir -p build && cd build
cmake ..
cmake --build . --parallel
```

O UF2 será gerado em `build/`.

## Integração de históricos

Este repositório pode ter sido mesclado com outros repositórios com históricos não relacionados (ex.: `Sensores_e_Atuadores_do_Kit_BitDogLab`). Quando necessário, a mesclagem foi feita com `--allow-unrelated-histories` para preservar histórico.
=======
# Sensores_e_Atuadores_do_Kit_BitDogLab
Firmware para Sensores e Atuadores utilizado no kit básico para a BitDogLab, os códigos foram feitos de forma que possam ser portado para outros microcontroladores, mas neste repositório as informações são ajustadas para a BitDogLab
>>>>>>> bitdog/main
