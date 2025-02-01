set(curl_directory ${OPTL_3RDPARTY_DIRECTORY}/curl/curl)

# No curl executable, docs, or tests
set(BUILD_SHARED_LIBS OFF CACHE BOOL "" FORCE)
set(BUILD_STATIC_LIBS ON CACHE BOOL "" FORCE)
set(BUILD_CURL_EXE OFF CACHE BOOL "" FORCE)
set(BUILD_TESTING OFF CACHE BOOL "" FORCE)
set(BUILD_LIBCURL_DOCS OFF CACHE BOOL "" FORCE)
set(BUILD_MISC_DOCS OFF CACHE BOOL "" FORCE)
set(ENABLE_CURL_MANUAL OFF CACHE BOOL "" FORCE)
set(CURL_DISABLE_INSTALL ON CACHE BOOL "" FORCE)
set(CURL_ENABLE_EXPORT_TARGET OFF CACHE BOOL "" FORCE)

# Disable unnecessary features
set(USE_NGHTTP2 OFF CACHE BOOL "" FORCE)
set(USE_LIBIDN2 OFF CACHE BOOL "" FORCE)
set(CURL_DISABLE_ALTSVC ON CACHE BOOL "" FORCE)
set(CURL_DISABLE_HSTS ON CACHE BOOL "" FORCE)
set(CURL_DISABLE_PROXY ON CACHE BOOL "" FORCE)
set(CURL_DISABLE_COOKIES ON CACHE BOOL "" FORCE)
set(CURL_DISABLE_SRP ON CACHE BOOL "" FORCE)
set(CURL_BROTLI OFF CACHE BOOL "" FORCE)
set(CURL_ZSTD OFF CACHE BOOL "" FORCE)
set(CURL_USE_LIBSSH2 OFF CACHE BOOL "" FORCE)
set(CURL_USE_GSSAPI OFF CACHE BOOL "" FORCE)
set(CURL_DISABLE_KERBEROS_AUTH ON CACHE BOOL "" FORCE)
set(CURL_DISABLE_NTLM ON CACHE BOOL "" FORCE)
set(CURL_DISABLE_NEGOTIATE_AUTH ON CACHE BOOL "" FORCE)

# SSL/TLS: Use Schannel for Windows
set(CURL_USE_SCHANNEL ON CACHE BOOL "" FORCE)
set(CURL_WINDOWS_SSPI OFF CACHE BOOL "" FORCE)

# Disable protocols except HTTP and HTTPS
set(HTTP_ONLY ON CACHE BOOL "" FORCE)
set(CURL_DISABLE_IPFS ON CACHE BOOL "" FORCE)
set(CURL_DISABLE_IPNS ON CACHE BOOL "" FORCE)
set(CURL_DISABLE_WEBSOCKETS ON CACHE BOOL "" FORCE)

# We only want IPv4
set(ENABLE_IPV6 OFF CACHE BOOL "" FORCE)

# Other optimizations
set(ENABLE_ARES OFF CACHE BOOL "" FORCE)
set(ENABLE_THREADED_RESOLVER OFF CACHE BOOL "" FORCE)
set(CURL_USE_LIBPSL OFF CACHE BOOL "" FORCE)
set(ENABLE_UNIX_SOCKETS OFF CACHE BOOL "" FORCE)

add_subdirectory(curl EXCLUDE_FROM_ALL)

# Redirect the find_packge(curl) to this directory
set(CMAKE_DISABLE_FIND_PACKAGE_CURL FALSE CACHE BOOL "" FORCE)
set(CURL_INCLUDE_DIR
  ${curl_directory}
  ${PROJECT_BINARY_DIR}/third_party/curl/curl
  CACHE INTERNAL "" FORCE
)
set(CURL_INCLUDE_DIRS ${CURL_INCLUDE_DIR} CACHE INTERNAL "" FORCE)
set(CURL_LIBRARY CURL::libcurl_static CACHE INTERNAL "" FORCE)
set(CURL_LIBRARIES ${CURL_LIBRARY} CACHE INTERNAL "" FORCE)
set(CURL_FOUND TRUE CACHE INTERNAL "" FORCE)

# --------------------------------
# Fix CMake Error in prometheus-cpp: 
# install(EXPORT "prometheus-cpp-targets" ...) includes target "push" which requires target "libcurl_static" that is not in any export set.
install(TARGETS libcurl_static
    EXPORT curl-targets
    ARCHIVE DESTINATION lib
    LIBRARY DESTINATION lib
    RUNTIME DESTINATION bin
    INCLUDES DESTINATION include
)

install(EXPORT curl-targets
    FILE curlTargets.cmake
    NAMESPACE CURL::
    DESTINATION lib/cmake/curl
)
# --------------------------------

GetAllTargets(all_targets)
foreach(target ${all_targets})
  get_target_property(vs_folder ${target} FOLDER)
  if(NOT vs_folder)
    set(vs_folder "")
  endif()

  set_target_properties(${target} PROPERTIES FOLDER "3rdparty/curl/${vs_folder}")
endforeach()
