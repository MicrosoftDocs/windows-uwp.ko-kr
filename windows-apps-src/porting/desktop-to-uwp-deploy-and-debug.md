---
Description: Deploy and debug a Universal Windows Platform (UWP) app converted from a Windows desktop application (Win32, WPF, and Windows Forms) by using the Desktop Conversion extensions.
Search.Product: eADQiWindows 10XVcnh
title: Deploy and debug a Universal Windows Platform (UWP) app converted from a Windows desktop application
---

# Deploy and debug your converted UWP app

\[Some information relates to pre-released product which may be substantially modified before it's commercially released. Microsoft makes no warranties, express or implied, with respect to the information provided here.\]

This topic contains info to help you be successful deploying and debugging your app after converting it. Also, if you're curious about some of the internals of the Desktop Conversion extensions, then this topic is for you.

## Deploy your converted UWP app
Any drive that you install your converted app onto must be formatted to NTFS format.

A converted app always runs as the interactive user. This has particular significance for a .NET app whose manifest specifies an execution level of __requireAdministrator__. If the interactive user has administrator privileges then a UAC prompt will be displayed _each time the app is launched_. For standard users, the app will fail to launch.

If you try to run the Add-AppxPackage cmdlet on a machine where you haven't imported the cert you created, you'll get an error.

Here's how you import a certificate that you created previously. You can install it directly, or you can install it from an appx that you've signed, like the customer will.
1.  In File Explorer, right click an appx that you've signed with a test cert and choose **Properties** from the context menu.
2.  Click or tap the **Digital Signatures** tab.
3.  Click or tap on the certificate and choose **Details**.
4.  Click or tap **View Certificate**.
5.  Click or tap **Install Certificate**.
6.  In the **Store Location** group, select **Local Machine**.
7.  Click or tap **Next** and **OK** to confirm the UAC dialog.
8.  In the next screen of the Certificate Import Wizard, change the selected option to **Place all certificates in the following store**.
9.  Click or tap **Browse**. In the Select Certificate Store window, scroll down and select **Trusted People** and click or tap **OK**.
10. Click or tap **Next**. A new screen appears. Click or tap **Finish**.
11. A confirmation dialog should appear. If so, click **OK**. If a different dialog indicates that there is a problem with the certificate, you may need to do some certificate troubleshooting.

## Additional info
For Windows to trust the certificate, the certificate must be located in either the **Certificates (Local Computer) > Trusted Root Certification Authorities > Certificates** node or the **Certificates (Local Computer) > Trusted People > Certificates** node. Only certificates in these two locations can validate the certificate trust in the context of the local machine. Otherwise, an error message that resembles the following string appears:
```CMD
“Add-AppxPackage : Deployment failed with HRESULT: 0x800B0109, A certificate chain processed,
but terminated in a rootcertificate which is not trusted by the trust provider.
(Exception from HRESULT: 0x800B0109) error 0x800B0109: The root certificate of the signature
in the app package must be trusted.”
```

## Debug your converted UWP app
When Microsoft Visual Studio is running "as administrator", the __Start Debugging__ and __Start Without Debugging__ commands will work for a converted app's project, but the launched app will run with [medium integrity level](https://msdn.microsoft.com/library/bb625963). That is, it will _not_ have elevated privileges. To confer administrator privileges onto the launched app, first you need to launch the "as administrator" via a shortcut or a tile. Once the app is running, from an instance of Microsoft Visual Studio running "as administrator", invoke the __Attach to Process__ and select your app's process from the dialog.

## Behind the scenes
When you run your converted app, your UWP app package is launched from \Program Files\WindowsApps\\&lt;_package name_&gt;\\&lt;_appname_&gt;.exe. If you look there, you'll see that your app has an app package manifest (named AppxManifest.xml), which references a special xml namespace that's used for converted apps. Inside that manifest file is an __&lt;EntryPoint&gt;__ element, which references a full-trust app. When that app is launched, it does not run inside an app container, but instead it runs as the user as it normally would.

But the app runs in a special environment where any accesses that the app makes to the file system and to the Registry are redirected. The file named Registry.dat is used for Registry redirection. It's actually a Registry hive, so you can view it in the Windows Registry Editor (Regedit). Note, that this mechanism means that you can't use the Registry for inter-process communication. The Registry wasn't designed for, and is not well-suited to, that practice in any case. When it comes to the file system, the only thing redirected is the AppData folder, and it is redirected to the same location that app data is stored for all UWP apps. This location is known as the local app data store, and you access it by using the [ApplicationData.LocalFolder](https://msdn.microsoft.com/library/windows/apps/br241621) property. This way, your code is already ported to read and write app data in the correct place without you doing anything. And you can also write there directly. One benefit of file system redirection is a cleaner uninstall experience.

Inside a folder named VFS, you will see folders that contain the DLLs that your app has dependencies on. These DLLs are installed into system folders for the classic desktop version of your app. But, as a UWP app, the DLLs are local to your app. This way, there are no versioning problems when UWP apps are installed and uninstalled.


<!--HONumber=Apr16_HO2-->


