# ESP-IDF Setup

To set up the [ESP-IDF v5.2.3](https://github.com/espressif/esp-idf/tree/v5.2.3) development environment, the steps 
below are required:

## Step 1: Install Prerequisites

```bash
sudo apt-get install git wget flex bison gperf python3 python3-pip python3-venv cmake ninja-build ccache libffi-dev libssl-dev dfu-util libusb-1.0-0
```

## Step 2: Get ESP-IDF

```bash
mkdir -p ~/esp
cd ~/esp
git clone -b v5.2.3 --recursive https://github.com/espressif/esp-idf.git
```

## Step 3: Set up the Tools

ESP-IDF provides a script `install.sh` that installs the required tools such as the compiler, debugger, Python packages,
etc.:

```bash
cd ~/esp/esp-idf
./install.sh all
```

## Step 4: Create get_idf Alias

ESP-IDF provides a script `export.sh` that sets environment variables so the tools are usable from the command line. The
script sets IDF_PATH, updates PATH with ESP-IDF tools, verifies Python compatibility, and enables idf.py
auto-completion.

Create an alias for executing export.sh by adding the following line to `~/.bashrc` file:

```bash
alias get_idf='. $HOME/esp/esp-idf/export.sh'
```

## Step 5: Refresh Configuration

Restart your terminal session or run:

```bash
source ~/.bashrc
```

Now, running `get_idf` will set up or refresh the esp-idf environment in any terminal session.

![get_idf output](image17.png){ thumbnail="true" width="300" }
