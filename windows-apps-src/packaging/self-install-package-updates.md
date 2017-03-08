---
author: mcleanbyron
ms.assetid: 414ACC73-2A72-465C-BD15-1B51CB2334F2
title: "앱에 대한 패키지 업데이트 다운로드 및 설치"
description: "개발자 센터 대시보드에 패키지를 필수로 표시하고 패키지 업데이트를 다운로드 및 설치하도록 앱에 코드를 작성하는 방법을 알아봅니다."
ms.author: mcleans
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
translationtype: Human Translation
ms.sourcegitcommit: c6b64cff1bbebc8ba69bc6e03d34b69f85e798fc
ms.openlocfilehash: ffd1af9475791b8190f9364d85f7a7fa23548856
ms.lasthandoff: 02/07/2017

---
# <a name="download-and-install-package-updates-for-your-app"></a>앱에 대한 패키지 업데이트 다운로드 및 설치

\[ Windows 10의 UWP 앱에 맞게 업데이트되었습니다. Windows 8.x 문서는 [보관](http://go.microsoft.com/fwlink/p/?linkid=619132)을 참조하세요. \]

Windows 10 버전 1607부터 [Windows.Services.Store](https://msdn.microsoft.com/library/windows/apps/windows.services.store.aspx) 네임스페이스에서 API를 사용하여 현재 앱에 대한 패키지 업데이트를 프로그래밍 방식으로 확인하고 업데이트된 패키지를 다운로드 및 설치할 수 있습니다. [Windows 개발자 센터 대시보드에 필수로 표시된](#mandatory-dashboard) 패키지를 쿼리하고 필수 업데이트가 설치될 때까지 앱의 기능을 사용하지 않도록 할 수 있습니다.

이러한 기능을 사용하면 최신 버전의 앱 및 관련 서비스를 사용하여 자동으로 사용자 기반을 최신 상태로 유지할 수 있습니다.

## <a name="api-overview"></a>API 개요

Windows 10 버전 1607 이상을 대상으로 하는 앱은 [StoreContext](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storecontext.aspx) 클래스의 다음 메서드를 사용하여 패키지 업데이트를 다운로드하고 설치할 수 있습니다.

|  메서드  |  설명  |
|----------|---------------|
| [GetAppAndOptionalStorePackageUpdatesAsync](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storecontext.getappandoptionalstorepackageupdatesasync.aspx) | 사용할 수 있는 패키지 업데이트의 목록을 가져오려면 이 메서드를 호출합니다.<br/><br/>**중요**&nbsp;&nbsp; 패키지에서 인증 프로세스를 통과한 후 [GetAppAndOptionalStorePackageUpdatesAsync](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storecontext.getappandoptionalstorepackageupdatesasync.aspx) 메서드에서 패키지 업데이트를 앱에서 사용할 수 있다고 인식할 때까지 최대 1일의 지연 시간이 있습니다. |
| [RequestDownloadStorePackageUpdatesAsync](https://msdn.microsoft.com/library/windows/apps/mt706586.aspx) | 사용 가능한 패키지 업데이트를 다운로드하려면(설치하지는 않음) 이 메서드를 호출합니다. 이 OS에 업데이트를 다운로드할 수 있는 사용자 권한을 요청하는 대화 상자가 표시됩니다. |
| [RequestDownloadAndInstallStorePackageUpdatesAsync](https://msdn.microsoft.com/library/windows/apps/mt706585.aspx) | 사용 가능한 패키지 업데이트를 다운로드 및 설치하려면 이 메서드를 호출합니다. OS에 업데이트를 다운로드 및 설치하기 위한 사용자의 권한을 요청하는 대화 상자가 표시됩니다. [RequestDownloadStorePackageUpdatesAsync](https://msdn.microsoft.com/library/windows/apps/mt706586.aspx)를 호출하여 패키지 업데이트를 이미 다운로드한 경우 이 메서드는 다운로드 프로세스를 건너뛰고 업데이트 설치만 진행합니다.  |

<span/>

이러한 메서드는 [StorePackageUpdate](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storepackageupdate.aspx) 개체를 사용하여 사용 가능한 업데이트 패키지를 표시합니다. 업데이트 패키지에 대한 정보를 가져오려면 다음의 [StorePackageUpdate](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storepackageupdate.aspx) 속성을 사용하세요.

|  속성  |  설명  |
|----------|---------------|
| [Mandatory](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storepackageupdate.mandatory.aspx) | 이 속성을 사용하여 개발자 센터 대시보드에 패키지가 필수로 표시되어 있는지 확인합니다. |
| [Package](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storepackageupdate.package.aspx) | 이 속성을 사용하여 기본 패키지 관련 데이터에 액세스합니다. |

<span/>

## <a name="code-examples"></a>코드 예제

다음 코드 예제에서는 앱에서 패키지 업데이트를 다운로드하고 설치하는 방법을 보여 줍니다. 이러한 예제에서는 다음을 가정합니다.
* 코드가 [Page](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.page.aspx) 컨텍스트에서 실행됩니다.
* **Page**에 다운로드 작업의 상태를 제공하는 ```downloadProgressBar```라는 이름의 [ProgressBar](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.progressbar.aspx)가 있습니다.
* 코드 파일에는 [Windows.Services.Store](https://msdn.microsoft.com/library/windows/apps/windows.services.store.aspx) 네임스페이스에 대한 **using** 문이 있습니다.
* 앱은 해당 앱을 실행한 사용자의 컨텍스트에서만 실행되는 단일 사용자 앱입니다. [다중 사용자 앱](https://msdn.microsoft.com/windows/uwp/xbox-apps/multi-user-applications)의 경우 [GetDefault](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storecontext.getdefault.aspx) 메서드 대신 [GetForUser](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storecontext.getforuser.aspx) 메서드를 사용하여 [StoreContext](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storecontext.aspx) 개체를 가져옵니다.

<span/>

### <a name="download-and-install-all-package-updates"></a>모든 패키지 업데이트 다운로드 및 설치

다음 코드 예제에서는 사용 가능한 모든 패키지 업데이트를 다운로드 및 설치하는 방법을 보여 줍니다.  

```csharp
private StoreContext context = null;

public async Task DownloadAndInstallAllUpdatesAsync()
{
    if (context == null)
    {
        context = StoreContext.GetDefault();
    }

    // Get the updates that are available.
    IReadOnlyList<StorePackageUpdate> updates =
        await context.GetAppAndOptionalStorePackageUpdatesAsync();

    if (updates.Count > 0)
    {
        // Alert the user that updates are available and ask for their consent
        // to start the updates.
        MessageDialog dialog = new MessageDialog(
            "Download and install updates now? This may cause the application to exit.", "Download and Install?");
        dialog.Commands.Add(new UICommand("Yes"));
        dialog.Commands.Add(new UICommand("No"));
        IUICommand command = await dialog.ShowAsync();

        if (command.Label.Equals("Yes", StringComparison.CurrentCultureIgnoreCase))
        {
            // Download and install the updates.
            IAsyncOperationWithProgress<StorePackageUpdateResult, StorePackageUpdateStatus> downloadOperation =
                context.RequestDownloadAndInstallStorePackageUpdatesAsync(updates);

            // The Progress async method is called one time for each step in the download
            // and installation process for each package in this request.
            downloadOperation.Progress = async (asyncInfo, progress) =>
            {
                await this.Dispatcher.RunAsync(Windows.UI.Core.CoreDispatcherPriority.Normal,
                () =>
                {
                    downloadProgressBar.Value = progress.PackageDownloadProgress;
                });
            };

            StorePackageUpdateResult result = await downloadOperation.AsTask();
        }
    }
}
```

### <a name="handle-mandatory-package-updates"></a>필수 패키지 업데이트 처리

다음 코드 예제는 이전 예제를 기반으로 빌드하고 업데이트 패키지가 [Windows 개발자 센터 대시보드에 필수로 표시](#mandatory-dashboard)되었는지 확인하는 방법을 보여 줍니다. 일반적으로 필수 패키지 업데이트가 성공적으로 다운로드 또는 설치되지 않은 경우 사용자를 위해 앱 환경을 정상적으로 다운그레이드해야 합니다.

```csharp
private StoreContext context = null;

// Downloads and installs package updates in separate steps.
public async Task DownloadAndInstallAllUpdatesAsync()
{
    if (context == null)
    {
        context = StoreContext.GetDefault();
    }  
    // Get the updates that are available.
    IReadOnlyList<StorePackageUpdate> updates =
        await context.GetAppAndOptionalStorePackageUpdatesAsync();

    if (updates.Count != 0)
    {
        // Download the packages.
        bool downloaded = await DownloadPackageUpdatesAsync(updates);

        if (downloaded)
        {
            // Install the packages.
            await InstallPackageUpdatesAsync(updates);
        }
    }
}

// Helper method for downloading package updates.
private async Task<bool> DownloadPackageUpdatesAsync(IEnumerable<StorePackageUpdate> updates)
{
    bool downloadedSuccessfully = false;

    IAsyncOperationWithProgress<StorePackageUpdateResult, StorePackageUpdateStatus> downloadOperation =
        this.context.RequestDownloadStorePackageUpdatesAsync(updates);

    // The Progress async method is called one time for each step in the download process for each
    // package in this request.
    downloadOperation.Progress = async (asyncInfo, progress) =>
    {
        await this.Dispatcher.RunAsync(Windows.UI.Core.CoreDispatcherPriority.Normal,
        () =>
        {
            downloadProgressBar.Value = progress.PackageDownloadProgress;
        });
    };

    StorePackageUpdateResult result = await downloadOperation.AsTask();

    switch (result.OverallState)
    {
        case StorePackageUpdateState.Completed:
            downloadedSuccessfully = true;
            break;
        default:
            // Get the failed updates.
            var failedUpdates = result.StorePackageUpdateStatuses.Where(
                status => status.PackageUpdateState != StorePackageUpdateState.Completed);

            // See if any failed updates were mandatory
            if (updates.Any(u => u.Mandatory && failedUpdates.Any(
                failed => failed.PackageFamilyName == u.Package.Id.FamilyName)))
            {
                // At least one of the updates is mandatory. Perform whatever actions you
                // want to take for your app: for example, notify the user and disable
                // features in your app.
                HandleMandatoryPackageError();
            }
            break;
    }

    return downloadedSuccessfully;
}

// Helper method for installing package updates.
private async Task InstallPackageUpdatesAsync(IEnumerable<StorePackageUpdate> updates)
{
    IAsyncOperationWithProgress<StorePackageUpdateResult, StorePackageUpdateStatus> installOperation =
        this.context.RequestDownloadAndInstallStorePackageUpdatesAsync(updates);

    // The package updates were already downloaded separately, so this method skips the download
    // operatation and only installs the updates; no download progress notifications are provided.
    StorePackageUpdateResult result = await installOperation.AsTask();

    switch (result.OverallState)
    {
        case StorePackageUpdateState.Completed:
            break;
        default:
            // Get the failed updates.
            var failedUpdates = result.StorePackageUpdateStatuses.Where(
                status => status.PackageUpdateState != StorePackageUpdateState.Completed);

            // See if any failed updates were mandatory
            if (updates.Any(u => u.Mandatory && failedUpdates.Any(failed => failed.PackageFamilyName == u.Package.Id.FamilyName)))
            {
                // At least one of the updates is mandatory, so tell the user.
                HandleMandatoryPackageError();
            }
            break;
    }
}

// Helper method for handling the scenario where a mandatory package update fails to
// download or install. Add code to this method to perform whatever actions you want
// to take, such as notifying the user and disabling features in your app.
private void HandleMandatoryPackageError()
{
}
```

<span id="mandatory-dashboard" />
## <a name="make-a-package-submission-mandatory-in-the-dev-center-dashboard"></a>개발자 센터 대시보드에 패키지 제출 필수 만들기

Windows 10 버전 1607 이상을 대상으로 하는 앱을 위해 패키지 제출을 만들면 해당 패키지를 필수로 표시하고 필수가 되는 날짜/시간을 표시할 수 있습니다. 이 속성이 설정되고 앱에서 이 문서의 앞부분에서 설명한 API를 사용하여 패키지 업데이트를 사용할 수 있다고 판단한 경우 앱에서는 업데이트 패키지가 필수인지 확인하고 업데이트가 설치될 때까지 동작을 변경합니다(예를 들어 앱에서 기능을 비활성화할 수 있음).

>**참고**&nbsp;&nbsp;Microsoft는 패키지 업데이트를 위한 필수 상태를 적용하지 않으며, OS는 필수 앱 업데이트를 설치해야 할 사용자를 나타내기 위한 UI를 제공하지 않습니다. 개발자가 필수 설정을 사용하여 자체 코드에서 필수 앱 업데이트를 적용합니다.  

패키지 제출을 필수로 표시하려면

1. [개발자 센터 대시보드](https://dev.windows.com/overview)에 로그인하고 앱 개요 페이지로 이동합니다.
2. 필수로 만들 패키지 업데이트가 포함된 제출의 이름을 클릭합니다.
3. 제출의 **패키지** 페이지로 이동합니다. 이 페이지 하단에서 **이 업데이트를 필수로 설정하세요.**를 선택한 다음 패키지 업데이트가 필수가 되는 날짜와 시간을 선택합니다. 이 옵션은 제출의 모든 UWP 패키지에 적용됩니다.

개발자 센터 대시보드에서 패키지를 구성하는 방법은 [앱 패키지 업로드](https://msdn.microsoft.com/windows/uwp/publish/upload-app-packages)를 참조하세요.

  >**참고**&nbsp;&nbsp;[패키지 플라이트](https://msdn.microsoft.com/windows/uwp/publish/package-flights)를 만든 경우 플라이트에 대한 **패키지** 페이지에서 유사한 UI를 사용하여 패키지를 필수로 표시할 수 있습니다. 이 경우 필수 패키지 업데이트는 해당 플라이트 그룹에 속한 고객에게만 적용됩니다.

