---
ms.assetid: 5c34c78e-9ff7-477b-87f6-a31367cd3f8b
title: Device Portal for Desktop
description: Learn how the Windows Device Portal opens up diagnostics and automation on your Windows desktop.
---
# Device Portal for Desktop

Starting in Windows 10, Version 1604, additional developer features are available for desktop. These features are available only when Developer mode is enabled.

For info about how to enable Developer mode, see [Enable your device for development](../get-started/enable-your-device-for-development.md).

Device Portal lets you view diagnostic info and interact with your desktop over HTTP from your browser. You can use Device Portal to do the following:
- See and manipulate a list of running processes
- Install, delete, launch, and terminate apps
- Change Wi-Fi profiles, view signal strength, and see ipconfig
- View live graphs of CPU, memory, I/O, network, and GPU usage
- Collect process dumps
- Collect ETW traces 
- Manipulate the isolated storage of sideloaded apps

## Set up device portal on Window Desktop

### Turn on device portal
In the Developer Settings menu, with Developer Mode enabled, you can enable Device Portal.  

When you enable Device Portal, you must also create a username and password for Device Portal.  Do not use your MSA or other Windows credentials.  

After Device Portal is enabled, you will see links to it at the bottom of the Settings section.  Take note of the port number applied to the end of the URL: this port number is randomly generated when Device Portal is enabled, but should remain consistent between reboots of the desktop. If you'd like to set the port numbers manually so they remain permanent, see [Setting port numbers](device-portal-desktop.md#setting-port-numbers)

You can choose from 2 ways to connect to Device Portal: local host and over the local network (including VPN).

**To connect to Device Portal**

1. In your browser, enter the address shown here for the connection type you're using.

    - Localhost: `http://127.0.0.1:PORT` or `http://localhost:PORT`

    Use this address to view Device Portal locally.
    
    - Local Network: `https://<The IP address of the desktop>:PORT`

    Use this address to connect over a local network.

HTTPS is required for authentication and secure communication.

If you are using Device Portal in a protected environment, like a test lab, where you trust everyone on your local network, have no personal information on the device, and have unique requirements, you can disable authentication. This enables unencrypted communication, and allows anyone with the IP address of your computer to control it.

## Device Portal pages

Device Portal on desktop provides the standard set of pages. For detailed descriptions, see [Windows Device Portal overview](device-portal.md).

- Apps
- Processes
- Performance
- Debugging
- Event Tracing for Windows (ETW)
- Performance tracing
- Devices
- Networking
- App File Explorer 

## Setting port numbers

If you would like to select port numbers for Device Portal (such as 80 and 443), you can set the following regkeys:
    Under HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\WebManagement\Service
    HttpPort: A required DWORD.  Contains the port number that Device Portal will listen for HTTP connections on. 
    HttpsPort: A required DWORD.  Contains the port number that Device Portal will listen for HTTPS connections on. 

## Failure to install Developer Mode package
Sometimes, due to network or compatibility issues, Developer Mode won't install correctly.  The Developer Mode package is required for remote deployment - Device Portal and SSH - but not for local development.  

### Failed to locate the package
"Developer mode package couldn’t be located in Windows Update. Error Code 0x001234 Learn more"   
This error may occur due to a network connectivity problem, Enterprise settings, or the package may be missing. 
To fix this issue:
1. Ensure your computer is connected to the internet. 
2. If you are on a domain-joined computer, speak to your network administrator. 
3. Check for Windows updates in the Settings > Updates and Security > [Windows Updates](ms-settings:windowsupdate).
4. Verify that the Windows Developer Mode package is present in Settings > System > Apps & Features > [Manage optional features](ms-settings:optionalfeatures) > Add a feature. If it is missing, Windows cannot find the correct package for your computer. 

After doing any of the above steps disable then re-enable Developer Mode to verify the fix. 


### Failed to install the package
"Developer mode package failed to install. Error code 0x001234  Learn more"
This error may occur due to incompatibilities between your build of Windows and the Developer Mode package. 
To fix this issue:
1. Check for Windows updates in the Settings > Updates and Security > [Windows Updates](ms-settings:windowsupdate).
2. Reboot your computer to ensure all updates are applied. 


<!--HONumber=Apr16_HO2-->


