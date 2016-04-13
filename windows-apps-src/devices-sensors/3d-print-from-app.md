---
title: 3D printing from your app
description: Learn how to add 3D printing functionality to your Universal Windows app. This topic covers how to launch the 3D print dialog after ensuring your 3D model is printable and in the correct format.
ms.assetid: D78C4867-4B44-4B58-A82F-EDA59822119C
---

# 3D printing from your app


\[ Updated for UWP apps on Windows 10. For Windows 8.x articles, see the [archive](http://go.microsoft.com/fwlink/p/?linkid=619132) \]


**Important APIs**

-   [**Windows.Graphics.Printing3D**](https://msdn.microsoft.com/library/windows/apps/dn998169)

Learn how to add 3D printing functionality to your Universal Windows app. This topic covers how to load 3D geometry data into your app and launch the 3D print dialog after ensuring your 3D model is printable and in the correct format. For a working example of these procedures in action, see the [3D Printing UWP sample](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/3DPrinting).

## Class setup


In your class that is to have 3D print functionality, add the [Windows.Graphics.Printing3D](https://msdn.microsoft.com/library/windows/apps/dn998169) namespace.

[!code-cs[3DPrintNamespace](./code/3dprinthowto/cs/MainPage.xaml.cs#Snippet3DPrintNamespace)]

The following additional namespaces will be used in this particular guide:

[!code-cs[OtherNamespaces](./code/3dprinthowto/cs/MainPage.xaml.cs#SnippetOtherNamespaces)]

Next, give your class some helpful member fields. Declare a [Print3DTask](https://msdn.microsoft.com/library/windows/apps/dn998044) object to serve as a reference to the printing task that is to be passed to the print driver. Declare a [StorageFile](https://msdn.microsoft.com/library/windows/apps/br227171) object to hold the original 3D data file. Finally, declare a [Printing3D3MFPackage](https://msdn.microsoft.com/library/windows/apps/dn998063) object, which represents a print-ready 3D model with all necessary metadata.

[!code-cs[DeclareVars](./code/3dprinthowto/cs/MainPage.xaml.cs#SnippetDeclareVars)]

## Create a simple UI


This sample uses three user controls: a load button which will bring a file into program memory, a fix button which will modify the file as necessary, and a print button which will initiate the printing job. The following code generates these buttons (with their click event handlers) in your class' XAML file:

[!code-xml[Buttons](./code/3dprinthowto/cs/MainPage.xaml#SnippetButtons)]

Add a **TextBlock** for UI feedback.

[!code-xml[OutputText](./code/3dprinthowto/cs/MainPage.xaml#SnippetOutputText)]

## Get the 3D data


The method by which your app acquires 3D geometry data to print will vary. Your app may retrieve data from a 3D scan, pull model data from a web resource, or generate a 3D mesh programmatically using equations. For the sake of simplicity, this guide will load a 3D data file (of any of several common file types) into program memory from File Explorer.

In your `OnLoadClick` method, use the [FileOpenPicker](https://msdn.microsoft.com/library/windows/apps/br207847) class to load a single file into your app's memory.

[!code-cs[FileLoad](./code/3dprinthowto/cs/MainPage.xaml.cs#SnippetFileLoad)]

## Use 3D Builder to convert to 3D Manufacturing Format (.3mf)

At this point, you are able to load a 3D data file into your app's memory. However, 3D geometry data comes in many different formats, and not all are efficient for 3D printing. Windows 10 uses the 3D Manufacturing Format (.3mf) file type for all 3D Printing tasks.

> **Note**  The 3MF file type offers a great deal of functionality not covered in this tutorial. To learn more about 3MF and the features it provides to producers and consumers of 3D products, refer to the [3MF Specification](http://3mf.io/what-is-3mf/3mf-specification/). To learn how to harness these features using Windows 10 APIs, see the [Generate a 3MF package](https://msdn.microsoft.com/windows/uwp/devices-sensors/generate-3mf) tutorial.

Fortunately, the [3D Builder](https://www.microsoft.com/store/apps/3d-builder/9wzdncrfj3t6) app can open files of most popular 3D formats and save them as .3mf files. In this example, where the file type may vary, a very simple solution is to open 3D Builder and prompt the user to save the imported data as a .3mf file and then reload it.

> **Note**  In addition to converting file formats, **3D Builder** provides simple tools to edit your models, add color data, and perform other print-specific operations, so it is often worth integrating into an app that deals with 3D printing.

[!code-cs[FileCheck](./code/3dprinthowto/cs/MainPage.xaml.cs#SnippetFileCheck)]

## Repair model data for 3D printing

Not all 3D model data is able to be printed, even in .3mf format. In order for the printer to correctly determine what space to fill and what to leave empty, the model(s) to be printed must be a single seamless mess, have outward-facing surface normals, and have manifold geometry. Issues in these areas can crop up in a variety of different forms and can be hard to spot in complex shapes. Fortunately, current software solutions are often adequate for converting raw geometry to printable 3D shapes. This will be done in the `OnFixClick` method.

The 3D data file must be converted to implement [IRandomAccessStream](https://msdn.microsoft.com/library/windows/apps/br241731), which can then be used to generate a [Printing3DModel](https://msdn.microsoft.com/library/windows/apps/mt203679) object.

[!code-cs[RepairModel](./code/3dprinthowto/cs/MainPage.xaml.cs#SnippetRepairModel)]

The **Printing3DModel** object is now repaired and printable. Use **SaveModelToPackageAsync** to assign the model to the Printing3D3MFPackage object you declared when creating the class.

[!code-cs[SaveModel](./code/3dprinthowto/cs/MainPage.xaml.cs#SnippetSaveModel)]

## Execute printing task: create a TaskRequested handler


Later on, when the 3D print dialog is displayed to the user and the user elects to begin printing, your app will need to pass in the desired parameters to the 3D print pipeline. The 3D print API will raise the **TaskRequested** event. You must write a method to handle this event appropriately. As always, it must be of the same type as its event: The **TaskRequested** event has parameters [Print3DManager](https://msdn.microsoft.com/library/windows/apps/dn998029) (its sender object) and a [Print3DTaskRequestedEventArgs](https://msdn.microsoft.com/library/windows/apps/dn998051) object, which holds most of the relevant information. Its return type is **void**.

[!code-cs[MyTaskTitle](./code/3dprinthowto/cs/MainPage.xaml.cs#SnippetMyTaskTitle)]

The core purpose of this method is to use the *args* object to send a **Printing3D3MFPackage** down the pipeline. The **Print3DTaskRequestedEventArgs** type has one property: **Request**. It is of the type [Print3DTaskRequest](https://msdn.microsoft.com/library/windows/apps/dn998050) and represents one print job request. Its method **CreateTask** allows the program to submit the right information for your print job, and it returns a reference to the [Print3DTask](https://msdn.microsoft.com/library/windows/apps/dn998044) object which is sent down the 3D print pipeline.

**CreateTask** has the following input parameters: A **string** for the print job name, a **string** for the ID of the printer to use, and a **Print3DTaskSourceRequestedHandler** delegate. The delegate is automatically invoked when the **3DTaskSourceRequested** event is raised (this is done by the API itself). The important thing to note is that this delegate is invoked when a print job is initiated, and it is responsible for providing the right 3D print package.

**Print3DTaskSourceRequestedHandler** takes one parameter, a [Print3DTaskSourceRequestedArgs](https://msdn.microsoft.com/library/windows/apps/dn998056) object which provides the data to be sent. The one public method of this class, **SetSource**, accepts the package to be printed. Implement a **Print3DTaskSourceRequestedHandler** delegate as follows:

[!code-cs[SourceHandler](./code/3dprinthowto/cs/MainPage.xaml.cs#SnippetSourceHandler)]

Next, call **CreateTask**, using the newly-defined delegate `sourceHandler`:

[!code-cs[CreateTask](./code/3dprinthowto/cs/MainPage.xaml.cs#SnippetCreateTask)]

The returned **Print3DTask** is assigned to the class variable declared in the beginning. You can now (optionally) use this reference to handle certain events thrown by the task:

[!code-cs[Optional](./code/3dprinthowto/cs/MainPage.xaml.cs#SnippetOptional)]

> **Note**  You must implement a `Task_Submitting` and `Task_Completed` method if you wish to register them to these events.

## Execute printing task: open 3D print dialog


The final bit of code needed to print from your app is that which launches the 3D print dialog. Like a conventional printing dialog window, the 3D print dialog provides a number of last-minute printing specifications and allows the user to choose which printer to use (whether connected via USB or the network).

First, register your `MyTaskRequested` method with the **TaskRequested** event.

[!code-cs[RegisterMyTaskRequested](./code/3dprinthowto/cs/MainPage.xaml.cs#SnippetRegisterMyTaskRequested)]

After registering your **TaskRequested** event handler, you can invoke the method **ShowPrintUIAsync**, which brings up the 3D print dialog in the current application window.

[!code-cs[ShowDialog](./code/3dprinthowto/cs/MainPage.xaml.cs#SnippetShowDialog)]

Finally, it is a good practice to de-register your event handlers once your app resumes control:
[!code-cs[DeregisterMyTaskRequested](./code/3dprinthowto/cs/MainPage.xaml.cs#SnippetDeregisterMyTaskRequested)]

## Related topics

[Generate a 3MF package](https://msdn.microsoft.com/windows/uwp/devices-sensors/generate-3mf)

[3D Printing UWP sample](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/3DPrinting)
 

 






<!--HONumber=Mar16_HO5-->


