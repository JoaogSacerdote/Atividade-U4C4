# == DO NOT EDIT THE FOLLOWING LINES for the Raspberry Pi Pico VS Code Extension to work ==
if(WIN32)
    set(USERHOME $ENV{USERPROFILE})
else()
    set(USERHOME $ENV{HOME})
endif()
set(sdkVersion 2.1.0)
set(toolchainVersion 13_3_Rel1)
set(picotoolVersion 2.1.0)
set(picoVscode ${USERHOME}/.pico-sdk/cmake/pico-vscode.cmake)
if (EXISTS ${picoVscode})
    include(${picoVscode})
endif()
# ====================================================================================
cmake_minimum_required(VERSION 3.13)
set(CMAKE_C_STANDARD 11)
set(CMAKE_CXX_STANDARD 17)
set(CMAKE_EXPORT_COMPILE_COMMANDS ON)

# Inicializa o SDK do Raspberry Pi Pico
if(WIN32)
    set(USERHOME $ENV{USERPROFILE})
else()
    set(USERHOME $ENV{HOME})
endif()

# Versões do SDK e toolchain
set(sdkVersion 2.1.0)
set(toolchainVersion 13_3_Rel1)
set(picotoolVersion 2.1.0)
set(picoVscode ${USERHOME}/.pico-sdk/cmake/pico-vscode.cmake)

if (EXISTS ${picoVscode})
    include(${picoVscode})
endif()

# Configuração da placa
set(PICO_BOARD pico_w CACHE STRING "Board type")

# Importa o SDK do Raspberry Pi Pico
include(pico_sdk_import.cmake)

# Define o projeto
project(bitdoglab_project C CXX ASM)

# Inicializa o SDK
pico_sdk_init()

# Adiciona o executável
add_executable(bitdoglab_project main.c)

# Gera o cabeçalho PIO a partir do arquivo `.pio`
pico_generate_pio_header(bitdoglab_project ${CMAKE_CURRENT_SOURCE_DIR}/ws2812.pio)

# Define o nome e a versão do programa
pico_set_program_name(bitdoglab_project "bitdoglab_project")
pico_set_program_version(bitdoglab_project "1.0")

# Habilita stdio via UART e USB
pico_enable_stdio_uart(bitdoglab_project 1)
pico_enable_stdio_usb(bitdoglab_project 1)

# Linka as bibliotecas necessárias
target_link_libraries(bitdoglab_project
    pico_stdlib
    hardware_pio
)

# Adiciona os diretórios de inclusão
target_include_directories(bitdoglab_project PRIVATE
    ${CMAKE_CURRENT_LIST_DIR}
)

# Gera arquivos adicionais (como .uf2)
pico_add_extra_outputs(bitdoglab_project)
