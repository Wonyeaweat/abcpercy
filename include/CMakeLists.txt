add_library(percy INTERFACE)

target_include_directories(percy INTERFACE ${PROJECT_SOURCE_DIR}/include)
# Make sure percy users also see the vendored abc/abcorig and syrup headers
target_include_directories(percy INTERFACE ${PROJECT_SOURCE_DIR}/abcorig/src)
target_include_directories(percy INTERFACE ${PROJECT_SOURCE_DIR}/zinterface/abcorig/src)
target_include_directories(percy INTERFACE ${PROJECT_SOURCE_DIR}/abc)
target_include_directories(percy INTERFACE ${PROJECT_SOURCE_DIR}/syrup)

#target_link_libraries(percy INTERFACE include fileprc)

if (${PERCY_BUILD_CMS})
    target_include_directories(percy INTERFACE ${CMS_INCLUDE_DIR})
    add_dependencies(percy libcryptominisat5)
endif()

if (${PERCY_SATOKO})
    target_compile_definitions(percy INTERFACE USE_SATOKO)
    target_link_libraries(percy INTERFACE satoko)
else()
    target_compile_definitions(percy INTERFACE DISABLE_SATOKO)
endif()

# ABC uses a namespace macro; define it and other compile-time flags so consumers
# of the percy target (zinterface executables) get consistent macros.
target_compile_definitions(percy INTERFACE ABC_NAMESPACE=pabc ABC_NO_USE_READLINE LIN64)

target_link_libraries(percy INTERFACE kitty libabcsat)

if (${DISABLE_NAUTY})
    target_compile_definitions(percy INTERFACE DISABLE_NAUTY)
else()
    target_link_libraries(percy INTERFACE nauty)
endif()
if (UNIX)
    if (${PERCY_USE_SYRUP})
        target_compile_definitions(percy INTERFACE USE_SYRUP)
        target_link_libraries(percy INTERFACE syrup pthread)
    elseif(${PERCY_USE_GLUCOSE})
        target_compile_definitions(percy INTERFACE USE_GLUCOSE)
        target_link_libraries(percy INTERFACE glucose)
    endif()
endif()
