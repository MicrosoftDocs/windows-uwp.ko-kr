---
title: 원격 앱 서비스와 통신
description: 프로젝트 로마를 사용하여 원격 디바이스에서 실행 중인 앱 서비스와 메시지를 교환하세요.
ms.assetid: a0261e7a-5706-4f9a-b79c-46a3c81b136f
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp, 연결 된 장치, 원격 시스템, 로마, 프로젝트 로마, 백그라운드 작업, 앱 서비스
ms.localizationpriority: medium
ms.openlocfilehash: ddadae05ca3243f9bbd6b53cbb98f234ac560acd
ms.sourcegitcommit: 681c70f964210ab49ac5d06357ae96505bb78741
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/26/2018
ms.locfileid: "7709065"
---
# <a name="communicate-with-a-remote-app-service"></a>원격 앱 서비스와 통신

URI를 사용하여 원격 디바이스에서 앱을 실행하는 것은 물론 원격 디바이스에서 *앱 서비스*를 실행하고 통신할 수도 있습니다. 모든 Windows 기반 디바이스를 클라이언트 또는 호스트 디바이스로 사용할 수 있습니다. 따라서 앱을 포그라운드로 전환할 필요 없이 연결된 디바이스를 다양한 방법으로 조작할 수 있습니다.

## <a name="set-up-the-app-service-on-the-host-device"></a>호스트 디바이스에서 앱 서비스 설정
원격 디바이스에서 앱 서비스를 실행하려면 해당 디바이스에 앱 서비스 공급자가 이미 설치되어 있어야 합니다. 이 가이드에서는 CSharp 버전의 [난수 생성기 앱 서비스 샘플](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/AppServices)을 사용합니다. 이 샘플은 [Windows 유니버설 샘플 리포지토리](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/AppServices)에 있습니다. 고유한 앱 서비스를 작성하는 방법에 대한 자세한 내용은 [앱 서비스 만들기 및 사용](how-to-create-and-consume-an-app-service.md)을 참조하세요.

이미 만들어진 앱 서비스를 사용하든, 고유한 앱 서비스를 작성하든 관계없이 서비스가 원격 시스템과 호환되도록 하려면 몇 가지 편집 작업이 필요합니다. Visual Studio에서 앱 서비스 공급자 프로젝트(샘플의 "AppServicesProvider")로 이동한 다음 해당 _Package.appxmanifest_ 파일을 선택합니다. 마우스 오른쪽 단추를 클릭하고 **코드 보기**를 선택하여 파일의 전체 내용을 표시합니다. 기본 **응용 프로그램** 요소 내부에서 **Extensions** 요소가 만듭니다 (또는 이미 있는 경우 찾기) 합니다. 그런 다음 **확장** 앱 서비스 프로젝트를 정의 하 고 부모 프로젝트 참조를 만듭니다.

``` xml
...
<Extensions>
    <uap:Extension Category="windows.appService" EntryPoint="RandomNumberService.RandomNumberGeneratorTask">
        <uap3:AppService Name="com.microsoft.randomnumbergenerator"/>
    </uap:Extension>
</Extensions>
...
```

**AppService** 요소 옆 **SupportsRemoteSystems** 특성을 추가 합니다.

``` xml
...
<uap3:AppService Name="com.microsoft.randomnumbergenerator" SupportsRemoteSystems="true"/>
...
```

이 **uap3** 네임 스페이스의 요소를 사용 하려면 추가 해야 네임 스페이스 정의 매니페스트 파일의 위쪽에 있지 않은 경우 있습니다.

```xml
<?xml version="1.0" encoding="utf-8"?>
<Package
  xmlns="http://schemas.microsoft.com/appx/manifest/foundation/windows10"
  xmlns:mp="http://schemas.microsoft.com/appx/2014/phone/manifest"
  xmlns:uap="http://schemas.microsoft.com/appx/manifest/uap/windows10"
  xmlns:uap3="http://schemas.microsoft.com/appx/manifest/uap/windows10/3">
  ...
</Package>
```

그런 다음 앱 서비스 공급자 프로젝트를 빌드하고 호스트 장치에 배포 합니다.

## <a name="target-the-app-service-from-the-client-device"></a>클라이언트 디바이스에서 앱 서비스를 대상으로 지정
원격 앱 서비스가 호출되는 디바이스에 원격 시스템 기능을 가진 앱이 있어야 합니다. 호스트 디바이스에서 앱 서비스를 제공하는 앱에 이 앱을 추가하거나(이 경우 두 디바이스에 동일한 앱을 설치함) 완전히 다른 앱에 구현할 수 있습니다.

다음 **using** 문은 이 섹션의 코드를 현재 그대로 실행하는 데 필요합니다.

[!code-cs[Main](./code/RemoteAppService/MainPage.xaml.cs#SnippetUsings)]


먼저 로컬에서 앱 서비스를 호출하는 것처럼 [**AppServiceConnection**](https://msdn.microsoft.com/library/windows/apps/Windows.ApplicationModel.AppService.AppServiceConnection) 개체를 인스턴스화해야 합니다. 이 프로세스는 [앱 서비스 만들기 및 사용](how-to-create-and-consume-an-app-service.md)에서 자세히 설명합니다. 이 예제에서 대상으로 지정할 앱 서비스는 난수 생성기 서비스입니다.

> [!NOTE]
> 다음 메서드를 호출하는 코드 내에서 임의 방법으로 [RemoteSystem](https://msdn.microsoft.com/library/windows/apps/Windows.System.RemoteSystems.RemoteSystem) 개체를 이미 얻었다고 가정합니다. 설정 방법에 대한 자세한 내용은 [원격 앱 실행](launch-a-remote-app.md)을 참조하세요.

[!code-cs[Main](./code/RemoteAppService/MainPage.xaml.cs#SnippetAppService)]

그 다음에 의도한 원격 장치에 대한 [**RemoteSystemConnectionRequest**](https://msdn.microsoft.com/library/windows/apps/Windows.System.RemoteSystems.RemoteSystemConnectionRequest) 개체가 생성됩니다. 이 개체를 사용하여 해당 디바이스에 대한 **AppServiceConnection**을 엽니다. 아래 예제에서는 간단한 설명을 위해 오류 처리와 보고가 매우 간소화되었습니다.

[!code-cs[Main](./code/RemoteAppService/MainPage.xaml.cs#SnippetRemoteConnection)]

이제 원격 컴퓨터의 앱 서비스에 대한 연결이 열려 있습니다.

## <a name="exchange-service-specific-messages-over-the-remote-connection"></a>원격 연결을 통해 서비스 관련 메시지 교환

여기에서 [**ValueSet**](https://msdn.microsoft.com/library/windows/apps/windows.foundation.collections.valueset) 개체의 형태로 서비스와 메시지를 주고받을 수 있습니다. 자세한 내용은 [앱 서비스 만들기 및 사용](how-to-create-and-consume-an-app-service.md)을 참조하세요. 난수 생성기 서비스는 `"minvalue"` 및 `"maxvalue"` 키를 가진 두 정수를 입력으로 사용하고 해당 범위 내의 정수를 임의로 선택하여 호출 프로세스에 `"Result"` 키로 반환합니다.

[!code-cs[Main](./code/RemoteAppService/MainPage.xaml.cs#SnippetSendMessage)]

이제 대상 호스트 디바이스의 앱 서비스에 연결되었으므로 해당 디바이스에서 작업을 실행하고 응답으로 클라이언트 디바이스에 데이터가 수신됩니다.

## <a name="related-topics"></a>관련 항목

[연결된 앱 및 디바이스(프로젝트 로마) 개요](connected-apps-and-devices.md)  
[원격 앱 실행](launch-a-remote-app.md)  
[앱 서비스 만들기 및 사용](how-to-create-and-consume-an-app-service.md)  
[원격 시스템 API 참조](https://msdn.microsoft.com/library/windows/apps/Windows.System.RemoteSystems)  
[원격 시스템 샘플](https://github.com/Microsoft/Windows-universal-samples/tree/dev/Samples/RemoteSystems)
