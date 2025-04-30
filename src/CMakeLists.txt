cmake_minimum_required(VERSION 3.10)

project(djview VERSION 4.12 LANGUAGES CXX)

if(DJVIEW_USE_QT6)
    set(CMAKE_CXX_STANDARD 17)
else()
    set(CMAKE_CXX_STANDARD 11)
endif()
set(CMAKE_CXX_STANDARD_REQUIRED ON)

set(CMAKE_AUTOMOC ON)
set(CMAKE_AUTORCC ON)
set(CMAKE_AUTOUIC ON)

option(DJVIEW_USE_QT6 "Use Qt6" OFF)

if(DJVIEW_USE_QT6)
    find_package(Qt6 REQUIRED COMPONENTS network opengl widgets printsupport Core5Compat LinguistTools)
else()
    find_package(Qt5 REQUIRED COMPONENTS network opengl widgets printsupport LinguistTools)
endif()

set(TS_FILES
    djview_fr.ts
    djview_uk.ts
    djview_de.ts
    djview_cs.ts
    djview_ru.ts
    djview_es.ts
    djview_zh_cn.ts
    djview_zh_tw.ts
    djview_pl.ts
    )
set_source_files_properties(${TS_FILES} PROPERTIES OUTPUT_LOCATION "${CMAKE_CURRENT_BINARY_DIR}/share/djvu/djview4")
if(DJVIEW_USE_QT6)
    qt_add_translation(QM_FILES ${TS_FILES})
else()
    qt5_add_translation(QM_FILES ${TS_FILES})
endif()
MESSAGE(STATUS "TS ${TS_FILES}")

add_executable(djview
    djview.rc
    qdjvu.h
    qdjvunet.h
    qdjvuwidget.h
    qdjvu.cpp
    qdjvunet.cpp
    qdjvuwidget.cpp
    qdjvuwidget.qrc
    qdjviewprefs.h
    qdjviewsidebar.h
    qdjviewdialogs.h
    qdjviewexporters.h
    qdjview.h
    djview.h
    version.h
    qdjviewprefs.cpp
    qdjviewsidebar.cpp
    qdjviewdialogs.cpp
    qdjviewexporters.cpp
    qdjview.cpp
    djview.cpp
    qdjview.qrc
    qdjviewerrordialog.ui
    qdjviewauthdialog.ui
    qdjviewinfodialog.ui
    qdjviewmetadialog.ui
    qdjviewsavedialog.ui
    qdjviewprintdialog.ui
    qdjviewexportdialog.ui
    qdjviewexportps1.ui
    qdjviewexportps2.ui
    qdjviewexportps3.ui
    qdjviewexporttiff.ui
    qdjviewexportprn.ui
    qdjviewprefsdialog.ui
    tiff2pdf.h
    tiff2pdf.c
    ${QM_FILES}
)

MESSAGE(STATUS "QM ${QM_FILES}")

if(MSVC)
    set_target_properties(djview PROPERTIES WIN32_EXECUTABLE TRUE)
endif(MSVC)

target_include_directories(djview PUBLIC
                           "${CMAKE_CURRENT_SOURCE_DIR}"
                           "${DJVULIBRE_INCLUDE}"
                           )

target_link_directories(djview PUBLIC
                        "${DJVULIBRE_LIBRARY}"
                       )

if(DJVIEW_USE_QT6)
    target_link_libraries(djview Qt6::Gui Qt6::Network Qt6::Widgets Qt6::PrintSupport Qt6::Core5Compat libdjvulibre)
else()
    target_link_libraries(djview Qt5::Gui Qt5::Network Qt5::Widgets Qt5::PrintSupport libdjvulibre)
endif()

