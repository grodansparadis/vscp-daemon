# vscp-daemon

The VSCP daemon is the server for the VSCP protocol.

## Build

The daemon now builds from this repository root and links against the vendored VSCP sources in `third-party/vscp`.

### Linux

```sh
cmake -S . -B build -DCMAKE_BUILD_TYPE=Release
cmake --build build --parallel
```

### macOS

Install the required dependencies with Homebrew first.

```sh
brew install openssl expat mosquitto curl cjson
cmake -S . -B build -DCMAKE_BUILD_TYPE=Release -DCMAKE_PREFIX_PATH="$(brew --prefix openssl);$(brew --prefix expat);$(brew --prefix mosquitto);$(brew --prefix curl);$(brew --prefix cjson)"
cmake --build build --parallel
```

### Windows

Use `vcpkg` manifest mode with the repository `vcpkg.json`.

```powershell
cmake -S . -B build -G "Visual Studio 17 2022" -A x64 `
	-DCMAKE_TOOLCHAIN_FILE=C:/src/vcpkg/scripts/buildsystems/vcpkg.cmake `
	-DVCPKG_TARGET_TRIPLET=x64-windows
cmake --build build --config Release --parallel
```

## Packages

Create binary packages with CPack after a successful build.

```sh
cd build
cpack -C Release
```

Default generators are:

- Linux: `TGZ` and `DEB`
- macOS: `TGZ`
- Windows: `ZIP` and `NSIS` when `makensis` is available

## CI

GitHub Actions builds and packages the daemon on Linux, macOS, and Windows from `.github/workflows/ci.yml`.
