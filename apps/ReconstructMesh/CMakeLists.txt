if(MSVC)
	FILE(GLOB LIBRARY_FILES_C "*.cpp" "*.rc")
else()
	FILE(GLOB LIBRARY_FILES_C "*.cpp")
endif()
FILE(GLOB LIBRARY_FILES_H "*.h" "*.inl")

cxx_executable_with_flags_no_pch(ReconstructMesh "Apps" "${cxx_default}" "${Boost_LIBRARIES};${OpenCV_LIBRARIES};MVS" ${LIBRARY_FILES_C} ${LIBRARY_FILES_H})
link_directories(${Boost_LIBRARY_DIRS})
target_link_libraries(ReconstructMesh
    boost_program_options
    boost_iostreams
    boost_system
    boost_serialization
    opencv_core
    opencv_imgproc
    opencv_highgui
    Common)

if(OpenMVS_USE_BREAKPAD AND BREAKPAD_FOUND)
	target_link_libraries(ReconstructMesh ${BREAKPAD_LIBS})
endif()

# Install
INSTALL(TARGETS ReconstructMesh
	EXPORT OpenMVSTargets
	RUNTIME DESTINATION "${INSTALL_BIN_DIR}" COMPONENT bin)
