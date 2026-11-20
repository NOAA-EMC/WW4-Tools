<p align="center">
  <img src="https://github.com/NOAA-EMC/WW4-Tools/wiki/images/WW_tools_banner.jpg" alt="WW4 banner" height="100">
</p>

# WW4-Tools Tests

C++ tests use GoogleTest and run through CTest. Python tests use pytest. Test levels follow the WW4 strategy: Level 1 files are unit tests (prefix `L1_test_`) and Level 2 files are integration tests (prefix `L2_test_`).

- C++ tests live in `tests/cpp/` and are registered in `CMakeLists.txt` with `ww4_add_test(<name> <source> <library>)`.
- Python tests live in `tests/python/`. Python code lives in `python/`, which pytest adds to the import path.

## Running Tests Interactively

1.  **C++** (from the project root, after `git submodule update --init --recursive`):
    ```bash
    cmake -B build -S .
    cmake --build build
    ctest --test-dir build --output-on-failure
    ```
    Strict warnings and sanitizers are available with `-DWW4_STRICT_WARNINGS=ON -DWW4_USE_SANITIZERS=ON`.
2.  **Python:**
    ```bash
    pytest
    ```

## Running Tests Automatically

Both test suites run in CI on pushes and pull requests for `main` and `develop`. See the [workflows README](../.github/workflows/README.md).

#
<p align="right">
  <img src="https://github.com/NOAA-EMC/WW4-Tools/wiki/images/noaa_logo.gif" alt="NOAA Logo" height="50" width="55">
  <img src="https://github.com/NOAA-EMC/WW4-Tools/wiki/images/nws.jpg" alt="NWS Logo" height="50" width="50">
 </p>
