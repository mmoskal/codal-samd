project(codal-samd)

include("${CODAL_UTILS_LOCATION}")

# TODO allow SAMD21
set(ASF "${CMAKE_CURRENT_SOURCE_DIR}/asf4/samd51")
set(SP "${CMAKE_CURRENT_SOURCE_DIR}/samd-peripherals/samd")
set(SP2 "${SP}/samd51")

# find sources and headers
SET(TOP_LEVEL_INCLUDE_DIRS0 
    "${ASF}"
    "${ASF}/include"
    "${ASF}/config"
    "${ASF}/hri"
    "${ASF}/hpl/port"
    "${ASF}/hal/include"
    "${ASF}/hal/utils/include"
    "${SP}"
    "${CMAKE_CURRENT_SOURCE_DIR}/samd-peripherals"
)

RECURSIVE_FIND_DIR(TOP_LEVEL_INCLUDE_DIRS "${CMAKE_CURRENT_SOURCE_DIR}/inc" "*.h")

RECURSIVE_FIND_FILE(TOP_LEVEL_SOURCE0 "${CMAKE_CURRENT_SOURCE_DIR}/src" "*.cpp")
RECURSIVE_FIND_FILE(TOP_LEVEL_SOURCE1 "${CMAKE_CURRENT_SOURCE_DIR}/src" "*.c")

#RECURSIVE_FIND_FILE(TOP_LEVEL_SOURCE "${ASF}" "*.[cs]")

LIST(APPEND TOP_LEVEL_SOURCE
    "${SP}/clocks.c"
    "${SP2}/clocks.c"
    "${SP2}/cache.c"
    "${SP2}/pins.c"
    "${SP}/timers.c"
    "${SP2}/timers.c"
    "${SP}/sercom.c"
    "${SP2}/sercom.c"
    "${ASF}/hal/src/hal_atomic.c"
    "${ASF}/hal/src/hal_spi_m_sync.c"
    "${ASF}/hpl/sercom/hpl_sercom.c"
    "${CMAKE_CURRENT_SOURCE_DIR}/asm/CortexContextSwitch.s")

# add them
include_directories(${TOP_LEVEL_INCLUDE_DIRS0} ${TOP_LEVEL_INCLUDE_DIRS} ${INCLUDE_DIRS})

# create our target
add_library(codal-samd ${TOP_LEVEL_SOURCE} ${TOP_LEVEL_SOURCE0} ${TOP_LEVEL_SOURCE1})

target_link_libraries(codal-samd codal-core ${OBJECT_FILES} ${ARCHIVE_FILES})

#set_target_properties(codal-samd PROPERTIES LINKER_LANGUAGE CXX)

# expose it to parent cmake.
target_include_directories(codal-samd PUBLIC ${TOP_LEVEL_INCLUDE_DIRS} ${TOP_LEVEL_INCLUDE_DIRS0} ${INCLUDE_DIRS})
