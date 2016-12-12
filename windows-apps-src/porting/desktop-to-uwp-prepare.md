---
author: awkoren
Description: This article lists things you need to know before converting your app with the Desktop to UWP Bridge. You may not need to do much to get your app ready for the conversion process.
Search.Product: eADQiWindows 10XVcnh
title: Prepare your app for the Desktop to UWP Bridge
translationtype: Human Translation
ms.sourcegitcommit: f7a8b8d586983f42fe108cd8935ef084eb108e35
ms.openlocfilehash: 81a2485d5be22dd392c21aaff281c1c9263883a9

---

# <a name="prepare-an-app-for-conversion-with-the-desktop-bridge"></a>Prepare an app for conversion with the Desktop Bridge

This article lists things you need to know before converting your app with the Desktop to UWP Bridge. You may not need to do much to get your app ready for the conversion process, but if any of the items below applies to your application, you need to address it before conversion. Remember that the Windows Store handles licensing and automatic updating for you, so you can remove those features from your codebase.

+ __Your app uses a version of .NET earlier than 4.6.1__. Only .NET 4.6.1 is supported. You must retarget your app to .NET 4.6.1 before converting. 

+ __Your app always runs with elevated security privileges__. Your app needs to work while running as the interactive user. Users who install your app from the Windows Store may not be system administrators, so requiring your app to run elevated means that it won't run correctly for standard users.

+ __Your app requires a kernel-mode driver or a Windows service__. The bridge is suitable for an app, but it does not support a kernel-mode driver or a Windows service that needs to run under a system account. Instead of a Windows service, use a [background task](https://msdn.microsoft.com/windows/uwp/launch-resume/create-and-register-a-background-task).

+ __Your app's modules are loaded in-process to processes that are not in your AppX package__. This isn't permitted, which means that in-process extensions, like [shell extensions](https://msdn.microsoft.com/library/windows/desktop/dd758089.aspx), aren't supported. But if you have two apps in the same package, you can do inter-process communication between them.

+ __Your app calls [SetDllDirectory](https://msdn.microsoft.com/library/windows/desktop/ms686203) or [AddDllDirectory](https://msdn.microsoft.com/library/windows/desktop/hh310513)__. These functions are not currently supported for converted apps. We are working on adding support in a future release. As a workaround, you can copy any .dlls you were locating using these functions to your package root. 

+ __Your app uses a custom Application User Model ID (AUMID)__. If your process calls [SetCurrentProcessExplicitAppUserModelID](https://msdn.microsoft.com/library/windows/desktop/dd378422.aspx) to set its own AUMID, then it may only use the AUMID generated for it by the app model environment/AppX package. You can't define custom AUMIDs.

+ __Your app modifies the HKEY_LOCAL_MACHINE (HKLM) registry hive__. Any attempt by your app to create an HKLM key, or to open one for modification, will result in an access-denied failure. Remember that your app has its own private virtualized view of the registry, so the notion of a user- and machine-wide registry hive (which is what HKLM is) does not apply. You will need to find another way of achieving what you were using HKLM for, like writing to HKEY_CURRENT_USER (HKCU) instead.

+ __Your app uses a ddeexec registry subkey as a means of launching another app__. Instead, use one of the DelegateExecute verb handlers as configured by the various Activatable* extensions in your [app package manifest](https://msdn.microsoft.com/library/windows/apps/br211474.aspx).

+ __Your app writes to the AppData folder with the intention of sharing data with another app__. After conversion, AppData is redirected to the local app data store, which is a private store for each UWP app. Use a different means of inter-process data sharing. For more info, see [Store and retrieve settings and other app data](https://msdn.microsoft.com/windows/uwp/app-settings/store-and-retrieve-app-data).

+ __Your app writes to the install directory for your app__. For example, your app writes to a log file that you put in the same directory as your exe. This isn't supported, so you'll need to find another location, like the local app data store.

+ __Your app installation requires user interaction__. Your app installer must be able to run silently, and it must install all of its prerequisites that aren't on by default on a clean OS image.

+ __Your app uses the Current Working Directory__. At runtime, your converted app won't get the same Working Directory that you previously specified in your desktop .LNK shortcut. You need to change your CWD at runtime if having the correct directory is important for your app to function correctly.

+ __Your app requires UIAccess__. If your application specifies `UIAccess=true` in the `requestedExecutionLevel` element of the UAC manifest, conversion to UWP isn't supported currently. For more info, see [UI Automation Security Overview](https://msdn.microsoft.com/library/ms742884.aspx).

+ __Your app exposes COM objects or GAC assemblies for use by other processes__. In the current release, your app cannot expose COM objects or GAC assemblies for use by processes originating from executables external to your AppX package. Processes from within the package can register and use COM objects and GAC assemblies as normal, but they will not be visible externally. This means interop scenarios like OLE will not function if invoked by external processes. 

+ __Your app is linking C runtime libraries (CRT) in an unsupported manner__. The Microsoft C/C++ runtime library provides routines for programming for the Microsoft Windows operating system. These routines automate many common programming tasks that are not provided by the C and C++ languages. If your app utilizes C/C++ runtime library, you need to ensure it is linked in a supported manner. 
    
    Visual Studio 2015 supports both dynamic linking, to let your code use common DLL files, or static linking, to link the library directly into your code, to the current version of the CRT. If possible, we recommend your app use dynamic linking with VS 2015. 

    Support in previous versions of Visual Studio varies. See the following table for details: 

    <table>
    <th>Visual Studio version</td><th>Dynamic linking</th><th>Static linking</th></th>
    <tr><td>2005 (VC 8)</td><td>Not supported</td><td>Supported</td>
    <tr><td>2008 (VC 9)</td><td>Not supported</td><td>Supported</td>
    <tr><td>2010 (VC 10)</td><td>Supported</td><td>Supported</td>
    <tr><td>2012 (VC 11)</td><td>Supported</td><td>Not supported</td>
    <tr><td>2013 (VC 12)</td><td>Supported</td><td>Not supported</td>
    <tr><td>2015 (VC 14)</td><td>Supported</td><td>Supported</td>
    </table>
    
    Note: In all cases, you must link to the latest publically available CRT.

+ __Your app installs and loads assemblies from the Windows side-by-side folder__. For example, your app uses C runtime libraries VC8 or VC9 and is dynamically linking them from Windows side-by-side folder, meaning your code is using the common DLL files from a shared folder. This is not supported. You will need to statically link them by linking to the redistributable library files directly into your code.

+ __Your app uses a dependency in the System32/SysWOW64 folder__. To get these DLLs to work, you must include them in the virtual file system portion of you AppX package. This ensures that the app behaves as if the DLLs were installed in the **System32**/**SysWOW64** folder. In the root of the package, create a folder called **VFS**. Inside that folder create a **SystemX64** and **SystemX86** folder. Then, place the 32-bit version of your DLL in the **SystemX86** folder, and place the 64-bit version in the **SystemX64** folder.

+ __Your app uses the Dev11 VCLibs framework package__. The VCLibs 11 libraries can be directly installed from the Windows Store if they are defined as a dependency in the AppX package. To do this, make the following change to your app package manifest: Under the `<Dependencies>` node, add:  
`<PackageDependency Name="Microsoft.VCLibs.110.00.UWPDesktop" MinVersion="11.0.24217.0" Publisher="CN=Microsoft Corporation, O=Microsoft Corporation, L=Redmond, S=Washington, C=US" />`  
During installation from the Windows Store, the appropriate version (x86 or x64) of the VCLibs 11 framework will get installed prior to the installation of the app.  
The dependencies will not get installed if the app is installed by sideloading. To install the dependencies manually on your machine, you must download and install the [VC 11.0 framework packages for Desktop Bridge](https://www.microsoft.com/download/details.aspx?id=53340&WT.mc_id=DX_MVP4025064). For more information on these scenarios, see [Using Visual C++ Runtime in Centennial project](https://blogs.msdn.microsoft.com/vcblog/2016/07/07/using-visual-c-runtime-in-centennial-project/).


<!--HONumber=Dec16_HO1-->


