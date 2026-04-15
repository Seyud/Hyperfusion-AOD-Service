# Hyperfusion AOD Wake Service

[![Go](https://img.shields.io/badge/Go-1.21-blue)](https://golang.org/)
[![License: MIT](https://img.shields.io/badge/License-MIT-green)](LICENSE)

Repository: https://github.com/CanStarRivers/AODWakeService

File: vendor.hyperfusion.aod.display-service.go

Hyperfusion AOD Wake Service is a zero-overhead Always-On Display (AOD) wake service for Android devices. It leverages low-level touch gestures to wake the screen efficiently while keeping power consumption minimal.

## Features

- Precise gesture-based wake detection using low-level keys (KEY_GOTO and others).
- Zero-overhead listener using a single thread; no coordinate calculations required.
- Monitors XML configuration changes using Inotify with a fallback polling mechanism.
- Smooth backlight fade-in and fade-out.
- Custom wake lock support to prevent CPU sleep during backlight fade or countdown.
- Automatic suspend state detection to prevent accidental wake-ups.

## Installation

### 1. Clone the repository

git clone https://github.com/CanStarRivers/AODWakeService.git
cd AODWakeService

### 2. Build the project

Ensure Go environment or cross-compilation toolchain is available:

go build -o hyperfusion_aod vendor.hyperfusion.aod.display-service.go

### 3. Deploy to Android

adb push hyperfusion_aod /data/local/tmp/
adb shell chmod 755 /data/local/tmp/hyperfusion_aod

### 4. Run the service

adb shell /data/local/tmp/hyperfusion_aod &

## Configuration

Configuration file path:

/data/user_de/0/com.miui.aod/shared_prefs/com.miui.aod_preferences.xml

- Set `aod_temporary_style` to `true` to enable AOD wake functionality.
- Configuration changes are automatically applied without restarting the service.

## Code Structure

| File / Module | Description |
|---------------|-------------|
| vendor.hyperfusion.aod.display-service.go | Core program handling configuration watch, gesture monitoring, backlight fading, and wake logic. |
| InputEvent struct | Maps to Linux 64-bit `input_event` structure. |
| Constants | Includes touch key codes, backlight paths, wake lock names, and other parameters. |

## Usage Example

When the service is running and the device screen is off:

1. A FTS touch gesture key is pressed.
2. The service checks the configuration and system suspend state.
3. A custom wake lock is acquired.
4. Backlight fades smoothly to the target brightness.
5. After a 10-second timeout, the screen fades out and wake locks are released.

## Notes

- Supports only devices with FTS touch drivers.
- Root access is required to access `/sys/class/backlight` and `/sys/power`.
- Changing `TargetBrightness` or `AODTimeout` requires restarting the service.
- Running on non-standard Android ROMs may result in permission or node access issues.

## License

This project is licensed under the MIT License. See the LICENSE file for details.

## Commercial Use Restriction

**This software is strictly prohibited from any paid commercial use.**  
Personal, educational, or open-source usage is allowed, but any charging, selling, or commercial monetization is forbidden without explicit permission from the author.
