# xWSL.cmd

- Simplicity - One command to set up a desktop environment in WSL1 with all the quirks taken care of
- Runs on Windows Server 2019 or Windows 10 Version 1803 (or newer)
- Ubuntu Linux 20.04 and custom themed XFCE 4.14 for a smooth user experience
- xRDP Display Server, no additional X Server downloads required
- RDP Audio playback enabled (YouTube playback in browser works)

<img width="641" alt="xWSL1" src="https://user-images.githubusercontent.com/33142753/82766604-ea801680-9df6-11ea-9045-6ab9540a5424.png">

xWSL is accessible from anywhere on your network, you connect to it via Microsoft's Remote Desktop Client (mstsc.exe)

**INSTRUCTIONS:  From an elevated CMD.EXE prompt change to your desired install directory and type/paste the following command:**

```
PowerShell -executionpolicy bypass -command "wget https://github.com/DesktopECHO/xWSL/raw/master/xWSL.cmd -UseBasicParsing -OutFile xWSL.cmd ; .\xWSL.cmd"
```
OR (For Windows 10 2004 or higher)

```
PowerShell -executionpolicy bypass -command "wget https://github.com/DesktopECHO/xWSL/raw/master/xWSL2.cmd -UseBasicParsing -OutFile xWSL.cmd ; .\xWSL.cmd"
```

You will be asked a few questions:

```
xWSL for Ubuntu 20.04
Enter a unique name for the distro or hit Enter to use default [xWSL]: 
Enter port number for xRDP traffic or hit Enter to use default [3399]: 
Enter port number for SSHd traffic or hit Enter to use default [3322]: 
xWSL (xWSL) To be installed in: C:\Users\danm\xWSL
```

Near the end of the script you will be prompted to create a non-root user.  This user will be automatically added to sudo'ers.

```
Enter name of xWSL user: danm
Enter password: ********
SUCCESS: The scheduled task "xWSL-Init" has successfully been created.

TaskPath                                       TaskName                          State
--------                                       --------                          -----
\                                              xWSL-Init                         Ready

  Start: Sun 05/24/2020 @ 20:08:00.84
    End: Sun 05/24/2020 @ 20:16:48.87

 Installation Complete.  xRDP server listening on port 3399 and SSH on port 3322
 Links for GUI and Console sessions have been placed on your desktop.
 Auto-launching RDP Desktop Session in 5 seconds...

C:\Users\danm>
```

Upon completion you'll be logged into an attractive and fully functional XFCE4 desktop.  A scheduled task is created that runs at login to start xWSL. 

   **If you prefer to start xWSL at boot (like a service) do the following:**

   - Right-click the task in Task Scheduler, click properties
   - Click the checkboxes for **Run whether user is logged on or not** and **Hidden** then click **OK**
   - Enter your Windows credentials when prompted

   Reboot your PC.  xWSL will automatically start at boot, no need to login to Windows.
   
**Convert to WSL2 Virtual Machine:** - For xWSL on WSL1
-  xWSL will convert easily to WSL2.  Only one additional adjustment is necessary; change the hostname in the .RDP connection file to point at the WSL2 instance.  First convert the instance:
    ```wsl --set-version [DistroName] 2```
- Assuming we're using the default distro name of ```xWSL``` (use whatever name you assigned to the distro)  Right click the .RDP file in Windows, click Edit.  Change the Computer name to your Windows hostname plus **```-xWSL.local```**  Your WSL2 instance resolves seamlessly using multicast DNS  
- For example, if the current value is ```LAPTOP:3399```, change it to ```LAPTOP-xwsl.local:3399``` and save the RDP connection file.  

**Quirks Addressed and other interesting tidbits:**
- WSL1 Has issues with the latest libc6 library.  The package is being held until fixes from MS are released over Windows Update.  Unmark and update libc6 after MS releases the update.
- WSL1 Doesn't work with PolicyKit.  Pulled-in GKSU and dependencies to allow runing GUI apps with elevated rights.  
- Rolled back and held xRDP until the version shipped in Ubuntu is better-behaved (xrdp-chansrv high CPU %)
- Current version of Chrome or Firefox does not work in WSL1 so Mozilla Seamonkey was included as a stable and maintaned browser
- Installed image consumes less than 2GB of disk
- Symlinked Windows fonts in Linux which make for a very nice looking XFCE4 session using Segoe UI and Consolas
- Password-saving magic for RDP connections performed safely using Windows credential store and Powershell ConvertTo-SecureString 
