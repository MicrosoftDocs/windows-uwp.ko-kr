---
title: 웹 인증 브로커
description: '이 문서에서는 UWP(유니버설 Windows 플랫폼) 앱을 OpenID 또는 OAuth 인증 프로토콜을 사용하는 온라인 ID 공급자(예: Facebook, Twitter, Flickr, Instagram 등)에 연결하는 방법을 설명합니다.'
ms.assetid: 05F06961-1768-44A7-B185-BCDB74488F85
author: PatrickFarley
ms.author: pafarley
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp, 보안
ms.localizationpriority: medium
ms.openlocfilehash: d354f0babec3ec2346c6e76fcae8666f40f3f6be
ms.sourcegitcommit: 933e71a31989f8063b020746fdd16e9da94a44c4
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/11/2018
ms.locfileid: "4533897"
---
# <a name="web-authentication-broker"></a>웹 인증 브로커




이 문서에서는 UWP(유니버설 Windows 플랫폼) 앱을 OpenID 또는 OAuth 인증 프로토콜을 사용하는 온라인 ID 공급자(예: Facebook, Twitter, Flickr, Instagram 등)에 연결하는 방법을 설명합니다. [**AuthenticateAsync**](https://msdn.microsoft.com/library/windows/apps/br212066) 메서드는 온라인 ID 공급자로 요청을 보내고 앱이 액세스할 수 있는 공급자 리소스에 대해 설명하는 액세스 토큰을 다시 가져옵니다.

>[!NOTE]
>완전한 코드 샘플을 사용하려면 [GitHub의 WebAuthenticationBroker 리포지토리](http://go.microsoft.com/fwlink/p/?LinkId=620622)를 복제하세요.

 

## <a name="register-your-app-with-your-online-provider"></a>온라인 공급자와 앱 등록


연결하려는 온라인 ID 공급자에 앱을 등록해야 합니다. ID 공급자에서 앱을 등록하는 방법을 확인할 수 있습니다. 등록하면 온라인 공급자는 일반적으로 앱에 대한 ID 또는 비밀 키를 제공합니다.

## <a name="build-the-authentication-request-uri"></a>인증 요청 URI 작성


요청 URI는 앱 ID 또는 암호, 인증 완료 후 사용자에게 전송되는 리디렉션 URI 및 예상 응답 유형과 같은 기타 필수 정보가 추가된 인증 요청을 온라인 공급자에 보내는 주소로 구성됩니다. 필요한 매개 변수는 공급자에서 확인할 수 있습니다.

요청 URI는 [**AuthenticateAsync**](https://msdn.microsoft.com/library/windows/apps/br212066) 메서드의 *requestUri* 매개 변수로 전송됩니다. `https://`로 시작하는 보안 주소여야 합니다.

다음 예에서는 요청 URI를 작성하는 방법을 보여 줍니다.

```cs
string startURL = "https://<providerendpoint>?client_id=<clientid>&scope=<scopes>&response_type=token";
string endURL = "http://<appendpoint>";

System.Uri startURI = new System.Uri(startURL);
System.Uri endURI = new System.Uri(endURL);
```

## <a name="connect-to-the-online-provider"></a>온라인 공급자에 연결


[**AuthenticateAsync**](https://msdn.microsoft.com/library/windows/apps/br212066) 메서드를 호출하여 온라인 ID 공급자에 연결하고 액세스 토큰을 가져옵니다. 메서드는 이전 단계에서 구성된 URI*requestUri* 매개 변수로 사용하고 사용자를 리디렉션할 URI를 *callbackUri* 매개 변수로 사용합니다.

```cs
string result;

try
{
    var webAuthenticationResult = 
        await Windows.Security.Authentication.Web.WebAuthenticationBroker.AuthenticateAsync( 
        Windows.Security.Authentication.Web.WebAuthenticationOptions.None, 
        startURI, 
        endURI);

    switch (webAuthenticationResult.ResponseStatus)
    {
        case Windows.Security.Authentication.Web.WebAuthenticationStatus.Success:
            // Successful authentication. 
            result = webAuthenticationResult.ResponseData.ToString(); 
            break;
        case Windows.Security.Authentication.Web.WebAuthenticationStatus.ErrorHttp:
            // HTTP error. 
            result = webAuthenticationResult.ResponseErrorDetail.ToString(); 
            break;
        default:
            // Other error.
            result = webAuthenticationResult.ResponseData.ToString(); 
            break;
    } 
}
catch (Exception ex)
{
    // Authentication failed. Handle parameter, SSL/TLS, and Network Unavailable errors here. 
    result = ex.Message;
}
```

>[!WARNING]
>[**AuthenticateAsync**](https://msdn.microsoft.com/library/windows/apps/br212066) 외에 [**Windows.Security.Authentication.Web**](https://msdn.microsoft.com/library/windows/apps/br227044) 네임스페이스는 [**AuthenticateAndContinue**](https://msdn.microsoft.com/library/windows/apps/dn632425) 메서드를 포함합니다. 이 메서드를 호출하지 마세요. 이 메서드는 Windows Phone 8.1만을 대상으로 하는 앱용으로 디자인되었으며 Windows 10부터 사용되지 않습니다.

## <a name="connecting-with-single-sign-on-sso"></a>SSO(Single Sign-On)로 연결


기본적으로 웹 인증 브로커는 쿠키가 유지되는 것을 허용하지 않습니다. 따라서 앱 사용자가 계속 로그인한 상태로 유지하고 있음을 표시(예: 공급자의 로그인 대화 상자에서 확인란 선택)해도 해당 공급자에 대한 리소스에 액세스할 때마다 로그인해야 합니다. SSO로 로그인하려면 온라인 ID 공급자는 웹 인증 브로커에 SSO를 사용하도록 설정해야 하며 앱은 *callbackUri* 매개 변수를 사용하지 않는 [**AuthenticateAsync**](https://msdn.microsoft.com/library/windows/apps/br212068)의 오버로드를 호출해야 합니다. 이렇게 하면 웹 인증 브로커가 영구적 쿠키를 저장할 수 있기 때문에 향후 같은 앱에서 인증을 호출할 때 사용자가 반복적으로 로그인을 할 필요가 없습니다(액세스 토큰이 만료될 때까지 사용자가 효과적으로 "로그인").

SSO를 지원하려면 온라인 공급자가 `ms-app://<appSID>` 형식의 리디렉션 URI 등록할 수 있도록 허용해야 합니다. 여기서 `<appSID>`는 앱의 SID입니다. 앱의 SID는 앱 개발자 페이지에서, 또는 [**GetCurrentApplicationCallbackUri**](https://msdn.microsoft.com/library/windows/apps/br212069) 메서드를 호출하여 확인할 수 있습니다.

```cs
string result;

try
{
    var webAuthenticationResult = 
        await Windows.Security.Authentication.Web.WebAuthenticationBroker.AuthenticateAsync( 
        Windows.Security.Authentication.Web.WebAuthenticationOptions.None, 
        startURI);

    switch (webAuthenticationResult.ResponseStatus)
    {
        case Windows.Security.Authentication.Web.WebAuthenticationStatus.Success:
            // Successful authentication. 
            result = webAuthenticationResult.ResponseData.ToString(); 
            break;
        case Windows.Security.Authentication.Web.WebAuthenticationStatus.ErrorHttp:
            // HTTP error. 
            result = webAuthenticationResult.ResponseErrorDetail.ToString(); 
            break;
        default:
            // Other error.
            result = webAuthenticationResult.ResponseData.ToString(); 
            break;
    } 
}
catch (Exception ex)
{
    // Authentication failed. Handle parameter, SSL/TLS, and Network Unavailable errors here. 
    result = ex.Message;
}
```

## <a name="debugging"></a>디버깅


작업 로그 검토, Fiddler를 사용하여 웹 요청 및 응답 검토 등 웹 인증 브로커 API 문제를 해결하는 여러 가지 방법이 있습니다.

### <a name="operational-logs"></a>작업 로그

대체로 작업 로그를 사용하여 작동하지 않는 항목을 확인할 수 있습니다. 웹 사이트 개발자가 웹 인증 브로커에 의한 웹 페이지 처리 방법을 이해할 수 있도록 도와주는 전용 이벤트 로그 채널인 Microsoft-Windows-WebAuth\\Operational이 있습니다. 이 채널을 사용하려면 eventvwr.exe를 시작하고 Application and Services\\Microsoft\\Windows\\WebAuth 아래의 작업 로그를 사용하도록 설정합니다. 또한 웹 인증 브로커는 웹 서버에서 자신을 식별하는 고유 문자열을 사용자 에이전트 문자열 뒤에 추가합니다. 해당 문자열은 "MSAuthHost/1.0"입니다. 버전 번호는 나중에 변경될 수 있으므로 사용자 코드에 이 버전 번호를 사용하면 안 됩니다. 전체 디버깅 단계가 뒤에 나오는 전체 사용자 에이전트 문자열의 예는 다음과 같습니다.

`User-Agent: Mozilla/5.0 (compatible; MSIE 10.0; Windows NT 6.2; Win64; x64; Trident/6.0; MSAuthHost/1.0)`

1.  작업 로그 사용.
2.  Contoso 소셜 앱 실행. ![webauth 작업 로그를 표시하는 이벤트 뷰어](images/wab-event-viewer-1.png)
3.  생성된 로그 항목을 사용하여 웹 인증 브로커의 동작을 자세히 파악할 수 있습니다. 이 경우 다음과 같은 항목이 포함될 수 있습니다.
    -   탐색 시작: AuthHost가 시작된 시간을 기록하고 시작 및 종료 URL에 대한 정보를 포함합니다.
    -   ![탐색 시작의 세부 정보를 보여 줍니다.](images/wab-event-viewer-2.png)
    -   탐색 완료: 웹 페이지 로드 완료를 기록합니다.
    -   메타 태그: 메타 태그가 발견된 시간을 기록하며 세부 정보를 포함합니다.
    -   탐색 종료: 사용자가 종료한 탐색입니다.
    -   탐색 오류: AuthHost가 URL에서 탐색 오류를 발견했으며 HttpStatusCode를 포함합니다.
    -   탐색 종료: 종료 URL이 발견되었습니다.

### <a name="fiddler"></a>Fiddler

앱에서 Fiddler 웹 디버거를 사용할 수 있습니다.

1.  AuthHost을 자체 앱 컨테이너에서 실행 되므로 개인 네트워크 기능을 제공 하도록 설정 해야 레지스트리 키: Windows 레지스트리 편집기 버전 5.00

    **HKEY\_LOCAL\_MACHINE**\\**SOFTWARE**\\**Microsoft**\\**Windows NT**\\**CurrentVersion**\\**Image File Execution Options**\\**authhost.exe**\\**EnablePrivateNetwork** = 00000001

    이 레지스트리 키가 없는 경우 관리자 권한으로 명령 프롬프트에서 만들 수 있습니다.

    ```cmd 
    REG ADD "HKLM\Software\Microsoft\Windows NT\CurrentVersion\Image File Execution Options\authhost.exe" /v EnablePrivateNetwork /t REG_DWORD /d 1 /f
    ```

2.  아웃바운드 트래픽을 생성하는 AuthHost에 대한 규칙을 추가합니다.
    ```syntax
    CheckNetIsolation.exe LoopbackExempt -a -n=microsoft.windows.authhost.a.p_8wekyb3d8bbwe
    CheckNetIsolation.exe LoopbackExempt -a -n=microsoft.windows.authhost.sso.p_8wekyb3d8bbwe
    CheckNetIsolation.exe LoopbackExempt -a -n=microsoft.windows.authhost.sso.c_8wekyb3d8bbwe
    D:\Windows\System32>CheckNetIsolation.exe LoopbackExempt -s
    List Loopback Exempted AppContainers
    [1] -----------------------------------------------------------------
        Name: microsoft.windows.authhost.sso.c_8wekyb3d8bbwe
        SID:  S-1-15-2-1973105767-3975693666-32999980-3747492175-1074076486-3102532000-500629349
    [2] -----------------------------------------------------------------
        Name: microsoft.windows.authhost.sso.p_8wekyb3d8bbwe
        SID:  S-1-15-2-166260-4150837609-3669066492-3071230600-3743290616-3683681078-2492089544
    [3] -----------------------------------------------------------------
        Name: microsoft.windows.authhost.a.p_8wekyb3d8bbwe
        SID:  S-1-15-2-3506084497-1208594716-3384433646-2514033508-1838198150-1980605558-3480344935
    ```

3.  Fiddler로 들어오는 트래픽에 대한 방화벽 규칙을 추가합니다.