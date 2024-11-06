cmake_minimum_required(VERSION 3.28)
project("xmp" LANGUAGES C CXX)
set(CMAKE_CXX_STANDARD 11)
set(CMAKE_CXX_STANDARD_REQUIRED ON)


if (APPLE)
    if (APPLE_IOS)
        set(XMP_PLATFORM_SHORT "ios")
        list(APPEND XMP_DEFINITIONS "-DIOS_ENV=1")
    else ()
        set(XMP_PLATFORM_SHORT "mac")
        list(APPEND XMP_DEFINITIONS "-DMAC_ENV=1")
    endif()
elseif (ANDROID)
    set(XMP_PLATFORM_SHORT "android")
    list(APPEND XMP_DEFINITIONS "-DANDROID_ENV=1")
elseif (UNIX)
    set(XMP_PLATFORM_SHORT "linux")
    list(APPEND XMP_DEFINITIONS "-DUNIX_ENV=1")
elseif (WIN32)
    set(XMP_PLATFORM_SHORT "win")
    list(APPEND XMP_DEFINITIONS "-DWIN_ENV=1")
endif()
if (BUILD_SHARED_LIBS)
    list(APPEND XMP_DEFINITIONS "-DXMP_StaticBuild=0")
else()
    list(APPEND XMP_DEFINITIONS "-DXMP_StaticBuild=1")
endif()

set(XMPROOT_DIR ${CMAKE_CURRENT_SOURCE_DIR})

add_subdirectory(${CMAKE_CURRENT_SOURCE_DIR}/XMPCore)
add_subdirectory(${CMAKE_CURRENT_SOURCE_DIR}/XMPFiles)

target_compile_definitions(XMPCore PUBLIC ${XMP_DEFINITIONS})
target_compile_definitions(XMPFiles PUBLIC ${XMP_DEFINITIONS})

include(CMakePackageConfigHelpers)
include(GNUInstallDirs)

set(XMPCORE_INCLUDE_INSTALL_DIR ${CMAKE_INSTALL_INCLUDEDIR}/XmpCore)
set(XMPFILES_INCLUDE_INSTALL_DIR ${CMAKE_INSTALL_INCLUDEDIR}/XmpFiles)

message("XMPCORE_INCLUDE_INSTALL_DIR: ${XMPCORE_INCLUDE_INSTALL_DIR}")
message("XMPFILES_INCLUDE_INSTALL_DIR: ${XMPFILES_INCLUDE_INSTALL_DIR}")

# Configure the package
configure_package_config_file(
    ${CMAKE_CURRENT_SOURCE_DIR}/Config.cmake.in
    ${CMAKE_CURRENT_BINARY_DIR}/XMPConfig.cmake
    PATH_VARS XMPCORE_INCLUDE_INSTALL_DIR XMPFILES_INCLUDE_INSTALL_DIR
    INSTALL_DESTINATION ${CMAKE_INSTALL_LIBDIR}/cmake/XMP
)

# Generate version file
write_basic_package_version_file(
    ${CMAKE_CURRENT_BINARY_DIR}/XMPConfigVersion.cmake
    VERSION 1.0.0  # Replace with your actual version
    COMPATIBILITY SameMajorVersion
)

# Install the config files
install(FILES
    ${CMAKE_CURRENT_BINARY_DIR}/XMPConfig.cmake
    ${CMAKE_CURRENT_BINARY_DIR}/XMPConfigVersion.cmake
    DESTINATION ${CMAKE_INSTALL_LIBDIR}/cmake/XMP
)

# Add install
install(TARGETS XMPCore XMPFiles EXPORT XMPTargets DESTINATION lib)

# Install the targets
install(EXPORT XMPTargets
    FILE XMPTargets.cmake
    NAMESPACE XMP::
    DESTINATION ${CMAKE_INSTALL_LIBDIR}/cmake/XMP
)


