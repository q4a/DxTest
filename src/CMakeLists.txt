project(DxTest)

add_executable(${PROJECT_NAME} WIN32
	camera.cpp
	camera.h
	cameraApp.cpp
	d3dUtility.cpp
	d3dUtility.h
)

target_link_libraries(${PROJECT_NAME}
	PRIVATE
	${DXVK_NATIVE_D3D9_LIB}
)
