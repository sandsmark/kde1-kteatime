cmake_minimum_required (VERSION 3.0)

project (kteatime)

set(CMAKE_INCLUDE_CURRENT_DIR ON)
set(CMAKE_BUILD_WITH_INSTALL_RPATH ON)

find_package(KDE1 REQUIRED)
find_package(X11 REQUIRED)

include_directories (
    ${KDE1_INCLUDE_DIR}
)


include(Qt1Macros)
include(KDE1Macros)
include(KDE1InstallDirs)

option(ENABLE_SANITIZERS "Enable runtime sanitizing (for development)")
if (ENABLE_SANITIZERS)
    message("Enabling asan and ubsan")
    set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -fsanitize=address")
    set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -fsanitize=undefined")
endif()

if (CMAKE_COMPILER_IS_GNUCXX)
    set(CMAKE_C_FLAGS "${CMAKE_C_FLAGS} -Wno-write-strings")
    set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -fpermissive -std=c++98 -Wno-write-strings")
    set(CMAKE_SHARED_LINKER_FLAGS "${CMAKE_SHARED_LINKER_FLAGS} -Wl,--no-undefined")
endif()

qt1_wrap_cpp(MOC_FILES
    toplevel.h
)

add_executable(kteatime1
    main.cpp toplevel.cpp

    ${MOC_FILES}
)
target_link_libraries(kteatime1
    ${KDE1_KDECORE}
    ${KDE1_KDEUI}
    Qt::Qt1
    X11::X11
)

install(TARGETS kteatime1 RUNTIME DESTINATION ${KDE1_BINDIR})

install_icon(kteatime.xpm)
install(FILES kteatime.kdelnk DESTINATION ${KDE1_APPSDIR}/Utilities/)

