if(MSVC)
	FILE(GLOB LIBRARY_FILES_C "*.cpp" "*.rc")
else()
	FILE(GLOB LIBRARY_FILES_C "*.cpp")
endif()
FILE(GLOB LIBRARY_FILES_H "*.h" "*.inl")

cxx_executable_with_flags_no_pch(InterfaceCOLMAP "Apps" "${cxx_default}" "MVS" ${LIBRARY_FILES_C} ${LIBRARY_FILES_H})
link_directories(${Boost_LIBRARY_DIRS})
target_link_libraries(InterfaceCOLMAP
    boost_program_options
    boost_iostreams
    boost_system
    boost_serialization
    opencv_core
    opencv_imgproc
    opencv_highgui
    Common)

if(3Dnovator_USE_BREAKPAD AND BREAKPAD_FOUND)
	target_link_libraries(InterfaceCOLMAP ${BREAKPAD_LIBS})
endif()

# Install
INSTALL(TARGETS InterfaceCOLMAP
	EXPORT 3DnovatorTargets
	RUNTIME DESTINATION "${INSTALL_BIN_DIR}" COMPONENT bin)
