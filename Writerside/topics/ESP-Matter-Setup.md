<show-structure/>

# ESP-Matter Setup

This section demonstrates how to set up the [Espressif's SDK for Matter](Espressif.md#esp-idf-framework) for building 
Matter applications on ESP32 SoCs.

This project uses Espressif's SDK for Matter [v1.4](https://github.com/espressif/esp-matter/tree/release/v1.4), locked to
commit [30af618](https://github.com/espressif/esp-matter/commit/30af618a6e962623a0098ad6a33b468f33dc49c7).

Prerequisites:
- [ESP-IDF](ESP-IDF-Setup.md) development environment is set up.

## Step 1: Install Prerequisites

```Bash
sudo apt-get install git gcc g++ pkg-config libssl-dev libdbus-1-dev \
 	libglib2.0-dev libavahi-client-dev ninja-build python3-venv python3-dev \
 	python3-pip unzip libgirepository1.0-dev libcairo2-dev libreadline-dev
```

Refer to the [Matter Build Guide](https://github.com/espressif/connectedhomeip/blob/v1.3-branch/docs/guides/BUILDING.md)
for more details.

## Step 2: Clone ESP-Matter Repository

It includes esp-matter SDK and tools (e.g., CHIP-tool, CHIP-cert, ZAP).

```Bash
cd ~/esp/
git clone -b release/v1.4 --recursive https://github.com/espressif/esp-matter.git esp-matter-1.4
cd esp-matter-1.4
git checkout 30af618a6e962623a0098ad6a33b468f33dc49c7
```

## Step 3: Bootstrap ESP-Matter

```Bash
get_idf
~/esp/esp-matter-1.4/install.sh
```

![Bootstrapping ESP-Matter](image14.png){ thumbnail="true" width="500" }

## Step 4: Create get_matter Alias

The script `~/esp/esp-matter/export.sh` configures the environment. Create an alias for executing it by adding the
following line to `~/.bashrc` file:

```Bash
alias get_matter='. $HOME/esp/esp-matter/export.sh'
```

## Step 5: Refresh Configuration

Restart your terminal session or run:

```Bash
source ~/.bashrc
```

Now, running `get_matter` will set up or refresh the ESP-Matter environment in any terminal session.

## References

- [Connected Home over IP v1.3](https://github.com/espressif/connectedhomeip/tree/v1.3-branch)
- [Connected Home IP Documentation](https://project-chip.github.io/connectedhomeip-doc/index.html)