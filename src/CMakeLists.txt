CMAKE_MINIMUM_REQUIRED(VERSION 3.5)

PROJECT("libexword-re")

FIND_PACKAGE(PkgConfig REQUIRED)
PKG_CHECK_MODULES(LIBUSB libusb-1.0)

IF(LIBUSB_FOUND)
    INCLUDE_DIRECTORIES(${LIBUSB_INCLUDE_DIRS})
    LINK_DIRECTORIES(${LIBUSB_LIBRARY_DIRS})
ENDIF()

# Find iconv (required for character encoding)
FIND_PACKAGE(Iconv REQUIRED)

SET(BIN_NAME "exword")
INCLUDE_DIRECTORIES(${PROJECT_SOURCE_DIR}/includes)
FILE(GLOB srcfile "databuffer.c" "dict.c" "exword.c" "obex.c" "util.c" )
ADD_EXECUTABLE(${BIN_NAME} main.c ${srcfile})

# Link with libusb, iconv, readline, and macOS frameworks
TARGET_LINK_LIBRARIES(${BIN_NAME} ${LIBUSB_LIBRARIES} Iconv::Iconv readline)
IF(APPLE)
    TARGET_LINK_LIBRARIES(${BIN_NAME} "-framework CoreFoundation" "-framework Carbon")
ENDIF()
