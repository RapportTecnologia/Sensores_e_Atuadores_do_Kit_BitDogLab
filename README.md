# Sensores e Atuadores do KIT

![visitors](https://visitor-badge.laobi.icu/badge?page_id=ArvoreDosSaberes.Sensores_e_Atuadores_do_KIT)
[![Build](https://img.shields.io/github/actions/workflow/status/ArvoreDosSaberes/Sensores_e_Atuadores_do_KIT/ci.yml?branch=main)](https://github.com/ArvoreDosSaberes/Sensores_e_Atuadores_do_KIT/actions)
[![Issues](https://img.shields.io/github/issues/ArvoreDosSaberes/Sensores_e_Atuadores_do_KIT)](https://github.com/ArvoreDosSaberes/Sensores_e_Atuadores_do_KIT/issues)
[![Stars](https://img.shields.io/github/stars/ArvoreDosSaberes/Sensores_e_Atuadores_do_KIT)](https://github.com/ArvoreDosSaberes/Sensores_e_Atuadores_do_KIT/stargazers)
[![Forks](https://img.shields.io/github/forks/ArvoreDosSaberes/Sensores_e_Atuadores_do_KIT)](https://github.com/ArvoreDosSaberes/Sensores_e_Atuadores_do_KIT/network/members)
[![Language](https://img.shields.io/badge/Language-C%2FC%2B%2B-brightgreen.svg)]()
[![License: CC BY 4.0](https://img.shields.io/badge/license-CC%20BY%204.0-blue.svg)](https://creativecommons.org/licenses/by/4.0/)


Repositório raiz (monorepo) para projetos do KIT BitDogLab/EmbarcaTech, organizado com submódulos Git.

## Estrutura

- `BH1750-RP2040-RP3050/` — Driver/exemplos para o sensor de luminosidade BH1750 (RP2040/RP2350)
- `HDMI-RP2040-RP2350/` — Exemplos/infra para saída de vídeo (HDMI/LVDS) no RP2040/RP2350
- `I2C-proxy-RP2040-RP2350/` — Biblioteca/proxy I2C para RP2040/RP2350 (base para drivers)
- `MPU6050-RP2040-RP2350/` — Driver/exemplos para o sensor IMU MPU6050
- `OLED_SSD1306-RP2040-RP2350/` — Driver/exemplos para display OLED SSD1306 (I2C)
- `RP-Pico-Mock/` — Mocks do Pico SDK para testes (headers e stubs)
- `VL53L0X-RP2040-RP2350/` — Driver/exemplos para o sensor de distância VL53L0X
- `unity/` — Framework de testes unitários (submódulo externo ThrowTheSwitch/Unity)
- `no-OS-FatFS-SD-SPI-RPi-Pico/` — Integração FatFS sobre SPI para cartão SD no Raspberry Pi Pico

## Clonagem

Para clonar com submódulos (recomendado):

```bash
# Clone já trazendo submódulos
git clone --recurse-submodules https://github.com/RapportTecnologia/Sensores_e_Atuadores_do_Kit_BitDogLab.git Sensores_e_Atuadores_do_KIT
cd Sensores_e_Atuadores_do_KIT
```

Caso já tenha clonado sem submódulos:

```bash
git submodule update --init --recursive
```

### Atualizar submódulos (buscar últimas revisões do upstream)

```bash
# Atualiza todos os submódulos para as revisões indicadas no commit atual
git submodule update --recursive --init

# Seguir branch remota dos submódulos e trazer últimas revisões (quando configurado)
git submodule update --remote --merge --recursive

# Alternativamente, para um submódulo específico
git submodule update --remote --merge -- I2C-proxy-RP2040-RP2350
```

## Remotos e protocolos

- O arquivo `.gitmodules` atualmente está configurado assim:
  - SSH (git@): `I2C-proxy-RP2040-RP2350`
  - HTTPS: `BH1750-RP2040-RP3050`, `HDMI-RP2040-RP2350`, `MPU6050-RP2040-RP2350`, `OLED_SSD1306-RP2040-RP2350`, `RP-Pico-Mock`, `VL53L0X-RP2040-RP2350`, `unity`, `no-OS-FatFS-SD-SPI-RPi-Pico`

- Você pode padronizar para SSH (útil para push) ou HTTPS (sem chaves), por exemplo:

```bash
# Para SSH (exemplos)
cd I2C-proxy-RP2040-RP2350 && git remote set-url origin git@github.com:RapportTecnologia/I2C-proxy-RP2040-RP2350.git && cd -

# Para HTTPS (exemplos)
cd BH1750-RP2040-RP3050 && git remote set-url origin https://github.com/RapportTecnologia/BH1750-RP2040-RP3050.git && cd -
cd HDMI-RP2040-RP2350 && git remote set-url origin https://github.com/RapportTecnologia/HDMI-RP2040-RP2350.git && cd -
cd I2C-proxy-RP2040-RP2350 && git remote set-url origin https://github.com/RapportTecnologia/I2C-proxy-RP2040-RP2350.git && cd -
cd MPU6050-RP2040-RP2350 && git remote set-url origin https://github.com/RapportTecnologia/MPU6050-RP2040-RP2350.git && cd -
cd OLED_SSD1306-RP2040-RP2350 && git remote set-url origin https://github.com/RapportTecnologia/OLED_SSD1306-RP2040-RP2350.git && cd -
cd RP-Pico-Mock && git remote set-url origin https://github.com/RapportTecnologia/RP-Pico-Mock.git && cd -
cd VL53L0X-RP2040-RP2350 && git remote set-url origin https://github.com/RapportTecnologia/VL53L0X-RP2040-RP2350.git && cd -
cd no-OS-FatFS-SD-SPI-RPi-Pico && git remote set-url origin https://github.com/ArvoreDosSaberes/no-OS-FatFS-SD-SPI-RPi-Pico.git && cd -
cd unity && git remote set-url origin https://github.com/ThrowTheSwitch/Unity.git && cd -
```

### Submódulos e repositórios

- `BH1750-RP2040-RP3050/` → https://github.com/RapportTecnologia/BH1750-RP2040-RP3050.git
- `HDMI-RP2040-RP2350/` → https://github.com/RapportTecnologia/HDMI-RP2040-RP2350.git
- `I2C-proxy-RP2040-RP2350/` → git@github.com:RapportTecnologia/I2C-proxy-RP2040-RP2350.git
- `MPU6050-RP2040-RP2350/` → https://github.com/RapportTecnologia/MPU6050-RP2040-RP2350.git
- `OLED_SSD1306-RP2040-RP2350/` → https://github.com/RapportTecnologia/OLED_SSD1306-RP2040-RP2350.git
- `RP-Pico-Mock/` → https://github.com/RapportTecnologia/RP-Pico-Mock.git
- `VL53L0X-RP2040-RP2350/` → https://github.com/RapportTecnologia/VL53L0X-RP2040-RP2350.git
- `unity/` → https://github.com/ThrowTheSwitch/Unity.git
- `no-OS-FatFS-SD-SPI-RPi-Pico/` → https://github.com/ArvoreDosSaberes/no-OS-FatFS-SD-SPI-RPi-Pico.git

Sincronizar URLs de submódulos com `.gitmodules` (após alterar `.gitmodules` ou clonar):

```bash
git submodule sync --recursive
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

## Documentação (Doxygen)

Este repositório possui um `Doxyfile` na raiz que agrega a documentação de todos os submódulos.

Pré-requisitos (Linux):
- Doxygen 1.9.x
- Graphviz (opcional, para gráficos): `dot`/`plantuml` se habilitado

Instalação (Ubuntu/Debian):
```bash
sudo apt-get update
sudo apt-get install -y doxygen graphviz
```

Gerar documentação consolidada:
```bash
doxygen Doxyfile
```

Saída:
- HTML em `docs/html/index.html`
- Log de avisos em `docs/doxygen_warnings.log`

Limpar/regerar:
```bash
rm -rf docs/html
doxygen Doxyfile
```

Observações:
- Diagramas (Graphviz) estão desabilitados por padrão no `Doxyfile` (`HAVE_DOT=NO`). Se desejar, habilite e instale o Graphviz.
- `INPUT` aponta para todos os submódulos e `EXCLUDE` evita pastas `build/`, `.git/`, `docs/html/`, etc.

## Integração de históricos

Este repositório pode ter sido mesclado com outros repositórios com históricos não relacionados (ex.: `Sensores_e_Atuadores_do_Kit_BitDogLab`). Quando necessário, a mesclagem foi feita com `--allow-unrelated-histories` para preservar histórico.
