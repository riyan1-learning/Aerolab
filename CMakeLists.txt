cmake_minimum_required(VERSION 3.20)

project(AeroLab
    VERSION 0.1
    LANGUAGES C CXX
)

# ============================================================
# C++ SETTINGS
# ============================================================

set(CMAKE_CXX_STANDARD 20)
set(CMAKE_CXX_STANDARD_REQUIRED ON)
set(CMAKE_CXX_EXTENSIONS OFF)

# ============================================================
# DEPENDENCIES
# ============================================================

include(FetchContent)

# ---------------- GLFW ----------------

set(GLFW_BUILD_DOCS OFF CACHE BOOL "" FORCE)
set(GLFW_BUILD_TESTS OFF CACHE BOOL "" FORCE)
set(GLFW_BUILD_EXAMPLES OFF CACHE BOOL "" FORCE)

FetchContent_Declare(
    glfw
    GIT_REPOSITORY https://github.com/glfw/glfw.git
    GIT_TAG 3.4
)

# ---------------- GLM ----------------

set(GLM_BUILD_TESTS OFF CACHE BOOL "" FORCE)

FetchContent_Declare(
    glm
    GIT_REPOSITORY https://github.com/g-truc/glm.git
    GIT_TAG 1.0.1
)

# ---------------- GLAD 2 ----------------

FetchContent_Declare(
    glad
    GIT_REPOSITORY https://github.com/Dav1dde/glad.git
    GIT_TAG glad2
    GIT_PROGRESS TRUE
    SOURCE_SUBDIR cmake
)

FetchContent_MakeAvailable(glfw glm glad)

# Generate an OpenGL 4.6 Core loader
glad_add_library(
    glad_gl_core_46
    REPRODUCIBLE
    LOADER
    API gl:core=4.6
)

# ============================================================
# AEROLAB
# ============================================================

add_executable(AeroLab
    src/main.cpp
)

# ============================================================
# INCLUDE DIRECTORIES
# ============================================================

target_include_directories(AeroLab PRIVATE
    ${glm_SOURCE_DIR}
)

# ============================================================
# LINK LIBRARIES
# ============================================================

target_link_libraries(AeroLab PRIVATE
    glfw
    glad_gl_core_46
)

# ============================================================
# WINDOWS SETTINGS
# ============================================================

if(WIN32)
    target_link_libraries(AeroLab PRIVATE
        opengl32
    )
endif()

# ============================================================
# OUTPUT
# ============================================================

set_target_properties(AeroLab PROPERTIES
    RUNTIME_OUTPUT_DIRECTORY "${CMAKE_BINARY_DIR}/bin"
)