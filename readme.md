# Easy Linux on Windows 10

Here's a little trick I set in order to run Linux applications (both command line and graphical) on my Windows 10.
With last updates, Windows 10 embeds a Linux bash. You just need to activate it.

**Note :** This will only work with 64-bits version of Windows 10.

## Activate Bash

First make sure you have the *Windows 10 Anniversary Update* installed (`Parameters > System > System Information`, the build number should be greater than `14393`).

Open the Settings app and go to `Update & Security > For Developers`. Then activate `Developper Mode` option.

Next, open the Control Panel and go to `Programs > Turn Windows Features On or Off` under `Programs and Features`. Enable the `Windows Subsystem for Linux (Beta)` option in the list. When prompted to reboot, do it.

You can know run Bash (<kbd>Windows</kbd> + <kbd>S</kbd> and then search for `Bash`). The first time you run it, it will ask you to accept the Terms of Service and will install Ubuntu from the Windows Store. Follow the install process.

You can now use this bash like a regular Ubuntu (`sudo apt-get update`, `sudo apt-get upgrade`, etc.).

## Run graphical applications

We'll now bind our Bash to a X server. We'll use Xming which is a X server for Windows. You can download Xming from its [SourceForge webpage](https://sourceforge.net/projects/xming/). Install it and remember the install location.

Now, we'll modify our `.bashrc` file (in your home directory) to run the Xming server each time you launch your Bash. We'll also set up a trick to kill the Xming server when you exit your Bash.

Open your `.bashrc` file with your preferred text editor.

```bash
nano ~/.bashrc
```

Append the following content to the file. Modify the path to `Xming.exe` with the according install location.

```bash
# Running Xming server 
# Redirecting error messages to nowhere, just a fucking windows error
/mnt/c/Program\ Files\ \(x86\)/Xming/Xming.exe :0 -clipboard -multiwindow -unixkill -winkill 2> /dev/null &

# Connecting to Xming
export DISPLAY=localhost:0.0

# Closing Xming when exiting bash
trap 'cmd.exe /C TASKKILL /F /IM Xming.exe' EXIT SIGKILL > /dev/null
```

Save the file and restart Bash. You can notice that Xming starts automagically. When executing the `exit` command, it will kill both Bash and Xming.

