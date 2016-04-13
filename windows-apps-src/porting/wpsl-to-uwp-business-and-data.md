---
description: UI의 뒤에는 비즈니스 및 데이터 계층이 있습니다.
title: Windows Phone Silverlight 비즈니스 및 데이터 계층을 UWP로 포팅
ms.assetid: 27c66759-2b35-41f5-9f7a-ceb97f4a0e3f
---

#  Windows Phone Silverlight 비즈니스 및 데이터 계층을 UWP로 포팅

\[ Windows 10의 UWP 앱에 맞게 업데이트되었습니다. Windows 8.x 문서는 [보관](http://go.microsoft.com/fwlink/p/?linkid=619132)을 참조하세요. \]

이전 항목에서는 [I/O, 디바이스 및 앱 모델에 대한 포팅](wpsl-to-uwp-input-and-sensors.md)을 살펴보았습니다.

UI의 뒤에는 비즈니스 및 데이터 계층이 있습니다. 이러한 계층의 코드는 운영 체제 및 .NET Framework API를 호출합니다(예제: 후순위 처리, 위치, 카메라, 파일 시스템, 네트워크 및 기타 데이터 액세스). 대부분은 [UWP(유니버설 Windows 플랫폼) 앱](https://msdn.microsoft.com/library/windows/apps/br211369)에서 사용할 수 있으므로, 이 코드의 대부분을 변경하지 않고 포팅할 수 있습니다.

## 비동기 메서드

UWP(유니버설 Windows 플랫폼)는 실질적이고 일관되게 반응하는 앱을 빌드할 수 있도록 하는 것이 최우선 목표 중 하나입니다. 애니메이션은 항상 자연스럽고 터치 조작(예제: 회전, 살짝 밀기)은 지연 없이 즉각적으로 이루어져서 UI가 손가락에 접착되어 있다는 느낌이 들 정도입니다. 이렇게 하기 위해 50밀리초 이내에 완료를 보장할 수 없는 UWP API를 비동기화하고 이름에 **Async** 접미사를 붙였습니다. UI 스레드는 **Async** 메서드를 호출하는 즉시 반환되고 작업은 다른 스레드에서 수행됩니다. **Async** 메서드를 사용하면 C# **await** 연산자, JavaScript promise 개체 및 C++ 연속을 구문적으로 매우 쉽게 사용할 수 있습니다. 자세한 내용은 [비동기 프로그래밍](https://msdn.microsoft.com/library/windows/apps/mt187335)을 참조하세요.

## 후순위 처리

Windows Phone Silverlight 앱은 포그라운드에 있지 않은 상태에서 관리되는 **ScheduledTaskAgent** 개체를 사용하여 작업을 수행할 수 있습니다. UWP 앱은 [**BackgroundTaskBuilder**](https://msdn.microsoft.com/library/windows/apps/br224768) 클래스를 사용하여 비슷한 방식으로 백그라운드 작업을 만들고 등록합니다. 백그라운드 작업의 작동을 구현하는 클래스를 정의합니다. 시스템에서는 클래스의 [**Run**](https://msdn.microsoft.com/library/windows/apps/br224811) 메서드를 호출하여 작업을 실행함으로써 백그라운드 작업을 주기적으로 실행합니다. UWP 앱에서는 앱 패키지 매니페스트에서 **백그라운드 작업** 선언을 설정합니다. 자세한 내용은 [백그라운드 작업을 사용하여 앱 지원](https://msdn.microsoft.com/library/windows/apps/mt299103)을 참조하세요.

백그라운드에서 대용량 데이터 파일을 전송하기 위해 Windows Phone Silverlight 앱은 **BackgroundTransferService** 클래스를 사용합니다. UWP 앱은 [**Windows.Networking.BackgroundTransfer**](https://msdn.microsoft.com/library/windows/apps/br207242) 네임스페이스의 API를 사용하여 이 작업을 수행합니다. 이러한 기능은 비슷한 패턴으로 전송을 시작하지만 새로운 API에서는 기능과 성능이 개선되었습니다. 자세한 내용은 [백그라운드에서 데이터 전송](https://msdn.microsoft.com/library/windows/apps/xaml/hh452975)을 참조하세요.

Windows Phone Silverlight 앱은 앱이 포그라운드에 없을 때 **Microsoft.Phone.BackgroundAudio** 네임스페이스의 관리 클래스를 사용하여 오디오를 재생합니다. UWP는 Windows Phone 스토어 앱 모델을 사용합니다. 자세한 내용은 [백그라운드 오디오](https://msdn.microsoft.com/library/windows/apps/mt282140) 및 [백그라운드 오디오](http://go.microsoft.com/fwlink/p/?linkid=619997) 샘플을 참조하세요.

## 클라우드 서비스, 네트워킹 및 데이터베이스

Azure를 사용하여 클라우드에서 데이터 및 앱 서비스를 호스팅할 수 있습니다. [모바일 서비스 시작](http://go.microsoft.com/fwlink/p/?LinkID=403138)을 참조하세요. 온라인과 오프라인 데이터가 모두 필요한 솔루션에 대해서는 [모바일 서비스에서 오프라인 데이터 동기화 사용](http://azure.microsoft.com/documentation/articles/mobile-services-windows-store-dotnet-get-started-offline-data/)을 참조하세요.

UWP에서는 **System.Net.HttpWebRequest** 클래스가 부분적으로 지원되지만 **System.Net.WebClient** 클래스는 지원되지 않습니다. 권장되는 미래 지향적인 대안은 [**Windows.Web.Http.HttpClient**](https://msdn.microsoft.com/library/windows/apps/dn298639) 클래스 (또는 .NET을 지원하는 다른 플랫폼으로 이식할 수 있는 코드가 필요한 경우 [System.Net.Http.HttpClient](https://msdn.microsoft.com/library/system.net.http.httpclient(v=vs.118).aspx))입니다. 이러한 API는 [System.Net.Http.HttpRequestMessage](https://msdn.microsoft.com/library/system.net.http.httprequestmessage.aspx)를 사용하여 HTTP 요청을 나타냅니다.

UWP 앱은 현재 LOB(기간 업무) 시나리오와 같이 데이터 사용이 많은 시나리오를 기본적으로 지원하지 않습니다. 그러나 로컬 트랜잭션 데이터베이스 서비스에 대해 SQLite를 사용할 수 있습니다. 자세한 내용은 [SQLite](https://visualstudiogallery.msdn.microsoft.com/4913e7d5-96c9-4dde-a1a1-69820d615936)를 참조하세요.

Windows 런타임 형식에 상대 URI가 아니라 절대 URI를 전달합니다. [Windows 런타임에 URI 전달](https://msdn.microsoft.com/library/hh763341.aspx)을 참조하세요.

## 시작 관리자 및 선택자

Windows Phone Silverlight 앱에서는 **Microsoft.Phone.Tasks** 네임스페이스에 있는 시작 관리자와 선택자를 통해 운영 체제를 조작하여 메일 작성, 사진 선택, 다른 앱과 특정 형식의 데이터 공유 등과 같은 일반 작업을 수행할 수 있습니다. [Windows Phone Silverlight를 Windows 10 네임스페이스 및 클래스에 매핑](wpsl-to-uwp-namespace-and-class-mappings.md) 항목에서 **Microsoft.Phone.Tasks**를 검색하여 해당 UWP 형식을 찾습니다. 여기에는 시작 관리자와 선택기라는 비슷한 메커니즘부터 앱 간 데이터 공유에 대한 계약 구현까지 포함됩니다.

Windows Phone Silverlight 앱은 유휴 상태로 전환되거나 삭제 표시될 수 있습니다(예제: 사진 선택자 작업을 사용할 때). UWP 앱은 [**FileOpenPicker**](https://msdn.microsoft.com/library/windows/apps/br207847) 클래스를 사용하는 동안 활성 및 실행 상태를 유지합니다.

## 수익 창출(평가 모드 및 앱에서 바로 구매)

Windows Phone Silverlight 앱에서는 대부분의 체험 모드 및 앱에서 바로 구매 기능에 대해 UWP [**CurrentApp**](https://msdn.microsoft.com/library/windows/apps/hh779765) 클래스를 사용할 수 있으므로 코드를 포팅할 필요가 없습니다. 하지만 Windows Phone Silverlight 앱에서는 구매용 앱을 제공하기 위해 **MarketplaceDetailTask.Show**를 호출합니다.

```csharp
    private void Buy()
    {
        MarketplaceDetailTask marketplaceDetailTask = new MarketplaceDetailTask();
        marketplaceDetailTask.ContentType = MarketplaceContentType.Applications;
        marketplaceDetailTask.Show();
    }
```

다음과 같이 해당 코드를 포팅하여 UWP [**RequestAppPurchaseAsync**](https://msdn.microsoft.com/library/windows/apps/hh967813) 메서드를 호출합니다.

```csharp
    private async void Buy()
    {
        await Windows.ApplicationModel.Store.CurrentApp.RequestAppPurchaseAsync(false);
    }
```

테스트를 위해 앱 구매 및 앱에서 바로 구매 기능을 시뮬레이션하는 코드가 있는 경우 해당 코드를 포팅하여 [**CurrentAppSimulator**](https://msdn.microsoft.com/library/windows/apps/hh779766) 클래스를 대신 사용할 수 있습니다.

## 타일 또는 알림 업데이트 알림

알림은 Windows Phone Silverlight 앱을 위한 푸시 알림 모델의 확장입니다. WNS(Windows 푸시 알림 서비스)에서 알림을 수신할 때 타일 업데이트 또는 알림을 통해 정보를 UI에 표시할 수 있습니다. 알림 기능의 UI 측면을 포팅하려면 [타일 및 알림](w8x-to-uwp-porting-xaml-and-ui.md#tiles-and-toasts)을 참조하세요.

UWP 앱에서 알림을 사용하는 방법에 대한 자세한 내용은 [알림 메시지 보내기](https://msdn.microsoft.com/library/windows/apps/xaml/hh868266)를 참조하세요.

C++, C# 또는 Visual Basic으로 작성한 Windows 런타임 앱에서 타일, 알림, 배지 및 배너를 사용하는 방법에 대한 정보와 자습서는 [타일, 배지 및 알림 메시지 작업](https://msdn.microsoft.com/library/windows/apps/xaml/hh868259)을 참조하세요.

## 저장소(파일 액세스)

앱 설정을 격리된 저장소에 키-값 쌍으로 저장하는 Windows Phone Silverlight 코드는 쉽게 포팅됩니다. 다음은 이전 및 이후 예제입니다. Windows Phone Silverlight 버전:

```csharp
    var propertySet = IsolatedStorageSettings.ApplicationSettings;
    const string key = "favoriteAuthor";
    propertySet[key] = "Charles Dickens";
    propertySet.Save();
    string myFavoriteAuthor = propertySet.Contains(key) ? (string)propertySet[key] : "<none>";
```

UWP 해당 버전:

```csharp
    var propertySet = Windows.Storage.ApplicationData.Current.LocalSettings.Values;
    const string key = "favoriteAuthor";
    propertySet[key] = "Charles Dickens";
    string myFavoriteAuthor = propertySet.ContainsKey(key) ? (string)propertySet[key] : "<none>";
```

**Windows.Storage** 네임스페이스의 하위 집합을 사용할 수 있지만 많은 Windows Phone Silverlight 앱에서는 이 네임스페이스가 더 이상 지원되지 않으므로 **IsolatedStorageFile** 클래스를 통해 파일 I/O를 수행합니다. 다음은 **IsolatedStorageFile**을 사용할 경우의 파일 읽기 및 쓰기에 대한 이전 및 이후 예제입니다. Windows Phone Silverlight 버전:

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

UWP를 사용하는 동일한 기능:

```csharp
    const string filename = "FavoriteAuthor.txt";
    var store = Windows.Storage.ApplicationData.Current.LocalFolder;
    Windows.Storage.StorageFile file = await store.CreateFileAsync(filename, Windows.Storage.CreationCollisionOption.ReplaceExisting);
    await Windows.Storage.FileIO.WriteTextAsync(file, "Charles Dickens");
    file = await store.GetFileAsync(filename);
    string myFavoriteAuthor = await Windows.Storage.FileIO.ReadTextAsync(file);
```

Windows Phone Silverlight 앱은 선택적 SD 카드에 대한 읽기 전용 권한이 있습니다. UWP 앱은 SD 카드에 대해 읽기/쓰기 권한이 있습니다. 자세한 내용은 [SD 카드에 액세스](https://msdn.microsoft.com/library/windows/apps/mt188699)를 참조하세요.

UWP 앱에서 사진, 음악 및 동영상 파일에 액세스하는 방법에 대한 자세한 내용은 [음악, 사진 및 비디오 라이브러리의 파일 및 폴더](https://msdn.microsoft.com/library/windows/apps/mt188703)를 참조하세요.

자세한 내용은 [파일, 폴더 및 라이브러리](https://msdn.microsoft.com/library/windows/apps/mt185399)를 참조하세요.

다음 항목은 [폼 팩터 및 UX에 대한 포팅](wpsl-to-uwp-form-factors-and-ux.md)입니다.

## 관련 항목

* [네임스페이스 및 클래스 매핑](wpsl-to-uwp-namespace-and-class-mappings.md)
 



<!--HONumber=Mar16_HO1-->


