if(MSVC)
	FILE(GLOB LIBRARY_FILES_C "*.cpp" "*.rc")
else()
	FILE(GLOB LIBRARY_FILES_C "*.cpp")
endif()
FILE(GLOB LIBRARY_FILES_H "*.h" "*.inl")

cxx_executable_with_flags_no_pch(RefineMesh "Apps" "${cxx_default}" "MVS" ${LIBRARY_FILES_C} ${LIBRARY_FILES_H})
link_directories(${Boost_LIBRARY_DIRS})
target_link_libraries(RefineMesh
    boost_program_options
    boost_iostreams
    boost_system
    boost_serialization
    opencv_core
    opencv_imgproc
    opencv_highgui
    Common)

if(OpenMVS_USE_BREAKPAD AND BREAKPAD_FOUND)
	target_link_libraries(RefineMesh ${BREAKPAD_LIBS})
endif()

# Install
INSTALL(TARGETS RefineMesh
	EXPORT OpenMVSTargets
	RUNTIME DESTINATION "${INSTALL_BIN_DIR}" COMPONENT bin)
