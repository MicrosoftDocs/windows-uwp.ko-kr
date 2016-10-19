---
author: PatrickFarley
title: "원격 디바이스 검색"
description: "프로젝트 로마를 사용하여 앱에서 원격 디바이스를 검색하는 방법을 알아봅니다."
translationtype: Human Translation
ms.sourcegitcommit: ff8e16d0e376d502157ae42b9cdae11875008554
ms.openlocfilehash: cb1f9cf6915378203919fdf63bcebc935af74a30

---

# 원격 디바이스 검색
앱에서 무선 네트워크, Bluetooth 및 클라우드 연결을 사용하여 검색 디바이스와 동일한 Microsoft 계정으로 로그온된 Windows 기반 디바이스를 검색할 수 있습니다. Surface Hub, Xbox One 등 익명 연결을 허용하는 공동 디바이스도 검색할 수 있습니다. 특별한 소프트웨어가 설치되어 있지 않아도 원격 디바이스를 검색할 수 있습니다.

> [!NOTE]
> 이 가이드에서는 [원격 앱 실행](launch-a-remote-app.md)의 단계에 따라 원격 시스템 기능에 대한 액세스 권한이 이미 부여되었다고 가정합니다.

## 검색할 수 있는 디바이스 집합 필터링
필터와 함께 [**RemoteSystemWatcher**](https://msdn.microsoft.com/library/windows/apps/Windows.System.RemoteSystems.RemoteSystemWatcher)를 사용하여 검색할 수 있는 디바이스 집합 범위를 좁힐 수 있습니다. 필터는 검색 유형(로컬 네트워크 및 클라우드 연결), 디바이스 유형(데스크톱, 휴대폰, Xbox, 허브 및 Holographic), 가용성 상태(원격 시스템 기능을 사용할 디바이스의 가용성 상태)를 감지할 수 있습니다.

필터 개체가 해당 생성자에 매개 변수로 전달되기 때문에 **RemoteSystemWatcher** 개체가 초기화되기 전에 필터 개체를 생성해야 합니다. 다음 코드에서는 사용 가능한 각 유형의 필터를 만들고 목록에 추가합니다.

> [!NOTE]
> 이 예제의 코드에서는 파일에 `using Windows.System.RemoteSystems` 문이 있다고 가정합니다.

[!code-cs[기본](./code/DiscoverDevices/MainPage.xaml.cs#SnippetMakeFilterList)]

[**IRemoteSystemFilter**](https://msdn.microsoft.com/library/windows/apps/Windows.System.RemoteSystems.IRemoteSystemFilter) 개체 목록이 생성되면 **RemoteSystemWatcher**의 생성자에 전달할 수 있습니다.

[!code-cs[기본](./code/DiscoverDevices/MainPage.xaml.cs#SnippetCreateWatcher)]

이 감시자의 [**Start**](https://msdn.microsoft.com/library/windows/apps/Windows.System.RemoteSystems.RemoteSystemWatcher.Start) 메서드는 호출 시 다음 조건을 모두 충족하는 디바이스가 검색된 경우에만 [**RemoteSystemAdded**](https://msdn.microsoft.com/library/windows/apps/Windows.System.RemoteSystems.RemoteSystemWatcher.RemoteSystemAdded) 이벤트를 발생합니다.
* 근접 연결을 통해 검색할 수 있는 경우
* 데스크톱 또는 휴대폰인 경우
* 사용 가능한 것으로 분류된 경우

여기서 이벤트를 처리하고, [**RemoteSystem**](https://msdn.microsoft.com/library/windows/apps/Windows.System.RemoteSystems.RemoteSystem) 개체를 검색하고, 원격 디바이스에 연결하는 절차는 [원격 앱 실행](launch-a-remote-app.md)과 동일합니다. 즉, **RemoteSystem** 개체가 각 **RemoteSystemAdded** 이벤트의 매개 변수인 [**RemoteSystemAddedEventArgs**](https://msdn.microsoft.com/library/windows/apps/Windows.System.RemoteSystems.RemoteSystemAddedEventArgs) 개체의 속성으로 저장됩니다.

## 주소를 입력하여 디바이스 검색
일부 디바이스는 사용자와 연결되어 있지 않거나 검색을 통해 찾을 수 없지만 검색 앱에서 직접 주소를 사용할 경우 연결할 수 있습니다. [**HostName**](https://msdn.microsoft.com/library/windows/apps/windows.networking.hostname.aspx) 클래스는 원격 디바이스의 주소를 나타내는 데 사용됩니다. IP 주소의 형태로 저장되는 경우가 많지만 다른 여러 형식도 허용됩니다(자세한 내용은 [**HostName 생성자**](https://msdn.microsoft.com/library/windows/apps/br207118.aspx) 참조).

유효한 **HostName** 개체를 제공하면 **RemoteSystem** 개체가 검색됩니다. 주소 데이터가 잘못된 경우 `null` 개체 참조가 반환됩니다.

[!code-cs[기본](./code/DiscoverDevices/MainPage.xaml.cs#SnippetFindByHostName)]

## 관련 항목
[연결된 앱 및 디바이스(프로젝트 "로마")](connected-apps-and-devices.md)  
[원격 앱 실행](launch-a-remote-app.md)  
[원격 시스템 API 참조](https://msdn.microsoft.com/library/windows/apps/Windows.System.RemoteSystems)  
[원격 시스템 샘플](https://github.com/Microsoft/Windows-universal-samples/tree/dev/Samples/RemoteSystems )은 원격 시스템 검색, 원격 시스템에서 앱 실행, 앱 서비스를 사용하여 두 시스템에서 실행 중인 앱 간에 메시지 전송 방법을 보여 줍니다.



<!--HONumber=Aug16_HO5-->


