# MATLAB Windows Version on Linux System

A step-by-step guide to install and run the MATLAB Windows version on a Linux system using [Wine](https://www.winehq.org).

### 1. Install Wine
First, install **[Wine](https://gitlab.winehq.org/wine/wine)**, the compatibility layer required to execute Windows software.
* **Fedora:** `sudo dnf install wine`
* **Ubuntu/Debian:** `sudo apt install wine`

### 2. Install Winetricks
Next, install **[Winetricks](https://github.com/Winetricks/winetricks)**, a helper script necessary to download closed-source Windows libraries that MATLAB needs to run.
* **Fedora:** `sudo dnf install winetricks`
* **Ubuntu/Debian:** `sudo apt install winetricks`

### 3. Install corefonts and vcrun
MATLAB's graphical interface relies on Microsoft fonts, and its underlying engine requires Visual C++ runtime libraries. Use Winetricks to install them:
```bash
winetricks corefonts vcrun2015 vcrun2022
```
> **Why are these libraries necessary?**
> If you try to run the setup without them, you will face two common errors:
> 1. **Unreadable UI with blank squares instead of text:** The MATLAB installer and interface are hardcoded to use specific Microsoft fonts (like Arial). Since Linux does not have these natively, Wine fails to render the text, resulting in empty boxes or completely blank spaces instead of letters. This is fixed by **[corefonts](https://sourceforge.net/projects/corefonts/)** (Microsoft TrueType Core Fonts), which downloads the original Microsoft fonts from a SourceForge mirror so the graphical interface becomes actually readable.
> 2. **Missing `.dll` terminal errors (e.g., MSVCP140.dll) and engine crashes:** The MATLAB C++ engine requires specific Windows runtime libraries to start. Without them, the program will simply crash in the terminal. This is fixed by **[vcrun](https://learn.microsoft.com/en-us/cpp/windows/latest-supported-vc-redist)** (Microsoft Visual C++ Redistributables), which fetches these libraries directly from Microsoft's servers.
> 
> *(Depending on the exact MATLAB release year, you might need to adjust the `vcrun` version, but 2015 and 2022 cover most modern releases).*

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
