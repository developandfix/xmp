cmake_minimum_required(VERSION 3.28)
project("xmp" LANGUAGES C CXX)
set(CMAKE_CXX_STANDARD 11)
set(CMAKE_CXX_STANDARD_REQUIRED ON)

find_package(EXPAT REQUIRED)
find_package(ZLIB REQUIRED)

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
    list(APPEND XMP_DEFINITIONS "-DNOMINMAX=1")
endif()
if (BUILD_SHARED_LIBS)
    list(APPEND XMP_DEFINITIONS "-DXMP_StaticBuild=0")
else()
    list(APPEND XMP_DEFINITIONS "-DXMP_StaticBuild=1")
endif()

if (CMAKE_CXX_BYTE_ORDER EQUAL "BIG_ENDIAN")
    add_definitions(-DkBigEndianHost=1)
else()
    add_definitions(-DkBigEndianHost=0)
endif()

set(XMPROOT_DIR ${CMAKE_CURRENT_SOURCE_DIR})
set(XMP_INSTALL_INCLUDE_DIR include/xmp)

add_subdirectory(${CMAKE_CURRENT_SOURCE_DIR}/XMPCore)
add_subdirectory(${CMAKE_CURRENT_SOURCE_DIR}/XMPFiles)

target_compile_definitions(XMPCore PUBLIC ${XMP_DEFINITIONS})
target_compile_definitions(XMPFiles PUBLIC ${XMP_DEFINITIONS})

# Install public headers

file(GLOB PUBLIC_HEADERS "${XMPROOT_DIR}/public/include/*.h*")
file(GLOB PUBLIC_HEADERS_CPP "${XMPROOT_DIR}/public/include/*.incl_cpp")
file(GLOB PUBLIC_CLIENTGLUE_HEADERS "${XMPROOT_DIR}/public/include/client-glue/*.h*")
file(GLOB PUBLIC_CLIENTGLUE_HEADERS_CPP "${XMPROOT_DIR}/public/include/client-glue/*.incl_cpp") 
file(GLOB PLUGIN_API_HEADERS "${XMPROOT_DIR}/XMPFilesPlugins/api/source/*.h*")
file(GLOB CORE_HEADERS "${XMPROOT_DIR}/public/include/XMPCore/*.h*")
file(GLOB COMMON_HEADERS "${XMPROOT_DIR}/public/include/XMPCommon/*.h*")

install(FILES ${PUBLIC_HEADERS} ${PUBLIC_HEADERS_CPP} DESTINATION ${XMP_INSTALL_INCLUDE_DIR})
install(FILES ${PUBLIC_CLIENTGLUE_HEADERS} ${PUBLIC_CLIENTGLUE_HEADERS_CPP} DESTINATION ${XMP_INSTALL_INCLUDE_DIR}/client-glue)
install(FILES ${PLUGIN_API_HEADERS} DESTINATION ${XMP_INSTALL_INCLUDE_DIR}/plugins/api/source)
install(FILES ${CORE_HEADERS} DESTINATION ${XMP_INSTALL_INCLUDE_DIR}/XMPCore)
install(FILES ${COMMON_HEADERS} DESTINATION ${XMP_INSTALL_INCLUDE_DIR}/XMPCommon)

include(CMakePackageConfigHelpers)
include(GNUInstallDirs)

# Configure the package
configure_package_config_file(
    ${CMAKE_CURRENT_SOURCE_DIR}/Config.cmake.in
    ${CMAKE_CURRENT_BINARY_DIR}/XMPConfig.cmake
    PATH_VARS XMP_INSTALL_INCLUDE_DIR
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


