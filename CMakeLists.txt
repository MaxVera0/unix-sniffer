cmake_minimum_required(VERSION 3.20)
project(ChafaSniffer VERSION 1.0 LANGUAGES CXX)

# Qt
find_package(Qt6 REQUIRED COMPONENTS Widgets)

set(CMAKE_AUTOMOC ON)
set(CMAKE_AUTOUIC ON)
set(CMAKE_AUTORCC ON)

# C++
set(CMAKE_CXX_STANDARD 17)
set(CMAKE_CXX_STANDARD_REQUIRED ON)

# 1. PRIMERO SE DEFINE EL EJECUTABLE
# Archivos fuente 
add_executable(chafa-sniffer
    src/main.cpp
    src/gui/mainwindow.cpp
    src/gui/mainwindow.h
    src/gui/mainwindow.ui
    src/gui/capturawindow.h src/gui/capturawindow.cpp src/gui/capturawindow.ui
)

# 2. LUEGO SE VINCULAN LAS LIBRERÍAS AL EJECUTABLE
# CONFIGURACIÓN MULTIPLATAFORMA (Windows vs Linux)
# ==========================================================
if(WIN32)
    # Configuración para Windows (Usando tu carpeta libs local)
    target_include_directories(chafa-sniffer PRIVATE "${CMAKE_SOURCE_DIR}/libs/npcap/Include")
    target_link_directories(chafa-sniffer PRIVATE "${CMAKE_SOURCE_DIR}/libs/npcap/Lib/x64")
    # Nota: Aquí agregamos ws2_32 porque Windows la necesita para los sockets
    target_link_libraries(chafa-sniffer PRIVATE wpcap Packet ws2_32 Qt6::Widgets)
    
elseif(UNIX)
    # Configuración para Linux (Uaabuntu usa las librerías nativas del sistema)
    target_link_libraries(chafa-sniffer PRIVATE pcap Qt6::Widgets)
endif()