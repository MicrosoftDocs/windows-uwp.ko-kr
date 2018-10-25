---
author: mcleanbyron
ms.assetid: 414ACC73-2A72-465C-BD15-1B51CB2334F2
title: Microsoft Store에서 패키지 업데이트 다운로드 및 설치
description: 개발자 센터 대시보드에 패키지를 필수로 표시하고 패키지 업데이트를 다운로드 및 설치하도록 앱에 코드를 작성하는 방법을 알아봅니다.
ms.author: mcleans
ms.date: 04/04/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 39b3ba05b06b6d484804999a935accc7223b5c60
ms.sourcegitcommit: 2c4daa36fb9fd3e8daa83c2bd0825f3989d24be8
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/25/2018
ms.locfileid: "5512895"
---
# <a name="download-and-install-package-updates-from-the-store"></a>Microsoft Store에서 패키지 업데이트 다운로드 및 설치

Windows 10 버전 1607부터 [Windows.Services.Store](https://docs.microsoft.com/uwp/api/windows.services.store) 네임스페이스의 [StoreContext](https://docs.microsoft.com/uwp/api/windows.services.store.storecontext) 클래스의 메서드를 사용하여 Microsoft Store의 현재 앱에 대한 패키지 업데이트를 프로그래밍 방식으로 확인하고 업데이트된 패키지를 다운로드 및 설치할 수 있습니다. Windows 개발자 센터 대시보드에서 필수로 표시한 패키지를 쿼리하고 필수 업데이트가 설치될 때까지 앱의 기능을 사용하지 않도록 할 수 있습니다.

Windows 10 버전 1803에 도입된 추가 [StoreContext](https://docs.microsoft.com/uwp/api/windows.services.store.storecontext) 메서드를 사용하면 자동으로(사용자에게 알림 UI를 표시하지 않고) 패키지 업데이트를 다운로드 및 설치하고, [선택적 패키지](optional-packages.md)를 제거하고, 앱의 다운로드 및 설치 큐의 패키지에 대한 정보를 확인할 수 있습니다.

이러한 기능을 사용하면 Microsoft Store에서 최신 버전의 앱, 선택적 패키지 및 관련 서비스를 사용하여 자동으로 사용자 기반을 최신 상태로 유지할 수 있습니다.

## <a name="download-and-install-package-updates-with-the-users-permission"></a>사용자의 권한을 사용한 다운로드 및 설치 패키지 업데이트

이 코드 예는 [GetAppAndOptionalStorePackageUpdatesAsync](https://docs.microsoft.com/uwp/api/windows.services.store.storecontext.getappandoptionalstorepackageupdatesasync) 메서드를 사용하여 Microsoft Store에서 이용할 수 있는 모든 패키지 업데이트를 검색한 다음 [RequestDownloadAndInstallStorePackageUpdatesAsync](https://docs.microsoft.com/uwp/api/windows.services.store.storecontext.requestdownloadandinstallstorepackageupdatesasync) 메서드를 호출하여 업데이트를 다운로드 및 설치하는 방법을 보여줍니다. 이 메서드를 사용하여 업데이트를 다운로드 및 설치할 때 OS는 업데이트를 다운로드하기 전에 사용자의 권한을 요청하는 대화 상자를 표시합니다.

> [!NOTE]
> 이러한 메서드는 앱에 대한 필수 및 [선택적 패키지](optional-packages.md)를 지원합니다. 선택적 패키지는 다운로드할 수 있는 콘텐츠(DLC) 추가 기능에서 크기 제한을 위해 대용량 앱을 분할하거나, 핵심 앱과의 분리를 위해 추가 콘텐츠를 전달하는 경우에 유용합니다. Microsoft Store에 DLC 추가 기능을 포함한 선택적 패키지를 사용하는 앱을 제출할 권한을 얻으려면 [Windows 개발자 지원](https://developer.microsoft.com/windows/support)을 참조하세요.

이 코드 예는 다음을 가정합니다.

* 코드가 [Page](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.page.aspx) 컨텍스트에서 실행됩니다.
* **Page**에 다운로드 작업의 상태를 제공하는 ```downloadProgressBar```라는 이름의 [ProgressBar](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.progressbar.aspx)가 있습니다.
* 코드 파일에는 **Windows.Services.Store**, **Windows.Threading.Tasks** 및 **Windows.UI.Popups** 네임스페이스에 대한 **using** 문이 있습니다.
* 앱은 해당 앱을 실행한 사용자의 컨텍스트에서만 실행되는 단일 사용자 앱입니다. [다중 사용자 앱](https://msdn.microsoft.com/windows/uwp/xbox-apps/multi-user-applications)의 경우 [GetDefault](https://docs.microsoft.com/uwp/api/windows.services.store.storecontext.GetDefault) 메서드 대신 [GetForUser](https://docs.microsoft.com/uwp/api/windows.services.store.storecontext.User) 메서드를 사용하여 **StoreContext** 개체를 가져옵니다.

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

> [!NOTE]
> 사용할 수 있는 패키지 업데이트를 다운로드만 하고 설치하지 않으려면 [RequestDownloadStorePackageUpdatesAsync](https://docs.microsoft.com/uwp/api/windows.services.store.storecontext.requestdownloadstorepackageupdatesasync) 메서드를 사용합니다.

### <a name="display-download-and-install-progress-info"></a>다운로드 및 설치 진행률 정보 표시

[RequestDownloadStorePackageUpdatesAsync](https://docs.microsoft.com/uwp/api/windows.services.store.storecontext.requestdownloadstorepackageupdatesasync) 또는 [RequestDownloadAndInstallStorePackageUpdatesAsync](https://docs.microsoft.com/uwp/api/windows.services.store.storecontext.requestdownloadandinstallstorepackageupdatesasync) 메서드를 호출할 때, 이 요청의 각 패키지에 대해 다운로드(또는 다운로드 및 설치)의 각 단계마다 한 번씩 호출되는 [Progress](https://docs.microsoft.com/uwp/api/windows.foundation.iasyncoperationwithprogress-2.progress) 처리기를 할당할 수 있습니다. 처리기는 [StorePackageUpdateStatus](https://docs.microsoft.com/uwp/api/windows.services.store.storepackageupdatestatus) 개체를 받으며, 이 개체는 진행률 알림을 보내는 업데이트 패키지에 대한 정보를 제공합니다. 이전 예는 **StorePackageUpdateStatus** 개체의 **PackageDownloadProgress** 필드를 사용하여 다운로드와 설치 프로세스의 진행률을 표시합니다.

패키지 다운로드와 설치를 하나의 작업으로 수행하기 위해 **RequestDownloadAndInstallStorePackageUpdatesAsync**를 호출할 경우, **PackageDownloadProgress** 필드는 패키지 다운로드 프로세스 동안 0.0에서 0.8까지 증가하고 설치하는 동안 0.8에서 1.0까지 증가함을 유의하십시오. 따라서 사용자 지정 진행률 UI에 표시되는 비율을 **PackageDownloadProgress** 필드의 값에 직접 매핑할 경우, 패키지 다운로드가 완료되면 UI가 80%까지 표시되고 OS가 설치 대화 상자를 표시합니다. 패키지가 다운로드되어 설치할 준비가 되었을 때 100%로 사용자 지정 진행률 UI가 표시되도록 하려면, **PackageDownloadProgress** 필드가 0.8에 도달했을 때 진행률 UI에 100%를 할당하도록 코드를 수정해야 합니다.

## <a name="download-and-install-package-updates-silently"></a>자동으로 패키지 업데이트 다운로드 및 설치

Windows 10 버전 1803부터 [TrySilentDownloadStorePackageUpdatesAsync](https://docs.microsoft.com/uwp/api/windows.services.store.storecontext.trysilentdownloadstorepackageupdatesasync) 및 [TrySilentDownloadAndInstallStorePackageUpdatesAsync](https://docs.microsoft.com/uwp/api/windows.services.store.storecontext.trysilentdownloadandinstallstorepackageupdatesasync) 메서드를 사용하여 사용자에 게 알림 UI를 표시하지 않고 패키지 업데이트를 자동으로 다운로드 및 설치할 수 있습니다. 사용자가 Microsoft Store에서 **자동으로 앱 업데이트** 설정을 활성화하고 사용자가 데이터 통신 연결 네트워크에 있지 않는 경우에만 이 작업은 성공합니다. 이러한 메서드를 호출하기 전에 먼저 [CanSilentlyDownloadStorePackageUpdates](https://docs.microsoft.com/uwp/api/windows.services.store.storecontext.cansilentlydownloadstorepackageupdates) 속성을 확인하여 해당 조건이 현재 충족되는지 확인할 수 있습니다.

이 코드 예는 [GetAppAndOptionalStorePackageUpdatesAsync](https://docs.microsoft.com/uwp/api/windows.services.store.storecontext.getappandoptionalstorepackageupdatesasync) 메서드를 사용하여 이용할 수 있는 모든 패키지 업데이트를 검색한 다음 [TrySilentDownloadStorePackageUpdatesAsync](https://docs.microsoft.com/uwp/api/windows.services.store.storecontext.trysilentdownloadstorepackageupdatesasync) 및 [TrySilentDownloadAndInstallStorePackageUpdatesAsync](https://docs.microsoft.com/uwp/api/windows.services.store.storecontext.trysilentdownloadandinstallstorepackageupdatesasync) 메서드를 호출하여 업데이트를 다운로드 및 설치하는 방법을 보여줍니다.

이 코드 예는 다음을 가정합니다.
* 코드 파일에는 **Windows.Services.Store** 및 **System.Threading.Tasks** 네임스페이스에 대한 **using** 문이 있습니다.
* 앱은 해당 앱을 실행한 사용자의 컨텍스트에서만 실행되는 단일 사용자 앱입니다. [다중 사용자 앱](https://msdn.microsoft.com/windows/uwp/xbox-apps/multi-user-applications)의 경우 [GetDefault](https://docs.microsoft.com/uwp/api/windows.services.store.storecontext.GetDefault) 메서드 대신 [GetForUser](https://docs.microsoft.com/uwp/api/windows.services.store.storecontext.User) 메서드를 사용하여 **StoreContext** 개체를 가져옵니다.

> [!NOTE]
> 이 예에서 코드에 의해 호출된 **IsNowAGoodTimeToRestartApp**, **RetryDownloadAndInstallLater**, 및 **RetryInstallLater** 메서드는 원하는 앱 디자인의 필요에 따라 구현을 돕기 위한 자리 표시자 메서드입니다.

```csharp
private StoreContext context = null;

public async Task DownloadAndInstallAllUpdatesInBackgroundAsync()
{
    if (context == null)
    {
        context = StoreContext.GetDefault();
    }

    // Get the updates that are available.
    IReadOnlyList<StorePackageUpdate> storePackageUpdates =
        await context.GetAppAndOptionalStorePackageUpdatesAsync();

    if (storePackageUpdates.Count > 0)
    {

        if (!context.CanSilentlyDownloadStorePackageUpdates)
        {
            return;
        }

        // Start the silent downloads and wait for the downloads to complete.
        StorePackageUpdateResult downloadResult =
            await context.TrySilentDownloadStorePackageUpdatesAsync(storePackageUpdates);

        switch (downloadResult.OverallState)
        {
            case StorePackageUpdateState.Completed:
                // The download has completed successfully. At this point, confirm whether your app
                // can restart now and then install the updates (for example, you might only install
                // packages silently if your app has been idle for a certain period of time). The
                // IsNowAGoodTimeToRestartApp method is not implemented in this example, you should
                // implement it as needed for your own app.
                if (IsNowAGoodTimeToRestartApp())
                {
                    await InstallUpdate(storePackageUpdates);
                }
                else
                {
                    // Retry/reschedule the installation later. The RetryInstallLater method is not  
                    // implemented in this example, you should implement it as needed for your own app.
                    RetryInstallLater();
                    return;
                }
                break;
            // If the user cancelled the download or you can't perform the download for some other
            // reason (for example, Wi-Fi might have been turned off and the device is now on
            // a metered network) try again later. The RetryDownloadAndInstallLater method is not  
            // implemented in this example, you should implement it as needed for your own app.
            case StorePackageUpdateState.Canceled:
            case StorePackageUpdateState.ErrorLowBattery:
            case StorePackageUpdateState.ErrorWiFiRecommended:
            case StorePackageUpdateState.ErrorWiFiRequired:
            case StorePackageUpdateState.OtherError:
                RetryDownloadAndInstallLater();
                return;
            default:
                break;
        }
    }
}

private async Task InstallUpdate(IReadOnlyList<StorePackageUpdate> storePackageUpdates)
{
    // Start the silent installation of the packages. Because the packages have already
    // been downloaded in the previous method, the following line of code just installs
    // the downloaded packages.
    StorePackageUpdateResult downloadResult =
        await context.TrySilentDownloadAndInstallStorePackageUpdatesAsync(storePackageUpdates);

    switch (downloadResult.OverallState)
    {
        // If the user cancelled the installation or you can't perform the installation  
        // for some other reason, try again later. The RetryInstallLater method is not  
        // implemented in this example, you should implement it as needed for your own app.
        case StorePackageUpdateState.Canceled:
        case StorePackageUpdateState.ErrorLowBattery:
        case StorePackageUpdateState.OtherError:
            RetryInstallLater();
            return;
        default:
            break;
    }
}
```

## <a name="mandatory-package-updates"></a>필수 패키지 업데이트

Windows 10 버전 1607 이상을 대상으로 하는 앱을 위해 패키지 제출을 만들 때 [해당 패키지를 필수로 표시](../publish/upload-app-packages.md#mandatory-update)하고 필수가 되는 날짜/시간을 표시할 수 있습니다. 이 속성이 설정되고 앱에서 패키지 업데이트를 사용할 수 있다고 판단한 경우 앱에서는 업데이트 패키지가 필수인지 확인하고 업데이트가 설치될 때까지 동작을 변경합니다(예를 들어 앱에서 기능을 비활성화할 수 있음).

> [!NOTE]
> Microsoft는 패키지 업데이트를 위한 필수 상태를 적용하지 않으며, OS는 필수 앱 업데이트를 설치해야 할 사용자를 나타내기 위한 UI를 제공하지 않습니다. 개발자가 필수 설정을 사용하여 자체 코드에서 필수 앱 업데이트를 적용합니다.  

패키지 제출을 필수로 표시하려면

1. [개발자 센터 대시보드](https://dev.windows.com/overview)에 로그인하고 앱 개요 페이지로 이동합니다.
2. 필수로 만들 패키지 업데이트가 포함된 제출의 이름을 클릭합니다.
3. 제출의 **패키지** 페이지로 이동합니다. 이 페이지 하단에서 **이 업데이트를 필수로 설정하세요.** 를 선택한 다음 패키지 업데이트가 필수가 되는 날짜와 시간을 선택합니다. 이 옵션은 제출의 모든 UWP 패키지에 적용됩니다.

자세한 내용은 [앱 패키지 업로드](../publish/upload-app-packages.md)를 참조하세요.

> [!NOTE]
> [패키지 플라이트](../publish/package-flights.md)를 만드는 경우, 플라이트에 대한 **패키지** 페이지에서 유사한 UI를 사용하여 패키지를 필수로 표시할 수 있습니다. 이 경우 필수 패키지 업데이트는 해당 플라이트 그룹에 속한 고객에게만 적용됩니다.

### <a name="code-example-for-mandatory-packages"></a>필수 패키지에 대한 코드 예

다음 코드 예제에서는 모든 필수 패키지 업데이트를 확인하는 방법을 보여 줍니다. 일반적으로 필수 패키지 업데이트가 성공적으로 다운로드 또는 설치되지 않은 경우 사용자를 위해 앱 환경을 정상적으로 다운그레이드해야 합니다.

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

## <a name="uninstall-optional-packages"></a>선택적 패키지 제거

Windows 10 버전 1803부터 [RequestUninstallStorePackageAsync](https://docs.microsoft.com/uwp/api/windows.services.store.storecontext.requestuninstallstorepackageasync) 또는 [RequestUninstallStorePackageByStoreIdAsync](https://docs.microsoft.com/uwp/api/windows.services.store.storecontext.requestuninstallstorepackagebystoreidasync) 메서드를 사용하여 현재 앱에 대해 DLC 패키지 포함 [선택적 패키지](optional-packages.md)를 제거할 수 있습니다. 예를 들어 선택적 패키지를 통해 설치된 콘텐츠가 있는 앱을 사용하는 경우 디스크 공간을 확보하기 위해 사용자가 선택적 패키지를 제거할 수 있도록 UI를 제공하고자 할 수 있습니다.

다음 코드 예제에서는 [RequestUninstallStorePackageAsync](https://docs.microsoft.com/uwp/api/windows.services.store.storecontext.requestuninstallstorepackageasync)를 호출하는 방법을 보여 줍니다. 이 예에서는 다음을 가정합니다.
* 코드 파일에는 **Windows.Services.Store** 및 **System.Threading.Tasks** 네임스페이스에 대한 **using** 문이 있습니다.
* 앱은 해당 앱을 실행한 사용자의 컨텍스트에서만 실행되는 단일 사용자 앱입니다. [다중 사용자 앱](https://msdn.microsoft.com/windows/uwp/xbox-apps/multi-user-applications)의 경우 [GetDefault](https://docs.microsoft.com/uwp/api/windows.services.store.storecontext.GetDefault) 메서드 대신 [GetForUser](https://docs.microsoft.com/uwp/api/windows.services.store.storecontext.User) 메서드를 사용하여 **StoreContext** 개체를 가져옵니다.

```csharp
public async Task UninstallPackage(Windows.ApplicationModel.Package package)
{
    if (context == null)
    {
        context = StoreContext.GetDefault();
    }

    var storeContext = StoreContext.GetDefault();
    IAsyncOperation<StoreUninstallStorePackageResult> uninstallOperation =
        storeContext.RequestUninstallStorePackageAsync(package);

    // At this point, you can update your app UI to show that the package
    // is installing.

    uninstallOperation.Completed += (asyncInfo, status) =>
    {
        StoreUninstallStorePackageResult result = uninstallOperation.GetResults();
        switch (result.Status)
        {
            case StoreUninstallStorePackageStatus.Succeeded:
                {
                    // Update your app UI to show the package as uninstalled.
                    break;
                }
            default:
                {
                    // Update your app UI to show that the package uninstall failed.
                    break;
                }
        }
    };
}
```

## <a name="get-download-queue-info"></a>다운로드 큐 정보 확인

Windows 10 버전 1803부터 [GetAssociatedStoreQueueItemsAsync](https://docs.microsoft.com/uwp/api/windows.services.store.storecontext.getassociatedstorequeueitemsasync) 및 [GetStoreQueueItemsAsync](https://docs.microsoft.com/uwp/api/windows.services.store.storecontext.getstorequeueitemsasync) 메서드를 사용하여 Microsoft Store의 현재 다운로드 및 설치 큐에 있는 패키지에 대한 정보를 얻을 수 있습니다. 이러한 메서드는 앱 또는 게임이 다운로드하고 설치하는 데 몇 시간에서 며칠이 걸릴 수 있는 대용량 선택적 패키지(DLC 포함)를 지원하고, 다운로드 및 설치 프로세스가 완료되기 전에 고객이 앱 또는 게임을 종료하는 경우를 원만하게 처리하고자 하는 경우 유용합니다. 고객이 앱 또는 게임을 다시 시작하면 코드는 이러한 메서드를 사용하여 아직 다운로드 및 설치 큐에 있는 패키지의 상태에 대한 정보를 얻고, 고객에게 각 패키지의 상태를 표시할 수 있습니다.

다음 코드 예는 [GetAssociatedStoreQueueItemsAsync](https://docs.microsoft.com/uwp/api/windows.services.store.storecontext.getassociatedstorequeueitemsasync)를 호출하여 현재 앱에 대한 진행 중인 패키지 업데이트의 목록을 가져오고 각 패키지에 대한 상태 정보를 검색하는 방법을 보여줍니다. 이 예에서는 다음을 가정합니다.
* 코드 파일에는 **Windows.Services.Store** 및 **System.Threading.Tasks** 네임스페이스에 대한 **using** 문이 있습니다.
* 앱은 해당 앱을 실행한 사용자의 컨텍스트에서만 실행되는 단일 사용자 앱입니다. [다중 사용자 앱](https://msdn.microsoft.com/windows/uwp/xbox-apps/multi-user-applications)의 경우 [GetDefault](https://docs.microsoft.com/uwp/api/windows.services.store.storecontext.GetDefault) 메서드 대신 [GetForUser](https://docs.microsoft.com/uwp/api/windows.services.store.storecontext.User) 메서드를 사용하여 **StoreContext** 개체를 가져옵니다.

> [!NOTE]
> 이 예에서 코드에 의해 호출된 **MarkUpdateInProgressInUI**, **RemoveItemFromUI**, **MarkInstallCompleteInUI**, **MarkInstallErrorInUI**, 및 **MarkInstallPausedInUI** 메서드는 앱 디자인의 필요에 따라 구현을 지원하기 위한 자리 표시자 메서드입니다.

```csharp
private StoreContext context = null;

private async Task GetQueuedInstallItemsAndBuildInitialStoreUI()
{
    if (context == null)
    {
        context = StoreContext.GetDefault();
    }

    // Get the Store packages in the install queue.
    IReadOnlyList<StoreQueueItem> storeUpdateItems = await context.GetAssociatedStoreQueueItemsAsync();

    foreach (StoreQueueItem storeItem in storeUpdateItems)
    {
        // In this example we only care about package updates.
        if (storeItem.InstallKind != StoreQueueItemKind.Update)
            continue;

        StoreQueueItemStatus currentStatus = storeItem.GetCurrentStatus();
        StoreQueueItemState installState = currentStatus.PackageInstallState;
        StoreQueueItemExtendedState extendedInstallState =
            currentStatus.PackageInstallExtendedState;

        // Handle the StatusChanged event to display current status to the customer.
        storeItem.StatusChanged += StoreItem_StatusChanged;

        switch (installState)
        {
            // Download and install are still in progress, so update the status for this  
            // item and provide the extended state info. The following methods are not
            // implemented in this example; you should implement them as needed for your
            // app's UI.
            case StoreQueueItemState.Active:
                MarkUpdateInProgressInUI(storeItem, extendedInstallState);
                break;
            case StoreQueueItemState.Canceled:
                RemoveItemFromUI(storeItem);
                break;
            case StoreQueueItemState.Completed:
                MarkInstallCompleteInUI(storeItem);
                break;
            case StoreQueueItemState.Error:
                MarkInstallErrorInUI(storeItem);
                break;
            case StoreQueueItemState.Paused:
                MarkInstallPausedInUI(storeItem, installState, extendedInstallState);
                break;
        }
    }
}

private void StoreItem_StatusChanged(StoreQueueItem sender, object args)
{
    StoreQueueItemStatus currentStatus = sender.GetCurrentStatus();
    StoreQueueItemState installState = currentStatus.PackageInstallState;
    StoreQueueItemExtendedState extendedInstallState = currentStatus.PackageInstallExtendedState;

    switch (installState)
    {
        // Download and install are still in progress, so update the status for this  
        // item and provide the extended state info. The following methods are not
        // implemented in this example; you should implement them as needed for your
        // app's UI.
        case StoreQueueItemState.Active:
            MarkUpdateInProgressInUI(sender, extendedInstallState);
            break;
        case StoreQueueItemState.Canceled:
            RemoveItemFromUI(sender);
            break;
        case StoreQueueItemState.Completed:
            MarkInstallCompleteInUI(sender);
            break;
        case StoreQueueItemState.Error:
            MarkInstallErrorInUI(sender);
            break;
        case StoreQueueItemState.Paused:
            MarkInstallPausedInUI(sender, installState, extendedInstallState);
            break;
    }
}
```

## <a name="related-topics"></a>관련 항목

* [선택적 패키지 및 관련 집합 제작](optional-packages.md)
