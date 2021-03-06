project(codelite-lldb)
message("-- Processing target codelite-lldb")

if ( APPLE )
    ## Under Apple, we only support monolithic build of wxWidgets
    find_package(wxWidgets COMPONENTS std REQUIRED)
else ( APPLE )
    find_package(wxWidgets COMPONENTS std aui propgrid stc richtext ribbon REQUIRED)
endif ( APPLE )

FILE(GLOB SRC "*.cpp")
include_directories(${CL_SRC_ROOT}/LLDBDebugger)

# codelite-lldb
add_executable(codelite-lldb ${SRC})
link_directories(${LLDB_LIB_PATH})
target_link_libraries(codelite-lldb ${LIBLLDB})
target_link_libraries(  codelite-lldb
                        ${LINKER_OPTIONS}
                        ${wxWidgets_LIBRARIES}
                        LLDBProtocol
                        libcodelite
                        plugin
                    )

CL_INSTALL_EXECUTABLE(codelite-lldb)
if ( MAKE_DEB AND LLDB_OFFICIAL_FOUND MATCHES 1 )
    message("-- Adding lldb-3.4-dev to deb dependencies")
    set(CPACK_DEBIAN_PACKAGE_DEPENDS  "${CPACK_DEBIAN_PACKAGE_DEPENDS} lldb-3.4-dev")
endif()
