project(nx-systemtray-applet)


set(PROJECT_VERSION "1.0")
set(PROJECT_VERSION_MAJOR 1)

cmake_minimum_required(VERSION 2.8.12 FATAL_ERROR)

set(QT_MIN_VERSION "5.7.0")
set(KF5_MIN_VERSION "5.32.0")

find_package(Qt5 ${QT_MIN_VERSION} CONFIG REQUIRED COMPONENTS Gui Quick Test DBus)
find_package(ECM 1.8.0 REQUIRED NO_MODULE)
set(CMAKE_MODULE_PATH ${ECM_MODULE_PATH} ${ECM_KDE_MODULE_DIR})

include(KDEInstallDirs)
include(KDECMakeSettings)
include(KDECompilerSettings NO_POLICY_SCOPE)
include(ECMPackageConfigHelpers)
include(ECMMarkNonGuiExecutable)
include(CMakePackageConfigHelpers)
include(WriteBasicConfigVersionFile)
include(CheckIncludeFiles)
include(FeatureSummary)
include(ECMOptionalAddSubdirectory)
include(ECMQtDeclareLoggingCategory)
include(KDEPackageAppTemplates)

find_package(KF5 ${KF5_MIN_VERSION} REQUIRED COMPONENTS
	Plasma Declarative I18n IconThemes XmlGui KIO )
	
find_package(KF5Notifications)	

add_definitions(-DTRANSLATION_DOMAIN=\"plasma_applet_org.nx.private.systemtray\")

plasma_install_package(package org.nx.private.systemtray)

set(systemtray_SRCS
    systemtray.cpp
)

ecm_qt_declare_logging_category(systemtray_SRCS HEADER debug.h
                                            IDENTIFIER SYSTEM_TRAY
                                            CATEGORY_NAME nx.systemtray
                                            DEFAULT_SEVERITY Info)

add_library(org.nx.private.systemtray MODULE ${systemtray_SRCS})

kcoreaddons_desktop_to_json(org.nx.private.systemtray package/metadata.desktop)

target_link_libraries(org.nx.private.systemtray
                      Qt5::Gui
                      Qt5::Quick
                      KF5::Plasma
                      Qt5::DBus
                      KF5::IconThemes
                      KF5::XmlGui
                      KF5::I18n)

install(TARGETS org.nx.private.systemtray DESTINATION ${KDE_INSTALL_PLUGINDIR}/plasma/applets)

add_subdirectory(container)
add_subdirectory(tests)
