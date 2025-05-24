cmake_minimum_required(VERSION 3.16)

# Use Vita toolchain if available
if(NOT DEFINED CMAKE_TOOLCHAIN_FILE)
  if(DEFINED ENV{VITASDK})
    set(CMAKE_TOOLCHAIN_FILE "$ENV{VITASDK}/share/vita.toolchain.cmake" CACHE PATH "toolchain file")
  else()
    message(FATAL_ERROR "Please define VITASDK to point to your SDK path!")
  endif()
endif()

project(OpenClaw)

# ===================== Vita Toolchain =====================
set(VITA TRUE)
set(CMAKE_CXX_STANDARD 11)
set(CMAKE_CXX_STANDARD_REQUIRED ON)
set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -O2 -g")

# ===================== Dependencies =====================
add_subdirectory(Box2D)
add_subdirectory(libwap)
add_subdirectory(./ThirdParty/Tinyxml)

# ===================== Include Paths =====================
include_directories(
    ${PROJECT_SOURCE_DIR}/OpenClaw
    ./Box2D/
    ./libwap/
    ./ThirdParty/Tinyxml/
    ${PROJECT_SOURCE_DIR}/ThirdParty
    ${VITASDK}/arm-vita-eabi/include/SDL2
)

# ===================== Library Search Paths =====================
link_directories(
    ${VITASDK}/arm-vita-eabi/lib
)

# ===================== Output Settings =====================
set(CMAKE_RUNTIME_OUTPUT_DIRECTORY ../Build_Release)

# ===================== Executable =====================
add_executable(openclaw "")
add_subdirectory(OpenClaw)

# ===================== Final Link Libraries =====================
target_link_libraries(openclaw
    SDL2_image
    SDL2_ttf
    SDL2_gfx
    SDL2_mixer
	SDL2

    libwap
    tinyxml
    Box2D

    freetype
    bz2
    png
    z
    m

    SceAppUtil_stub
    SceAudio_stub
    SceAudioIn_stub
    SceCtrl_stub
    SceDisplay_stub
    SceGxm_stub
    SceMotion_stub
    SceTouch_stub
    ScePower_stub
    SceHid_stub
    SceCommonDialog_stub
)

# ===================== Notes =====================
# ✅ SAFE FORMATS: .wav, .png, .bmp, .pcx
# ❌ DISABLED: .ogg, .webp, .mod, .opus, etc.
# 🛠 You have patched Audio.cpp and Image.cpp to remove unsupported formats.
# ⚠️ SDL2_gfx was removed. It is not available in the Vita SDK by default.
