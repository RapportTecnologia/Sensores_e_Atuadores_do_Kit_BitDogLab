# Sensores e Atuadores do KIT

Repositório raiz (monorepo) para projetos do KIT BitDogLab/EmbarcaTech, organizado com submódulos Git.

## Estrutura

- `BH1750-RP2040-RP3050/` — Driver/exemplos para o sensor de luminosidade BH1750 (RP2040/RP2350)
- `HDMI-RP2040-RP2350/` — Exemplos/infra para saída de vídeo (HDMI/LVDS) no RP2040/RP2350
- `I2C-RP2040-RP2350/` — Biblioteca I2C para RP2040/RP2350 (base para drivers)
- `MPU6050-RP2040-RP2350/` — Driver/exemplos para o sensor IMU MPU6050
- `OLED_SSD1306-RP2040-RP2350/` — Driver/exemplos para display OLED SSD1306 (I2C)
- `RP-Pico-Mock/` — Mocks do Pico SDK para testes (headers e stubs)
- `VL53L0X-RP2040-RP2350/` — Driver/exemplos para o sensor de distância VL53L0X
- `unity/` — Framework de testes unitários (submódulo externo ThrowTheSwitch/Unity)

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
git submodule update --remote --merge -- I2C-RP2040-RP2350
```

## Remotos e protocolos

- O arquivo `.gitmodules` usa uma combinação de HTTPS e SSH (git@) nos submódulos:
  - HTTPS: `I2C-RP2040-RP2350`, `VL53L0X-RP2040-RP2350`, `OLED_SSD1306-RP2040-RP2350`, `MPU6050-RP2040-RP2350`, `RP-Pico-Mock`, `unity`
  - SSH: `HDMI-RP2040-RP2350`, `BH1750-RP2040-RP3050`

- Você pode padronizar para SSH (útil para push) ou HTTPS (sem chaves), por exemplo:

```bash
# Para SSH (exemplos)
cd HDMI-RP2040-RP2350 && git remote set-url origin git@github.com:RapportTecnologia/HDMI-RP2040-RP2350.git && cd -
cd BH1750-RP2040-RP3050 && git remote set-url origin git@github.com:RapportTecnologia/BH1750-RP2040-RP3050.git && cd -

# Para HTTPS (exemplos)
cd I2C-RP2040-RP2350 && git remote set-url origin https://github.com/carlosdelfino/I2C_proxy_RP2040_RP2350.git && cd -
cd VL53L0X-RP2040-RP2350 && git remote set-url origin https://github.com/RapportTecnologia/VL53L0X-RP2040-RP2350.git && cd -
cd OLED_SSD1306-RP2040-RP2350 && git remote set-url origin https://github.com/RapportTecnologia/OLED_SSD1306-RP2040-RP2350.git && cd -
cd MPU6050-RP2040-RP2350 && git remote set-url origin https://github.com/RapportTecnologia/MPU6050-RP2040-RP2350.git && cd -
cd RP-Pico-Mock && git remote set-url origin https://github.com/RapportTecnologia/RP-Pico-Mock.git && cd -
cd unity && git remote set-url origin https://github.com/ThrowTheSwitch/Unity.git && cd -
```

### Submódulos e repositórios

- `BH1750-RP2040-RP3050/` → git@github.com:RapportTecnologia/BH1750-RP2040-RP3050.git
- `HDMI-RP2040-RP2350/` → git@github.com:RapportTecnologia/HDMI-RP2040-RP2350.git
- `I2C-RP2040-RP2350/` → https://github.com/carlosdelfino/I2C_proxy_RP2040_RP2350.git
- `MPU6050-RP2040-RP2350/` → https://github.com/RapportTecnologia/MPU6050-RP2040-RP2350.git
- `OLED_SSD1306-RP2040-RP2350/` → https://github.com/RapportTecnologia/OLED_SSD1306-RP2040-RP2350.git
- `RP-Pico-Mock/` → https://github.com/RapportTecnologia/RP-Pico-Mock.git
- `VL53L0X-RP2040-RP2350/` → https://github.com/RapportTecnologia/VL53L0X-RP2040-RP2350.git
- `unity/` → https://github.com/ThrowTheSwitch/Unity.git

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
