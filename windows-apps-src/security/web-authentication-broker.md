---
title: 웹 인증 브로커
description: '이 문서에서는 Openid connect 또는 OAuth와 같은 인증 프로토콜 (예: Facebook, Twitter, Flickr, Instagram 등)을 사용 하는 온라인 id 공급자에 UWP (유니버설 Windows 플랫폼) 앱을 연결 하는 방법을 설명 합니다.'
ms.assetid: 05F06961-1768-44A7-B185-BCDB74488F85
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp, 보안
ms.localizationpriority: medium
ms.openlocfilehash: 52dc4364689ac04910c5b42cfe2dbcfd3b895b21
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89172827"
---
# <a name="web-authentication-broker"></a>웹 인증 브로커




이 문서에서는 Openid connect 또는 OAuth와 같은 인증 프로토콜 (예: Facebook, Twitter, Flickr, Instagram 등)을 사용 하는 온라인 id 공급자에 UWP (유니버설 Windows 플랫폼) 앱을 연결 하는 방법을 설명 합니다. [**AuthenticateAsync**](/uwp/api/windows.security.authentication.web.webauthenticationbroker.authenticateasync) 메서드는 온라인 id 공급자에 게 요청을 보내고 앱이 액세스할 수 있는 공급자 리소스를 설명 하는 액세스 토큰을 다시 가져옵니다.

>[!NOTE]
>전체 작업 코드 샘플은 [GitHub에서 Webauthenticationbroker 리포지토리](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/WebAuthenticationBroker)를 복제 합니다.

 

## <a name="register-your-app-with-your-online-provider"></a>온라인 공급자에 앱 등록


연결 하려는 온라인 id 공급자를 사용 하 여 앱을 등록 해야 합니다. Id 공급자에서 앱을 등록 하는 방법을 확인할 수 있습니다. 등록 후 온라인 공급자는 일반적으로 앱에 대 한 Id 또는 비밀 키를 제공 합니다.

## <a name="build-the-authentication-request-uri"></a>인증 요청 URI 작성


요청 URI는 응용 프로그램 ID 또는 암호와 같은 다른 필수 정보 (예: 인증을 완료 한 후 사용자가 전송 된 리디렉션 URI 및 예상 되는 응답 형식)와 함께 인증 요청을 온라인 공급자에 게 보내는 주소로 구성 됩니다. 필요한 매개 변수는 공급자에서 확인할 수 있습니다.

요청 URI는 [**AuthenticateAsync**](/uwp/api/windows.security.authentication.web.webauthenticationbroker.authenticateasync) 메서드의 *requestUri* 매개 변수로 전송 됩니다. 보안 주소 여야 합니다 (로 시작 해야 함). `https://`

다음 예제에서는 요청 URI를 빌드하는 방법을 보여 줍니다.

```cs
string startURL = "https://<providerendpoint>?client_id=<clientid>&scope=<scopes>&response_type=token";
string endURL = "http://<appendpoint>";

System.Uri startURI = new System.Uri(startURL);
System.Uri endURI = new System.Uri(endURL);
```

## <a name="connect-to-the-online-provider"></a>온라인 공급자에 연결


[**AuthenticateAsync**](/uwp/api/windows.security.authentication.web.webauthenticationbroker.authenticateasync) 메서드를 호출 하 여 온라인 id 공급자에 연결 하 고 액세스 토큰을 가져옵니다. 메서드는 이전 단계에서 생성 된 URI를 *requestUri* 매개 변수로 사용 하 고 사용자를 *callbackuri* 매개 변수로 리디렉션할 uri를 사용 합니다.

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
>[**AuthenticateAsync**](/uwp/api/windows.security.authentication.web.webauthenticationbroker.authenticateasync) [**네임 스페이스에는**](/uwp/api/Windows.Security.Authentication.Web) [**AuthenticateAndContinue**](/uwp/api/Windows.Security.Authentication.Web.WebAuthenticationBroker#methods) 메서드가 포함 되어 있습니다. 이 메서드를 호출 하지 마십시오. Windows Phone 8.1만을 대상으로 하는 앱 용으로 설계 되었으며 Windows 10부터 사용 되지 않습니다.

## <a name="connecting-with-single-sign-on-sso"></a>Single Sign-On (SSO)를 사용 하 여 연결 합니다.


기본적으로 웹 인증 브로커에서는 쿠키를 유지할 수 없습니다. 따라서 응용 프로그램 사용자가 로그인 상태를 유지 하려는 경우 (예: 공급자의 로그인 대화 상자에서 확인란을 선택 하는 경우)에도 해당 공급자의 리소스에 액세스할 때마다 로그인 해야 합니다. SSO를 사용 하 여 로그인 하려면 온라인 id 공급자가 웹 인증 브로커에 대해 SSO를 사용 하도록 설정 해야 하 고, 앱에서 *Callbackuri* 매개 변수를 사용 하지 않는 [**AuthenticateAsync**](/uwp/api/windows.security.authentication.web.webauthenticationbroker.authenticateasync) 의 오버 로드를 호출 해야 합니다. 이렇게 하면 웹 인증 브로커에서 지속형 쿠키를 저장할 수 있으므로 동일한 앱에서 호출 하는 이후 인증에는 사용자가 반복적으로 로그인 할 필요가 없습니다 (액세스 토큰이 만료 될 때까지 사용자가 효과적으로 "로그인" 됨).

SSO를 지원 하려면 온라인 공급자가 형식으로 리디렉션 URI를 등록할 수 있도록 허용 해야 합니다 `ms-app://<appSID>` . 여기서 `<appSID>` 은 앱의 SID입니다. 앱에 대 한 앱 개발자 페이지에서 또는 [**Getcurrentapplicationcallbackuri**](/uwp/api/windows.security.authentication.web.webauthenticationbroker.getcurrentapplicationcallbackuri) 메서드를 호출 하 여 앱의 SID를 찾을 수 있습니다.

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


웹 인증 브로커 Api 문제를 해결 하는 방법에는 여러 가지가 있습니다. 예를 들어 Fiddler를 사용 하 여 작업 로그를 검토 하 고 웹 요청 및 응답을 검토

### <a name="operational-logs"></a>작업 로그

작업 로그를 사용 하 여 작동 하지 않는 항목을 확인할 수 있는 경우가 많습니다. 웹 \\ 사이트 개발자가 웹 인증 브로커에서 웹 페이지를 처리 하는 방법을 이해 하는 데 사용할 수 있는 전용 이벤트 로그 채널 Microsoft-Windows-WebAuth가 있습니다. 이 기능을 사용 하도록 설정 하려면 eventvwr.exe를 시작 하 고 응용 프로그램 및 서비스 \\ Microsoft Windows webauth에서 작업 로그를 사용 하도록 설정 \\ \\ 합니다. 또한 웹 인증 브로커는 웹 서버에서 자신을 식별하는 고유 문자열을 사용자 에이전트 문자열 뒤에 추가합니다. 문자열은 "MSAuthHost/1.0"입니다. 버전 번호는 나중에 변경 될 수 있으므로 코드의 해당 버전 번호에 종속 되지 않아야 합니다. 전체 사용자 에이전트 문자열의 예 다음에 전체 디버깅 단계가 수행 되는 경우는 다음과 같습니다.

`User-Agent: Mozilla/5.0 (compatible; MSIE 10.0; Windows NT 6.2; Win64; x64; Trident/6.0; MSAuthHost/1.0)`

1.  작업 로그를 사용 하도록 설정 합니다.
2.  Contoso 소셜 응용 프로그램을 실행 합니다. ![webauth 작업 로그를 표시 하는 이벤트 뷰어](images/wab-event-viewer-1.png)
3.  생성 된 로그 항목을 사용 하 여 웹 인증 브로커의 동작을 보다 자세히 파악할 수 있습니다. 이 경우 다음이 포함 될 수 있습니다.
    -   탐색 시작: AuthHost를 시작할 때 기록 하 고 시작 및 종료 Url에 대 한 정보를 포함 합니다.
    -   ![탐색 시작의 세부 정보를 보여 줍니다.](images/wab-event-viewer-2.png)
    -   탐색 완료: 웹 페이지 로드 완료를 로깅합니다.
    -   Meta Tag: 세부 정보를 포함 하 여 meta 태그를 발견 한 경우 기록 합니다.
    -   탐색 종료: 사용자가 탐색을 종료 했습니다.
    -   탐색 오류: AuthHost가 HttpStatusCode를 포함 하 여 URL에서 탐색 오류를 발견 했습니다.
    -   탐색 끝: 종료 URL이 있습니다.

### <a name="fiddler"></a>Fiddler

Fiddler 웹 디버거는 앱과 함께 사용할 수 있습니다.

1.  AuthHost는 자체 앱 컨테이너에서 실행 되므로 개인 네트워크 기능을 제공 하려면 레지스트리 키를 설정 해야 합니다. Windows 레지스트리 편집기 버전 5.00

    **HKEY \_ 로컬 \_ 컴퓨터** \\ **소프트웨어** \\ **Microsoft** \\ **Windows NT** \\ **CurrentVersion** \\ **Image 파일 실행 옵션** \\ **authhost.exe** \\ **EnablePrivateNetwork** = 00000001

    이 레지스트리 키가 없는 경우 관리자 권한으로 명령 프롬프트에서 만들 수 있습니다.

    ```cmd 
    REG ADD "HKLM\Software\Microsoft\Windows NT\CurrentVersion\Image File Execution Options\authhost.exe" /v EnablePrivateNetwork /t REG_DWORD /d 1 /f
    ```

2.  이는 아웃 바운드 트래픽을 생성 하는 것 이므로 AuthHost에 대 한 규칙을 추가 합니다.
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

3.  들어오는 트래픽에 대 한 방화벽 규칙을 Fiddler에 추가 합니다.