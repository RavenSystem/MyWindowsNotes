# My Notes about how to Install and Configure Windows 11

[![Donate](https://img.shields.io/badge/donate-PayPal-blue.svg)](https://paypal.me/ravensystem)

[ [Español](README_ES.md) ]

[ [GitHub Source](https://github.com/RavenSystem/MyWindowsNotes) ]

These are just my personal notes on how to install and configure a Windows 11 PC to take full advantage of all its power, for gaming, VR, heavy workflows...

**I am not responsible for any possible damage or data loss that may occur.**

## Create Installation USB to install without TPM 2.0 requirement
- A Windows PC (Real or virtual) and a USB stick with at least 8GB capacity are required.
- Download the Windows 10 and Windows 11 media creators from [Windows Installation Media Creator](https://support.microsoft.com/en-us/windows/create-windows-installation-media-99a58364-8c02-206f-aa6f-40c3b507420d)
- Use the **Windows 11** Media Creator and create an installation USB.
- Copy the `\sources\install.esd` file from the pendrive to the computer.
- Use the **Windows 10** Media Creator and create an installation USB.
- Replace the `\sources\install.esd` file on the pendrive with the one we previously copied to the computer.
- The pendrive is now ready to boot the computer from USB and install Windows.

## Create BootCamp compatible ISO for Intel Macs
- [BootCamp ISO creation utility](https://github.com/RavenSystem/BootCampW11Installer)

## Install Windows
- Boot from the USB.
  - For Intel Macs, use the BoorCamp Installation Wizard with the corresponding ISO file.
- Do not enter the Windows activation key here (It is better to activate Windows once it is installed and configured, although you can leave it unactivated and everything will work without restrictions, except for the part of customizing the colors and the desktop background).
- Once installed, the initial setup wizard will run, asking for the country:
  - Press `Shift + F10` to open a Terminal.
  - Run the following command (If your current keyboard layout doesn't allow you to type the `\` slash, you can copy and paste one from the Terminal itself with the mouse):
```shell
OOBE\BYPASSNRO
```
   - When the computer restarts, disconnect the network cable.
      - You can also take it offline by opening a Terminal again (`Shift + F10`) and running:
```shell
ipconfig /release
```
   - Follow the steps of the wizard, indicating that you don't have an Internet connection and creating a local user account.
   - In the questions at the end of the configuration, about permissions and data collection, always select the option not to send or share (the second of the two options).
- Once everything is finished, connect it to the Internet.
- Open Settings -> Windows Update -> Search for updates, and install them. And reboot, even if it doesn't ask for it.

## Remove unnecessary software
(Source: [https://github.com/Raphire/Win11Debloat](https://github.com/Raphire/Win11Debloat))
- Open a Terminal as Administrator (Right-click on its icon and Run as administrator) and run:
```shell
& ([scriptblock]::Create((irm "https://win11debloat.raphi.re/"))) -RunDefaults -RemoveCommApps -RemoveW11Outlook -RemoveGamingApps -DisableDVR -DisableLockscreenTips -ClearStartAllUsers -RevertContextMenu -ShowHiddenFolders -HideTaskview
```
- Then, run:
```shell
& ([scriptblock]::Create((irm "https://win11debloat.raphi.re/")))
```
- Select option `3`.
- Check: Only show installed apps.
- Select:
  - Microsoft.Edge
  - Microsoft.GetHelp
  - Microsoft.MSPaint
  - Microsoft.Windows.DevHome
  - Microsoft.YourPhone
  - Microsoft.ZuneMusic
  - MicrosoftWindows.CrossDevice
- Press Confirm.

## Disable unnecessary functions and processes
(Source: [https://github.com/ChrisTitusTech/winutil](https://github.com/ChrisTitusTech/winutil))
- Open a Terminal as Administrator (Right-click on its icon and Run as administrator) and run:
```shell
irm "https://christitus.com/win" | iex
```
- A window will open: Click on "Tweaks", then on "Standard", and finally on "Run Tweaks" at bottom.
- Wait for it to indicate "Tweaks finished" at the top right and close it.

## Enable automatic user login (Autologin)
- Open a Terminal and run:
```shell
regedit
```
- Go to the key:
```
HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows NT\CurrentVersion\PasswordLess\Device
```
- Set the value to `0` (Default is `2`) of:
```
DevicePasswordLessBuildVersion
```
- Close the Registry Editor.
- In the Terminal, run:
```shell
netplwiz
```
- Disable the option that requires users to enter a password and follow the on-screen instructions.

## Disable memory integrity
- Increases the computer's performance, in exchange for having the Windows 10 security level.
- Open Settings -> Privacy and security -> Windows Security.
- Click: Open Windows Security.
- Open: Device security -> Core isolation details.
  - Disable: Memory integrity.
  - Disable: Local security authority (LSA) protection.
- Restart the computer.
- Open a Terminal and run:
```shell
msinfo32
```
- Select: System summary.
  - On the right, search for: Virtualization-based security.
  - If enabled or activated:
    - In Terminal, run:
```shell
bcdedit /set hypervisorlaunchtype off
```
  - Restart your computer.
    - Note: To revert this change, open a Terminal and run:
```shell
bcdedit /set hypervisorlaunchtype on
```

## Minimize the impact of antivirus
- Open Settings -> Privacy & security -> Windows Security.
- Click: Open Windows Security.
- Open: Device security -> Virus and threat protection.
- Under: Virus and threat protection settings, click: Manage settings.
  - Disable everything except: Real-time protection.
  - Optional: At the bottom, under Exclusions, click: Add or remove exclusions.
    - Click on + Add exclusion.
    - Folder: `C:\Program Files (x86)\Steam\`
    - Add any other folders that contain software that needs the full power of the system.

## Disable system components from running in the background
- Open Settings -> System -> System components.
- For each of the components, click the 3 dots icon `···` on the right -> Advanced options.
  - Set the Background component permissions option to Never.

## Disable some visual options
- Open Settings -> System -> Advanced system settings.
- Under Performance, click Settings.
- On the Visual Effects tab, select Customize.
  - Leave only checked:
    - Show window contents while dragging.
    - Show thumbnails instead of icons.
    - Smooth edges of screen fonts.
  - Click OK.

## Show the Maximum Performance power plan in the power options (Optional)
- Open a Terminal and run:
```shell
powercfg -duplicatescheme e9a42b02-d5df-448d-aa00-03f14749eb61
```

## Improved Power Plan
- Open a Terminal and run:
```shell
powercfg -attributes SUB_PROCESSOR PERFINCTHRESHOLD -ATTRIB_HIDE
powercfg -attributes SUB_PROCESSOR PERFCHECK -ATTRIB_HIDE
powercfg -attributes SUB_PROCESSOR CPMINCORES -ATTRIB_HIDE
powercfg -attributes SUB_PROCESSOR CPPERF -ATTRIB_HIDE
powercfg -attributes SUB_PROCESSOR IDLEPROMOTE -ATTRIB_HIDE
powercfg.cpl
```
- The recommended plan to maximize the computer's power is: **Balanced**.
- Click: Change plan settings (The settings only apply to the selected plan).
  - Click: Change advanced power settings.
    - Click: `Restore default values`.
    - Go to: Processor power management, and configure:
      - Processor performance increase threshold: `90%`
      - Processor performance time check interval: `12ms`
      - Processor performance core parking min cores: `0%`.
        - This option indicates the percentage of cores that will never be turned off.
      - Processor performance core parking parked performance state: `Deep performance state`.
      - Processor idle promote threshold: `100%`.
    - Click: OK.

## Prevent certain programs from changing the active power plan
- Some programs, such as SteamVR, change the active power plan, causing a loss of performance.
- Open a Terminal and run:
```shell
powercfg /l
```
- Copy the GUID of the plan to always leave active.
- Run:
```shell
gpedit.msc
```
- Select: Local Computer Policy -> Administrative Templates -> System -> Power Management
- On the right, double-click: Specify a custom active power plan.
- Select: Enabled.
- Paste the GUID of the plan in: Custom active power plan (GUID).
- Click: Apply.

## Configure the process manager to prioritize foreground processes
(Source: https://www.xbitlabs.com/win32priorityseparation-performance/)
- Open a Terminal and run:
```shell
regedit
```
- Go to the key:
```
HKEY_LOCAL_MACHINE\SYSTEM\ControlSet001\Control\PriorityControl
```
- Set the value to `1a` for general use or `24` Hexadecimal for RV use (Default is `2`) of:
```
Win32PrioritySeparation
```
- Close the Registry Editor.

## Disable the creation of restore points (Optional)
- Open Settings -> System -> System Protection.
  - Click Configure and select: Disable system protection.

## Disable using disk as RAM when needed (Optional)
- Only disable if your computer has plenty of RAM (16GB or more, depending on the software you're running).
- Open Settings -> System -> Advanced system settings.
- Under Performance, click Settings.
- Under the Advanced tab, click Change.
  - Uncheck: Automatically manage paging file size for all drives.
  - For each drive, select: No paging file, and click Set.
  - Click OK.

## NVIDIA: Control Panel
- Recommended driver version: 576.52
- All by default, except:
  - CUDA - System Memory Usage Policy: Do not use system memory as backup
  - Prerendered Frames for VR: Use 3D Application Settings
  - Power Control Mode: Maximum Performance Preferred
  - Shader Cache Size: 100 GB

## AMD: Adrenaline Software
- Tessellation: Disabled.
