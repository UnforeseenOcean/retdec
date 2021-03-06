find_package(Doxygen REQUIRED)

# Variables.
set(DOXYGEN_CFG_IN "doxygen.in")
set(DOXYGEN_CFG "${CMAKE_CURRENT_BINARY_DIR}/doxygen.cfg")
set(DOXYGEN_LOG "${CMAKE_CURRENT_BINARY_DIR}/doxygen.log")
# The trailing '/' after html is significant.
set(DOXYGEN_OUTPUT_DIR "${CMAKE_CURRENT_BINARY_DIR}/html/")

# Properly configure the Doxygen configuration file.
set(DOXYGEN_WARNINGS "YES")
set(DOXYGEN_EXTRACT_PRIVATE "YES")
set(DOXYGEN_EXTRACT_LOCAL_CLASSES "YES")
set(DOXYGEN_INTERNAL_DOCS "YES")
set(DOXYGEN_EXCLUDE_PATTERNS "")
set(DOXYGEN_EXCLUDE_SYMBOLS "")
set(DOXYGEN_ENABLED_SECTIONS "internal")

# Configuration.
configure_file("${DOXYGEN_CFG_IN}" "${DOXYGEN_CFG}" @ONLY)

# Add a target to generate the documentation by running Doxygen.
add_custom_target(doc ALL
	COMMAND ${DOXYGEN_EXECUTABLE} "${DOXYGEN_CFG}"
	SOURCES "${DOXYGEN_CFG}"
	WORKING_DIRECTORY "${CMAKE_CURRENT_BINARY_DIR}"
	COMMENT "Generating API documentation with Doxygen" VERBATIM
)

# Cleanup.
set_directory_properties(PROPERTIES ADDITIONAL_MAKE_CLEAN_FILES "${DOXYGEN_OUTPUT_DIR}")

# Install.
install(DIRECTORY "${DOXYGEN_OUTPUT_DIR}" DESTINATION share/retdec/doc)
