cmake_minimum_required(VERSION 3.8)

project(BetterSunshineModule VERSION 1.0)

set(CMAKE_CXX_STANDARD 20)

file(GLOB BETTER_MOVESET_SRC
    "src/*.c"
    "src/*.cpp"
    "include/*.h"
    "include/*.hxx"
)

add_executable(BetterSunshineMoveset ${BETTER_MOVESET_SRC})
add_subdirectory(lib/BetterSunshineEngine)

target_link_libraries(BetterSunshineMoveset PUBLIC
                      BetterSunshineEngine
                      ${PROJECT_SOURCE_DIR}/lib/libKuriboClang/libKuriboClang.a
)

target_include_directories(BetterSunshineMoveset PUBLIC "include")

target_compile_options(BetterSunshineMoveset PUBLIC ${SMS_COMPILE_FLAGS})
target_link_options(BetterSunshineMoveset PUBLIC ${SMS_LINK_FLAGS})

list(APPEND BETTER_MOVESET_CONFIG_DEFINES "KURIBO_NO_TYPES" "BETTER_MOVESET_VERSION=\"v1.0\"")

target_compile_definitions(BetterSunshineModule PUBLIC ${BETTER_MOVESET_CONFIG_DEFINES})

add_custom_target(${PROJECT_NAME}.kxe ALL DEPENDS ${PROJECT_NAME})
add_custom_command(
    TARGET ${PROJECT_NAME}.kxe
    COMMAND ${SMS_KURIBO_CONVERTER_PATH} ARGS ${PROJECT_NAME} ${PROJECT_NAME}.kxe ${SMS_LINKER_MAP_PATH}
)