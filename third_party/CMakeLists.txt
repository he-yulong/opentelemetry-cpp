set(OPTL_3RDPARTY_DIRECTORY ${CMAKE_CURRENT_SOURCE_DIR})

function(GetAllTargets var)
  set(targets)
  GetAllTargetsRecursive(targets ${CMAKE_CURRENT_SOURCE_DIR})
  set(${var} ${targets} PARENT_SCOPE)
endfunction()

macro(GetAllTargetsRecursive targets dir)
  get_property(subdirectories DIRECTORY ${dir} PROPERTY SUBDIRECTORIES)
  foreach(subdir ${subdirectories})
    GetAllTargetsRecursive(${targets} ${subdir})
  endforeach()

  get_property(current_targets DIRECTORY ${dir} PROPERTY BUILDSYSTEM_TARGETS)
  list(APPEND ${targets} ${current_targets})
endmacro()

add_subdirectory(curl EXCLUDE_FROM_ALL)
add_subdirectory(protobuf EXCLUDE_FROM_ALL)