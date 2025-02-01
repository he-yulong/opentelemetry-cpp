set(protobuf_directory ${OPTL_3RDPARTY_DIRECTORY}/protobuf/protobuf)

set(BUILD_SHARED_LIBS OFF)
set(protobuf_MSVC_STATIC_RUNTIME ON CACHE BOOL "" FORCE)
set(protobuf_BUILD_TESTS OFF CACHE BOOL "" FORCE)
set(protobuf_BUILD_CONFORMANCE OFF CACHE BOOL "" FORCE)
set(protobuf_BUILD_EXAMPLES OFF CACHE BOOL "" FORCE)
set(protobuf_BUILD_PROTOBUF_BINARIES ON CACHE BOOL "" FORCE)
set(protobuf_BUILD_PROTOC_BINARIES ON CACHE BOOL "" FORCE)
set(protobuf_BUILD_SHARED_LIBS OFF CACHE BOOL "" FORCE)
set(protobuf_INSTALL OFF CACHE BOOL "" FORCE)
set(protobuf_WITH_ZLIB OFF CACHE BOOL "" FORCE)
set(ABSL_BUILD_TESTING OFF CACHE BOOL "" FORCE)
set(ABSL_PROPAGATE_CXX_STD ON CACHE BOOL "" FORCE)

add_subdirectory(protobuf EXCLUDE_FROM_ALL)

# Redirect the find_packge(Protobuf) to this directory
set(CMAKE_DISABLE_FIND_PACKAGE_PROTOBUF FALSE CACHE BOOL "" FORCE)
set(Protobuf_INCLUDE_DIR
  ${protobuf_directory}/src
  CACHE INTERNAL "" FORCE
)
set(Protobuf_INCLUDE_DIRS ${Protobuf_INCLUDE_DIR} CACHE INTERNAL "" FORCE)
set(Protobuf_LIBRARY protobuf CACHE INTERNAL "" FORCE)
set(Protobuf_LIBRARIES ${Protobuf_LIBRARY} CACHE INTERNAL "" FORCE)
set(Protobuf_FOUND TRUE CACHE INTERNAL "" FORCE)

GetAllTargets(all_targets)
foreach(target ${all_targets})
  get_target_property(vs_folder ${target} FOLDER)
  if(NOT vs_folder)
    set(vs_folder "")
  endif()

  set_target_properties(${target} PROPERTIES FOLDER "3rdparty/protobuf/${vs_folder}")
endforeach()
