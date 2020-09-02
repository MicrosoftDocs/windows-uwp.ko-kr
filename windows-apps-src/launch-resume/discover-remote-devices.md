---
title: 원격 디바이스 검색
description: Project 로마를 사용 하 여 앱에서 원격 장치를 검색 하는 방법을 알아봅니다.
ms.assetid: 5b4231c0-5060-49e2-a577-b747e20cf633
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp, 연결 된 장치, 원격 시스템, 로마, 프로젝트 로마
ms.localizationpriority: medium
ms.openlocfilehash: a479cb20943b9c4b2df53b22751c9de2f5a8402c
ms.sourcegitcommit: c3ca68e87eb06971826087af59adb33e490ce7da
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 09/02/2020
ms.locfileid: "89363736"
---
# <a name="discover-remote-devices"></a>원격 디바이스 검색
앱은 무선 네트워크, Bluetooth 및 클라우드 연결을 사용 하 여 검색 중인 장치와 동일한 Microsoft 계정로 로그온 한 Windows 장치를 검색할 수 있습니다. 원격 장치는 검색 하기 위해 특별 한 소프트웨어를 설치 하지 않아도 됩니다.

> [!NOTE]
> 이 가이드에서는 [원격 앱 시작](launch-a-remote-app.md)의 단계에 따라 원격 시스템 기능에 대 한 액세스 권한이 이미 부여 된 것으로 가정 합니다.

## <a name="filter-the-set-of-discoverable-devices"></a>검색 가능한 장치 집합 필터링
필터와 함께 [**RemoteSystemWatcher**](/uwp/api/Windows.System.RemoteSystems.RemoteSystemWatcher) 를 사용 하 여 검색 가능한 장치 집합의 범위를 좁힐 수 있습니다. 필터는 검색 유형 (proximal과 로컬 네트워크 및 클라우드 연결), 장치 유형 (데스크톱, 모바일 장치, Xbox, 허브 및 Holographic) 및 가용성 상태 (원격 시스템 기능을 사용 하는 장치의 가용성 상태)를 검색할 수 있습니다.

Filter 개체는 생성자에 매개 변수로 전달 되기 때문에 **RemoteSystemWatcher** 개체가 초기화 되기 전이나 도중에 생성 되어야 합니다. 다음 코드에서는 사용 가능한 각 유형의 필터를 만들고 목록에 추가합니다.

> [!NOTE]
> 이 예제의 코드를 사용 하려면 `using Windows.System.RemoteSystems` 파일에 문이 있어야 합니다.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/launch-resume/DiscoverDevices/cs/MainPage.xaml.cs" id="SnippetMakeFilterList":::

> [!NOTE]
> "Proximal" 필터 값은 물리적인 근접 정도를 보장 하지 않습니다. 안정적인 물리적 근접이 필요한 시나리오의 경우 필터에 SpatiallyProximal 값을 사용 [**RemoteSystemDiscoveryType.**](/uwp/api/windows.system.remotesystems.remotesystemdiscoverytype) 현재이 필터는 Bluetooth에서 검색 된 장치만 허용 합니다. 실제 근접성을 보장 하는 새로운 검색 메커니즘과 프로토콜이 지원 되므로 여기에도 포함 됩니다.  
[**RemoteSystem**](/uwp/api/Windows.System.RemoteSystems.RemoteSystem) 클래스에는 검색 된 장치가 실제로 근접 한 위치 내에 있는지 여부를 나타내는 속성도 있습니다. [**RemoteSystem. IsAvailableBySpatialProximity**](/uwp/api/Windows.System.RemoteSystems.RemoteSystem.IsAvailableByProximity).

> [!NOTE]
> 로컬 네트워크를 통해 장치를 검색 하려는 경우 (검색 유형 필터 선택에 따라 결정) 네트워크에서 "개인" 또는 "도메인" 프로필을 사용 해야 합니다. 장치는 "공용" 네트워크를 통해 다른 장치를 검색 하지 않습니다.

[**IRemoteSystemFilter**](/uwp/api/Windows.System.RemoteSystems.IRemoteSystemFilter) 개체 목록을 만든 후에는 **RemoteSystemWatcher**의 생성자에 전달할 수 있습니다.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/launch-resume/DiscoverDevices/cs/MainPage.xaml.cs" id="SnippetCreateWatcher":::

이 감시자의 [**시작**](/uwp/api/windows.system.remotesystems.remotesystemwatcher.start) 메서드를 호출 하면 다음 조건을 모두 충족 하는 장치가 검색 된 경우에만 [**RemoteSystemAdded**](/uwp/api/windows.system.remotesystems.remotesystemwatcher.remotesystemadded) 이벤트가 발생 합니다.
* Proximal 연결에서 검색할 수 있습니다.
* 데스크톱 또는 전화
* 사용 가능한 것으로 분류 됩니다.

여기에서 이벤트를 처리 하 고, [**RemoteSystem**](/uwp/api/Windows.System.RemoteSystems.RemoteSystem) 개체를 검색 하 고, 원격 장치에 연결 하는 절차는 [원격 앱을 시작](launch-a-remote-app.md)하는 것과 동일 합니다. 간단히 말해서 **RemoteSystem** 개체는 [**RemoteSystemAddedEventArgs**](/uwp/api/Windows.System.RemoteSystems.RemoteSystemAddedEventArgs) 개체의 속성으로 저장 됩니다 .이 속성은 각 **RemoteSystemAdded** 이벤트와 함께 전달 됩니다.

## <a name="discover-devices-by-address-input"></a>주소 입력으로 장치 검색
일부 장치는 사용자와 연결 되어 있지 않거나 검색을 통해 검색할 수 있지만 검색 하는 앱이 직접 주소를 사용 하는 경우에도 계속 연결할 수 있습니다. [**HostName**](/uwp/api/windows.networking.hostname) 클래스는 원격 장치의 주소를 나타내는 데 사용 됩니다. 이는 일반적으로 IP 주소 형식으로 저장 되지만 다른 몇 가지 형식을 사용할 수 있습니다. 자세한 내용은 [**HostName 생성자**](/uwp/api/windows.networking.hostname.-ctor) 를 참조 하십시오.

유효한 **HostName** 개체가 제공 되 면 **RemoteSystem** 개체가 검색 됩니다. 주소 데이터가 유효 하지 않으면 `null` 개체 참조가 반환 됩니다.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/launch-resume/DiscoverDevices/cs/MainPage.xaml.cs" id="SnippetFindByHostName":::

## <a name="querying-a-capability-on-a-remote-system"></a>원격 시스템에서 기능 쿼리

검색 필터링과는 별개 이지만 장치 기능을 쿼리 하는 작업은 검색 프로세스의 중요 한 부분입니다. [**RemoteSystem GetCapabilitySupportedAsync**](/uwp/api/windows.system.remotesystems.remotesystem.GetCapabilitySupportedAsync) 메서드를 사용 하 여 원격 세션 연결 또는 holographic (공간 엔터티) 공유와 같은 특정 기능을 지원 하기 위해 검색 된 원격 시스템을 쿼리할 수 있습니다. 쿼리 가능한 기능 목록은 [**KnownRemoteSystemCapabilities**](/uwp/api/windows.system.remotesystems.knownremotesystemcapabilities) 클래스를 참조 하세요.

```csharp
// Check to see if the given remote system can accept LaunchUri requests
bool isRemoteSystemLaunchUriCapable = remoteSystem.GetCapabilitySupportedAsync(KnownRemoteSystemCapabilities.LaunchUri);
```

## <a name="cross-user-discovery"></a>사용자 간 검색

개발자는 동일한 사용자에 등록 된 장치만이 아니라 클라이언트 장치에 근접 한 _모든_ 장치의 검색을 지정할 수 있습니다. 특수 한 **IRemoteSystemFilter**, [**RemoteSystemAuthorizationKindFilter**](/uwp/api/windows.system.remotesystems.remotesystemauthorizationkindfilter)을 통해 구현 됩니다. 다른 필터 형식과 같이 구현 됩니다.

```csharp
// Construct a user type filter that includes anonymous devices
RemoteSystemAuthorizationKindFilter authorizationKindFilter = new RemoteSystemAuthorizationKindFilter(RemoteSystemAuthorizationKind.Anonymous);
// then add this filter to the RemoteSystemWatcher
```

* [**RemoteSystemAuthorizationKind**](/uwp/api/windows.system.remotesystems.remotesystemauthorizationkind) 값이 **익명** 이면 신뢰할 수 없는 사용자의 모든 proximal 장치를 검색할 수 있습니다.
* **SameUser** 값은 검색을 클라이언트 장치와 동일한 사용자에 등록 된 장치만 필터링 합니다. 기본 동작입니다.

### <a name="checking-the-cross-user-sharing-settings"></a>사용자 간 공유 설정 확인

검색 앱에서 위의 필터를 지정 하는 것 외에도 클라이언트 장치 자체를 구성 하 여 다른 사용자와 로그인 한 장치의 공유 환경을 허용 해야 합니다. 다음은 **RemoteSystem** 클래스에서 정적 메서드를 사용 하 여 쿼리할 수 있는 시스템 설정입니다.

```csharp
if (!RemoteSystem.IsAuthorizationKindEnabled(RemoteSystemAuthorizationKind.Anonymous)) {
    // The system is not authorized to connect to cross-user devices. 
    // Inform the user that they can discover more devices if they
    // update the setting to "Anonymous".
}
```

이 설정을 변경 하려면 사용자가 **설정** 앱을 열어야 합니다. 장치에서 **시스템**  >  **공유 환경**  >  **공유** 메뉴에는 사용자가 시스템을 공유할 수 있는 장치를 지정할 수 있는 드롭다운 상자가 있습니다.

![공유 환경 설정 페이지](images/shared-experiences-settings.png)

## <a name="related-topics"></a>관련 항목
* [연결된 앱 및 디바이스(프로젝트 로마)](connected-apps-and-devices.md)
* [원격 앱 시작](launch-a-remote-app.md)
* [원격 시스템 API 참조](/uwp/api/Windows.System.RemoteSystems)
* [원격 시스템 샘플](https://github.com/Microsoft/Windows-universal-samples/tree/dev/Samples/RemoteSystems)
