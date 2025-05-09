<show-structure/>

# ESP-IDF Setup

## Overview

This section demonstrates how to set up the [ESP-IDF](Espressif.md#esp-idf-framework) development environment for
building and running applications on ESP32 SoCs.

This project uses [ESP-IDF v5.3.2](https://docs.espressif.com/projects/esp-idf/en/v5.3.2/), locked to
commit [fb25eb0](https://github.com/espressif/esp-idf/commit/fb25eb02ebcf78a78b4c34a839238a4a56accec7).

## Step 1: Install Prerequisites

The following packages are required for the ESP-IDF development environment:

```Bash
sudo apt-get install git wget flex bison gperf python3 python3-pip python3-venv cmake ninja-build ccache libffi-dev libssl-dev dfu-util libusb-1.0-0
```

`idf.py` uses `#!/usr/bin/env python` in its shebang, which expects the `python` command to be available. On Ubuntu
20.04 and later, `python` is not included by default, as Python 2 has been deprecated. Check with:

```bash
which python
which python3
```

If `python` is missing, a symlink can be created to point to `python3`:

```bash
sudo ln -s /usr/bin/python3 /usr/bin/python
```

## Step 2: Get ESP-IDF

```Bash
mkdir -p ~/esp
cd ~/esp
git clone -b release/v5.3 --recursive https://github.com/espressif/esp-idf.git esp-idf-5.3
cd ~/esp/esp-idf-5.3
git checkout fb25eb02ebcf78a78b4c34a839238a4a56accec7
```

## Step 3: Set up the Tools

For newer versions of Ubuntu, the `install.sh` script may not work as expected because `

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

![get_idf output](esp-idf-v5-4.jpg){ thumbnail="true" width="500" }
