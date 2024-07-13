cmake_minimum_required(VERSION 3.22)

# set CMake control variables
set(CMAKE_C_STANDARD 99)
set(CMAKE_PROJECT_NAME lua)
set(CMAKE_EXPORT_COMPILE_COMMANDS TRUE)

# set Lua control variables
set(CORE_T lualib)
set(CORE_O_SRC
  lua/lapi.c
  lua/lcode.c
  lua/lctype.c
  lua/ldebug.c
  lua/ldo.c
  lua/ldump.c
  lua/lfunc.c
  lua/lgc.c
  lua/llex.c
  lua/lmem.c
  lua/lobject.c
  lua/lopcodes.c
  lua/lparser.c
  lua/lstate.c
  lua/lstring.c
  lua/ltable.c
  lua/ltm.c
  lua/lundump.c
  lua/lvm.c
  lua/lzio.c
  lua/ltests.c
)
set(AUX_O_SRC lua/lauxlib.c)
set(LIB_O_SRC
  lua/lbaselib.c
  lua/ldblib.c
  lua/liolib.c
  lua/lmathlib.c
  lua/loslib.c
  lua/ltablib.c
  lua/lstrlib.c
  lua/lutf8lib.c
  lua/loadlib.c
  lua/lcorolib.c
  lua/linit.c
)
set(CWARNSCPP
  -Wfatal-errors
  -Wextra
  -Wshadow
  -Wundef
  -Wwrite-strings
  -Wredundant-decls
  -Wdisabled-optimization
  -Wdouble-promotion
  -Wmissing-declarations
        # the next warnings might be useful sometimes,
  # but usually they generate too much noise
  # -Werror
  # -pedantic   # warns if we use jump tables
  # -Wconversion 
  # -Wsign-conversion
  # -Wstrict-overflow=2
  # -Wformat=2
  # -Wcast-qual
)
set(CWARNGCC
  -Wlogical-op
  -Wno-aggressive-loop-optimizations
)
set(CWARNSC
  -Wdeclaration-after-statement
  -Wmissing-prototypes
  -Wnested-externs
  -Wstrict-prototypes
  -Wc++-compat
  -Wold-style-definition
)
set(CWARNS ${CWARNSCPP} ${CWARNSC} ${CWARNGCC})
set(MYCFLAGS ${CWARNS} -DLUA_USE_LINUX -DLUA_USE_READLINE)
set(CFLAGS -Wall -O2 ${MYCFLAGS} -fno-stack-protector -fno-common -march=native)
set(CLIBS m dl readline)


# Core project settings
project(${CMAKE_PROJECT_NAME})

# Create libraries liked to exexutable
add_library(${CORE_T} STATIC ${CORE_O_SRC} ${AUX_O_SRC} ${LIB_O_SRC})
target_compile_options(${CORE_T} PUBLIC ${CFLAGS})

# Create an executable object type
add_executable(${CMAKE_PROJECT_NAME} lua/lua.c)

# Add linked libraries
target_link_libraries(${CMAKE_PROJECT_NAME} ${CORE_T} ${CLIBS})

# Create a symoblic link to compile_commands.json in the build directory and pass it to the IntelliSence engine in a build type independent manner.
if (UNIX)
  execute_process(COMMAND ${CMAKE_COMMAND} -E create_symlink ${CMAKE_SOURCE_DIR}/build/${CMAKE_BUILD_TYPE}/compile_commands.json ${CMAKE_SOURCE_DIR}/build/compile_commands.json)
endif()