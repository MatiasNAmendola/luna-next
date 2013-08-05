include_directories(
    ${GLIB2_INCLUDE_DIRS}
    ${LUNA_SYSMGR_COMMON_INCLUDE_DIRS}
    ${CMAKE_CURRENT_SOURCE_DIR}
    ${CMAKE_CURRENT_BINARY_DIR}
    ${Qt5Gui_PRIVATE_INCLUDE_DIRS})

set(SOURCES
    lunanextplugin.cpp
    settingsadapter.cpp)

add_library(lunanext-qml SHARED ${SOURCES})
target_link_libraries(lunanext-qml ${LUNA_SYSMGR_COMMON_LIBRARIES})

qt5_use_modules(lunanext-qml Qml Quick)

execute_process(COMMAND ${CMAKE_COMMAND} -E copy_if_different "${CMAKE_CURRENT_SOURCE_DIR}/qmldir"
                                                              "${CMAKE_CURRENT_BINARY_DIR}/qmldir")

# FIXME Using ${OE_QMAKE_PATH_QML} isn't really portable but the only way atm
set(QMLPLUGIN_INSTALL_PREFIX "${OE_QMAKE_PATH_QML}/LunaNext")
install(TARGETS lunanext-qml DESTINATION ${QMLPLUGIN_INSTALL_PREFIX})
install(FILES qmldir DESTINATION ${QMLPLUGIN_INSTALL_PREFIX})