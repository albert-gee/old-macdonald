<show-structure/>

# Development Environment Setup

## Overview

- [ESP-IDF v5.2.3](https://github.com/espressif/esp-idf/tree/v5.2.3)
- [ESP-Matter v1.3](https://github.com/espressif/esp-matter/tree/release/v1.3)
- [Connected Home over IP v1.3](https://github.com/espressif/connectedhomeip/tree/v1.3-branch)
- [Connected Home IP Documentation](https://project-chip.github.io/connectedhomeip-doc/index.html)

## ESP-IDF Setup

To set up the ESP-IDF development environment, the steps below are required:

**Step 1: Install Prerequisites**

```bash
sudo apt-get install git wget flex bison gperf python3 python3-pip python3-venv cmake ninja-build ccache libffi-dev libssl-dev dfu-util libusb-1.0-0
```

**Step 2: Get ESP-IDF**

```bash
mkdir -p ~/esp
cd ~/esp
git clone -b v5.2.3 --recursive https://github.com/espressif/esp-idf.git
```

**Step 3: Set up the Tools**

ESP-IDF provides a script `install.sh` that installs the required tools such as the compiler, debugger, Python packages,
etc.:

```bash
cd ~/esp/esp-idf
./install.sh all
```

**Step 4: Create get_idf Alias**

ESP-IDF provides a script `export.sh` that sets environment variables so the tools are usable from the command line. The
script sets IDF_PATH, updates PATH with ESP-IDF tools, verifies Python compatibility, and enables idf.py
auto-completion.

Create an alias for executing export.sh by adding the following line to `~/.bashrc` file:

```bash
alias get_idf='. $HOME/esp/esp-idf/export.sh'
```

**Step 5: Refresh Configuration**

Restart your terminal session or run:

```bash
source ~/.bashrc
```

Now, running `get_idf` will set up or refresh the esp-idf environment in any terminal session.

![get_idf output](image17.png){ thumbnail="true" width="300" }

## ESP-Matter Setup

**Step 1: Install Prerequisites**

```bash
sudo apt-get install git gcc g++ pkg-config libssl-dev libdbus-1-dev \
 	libglib2.0-dev libavahi-client-dev ninja-build python3-venv python3-dev \
 	python3-pip unzip libgirepository1.0-dev libcairo2-dev libreadline-dev
```

Refer to the [Matter Build Guide](https://github.com/espressif/connectedhomeip/blob/v1.3-branch/docs/guides/BUILDING.md)
for more details.

**Step 2: Clone ESP-Matter Repository**

It includes esp-matter SDK and tools (e.g., CHIP-tool, CHIP-cert, ZAP).

```bash
cd ~/esp/
git clone -b release/v1.3 --recursive https://github.com/espressif/esp-matter.git
```

**Step 3: Bootstrap ESP-Matter**

```bash
cd esp-matter
./install.sh
```

![Bootstrapping ESP-Matter](image14.png){ thumbnail="true" width="300" }

**Step 4: Create get_matter Alias**

The script `~/esp/esp-matter/export.sh` configures the environment. Create an alias for executing it by adding the
following line to `~/.bashrc` file:

```bash
alias get_matter='. $HOME/esp/esp-matter/export.sh'
```

**Step 5: Refresh Configuration**

Restart your terminal session or run:

```bash
source ~/.bashrc
```

Now, running `get_matter` will set up or refresh the esp-matter environment in any terminal session.

## Project Setup and Deployment

To create, build, and deploy a new project for ESP32, the steps below are required.

**Step 1: Set Up ESP-IDF Environment**

Run the alias, created during the development setup, to initialise the ESP-IDF environment in the current terminal
session:

```bash
get_idf
```

**Step 2: Create a New Project**

ESP-IDF provides the [idf.py](https://docs.espressif.com/projects/esp-idf/en/v5.2.3/esp32/api-guides/tools/idf-py.html)
command-line tool as a front-end for managing project builds, deployment, debugging, and other tasks, simplifying the
workflow significantly. It integrates several essential tools, including CMake for project configuration, Ninja for
building, and esptool.py for flashing the target device.

```bash
idf.py create-project <project name>
```

**Step 3: Set ESP32 as the target device**

This command creates a new `sdkconfig` file in the root directory of the project. This configuration file is modified
via idf.py menuconfig to customise the configuration of the project:

```bash
idf.py set-target esp32
```

**Step 4: Open the Project in CLion IDE**

Create a toolchain named **ESP-IDF** with the environment file set to `esp-idf/script.sh`:

![Toolchain setup](image10.png){ thumbnail="true" width="300" }

Set up a CMake profile with the following Matter environmental variables:

```bash
ESP_MATTER_PATH=/home/albert/esp/esp-matter;ZAP_INSTALL_PATH=/home/albert/esp/esp-matter/connectedhomeip/connectedhomeip/.environment/cipd/packages/zap;PATH=/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin:/usr/games:/usr/local/games:/snap/bin:/snap/bin:/home/albert/.local/share/JetBrains/Toolbox/scripts:/home/albert/esp/esp-matter/connectedhomeip/connectedhomeip/.environment/cipd/packages/pigweed
```

![CMake profile configuration](image26.png){ thumbnail="true" width="300" }

Structure of a new ESP-IDF project:

![Structure of a new ESP-IDF project](image12.png){ thumbnail="true" width="300" }

**Step 5: Build the Project**

```bash
idf.py build
```

**Step 6: Determine Serial Port**

Connect the ESP32 board to the computer and check under which serial port the board is visible. Serial ports have the
following naming patterns: `/dev/tty`.

**Step 7: Flash Project to Target**

```bash
idf.py -p <PORT> flash
```

**Step 8: Launch IDF Monitor**

Use the monitor application and exit using `CTRL+]`:

```bash
idf.py -p <PORT> monitor
```
