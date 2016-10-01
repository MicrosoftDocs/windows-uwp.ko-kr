---
author: TylerMSFT
title: "원격 디바이스에서 앱 시작"
description: "&quot;로마&quot; 프로젝트를 사용하여 원격 디바이스에서 앱을 시작하는 방법을 알아봅니다."
translationtype: Human Translation
ms.sourcegitcommit: ff8e16d0e376d502157ae42b9cdae11875008554
ms.openlocfilehash: d8c3783d68a1b3b216058790d84255a7fb4b612c

---

# 원격 디바이스에서 앱 시작

이 문서에서는 원격 디바이스에서 UWP(유니버설 Windows 플랫폼) 앱 또는 Windows 데스크톱 앱을 시작하는 방법을 설명합니다.

Windows 10 버전 1607부터 UWP 앱은 Windows 10 버전 1607 이상을 실행하는 다른 디바이스에서 UWP 앱 또는 Windows 데스크톱 응용 프로그램을 원격으로 시작할 수 있습니다.

원격 디바이스에서 앱을 시작하는 시나리오 중 하나는 한 디바이스에서 작업을 시작하고 다른 디바이스에서 종료하는 것입니다. 예를 들어 퇴근 중 차 안에서 휴대폰으로 음악을 듣고 집에 도착한 후 원격 시작을 사용하여 Xbox를 통해 계속 재생할 수 있습니다. 원격 앱으로 데이터를 전달하여 원격 앱에서 작업을 계속하기 위한 컨텍스트를 제공할 수 있습니다.

## RemoteSystem 접근 권한 값 추가

사용 중인 앱에서 원격 디바이스의 앱을 시작하려면 앱 패키지 매니페스트에 &lt;uap3:Capability Name="remoteSystem"/&gt; 접근 권한 값을 추가해야 합니다. 패키지 매니페스트 디자이너의 **접근 권한 값** 탭에서 **원격 시스템**을 선택하여 추가하거나 패키지 매니페스트 디자이너에서 수행할 작업을 수동으로 수행하고 Package.appxmanifest 파일에 다음을 추가할 수 있습니다.

``` xml
<Capabilities>
   <uap3:Capability Name="remoteSystem"/>
 </Capabilities>
```
## 원격 디바이스 찾기

먼저 연결할 디바이스를 찾아야 합니다. [원격 디바이스 검색](discover-remote-devices.md)에서 이 작업 방법에 대해 자세히 설명합니다. 여기서는 간단하게 디바이스나 연결 형식별로 필터링하는 방법을 사용합니다. 원격 디바이스를 검색하는 원격 시스템 감시자를 만들고 동일한 Microsoft 계정을 사용하는 디바이스가 검색 또는 제거될 때 알림을 표시하는 이벤트 처리기를 작성합니다. 이렇게 하면 원격 디바이스 모음이 제공됩니다.

이 예제의 코드에서는 파일에 `using Windows.System.RemoteSystems` 문이 있다고 가정합니다.

[!code-cs[기본](./code/RemoteLaunchScenario/MainPage.xaml.cs#SnippetBuildDeviceList)]

원격 시작 전에 제일 먼저 수행해야 할 작업은 `RemoteSystem.RequestAccessAsync()` 호출입니다. 앱에서 원격 디바이스에 액세스할 수 있도록 하려면 반환 값을 확인합니다. `remoteSystem` 접근 권한 값을 앱에 추가하지 않은 경우 이 검사가 실패할 수 있습니다.

시스템 감시자 이벤트 처리기는 연결할 수 있는 디바이스를 찾거나 더 이상 사용할 수 없을 때 호출됩니다. 이러한 이벤트 처리기를 사용하여 연결할 수 있는 디바이스의 업데이트된 목록을 유지합니다.

[!code-cs[기본](./code/RemoteLaunchScenario/MainPage.xaml.cs#SnippetEventHandlers)]

사전을 사용하여 원격 시스템 ID별로 디바이스를 추적합니다. ObservableCollection은 열거할 수 있는 디바이스 목록을 저장하는 데 사용됩니다. 또한 디바이스 목록을 UI에 쉽게 바인딩하는 용도로도 사용되지만 이 예제에서는 다루지 않습니다.

[!code-cs[기본](./code/RemoteLaunchScenario/MainPage.xaml.cs#SnippetMembers)]

원격 앱을 시작하기 전에 앱 시작 코드의 `BuildDeviceList()`에 호출을 추가합니다.

## 원격 디바이스에서 앱 시작

[RemoteLauncher.LaunchUri](https://msdn.microsoft.com/en-us/library/windows/apps/windows.system.remotelauncher.launchuriasync.aspx) API와 연결하려는 디바이스를 전달하여 원격으로 앱을 시작합니다. 이 함수에 대한 오버로드는 세 가지가 있습니다. 가장 간단한 오버로드는 이 예제처럼 원격 디바이스에서 앱을 활성화하는 URI를 지정합니다. 이 예제에서는 URI가 Space Needle의 3D 뷰를 사용하여 원격 컴퓨터에서 지도 앱을 엽니다.

다른 **RemoteLauncher.LaunchUriAsync** 오버로드를 사용하면 URI를 처리할 수 있는 앱을 원격 디바이스에서 시작할 수 없는 경우 표시할 웹 사이트의 URI 및 원격 디바이스에서 URI를 시작하는 데 사용할 수 있는 패키지 패밀리 이름의 선택적 목록과 같은 옵션을 지정할 수 있습니다. 키/값 쌍의 형태로 데이터를 제공할 수도 있습니다. 원격 디바이스에서 활성화 중인 앱에 데이터를 전달하여 디바이스 간에 재생을 전달할 때 재생할 곡 이름 및 현재 재생 위치 등 컨텍스트를 원격 앱에 제공할 수 있습니다.

실제로 사용할 디바이스를 선택하는 UI를 제공할 수 있습니다. 하지만 이 예제에서는 간단히 목록의 첫 번째 방법을 사용하여 원격 호출을 수행합니다.

[!code-cs[기본](./code/RemoteLaunchScenario/MainPage.xaml.cs#SnippetRemoteUriLaunch)]

**RemoteLauncher.LaunchUriAsync()**에서 반환되는 [RemoteLaunchUriStatus](https://msdn.microsoft.com/en-us/library/windows/apps/windows.system.remotelaunchuristatus.aspx)는 원격 시작의 성공 여부와 그 이유에 대한 정보를 제공합니다.

## 관련 항목

[원격 시스템 API 참조](https://msdn.microsoft.com/en-us/library/windows/apps/Windows.System.RemoteSystems)  
[연결된 앱 및 디바이스(프로젝트 "로마") 개요](connected-apps-and-devices.md)  
[원격 시스템 샘플](https://github.com/Microsoft/Windows-universal-samples/tree/dev/Samples/RemoteSystems )은 원격 시스템 검색, 원격 시스템에서 앱 실행, 앱 서비스를 사용하여 두 시스템에서 실행 중인 앱 간에 메시지 전송 방법을 보여 줍니다.



<!--HONumber=Aug16_HO5-->


