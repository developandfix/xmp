# ==============================================================================
# This project
set(TARGET_NAME XMPCore)

add_definitions(-DBUILDING_XMPCORE_LIB=1 -DXMP_COMPONENT_INT_NAMESPACE=AdobeXMPCore_Int)

set(SOURCE_ROOT ${CMAKE_CURRENT_SOURCE_DIR}/source)
set(RESOURCE_ROOT ${CMAKE_CURRENT_SOURCE_DIR}/resource)


list (APPEND INTERNAL_HEADER_FILES
		${XMPROOT_DIR}/source/ExpatAdapter.hpp
		${XMPROOT_DIR}/source/UnicodeConversions.hpp
		${XMPROOT_DIR}/source/UnicodeInlines.incl_cpp
		${XMPROOT_DIR}/source/XMLParserAdapter.hpp
		${XMPROOT_DIR}/build/XMP_BuildInfo.h
		${SOURCE_ROOT}/XMPCore_Impl.hpp
		${SOURCE_ROOT}/XMPIterator.hpp
		${SOURCE_ROOT}/XMPMeta.hpp
		${SOURCE_ROOT}/XMPUtils.hpp
)

file (GLOB_RECURSE PRIVATE_XMPCOMMON_HEADERS ${XMPROOT_DIR}/XMPCommon/*.h)

file (GLOB_RECURSE PRIVATE_XMPCOMMON_SOURCES ${XMPROOT_DIR}/XMPCommon/source/*.cpp)

file (GLOB PRIVATE_XMPCORE_HEADERS
		${XMPROOT_DIR}/XMPCore/*.h
		${XMPROOT_DIR}/XMPCore/ImplHeaders/*.h
		${XMPROOT_DIR}/XMPCore/headers/*.h
		${XMPROOT_DIR}/XMPCore/Interfaces/*.h )

file (GLOB_RECURSE PUBLIC_CLIENTGLUE_HEADER_FILES ${XMPROOT_DIR}/public/include/client-glue/*.*)
list (REMOVE_ITEM PUBLIC_CLIENTGLUE_HEADER_FILES
		${XMPROOT_DIR}/public/include/client-glue/TXMPFiles.incl_cpp
		${XMPROOT_DIR}/public/include/client-glue/WXMPFiles.hpp
)

file (GLOB PUBLIC_HEADER_FILES ${XMPROOT_DIR}/public/include/*.*)
list (REMOVE_ITEM PUBLIC_HEADER_FILES
		${XMPROOT_DIR}/public/include/TXMPFiles.hpp
		${XMPROOT_DIR}/public/include/XMP_IO.hpp
)

file (GLOB_RECURSE PUBLIC_XMPCOMMON_HEADERS ${XMPROOT_DIR}/public/include/XMPCommon/*.h )

file (GLOB_RECURSE PUBLIC_XMPCOMMON_SOURCES ${XMPROOT_DIR}/public/include/XMPCommon/source/*.cpp )

file (GLOB_RECURSE PUBLIC_XMPCORE_HEADERS ${XMPROOT_DIR}/public/include/XMPCore/*.h)

file (GLOB_RECURSE PUBLIC_XMPCORE_SOURCES ${XMPROOT_DIR}/public/include/XMPCore/source/*.cpp )

file (GLOB ABIWRAPPER_SOURCE_FILES ${SOURCE_ROOT}/WXMP*.cpp)

file (GLOB PRIVATE_XMPCORE_ISOURCE_FILES ${SOURCE_ROOT}/*_I.cpp)
file (GLOB PRIVATE_XMPCORE_IMPLSOURCE_FILES ${SOURCE_ROOT}/*Impl.cpp)
list (REMOVE_ITEM PRIVATE_XMPCORE_IMPLSOURCE_FILES ${SOURCE_ROOT}/XMPCore_Impl.cpp)

#XMP Core classes
file (GLOB CORECLASS_SOURCE_FILES ${SOURCE_ROOT}/XMP*.cpp)
list (REMOVE_ITEM CORECLASS_SOURCE_FILES ${SOURCE_ROOT}/XMPCore_Impl.cpp)

set(TP_ZUID_PATH "${XMPROOT_DIR}/third-party/zuid/interfaces")
file (GLOB ZUID_SOURCE_FILES ${TP_ZUID_PATH}/*.cpp)

list (APPEND THIRDPARTY_SOURCE_FILES
		${ZUID_SOURCE_FILES}
)

list (APPEND UTILITY_SOURCE_FILES
		${SOURCE_ROOT}/ExpatAdapter.cpp
		${SOURCE_ROOT}/ParseRDF.cpp
		${SOURCE_ROOT}/XMPCore_Impl.cpp
		${XMPROOT_DIR}/source/UnicodeConversions.cpp
		${XMPROOT_DIR}/source/XML_Node.cpp
		${XMPROOT_DIR}/source/XMP_LibUtils.cpp
)

#resource files
if (WIN32)
	file (GLOB RESOURCE_FILES ${RESOURCE_ROOT}/win/*.*)
else ()
	file (GLOB RESOURCE_FILES ${RESOURCE_ROOT}/${XMP_PLATFORM_SHORT}/*.*)
endif()
if(WIN32 AND NOT BUILD_SHARED_LIBS)
	list(REMOVE_ITEM RESOURCE_FILES ${RESOURCE_ROOT}/${XMP_PLATFORM_SHORT}/${TARGET_NAME}.rc)
endif()

list(APPEND SOURCE_FILES
		${PUBLIC_CLIENTGLUE_HEADER_FILES}
		${PUBLIC_HEADER_FILES}
		${INTERNAL_HEADER_FILES}
		${ABIWRAPPER_SOURCE_FILES}
		${ABIWRAPPER_SOURCE_FILES}
		${CORECLASS_SOURCE_FILES}
		${THIRDPARTY_SOURCE_FILES}
		${UTILITY_SOURCE_FILES}
		${PRIVATE_XMPCORE_ISOURCE_FILES}
		${PRIVATE_XMPCORE_IMPLSOURCE_FILES}
		${PRIVATE_XMPCOMMON_HEADERS}
		${PRIVATE_XMPCOMMON_SOURCES}
		${PRIVATE_XMPCORE_HEADERS}
		${PUBLIC_XMPCOMMON_HEADERS}
		${PUBLIC_XMPCOMMON_SOURCES}
		${PUBLIC_XMPCORE_HEADERS}
		${PUBLIC_XMPCORE_SOURCES}
		${ANDROID_UTILS}
		${RESOURCE_FILES}
)

add_library(XMPCore ${SOURCE_FILES})

target_include_directories(XMPCore PUBLIC
		${XMPROOT_DIR}
		${XMPROOT_DIR}/public/include
		${XMPROOT_DIR}/third-party/expat/public/lib
		${RESOURCE_ROOT}/${XMP_PLATFORM_SHORT})
if (WIN32)
	target_include_directories(XMPCore PUBLIC ${RESOURCE_ROOT}/win)
else ()
	target_include_directories(XMPCore PUBLIC ${RESOURCE_ROOT}/${XMP_PLATFORM_SHORT})
endif ()

set(XMPCoreLibraries expat)
if (APPLE)
	target_link_libraries(XMPCore PRIVATE ${XMPCoreLibraries} "-framework CoreFoundation -framework CoreServices")
elseif (WIN32)
	target_link_libraries(XMPCore PRIVATE ${XMPCoreLibraries} rpcrt ole32)
else()
	target_link_libraries(XMPCore PRIVATE ${XMPCoreLibraries})
endif()