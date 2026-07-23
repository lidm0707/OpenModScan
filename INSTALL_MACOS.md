# Building and Installing OpenModScan on macOS

OpenModScan builds natively on macOS via CMake with Qt 5.15+ or Qt 6.
The required Qt modules are: `Core`, `Gui`, `Widgets`, `Network`, `PrintSupport`,
`SerialBus`, `SerialPort` (and `Core5Compat` + `LinguistTools` when using Qt 6).

The minimum supported version of macOS for building OpenModScan from sources is macOS 11 (Big Sur).

## Prerequisites

1. Install [Xcode](https://developer.apple.com/xcode/) Command Line Tools
```bash
xcode-select --install
```
2. Install [Homebrew](https://brew.sh)
```bash
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
```
3. Install [git](https://git-scm.com/downloads/mac), [CMake](https://cmake.org), Ninja and Qt
```bash
brew install git cmake ninja qt
```
`brew install qt` provides Qt 6 with all required modules.

If you prefer Qt 5.15, use the [Qt Online Installer](https://www.qt.io/download-open-source)
and select the macOS archive.

## Building

1. Clone OpenModScan sources from the github repository
```bash
git clone https://github.com/sanny32/OpenModScan.git
```
2. Go to the OpenModScan folder
```bash
cd OpenModScan
```
3. Configure the project with CMake
```bash
mkdir build
cd build
cmake ../src -GNinja \
    -DCMAKE_BUILD_TYPE=Release \
    -DUSE_QT6=ON \
    -DCMAKE_PREFIX_PATH="$(brew --prefix qt)/lib/cmake"
```
For Qt 5.15, replace `-DUSE_QT6=ON` with `-DUSE_QT5=ON` and point
`CMAKE_PREFIX_PATH` at your Qt 5 installation (e.g. `~/Qt/5.15.2/macos/lib/cmake`).

4. Build the project
```bash
ninja
```

The build produces a macOS application bundle at `build/omodscan.app`.

## Install

Install the bundle into `/Applications`:
```bash
cmake --install . --prefix /Applications
```

Alternatively, copy it manually:
```bash
cp -R omodscan.app /Applications/
```

### Launch

```bash
open /Applications/omodscan.app
```

### macOS Gatekeeper

The bundle is not code-signed, so the first launch from Finder may be blocked by
Gatekeeper. Either right-click the app and choose **Open**, or clear the
quarantine attribute:
```bash
xattr -dr com.apple.quarantine /Applications/omodscan.app
```

## Remove

To remove the bundle run:
```bash
rm -rf /Applications/omodscan.app
```

## Serial port usage

Serial port access on macOS does **not** require a `dialout` group (that is Linux only).
USB-RS485 / USB-RS232 adapters appear automatically as `/dev/cu.usbserial-*` and are
selectable directly in the OpenModScan connection dialog.
