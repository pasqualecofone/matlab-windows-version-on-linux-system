# MATLAB Windows Version on Linux System

A step-by-step guide to install and run the MATLAB Windows version on a Linux system using Wine.

### 1. Install Wine
First, install **Wine**, the compatibility layer required to execute Windows software.
* **Fedora:** `sudo dnf install wine`
* **Ubuntu/Debian:** `sudo apt install wine`

### 2. Install Winetricks
Next, install **Winetricks**, a helper script necessary to download closed-source Windows libraries that MATLAB needs to run.
* **Fedora:** `sudo dnf install winetricks`
* **Ubuntu/Debian:** `sudo apt install winetricks`

### 3. Install corefonts and vcrun
MATLAB's graphical interface relies on Microsoft fonts, and its underlying engine requires Visual C++ runtime libraries. Use Winetricks to install them:
```bash
winetricks corefonts vcrun2015 vcrun2022
```
*(Depending on the exact MATLAB release year, you might need to adjust the `vcrun` version, but 2015 and 2022 cover most modern releases).*

#### 4. Configure Wine to Windows 10
Modern versions of MATLAB will refuse to install or run if the operating system is reported as anything older than Windows 10. Open the Wine configuration utility:
```bash
winecfg
```
*When the configuration window opens, look at the bottom of the **Applications** tab. Change the **Windows Version** dropdown to **Windows 10**. Click **Apply**, then **OK**.*

### 5. Run the MATLAB Setup
Navigate to the directory where your MATLAB installation files are located, and launch the setup executable:
```bash
cd /path/to/matlab/installer/folder
wine setup.exe
```
From here, simply follow the standard on-screen instructions, insert your license keys, and complete any other required steps exactly as you would during a normal Windows installation.
