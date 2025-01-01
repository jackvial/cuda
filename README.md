# CUDA Toolkit Installation Guide (Ubuntu 24.04)

This guide walks through installing the CUDA Toolkit while **skipping** the included NVIDIA driver, useful if you already have a compatible driver installed from your Linux distribution's package manager (e.g., `apt`, `dnf`, etc.). This is based on a real scenario where the system already had driver version **550.120** (newer than the one bundled with CUDA 12.4).

---

## 1. Check Your Existing Driver

Before installing any new CUDA Toolkit:

1. Run `nvidia-smi` to confirm your current driver version:
```bash
nvidia-smi
```

Ensure the driver version meets the minimum requirement for the CUDA version you want. For CUDA 12.4, the required driver version is at least R550 (e.g., 550.00 or newer). If you see something like:

```bash
+-----------------------------------------------------------------------------------------+
| NVIDIA-SMI 550.120                Driver Version: 550.120        CUDA Version: 12.4     |
|
```

then your system driver is 550.120, which is definitely compatible with CUDA 12.4.
Note: If your driver is older than what CUDA 12.4 requires, you may need to update it (via your package manager or with the .run file) before installing the toolkit.

## 2. Download the CUDA Toolkit Installer
Download the CUDA .run file from NVIDIA's developer site:

```bash
wget https://developer.download.nvidia.com/compute/cuda/12.4.0/local_installers/cuda_12.4.0_550.54.14_linux.run
```

This file typically includes:

A bundled NVIDIA driver (in this case, 550.54.14)
The CUDA 12.4 Toolkit and associated libraries

## 3. Run the Installer and Skip the Driver
When you execute the .run file, you'll see a warning if you already have a driver installed via your package manager:

```bash
Existing package manager installation of the driver found. It is strongly recommended
that you remove this before continuing.
Abort
Continue
```

In our scenario, we chose to Continue, because the existing driver was both newer and compatible. Execute:

```bash
sudo sh cuda_12.4.0_550.54.14_linux.run
```

Key Step: During the interactive process, deselect the "Driver" (550.54.14) option. The final selection should look like this:

```bash
CUDA Installer
 - [ ] Driver
      [ ] 550.54.14
 - [X] CUDA Toolkit 12.4
    + [X] CUDA Libraries 12.4
    + [X] CUDA Tools 12.4
    + [X] CUDA Compiler 12.4
   [X] CUDA Demo Suite 12.4
   [X] CUDA Documentation 12.4
 - [ ] Kernel Objects
      [ ] nvidia-fs
   Options
   Install
```

The installer will show a "WARNING: Incomplete installation!" message if you skip the driver:

```bash
***WARNING: Incomplete installation! This installation did not install the CUDA Driver.
A driver of version at least 550.00 is required for CUDA 12.4 functionality to work.
```

You can safely ignore this if you are certain your existing driver is new enough. We had Driver 550.120, which meets the requirement.

You can safely ignore this if you are certain your existing driver is new enough. We had Driver 550.120, which meets the requirement.
## 4. Post-Installation Steps
### 4.1 Update Environment Variables
Add the CUDA 12.4 toolkit to your PATH and LD_LIBRARY_PATH. For instance, if you installed into /usr/local/cuda-12.4, update your shell's configuration file (e.g., ~/.bashrc or ~/.zshrc):

```bash
echo 'export PATH=/usr/local/cuda-12.4/bin:$PATH' >> ~/.bashrc
echo 'export LD_LIBRARY_PATH=/usr/local/cuda-12.4/lib64:$LD_LIBRARY_PATH' >> ~/.bashrc
```

Reload your bash configuration:

```bash
source ~/.bashrc
```

### 4.2 Verify the Installation
Check nvcc version:

```bash
nvcc --version
```

It should report something like:

```bash
nvcc: NVIDIA (R) Cuda compiler driver
Cuda compilation tools, release 12.4, V12.4.99
```

Check nvidia-smi:
```
nvidia-smi
```

It should still show your original driver version (e.g., 550.120) and also list CUDA 12.4.

## 5. Common Questions & Answers
- Q: Is it okay to skip the driver in the .run file?
- A: Yes, if your current driver meets or exceeds the version required for the CUDA release. For example, if you have 550.120 installed and the installer bundles 550.54.14, you're already at a newer patch in the same driver branch.
- Q: Why does the installer warn about "Incomplete installation"?
- A: Because NVIDIA expects you to install a driver if none is found or if the found one is older. In your case, you intentionally skipped it because you had a newer driver from your package manager.
- Q: What if I want to install the driver in the .run file?
- A: You could simply leave that checkbox selected, but be aware it may override your existing driver with a possibly older version. If you do want that driver, follow the instructions, but it's generally recommended to only keep one method of driver installation (package manager vs. .run file) to avoid conflicts.

## 6. Conclusion
By installing only the CUDA 12.4 toolkit and skipping the included (older) driver, you can:

- Retain the newer driver (550.120) already installed
- Gain immediate access to the CUDA 12.4 development tools
- Avoid potential driver conflicts or downgrades

Once everything is set up, you'll have a fully functional CUDA environment with your updated driver and the latest toolkit.
