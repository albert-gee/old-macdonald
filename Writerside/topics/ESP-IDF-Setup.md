# ESP-IDF Setup

## Overview

This section demonstrates how to set up the ESP-IDF development environment for building and running applications on
ESP32 SoCs.

This project uses [ESP-IDF v5.4](https://github.com/espressif/esp-idf/commits/release/v5.4/), locked to
commit [473771b](https://github.com/espressif/esp-idf/commit/473771bc14b7f76f9f7721e71b7ee16a37713f26).

~~To set up the [ESP-IDF v5.2.1](https://github.com/espressif/esp-idf/tree/v5.2.1) development environment, the steps
below are required:~~

## Step 1: Install Prerequisites

```Bash
sudo apt-get install git wget flex bison gperf python3 python3-pip python3-venv cmake ninja-build ccache libffi-dev libssl-dev dfu-util libusb-1.0-0
```

## Step 2: Get ESP-IDF

```Bash
mkdir -p ~/esp
cd ~/esp
// git clone -b v5.2.1 --recursive https://github.com/espressif/esp-idf.git
git clone -b release/v5.3 --recursive https://github.com/espressif/esp-idf.git esp-idf-5.3
cd ~/esp/esp-idf-5.3
git checkout fb25eb02ebcf78a78b4c34a839238a4a56accec7
```

## Step 3: Set up the Tools

ESP-IDF provides a script `install.sh` that installs the required tools such as the compiler, debugger, Python packages,
etc.:

```Bash
./install.sh all
```

## Step 4: Create get_idf Alias

ESP-IDF provides a script `export.sh` that sets environment variables so the tools are usable from the command line. The
script sets IDF_PATH, updates PATH with ESP-IDF tools, verifies Python compatibility, and enables idf.py
auto-completion.

Create an alias for executing export.sh by adding the following line to `~/.bashrc` file:

```Bash
alias get_idf='. $HOME/esp/esp-idf-5.3/export.sh'
```

## Step 5: Refresh Configuration

Restart your terminal session or run:

```Bash
source ~/.bashrc
```

Now, running `get_idf` will set up or refresh the esp-idf environment in any terminal session.

![get_idf output](esp-idf-v5-4.png){ thumbnail="true" width="300" }
