---
ms.assetid: 414ACC73-2A72-465C-BD15-1B51CB2334F2
title: Microsoft Store에서 패키지 업데이트 다운로드 및 설치
description: 파트너 센터에 패키지를 필수로 표시하고 패키지 업데이트를 다운로드 및 설치하도록 앱에서 코드를 작성하는 방법을 알아봅니다.
ms.date: 04/04/2018
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: bec08dfa19f0adab12d6337c00e19bd98459bf3a
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89164337"
---
# <a name="download-and-install-package-updates-from-the-store"></a>Microsoft Store에서 패키지 업데이트 다운로드 및 설치

Windows 10 버전 1607부터 [Windows.Services.Store](/uwp/api/windows.services.store) 네임스페이스에 있는 [StoreContext](/uwp/api/windows.services.store.storecontext) 클래스의 메서드를 사용하여 Microsoft Store에서 현재 앱의 패키지 업데이트를 프로그래밍 방식으로 확인하고 업데이트된 패키지를 다운로드 및 설치할 수 있습니다. 파트너 센터에 필수로 표시한 패키지를 쿼리하고, 필수 업데이트가 설치될 때까지 앱의 기능을 사용하지 않도록 설정할 수도 있습니다.

Windows 10, 버전 1803에서 도입된 추가 [StoreContext](/uwp/api/windows.services.store.storecontext) 메서드를 사용하면 사용자에게 알림 UI를 표시하지 않고 자동으로 패키지 업데이트를 다운로드 및 설치하고, [선택적 패키지](/windows/msix/package/optional-packages)를 제거하고, 앱의 다운로드 및 설치 큐에 있는 패키지 정보를 가져올 수 있습니다.

해당 기능은 Microsoft Store에 있는 최신 버전의 앱, 선택적 패키지 및 관련 서비스로 사용자 기반을 최신 상태로 자동 유지하는 데 도움이 됩니다.

## <a name="download-and-install-package-updates-with-the-users-permission"></a>사용자의 권한으로 패키지 업데이트 다운로드 및 설치

다음 코드 예제는 [GetAppAndOptionalStorePackageUpdatesAsync](/uwp/api/windows.services.store.storecontext.getappandoptionalstorepackageupdatesasync) 메서드를 사용하여 Microsoft Store에서 이용할 수 있는 모든 패키지 업데이트를 검색한 다음, [RequestDownloadAndInstallStorePackageUpdatesAsync](/uwp/api/windows.services.store.storecontext.requestdownloadandinstallstorepackageupdatesasync) 메서드를 호출하여 업데이트를 다운로드 및 설치하는 방법을 보여 줍니다. 이 메서드를 사용하여 업데이트를 다운로드 및 설치하는 경우 OS에서 업데이트를 다운로드하기 전에 사용자의 권한을 요청하는 대화 상자가 표시됩니다.

> [!NOTE]
> 해당 메서드는 앱의 필수 및 [선택적 패키지](/windows/msix/package/optional-packages)를 지원합니다. 선택적 패키지는 DLC(다운로드 가능한 콘텐츠) 추가 기능을 위해 크기 제약 조건에 맞게 대용량 앱을 분할하거나, 핵심 앱과 별도로 추가 콘텐츠를 전달하는 데 유용합니다. 선택적 패키지(DLC 추가 기능 포함)를 사용하는 앱을 Microsoft Store에 제출할 수 있는 권한을 얻으려면 [Windows 개발자 지원](https://developer.microsoft.com/windows/support)을 참조하세요.

이 코드 예제에서는 다음을 가정합니다.

* 코드가 [Page](/uwp/api/windows.ui.xaml.controls.page) 컨텍스트에서 실행됩니다.
* **Page**에 다운로드 작업의 상태를 제공하는 ```downloadProgressBar```라는 이름의 [ProgressBar](/uwp/api/windows.ui.xaml.controls.progressbar)가 있습니다.
* 코드 파일에는 **Windows.Services.Store**, **Windows.Threading.Tasks** 및 **Windows.UI.Popups** 네임스페이스에 대한 **using** 문이 있습니다.
* 앱은 해당 앱을 실행한 사용자의 컨텍스트에서만 실행되는 단일 사용자 앱입니다. [다중 사용자 앱](../xbox-apps/multi-user-applications.md)의 경우 [GetDefault](/uwp/api/windows.services.store.storecontext.GetDefault) 메서드 대신 [GetForUser](/uwp/api/windows.services.store.storecontext.User) 메서드를 사용하여 **StoreContext** 개체를 가져옵니다.

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
> 사용할 수 있는 패키지 업데이트를 다운로드만 하고 설치하지 않으려면 [RequestDownloadStorePackageUpdatesAsync](/uwp/api/windows.services.store.storecontext.requestdownloadstorepackageupdatesasync) 메서드를 사용합니다.

### <a name="display-download-and-install-progress-info"></a>다운로드 및 설치 진행률 정보 표시

[RequestDownloadStorePackageUpdatesAsync](/uwp/api/windows.services.store.storecontext.requestdownloadstorepackageupdatesasync) 또는 [RequestDownloadAndInstallStorePackageUpdatesAsync](/uwp/api/windows.services.store.storecontext.requestdownloadandinstallstorepackageupdatesasync) 메서드를 호출할 때, 이 요청의 각 패키지에 대해 다운로드(또는 다운로드 및 설치) 프로세스의 각 단계에서 한 번씩 호출되는 [Progress](/uwp/api/windows.foundation.iasyncoperationwithprogress-2.progress) 처리기를 할당할 수 있습니다. 처리기는 진행률 알림을 발생시킨 업데이트 패키지 정보를 제공하는 [StorePackageUpdateStatus](/uwp/api/windows.services.store.storepackageupdatestatus) 개체를 받습니다. 이전 예제에서는 **StorePackageUpdateStatus** 개체의 **PackageDownloadProgress** 필드를 사용하여 다운로드 및 설치 프로세스의 진행률을 표시합니다.

패키지 다운로드와 설치를 하나의 작업으로 수행하기 위해 **RequestDownloadAndInstallStorePackageUpdatesAsync**를 호출할 경우, **PackageDownloadProgress** 필드는 패키지 다운로드 중에 0.0에서 0.8까지 증가하고, 설치 중에 0.8에서 1.0까지 증가합니다. 따라서 사용자 지정 진행 UI에 표시되는 백분율을 **PackageDownloadProgress** 필드 값에 직접 매핑할 경우, 패키지 다운로드가 완료되고 OS에서 설치 대화 상자가 표시될 때 80%가 UI에 표시됩니다. 패키지가 다운로드되어 설치할 준비가 되었을 때 사용자 지정 진행 UI에 100%를 표시하려면 **PackageDownloadProgress** 필드가 0.8에 도달했을 때 진행 UI에 100%를 할당하도록 코드를 수정해야 합니다.

## <a name="download-and-install-package-updates-silently"></a>자동으로 패키지 업데이트 다운로드 및 설치

Windows 10, 버전 1803부터 [TrySilentDownloadStorePackageUpdatesAsync](/uwp/api/windows.services.store.storecontext.trysilentdownloadstorepackageupdatesasync) 및 [TrySilentDownloadAndInstallStorePackageUpdatesAsync](/uwp/api/windows.services.store.storecontext.trysilentdownloadandinstallstorepackageupdatesasync) 메서드를 사용하여 사용자에게 알림 UI를 표시하지 않고 패키지 업데이트를 자동으로 다운로드 및 설치할 수 있습니다. 이 작업은 사용자가 Microsoft Store에서 **자동으로 앱 업데이트** 설정을 사용하도록 설정했으며 데이터 통신 연결 네트워크를 사용하고 있지 않은 경우에만 성공합니다. 메서드를 호출하기 전에 먼저 [CanSilentlyDownloadStorePackageUpdates](/uwp/api/windows.services.store.storecontext.cansilentlydownloadstorepackageupdates) 속성을 확인하여 해당 조건이 현재 충족되었는지 확인할 수 있습니다.

다음 코드 예제는 [GetAppAndOptionalStorePackageUpdatesAsync](/uwp/api/windows.services.store.storecontext.getappandoptionalstorepackageupdatesasync) 메서드를 사용하여 이용할 수 있는 모든 패키지 업데이트를 검색한 다음, [TrySilentDownloadStorePackageUpdatesAsync](/uwp/api/windows.services.store.storecontext.trysilentdownloadstorepackageupdatesasync) 및 [TrySilentDownloadAndInstallStorePackageUpdatesAsync](/uwp/api/windows.services.store.storecontext.trysilentdownloadandinstallstorepackageupdatesasync) 메서드를 호출하여 업데이트를 자동으로 다운로드 및 설치하는 방법을 보여 줍니다.

이 코드 예제에서는 다음을 가정합니다.
* 코드 파일에는 **Windows.Services.Store** 및 **System.Threading.Tasks** 네임스페이스에 대한 **using** 문이 있습니다.
* 앱은 해당 앱을 실행한 사용자의 컨텍스트에서만 실행되는 단일 사용자 앱입니다. [다중 사용자 앱](../xbox-apps/multi-user-applications.md)의 경우 [GetDefault](/uwp/api/windows.services.store.storecontext.GetDefault) 메서드 대신 [GetForUser](/uwp/api/windows.services.store.storecontext.User) 메서드를 사용하여 **StoreContext** 개체를 가져옵니다.

> [!NOTE]
> 이 예제 코드에서 호출된 **IsNowAGoodTimeToRestartApp**, **RetryDownloadAndInstallLater** 및 **RetryInstallLater** 메서드는 앱 디자인에 따라 필요한 경우 구현하려는 자리 표시자 메서드입니다.

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

파트너 센터에서 Windows 10 버전 1607 이상을 대상으로 하는 앱에 대한 패키지 제출을 만드는 경우, [해당 패키지를 필수로 표시](../publish/upload-app-packages.md#mandatory-update)하고 필수가 되는 날짜/시간을 지정할 수 있습니다. 이 속성이 설정되었으며 앱에서 패키지 업데이트를 사용할 수 있는 것을 확인한 경우, 앱은 업데이트 패키지가 필수인지 여부를 확인하고 업데이트가 설치될 때까지 동작을 변경합니다(예: 앱에서 기능을 사용하지 않도록 설정할 수 있음).

> [!NOTE]
> Microsoft는 패키지 업데이트의 필수 상태를 적용하지 않으며, 필수 앱 업데이트를 설치해야 함을 사용자에게 알리는 UI를 OS에서 제공하지 않습니다. 개발자가 필수 설정을 사용하여 자체 코드에서 필수 앱 업데이트를 적용합니다.  

패키지 제출을 필수로 표시하려면

1. [파트너 센터](https://partner.microsoft.com/dashboard)에 로그인한 다음, 앱 개요 페이지로 이동합니다.
2. 필수로 만들 패키지 업데이트가 포함된 제출의 이름을 클릭합니다.
3. 제출의 **패키지** 페이지로 이동합니다. 이 페이지 하단에서 **이 업데이트를 필수로 설정하세요.** 를 선택한 다음 패키지 업데이트가 필수가 되는 날짜와 시간을 선택합니다. 이 옵션은 제출의 모든 UWP 패키지에 적용됩니다.

자세한 내용은 [앱 패키지 업로드](../publish/upload-app-packages.md)를 참조하세요.

> [!NOTE]
> [패키지 플라이트](../publish/package-flights.md)를 만드는 경우, 플라이트 **패키지** 페이지에서 유사한 UI를 사용하여 패키지를 필수로 표시할 수 있습니다. 이 경우 필수 패키지 업데이트는 해당 플라이트 그룹에 속한 고객에게만 적용됩니다.

### <a name="code-example-for-mandatory-packages"></a>필수 패키지의 코드 예제

다음 코드 예제는 업데이트 패키지가 필수인지 여부를 확인하는 방법을 보여 줍니다. 일반적으로 필수 패키지 업데이트가 성공적으로 다운로드 또는 설치되지 않은 경우 사용자를 위해 앱 환경을 정상적으로 다운그레이드해야 합니다.

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

Windows 10, 버전 1803부터 [RequestUninstallStorePackageAsync](/uwp/api/windows.services.store.storecontext.requestuninstallstorepackageasync) 또는 [RequestUninstallStorePackageByStoreIdAsync](/uwp/api/windows.services.store.storecontext.requestuninstallstorepackagebystoreidasync) 메서드를 사용하여 현재 앱의 [선택적 패키지](/windows/msix/package/optional-packages)(DLC 패키지 포함)를 제거할 수 있습니다. 예를 들어 선택적 패키지를 통해 설치되는 콘텐츠를 포함하는 앱이 있는 경우, 사용자가 디스크 공간을 확보하기 위해 선택적 패키지를 제거할 수 있는 UI를 제공하는 것이 좋습니다.

다음 코드 예제는 [RequestUninstallStorePackageAsync](/uwp/api/windows.services.store.storecontext.requestuninstallstorepackageasync)를 호출하는 방법을 보여 줍니다. 이 예제에서는 다음을 가정합니다.
* 코드 파일에는 **Windows.Services.Store** 및 **System.Threading.Tasks** 네임스페이스에 대한 **using** 문이 있습니다.
* 앱은 해당 앱을 실행한 사용자의 컨텍스트에서만 실행되는 단일 사용자 앱입니다. [다중 사용자 앱](../xbox-apps/multi-user-applications.md)의 경우 [GetDefault](/uwp/api/windows.services.store.storecontext.GetDefault) 메서드 대신 [GetForUser](/uwp/api/windows.services.store.storecontext.User) 메서드를 사용하여 **StoreContext** 개체를 가져옵니다.

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

## <a name="get-download-queue-info"></a>다운로드 큐 정보 가져오기

Windows 10, 버전 1803부터 [GetAssociatedStoreQueueItemsAsync](/uwp/api/windows.services.store.storecontext.getassociatedstorequeueitemsasync) 및 [GetStoreQueueItemsAsync](/uwp/api/windows.services.store.storecontext.getstorequeueitemsasync) 메서드를 사용하여 Microsoft Store의 현재 다운로드 및 설치 큐에 있는 패키지 정보를 가져올 수 있습니다. 해당 메서드는 앱이나 게임이 다운로드 및 설치하는 데 몇 시간에서 며칠이 걸릴 수 있는 대용량 선택적 패키지(DLC 포함)를 지원하고, 다운로드 및 설치 프로세스가 완료되기 전에 고객이 앱이나 게임을 종료하는 경우를 정상적으로 처리하려는 경우에 유용합니다. 고객이 앱이나 게임을 다시 시작할 때 고객에게 각 패키지 상태를 표시할 수 있도록 코드에서 이 메서드를 사용하여 아직 다운로드 및 설치 큐에 있는 패키지의 상태 정보를 가져올 수 있습니다.

다음 코드 예제는 [GetAssociatedStoreQueueItemsAsync](/uwp/api/windows.services.store.storecontext.getassociatedstorequeueitemsasync)를 호출하여 현재 앱에 대해 진행 중인 패키지 업데이트 목록을 가져오고 각 패키지의 상태 정보를 검색하는 방법을 보여 줍니다. 이 예제에서는 다음을 가정합니다.
* 코드 파일에는 **Windows.Services.Store** 및 **System.Threading.Tasks** 네임스페이스에 대한 **using** 문이 있습니다.
* 앱은 해당 앱을 실행한 사용자의 컨텍스트에서만 실행되는 단일 사용자 앱입니다. [다중 사용자 앱](../xbox-apps/multi-user-applications.md)의 경우 [GetDefault](/uwp/api/windows.services.store.storecontext.GetDefault) 메서드 대신 [GetForUser](/uwp/api/windows.services.store.storecontext.User) 메서드를 사용하여 **StoreContext** 개체를 가져옵니다.

> [!NOTE]
> 이 예제 코드에서 호출된 **MarkUpdateInProgressInUI**, **RemoveItemFromUI**, **MarkInstallCompleteInUI**, **MarkInstallErrorInUI** 및 **MarkInstallPausedInUI** 메서드는 앱 디자인에 따라 필요한 경우 구현하려는 자리 표시자 메서드입니다.

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

* [선택형 패키지 및 관련 세트 제작](/windows/msix/package/optional-packages)