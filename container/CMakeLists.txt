
plasma_install_package(package org.nx.systemtray)

set(systemtraycontainer_SRCS
    systemtraycontainer.cpp
)

ecm_qt_declare_logging_category(systemtraycontainer_SRCS HEADER debug.h
                                            IDENTIFIER SYSTEM_TRAY_CONTAINER
                                            CATEGORY_NAME kde.systemtraycontainer
                                            DEFAULT_SEVERITY Info)

add_library(org.nx.systemtray MODULE ${systemtraycontainer_SRCS})

kcoreaddons_desktop_to_json(org.nx.systemtray package/metadata.desktop)

target_link_libraries(org.nx.systemtray
                      Qt5::Gui
                      Qt5::Quick
                      KF5::Plasma
                      KF5::XmlGui
                      KF5::I18n)


install(TARGETS org.nx.systemtray DESTINATION ${KDE_INSTALL_PLUGINDIR}/plasma/applets)


