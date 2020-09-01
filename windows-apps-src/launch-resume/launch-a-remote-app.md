---
title: 원격 디바이스에서 앱 시작
description: 다른 장치에서 UWP 앱 또는 Windows 데스크톱 응용 프로그램을 원격으로 시작 하 여 사용자가 한 장치에서 작업을 시작 하 고 다른 장치에서 작업을 완료할 수 있도록 하는 방법을 알아봅니다.
ms.date: 02/12/2018
ms.topic: article
keywords: windows 10, uwp, 연결 된 장치, 원격 시스템, 로마, 프로젝트 로마
ms.assetid: 54f6a33d-a3b5-4169-8664-653dbab09175
ms.localizationpriority: medium
ms.openlocfilehash: 784403ede6b21b79dcb14d1da6dde22df68c410e
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89158787"
---
# <a name="launch-an-app-on-a-remote-device"></a>원격 디바이스에서 앱 시작

이 문서에서는 원격 장치에서 Windows 앱을 시작 하는 방법을 설명 합니다.

Windows 10 버전 1607부터 UWP 앱은 동일한 Microsoft 계정 (MSA)을 사용 하 여 두 장치를 모두 사용 하는 경우 Windows 10, 버전 1607 이상을 실행 하는 다른 장치에서 UWP 앱 또는 Windows 데스크톱 응용 프로그램을 원격으로 시작할 수 있습니다. 이는 Project 로마의 가장 간단한 사용 사례입니다.

원격 시작 기능을 사용 하면 작업 지향적인 사용자 환경을 사용할 수 있습니다. 사용자는 한 장치에서 작업을 시작 하 고 다른 장치에서 작업을 완료할 수 있습니다. 예를 들어 사용자가 자동차의 휴대폰에서 음악을 수신 하는 경우 집에 도착할 때 Xbox One에 재생 기능을 직접 제공할 수 있습니다. 원격 시작을 사용 하면 앱이 시작 되는 원격 앱에 컨텍스트 데이터를 전달 하 여 작업이 중단 된 위치를 선택할 수 있습니다.

## <a name="preliminary-setup"></a>예비 설정

### <a name="add-the-remotesystem-capability"></a>RemoteSystem 기능 추가

앱이 원격 장치에서 앱을 시작 하도록 하려면 `remoteSystem` 앱 패키지 매니페스트에 기능을 추가 해야 합니다. 패키지 매니페스트 디자이너를 사용 하 여 **기능** 탭에서 **원격 시스템** 을 선택 하 여 추가 하거나 프로젝트의 _appxmanifest.xml_ 파일에 다음 줄을 수동으로 추가할 수 있습니다.

``` xml
<Capabilities>
   <uap3:Capability Name="remoteSystem"/>
</Capabilities>
```

### <a name="enable-cross-device-sharing"></a>장치 간 공유 사용

또한 클라이언트 장치에서 장치 간 공유를 허용 하도록 설정 해야 합니다. **설정**에서 액세스 하는이 설정: 장치에서 **시스템**  >  **공유 환경**  >  **공유**는 기본적으로 사용 하도록 설정 되어 있습니다. 

![공유 환경 설정 페이지](images/shared-experiences-settings.png)

## <a name="find-a-remote-device"></a>원격 장치 찾기

먼저 연결 하려는 장치를 찾아야 합니다. [원격 장치 검색](discover-remote-devices.md) 이 작업을 수행 하는 방법에 대해 자세히 설명 합니다. 여기서는 장치 또는 연결 유형별로 필터링 하는 간단한 방법을 사용 합니다. 원격 장치를 검색 하는 원격 시스템 감시자를 만들고 장치가 검색 되거나 제거 될 때 발생 하는 이벤트에 대 한 처리기를 작성 합니다. 그러면 원격 장치 컬렉션을 제공 합니다.

이 예제의 코드를 사용 하려면 `using Windows.System.RemoteSystems` 클래스 파일에 문이 있어야 합니다.

[!code-cs[Main](./code/RemoteLaunchScenario/MainPage.xaml.cs#SnippetBuildDeviceList)]

원격 시작을 수행 하기 전에 수행 해야 하는 첫 번째 작업은 호출 `RemoteSystem.RequestAccessAsync()` 입니다. 반환 값을 확인 하 여 앱이 원격 장치에 액세스할 수 있는지 확인 합니다. 앱에 기능을 추가 하지 않은 경우이 검사가 실패할 수 있습니다 `remoteSystem` .

시스템 감시자 이벤트 처리기는 연결할 수 있는 장치를 검색 하거나 더 이상 사용할 수 없을 때 호출 됩니다. 이러한 이벤트 처리기를 사용 하 여 연결할 수 있는 장치의 업데이트 된 목록을 유지 합니다.

[!code-cs[Main](./code/RemoteLaunchScenario/MainPage.xaml.cs#SnippetEventHandlers)]


**사전을**사용 하 여 원격 시스템 ID로 장치를 추적 합니다. **System.collections.objectmodel.observablecollection** 는 열거할 수 있는 장치 목록을 저장 하는 데 사용 됩니다. 또한 **system.collections.objectmodel.observablecollection** 를 사용 하면 장치 목록을 UI에 쉽게 바인딩할 수 있지만이 예제에서는이 작업을 수행 하지 않습니다.

[!code-cs[Main](./code/RemoteLaunchScenario/MainPage.xaml.cs#SnippetMembers)]

`BuildDeviceList()`원격 앱을 시작 하기 전에 앱 시작 코드에에 대 한 호출을 추가 합니다.

## <a name="launch-an-app-on-a-remote-device"></a>원격 디바이스에서 앱 시작

연결 하려는 장치를 [**RemoteLauncher LaunchUriAsync**](/uwp/api/windows.system.remotelauncher.launchuriasync) API에 전달 하 여 원격으로 앱을 시작 합니다. 이 메서드에는 세 가지 오버 로드가 있습니다. 이 예제에서 설명 하는 가장 간단한 방법은 원격 장치에서 앱을 활성화할 URI를 지정 하는 것입니다. 이 예제에서 URI는 공간 니 들의 3D 뷰를 사용 하 여 원격 컴퓨터에서 맵 앱을 엽니다.

다른 **RemoteLauncher LaunchUriAsync** 오버 로드를 사용 하 여 원격 장치에서 적절 한 앱을 시작할 수 없는 경우 표시할 웹 사이트의 uri와 같은 옵션을 지정 하 고, 원격 장치에서 uri를 시작 하는 데 사용할 수 있는 패키지 패밀리 이름의 선택적 목록을 지정할 수 있습니다. 데이터를 키/값 쌍의 형식으로 제공할 수도 있습니다. 활성화 하는 앱에 데이터를 전달 하 여 재생할 노래 이름과 현재 재생 위치 (예: 한 장치에서 다른 장치로 재생을 전달할 때)와 같은 원격 앱에 컨텍스트를 제공할 수 있습니다.

실용적인 시나리오에서는 대상으로 지정할 장치를 선택 하는 UI를 제공할 수 있습니다. 그러나이 예제를 간소화 하기 위해 목록의 첫 번째 원격 장치만 사용 합니다.

[!code-cs[Main](./code/RemoteLaunchScenario/MainPage.xaml.cs#SnippetRemoteUriLaunch)]

**LaunchUriAsync ()** 에서 반환 된 [**RemoteLaunchUriStatus**](/uwp/api/windows.system.remotelaunchuristatus) 개체는 원격 시작에 성공 했는지 여부에 대 한 정보를 제공 하 고, 그렇지 않은 경우 이유를 제공 합니다.

## <a name="related-topics"></a>관련 항목

[원격 시스템 API 참조](/uwp/api/Windows.System.RemoteSystems)  
[연결 된 앱 및 장치 (Project 로마) 개요](connected-apps-and-devices.md)  
[원격 디바이스 검색](discover-remote-devices.md)  
[원격 시스템 샘플](https://github.com/Microsoft/Windows-universal-samples/tree/dev/Samples/RemoteSystems) 은 원격 시스템을 검색 하 고, 원격 시스템에서 앱을 시작 하 고, 앱 서비스를 사용 하 여 두 시스템에서 실행 되는 앱 간에 메시지를 보내는 방법을 보여 줍니다.