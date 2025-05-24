# Vita-Claw

A personal porting attempt of [OpenClaw](https://github.com/pjasicek/OpenClaw) to the **PlayStation Vita** using the `vitasdk` toolchain.

This is a WIP. The `.vpk` currently compiles but **crashes on launch**.

---

## 🛠️ Requirements

- [VitaSDK](https://vitasdk.org/) installed and set up
- CMake ≥ 3.10
- A Vita with HENkaku or similar homebrew support
- CLAW.REZ

---

## 🔧 Building

Make a `build/` directory inside the project and run CMake like this:

```bash
mkdir -p build
cd build
cmake ../openclaw -DCMAKE_TOOLCHAIN_FILE=$VITASDK/share/vita.toolchain.cmake
make -j$(nproc)
