

# global needed variables
set(APPLICATION_NAME "ocsync")

set(APPLICATION_VERSION_MAJOR "0")
set(APPLICATION_VERSION_MINOR "91")
set(APPLICATION_VERSION_PATCH "4")

set(APPLICATION_VERSION "${APPLICATION_VERSION_MAJOR}.${APPLICATION_VERSION_MINOR}.${APPLICATION_VERSION_PATCH}")

set(LIBRARY_VERSION "0.2.1")
set(LIBRARY_SOVERSION "0")

# add definitions
include(DefineCMakeDefaults)
include(DefinePlatformDefaults)
include(DefineCompilerFlags)
include(DefineOptions.cmake)

include(DefineInstallationPaths)

# add macros
include(MacroAddPlugin)
include(MacroCopyFile)

if (NOT WIN32)
    find_package(Iconv)
endif (NOT WIN32)
find_package(CMocka)
if (CMOCKA_FOUND AND UNIT_TESTING)
    include(AddCMockaTest)
endif (CMOCKA_FOUND AND UNIT_TESTING)

include(ConfigureChecks.cmake)


set(SOURCE_DIR ${CMAKE_SOURCE_DIR})

include_directories(${CMAKE_CURRENT_BINARY_DIR})

macro_copy_file(${CMAKE_CURRENT_SOURCE_DIR}/CTestCustom.cmake ${CMAKE_CURRENT_BINARY_DIR}/CTestCustom.cmake)

if (MEM_NULL_TESTS)
  add_definitions(-DCSYNC_MEM_NULL_TESTS)
endif (MEM_NULL_TESTS)

add_subdirectory(src)
add_subdirectory(config)
add_subdirectory(csync_doc)

if (CMOCKA_FOUND AND UNIT_TESTING)
    add_subdirectory(tests)
endif (CMOCKA_FOUND AND UNIT_TESTING)

configure_file(config_csync.h.cmake ${CMAKE_CURRENT_BINARY_DIR}/config_csync.h)
configure_file(config_test.h.cmake  ${CMAKE_CURRENT_BINARY_DIR}/config_test.h)


