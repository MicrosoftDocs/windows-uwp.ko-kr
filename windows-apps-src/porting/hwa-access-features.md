---
author: seksenov
title: "호스트된 웹앱 - UWP(유니버설 Windows 플랫폼) 기능 및 런타임 API에 액세스"
description: "원격 JavaScript에서 Cortana 음성 명령, 라이브 타일, 보안용 ACUR, OpenID 및 OAuth를 비롯하여 UWP(유니버설 Windows 플랫폼) 기본 기능 및 Windows 10 런타임 API에 모두 액세스할 수 있습니다."
kw: Hosted Web Apps, Accessing Windows 10 features from remote JavaScript, Building a Win10 Web Application, Windows JavaScript Apps, Microsoft Web Apps, HTML5 app for PC, ACUR URI Rules for Windows App, Call Live Tiles with web app, Use Cortana with web app, Access Cortana from website, msapplication-cortanavcd
translationtype: Human Translation
ms.sourcegitcommit: 3de603aec1dd4d4e716acbbb3daa52a306dfa403
ms.openlocfilehash: fb74bfc40750941860dae0a8f811fde4a614e403

---

# UWP(유니버설 Windows 플랫폼) 기능에 액세스

웹 응용 프로그램은 UWP(유니버설 Windows 플랫폼)에 대한 모든 권한을 가질 수 있습니다. 즉, Windows 디바이스에서 기본 기능을 활성화하고, [Windows 보안의 이점을 활용하고](#keep-your-app-secure-setting-application-content-uri-rules-acurs), 서버에 호스트된 스크립트에서 직접 [Windows 런타임 API를 호출하고](#call-windows-runtime-apis), [Cortana 통합](#integrate-cortana-voice-commands)을 활용하고, [온라인 인증 공급자](#web-authentication-broker)를 사용할 수 있습니다. 호스트된 스크립트에서 호출할 로컬 코드를 포함하고 원격 페이지와 로컬 페이지 간 앱 탐색을 관리할 수 있으므로 [하이브리드 앱](#create-hybrid-apps-packaged-web-apps-vs-hosted-web-apps)도 지원됩니다.

## 앱의 보안 유지 - ACUR(응용 프로그램 콘텐츠 URI 규칙) 설정

URL 허용 목록이라고도 하는 ACUR을 통해 원격 HTML, CSS 및 JavaScript에서 유니버설 Windows API에 대한 원격 URL 직접 액세스를 제공할 수 있습니다. Windows OS 수준에서 적절한 정책 범위를 설정하여 웹 서버에 호스트된 코드에서 플랫폼 API를 직접 호출할 수 있도록 합니다. 호스트된 웹앱을 구성하는 URL 집합을 ACUR(응용 프로그램 콘텐츠 URI 규칙)에 배치할 때 앱 패키지 매니페스트에서 이러한 범위를 정의합니다. 규칙에는 앱의 시작 페이지 및 앱 페이지로 포함하려는 다른 모든 페이지가 포함되어야 합니다. 선택적으로 특정 URL을 제외할 수도 있습니다.

규칙에 URL 일치를 지정하는 방법은 다음과 같이 여러 가지입니다.

- 정확한 호스트 이름
- 해당 하위 도메인의 URI가 포함되거나 제외된 호스트 이름
- 정확한 URI
- 쿼리 속성을 포함할 수 있는 정확한 URI
- 부분 경로와 포함 규칙에 대한 특정 파일 확장명을 나타내기 위한 와일드카드
- 제외 규칙에 대한 상대 경로

사용자가 규칙에 포함되지 않은 URL로 이동하는 경우 Windows는 브라우저에서 대상 URL을 엽니다.

다음은 ACUR의 몇 가지 예입니다.

```HTML
<Application
Id="App"
StartPage="http://contoso.com/home">
<uap:ApplicationContentUriRules>
    <uap:Rule Type="include" Match="https://contoso.com/" WindowsRuntimeAccess="all" />
    <uap:Rule Type="include" Match="https://*.contoso.com/" WindowsRuntimeAccess="all" />
    <uap:Rule Type="exclude" Match="https://contoso.com/excludethispage.aspx" />
</uap:ApplicationContentUriRules>
```

## Windows 런타임 API 호출

URL이 앱의 범위(ACUR) 내에서 정의된 경우 “WindowsRuntimeAccess” 특성을 사용하여 JavaScript로 Windows 런타임 API를 호출할 수 있습니다. 적절한 액세스 권한으로 URL이 앱 호스트에 로드될 때 Windows 네임스페이스는 스크립트 엔진에 삽입되며 표시됩니다. 이렇게 하면 유니버설 Windows API를 앱의 스크립트에서 직접 호출할 수 있습니다. 개발자는 호출하려는 Windows API에 대한 기능을 검색하기만 하면 됩니다. 사용 가능한 경우 계속 플랫폼 기능을 사용할 수 있습니다.

이렇게 하려면 ACUR에서 `(WindowsRuntimeAccess="<<level>>")` 특성을 다음 값 중 하나로 지정해야 합니다.

- **all**: 원격 JavaScript 코드가 모든 UWP API 및 패키지된 로컬 구성 요소에 액세스할 수 있습니다.
- **allowForWeb**: 원격 JavaScript 코드가 패키지 코드의 사용자 지정에만 액세스할 수 있습니다. 사용자 지정 C++/C# 구성 요소에 로컬 액세스할 수 있습니다.
- **none**: 기본값입니다. 지정된 URL에 플랫폼 액세스 권한이 없습니다.

다음은 예제 규칙 종류입니다.

```HTML
<uap:ApplicationContentUriRules>
    <uap:Rule Type="include" Match="http://contoso.com/" WindowsRuntimeAccess="all"  />
</uap:ApplicationContentUriRules>
```

이 규칙은 http://contoso.com/에서 실행되는 스크립트에 패키지의 사용자 지정 패키지 구성 요소 및 Windows 런타임 네임스페이스에 대한 액세스 권한을 부여합니다. 알림 메시지에 대해서는 GitHub의 [Windows.UI.Notifications.js](https://gist.github.com/Gr8Gatsby/3d471150e5b317eb1813#file-windows-ui-notifications-js) 예제를 참조하세요.

다음은 라이브 타일을 구현하고 원격 JavaScript에서 업데이트하는 방법의 예입니다.

```Javascript
function updateTile(message, imgUrl, imgAlt) {
    // Namespace: Windows.UI.Notifications

    if (typeof Windows !== 'undefined'&&
            typeof Windows.UI !== 'undefined' &&
            typeof Windows.UI.Notifications !== 'undefined') {  
        var notifications = Windows.UI.Notifications,
        tile = notifications.TileTemplateType.tileSquare150x150PeekImageAndText01,
        tileContent = notifications.TileUpdateManager.getTemplateContent(tile),
        tileText = tileContent.getElementsByTagName('text'),
        tileImage = tileContent.getElementsByTagName('image');  
        tileText[0].appendChild(tileContent.createTextNode(message || 'Demo Message'));
        tileImage[0].setAttribute('src', imgUrl || 'https://unsplash.it/150/150/?random');
        tileImage[0].setAttribute('alt', imgAlt || 'Random demo image');    
        var tileNotification = new notifications.TileNotification(tileContent);
        var currentTime = new Date();
        tileNotification.expirationTime = new Date(currentTime.getTime() + 600 * 1000);
        notifications.TileUpdateManager.createTileUpdaterForApplication().update(tileNotification);
    } else {
        //Alternative behavior

    }
}
```

이 코드는 다음과 같이 표시되는 타일을 생성합니다.

![라이브 타일을 호출하는 Windows 10](images/hwa-to-uwp/hwa_livetile.png)

호출하기 전에 Windows 기능을 검색하는 서버 기능에 리소스를 유지함으로써 환경이나 기법이 무엇이든지 간에 가장 익숙한 것을 사용해 Windows 런타임 API를 호출합니다. 플랫폼 기능을 사용할 수 없으며 웹앱이 다른 호스트에서 실행되는 경우 브라우저에서 작동하는 표준 기본 환경을 사용자에게 제공할 수 있습니다.

## Cortana 음성 명령 통합

html 페이지에 VCD(음성 명령 정의) 파일을 지정하여 Cortana 통합을 활용할 수 있습니다. VCD 파일은 명령을 특정 구문에 매핑하는 xml 파일입니다. 예를 들어 사용자가 시작 단추를 탭하고 “Contoso Books, show best sellers”를 말해 Contoso Books 앱을 시작하고 “best sellers” 페이지로 이동할 수 있습니다.

VCD 파일의 위치를 나열하는 `<meta>` 요소 태그를 추가하면 Windows가 자동으로 음성 명령 정의 파일을 다운로드 및 등록합니다.

다음은 호스트된 웹앱의 html 페이지에서 태그를 사용하는 예입니다.

```HTML
<meta name="msapplication-cortanavcd" content="http:// contoso.com/vcd.xml"/>
```

Cortana 통합 및 VCD에 대한 자세한 내용은 Cortana 조작 및 VCD(음성 명령 정의) 요소 및 특성 v1.2를 참조하세요.

## 하이브리드 앱 만들기 - 패키지된 웹앱 및 호스트된 웹앱

UWP 앱을 만드는 옵션은 다음과 같습니다. Windows 스토어에서 다운로드하여 흔히 **패키지된 웹앱**이라고 하는 로컬 클라이언트에서 완전히 호스트되도록 앱을 설계할 수 있습니다. 그러면 호환 가능한 모든 플랫폼에서 오프라인으로 앱을 실행할 수 있습니다. 또는 앱이 일반적으로 **Hosted Web App**이라고 하는 원격 웹 서버에서 실행되는 완전히 호스트된 웹앱일 수 있습니다. 세 번째 옵션으로 앱은 로컬 클라이언트에서 일부가 호스트되고 다른 일부는 원격 웹 서버에서 실행될 수 있습니다. 이 세 번째 옵션은 **Hybrid app**이라고 하며 일반적으로 **WebView** 구성 요소를 사용하여 원격 콘텐츠를 로컬 콘텐츠와 같이 보이게 만듭니다. 하이브리드 앱에는 로컬 앱 클라이언트의 패키지로 실행되는 HTML5, CSS 및 Javascript 코드가 포함될 수 있으며 원격 콘텐츠를 조작하는 기능이 있습니다.

## 웹 인증 브로커

OpenID 또는 OAuth와 같은 인터넷 인증 및 권한 부여 프로토콜을 사용하는 온라인 ID 공급자가 있는 경우 웹 인증 브로커를 사용하여 사용자에 대한 로그인 흐름을 처리할 수 있습니다. 앱 html 페이지의 `<meta>` 태그에 시작 및 끝 URI를 지정합니다. Windows는 이러한 URI를 검색하고 웹 인증 브로커에 전달하여 로그인 흐름을 완료합니다. 시작 URI는 앱 ID, 인증 완료 후 사용자에게 전송되는 리디렉션 URI 및 예상 응답 유형과 같은 기타 필수 정보가 추가된 인증 요청을 온라인 공급자에 보내는 주소로 구성됩니다. 필요한 매개 변수는 공급자에서 확인할 수 있습니다. 다음은 `<meta>` 태그의 사용 예입니다.

```HTML
<meta name="ms-webauth-uris" content="https://<providerstartpoint>?client_id=<clientid>&response_type=token, https://<appendpoint>"/>
```

자세한 지침은 [온라인 공급자에 대한 웹 인증 브로커 고려 사항](https://msdn.microsoft.com/library/windows/apps/dn448956.aspx)을 참조하세요.

## 앱 접근 권한 값 선언

앱이 사용자 리소스(예: 사진 또는 음악) 또는 디바이스(예: 카메라 또는 마이크)에 프로그래밍 방식으로 액세스해야 하는 경우 적절한 접근 권한 값을 선언해야 합니다. 앱 접근 권한 값 선언 범주는 다음과 같이 세 가지입니다. 

- 대부분의 공통적인 앱 시나리오에 적용되는 [범용 접근 권한 값](https://msdn.microsoft.com/library/windows/apps/Mt270968.aspx#General-use_capabilities) 
- 앱이 주변 디바이스 및 내부 디바이스에 액세스할 수 있게 하는 [디바이스 접근 권한 값](https://msdn.microsoft.com/library/windows/apps/Mt270968.aspx#Device_capabilities) 
- 스토어에 제출하기 위한 특별 회사 계정이 있어야 사용할 수 있는 [특수 용도 접근 권한 값](https://msdn.microsoft.com/library/windows/apps/Mt270968.aspx#Special_and_restricted_capabilities) 

회사 계정에 대한 자세한 내용은 [계정 유형, 위치 및 수수료](https://msdn.microsoft.com/library/windows/apps/jj863494.aspx)를 참조하세요.

> [!NOTE]
> 고객은 Windows 스토어에서 앱 구입 시 앱에서 선언한 모든 접근 권한 값에 관한 알림을 받아야 합니다. 따라서 앱에 필요하지 않은 접근 권한 값을 사용하지 마세요.

앱의 [패키지 매니페스트](https://msdn.microsoft.com/library/windows/apps/br211474.aspx)에 접근 권한 값을 선언하여 액세스 권한을 요청합니다. Microsoft Visual Studio에서 [매니페스트 디자이너](https://msdn.microsoft.com/library/windows/apps/xaml/hh454036(v=vs.140).aspx#Configure)를 사용하여 일반 접근 권한 값을 선언하거나 수동으로 추가할 수 있습니다([패키지 매니페스트에 접근 권한 값을 지정하는 방법](https://msdn.microsoft.com/library/windows/apps/br211477.aspx) 항목 참조).

일부 접근 권한 값은 중요한 리소스에 대한 액세스 권한을 앱에 제공합니다. 이러한 리소스는 사용자의 개인 데이터에 액세스하거나 사용자의 비용이 들 수 있으므로 중요한 것으로 간주됩니다. 설정 앱에서 관리되는 개인 정보 설정을 통해 사용자는 중요한 리소스에 대한 액세스를 동적으로 제어할 수 있습니다. 따라서 앱에서는 중요한 리소스를 항상 사용 가능한 것으로 가정하지 않아야 합니다. 중요한 리소스에 액세스하는 방법에 대한 자세한 내용은 [개인정보 인식 앱에 대한 지침](https://msdn.microsoft.com/library/windows/apps/hh768223.aspx)을 참조하세요.

## manifoldjs 및 앱 매니페스트

웹 사이트를 UWP 앱으로 전환하는 가장 쉬운 방법은 **app manifest** 및 **manifoldjs**를 사용하는 것입니다. 앱 매니페스트는 앱에 대한 메타데이터를 포함하는 xml 파일입니다. 앱의 이름, 리소스 링크, 디스플레이 모드, URL 및 앱의 배포와 실행 방법을 설명하는 기타 데이터 등을 지정합니다. manifoldjs를 사용하면 웹앱을 지원하지 않는 시스템에서도 이 프로세스를 수행하기가 매우 쉬워집니다. 해당 작동 방식에 대한 자세한 내용은 [manifoldjs.com](http://www.manifoldjs.com/)으로 이동합니다. 이 [Windows 10 웹앱 프레젠테이션](http://channel9.msdn.com/Events/WebPlatformSummit/2015/Hosted-web-apps-and-web-platform-innovations?wt.mc_id=relatedsession)의 일부로 manifoldjs 데모도 볼 수 있습니다.

## 관련 항목
- [Windows 런타임 API: JavaScript 코드 샘플](http://rjs.azurewebsites.net/)
- [Codepen: Windows 런타임 API 호출에 사용할 샌드박스](http://codepen.io/seksenov/pen/wBbVyb/)
- [Cortana 조작](https://msdn.microsoft.com/library/windows/apps/dn974231.aspx)
- [VCD(음성 명령 정의) 요소 및 특성 v1.2](https://msdn.microsoft.com/library/windows/apps/dn954977.aspx)
- [온라인 공급자에 대한 웹 인증 브로커 고려 사항](https://msdn.microsoft.com/library/windows/apps/dn448956.aspx)
- [앱 접근 권한 값 선언](https://msdn.microsoft.com/ibrary/windows/apps/hh464936.aspx)


<!--HONumber=Aug16_HO3-->


