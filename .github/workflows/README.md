<p align="center">
  <img src="https://github.com/NOAA-EMC/WW4-Tools/wiki/images/WW_tools_banner.jpg" alt="WW4 banner" height="100">
</p>

# WW4-Tools GitHub Workflows

This directory contains the GitHub Actions workflow configurations for WW4-Tools. They cover C++ and Python formatting, linting and unit testing, and mirror the workflows of the WAVEWATCH IV (WW4) repository.

| Workflow | File | Checks |
|----------|------|--------|
| Lint and Format | `linting_cpp.yaml` | `clang-format` (dry run) and `cppcheck` on C++ in `src/` and `tests/` |
| Python Linting | `linting_python.yaml` | `ruff check` and `ruff format --check` |
| Unit Tests | `tests.yaml` | C++ (CMake, GoogleTest, CTest) and Python (pytest) |

The linting workflows run on every push and pull request. The test workflow runs on pushes and pull requests for `main` and `develop`.

## Running checks interactively

1.  **C++ formatting:**
    ```bash
    find src tests \( -name "*.cpp" -o -name "*.hpp" -o -name "*.h" \) | xargs clang-format -i
    ```
2.  **C++ static analysis:**
    ```bash
    cppcheck --enable=all --suppress=missingIncludeSystem --suppress=unmatchedSuppression --suppress=unusedFunction --suppress=missingInclude -I src src
    ```
3.  **Python linting and formatting:**
    ```bash
    ruff check .
    ruff format .
    ```
4.  **Tests:** see the [Tests README](../../tests/README.md).

## Documentation

C++ uses Doxygen comments and Python uses NumPy-style docstrings, as required in [AGENTS.md](../../AGENTS.md). Build the Doxygen documentation with `doxygen Doxyfile` from the repository root. Ruff enforces the NumPy docstring convention for Python.

#
<p align="right">
  <img src="https://github.com/NOAA-EMC/WW4-Tools/wiki/images/noaa_logo.gif" alt="NOAA Logo" height="50" width="55">
  <img src="https://github.com/NOAA-EMC/WW4-Tools/wiki/images/nws.jpg" alt="NWS Logo" height="50" width="50">
 </p>
