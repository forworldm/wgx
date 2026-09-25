cmake_minimum_required(VERSION 3.16)

project(wgx LANGUAGES C)

set(CMAKE_C_STANDARD 11)
set(CMAKE_C_STANDARD_REQUIRED ON)

find_package(PkgConfig REQUIRED)
find_package(Threads REQUIRED)

pkg_check_modules(CARES REQUIRED IMPORTED_TARGET libcares)
pkg_check_modules(LIBUV REQUIRED IMPORTED_TARGET libuv)
pkg_check_modules(SODIUM REQUIRED IMPORTED_TARGET libsodium)

add_executable(wgx
    src/main.c
    src/blake2s.c
    src/crypto.c
    src/tai64n.c
    src/replay.c
    src/allowedips.c
    src/noise.c
    src/tun.c
    src/device.c
    src/timers.c
    src/uapi.c
    src/tcpstack.c
    src/tcp_worker.c
    src/socks5.c
    src/forward.c
    src/conf.c
)

target_include_directories(wgx PRIVATE
    src
)

target_link_libraries(wgx PRIVATE
    PkgConfig::CARES
    PkgConfig::LIBUV
    PkgConfig::SODIUM
    Threads::Threads
)

install(TARGETS wgx
    RUNTIME DESTINATION bin
)
