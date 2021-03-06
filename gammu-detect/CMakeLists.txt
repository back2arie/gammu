project (Gammu-Detect C)

CONFIGURE_FILE(${CMAKE_CURRENT_SOURCE_DIR}/config.h.in ${CMAKE_CURRENT_BINARY_DIR}/config.h)

if (GLIB_FOUND AND GOBJECT_FOUND)
    set(DETECT_SRC "main.c")

    if (GUDEV_FOUND)
        list(APPEND DETECT_SRC "udev.c")
    endif (GUDEV_FOUND)
    if (BLUEZ_FOUND)
        list(APPEND DETECT_SRC "bluez.c")
    endif (BLUEZ_FOUND)
    if (WIN32)
        list(APPEND DETECT_SRC "win32-serial.c")
    endif (WIN32)

    add_executable(gammu-detect ${DETECT_SRC})
    target_link_libraries (gammu-detect ${GLIB_LIBRARIES} ${GOBJECT_LIBRARIES} libGammu)
    include_directories(${GLIB_INCLUDE_DIRS} ${GOBJECT_INCLUDE_DIRS})
    if (GUDEV_FOUND)
        target_link_libraries (gammu-detect ${GUDEV_LIBRARIES})
        include_directories(${GUDEV_INCLUDE_DIRS})
    endif (GUDEV_FOUND)
    if (BLUEZ_FOUND)
        target_link_libraries (gammu-detect ${BLUEZ_LIBRARIES})
        include_directories (${BLUEZ_INCLUDE_DIR})
    endif (BLUEZ_FOUND)
    if (LIBINTL_LIB_FOUND AND LIBINTL_LIBRARIES)
        target_link_libraries (gammu-detect ${LIBINTL_LIBRARIES})
        include_directories (${LIBINTL_INCLUDE_DIR})
    endif (LIBINTL_LIB_FOUND AND LIBINTL_LIBRARIES)
    if (WIN32)
        target_link_libraries (gammu-detect string)
    endif (WIN32)

    include_directories (
        "${CMAKE_CURRENT_BINARY_DIR}/../include"
        "${CMAKE_CURRENT_BINARY_DIR}"
        )
    install (TARGETS gammu-detect
            RUNTIME DESTINATION "${INSTALL_BIN_DIR}"
            COMPONENT gammu
            )

endif (GLIB_FOUND AND GOBJECT_FOUND)

