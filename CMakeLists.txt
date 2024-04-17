cmake_minimum_required(VERSION 3.16)
project(lychee-meeting LANGUAGES CXX)

set(CMAKE_CXX_STANDARD_REQUIRED ON)

add_definitions(-DRTC_DESKTOP_DEVICE)

find_package(Qt6 6.4 REQUIRED COMPONENTS Core Gui Qml Quick)

include_directories(
  "${CMAKE_CURRENT_SOURCE_DIR}/third_party/libwebrtc/include"
)

link_directories(
    "${CMAKE_CURRENT_SOURCE_DIR}/third_party/libwebrtc/lib/${CMAKE_SYSTEM_PROCESSOR}"
)

qt_standard_project_setup()
qt_add_executable(${PROJECT_NAME}
    main.cpp
)

target_link_libraries(${PROJECT_NAME} PRIVATE
    Qt::Core
    Qt::Gui
    Qt::Qml
    Qt::Quick
)

qt_add_resources(qml qml.qrc)
target_sources(${PROJECT_NAME} PRIVATE ${qml})

include(GNUInstallDirs)
install(TARGETS ${PROJECT_NAME}
    BUNDLE DESTINATION .
    LIBRARY DESTINATION ${CMAKE_INSTALL_LIBDIR}
    RUNTIME DESTINATION ${CMAKE_INSTALL_BINDIR}
)

set_property(
    TARGET ${PROJECT_NAME}
    PROPERTY BUILD_RPATH
    "\$ORIGIN"
)
