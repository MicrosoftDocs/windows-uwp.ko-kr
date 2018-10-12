---
author: PatrickFarley
title: 원격 장치 검색
description: 프로젝트 로마를 사용하여 앱에서 원격 장치를 검색하는 방법을 알아봅니다.
ms.assetid: 5b4231c0-5060-49e2-a577-b747e20cf633
ms.author: pafarley
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp, 연결 된 장치, 원격 시스템, "로마" 프로젝트 "로마"
ms.localizationpriority: medium
ms.openlocfilehash: 02d04074ece0033da8c3454a95bc35af201903f3
ms.sourcegitcommit: d10fb9eb5f75f2d10e1c543a177402b50fe4019e
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/12/2018
ms.locfileid: "4565945"
---
# <a name="discover-remote-devices"></a>원격 장치 검색
앱에서 무선 네트워크, Bluetooth 및 클라우드 연결을 사용하여 검색 장치와 동일한 Microsoft 계정으로 로그온된 Windows 장치를 검색할 수 있습니다. 특별한 소프트웨어가 설치되어 있지 않아도 원격 장치를 검색할 수 있습니다.

> [!NOTE]
> 이 가이드에서는 [원격 앱 실행](launch-a-remote-app.md)의 단계에 따라 원격 시스템 기능에 대한 액세스 권한이 이미 부여되었다고 가정합니다.

## <a name="filter-the-set-of-discoverable-devices"></a>검색할 수 있는 장치 집합 필터링
필터와 함께 [**RemoteSystemWatcher**](https://msdn.microsoft.com/library/windows/apps/Windows.System.RemoteSystems.RemoteSystemWatcher)를 사용하여 검색할 수 있는 장치 집합 범위를 좁힐 수 있습니다. 필터는 검색 유형(근접 네트워크, 로컬 네트워크 및 클라우드 연결), 장치 유형(데스크톱, 휴대폰, Xbox, 허브 및 Holographic), 가용성 상태(원격 시스템 기능을 사용할 장치의 가용성 상태)를 감지할 수 있습니다.

필터 개체가 해당 생성자에 매개 변수로 전달되기 때문에 **RemoteSystemWatcher** 개체가 초기화되기 전이나 초기화되는 동안 필터 개체를 생성해야 합니다. 다음 코드에서는 사용 가능한 각 유형의 필터를 만들고 목록에 추가합니다.

> [!NOTE]
> 이 예의 코드를 사용하려면 파일에 `using Windows.System.RemoteSystems` 문이 있어야 합니다.

[!code-cs[Main](./code/DiscoverDevices/MainPage.xaml.cs#SnippetMakeFilterList)]

> [!NOTE]
> "근접" 필터 값이 물리적 근접도를 보증하지는 않습니다. 신뢰할 수 있는 물리적 접근성이 필요한 시나리오의 경우 필터에 [**RemoteSystemDiscoveryType.SpatiallyProximal**](https://docs.microsoft.com/uwp/api/windows.system.remotesystems.remotesystemdiscoverytype) 값을 사용합니다. 현재 이 필터는 Bluetooth로 검색되는 장치만 허용합니다. 물리적 근접성을 보증하는 새로운 검색 메커니즘과 프로토콜이 지원됨에 따라 여기에도 이들이 포함됩니다.  
[**RemoteSystem**](https://msdn.microsoft.com/library/windows/apps/Windows.System.RemoteSystems.RemoteSystem) 클래스에는 검색된 장치가 물리적 근접성 내에 실제로 있는지를 나타내는 속성인 [**RemoteSystem.IsAvailableBySpatialProximity**](https://docs.microsoft.com/uwp/api/Windows.System.RemoteSystems.RemoteSystem.IsAvailableByProximity)도 있습니다.

> [!NOTE]
> (검색 유형 필터 선택에 의해 결정된)로컬 네트워크를 통해 장치를 검색하려는 경우, 네트워크는 "비공개" 또는 "도메인" 프로필을 사용해야 합니다. 디바이스는 "공용" 네트워크를 통해 다른 디바이스를 검색하지 않습니다.

[**IRemoteSystemFilter**](https://msdn.microsoft.com/library/windows/apps/Windows.System.RemoteSystems.IRemoteSystemFilter) 개체 목록이 생성되면 **RemoteSystemWatcher**의 생성자에 전달할 수 있습니다.

[!code-cs[Main](./code/DiscoverDevices/MainPage.xaml.cs#SnippetCreateWatcher)]

이 감시자의 [**Start**](https://msdn.microsoft.com/library/windows/apps/Windows.System.RemoteSystems.RemoteSystemWatcher.Start) 메서드는 호출 시 다음 조건을 모두 충족하는 디바이스가 검색된 경우에만 [**RemoteSystemAdded**](https://msdn.microsoft.com/library/windows/apps/Windows.System.RemoteSystems.RemoteSystemWatcher.RemoteSystemAdded) 이벤트를 발생합니다.
* 근접 연결을 통해 검색할 수 있는 경우
* 데스크톱 또는 휴대폰인 경우
* 사용 가능한 것으로 분류된 경우

여기서 이벤트를 처리하고, [**RemoteSystem**](https://msdn.microsoft.com/library/windows/apps/Windows.System.RemoteSystems.RemoteSystem) 개체를 검색하고, 원격 디바이스에 연결하는 절차는 [원격 앱 실행](launch-a-remote-app.md)과 동일합니다. 즉, **RemoteSystem** 개체가 각 **RemoteSystemAdded** 이벤트와 함께 전달되는 [**RemoteSystemAddedEventArgs**](https://msdn.microsoft.com/library/windows/apps/Windows.System.RemoteSystems.RemoteSystemAddedEventArgs) 개체의 속성으로 저장됩니다.

## <a name="discover-devices-by-address-input"></a>주소를 입력하여 장치 검색
일부 디바이스는 사용자와 연결되어 있지 않거나 검색을 통해 찾을 수 없지만 검색 앱에서 직접 주소를 사용할 경우 연결할 수 있습니다. [**HostName**](https://msdn.microsoft.com/library/windows/apps/windows.networking.hostname.aspx) 클래스는 원격 디바이스의 주소를 나타내는 데 사용됩니다. IP 주소의 형태로 저장되는 경우가 많지만 다른 여러 형식도 허용됩니다(자세한 내용은 [**HostName 생성자**](https://msdn.microsoft.com/library/windows/apps/br207118.aspx) 참조).

유효한 **HostName** 개체를 제공하면 **RemoteSystem** 개체가 검색됩니다. 주소 데이터가 잘못된 경우 `null` 개체 참조가 반환됩니다.

[!code-cs[Main](./code/DiscoverDevices/MainPage.xaml.cs#SnippetFindByHostName)]

## <a name="querying-a-capability-on-a-remote-system"></a>원격 시스템에서 접근 권한 값 쿼리

검색 필터링과 별개이지만 장치 접근 권한 값 쿼리는 검색 프로세스의 중요한 부분입니다. [**RemoteSystem.GetCapabilitySupportedAsync**](https://docs.microsoft.com/uwp/api/windows.system.remotesystems.remotesystem.GetCapabilitySupportedAsync) 메서드를 사용하여 검색된 원격 시스템에서 원격 세션 연결이나 공간 엔터티(홀로그램) 공유 등의 특정 기능을 지원하는지 쿼리할 수 있습니다. 쿼리 가능한 기능 목록은 [**KnownRemoteSystemCapabilities**](https://docs.microsoft.com/uwp/api/windows.system.remotesystems.knownremotesystemcapabilities) 클래스를 참조하세요.

```csharp
// Check to see if the given remote system can accept LaunchUri requests
bool isRemoteSystemLaunchUriCapable = remoteSystem.GetCapabilitySupportedAsync(KnownRemoteSystemCapabilities.LaunchUri);
```

## <a name="cross-user-discovery"></a>사용자 간 검색

개발자는 동일한 사용자에게 등록된 장치뿐만 아니라 클라이언트 장치에 근접한 _모든_ 장치의 검색을 지정할 수 있습니다. 이는 특별 **IRemoteSystemFilter**인 [**RemoteSystemAuthorizationKindFilter**](https://docs.microsoft.com/uwp/api/windows.system.remotesystems.remotesystemauthorizationkindfilter)를 통해 구현됩니다. 다른 필터 유형과 같이 구현됩니다.

```csharp
// Construct a user type filter that includes anonymous devices
RemoteSystemAuthorizationKindFilter authorizationKindFilter = new RemoteSystemAuthorizationKindFilter(RemoteSystemAuthorizationKind.Anonymous);
// then add this filter to the RemoteSystemWatcher
```

* [**RemoteSystemAuthorizationKind**](https://docs.microsoft.com/uwp/api/windows.system.remotesystems.remotesystemauthorizationkind) 값으로 **Anonymous**을 사용하면 신뢰할 수 없는 사용자의 장치를 포함한 모든 근접 장치를 검색할 수 있습니다.
* 값으로 **SameUser**를 사용하면 검색이 클라이언트 장치와 동일한 사용자에게 등록된 장치로만 제한됩니다. 이것이 기본 동작입니다.

### <a name="checking-the-cross-user-sharing-settings"></a>사용자 간 공유 설정 확인

검색 앱에 지정되는 위의 필터 외에도 다른 사용자로 로그인한 장치의 공유 환경을 허용하도록 클라이언트 장치 자체를 구성해야 합니다. 이는 **RemoteSystem** 클래스의 정적 메서드로 쿼리할 수 있는 시스템 설정입니다.

```csharp
if (!RemoteSystem.IsAuthorizationKindEnabled(RemoteSystemAuthorizationKind.Anonymous)) {
    // The system is not authorized to connect to cross-user devices. 
    // Inform the user that they can discover more devices if they
    // update the setting to "Anonymous".
}
```

이 설정을 변경하려면 사용자가 **설정** 앱을 열어야 합니다. **시스템** > **공유 환경** > **장치 간 공유** 메뉴에 사용자가 시스템에서 공유할 수 있는 장치를 지정하는 드롭다운 상자가 있습니다.

![공유 환경 설정 페이지](images/shared-experiences-settings.png)

## <a name="related-topics"></a>관련 항목
* [연결된 앱 및 장치(프로젝트 로마)](connected-apps-and-devices.md)
* [원격 앱 실행](launch-a-remote-app.md)
* [원격 시스템 API 참조](https://msdn.microsoft.com/library/windows/apps/Windows.System.RemoteSystems)
* [원격 시스템 샘플](https://github.com/Microsoft/Windows-universal-samples/tree/dev/Samples/RemoteSystems)
