---
author: awkoren
Description: Run the Desktop Converter App to convert a Windows desktop application (like Win32, WPF, and Windows Forms) to a Universal Windows Platform (UWP) app.
Search.Product: eADQiWindows 10XVcnh
title: Desktop App Converter
translationtype: Human Translation
ms.sourcegitcommit: a5ac8acdbb7480bb776cef6d1dffa303dab5a9e1
ms.openlocfilehash: 4095cfbe96f239afb5f3173e0a7f84e01d63452c

---

# <a name="desktop-app-converter"></a>Desktop App Converter

[Get the Desktop App Converter](https://aka.ms/converter)

The Desktop App Converter (DAC) is a tool that enables you to bring your existing desktop apps written for .NET 4.6.1 or Win32 to the Universal Windows Platform (UWP). You can run your desktop installers through the converter in an unattended (silent) mode and obtain an AppX package that you can install by using the Add-AppxPackage PowerShell cmdlet on your development machine.

The Desktop App Converter is available now in the [Windows Store](https://aka.ms/converter).

The converter runs the desktop installer in an isolated Windows environment using a clean base image provided as part of the converter download. It captures any registry and file system I/O made by the desktop installer and packages it as part of the output. The converter outputs an AppX with package identity and the ability to call a vast range of WinRT APIs.

## <a name="whats-new"></a>What's new

This section outlines changes between versions of the Desktop App Converter. 

### <a name="1122016-v101"></a>11/2/2016 (v1.0.1)

* Improved manifest schema validation. 
* Improved error messaging. 
* Added validation of supported Windows versions. 
* Bug fixes for registry filter tests.

### <a name="9142016-v10"></a>9/14/2016 (v1.0)

* The Desktop App Converter is now available for download in the [Windows Store](https://aka.ms/converter)! 
* Grab the latest Windows 10 Base Images (.wim) on the [Download Center](https://aka.ms/converterimages) for use with the DAC.
* With the store app, you can now use the new entry point *DesktopAppConverter.exe <arguments>* to run the converter from anywhere in an elevated command prompt or PowerShell window.  


### <a name="922016-v0125"></a>9/2/2016 (v0.1.25)

* Integrated the latest dotnet-computervirtualization NuGet package.
* Added newly introduced dependencies on common.dll.
* Several bug fixes.

### <a name="842016-v0124"></a>8/4/2016 (v0.1.24)

* Added support for auto-signing the converted apps produced by DAC for testing purposes. Check out the ```–Sign``` flag to give it a try. 
* Added warnings if any of the COM registrations in the virtual registry hive are not supported within the packaged AppX.  
* Added support for auto-detecting app dependencies on VC++ libraries and then converting them in to AppX manifest dependencies. Note that in order to sideload and test apps using VC++ runtime, you'll need to download the VCLib framework packages as outlined in the blog post [Using Visual C++ Runtime in a Centennial project](https://blogs.msdn.microsoft.com/vcblog/2016/07/07/using-visual-c-runtime-in-centennial-project). Locate the packages under the folder ```Program Files (x86)\Microsoft SDKs\Windows Kits\10\ExtensionSDKs\Microsoft.VCLibs.Desktop``` on your machine, navigate to the version you depend on (e.g., 11.0, 12.0, 14.0), and double click on the appropriate architecture package (x64, x86) to install it.
* Updated the manifest schema to align with the Windows 10 Anniversary Update (10.0.14393.0). 
* Several bug fixes and improved output layout. 

### <a name="772016-v0122"></a>7/7/2016 (v0.1.22)

* Added support for auto detecting shell extensions from your desktop application and declaring them in the AppXManifest for your UWP package. To learn more about the desktop extensions, see [**Converted desktop app extensions**](desktop-to-uwp-extensions.md). 
* Improved AppExecutable detection for a large set of apps. 

### <a name="6162016-v0120"></a>6/16/2016 (v0.1.20)

* Fixes issues blocking successful conversions on the latest Windows 10 Insider Preview builds. 
* Replaced ```–CreateX86Package``` with ```–PackageArch```, which allows you to specify the architecture for the generated package. 

### <a name="682016"></a>6/8/2016

* Added support for generating x86 appx packages on AMD64 host machines running the converter.
* Reduced disk space usage by removing any previously expanded base images.
* Added support for cleaning up temporary files and any unnecessary base images.
* Improved support for detecting file type and protocol associations.
* Improved logic to detect the AppExecutable property for a large set of apps.
* Added support for providing additional –InstallerArguments for MSI based installers.
* Bug fixes for any PathTooLongException errors during the conversion process.

### <a name="5122016"></a>5/12/2016

- Restored support for Pro edition of Windows. 
- Converter ```-Setup``` flag now enables Windows Containers feature and handles base image expansion. Run the following from an elevated PowerShell prompt to do one time setup: ```PS C:\> .\DesktopAppConverter.ps1 -Setup -BaseImage BaseImage-12345.wim -Verbose```
- Added auto-detection of app install path and moving application root outside of VFS to reduce any unnecessary file system redirections at runtime.
- Added auto-detection of the expanded base image as part of the conversion process.
- Added auto-detection for file type associations and protocols.
- Improved logic to detect Start Menu shortcut.
- Improved file system filtering to retain app installed MUI files.
- Updated the minimum supported desktop version (10.0.14342.0) in the manifest.

## <a name="system-requirements"></a>System requirements

### <a name="operating-system"></a>Operating system

+ Windows 10 Anniversary Update (10.0.14393.0 and later) Pro or Enterprise edition.

### <a name="hardware-configuration"></a>Hardware configuration

+ 64 bit (x64) processor
+ Hardware-assisted virtualization
+ Second Level Address Translation (SLAT)

### <a name="required-resources"></a>Required resources

+ [Windows Software Development Kit (SDK) for Windows 10](https://go.microsoft.com/fwlink/?linkid=821375)

## <a name="set-up-the-desktop-app-converter"></a>Set up the Desktop App Converter

Desktop App Converter relies on the latest Windows 10 features. Please ensure that you're on the Windows 10 Anniversary Update (14393.0) or later builds.

### <a name="store-download"></a>Store download

1.  Download the [DesktopAppConverter from the Windows Store](https://aka.ms/converter) and the [base image .wim file that matches your build](https://aka.ms/converterimages).  
2.  Run the DesktopAppConverter as admin. You can do this from the start menu by by right-clicking the tile and selecting *Run as administrator* from under *More*, or from the taskbar by right-clicking the tile, right clicking a second time on the app name that pops up, and then selecting *Run as administrator.*
3.  From the app console window, run ```CMD PS C:\> Set-ExecutionPolicy bypass```.
4.  Set up the converter by running ```CMD PS C:\> DesktopAppConverter.exe -Setup -BaseImage .\BaseImage-1XXXX.wim -Verbose``` from the app console window.
5.  If running the previous command prompts you to reboot, please restart your machine.

### <a name="zip-file"></a>Zip file 

The DAC remains available as a zip file in the [download center](https://aka.ms/converterimages) to facilitate offline scenarios. However, all future updates will only be published in the store version.

1.  Download the DAC zip and the [base image .wim file that matches your build](https://aka.ms/converterimages).  
2. Extract DesktopAppConverter.zip to a local folder.
3. From an admin PowerShell window, run ```CMD PS C:\> Set-ExecutionPolicy bypass```.
4. Set up the converter by running ```CMD PS C:\> .\DesktopAppConverter.ps1 -Setup -BaseImage .\BaseImage-1XXXX.wim -Verbose``` from an admin PowerShell window.
5. If running the previous command prompts you to reboot, please restart your machine.

## <a name="run-the-desktop-app-converter"></a>Run the Desktop App Converter

+ **Store download**: Use ```DesktopAppConverter.exe``` to run the converter.
+ **Zip file**: Use ```DesktopAppConverter.ps1``` to run the converter. 

### <a name="usage"></a>Usage

```CMD
DesktopAppConverter.exe
-Installer <String> [-InstallerArguments <String>] [-InstallerValidExitCodes <Int32>]
-Destination <String>
-PackageName <String>
-Publisher <String>
-Version <Version>
[-ExpandedBaseImage <String>]
[-AppExecutable <String>]
[-AppFileTypes <String>]
[-AppId <String>]
[-AppDisplayName <String>]
[-AppDescription <String>]
[-PackageDisplayName <String>]
[-PackagePublisherDisplayName <String>]
[-MakeAppx]
[-LogFile <String>]
[<CommonParameters>]  
```

### <a name="example"></a>Example

The following example shows how to convert a desktop app named *MyApp* by *&lt;publisher_name&gt;* to a UWP package (AppX).

```CMD
DesktopAppConverter.exe -Installer C:\Installer\MyApp.exe 
-InstallerArguments "/S" -Destination C:\Output\MyApp -PackageName "MyApp" 
-Publisher "CN=<publisher_name>" -Version 0.0.0.1 -MakeAppx -Verbose
```

## <a name="deploy-your-converted-appx"></a>Deploy your converted AppX

Use the [Add-AppxPackage](https://technet.microsoft.com/library/hh856048.aspx) cmdlet in PowerShell to deploy a signed app package (.appx) to a user account. 

You can use the ```-Sign``` flag in the Desktop App Converter (v0.1.24) to auto-sign your converted app. Alternatively, refer to [Sign your converted desktop app](desktop-to-uwp-signing.md) to learn how to self-sign AppX packages.

You can also utilize the ```-Register``` parameter of the Add-AppXPackage PowerShell cmdlet to install from a folder of unpackaged files during the development process. 

For more information on deploying and debugging your converted app, see [Deploy and debug your converted UWP app](desktop-to-uwp-deploy-and-debug.md). 

## <a name="sign-your-appx-package"></a>Sign your .Appx Package

The Add-AppxPackage cmdlet requires that the application package (.appx) being deployed must be signed. Use ```-Sign``` flag as part of the converter command line or SignTool.exe, which ships in the Microsoft Windows 10 SDK, to sign the .appx package.

For additional details on how to sign your .appx package, see [Sign your converted desktop app](desktop-to-uwp-signing.md). 

## <a name="caveats"></a>Caveats

1. The Windows 10 build on the host machine must match the base image that you obtained as part of the Desktop App Converter download.  
2. Ensure that the desktop installer is in an independent directory, because the converter copies all of the directory's content to the isolated Windows environment.  
3. Currently, the Desktop App Converter supports running the conversion process on a 64-bit operating system only. You can deploy the converted .appx packages to a 64-bit (x64) OS only.  
4. Desktop App Converter requires the desktop installer to run under unattended mode. Ensure that you pass the silent flag for your installer to the converter by using the *-InstallerArguments* parameter.
5. Publishing public SxS Fusion assemblies won't work. During install, an application can publish public side-by-side Fusion assemblies, accessible to any other process. During process activation context creation, these assemblies are retrieved by a system process named CSRSS.exe. When this is done for a converted process, activation context creation and module loading of these assemblies will fail. Inbox assemblies, like ComCtl, are shipped with the OS, so taking a dependency on them is safe. The SxS Fusion assemblies are registered in the following locations:
  + Registry: `HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\SideBySide\Winners`
  + File System: %windir%\\SideBySide

## <a name="known-issues"></a>Known issues

+ If you receive a Windows Insider flight on a developer machine that previously had the Desktop App Converter installed, you may receive the error `New-ContainerNetwork: The object already exists` when you setup the new base image. As a workaround, run the command `Netsh int ipv4 reset` from an elevated command prompt, then reboot your machine. 
+ A .NET app compiled with "AnyCPU" build option will fail to install if the main executable or any of the dependencies were placed under "Program Files" or "Windows\System32". As a workaround, please use your architecture specific desktop installer (32 bit or 64 bit) to successfully generate an AppX package.

## <a name="telemetry-from-desktop-app-converter"></a>Telemetry from Desktop App Converter  
Desktop App Converter may collect information about you and your use of the software and send this info to Microsoft. You can learn more about Microsoft's data collection and use in the product documentation and in the [Microsoft Privacy Statement](http://go.microsoft.com/fwlink/?LinkId=521839). You agree to comply with all applicable provisions of the Microsoft Privacy Statement.

By default, telemetry will be enabled for the Desktop App Converter. Add the following registry key to configure telemetry to a desired setting:  
```CMD
HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\DesktopAppConverter
```
+ Add or edit the *DisableTelemetry* value by using a DWORD set to 1.
+ To enable telemetry, remove the key or set the value to 0.

## <a name="desktop-app-converter-usage"></a>Desktop App Converter usage

Here's a list of parameters to the Desktop App Converter. You can also view this list by running:   

```CMD
Get-Help DesktopAppConverter.exe -detailed
```

### <a name="setup-parameters"></a>Setup Parameters  

|Parameter|Description|
|---------|-----------|
|```-Setup [<SwitchParameter>]``` | Runs DesktopAppConverter in setup mode. Setup mode supports expanding a provided base image.|
|```-BaseImage <String>``` | Full path to an unexpanded base image. This parameter is required if -Setup is specified.|
|```-LogFile <String>``` [optional] | Specifies a log file. If omitted, a log file temporary location will be created.|
|```-NatSubnetPrefix <String>``` [optional] | Prefix value to be used for the Nat instance. Typically, you would want to change this only if your host machine is attached to the same subnet range as the converter's NetNat. You can query the current converter NetNat config by using the **Get-NetNat** cmdlet. |
|```-NoRestart [<SwitchParameter>]``` | Don't prompt for reboot when running setup (reboot is required to enable the container feature). |

### <a name="conversion-parameters"></a>Conversion Parameters  

|Parameter|Description|
|---------|-----------|
|```-AppInstallPath <String> [optional]``` | The full path to your application's root folder for the installed files if it were installed (e.g., "C:\Program Files (x86)\MyApp").| 
|```-Destination <String>``` | The desired destination for the converter's appx output - DesktopAppConverter can create this location if it doesn't already exist.|
|```-Installer <String>``` | The path to the installer for your application - must be able to run unattended/silently|
|```-InstallerArguments <String>``` [optional] | A comma-separated list or string of arguments to force your installer to run unattended/silently. This parameter is optional if your installer is an msi. To get a log from your installer, supply the logging argument for the installer here and use the path ```<log_folder>```, which is a token that the converter replaces with the appropriate path. <br><br>**NOTE: The unattended/silent flags and log arguments will vary between installer technologies.** <br><br>An example usage for this parameter: ```-InstallerArguments "/silent /log <log_folder>\install.log"``` Another example that doesn't produce a log file may look like: ```-InstallerArguments "/quiet", "/norestart"``` Again, you must literally direct any logs to the token path ```<log_folder>``` if you want the converter to capture it and put it in the final log folder.|
|```-InstallerValidExitCodes <Int32>``` [optional] | A comma-separated list of exit codes that indicate your installer ran successfully (for example: 0, 1234, 5678).  By default this is 0 for non-msi, and 0, 1641, 3010 for msi.|

### <a name="appx-identity-parameters"></a>Appx Identity Parameters  

|Parameter|Description|
|---------|-----------|
|```-PackageName <String>``` | The name of your Universal Windows App package
|```-Publisher <String>``` | The publisher of your Universal Windows App package
|```-Version <Version>``` | The version number for your Universal Windows App package

### <a name="optional-appx-manifest-parameters"></a>Optional Appx Manifest Parameters  

|Parameter|Description|
|---------|-----------|
|```-AppExecutable <String> [optional]``` [optional] | The name of your application's main executable (eg "MyApp.exe"). |
|```-AppFileTypes <String>``` [optional] | A comma-separated list of file types which the application will be associated with (eg. ".txt, .doc", without the quotes).|
|```-AppId <String>``` [optional] | Specifies a value to set Application Id to in the appx manifest. If it is not specified, it will be set to the value passed in for *PackageName*.|
|```-AppDisplayName <String>``` [optional] | Specifies a value to set Application Display Name to in the appx manifest. If it is not specified, it will be set to the value passed in for *PackageName*. |
|```-AppDescription <String>``` [optional] | Specifies a value to set Application Description to in the appx manifest. If it is not specified, it will be set to the value passed in for *PackageName*.|
|```-PackageDisplayName <String>``` [optional] | Specifies a value to set Package Display Name to in the appx manifest. If it is not specified, it will be set to the value passed in for *PackageName*. |
|```-PackagePublisherDisplayName <String>``` [optional] | Specifies a value to set Package Publisher Display Name to in the appx manifest. If it is not specified, it will be set to the value passed in for *Publisher*. |

### <a name="other-conversion-parameters"></a>Other Conversion Parameters  

|Parameter|Description|
|---------|-----------|
|```-ExpandedBaseImage <String>``` [optional] | Full path to an already expanded base image.|
|```-MakeAppx [<SwitchParameter>]``` [optional] | A switch that, when present, tells this script to call MakeAppx on the output. |
|```-LogFile <String>``` [optional] | Specifies a log file. If omitted, a log file temporary location will be created. |
| ```Sign [<SwitchParameter>] [optional]``` | Tells this script to sign the output appx. This switch should be present alongside the switch ```-MakeAppx```. 
|```<Common parameters>``` | This cmdlet supports the common parameters: *Verbose*, *Debug*, *ErrorAction*, *ErrorVariable*, *WarningAction*, *WarningVariable*, *OutBuffer*, *PipelineVariable*, and *OutVariable*. For more info, see [about_CommonParameters](http://go.microsoft.com/fwlink/?LinkID=113216). |

### <a name="cleanup-parameters"></a>Cleanup Parameters

|Parameter|Description|
|---------|-----------|
|```Cleanup [<Option>]``` | Runs cleanup for the DesktopAppConverter artifacts. There are 3 valid options for the Cleanup mode. |
|```Cleanup All``` | Deletes all expanded base images, removes any temporary converter files, removes the container network, and disables the optional Windows feature, Containers. |
|```Cleanup WorkDirectory``` | Removes all the temporary converter files. |
|```Cleanup ExpandedImage``` | Deletes all the expanded base images installed on your host machine. |

### <a name="package-architecture"></a>Package Architecture

The Desktop App Converter now supports creation of both x86 and x64 app packages that you can install and run on x86 and amd64 machines. Note the Desktop App Converter still needs to run on an AMD64 machine to perform a successful conversion.

|Parameter|Description|
|---------|-----------|
|```-PackageArch <String>``` | Generates a package with the specified architecture. Valid options are 'x86' or 'x64'; for example, -PackageArch x86. This parameter is optional. If unspecified, the DesktopAppConverter will try to auto-detect package architecture. If auto-detection fails, it will default to x64 package. 

### <a name="running-the-peheadercertfixtool"></a>Running the PEHeaderCertFixTool

During the conversion process, the DesktopAppConverter automatically runs the PEHeaderCertFixTool in order to fixup any corrupted PE headers. However, you can also run the PEHeaderCertFixTool on a UWP appx, loose files or a specific binary. 

PEHeaderCertFixTool ships as part of the DesktopAppConverter.zip. Example usage: 

```CMD
PEHeaderCertFixTool.exe <binary file>|<.appx package>|<folder> [/c] [/v]
 /c   -- check for corrupted certificate but do not fix (optional)
 /v   -- verbose (optional)
example1: PEHeaderCertFixTool app.exe
example2: PEHeaderCertFixTool c:\package.appx /c
example3: PEHeaderCertFixTool c:\myapp /c /v
```

## <a name="language-support"></a>Language support

The Desktop App Converter does not support Unicode; thus, no Chinese characters or non-ASCII characters can be used with the tool.

## <a name="see-also"></a>See also

+ [Bringing Desktop Apps to the UWP Using Desktop App Converter](https://channel9.msdn.com/events/Build/2016/P504)
+ [Project Centennial: Bringing Existing Desktop Applications to the Universal Windows Platform](https://channel9.msdn.com/events/Build/2016/B829)  


<!--HONumber=Dec16_HO1-->


