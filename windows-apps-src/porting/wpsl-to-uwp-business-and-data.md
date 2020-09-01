---
title: WPSL 비즈니스 및 데이터 계층을 UWP로 포팅
description: Windows Phone Silverlight 앱에서 UWP (유니버설 Windows 플랫폼)로 비즈니스 및 데이터 계층을 이식 하는 방법에 대해 알아봅니다.
ms.assetid: 27c66759-2b35-41f5-9f7a-ceb97f4a0e3f
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 833dabc0adf759869361dfba9560062acc77e35d
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89171157"
---
# <a name="porting-windowsphone-silverlight-business-and-data-layers-to-uwp"></a>Windows Phone Silverlight 비즈니스 및 데이터 계층을 UWP로 포팅

이전 항목은 [i/o, 장치 및 앱 모델에 대해 포팅](wpsl-to-uwp-input-and-sensors.md)되었습니다.

UI 뒤에는 비즈니스 및 데이터 계층이 있습니다. 이러한 계층의 코드는 운영 체제 및 .NET Framework Api (예: 백그라운드 처리, 위치, 카메라, 파일 시스템, 네트워크 및 기타 데이터 액세스)를 호출 합니다. 이러한 대부분은 [UWP (유니버설 Windows 플랫폼) 앱에서 사용할 수](/previous-versions/windows/br211369(v=win.10))있으므로이 코드를 변경 하지 않고 이식할 수 있는 것으로 예측할 수 있습니다.

## <a name="asynchronous-methods"></a>비동기 메서드

UWP (유니버설 Windows 플랫폼)의 우선 순위 중 하나는 실제로 지속적이 고 응답성이 뛰어난 앱을 빌드할 수 있게 하는 것입니다. 애니메이션은 항상 부드러운 곡선 이며 패닝 및 살짝 밀기와 같은 터치 상호 작용은 즉각적 이며 지연 시간이 없으며 UI가 손가락에 붙어 있는 것 처럼 보입니다. 이를 위해 50ms 이내에 완료를 보장할 수 없는 UWP API가 비동기적으로 수행 되었으며 이름에 **Async**가 붙습니다. UI 스레드는 **비동기** 메서드를 호출 하 여 즉시 반환 하 고 다른 스레드에서 작업을 수행 합니다. **비동기** 메서드를 사용 하는 것은 c # **Wait** 연산자, JavaScript 약속 개체 및 c + + 연속을 사용 하 여 매우 쉽게 구문이 가능 합니다. 자세한 내용은 [비동기 프로그래밍](../threading-async/asynchronous-programming-universal-windows-platform-apps.md)을 참조 하세요.

## <a name="background-processing"></a>백그라운드 처리

Windows Phone Silverlight 앱은 관리 되는 **ScheduledTaskAgent** 개체를 사용 하 여 응용 프로그램이 전경에 있지 않은 상태에서 작업을 수행할 수 있습니다. UWP 앱은 [**BackgroundTaskBuilder**](/uwp/api/Windows.ApplicationModel.Background.BackgroundTaskBuilder) 클래스를 사용 하 여 비슷한 방식으로 백그라운드 작업을 만들고 등록 합니다. 백그라운드 작업의 작업을 구현 하는 클래스를 정의 합니다. 시스템은 주기적으로 백그라운드 작업을 실행 하 고 클래스의 [**Run**](/uwp/api/windows.applicationmodel.background.ibackgroundtask.run) 메서드를 호출 하 여 작업을 실행 합니다. UWP 앱에서는 앱 패키지 매니페스트에 **백그라운드 작업** 선언을 설정 해야 합니다. 자세한 내용은 [백그라운드 작업을 사용 하 여 앱 지원](../launch-resume/support-your-app-with-background-tasks.md)을 참조 하세요.

백그라운드에서 많은 데이터 파일을 전송 하기 위해 Windows Phone Silverlight 앱은 **BackgroundTransferService** 클래스를 사용 합니다. UWP 앱은 [**windows.networking.backgroundtransfer**](/uwp/api/Windows.Networking.BackgroundTransfer) 네임 스페이스에서 api를 사용 하 여이 작업을 수행 합니다. 이 기능은 비슷한 패턴을 사용 하 여 전송을 시작 하지만 새 API는 향상 된 기능 및 성능을 제공 합니다. 자세한 내용은 [백그라운드에서 데이터 전송](/previous-versions/windows/apps/hh452975(v=win.10))을 참조 하세요.

Windows Phone Silverlight 앱은 **BackgroundAudio** 네임 스페이스의 관리 되는 클래스를 사용 하 여 앱이 전경에 있지 않은 상태에서 오디오를 재생 합니다. UWP는 Windows Phone 스토어 앱 모델을 사용 합니다. [배경 오디오](../audio-video-camera/background-audio.md) 및 [배경 오디오](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/BackgroundAudio) 샘플을 참조 하세요.

## <a name="cloud-services-networking-and-databases"></a>클라우드 서비스, 네트워킹 및 데이터베이스

Azure를 사용 하 여 클라우드에서 데이터 및 앱 서비스를 호스트할 수 있습니다. [Mobile Services 시작을](/azure/)참조 하세요. 온라인 및 오프 라인 데이터를 모두 필요로 하는 솔루션의 경우: [Mobile Services에서 오프 라인 데이터 동기화 사용](/azure/)을 참조 하세요.

UWP에는 **시스템 .Net HttpWebRequest** 클래스에 대 한 부분 지원이 있지만 **시스템 .net. WebClient** 클래스는 지원 되지 않습니다. 앞으로의 권장 대안은 .NET을 지 원하는 다른 플랫폼으로 코드를 이식 해야 하는 경우에는 client.msi 클라이언트 클래스 (또는 [시스템 .Net http](/previous-versions/visualstudio/hh193681(v=vs.118)) . [**w**](/uwp/api/Windows.Web.Http.HttpClient) i n 클라이언트)입니다. 이러한 API는 [System.Net.Http.HttpRequestMessage](/previous-versions/visualstudio/hh159020(v=vs.118))를 사용하여 HTTP 요청을 나타냅니다.

UWP 앱은 현재 LOB (기간 업무) 시나리오와 같은 데이터 집약적 시나리오에 대 한 기본 제공 지원을 포함 하지 않습니다. 그러나 로컬 트랜잭션 데이터베이스 서비스에 대해 SQLite를 사용할 수 있습니다. 자세한 내용은 [SQLite](https://marketplace.visualstudio.com/items?itemName=SQLiteDevelopmentTeam.SQLiteforUniversalWindowsPlatform)를 참조 하십시오.

상대 Uri가 아닌 절대 Uri를 Windows 런타임 형식으로 전달 합니다. [Windows 런타임에 URI 전달을](/dotnet/standard/cross-platform/passing-a-uri-to-the-windows-runtime)참조 하세요.

## <a name="launchers-and-choosers"></a>시작 관리자 및 선택자

Choosers 네임 스페이스에 있는 시작 관리자와를 사용 하 Windows Phone는 경우 Silverlight 앱은 운영 체제와 상호 작용 하 여 전자 메일 작성, 사진 선택, 특정 종류의 데이터를 다른 앱과 공유 하는 등의 일반적인 작업을 수행할 **수 있습니다.** [Silverlight에서 Windows 10 네임 스페이스 및 클래스 매핑 Windows Phone](wpsl-to-uwp-namespace-and-class-mappings.md) 항목의 **Microsoft Phone 작업** 을 검색 하 여 동일한 UWP 형식을 찾습니다. 이러한 범위는 응용 프로그램 간에 데이터를 공유 하기 위한 계약을 구현 하기 위해 시작 관리자 및 선택기 라는 유사한 메커니즘과 동일 합니다.

Windows Phone Silverlight 앱을 사용 하는 경우, 예를 들어 사진 선택 작업을 사용 하는 경우에도 유휴 상태 또는 삭제 표시로 전환할 수 있습니다. UWP 앱은 [**Fileopenpicker**](/uwp/api/Windows.Storage.Pickers.FileOpenPicker) 클래스를 사용 하는 동안 활성 상태로 유지 되 고 실행 됩니다.

## <a name="monetization-trial-mode-and-in-app-purchases"></a>수익 화 (평가판 모드 및 앱 내 구매)

Windows Phone Silverlight 앱은 대부분의 평가판 모드 및 앱 내 구매 기능에 UWP [**currentapp**](/uwp/api/Windows.ApplicationModel.Store.CurrentApp) 클래스를 사용할 수 있으므로 코드를 이식할 필요가 없습니다. 그러나 Windows Phone Silverlight 앱은 **MarketplaceDetailTask** 를 호출 하 여 구매할 앱을 제공 합니다.

```csharp
    private void Buy()
    {
        MarketplaceDetailTask marketplaceDetailTask = new MarketplaceDetailTask();
        marketplaceDetailTask.ContentType = MarketplaceContentType.Applications;
        marketplaceDetailTask.Show();
    }
```

UWP [**RequestAppPurchaseAsync**](/uwp/api/windows.applicationmodel.store.currentapp.requestapppurchaseasync) 메서드를 호출 하는 코드를 나타내는 포트:

```csharp
    private async void Buy()
    {
        await Windows.ApplicationModel.Store.CurrentApp.RequestAppPurchaseAsync(false);
    }
```

테스트 목적으로 앱 구매 및 앱 내 구매 기능을 시뮬레이트하는 코드를 사용 하는 경우 대신 [**Currentappsimulator**](/uwp/api/Windows.ApplicationModel.Store.CurrentAppSimulator) 클래스를 사용 하도록 포트를 설정할 수 있습니다.

## <a name="notifications-for-tile-or-toast-updates"></a>타일 또는 알림 업데이트에 대 한 알림

알림은 Windows Phone Silverlight 앱에 대 한 푸시 알림 모델의 확장입니다. WNS (Windows 푸시 알림 서비스)에서 알림을 받으면 타일 업데이트 또는 알림 메시지를 사용 하 여 UI에 정보를 표시할 수 있습니다. 알림 기능의 UI 쪽 포팅은 [타일 및 알림을](w8x-to-uwp-porting-xaml-and-ui.md)을 참조 하세요.

UWP 앱에서 알림을 사용 하는 방법에 대 한 자세한 내용은 알림 메시지 [보내기](/previous-versions/windows/apps/hh868266(v=win.10))를 참조 하세요.

C + +, c # 또는 Visual Basic를 사용 하는 Windows 런타임 앱에서 타일, 알림을, 배지, 배너 및 알림을 사용 하는 방법에 대 한 정보 및 자습서는 [타일, 배지 및 알림 메시지 작업](/previous-versions/windows/apps/hh868259(v=win.10))을 참조 하세요.

## <a name="storage-file-access"></a>저장소 (파일 액세스)

앱 설정을 격리 된 저장소의 키-값 쌍으로 저장 하는 Silverlight 코드를 쉽게 이식할 수 Windows Phone. 다음은 이전 및 이후 예제 이며 먼저 Windows Phone Silverlight 버전입니다.

```csharp
    var propertySet = IsolatedStorageSettings.ApplicationSettings;
    const string key = "favoriteAuthor";
    propertySet[key] = "Charles Dickens";
    propertySet.Save();
    string myFavoriteAuthor = propertySet.Contains(key) ? (string)propertySet[key] : "<none>";
```

및 UWP와 동일 합니다.

```csharp
    var propertySet = Windows.Storage.ApplicationData.Current.LocalSettings.Values;
    const string key = "favoriteAuthor";
    propertySet[key] = "Charles Dickens";
    string myFavoriteAuthor = propertySet.ContainsKey(key) ? (string)propertySet[key] : "<none>";
```

System.io.isolatedstorage.isolatedstoragefile> **네임 스페이스의 하위** 집합을 사용할 수 있지만 많은 Windows Phone Silverlight 앱은 더 이상 지원 되기 때문에 **IsolatedStorageFile** 클래스를 사용 하 여 파일 i/o를 수행 합니다. **System.io.isolatedstorage.isolatedstoragefile>** 를 사용 하 고 있다고 가정 하 고, 다음은 파일 쓰기 및 읽기의 이전 및 이후 예제 이며 먼저 Windows Phone Silverlight 버전입니다.

```csharp
    const string filename = "FavoriteAuthor.txt";
    using (var store = IsolatedStorageFile.GetUserStoreForApplication())
    {
        using (var streamWriter = new StreamWriter(store.CreateFile(filename)))
        {
            streamWriter.Write("Charles Dickens");
        }
        using (var StreamReader = new StreamReader(store.OpenFile(filename, FileMode.Open, FileAccess.Read)))
        {
            string myFavoriteAuthor = StreamReader.ReadToEnd();
        }
    }
```

UWP를 사용 하 여 동일한 기능을 사용 합니다.

```csharp
    const string filename = "FavoriteAuthor.txt";
    var store = Windows.Storage.ApplicationData.Current.LocalFolder;
    Windows.Storage.StorageFile file = await store.CreateFileAsync(filename, Windows.Storage.CreationCollisionOption.ReplaceExisting);
    await Windows.Storage.FileIO.WriteTextAsync(file, "Charles Dickens");
    file = await store.GetFileAsync(filename);
    string myFavoriteAuthor = await Windows.Storage.FileIO.ReadTextAsync(file);
```

Windows Phone Silverlight 앱에는 선택적 SD 카드에 대 한 읽기 전용 액세스 권한이 있습니다. UWP 앱에는 SD 카드에 대 한 읽기/쓰기 권한이 있습니다. 자세한 내용은 [SD 카드 액세스](../files/access-the-sd-card.md)를 참조 하세요.

UWP 앱에서 사진, 음악 및 비디오 파일에 액세스 하는 방법에 대 한 정보는 [음악, 사진 및 비디오 라이브러리의 파일 및 폴더](../files/quickstart-managing-folders-in-the-music-pictures-and-videos-libraries.md)를 참조 하세요.

자세한 내용은 [파일, 폴더 및 라이브러리](../files/index.md)를 참조 하십시오.

다음 항목은 [폼 팩터 및 UX를 위한 포팅](wpsl-to-uwp-form-factors-and-ux.md)입니다.

## <a name="related-topics"></a>관련 항목

* [네임스페이스 및 클래스 매핑](wpsl-to-uwp-namespace-and-class-mappings.md)
 