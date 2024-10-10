
# Yocto Project: Building core-image-sato for QEMU ARM

This project demonstrates how to build a **Yocto Linux distribution** from scratch for the `qemuarm` target using **Bitbake**. The image used is **core-image-sato**, a graphical desktop environment for embedded devices.

## Requirements

Before you begin, ensure you have the following installed on your system:
- A Linux distribution (preferably Ubuntu 20.04 or later)
- Git
- Essential build tools: `gcc`, `g++`, `make`, etc.
- At least **50 GB** of free disk space
- Around **4-8 GB** of RAM
- QEMU for running the emulation (`qemu-system-arm`)

### Installing Dependencies

You can install the necessary dependencies with the following commands:

```bash
sudo apt-get update
sudo apt-get install gawk wget git-core diffstat unzip texinfo gcc-multilib      build-essential chrpath socat libsdl1.2-dev xterm
```

## Setup

### 1. Clone Yocto Project Sources

First, clone the required Yocto layers from the **poky** repository and other necessary layers.

```bash
git clone git://git.yoctoproject.org/poky
cd poky
git checkout kirkstone  # or any branch/tag you're using
```

### 2. Initialize the Build Environment

Set up the build environment by running the environment setup script:

```bash
source oe-init-build-env
```

This command will set up a default **build** directory and create the necessary environment for building Yocto images.

### 3. Configure `local.conf`

Edit `conf/local.conf` in the build directory to optimize the build process. You can adjust the following settings for better performance:

```bash
BB_NUMBER_THREADS = "4"  # Set this to the number of CPU cores available 
PARALLEL_MAKE = "-j 4"   # Also set this to match the number of cores
```

You can also add specific machine support by setting the machine type to `qemuarm`:

```bash
MACHINE = "qemuarm"
```

### 4. Build the Image

To build the **core-image-sato** for `qemuarm`, run the following command:

```bash
bitbake core-image-sato
```

This process may take several hours, depending on your hardware and network speed.

### 5. Running the Image

Once the build completes, you can run the image on **QEMU** with:

```bash
runqemu qemuarm
```

This command launches the image in an emulated ARM environment.

## Directory Structure

After building, your output files will be located in:

```
build/
├── conf/               # Configuration files (local.conf, bblayers.conf)
├── tmp/
│   ├── deploy/
│   │   ├── images/qemuarm/   # Final image files
│   ├── work/           # Temporary build files
│   └── cache/          # Cached files
```

## Files Produced

Key files generated during the build process:

- **rootfs.tar.bz2**: The root filesystem
- **core-image-sato-qemuarm.ext4**: The EXT4 filesystem image
- **zImage**: The Linux kernel image
- **u-boot**: Bootloader for ARM platforms

These files are located in the `tmp/deploy/images/qemuarm/` directory.

## Troubleshooting

### "No Space Left on Device" Error (ENOSPC)

![Screenshot from 2024-10-09 16-41-02](https://github.com/user-attachments/assets/424e7a62-922d-4690-ac1b-44e8e5cb7de9)

If you encounter an error similar to:

```bash
WatchManagerError: add_watch: cannot watch <path> WD=-1, Errno=No space left on device (ENOSPC)
```

This indicates a lack of available disk space. To resolve this:

- Free up disk space by deleting unnecessary files or directories.
- Use `bitbake -c cleansstate <recipe>` to clean up the build files.
- Increase the available disk space on the partition or move the build directory to a partition with more space.

### Common Build Errors

![Screenshot from 2024-10-10 16-05-31](https://github.com/user-attachments/assets/6c35c4c9-8d10-431e-91b1-10806c3e4f17)

- **BB_NUMBER_THREADS** and **PARALLEL_MAKE** should match your CPU core count for faster builds.
- Ensure you have sufficient disk space (at least **50 GB** is recommended).

```bash
bitbake -c cleanall binutils
bitbake -c cleansstate core-image-sato
bitbake core-image-sato
```

## Resources

For more information on Yocto and Bitbake, refer to the following resources:

- [Yocto Project Quick Start Guide](https://www.yoctoproject.org/docs/current/brief-yoctoprojectqs/brief-yoctoprojectqs.html)
- [Bitbake User Manual](https://www.yoctoproject.org/docs/current/bitbake-user-manual/bitbake-user-manual.html)

## Contribution
If you would like to contribute to myls, feel free to fork the repository and submit a pull request. For major changes, please open an issue first to discuss what you would like to change.

## License
(mahamedhamam15@gmail.com)
