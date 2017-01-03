project(ifstat)
include_directories ("${PROJECT_SOURCE_DIR}")
add_definitions (
    -DHAVE_CONFIG_H
    )
add_executable(ifstat ifstat.c drivers.c data.c)
