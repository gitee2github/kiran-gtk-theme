

set (KIRAN_LIGHT_THEME_NAME "Kiran")
set (KIRAN_DARK_THEME_NAME "Kiran-dark")

set (KIRAN_THEME_ROOT_CSS ${PROJECT_BINARY_DIR}/Kiran/gtk-3.0/gtk.css)
set (KIRAN_DARK_THEME_ROOT_CSS ${PROJECT_BINARY_DIR}/Kiran-dark/gtk-3.0/gtk.css)

#add_custom_command(OUTPUT ${KIRAN_THEME_ROOT_CSS}
#                   COMMAND ${SASSC} -I ${PROJECT_SOURCE_DIR}//gtk-3.0)


function(gen_theme_files THEME_NAME)
    set(THEME_BUILD_DIR "${PROJECT_BINARY_DIR}/${THEME_NAME}")
    set(${THEME_NAME}_FILES
        "${THEME_BUILD_DIR}/gtk-2.0/gtkrc"
        "${THEME_BUILD_DIR}/gtk-3.0/gtk.css"
        PARENT_SCOPE
    )
endfunction()


gen_theme_files(${KIRAN_LIGHT_THEME_NAME})
gen_theme_files(${KIRAN_DARK_THEME_NAME})

# light theme
add_custom_command(
    OUTPUT ${KIRAN_LIGHT_THEME_NAME}_FILES
    COMMAND ./build-theme.sh -c light
                             -t ${KIRAN_LIGHT_THEME_NAME}
                             -k light
                             -d ${PROJECT_BINARY_DIR}/${KIRAN_LIGHT_THEME_NAME}
    WORKING_DIRECTORY ${CMAKE_CURRENT_SOURCE_DIR}
)
add_custom_target(${KIRAN_LIGHT_THEME_NAME}_TARGET ALL DEPENDS ${KIRAN_LIGHT_THEME_NAME}_FILES)

# dark theme
add_custom_command(
    OUTPUT ${KIRAN_DARK_THEME_NAME}_FILES
    COMMAND ./build-theme.sh -c dark
                             -t ${KIRAN_DARK_THEME_NAME}
                             -k dark
                             -d ${PROJECT_BINARY_DIR}/${KIRAN_DARK_THEME_NAME}
    WORKING_DIRECTORY ${CMAKE_CURRENT_SOURCE_DIR}
)
add_custom_target(${KIRAN_DARK_THEME_NAME}_TARGET ALL DEPENDS ${KIRAN_DARK_THEME_NAME}_FILES)

install(DIRECTORY ${PROJECT_BINARY_DIR}/${KIRAN_LIGHT_THEME_NAME}
        DESTINATION ${KGT_INSTALL_DATADIR}/themes)

install(DIRECTORY ${PROJECT_BINARY_DIR}/${KIRAN_DARK_THEME_NAME}
        DESTINATION ${KGT_INSTALL_DATADIR}/themes)