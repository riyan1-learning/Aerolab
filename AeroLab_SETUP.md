# AeroLab — Project Setup & Development Guide

## 1. What is AeroLab?

AeroLab is a custom aerodynamics / CFD learning simulator.

The long-term goal is to build the simulator step by step:

1. 2D airflow around an obstacle
2. Velocity / airflow visualization
3. Pressure field
4. Drag calculation
5. Airfoil simulation
6. 3D visualization
7. GPU acceleration / visualization
8. More realistic CFD methods

This is an educational simulator. The first versions will use simplified physics before moving toward real CFD techniques such as numerical solutions of the Navier–Stokes equations.

---

# 2. Current Technology Stack

AeroLab uses:

- **C++20** — main programming language
- **OpenGL 4.6** — graphics API
- **GLSL** — GPU shader language
- **GLFW** — window creation and keyboard/mouse input
- **GLAD 2** — OpenGL function loader
- **GLM** — graphics mathematics
- **CMake** — build system and dependency management
- **MSVC** — Microsoft C++ compiler
- **NVIDIA GeForce GT 730** — graphics card

The basic architecture is:

```text
C++20
  |
  +-- GLFW
  |     |
  |     +-- Window
  |     +-- Keyboard
  |     +-- Mouse
  |
  +-- GLAD
  |     |
  |     +-- OpenGL 4.6 functions
  |
  +-- GLM
  |     |
  |     +-- Vectors
  |     +-- Matrices
  |     +-- Transformations
  |
  +-- OpenGL
        |
        +-- GLSL shaders
        +-- GPU rendering
        |
        v
   NVIDIA GT 730
```

---

# 3. PC Information

Current development PC:

- OS: Windows 11 Pro 64-bit
- CPU: Intel Core i5-11400F
- CPU: 6 cores / 12 threads
- RAM: 16 GB
- GPU: NVIDIA GeForce GT 730
- OpenGL: 4.6
- Monitor: Dell S2421HN
- Resolution: 1920 × 1080
- Refresh rate: 75 Hz

The GT 730 was successfully detected through OpenGL.

For AeroLab, pure OpenGL is being used because it works directly with the actual GPU.

---

# 4. Project Location

The project is located at:

```text
D:\Areolab
```

Note:

The folder is named `Areolab`, not `AeroLab`.

Keep using:

```text
D:\Areolab
```

---

# 5. Project Structure

Current intended structure:

```text
D:\Areolab
│
├── CMakeLists.txt
│
├── src
│   └── main.cpp
│
├── shaders
│   ├── vertex.glsl
│   └── fragment.glsl
│
└── build
```

CMake automatically downloads/builds the third-party dependencies, so an `external` folder is NOT required.

The dependencies are stored inside the CMake build directory.

For example:

```text
build
└── _deps
    ├── glfw-*
    ├── glm-*
    └── glad-*
```

Do not manually edit those dependency folders.

---

# 6. What the Dependencies Do

## GLFW

GLFW handles:

- Creating the application window
- Creating the OpenGL context
- Keyboard input
- Mouse input
- Window events

Think:

```text
GLFW = Window + Input
```

---

## GLAD

GLAD loads OpenGL functions so C++ can call modern OpenGL functions.

Think:

```text
GLAD = OpenGL function loader
```

---

## GLM

GLM provides graphics mathematics.

It will eventually be used for:

- 2D vectors
- 3D vectors
- Matrices
- Camera transformations
- Rotation
- Translation
- Projection
- Coordinate systems

Think:

```text
GLM = Graphics math
```

---

## OpenGL

OpenGL is the actual graphics API.

It communicates with the GPU to render things.

Think:

```text
OpenGL = Draw using the GPU
```

---

# 7. CMake

CMake is responsible for configuring and building the project.

It also downloads the dependencies using `FetchContent`.

The important idea is:

```text
CMakeLists.txt
      |
      +-- GLFW
      +-- GLM
      +-- GLAD
      |
      v
   Build system
      |
      v
   AeroLab.exe
```

Normally, the CMake file should NOT need to be changed when only changing `main.cpp`.

---

# 8. Current CMakeLists.txt

The current `CMakeLists.txt` is:

```cmake
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
```

---

# 9. Initial Test Program

The first test program was:

```cpp
#include <iostream>

int main()
{
    std::cout << "AeroLab C++ build works!\n";
    return 0;
}
```

This successfully compiled and ran.

Output:

```text
AeroLab C++ build works!
```

This confirmed that:

- CMake works
- MSVC works
- C++ compilation works
- GLFW downloaded successfully
- GLM downloaded successfully
- The executable was created successfully

---

# 10. Build Directory

The build directory is:

```text
D:\Areolab\build
```

CMake generates Visual Studio/MSBuild files here.

The executable is configured to go into:

```text
D:\Areolab\build\bin\AeroLab.exe
```

---

# 11. Normal Development Workflow

When changing only `main.cpp`, use:

```powershell
cd D:\Areolab
cmake --build build
.\build\bin\AeroLab.exe
```

This is the normal workflow.

In short:

```text
Edit main.cpp
      ↓
Build
      ↓
Run
```

Commands:

```powershell
cmake --build build
.\build\bin\AeroLab.exe
```

---

# 12. When CMakeLists.txt Changes

If `CMakeLists.txt` itself changes, reconfigure first:

```powershell
cd D:\Areolab
cmake -S . -B build
cmake --build build
.\build\bin\AeroLab.exe
```

The important difference is:

```text
main.cpp changed
    ↓
cmake --build build
```

but:

```text
CMakeLists.txt changed
    ↓
cmake -S . -B build
    ↓
cmake --build build
```

---

# 13. If the Build Folder Gets Messed Up

You can delete the build directory and recreate it.

From:

```text
D:\Areolab
```

run:

```powershell
Remove-Item -Recurse -Force build
```

Then:

```powershell
cmake -S . -B build
cmake --build build
```

This forces CMake to configure everything again.

You normally do NOT need to do this.

---

# 14. First Real OpenGL Milestone

The next major milestone is:

```text
C++
 ↓
GLFW
 ↓
Create OpenGL window
 ↓
GLAD
 ↓
Load OpenGL 4.6
 ↓
Check OpenGL version
 ↓
Clear screen
```

After that:

```text
OpenGL window
      ↓
Triangle
      ↓
Shaders
      ↓
Vertex buffers
      ↓
Camera
      ↓
3D
      ↓
Meshes
      ↓
Airflow visualization
```

---

# 15. Shader Files

Shaders will live in:

```text
D:\Areolab\shaders
```

Planned files:

```text
shaders
├── vertex.glsl
└── fragment.glsl
```

The vertex shader controls how vertices are transformed.

The fragment shader controls the color/output of pixels.

Basic pipeline:

```text
Vertex data
     ↓
Vertex Shader
     ↓
Rasterization
     ↓
Fragment Shader
     ↓
Screen
```

---

# 16. AeroLab Graphics Roadmap

## Stage 1 — OpenGL Basics

Learn/build:

- Window
- OpenGL context
- OpenGL version
- Clear screen
- Triangle
- Vertex buffers
- Vertex arrays
- Shaders

---

## Stage 2 — 3D Rendering

Build:

- 3D coordinate system
- Transformations
- Camera
- Perspective projection
- Depth testing
- 3D meshes

GLM will become important here.

---

## Stage 3 — Airflow Visualization

Represent airflow as:

```text
→ → → → → → → →

        ███
      ███████
        ███

→ → ↘       ↗ → →
     ↘     ↗
       ↘ ↗
```

Eventually this becomes a velocity field.

---

## Stage 4 — Velocity Field

Represent velocity as a vector:

```text
v = (vx, vy)
```

Each location in the simulation has a velocity.

For example:

```text
→ → → →
→ ↗ → →
→ ↑ → →
→ → → →
```

The GPU can visualize thousands of arrows/particles.

---

## Stage 5 — Pressure Field

Introduce pressure:

```text
P
```

and visualize pressure across the simulation.

A simplified introductory relationship is:

```text
P + 1/2 ρv² = constant
```

where:

```text
P = pressure
ρ = air density
v = velocity
```

This is based on Bernoulli's equation and is useful for learning, but it is NOT a complete CFD solver.

---

## Stage 6 — Drag

Eventually calculate aerodynamic drag.

Introductory drag equation:

```text
F_D = 1/2 ρv² C_D A
```

where:

```text
F_D = drag force
ρ   = air density
v   = air speed
C_D = drag coefficient
A   = reference area
```

---

## Stage 7 — Airfoil

Move from simple obstacles to an airfoil.

Eventually visualize:

```text
             airflow
→ → → → → → → → → → →

          ______
       __/      \__
     _/            \_

→ → → → → → → → → → →
```

Possible outputs:

- Velocity
- Pressure
- Lift
- Drag
- Streamlines

---

## Stage 8 — More Realistic CFD

Only after the visualization and fundamentals work should AeroLab move toward more realistic CFD.

Possible topics:

- Grid-based simulation
- Finite differences
- Numerical stability
- Boundary conditions
- Incompressible flow
- Navier–Stokes equations
- Pressure solving
- Iterative numerical methods
- GPU compute

This is much harder than simply drawing airflow.

---

# 17. Important Development Principle

Do NOT try to build the complete CFD simulator immediately.

Use this progression:

```text
Window
  ↓
Triangle
  ↓
Shader
  ↓
3D
  ↓
Mesh
  ↓
Grid
  ↓
Velocity
  ↓
Particles
  ↓
Pressure
  ↓
Flow
  ↓
Aerodynamics
  ↓
CFD
```

Each stage should work before moving to the next one.

---

# 18. Why OpenGL?

The current PC successfully exposes OpenGL 4.6 through the GT 730.

Previous GPU/API testing showed:

```text
OpenGL → GT 730                         ✅
Vulkan  → no adapters                   ❌
DX12    → Microsoft Basic Render Driver ⚠️
```

Therefore OpenGL is the practical graphics API for this machine and this project.

---

# 19. Why C++?

C++ was selected because AeroLab is a graphics/simulation project.

Advantages for this project:

- Excellent OpenGL ecosystem
- Large amount of graphics programming material
- Good performance
- Direct GPU/API access
- Useful for learning graphics programming
- Useful foundation for rendering and simulation work

The goal is not simply maximum speed.

The goal is:

```text
Learn C++
+
Learn graphics
+
Learn GPU programming
+
Learn physics
+
Build AeroLab
```

---

# 20. What NOT to Do

Do not manually modify:

```text
build/_deps/
```

Do not manually edit downloaded GLFW/GLM/GLAD source.

Do not randomly replace the NVIDIA driver just because another graphics API does not work.

Do not delete the whole project when a build fails.

First read the error.

---

# 21. Useful Commands

## Enter project

```powershell
cd D:\Areolab
```

## Configure

```powershell
cmake -S . -B build
```

## Build

```powershell
cmake --build build
```

## Run

```powershell
.\build\bin\AeroLab.exe
```

## Configure + build

```powershell
cmake -S . -B build
cmake --build build
```

## Clean rebuild

```powershell
Remove-Item -Recurse -Force build
cmake -S . -B build
cmake --build build
```

---

# 22. Current Status

As of the initial setup:

```text
C++20             ✅
MSVC              ✅
CMake             ✅
GLFW              ✅
GLM               ✅
GLAD configuration 🔄
OpenGL window      ⏳
Triangle           ⏳
3D renderer        ⏳
Airflow            ⏳
Pressure           ⏳
CFD                ⏳
```

The basic C++/CMake pipeline is already proven to work.

---

# 23. Everyday Command

For normal development, remember only:

```powershell
cd D:\Areolab
cmake --build build
.\build\bin\AeroLab.exe
```

If you changed `CMakeLists.txt`:

```powershell
cd D:\Areolab
cmake -S . -B build
cmake --build build
.\build\bin\AeroLab.exe
```

---

# 24. Long-Term Goal

The final AeroLab concept is:

```text
                         AEROLAB
                            │
             ┌──────────────┴──────────────┐
             │                             │
         Simulation                    Visualization
             │                             │
      ┌──────┼──────┐               ┌──────┼──────┐
      │      │      │               │      │      │
 Velocity Pressure Physics        Meshes Shaders Particles
      │      │      │               │      │      │
      └──────┴──────┴───────────────┴──────┴──────┘
                            │
                          OpenGL
                            │
                           GPU
                            │
                         GT 730
```

The project is intended to become both a graphics-programming project and a physics/simulation learning project.
