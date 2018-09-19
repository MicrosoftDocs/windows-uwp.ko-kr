---
author: PatrickFarley
title: 원격 디바이스에서 앱 실행
description: 프로젝트 로마를 사용하여 원격 장치에서 앱을 시작하는 방법을 알아봅니다.
ms.author: pafarley
ms.date: 02/12/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp, 연결 된 장치, 원격 시스템, 로마, 프로젝트 로마
ms.assetid: 54f6a33d-a3b5-4169-8664-653dbab09175
ms.localizationpriority: medium
ms.openlocfilehash: 58a420d73ba4a0cd51f909fd5d7d417af1cfb38f
ms.sourcegitcommit: 68fcac3288d5698a13dbcbd57f51b30592f24860
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 09/19/2018
ms.locfileid: "4056739"
---
# <a name="launch-an-app-on-a-remote-device"></a>원격 디바이스에서 앱 실행

이 문서에서는 원격 디바이스에서 Windows 앱을 실행하는 방법을 설명합니다.

Windows 10 버전 1607부터 UWP 앱은 두 디바이스가 동일한 MSA(Microsoft 계정)로 서명된 경우 Windows 10 버전 1607 이상을 실행하는 다른 디바이스에서 UWP 앱 또는 Windows 데스크톱 응용 프로그램을 원격으로 실행할 수 있습니다. 다음은 프로젝트 로마의 가장 간단한 사용 사례입니다.

원격 실행 기능은 작업 지향 사용자 환경을 지원합니다. 사용자는 한 장치에서 작업을 시작한 후 다른 장치에서 완료할 수 있습니다. 예를 들어, 사용자가 자동차에서 휴대폰으로 음악을 듣고 있는 경우 집에 도착해서 재생 기능을 Xbox One으로 넘길 수 있습니다. 원격 실행을 사용하면 작업이 종료된 지점부터 시작하기 위해 앱이 실행 중인 원격 앱에 맥락 기반 데이터(contextual data)를 전달할 수 있습니다.

## <a name="preliminary-setup"></a>사전 설정

### <a name="add-the-remotesystem-capability"></a>remoteSystem 접근 권한 값 추가

앱이 원격 디바이스에서 다른 앱을 실행하려면 앱 패키지 매니페스트에 `remoteSystem` 접근 권한 값을 추가해야 합니다. 패키지 매니페스트 디자이너의 **접근 권한 값** 탭에서 **원격 시스템**을 선택하여 접근 권한 값을 추가하거나 프로젝트의 _Package.appxmanifest_ 파일에 다음 줄을 수동으로 추가할 수 있습니다.

``` xml
<Capabilities>
   <uap3:Capability Name="remoteSystem"/>
</Capabilities>
```

### <a name="enable-cross-device-sharing"></a>장치 간 공유 사용

또한 클라이언트 장치를 장치 간 공유를 허용하도록 설정해야 합니다. 이 설정은 기본적으로 사용되며 **설정**: **시스템** > **공유 환경** > **장치 간 공유**에서 액세스할 수 있습니다. 

![공유 환경 설정 페이지](images/shared-experiences-settings.png)

## <a name="find-a-remote-device"></a>원격 장치 찾기

먼저 연결할 디바이스를 찾아야 합니다. [원격 디바이스 검색](discover-remote-devices.md)에서 이 작업 방법에 대해 자세히 설명합니다. 여기서는 디바이스나 연결 형식별 필터링을 지원하지 않는 간단한 방법을 사용합니다. 원격 디바이스를 검색하는 원격 시스템 감시자를 만들고 디바이스가 검색 또는 제거될 때 발생하는 이벤트 처리기를 작성합니다. 이렇게 하면 원격 장치 컬렉션이 제공됩니다.

이 예의 코드를 사용하려면 클래스 파일에 `using Windows.System.RemoteSystems` 문이 있어야 합니다.

[!code-cs[Main](./code/RemoteLaunchScenario/MainPage.xaml.cs#SnippetBuildDeviceList)]

원격 시작 전에 제일 먼저 수행해야 할 작업은 `RemoteSystem.RequestAccessAsync()` 호출입니다. 앱에서 원격 디바이스에 액세스할 수 있도록 하려면 반환 값을 확인합니다. `remoteSystem` 접근 권한 값을 앱에 추가하지 않은 경우 이 검사가 실패할 수 있습니다.

시스템 감시자 이벤트 처리기는 연결할 수 있는 디바이스를 찾거나 더 이상 사용할 수 없을 때 호출됩니다. 이러한 이벤트 처리기를 사용하여 연결할 수 있는 장치의 업데이트된 목록을 유지합니다.

[!code-cs[Main](./code/RemoteLaunchScenario/MainPage.xaml.cs#SnippetEventHandlers)]


**사전**을 사용하여 원격 시스템 ID별로 장치를 추적합니다. **ObservableCollection**은 열거할 수 있는 디바이스 목록을 저장하는 데 사용됩니다. **ObservableCollection**은 장치 목록을 UI에 쉽게 바인딩하는 용도로도 사용되지만 이 예제에서는 다루지 않습니다.

[!code-cs[Main](./code/RemoteLaunchScenario/MainPage.xaml.cs#SnippetMembers)]

원격 앱을 시작하기 전에 앱 시작 코드의 `BuildDeviceList()`에 호출을 추가합니다.

## <a name="launch-an-app-on-a-remote-device"></a>원격 디바이스에서 앱 실행

[**RemoteLauncher.LaunchUriAsync**](https://msdn.microsoft.com/library/windows/apps/windows.system.remotelauncher.launchuriasync.aspx) API와 연결하려는 디바이스를 전달하여 원격으로 앱을 시작합니다. 이 메서드에 대한 오버로드는 세 가지가 있습니다. 가장 간단한 오버로드는 이 예제처럼 원격 디바이스에서 앱을 활성화하는 URI를 지정합니다. 이 예제에서는 URI가 Space Needle의 3D 뷰를 사용하여 원격 컴퓨터에서 지도 앱을 엽니다.

다른 **RemoteLauncher.LaunchUriAsync** 오버로드를 사용하면 적절한 앱을 원격 디바이스에서 시작할 수 없는 경우 표시할 웹 사이트의 URI 및 원격 디바이스에서 URI를 시작하는 데 사용할 수 있는 패키지 패밀리 이름의 선택적 목록과 같은 옵션을 지정할 수 있습니다. 키/값 쌍의 형식으로 데이터를 제공할 수도 있습니다. 활성화 중인 앱에 데이터를 전달하여 디바이스 간에 재생을 전달할 때 재생할 곡 이름 및 현재 재생 위치 등의 컨텍스트를 원격 앱에 제공할 수 있습니다.

실제 시나리오에서는 대상 디바이스를 선택하기 위한 UI를 제공할 수도 있습니다. 하지만 이 예제에서는 간단히 목록의 첫 번째 원격 장치를 사용합니다.

[!code-cs[Main](./code/RemoteLaunchScenario/MainPage.xaml.cs#SnippetRemoteUriLaunch)]

**RemoteLauncher.LaunchUriAsync()** 에서 반환되는 [**RemoteLaunchUriStatus**](https://msdn.microsoft.com/library/windows/apps/windows.system.remotelaunchuristatus.aspx) 개체는 원격 시작의 성공 여부와 실패한 경우 그 이유에 대한 정보를 제공합니다.

## <a name="related-topics"></a>관련 항목

[원격 시스템 API 참조](https://msdn.microsoft.com/library/windows/apps/Windows.System.RemoteSystems)  
[연결된 앱 및 디바이스(프로젝트 로마) 개요](connected-apps-and-devices.md)  
[원격 디바이스 검색](discover-remote-devices.md)  
[원격 시스템 샘플](https://github.com/Microsoft/Windows-universal-samples/tree/dev/Samples/RemoteSystems)은 원격 시스템을 검색하고, 원격 시스템에서 앱을 실행하고, 앱 서비스를 사용하여 두 시스템에서 실행 중인 앱 간에 메시지를 보내는 방법을 보여 줍니다.
