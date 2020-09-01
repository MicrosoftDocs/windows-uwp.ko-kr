---
title: 파일에 대한 기본 앱 시작
description: Windows.System를 사용 하는 방법에 대해 알아봅니다. 응용 프로그램 자체에서 처리할 수 없는 파일에 대 한 기본 처리기를 시작 하는 시작 관리자 API.
ms.assetid: BB45FCAF-DF93-4C99-A8B5-59B799C7BD98
ms.date: 07/05/2018
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 34aee5b2e2f04b7e5d72a7bc31d2cfafc1bcb6fb
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89167847"
---
# <a name="launch-the-default-app-for-a-file"></a>파일에 대한 기본 앱 시작

**중요 API**

-   [** Tem를Windows.Sys합니다. LaunchFileAsync**](/uwp/api/windows.system.launcher.launchfileasync)

파일에 대 한 기본 앱을 시작 하는 방법을 알아봅니다. 많은 앱은 스스로 처리할 수 없는 파일을 사용 해야 합니다. 예를 들어 전자 메일 앱은 다양 한 파일 형식을 받고 이러한 파일을 기본 처리기에서 시작 하는 방법이 필요 합니다. 이러한 단계에서는Windows.System를 사용 하는 방법을 보여 줍니다 [** . **](/uwp/api/Windows.System.Launcher) 응용 프로그램 자체에서 처리할 수 없는 파일에 대 한 기본 처리기를 시작 하는 시작 관리자 API.

## <a name="get-the-file-object"></a>파일 개체 가져오기

먼저 파일의 [**StorageFile**](/uwp/api/Windows.Storage.StorageFile) 개체를 가져옵니다.

파일이 앱에 대 한 패키지에 포함 된 경우 [**InstalledLocation**](/uwp/api/windows.applicationmodel.package.installedlocation) 속성을 사용 하 여 StorageFile [**개체를**](/uwp/api/Windows.Storage.StorageFolder) 가져오고 [**GetFileAsync**](/uwp/api/windows.storage.storagefolder.getfileasync) 메서드를 사용 하 여 [**StorageFile**](/uwp/api/Windows.Storage.StorageFile) 개체를 가져올 수 있습니다.

파일이 알려진 폴더에 있는 경우에는 GetFileAsync 클래스의 속성을 사용 하 여 [**Storagefolder**](/uwp/api/Windows.Storage.StorageFolder) 를 가져오고 [**StorageFile**](/uwp/api/Windows.Storage.StorageFile) 개체를 가져오는 [**GetFileAsync**](/uwp/api/windows.storage.storagefolder.getfileasync) 메서드를 사용할 수 [**있습니다.**](/uwp/api/Windows.Storage.KnownFolders)

## <a name="launch-the-file"></a>파일을 시작 합니다.

Windows는 파일의 기본 처리기를 시작하는 여러 가지 다양한 옵션을 제공합니다. 이러한 옵션은이 차트 및 다음 섹션에서 설명 합니다.

| 옵션 | 메서드 | Description |
|--------|--------|-------------|
| 기본 시작 | [**LaunchFileAsync (IStorageFile)**](/uwp/api/windows.system.launcher.launchfileasync) | 기본 처리기를 사용 하 여 지정 된 파일을 시작 합니다. |
| 시작으로 열기 | [**LaunchFileAsync (IStorageFile, LauncherOptions)**](/uwp/api/windows.system.launcher.launchfileasync#Windows_System_Launcher_LaunchFileAsync_Windows_Storage_IStorageFile_Windows_System_LauncherOptions_) | 사용자가 연결 프로그램 대화 상자를 통해 처리기를 선택할 수 있도록 지정 된 파일을 시작 합니다. |
| 권장 앱 대체를 사용 하 여 시작 | [**LaunchFileAsync (IStorageFile, LauncherOptions)**](/uwp/api/windows.system.launcher.launchfileasync#Windows_System_Launcher_LaunchFileAsync_Windows_Storage_IStorageFile_Windows_System_LauncherOptions_) | 기본 처리기를 사용 하 여 지정 된 파일을 시작 합니다. 시스템에 처리기가 설치 되어 있지 않은 경우 사용자에 게 스토어의 앱을 권장 합니다. |
| 원하는 나머지 뷰로 시작 | [**LaunchFileAsync (IStorageFile, LauncherOptions)**](/uwp/api/windows.system.launcher.launchfileasync#Windows_System_Launcher_LaunchFileAsync_Windows_Storage_IStorageFile_Windows_System_LauncherOptions_) (Windows 전용) | 기본 처리기를 사용 하 여 지정 된 파일을 시작 합니다. 시작 후 화면에 유지 되는 기본 설정을 지정 하 고 특정 창 크기를 요청 합니다. [**DesiredRemainingView**](/uwp/api/windows.system.launcheroptions.desiredremainingview) 은 모바일 장치 제품군에서 지원 되지 않습니다. |

### <a name="default-launch"></a>기본 시작

[**Windows.System를 호출 합니다. LaunchFileAsync (IStorageFile)**](/uwp/api/windows.system.launcher.launchfileasync) 메서드는 기본 앱을 시작 합니다. 이 예제에서는 [**GetFileAsync**](/uwp/api/windows.storage.storagefolder.getfileasync) 메서드를 사용 하 여 앱 패키지에 포함 된 이미지 파일 test.png를 시작 합니다.

```csharp
async void DefaultLaunch()
{
   // Path to the file in the app package to launch
   string imageFile = @"images\test.png";
   
   var file = await Windows.ApplicationModel.Package.Current.InstalledLocation.GetFileAsync(imageFile);
   
   if (file != null)
   {
      // Launch the retrieved file
      var success = await Windows.System.Launcher.LaunchFileAsync(file);

      if (success)
      {
         // File launched
      }
      else
      {
         // File launch failed
      }
   }
   else
   {
      // Could not find file
   }
}
```

```vb
async Sub DefaultLaunch()
   ' Path to the file in the app package to launch
   Dim imageFile = "images\test.png"
   Dim file = await Windows.ApplicationModel.Package.Current.InstalledLocation.GetFileAsync(imageFile)
   
   If file IsNot Nothing Then
      ' Launch the retrieved file
      Dim success = await Windows.System.Launcher.LaunchFileAsync(file)

      If success Then
         ' File launched
      Else
         ' File launch failed
      End If
   Else
      ' Could not find file
   End If
End Sub
```

```cppwinrt
Windows::Foundation::IAsyncAction MainPage::DefaultLaunch()
{
    auto installFolder{ Windows::ApplicationModel::Package::Current().InstalledLocation() };

    Windows::Storage::StorageFile file{ co_await installFolder.GetFileAsync(L"images\\test.png") };

    if (file)
    {
        // Launch the retrieved file
        bool success = co_await Windows::System::Launcher::LaunchFileAsync(file);
        if (success)
        {
            // File launched
        }
        else
        {
            // File launch failed
        }
    }
    else
    {
        // Could not find file
    }
}
```

```cpp
void MainPage::DefaultLaunch()
{
   auto installFolder = Windows::ApplicationModel::Package::Current->InstalledLocation;

   concurrency::task<Windows::Storage::StorageFile^getFileOperation(installFolder->GetFileAsync("images\\test.png"));
   getFileOperation.then([](Windows::Storage::StorageFile^ file)
   {
      if (file != nullptr)
      {
         // Launch the retrieved file
         concurrency::task<bool> launchFileOperation(Windows::System::Launcher::LaunchFileAsync(file));
         launchFileOperation.then([](bool success)
         {
            if (success)
            {
               // File launched
            }
            else
            {
               // File launch failed
            }
         });
      }
      else
      {
         // Could not find file
      }
   });
}
```

### <a name="open-with-launch"></a>시작으로 열기

[**Windows.System를 호출 합니다. LaunchFileAsync (IStorageFile, LauncherOptions) 메서드 (**](/uwp/api/windows.system.launcher.launchfileasync#Windows_System_Launcher_LaunchFileAsync_Windows_Storage_IStorageFile_Windows_System_LauncherOptions_) [**launcheroptions). DisplayApplicationPicker**](/uwp/api/windows.system.launcheroptions.displayapplicationpicker) 는 **연결** 프로그램 대화 상자에서 사용자가 선택 하는 앱을 시작 하려면 **true** 로 설정 합니다.

사용자가 특정 파일에 대해 기본값이 아닌 다른 앱을 선택할 수 있는 경우 **연결** 프로그램 대화 상자를 사용 하는 것이 좋습니다. 예를 들어 사용자가 앱에서 이미지 파일을 시작할 수 있는 경우 기본 처리기는 뷰어 앱이 될 수 있습니다. 경우에 따라 사용자가 이미지를 표시 하는 대신 편집 하려고 할 수도 있습니다. **Appbar** 또는 상황에 맞는 메뉴의 대체 명령과 함께 **열기** 옵션을 사용 하 여 사용자가 **연결 프로그램** 대화 상자를 표시 하 고 이러한 유형의 시나리오에서 편집기 앱을 선택할 수 있습니다.

![.png 파일 시작을 위한 연결 프로그램 대화 상자. 이 대화 상자에는 사용자의 선택 항목을 모든 .png 파일에 사용할지, 아니면 하나의 .png 파일만 사용할지를 지정 하는 확인란이 포함 되어 있습니다. 이 대화 상자에는 파일을 시작 하는 네 가지 앱 옵션과 ' 기타 옵션 ' 링크가 있습니다.](images/checkboxopenwithdialog.png)

```csharp
async void DefaultLaunch()
{
   // Path to the file in the app package to launch
      string imageFile = @"images\test.png";
      
   var file = await Windows.ApplicationModel.Package.Current.InstalledLocation.GetFileAsync(imageFile);

   if (file != null)
   {
      // Set the option to show the picker
      var options = new Windows.System.LauncherOptions();
      options.DisplayApplicationPicker = true;

      // Launch the retrieved file
      bool success = await Windows.System.Launcher.LaunchFileAsync(file, options);
      if (success)
      {
         // File launched
      }
      else
      {
         // File launch failed
      }
   }
   else
   {
      // Could not find file
   }
}
```

```vb
async Sub DefaultLaunch()

   ' Path to the file in the app package to launch
   Dim imageFile = "images\test.png"

   Dim file = await Windows.ApplicationModel.Package.Current.InstalledLocation.GetFileAsync(imageFile)

   If file IsNot Nothing Then
      ' Set the option to show the picker
      Dim options = Windows.System.LauncherOptions()
      options.DisplayApplicationPicker = True

      ' Launch the retrieved file
      Dim success = await Windows.System.Launcher.LaunchFileAsync(file)

      If success Then
         ' File launched
      Else
         ' File launch failed
      End If
   Else
      ' Could not find file
   End If
End Sub
```

```cppwinrt
Windows::Foundation::IAsyncAction MainPage::DefaultLaunch()
{
    auto installFolder{ Windows::ApplicationModel::Package::Current().InstalledLocation() };

    Windows::Storage::StorageFile file{ co_await installFolder.GetFileAsync(L"images\\test.png") };

    if (file)
    {
        // Set the option to show the picker
        Windows::System::LauncherOptions launchOptions;
        launchOptions.DisplayApplicationPicker(true);

        // Launch the retrieved file
        bool success = co_await Windows::System::Launcher::LaunchFileAsync(file, launchOptions);
        if (success)
        {
            // File launched
        }
        else
        {
            // File launch failed
        }
    }
    else
    {
        // Could not find file
    }
}
```

```cpp
void MainPage::DefaultLaunch()
{
   auto installFolder = Windows::ApplicationModel::Package::Current->InstalledLocation;

   concurrency::task<Windows::Storage::StorageFile^> getFileOperation(installFolder->GetFileAsync("images\\test.png"));
   getFileOperation.then([](Windows::Storage::StorageFile^ file)
   {
      if (file != nullptr)
      {
         // Set the option to show the picker
         auto launchOptions = ref new Windows::System::LauncherOptions();
         launchOptions->DisplayApplicationPicker = true;

         // Launch the retrieved file
         concurrency::task<bool> launchFileOperation(Windows::System::Launcher::LaunchFileAsync(file, launchOptions));
         launchFileOperation.then([](bool success)
         {
            if (success)
            {
               // File launched
            }
            else
            {
               // File launch failed
            }
         });
      }
      else
      {
         // Could not find file
      }
   });
}
```

**권장 앱 대체를 사용 하 여 시작**

사용자에 게 시작 하는 파일을 처리 하는 앱이 설치 되어 있지 않은 경우도 있습니다. 기본적으로 Windows에서는 사용자에 게 스토어에서 적절 한 앱을 검색할 수 있는 링크를 제공 하 여 이러한 사례를 처리 합니다. 사용자에 게이 시나리오에서 가져올 앱에 대 한 특정 권장 사항을 제공 하려는 경우 시작 하는 파일과 함께 권장 사항을 전달 하면 됩니다. 이렇게 하려면Windows.System를 호출 [**합니다. LaunchFileAsync (IStorageFile, LauncherOptions)**](/uwp/api/windows.system.launcher.launchfileasync#Windows_System_Launcher_LaunchFileAsync_Windows_Storage_IStorageFile_Windows_System_LauncherOptions_) 메서드를 사용 하 여 권장 하려는 스토어의 앱 패키지 패밀리 이름으로 설정 [**합니다.**](/uwp/api/windows.system.launcheroptions.preferredapplicationpackagefamilyname) 그런 다음 PreferredApplicationDisplayName를 해당 앱의 이름으로 설정 합니다 [**.**](/uwp/api/windows.system.launcheroptions.preferredapplicationdisplayname) Windows에서는이 정보를 사용 하 여 저장소에서 권장 앱을 얻기 위한 특정 옵션을 사용 하 여 스토어에서 앱을 검색 하는 일반 옵션을 대체 합니다.

> [!NOTE]
> 앱을 권장 하려면 이러한 두 옵션을 모두 설정 해야 합니다. 다른 하나를 사용 하지 않고 설정 하면 오류가 발생 합니다.

![contoso 파일 시작을 위한 연결 프로그램 대화 상자. . contoso에는 컴퓨터에 설치 된 처리기가 없으므로이 대화 상자에는 저장소 아이콘과 사용자가 저장소의 올바른 처리기를 가리키는 텍스트가 포함 된 옵션이 포함 되어 있습니다. 또한이 대화 상자에는 ' 추가 옵션 ' 링크 '가 포함 되어 있습니다.](images/howdoyouwanttoopen.png)

```csharp
async void DefaultLaunch()
{
   // Path to the file in the app package to launch
   string imageFile = @"images\test.contoso";

   // Get the image file from the package's image directory
   var file = await Windows.ApplicationModel.Package.Current.InstalledLocation.GetFileAsync(imageFile);

   if (file != null)
   {
      // Set the recommended app
      var options = new Windows.System.LauncherOptions();
      options.PreferredApplicationPackageFamilyName = "Contoso.FileApp_8wknc82po1e";
      options.PreferredApplicationDisplayName = "Contoso File App";

      // Launch the retrieved file pass in the recommended app
      // in case the user has no apps installed to handle the file
      bool success = await Windows.System.Launcher.LaunchFileAsync(file, options);
      if (success)
      {
         // File launched
      }
      else
      {
         // File launch failed
      }
   }
   else
   {
      // Could not find file
   }
}
```

```vb
async Sub DefaultLaunch()

   ' Path to the file in the app package to launch
   Dim imageFile = "images\test.contoso"

   ' Get the image file from the package's image directory
   Dim file = await Windows.ApplicationModel.Package.Current.InstalledLocation.GetFileAsync(imageFile)

   If file IsNot Nothing Then
      ' Set the recommended app
      Dim options = Windows.System.LauncherOptions()
      options.PreferredApplicationPackageFamilyName = "Contoso.FileApp_8wknc82po1e";
      options.PreferredApplicationDisplayName = "Contoso File App";

      ' Launch the retrieved file pass in the recommended app
      ' in case the user has no apps installed to handle the file
      Dim success = await Windows.System.Launcher.LaunchFileAsync(file)

      If success Then
         ' File launched
      Else
         ' File launch failed
      End If
   Else
      ' Could not find file
   End If
End Sub
```

```cppwinrt
Windows::Foundation::IAsyncAction MainPage::DefaultLaunch()
{
    auto installFolder{ Windows::ApplicationModel::Package::Current().InstalledLocation() };

    Windows::Storage::StorageFile file{ co_await installFolder.GetFileAsync(L"images\\test.png") };

    if (file)
    {
        // Set the recommended app
        Windows::System::LauncherOptions launchOptions;
        launchOptions.PreferredApplicationPackageFamilyName(L"Contoso.FileApp_8wknc82po1e");
        launchOptions.PreferredApplicationDisplayName(L"Contoso File App");

        // Launch the retrieved file, and pass in the recommended app
        // in case the user has no apps installed to handle the file.
        bool success = co_await Windows::System::Launcher::LaunchFileAsync(file, launchOptions);
        if (success)
        {
            // File launched
        }
        else
        {
            // File launch failed
        }
    }
    else
    {
        // Could not find file
    }
}
```

```cpp
void MainPage::DefaultLaunch()
{
   auto installFolder = Windows::ApplicationModel::Package::Current->InstalledLocation;

   concurrency::task<Windows::Storage::StorageFile^> getFileOperation(installFolder->GetFileAsync("images\\test.contoso"));
   getFileOperation.then([](Windows::Storage::StorageFile^ file)
   {
      if (file != nullptr)
      {
         // Set the recommended app
         auto launchOptions = ref new Windows::System::LauncherOptions();
         launchOptions->PreferredApplicationPackageFamilyName = "Contoso.FileApp_8wknc82po1e";
         launchOptions->PreferredApplicationDisplayName = "Contoso File App";
         
         // Launch the retrieved file pass, and in the recommended app
         // in case the user has no apps installed to handle the file.
         concurrency::task<bool> launchFileOperation(Windows::System::Launcher::LaunchFileAsync(file, launchOptions));
         launchFileOperation.then([](bool success)
         {
            if (success)
            {
               // File launched
            }
            else
            {
               // File launch failed
            }
         });
      }
      else
      {
         // Could not find file
      }
   });
}
```

### <a name="launch-with-a-desired-remaining-view-windows-only"></a>원하는 나머지 뷰로 시작 (Windows 전용)

[**LaunchFileAsync**](/uwp/api/windows.system.launcher.launchfileasync) 를 호출 하는 원본 앱은 파일을 시작한 후 화면에 유지 되도록 요청할 수 있습니다. 기본적으로 Windows에서는 파일을 처리 하는 대상 앱과 소스 앱 간에 사용 가능한 모든 공간을 동일 하 게 공유 하려고 합니다. 원본 앱은 [**DesiredRemainingView**](/uwp/api/windows.system.launcheroptions.desiredremainingview) 속성을 사용 하 여 사용 가능한 공간을 늘리거나 줄일 수 있도록 응용 프로그램 창을 선호 하는 운영 체제를 나타낼 수 있습니다. **DesiredRemainingView** 를 사용 하 여 파일을 시작한 후 원본 앱이 화면에 남아 있어야 하 고 대상 앱에 의해 완전히 바뀔 수 있음을 나타낼 수도 있습니다. 이 속성은 호출 하는 앱의 기본 창 크기만 지정 합니다. 동시에 화면에 있을 수도 있는 다른 앱의 동작을 지정 하지 않습니다.

> [!NOTE]
> Windows는 원본 앱의 최종 창 크기 (예: 원본 앱의 기본 설정, 화면에 있는 앱의 수, 화면 방향 등)를 결정할 때 여러 가지 요인을 고려 합니다. [**DesiredRemainingView**](/uwp/api/windows.system.launcheroptions.desiredremainingview)를 설정 하 여 원본 앱에 대 한 특정 창 지정 동작을 보장할 수 없습니다.

**모바일 장치 제품군:  **[**DesiredRemainingView**](/uwp/api/windows.system.launcheroptions.desiredremainingview) 은 모바일 장치 제품군에서 지원 되지 않습니다.

```csharp
async void DefaultLaunch()
{
   // Path to the file in the app package to launch
   string imageFile = @"images\test.png";
   
   var file = await Windows.ApplicationModel.Package.Current.InstalledLocation.GetFileAsync(imageFile);

   if (file != null)
   {
      // Set the desired remaining view
      var options = new Windows.System.LauncherOptions();
      options.DesiredRemainingView = Windows.UI.ViewManagement.ViewSizePreference.UseLess;

      // Launch the retrieved file
      bool success = await Windows.System.Launcher.LaunchFileAsync(file, options);
      if (success)
      {
         // File launched
      }
      else
      {
         // File launch failed
      }
   }
   else
   {
      // Could not find file
   }
}
```

```cppwinrt
Windows::Foundation::IAsyncAction MainPage::DefaultLaunch()
{
    auto installFolder{ Windows::ApplicationModel::Package::Current().InstalledLocation() };

    Windows::Storage::StorageFile file{ co_await installFolder.GetFileAsync(L"images\\test.png") };

    if (file)
    {
        // Set the desired remaining view.
        Windows::System::LauncherOptions launchOptions;
        launchOptions.DesiredRemainingView(Windows::UI::ViewManagement::ViewSizePreference::UseLess);

        // Launch the retrieved file.
        bool success = co_await Windows::System::Launcher::LaunchFileAsync(file, launchOptions);
        if (success)
        {
            // File launched
        }
        else
        {
            // File launch failed
        }
    }
    else
    {
        // Could not find file
    }
}
```

```cpp
void MainPage::DefaultLaunch()
{
   auto installFolder = Windows::ApplicationModel::Package::Current->InstalledLocation;

   concurrency::task<Windows::Storage::StorageFile^> getFileOperation(installFolder->GetFileAsync("images\\test.png"));
   getFileOperation.then([](Windows::Storage::StorageFile^ file)
   {
      if (file != nullptr)
      {
         // Set the desired remaining view.
         auto launchOptions = ref new Windows::System::LauncherOptions();
         launchOptions->DesiredRemainingView = Windows::UI::ViewManagement::ViewSizePreference::UseLess;

         // Launch the retrieved file.
         concurrency::task<bool> launchFileOperation(Windows::System::Launcher::LaunchFileAsync(file, launchOptions));
         launchFileOperation.then([](bool success)
         {
            if (success)
            {
               // File launched
            }
            else
            {
               // File launch failed
            }
         });
      }
      else
      {
         // Could not find file
      }
   });
}
```

## <a name="remarks"></a>설명

앱에서 시작 되는 앱을 선택할 수 없습니다. 사용자는 시작 되는 앱을 결정 합니다. 사용자는 UWP (유니버설 Windows 플랫폼) 앱 또는 Windows 데스크톱 앱을 선택할 수 있습니다.

파일을 시작할 때 앱은 전경 앱 이어야 합니다. 즉, 사용자에 게 표시 되어야 합니다. 이 요구 사항은 사용자가 컨트롤에 유지 되도록 하는 데 도움이 됩니다. 이 요구 사항을 충족 하려면 모든 파일 시작을 응용 프로그램의 UI에 직접 연결 해야 합니다. 일반적으로 사용자는 파일 시작을 시작 하기 위해 항상 몇 가지 작업을 수행 해야 합니다.

운영 체제에서 자동으로 실행 하는 경우 코드 또는 스크립트가 포함 된 파일 형식 (예: .exe, .msi 및 .js 파일)을 시작할 수 없습니다. 이 제한은 운영 체제를 수정할 수 있는 악의적인 파일 로부터 사용자를 보호 합니다. 이 메서드를 사용 하 여 스크립트를 격리 하는 응용 프로그램 (예: .docx 파일)에서 실행 되는 경우 스크립트를 포함할 수 있는 파일 형식을 시작할 수 있습니다. Microsoft Word와 같은 앱은 스크립트를 .docx 파일에 보관 하 여 운영 체제를 수정 하지 못하도록 합니다.

제한 된 파일 형식을 시작 하려고 하면 시작이 실패 하 고 오류 콜백이 호출 됩니다. 앱에서 다양 한 형식의 파일을 처리 하 고이 오류가 발생할 것으로 생각 되는 경우 사용자에 게 대체 (fallback) 환경을 제공 하는 것이 좋습니다. 예를 들어 사용자에 게 바탕 화면에 파일을 저장 하는 옵션을 제공 하 고 해당 파일을 열 수 있습니다.

## <a name="related-topics"></a>관련 항목

### <a name="tasks"></a>작업

* [URI에 대한 기본 앱 실행](launch-default-app.md)
* [파일 활성화 처리](handle-file-activation.md)

### <a name="guidelines"></a>지침

* [파일 형식 및 URI에 대한 지침](../files/index.md)

### <a name="reference"></a>참조

* [**Windows.Storage.StorageFile**](/uwp/api/Windows.Storage.StorageFile)
* [** Tem를Windows.Sys합니다. LaunchFileAsync**](/uwp/api/windows.system.launcher.launchfileasync)