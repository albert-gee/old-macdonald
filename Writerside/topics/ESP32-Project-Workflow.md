<show-structure/>

# ESP32 Project Workflow

This section describes the workflow for creating, building, and deploying a new project.

Prerequisites:

- [ESP-IDF](ESP-IDF-Setup.md) development environment is set up.
- [ESP-Matter](ESP-Matter-Setup.md) development environment is set up.
- [ESP32](Espressif.md#development-boards) development board is available.
- CLion 2024.3.3 or later is installed.

## Step 1: Set Up ESP-IDF Environment

Run the alias, created during the development setup, to initialise the ESP-IDF environment in the current terminal
session:

```Bash
get_idf
```

## Step 2: Create a New Project

ESP-IDF provides the [idf.py](https://docs.espressif.com/projects/esp-idf/en/v5.2.3/esp32/api-guides/tools/idf-py.html)
command-line tool as a front-end for managing project builds, deployment, debugging, and other tasks, simplifying the
workflow significantly. It integrates several essential tools, including CMake for project configuration, Ninja for
building, and esptool.py for flashing the target device.

```Bash
idf.py create-project <project name>
```

## Step 3: Set ESP32 as the target device

The following sets the target device:

```Bash
idf.py set-target <chip_name>
idf.py set-target esp32h2
```

It creates a new `sdkconfig` file in the root directory of the project. This configuration file can be modified via
`idf.py menuconfig`.

## Step 4: Open the Project in CLion IDE

Create a toolchain named **ESP-IDF** with the environment file set to `esp-idf/script.sh`:

![Toolchain setup](image10.png){ thumbnail="true" width="500" }

Set up a CMake profile with the following Matter environmental variables:

```Bash
ESP_MATTER_PATH=/home/albert/esp/esp-matter-1.4;ZAP_INSTALL_PATH=/home/albert/esp/esp-matter-1.4/connectedhomeip/connectedhomeip/.environment/cipd/packages/zap;PATH=/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin:/usr/games:/usr/local/games:/snap/bin:/snap/bin:/home/albert/.local/share/JetBrains/Toolbox/scripts:/home/albert/esp/esp-matter-1.4/connectedhomeip/connectedhomeip/.environment/cipd/packages/pigweed
```

![CMake profile configuration](image26.png){ thumbnail="true" width="500" }

Structure of a new ESP-IDF project:

![Structure of a new ESP-IDF project](image12.png){ thumbnail="true" width="500" }

## Step 5: Build the Project

```Bash
idf.py build
```

## Step 6: Determine Serial Port

Connect the ESP32 board to the computer and check under which serial port the board is visible. Serial ports have the
following naming patterns: `/dev/tty`.

## Step 7: Flash Project to Target

```Bash
idf.py -p <PORT> flash
```

## Step 8: Launch IDF Monitor

Use the monitor application and exit using `CTRL+]`:

```Bash
idf.py -p <PORT> monitor
```

## References

[IDF Frontend - idf.py](https://docs.espressif.com/projects/esp-idf/en/stable/esp32h2/api-guides/tools/idf-py.html)