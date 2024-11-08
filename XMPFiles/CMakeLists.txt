# ==============================================================================
# This project
set(TARGET_NAME XMPFiles)

set(TP_ZUID_PATH "${XMPROOT_DIR}/third-party/zuid/interfaces")
set(LIB_ADOBEXMP XMPCore)

set(SOURCE_ROOT ${CMAKE_CURRENT_SOURCE_DIR}/source)
set(RESOURCE_ROOT ${CMAKE_CURRENT_SOURCE_DIR}/resource)

#file (GLOB INTERNAL_HEADER_FILES ${PRODUCT_ROOT}/source/*.hpp ${PRODUCT_ROOT}/source/*.incl_cpp ${PRODUCT_ROOT}/build/*.h)
file (GLOB INTERNAL_HEADER_COMMONCODE_NMDS ${PRODUCT_ROOT}/XMPFiles/source/NativeMetadataSupport/*.h)
source_group("Header Files\\Internal Headers\\Common Code\\NativeMetadataSupport" FILES ${INTERNAL_HEADER_COMMONCODE_NMDS})

list (APPEND INTERNAL_HEADER_COMMONCODE
		${XMPROOT_DIR}/source/Endian.h
		${XMPROOT_DIR}/source/SafeStringAPIs.h
		${XMPROOT_DIR}/source/SafeTypes.h
		${XMPROOT_DIR}/source/SuppressSAL.h
		${XMPROOT_DIR}/build/XMP_BuildInfo.h
		${SOURCE_ROOT}/XMPFiles.hpp
		${SOURCE_ROOT}/XMPFiles_Impl.hpp
)
source_group("Header Files\\Internal Headers\\Common Code" FILES ${INTERNAL_HEADER_COMMONCODE})

file (GLOB INTERNAL_HEADER_FILEHANDLERS ${SOURCE_ROOT}/FileHandlers/*.hpp)
list (REMOVE_ITEM INTERNAL_HEADER_FILEHANDLERS
		${SOURCE_ROOT}/FileHandlers/AIFF_Handler.hpp
)
source_group("Header Files\\Internal Headers\\File Handlers" FILES ${INTERNAL_HEADER_FILEHANDLERS})

file (GLOB INTERNAL_HEADER_FORMATSUPPORT_AIFF ${SOURCE_ROOT}/FormatSupport/AIFF/*.h)
source_group("Header Files\\Internal Headers\\Format Support\\AIFF" FILES ${INTERNAL_HEADER_FORMATSUPPORT_AIFF})

file (GLOB INTERNAL_HEADER_FORMATSUPPORT_IFF ${SOURCE_ROOT}/FormatSupport/IFF/*.h)
source_group("Header Files\\Internal Headers\\Format Support\\IFF" FILES ${INTERNAL_HEADER_FORMATSUPPORT_IFF})

file (GLOB INTERNAL_HEADER_FORMATSUPPORT_WAVE ${SOURCE_ROOT}/FormatSupport/WAVE/*.h)
source_group("Header Files\\Internal Headers\\Format Support\\WAVE" FILES ${INTERNAL_HEADER_FORMATSUPPORT_WAVE})

file (GLOB INTERNAL_HEADER_FORMATSUPPORT ${SOURCE_ROOT}/FormatSupport/*.hpp)

list (REMOVE_ITEM INTERNAL_HEADER_FORMATSUPPORT
		#	${SOURCE_ROOT}/source/MD5.cpp
		${XMPROOT_DIR}/source/UnicodeConversions.cpp
)
source_group("Header Files\\Internal Headers\\Format Support" FILES ${INTERNAL_HEADER_FORMATSUPPORT})

file (GLOB INTERNAL_HEADER_PLUGINHANDLER ${SOURCE_ROOT}/PluginHandler/*.h)
source_group("Header Files\\Internal Headers\\PluginHandler" FILES ${INTERNAL_HEADER_PLUGINHANDLER})

list (APPEND PUBLIC_HEADER_CLIENTGLUE
		${XMPROOT_DIR}/public/include/client-glue/TXMPFiles.incl_cpp
		${XMPROOT_DIR}/public/include/client-glue/WXMP_Common.hpp
		${XMPROOT_DIR}/public/include/client-glue/WXMPFiles.hpp
)
source_group("Header Files\\Public Headers\\Client Glue" FILES ${PUBLIC_HEADER_CLIENTGLUE})
list (APPEND PUBLIC_HEADER
		${XMPROOT_DIR}/public/include/TXMPFiles.hpp
		${XMPROOT_DIR}/public/include/TXMPIterator.hpp
		${XMPROOT_DIR}/public/include/TXMPMeta.hpp
		${XMPROOT_DIR}/public/include/TXMPUtils.hpp
		${XMPROOT_DIR}/public/include/XMP.hpp
		${XMPROOT_DIR}/public/include/XMP.incl_cpp
		${XMPROOT_DIR}/public/include/XMP_Const.h
		${XMPROOT_DIR}/public/include/XMP_Environment.h
		${XMPROOT_DIR}/public/include/XMP_IO.hpp
		${XMPROOT_DIR}/public/include/XMP_Version.h
)
source_group("Header Files\\Public Headers" FILES ${PUBLIC_HEADER})

file (GLOB HEADERFILES_THIRDPARTY_ZLIB ${XMPROOT_DIR}/third-party/zlib/*.h)
list (REMOVE_ITEM HEADERFILES_THIRDPARTY_ZLIB
		${CMAKE_CURRENT_SOURCE_DIR}/${XMPROOT_DIR}/third-party/zlib/gzguts.h
)
source_group("Header Files\\ThirdParty\\zlib" FILES ${HEADERFILES_THIRDPARTY_ZLIB})


list (APPEND HEADERFILES
		${XMPROOT_DIR}/source/Host_IO.hpp
		${XMPROOT_DIR}/source/XIO.hpp
		${XMPROOT_DIR}/source/IOUtils.hpp
		${XMPROOT_DIR}/source/XMPFiles_IO.hpp
)
source_group("Header Files" FILES ${HEADERFILES})

#resource files
file (GLOB RESOURCE_FILES ${RESOURCE_ROOT}/${XMP_PLATFORM_SHORT}/${TARGET_NAME}.*)
if(WIN32 AND NOT BUILD_SHARED_LIBS)
	list (REMOVE_ITEM RESOURCE_FILES ${RESOURCE_ROOT}/${XMP_PLATFORM_SHORT}/${TARGET_NAME}.rc)
endif()
source_group("Resource Files" FILES ${RESOURCE_FILES})

#source files
file (GLOB SOURCEFILES_COMMONCODE_NMDS ${SOURCE_ROOT}/NativeMetadataSupport/*.cpp)
source_group("Source Files\\Common Code\\NativeMetadataSupport" FILES ${SOURCEFILES_COMMONCODE_NMDS})

list (APPEND SOURCEFILES_COMMONCODE
		${TP_ZUID_PATH}/MD5.cpp
		${XMPROOT_DIR}/source/UnicodeConversions.cpp
		${SOURCE_ROOT}/HandlerRegistry.cpp
		${XMPROOT_DIR}/source/SafeStringAPIs.cpp
		${XMPROOT_DIR}/source/PerfUtils.cpp
		${SOURCE_ROOT}/WXMPFiles.cpp
		${XMPROOT_DIR}/source/XIO.cpp
		${XMPROOT_DIR}/source/IOUtils.cpp
		${XMPROOT_DIR}/source/XML_Node.cpp
		${XMPROOT_DIR}/source/XMP_LibUtils.cpp
		${XMPROOT_DIR}/source/XMP_ProgressTracker.cpp
		${SOURCE_ROOT}/XMPFiles.cpp
		${SOURCE_ROOT}/XMPFiles_Impl.cpp
		${XMPROOT_DIR}/source/XMPFiles_IO.cpp
)
if(UNIX)
	list(APPEND SOURCEFILES_COMMONCODE ${XMPROOT_DIR}/source/Host_IO-POSIX.cpp)
else()
	list(APPEND SOURCEFILES_COMMONCODE ${XMPROOT_DIR}/source/Host_IO-Win.cpp)
endif()
source_group("Source Files\\Common Code" FILES ${SOURCEFILES_COMMONCODE})

file (GLOB SOURCEFILES_FILEHANDLERS ${SOURCE_ROOT}/FileHandlers/*.cpp)
list (APPEND SOURCEFILES_FILEHANDLERS
		${SOURCE_ROOT}/FileHandlers/AIFF_Handler.hpp
)

source_group("Source Files\\File Handlers" FILES ${SOURCEFILES_FILEHANDLERS})

file (GLOB SOURCEFILES_FORMATSUPPORT_AIFF ${SOURCE_ROOT}/FormatSupport/AIFF/*.cpp)
source_group("Source Files\\Format Support\\AIFF" FILES ${SOURCEFILES_FORMATSUPPORT_AIFF})

file (GLOB SOURCEFILES_FORMATSUPPORT_IFF ${SOURCE_ROOT}/FormatSupport/IFF/*.cpp)
source_group("Source Files\\Format Support\\IFF" FILES ${SOURCEFILES_FORMATSUPPORT_IFF})

file (GLOB SOURCEFILES_FORMATSUPPORT_WAVE ${SOURCE_ROOT}/FormatSupport/WAVE/*.cpp)
source_group("Source Files\\Format Support\\WAVE" FILES ${SOURCEFILES_FORMATSUPPORT_WAVE})

file (GLOB SOURCEFILES_FORMATSUPPORT ${SOURCE_ROOT}/FormatSupport/*.cpp)

source_group("Source Files\\Format Support" FILES ${SOURCEFILES_FORMATSUPPORT})

if (NOT APPLE_IOS AND NOT CMAKE_WINUNIVERSAL )
	file (GLOB SOURCEFILES_PLUGINHANDLER ${SOURCE_ROOT}/PluginHandler/*.cpp)
	list (REMOVE_ITEM SOURCEFILES_PLUGINHANDLER
			${SOURCE_ROOT}/PluginHandler/OS_Utils_Linux.cpp
			${SOURCE_ROOT}/PluginHandler/OS_Utils_WIN.cpp
			${SOURCE_ROOT}/PluginHandler/OS_Utils_Mac.cpp
			${SOURCE_ROOT}/PluginHandler/OS_Utils_Android.cpp
	)
	if (UNIX)
		if (APPLE)
			list (APPEND SOURCEFILES_PLUGINHANDLER ${SOURCE_ROOT}/PluginHandler/OS_Utils_Mac.cpp)
		elseif(ANDROID)
			list (APPEND SOURCEFILES_PLUGINHANDLER ${SOURCE_ROOT}/PluginHandler/OS_Utils_ANDROID.cpp)
		else()
			list (APPEND SOURCEFILES_PLUGINHANDLER ${SOURCE_ROOT}/PluginHandler/OS_Utils_Linux.cpp)
		endif()
	else()
		list (APPEND SOURCEFILES_PLUGINHANDLER ${SOURCE_ROOT}/PluginHandler/OS_Utils_WIN.cpp)
	endif()

	source_group("Source Files\\PluginHandler" FILES ${SOURCEFILES_PLUGINHANDLER})
endif()

#Core & Common public source files
#if(NOT ANDROID)
if (INCLUDE_CPP_DOM_SOURCE)
	file (GLOB PUBLIC_XMPCOMMON_CLIENTGLUE_FILES ${XMPROOT_DIR}/public/include/XMPCommon/source/*.*)
	source_group("Source Files\\Public\\XMPCommon" FILES ${PUBLIC_XMPCOMMON_CLIENTGLUE_FILES})

	file (GLOB PUBLIC_XMPCORE_CLIENTGLUE_FILES ${XMPROOT_DIR}/public/include/XMPCore/source/*.*)
	source_group("Source Files\\Public\\XMPCore" FILES ${PUBLIC_XMPCORE_CLIENTGLUE_FILES})
endif()
#endif()
list(APPEND SOURCE_FILES
		${INTERNAL_HEADER_COMMONCODE_NMDS}
		${INTERNAL_HEADER_COMMONCODE}
		${INTERNAL_HEADER_FILEHANDLERS}
		${INTERNAL_HEADER_FORMATSUPPORT_AIFF}
		${INTERNAL_HEADER_FORMATSUPPORT_IFF}
		${INTERNAL_HEADER_FORMATSUPPORT_WAVE}
		${INTERNAL_HEADER_FORMATSUPPORT}
		${INTERNAL_HEADER_PLUGINHANDLER}
		${PUBLIC_HEADER_CLIENTGLUE}
		${PUBLIC_HEADER}
		${HEADERFILES}
		${RESOURCE_FILES}
		${SOURCEFILES_COMMONCODE_NMDS}
		${SOURCEFILES_COMMONCODE}
		${SOURCEFILES_FILEHANDLERS}
		${SOURCEFILES_FORMATSUPPORT_AIFF}
		${SOURCEFILES_FORMATSUPPORT_IFF}
		${SOURCEFILES_FORMATSUPPORT_WAVE}
		${SOURCEFILES_FORMATSUPPORT}
		${SOURCEFILES_PLUGINHANDLER}
		${PUBLIC_XMPCOMMON_CLIENTGLUE_FILES}
		${PUBLIC_XMPCORE_CLIENTGLUE_FILES}
		${FILES_THIRDPARTY_EXPAT}
)

add_library(XMPFiles ${SOURCE_FILES})
target_include_directories(XMPFiles PUBLIC
		$<BUILD_INTERFACE:${XMPROOT_DIR}>
		$<BUILD_INTERFACE:${XMPROOT_DIR}/public/include>
		$<BUILD_INTERFACE:${XMPROOT_DIR}/public/include/client-glue>
		$<BUILD_INTERFACE:${XMPROOT_DIR}/XMPFilesPlugins/api/source>
		$<BUILD_INTERFACE:${RESOURCE_ROOT}/${XMP_PLATFORM_SHORT}>
		$<INSTALL_INTERFACE:${XMP_INSTALL_INCLUDE_DIR}>
)

if (CMAKE_WINUNIVERSAL)
	add_definitions(-DWIN_UNIVERSAL_ENV -DWIN_ENV)
endif()
set(XMPFilesLibraries XMPCore EXPAT::EXPAT ZLIB::ZLIB)
if (APPLE)
	target_link_libraries(XMPFiles PRIVATE ${XMPFilesLibraries} "-framework CoreFoundation -framework CoreServices")
else()
	target_link_libraries(XMPFiles PRIVATE ${XMPFilesLibraries})
endif()



