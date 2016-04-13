---
Description: Prepare your Windows desktop application (like Win32, WPF, and Windows Forms) for conversion to a Universal Windows Platform (UWP) app by using the Desktop Conversion extensions.
Search.Product: eADQiWindows 10XVcnh
title: Convert your desktop application to a Universal Windows Platform (UWP) app
---

# Convert your desktop application to a Universal Windows Platform (UWP) app

\[Some information relates to pre-released product which may be substantially modified before it's commercially released. Microsoft makes no warranties, express or implied, with respect to the information provided here.\]

Prepare your Windows desktop application (like Win32, WPF, and Windows Forms) for conversion to a Universal Windows Platform (UWP) app by using the Desktop Conversion extensions.

## Benefits of converting your application to UWP

UWP using Desktop Conversion extensions is a bridge that enables you to convert your classic desktop application (like Win32, Windows Forms, and WPF) or game to a Universal Windows Platform (UWP) app or game. For more info, see [Guide to UWP apps](https://msdn.microsoft.com/library/windows/apps/dn894631.aspx). After conversion, your classic desktop app is packaged, serviced, and deployed in the form of a UWP app package (an .appx or an .appxbundle) targeting Windows 10 Desktop.

There are two parts to the technology that enables desktop apps to be converted to UWP packages. The first is the Desktop App Converter, which takes your existing binaries and repackages them as a UWP package. Your code is still the same, it's just packaged differently. The second piece comprises runtime technologies in the Windows Anniversary update that enable a UWP package to have executables that run as full trust instead of in an app container. This technology also gives a converted app a package identity, which is required to use some UWP APIs.

Here are some of the benefits of converting your classic desktop app.
* Your app's installation experience is much smoother for your customers. You can deploy it to computers using sideloading (see [Sideload LOB apps in Windows 10](https://technet.microsoft.com/library/mt269549.aspx)), and it leaves no trace behind after being uninstalled. Longer term, you'll also be able to publish your app to the Windows Store.
* Because your converted app has package identity, you can call more UWP APIs, even from the full-trust partition, than you could before.
* At your own pace, you can add UWP features to your app's package, like a XAML user-interface, live tile updates, UWP background tasks, app services, and many more. All of the functionality available to any other UWP app is available to your app.
* If you choose to move all of your app's functionality out of the full-trust partition of the app and into the app container partition, then your app will be able to run on any Windows 10 device.
* As a UWP app, your app is able to do the things it could do as a classic desktop app. It interacts with a virtualized view of the registry and file system that's indistinguishable from the actual registry and file system.
* Your app can participate in the Windows Store's built-in licensing and automatic update facilities. Automatic update is a highly reliable and efficient mechanism, because only the changed parts of files are downloaded.

## Preparing your desktop app for conversion to UWP
You may not need to do much to get your app ready for the conversion process. Remember that the Windows Store handles licensing and automatic updating for you, so you can remove those features from your codebase. If any of these situations applies to your application, you need to address this issue before conversion.

+ __Your app always runs with elevated security privileges__. Your app needs to work while running as the interactive user. Users who install your app from the Windows Store may not be system administrators, so requiring your app to run elevated means that it won't run correctly for standard users.
+ __Your app requires a kernel-mode driver or a Windows service__. The bridge is suitable for an app, but it does not support a kernel-mode driver or a Windows service that needs to run under a system account. Instead of a Windows service, use a [background task](https://msdn.microsoft.com/windows/uwp/launch-resume/create-and-register-a-background-task).
+ __Your app's modules are loaded in-process to processes that are not in your AppX package__. This isn't permitted, which means that in-process extensions, like [shell extensions](https://msdn.microsoft.com/library/windows/desktop/dd758089.aspx), aren't supported. But if you have two apps in the same package, you can do inter-process communication between them.
+ __Your app uses a custom Application User Model ID (AUMID)__. If your process calls [SetCurrentProcessExplicitAppUserModelID](https://msdn.microsoft.com/library/windows/desktop/dd378422.aspx) to set its own AUMID, then it may only use the AUMID generated for it by the app model environment/AppX package. You can't define custom AUMIDs.
+ __Your app modifies the HKEY_LOCAL_MACHINE (HKLM) registry hive__. Any attempt by your app to create an HKLM key, or to open one for modification, will result in an access-denied failure. Remember that your app has its own private virtualized view of the registry, so the notion of a user- and machine-wide registry hive (which is what HKLM is) does not apply. You will need to find another way of achieving what you were using HKLM for, like writing to HKEY_CURRENT_USER (HKCU) instead.
+ __Your app uses a ddeexec registry subkey as a means of launching another app__. Instead, use one of the DelegateExecute verb handlers as configured by the various Activatable* extensions in your [app package manifest](https://msdn.microsoft.com/library/windows/apps/br211474.aspx).
+ __Your app writes to the AppData folder with the intention of sharing data with another app__. After conversion, AppData is redirected to the local app data store, which is a private store for each UWP app. Use a different means of inter-process data sharing. For more info, see [Store and retrieve settings and other app data](https://msdn.microsoft.com/windows/uwp/app-settings/store-and-retrieve-app-data).
+ __Your app writes to the install directory for your app__. For example, your app writes to a log file that you put in the same directory as your exe. This isn't supported, so you'll need to find another location, like the local app data store.
+ __Your app installation requires user interaction__. Your app installer must be able to run silently, and it must install all of its prerequisites that aren't on by default on a clean OS image.
+ __Your app uses the Current Working Directory__. At runtime, your converted app won't get the same Working Directory that you previously specified in your desktop .LNK shortcut. You need to change your CWD at runtime if having the correct directory is important for your app to function correctly.

## In this section

| Topic | Description |
|-------|-------------|
| [Desktop App Converter Preview (Project Centennial)](desktop-to-uwp-run-desktop-app-converter.md) | Shows how to run Desktop App Converter. You probably won't need to do much, if anything, to get your app ready for the conversion process. |
| [Deploy and debug your converted UWP app](desktop-to-uwp-deploy-and-debug.md) | Contains info to help you be successful deploying and debugging your app after converting it. Also, if you're curious about some of the internals of the Desktop Conversion extensions, this topic is for you. |


<!--HONumber=Apr16_HO1-->


