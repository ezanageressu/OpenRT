file(GLOB CORE_INCLUDE  ${PROJECT_SOURCE_DIR}/include/openrt.h
                        ${PROJECT_SOURCE_DIR}/include/types.h
                        ${PROJECT_SOURCE_DIR}/include/macroses.h
						)
file(GLOB CORE_SOURCES "*.cpp")
file(GLOB CORE_HEADERS "*.h")

source_group("Include" FILES ${CORE_INCLUDE})
source_group("" FILES ${CORE_SOURCES} ${CORE_HEADERS}) 
source_group("Source Files\\cameras" FILES "ICamera.h" )
source_group("Source Files\\cameras\\perspective" FILES "CameraPerspective.h" "CameraPerspective.cpp"  "CameraPerspectiveTarget.h")
source_group("Source Files\\lights" FILES "ILight.h")
source_group("Source Files\\lights\\omni" FILES "LightOmni.h" "LightOmni.cpp")
source_group("Source Files\\lights\\spot" FILES "LightSpot.h" "LightSpot.cpp" "LightSpotTarget.h")
source_group("Source Files\\lights\\area" FILES "LightArea.h" "LightArea.cpp")
source_group("Source Files\\lights\\sky" FILES "LightSky.h" "LightSky.cpp")
source_group("Source Files\\primitives" FILES "IPrim.h")
source_group("Source Files\\primitives\\plane" FILES "PrimPlane.h" "PrimPlane.cpp")
source_group("Source Files\\primitives\\sphere" FILES "PrimSphere.h" "PrimSphere.cpp")
source_group("Source Files\\primitives\\triangle" FILES "PrimTriangle.h" "PrimTriangle.cpp" "PrimTriangleSmooth.h" "PrimTriangleSmooth.cpp")
source_group("Source Files\\solids" FILES "Solid.h" "Solid.cpp" "SolidQuad.h" "SolidBox.h" "SolidCone.h" "SolidCylinder.h")
source_group("Source Files\\shaders" FILES "IShader.h" "Shader.h" "Shader.cpp" "ShaderFlat.h" "ShaderEyelight.h" "ShaderChrome.h" "ShaderSSLT.h" "ShaderSSLT.cpp")
source_group("Source Files\\scene" FILES "Scene.h" "Scene.cpp")
source_group("Source Files\\utilities" FILES "ray.h" "random.h" "timer.h")
source_group("Source Files\\utilities\\BSP Tree" FILES "BSPNode.h" "BSPTree.h" "BoundingBox.h" "BoundingBox.cpp")
source_group("Source Files\\utilities\\samplers" FILES "Sampler.h" "Sampler.cpp" "Sampler3f.h" "Sampler3f.cpp")
source_group("Source Files\\utilities\\transform" FILES "Transform.h" "Transform.cpp")
source_group("Source Files\\utilities\\texture" FILES "Texture.h" "Texture.cpp")


# Properties -> C/C++ -> General -> Additional Include Directories
include_directories(${PROJECT_SOURCE_DIR}/include ${OpenCV_INCLUDE_DIRS})

add_library(core SHARED ${CORE_INCLUDE} ${CORE_SOURCES} ${CORE_HEADERS})

# Properties -> Linker -> Input -> Additional Dependencies
target_link_libraries(core ${OpenCV_LIBS})

set_target_properties(core PROPERTIES OUTPUT_NAME openrt_core${OPENRT_VERSION_MAJOR}${OPENRT_VERSION_MINOR}${OPENRT_VERSION_PATCH})
set_target_properties(core PROPERTIES VERSION ${OPENRT_VERSION_MAJOR}.${OPENRT_VERSION_MINOR}.${OPENRT_VERSION_PATCH} SOVERSION ${OPENRT_VERSION_MAJOR}.${OPENRT_VERSION_MINOR}.${OPENRT_VERSION_PATCH})
set_target_properties(core PROPERTIES LINKER_LANGUAGE CXX)

#install
install(TARGETS core 
	EXPORT OpenRTTargets
	RUNTIME DESTINATION ${CMAKE_INSTALL_PREFIX}/bin 
	LIBRARY DESTINATION ${CMAKE_INSTALL_PREFIX}/lib
	ARCHIVE DESTINATION ${CMAKE_INSTALL_PREFIX}/lib)
install(FILES ${CORE_INCLUDE} DESTINATION ${CMAKE_INSTALL_PREFIX}/include)
install(FILES ${CORE_HEADERS} DESTINATION ${CMAKE_INSTALL_PREFIX}/include/openrt)
 
# Creates folder "Modules" and adds target project 
set_target_properties(core PROPERTIES FOLDER "Modules")
