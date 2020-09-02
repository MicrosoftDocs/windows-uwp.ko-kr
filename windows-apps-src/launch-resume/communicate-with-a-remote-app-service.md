---
title: 원격 앱 서비스와 통신
description: Project 로마를 사용 하 여 원격 장치에서 실행 되는 app service를 사용 하 여 메시지를 교환 합니다.
ms.assetid: a0261e7a-5706-4f9a-b79c-46a3c81b136f
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp, 연결 된 장치, 원격 시스템, 로마, 프로젝트 로마, 백그라운드 작업, app service
ms.localizationpriority: medium
ms.openlocfilehash: 779205a47b85cf9f9a0aec9db910b97995dc2cd8
ms.sourcegitcommit: c3ca68e87eb06971826087af59adb33e490ce7da
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 09/02/2020
ms.locfileid: "89363916"
---
# <a name="communicate-with-a-remote-app-service"></a>원격 앱 서비스와 통신

URI를 사용 하 여 원격 장치에서 앱을 시작 하는 것 외에도 원격 장치에서 *앱 서비스* 를 실행 하 고 통신할 수 있습니다. 모든 Windows 기반 장치를 클라이언트 또는 호스트 장치로 사용할 수 있습니다. 이렇게 하면 응용 프로그램을 포그라운드로 가져올 필요 없이 거의 무제한으로 연결 된 장치와 상호 작용할 수 있습니다.

## <a name="set-up-the-app-service-on-the-host-device"></a>호스트 장치에서 앱 서비스 설정
원격 장치에서 app service를 실행 하려면 해당 장치에 해당 app service의 공급자가 이미 설치 되어 있어야 합니다. 이 가이드에서는 [Windows 유니버설 샘플](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/AppServices)리포지토리에서 사용할 수 있는 [임의의 숫자 생성기 app Service 샘플](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/AppServices)의 CSharp 버전을 사용 합니다. 사용자 고유의 app service를 작성 하는 방법에 대 한 지침은 [app Service 만들기 및 사용](how-to-create-and-consume-an-app-service.md)을 참조 하세요.

이미 만들어진 app service를 사용 하 든, 직접 작성 하 든 관계 없이 서비스를 원격 시스템과 호환 하려면 몇 가지 사항을 편집 해야 합니다. Visual Studio에서 app service provider의 프로젝트 (샘플의 "App서비스 공급자")로 이동 하 여 _appxmanifest.xml_ 파일을 선택 합니다. 마우스 오른쪽 단추를 클릭 하 고 **코드 보기** 를 선택 하 여 파일의 전체 내용을 확인 합니다. 주 **응용 프로그램** 요소 내에 **확장** 요소를 만들거나 이미 있는 경우이를 찾습니다. 그런 다음 **확장** 을 만들어 프로젝트를 app service로 정의 하 고 해당 부모 프로젝트를 참조 합니다.

``` xml
...
<Extensions>
    <uap:Extension Category="windows.appService" EntryPoint="RandomNumberService.RandomNumberGeneratorTask">
        <uap3:AppService Name="com.microsoft.randomnumbergenerator"/>
    </uap:Extension>
</Extensions>
...
```

**AppService** 요소 옆에 **SupportsRemoteSystems** 특성을 추가 합니다.

``` xml
...
<uap3:AppService Name="com.microsoft.randomnumbergenerator" SupportsRemoteSystems="true"/>
...
```

이 **uap3** 네임 스페이스의 요소를 사용 하려면 매니페스트 파일의 맨 위에 네임 스페이스 정의를 추가 해야 합니다 (없는 경우).

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

그런 다음 app service 공급자 프로젝트를 빌드하고 호스트 장치에 배포 합니다.

## <a name="target-the-app-service-from-the-client-device"></a>클라이언트 장치에서 app service를 대상으로 합니다.
원격 앱 서비스를 호출할 장치에는 원격 시스템 기능을 사용 하는 앱이 필요 합니다. 호스트 장치에서 app service를 제공 하는 동일한 앱에 추가 하거나 (이 경우 두 장치에 동일한 앱을 설치) 완전히 다른 앱에서 구현 될 수 있습니다.

이 단원의 코드를 그대로 실행 하려면 다음 **using 문을 사용** 해야 합니다.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/launch-resume/RemoteAppService/cs/MainPage.xaml.cs" id="SnippetUsings":::


앱 서비스를 로컬로 호출 하는 것 처럼 먼저 [**AppServiceConnection**](/uwp/api/Windows.ApplicationModel.AppService.AppServiceConnection) 개체를 인스턴스화해야 합니다. 이 프로세스는 [앱 서비스 만들기 및 사용](how-to-create-and-consume-an-app-service.md)에서 자세히 설명합니다. 이 예제에서 대상으로 하는 app service는 난수 생성기 서비스입니다.

> [!NOTE]
> [RemoteSystem](/uwp/api/Windows.System.RemoteSystems.RemoteSystem) 개체는 다음 메서드를 호출 하는 코드 내의 일부 방법으로 이미 획득 한 것으로 가정 합니다. 이를 설정 하는 방법에 대 한 지침은 [원격 앱 시작](launch-a-remote-app.md) 을 참조 하세요.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/launch-resume/RemoteAppService/cs/MainPage.xaml.cs" id="SnippetAppService":::

다음으로, 의도 된 원격 장치에 대해 [**RemoteSystemConnectionRequest**](/uwp/api/Windows.System.RemoteSystems.RemoteSystemConnectionRequest) 개체가 생성 됩니다. 그런 다음 해당 장치에 대 한 **AppServiceConnection** 를 여는 데 사용 됩니다. 아래 예제에서 오류 처리 및 보고는 간결 하 게 하기 위해 크게 간소화 되었습니다.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/launch-resume/RemoteAppService/cs/MainPage.xaml.cs" id="SnippetRemoteConnection":::

이제 원격 컴퓨터의 앱 서비스에 대한 연결이 열려 있습니다.

## <a name="exchange-service-specific-messages-over-the-remote-connection"></a>원격 연결을 통해 서비스별 메시지 교환

여기에서 [**Valueset**](/uwp/api/windows.foundation.collections.valueset) 개체 형식으로 서비스와 메시지를 주고받을 수 있습니다. 자세한 내용은 [app service 만들기 및 사용](how-to-create-and-consume-an-app-service.md)을 참조 하세요. 난수 생성기 서비스는 키와 키를 사용 하 여 두 개의 정수를 사용 하 고 `"minvalue"` `"maxvalue"` 해당 범위 내에서 정수를 임의로 선택 하 고 키를 사용 하 여 호출 프로세스로 반환 합니다 `"Result"` .

:::code language="csharp" source="~/../snippets-windows/windows-uwp/launch-resume/RemoteAppService/cs/MainPage.xaml.cs" id="SnippetSendMessage":::

이제 대상 호스트 장치에서 app service에 연결 하 고 해당 장치에서 작업을 실행 한 후 클라이언트 장치에 응답 하 여 데이터를 수신 했습니다.

## <a name="related-topics"></a>관련 항목

[연결 된 앱 및 장치 (Project 로마) 개요](connected-apps-and-devices.md)  
[원격 앱 시작](launch-a-remote-app.md)  
[앱 서비스 만들기 및 사용](how-to-create-and-consume-an-app-service.md)  
[원격 시스템 API 참조](/uwp/api/Windows.System.RemoteSystems)  
[원격 시스템 샘플](https://github.com/Microsoft/Windows-universal-samples/tree/dev/Samples/RemoteSystems)
