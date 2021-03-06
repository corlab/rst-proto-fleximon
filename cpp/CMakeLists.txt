SET_PROPERTY(GLOBAL APPEND PROPERTY ENABLED_FEATURES "cpp binding")

INCLUDE(PedanticCompilerWarnings)
INCLUDE(InstallFilesRecursive)

SET(STABLE_NAME "rst")
SET(STABLE_LIBRARY_NAME "${STABLE_NAME}${VERSION_SUFFIX}")
SET(SANDBOX_NAME "rstsandbox")
SET(SANDBOX_LIBRARY_NAME "${SANDBOX_NAME}${VERSION_SUFFIX}")
SET(SO_VERSION "${RST_VERSION_MAJOR}.${RST_VERSION_MINOR}")

INCLUDE_DIRECTORIES(BEFORE SYSTEM ${PROTOBUF_INCLUDE_DIRS})

ADD_SUBDIRECTORY(stable)
ADD_SUBDIRECTORY(sandbox)

# --- decide on export macros for config files ---

IF(WIN32)
    SET(RST_CFLAGS "/DRST_EXPORT=__declspec(dllimport)")
    SET(RSTSANDBOX_CFLAGS "/DRSTSANDBOX_EXPORT=__declspec(dllimport)")
ELSE()
    SET(RST_CFLAGS "-DRST_EXPORT=")
    SET(RSTSANDBOX_CFLAGS "-DRSTSANDBOX_EXPORT=")
ENDIF()

# --- user options ---

OPTION(EXPORT_TO_CMAKE_PAKAGE_REGISTRY "If set to ON, ${CMAKE_PROJECT_NAME} will be exported to the CMake user package registry so that downstream projects automatically find the workspace location in find_package calls." OFF)

# --- pkgconfig files ---

CONFIGURE_FILE("${CMAKE_CURRENT_SOURCE_DIR}/${STABLE_NAME}.pc.in"
               "${CMAKE_CURRENT_BINARY_DIR}/${STABLE_NAME}${VERSION_SUFFIX}.pc"
               @ONLY)
CONFIGURE_FILE("${CMAKE_CURRENT_SOURCE_DIR}/${SANDBOX_NAME}.pc.in"
               "${CMAKE_CURRENT_BINARY_DIR}/${SANDBOX_NAME}${VERSION_SUFFIX}.pc"
               @ONLY)
INSTALL(FILES "${CMAKE_CURRENT_BINARY_DIR}/${STABLE_NAME}${VERSION_SUFFIX}.pc"
              "${CMAKE_CURRENT_BINARY_DIR}/${SANDBOX_NAME}${VERSION_SUFFIX}.pc"
        DESTINATION lib/pkgconfig)

# --- cmake export ---

IF(EXPORT_TO_CMAKE_PAKAGE_REGISTRY)
    MESSAGE(STATUS "Exported ${CMAKE_PROJECT_NAME} to CMake package registry")
    EXPORT(PACKAGE "${CMAKE_PROJECT_NAME}")
ENDIF()

CONFIGURE_FILE("${CMAKE_PROJECT_NAME}BuildTreeSettings.cmake.in"
               "${CMAKE_CURRENT_BINARY_DIR}/${CMAKE_PROJECT_NAME}BuildTreeSettings.cmake"
               @ONLY)

FILE(TO_CMAKE_PATH "${RSC_DIR}" RSC_DIR)
FILE(TO_CMAKE_PATH "${PROTOBUF_ROOT}" PROTOBUF_ROOT)
CONFIGURE_FILE("${CMAKE_PROJECT_NAME}Config.cmake.in"
               "${CMAKE_CURRENT_BINARY_DIR}/${CMAKE_PROJECT_NAME}Config.cmake"
               @ONLY)
INSTALL(FILES "${CMAKE_CURRENT_BINARY_DIR}/${CMAKE_PROJECT_NAME}Config.cmake"
        DESTINATION "share/${INSTALL_PREFIX}")
CONFIGURE_FILE("${CMAKE_PROJECT_NAME}ConfigVersion.cmake.in"
               "${CMAKE_CURRENT_BINARY_DIR}/${CMAKE_PROJECT_NAME}ConfigVersion.cmake"
               @ONLY)
INSTALL(FILES "${CMAKE_CURRENT_BINARY_DIR}/${CMAKE_PROJECT_NAME}ConfigVersion.cmake"
        DESTINATION "share/${INSTALL_PREFIX}")

EXPORT(TARGETS ${STABLE_LIBRARY_NAME} ${SANDBOX_LIBRARY_NAME}
       FILE "${CMAKE_CURRENT_BINARY_DIR}/${CMAKE_PROJECT_NAME}Depends.cmake")
INSTALL(EXPORT "${CMAKE_PROJECT_NAME}Depends"
        DESTINATION "share/${INSTALL_PREFIX}")
