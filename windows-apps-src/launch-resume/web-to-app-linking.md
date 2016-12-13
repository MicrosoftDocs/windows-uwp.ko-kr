---
author: TylerMSFT
title: "앱 URI 처리기로 웹과 앱 연결 지원"
description: "앱 URI 처리기를 사용하여 사용자의 앱 참여를 강화합니다."
keywords: "Windows 딥 링크 설정"
translationtype: Human Translation
ms.sourcegitcommit: 3e57ca2cf0e7c788f5a8be12ebaf3e6b05a4fe51
ms.openlocfilehash: e5c815ef0c776954e5b0d7f1cb9bd5e32e10356c

---

# <a name="support-web-to-app-linking-with-app-uri-handlers"></a>앱 URI 처리기로 웹과 앱 연결 지원

웹과 앱 연결을 지원하여 사용자의 앱 참여를 강화하는 방법을 알아봅니다. 웹과 앱 연결을 통해 웹 사이트와 앱을 연결할 수 있습니다. 사용자가 브라우저를 여는 대신 웹 사이트에 대한 http 또는 https 링크를 열면 앱이 시작됩니다. 앱이 설치되지 않은 경우 브라우저에서 웹 사이트를 여는 링크가 제공됩니다. 확인된 콘텐츠 소유자만 링크를 등록할 수 있기 때문에 사용자는 이 환경을 신뢰할 수 있습니다.

다음은 웹과 앱 연결을 사용하는 데 필요한 사항입니다.
- 매니페스트 파일에서 앱이 처리할 URI를 식별합니다.
- 앱 매니페스트 선언과 같은 호스트 루트에 앱 패키지 패밀리 이름을 가진 JSON 파일이 있어야 합니다.
- 앱의 활성화를 처리합니다.

## <a name="register-to-handle-http-and-https-links-in-the-app-manifest"></a>앱 매니페스트에서 http 및 https 링크를 처리하도록 등록

앱은 처리할 웹 사이트의 URI를 식별해야 합니다. 이렇게 하려면 앱의 매니페스트 파일 **Package.appxmanifest**에 **Windows.appUriHandler** 확장 등록을 추가합니다.

예를 들어 웹 사이트의 주소가 "msn.com"이면 앱의 매니페스트에 다음 항목을 만듭니다.

```xml
<Applications>
    ...
  <Extensions>
     <uap3:Extension Category="windows.appUriHandler">
      <uap3:AppUriHandler>
        <uap3:Host Name="msn.com" />
      </uap3:AppUriHandler>
    </uap3:Extension>
  </Extensions>
    ...
</Applications>
```

위의 선언은 지정된 호스트의 링크를 처리하도록 앱을 등록합니다. 웹 사이트의 주소가 여러 개인 경우(예: m.example.com, www.example.com 및 example.com) 각 주소에 대해 `<uap3:AppUriHandler>` 내에 `<uap3:Host Name=... />` 항목을 별도로 추가합니다.

## <a name="associate-your-app-and-website-with-a-json-file"></a>JSON 파일을 사용하여 앱과 웹 사이트 연결

앱에서만 웹 사이트의 콘텐츠를 열 수 있도록 하려면 웹 서버 루트 또는 도메인의 잘 알려진 디렉터리에 있는 JSON 파일에 앱의 패키지 패밀리 이름을 포함시킵니다. 이는 웹 사이트가 나열된 앱에서 사이트의 콘텐츠를 여는 것에 동의함을 의미합니다. 앱 매니페스트 디자이너의 패키지 섹션에서 패키지 패밀리 이름을 찾을 수 있습니다.

>[!Important]
> JSON 파일에는 .json 파일 접미사가 없어야 합니다.

**windows-app-web-link**라는 JSON 파일을(.json 파일 확장명 없이) 만들고 앱의 패키지 패밀리 이름을 제공합니다. 예를 들면 다음과 같습니다.

``` JSON
[{
  "packageFamilyName": "YourAppsPFN",
  "paths": [ "*" ],
  "excludePaths" : [ "/news/*", "/blog/*" ]
 }]
```

Windows는 웹 사이트에 https로 연결하여 웹 서버에서 해당 JSON 파일을 찾습니다.

### <a name="wildcards"></a>와일드카드

위의 JSON 파일 예제에서는 와일드카드를 사용하는 방법을 보여 줍니다. 와일드카드를 사용하면 더 적은 줄의 코드로 다양한 링크를 지원할 수 있습니다. 웹과 앱 연결은 JSON 파일에서 두 가지 유형의 와일드카드를 지원합니다.

| **와일드카드** | **설명**               |
|--------------|-------------------------------|
| *****       | 모든 하위 문자열을 나타냅니다.      |
| **?**        | 단일 문자를 나타냅니다. |

예를 들어 위의 예제에서처럼 `"excludePaths" : [ "/news/*", "/blog/*" ]`를 지정하면 앱은 `/news/` 및 `/blog/` 아래의 경로를 **제외**하고 웹 사이트의 주소(예: msn.com)로 시작하는 모든 경로를 지원합니다. 즉 **msn.com/weather.html**은 지원되지만 ****msn.com/news/topnews.html****은 지원되지 않습니다.


### <a name="multiple-apps"></a>여러 앱

웹 사이트에 연결할 두 개의 앱이 있는 경우 **windows-app-web-link** JSON 파일에 응용 프로그램 패키지 패밀리 이름이 모두 나열됩니다. 두 앱이 모두 지원될 수 있습니다. 둘 다 설치되어 있는 경우 사용자가 기본 링크를 선택할 수 있습니다. 나중에 기본 링크를 변경하려면 **설정 &gt; 웹 사이트용 앱**에서 변경할 수 있습니다. 또한 개발자는 언제든지 JSON 파일을 변경할 수 있으며 변경 사항은 빠르면 당일에, 늦어도 업데이트 후 8일 이내에 확인할 수 있습니다.

``` JSON
[{
  "packageFamilyName": "YourAppsPFN",
  "paths": [ "*" ],
  "excludePaths" : [ "/news/*", "/blog/*" ]
 },
 {
  "packageFamilyName": "Your2ndAppsPFN",
  "paths": [ "/example/*", "/links/*" ]
 }]
```

사용자에게 최상의 환경을 제공하려면 제외 경로를 사용하여 온라인 전용 콘텐츠가 JSON 파일의 지원되는 경로에서 제외되도록 합니다.

제외 경로를 먼저 확인하고 일치하는 경로가 있으면 지정된 앱 대신 브라우저에서 해당 페이지가 열립니다. 위의 예제에서 ‘/news/\*’(슬래시 없는 'news')에는 ‘newslocal/’, ‘newsinternational/’과 같이 ‘news\*’ 아래의 모든 경로가 포함되고 그 경로 아래의 모든 페이지가 ‘/news/\*’에 포함됩니다.

## <a name="handle-links-on-activation-to-link-to-content"></a>콘텐츠 연결 활성화에 대한 링크 처리

앱의 Visual Studio 솔루션에서 **App.xaml.cs**로 이동하고 **OnActivated()**에서 연결된 콘텐츠에 대한 처리를 추가합니다. 다음 예제에서는 앱에서 열리는 페이지가 URI 경로에 따라 달라집니다.

``` CS
protected override void OnActivated(IActivatedEventArgs e)
{
    Frame rootFrame = Window.Current.Content as Frame;
    if (rootFrame == null)
    {
        ...
    }

    // Check ActivationKind, Parse URI, and Navigate user to content
    Type deepLinkPageType = typeof(MainPage);
    if (e.Kind == ActivationKind.Protocol)
    {
        var protocolArgs = (ProtocolActivatedEventArgs)e;        
        switch (protocolArgs.Uri.AbsolutePath)
        {
            case "/":
                break;
            case "/index.html":
                break;
            case "/sports.html":
                deepLinkPageType = typeof(SportsPage);
                break;
            case "/technology.html":
                deepLinkPageType = typeof(TechnologyPage);
                break;
            case "/business.html":
                deepLinkPageType = typeof(BusinessPage);
                break;
            case "/science.html":
                deepLinkPageType = typeof(SciencePage);
                break;
        }
    }

    if (rootFrame.Content == null)
    {
        // Default navigation
        rootFrame.Navigate(deepLinkPageType, e);
    }

    // Ensure the current window is active
    Window.Current.Activate();
}
```

**중요** 위 예제에서와 같이 마지막 `if (rootFrame.Content == null)` 논리를 `rootFrame.Navigate(deepLinkPageType, e);`으로 대체해야 합니다.

## <a name="test-it-out-local-validation-tool"></a>테스트: 로컬 유효성 검사 도구

다음에서 사용할 수 있는 앱 호스트 등록 검증 도구를 실행하여 앱 및 웹 사이트의 구성을 테스트할 수 있습니다.

%windir%\\system32\\**AppHostRegistrationVerifier.exe**

다음 매개 변수로 이 도구를 실행하여 앱 및 웹 사이트의 구성을 테스트합니다.

**AppHostRegistrationVerifier.exe** *hostname packagefamilyname filepath*

-   호스트 이름: 웹 사이트(예: microsoft.com)
-   PFN(패키지 패밀리 이름): 앱의 PFN
-   파일 경로: 로컬 유효성 검사용 JSON 파일(예: C:\\SomeFolder\\windows-app-web-link)

## <a name="test-it-web-validation"></a>테스트: 웹 유효성 검사

응용 프로그램을 닫고 링크를 클릭하면 앱이 활성화되는지 확인합니다. 그런 다음 웹 사이트에서 지원되는 경로 중 하나의 주소를 복사합니다. 예를 들어 웹 사이트의 주소가 "msn.com"이고 지원 경로 중 하나가 “path1”이면 다음을 사용합니다. `http://msn.com/path1`

앱이 닫혀 있는지 확인합니다. **Windows 키+R**을 눌러 **실행** 대화 상자를 열고 창에 링크를 붙여넣습니다. 웹 브라우저 대신 앱이 실행되어야 합니다.

또한 [LaunchUriAsync](https://msdn.microsoft.com/en-us/library/windows/apps/hh701480.aspx) API를 사용하여 다른 앱에서 앱을 시작하여 테스트할 수 있습니다. 휴대폰에서도 이 API를 사용하여 테스트할 수 있습니다.

프로토콜 활성화 논리를 수행하려면 **OnActivated** 이벤트 처리기에 중단점을 설정합니다.

**참고:** Microsoft Edge 브라우저에서 링크를 클릭하면 앱이 시작되는 것이 아니라 웹 사이트로 이동합니다.

## <a name="appurihandlers-tips"></a>AppUriHandlers 팁:

- 앱에서 처리할 수 있는 링크만 지정해야 합니다.

- 지원하는 모든 호스트를 나열합니다.  www.example.com과 example.com은 서로 다른 호스트입니다.

- 설정에서 웹 사이트를 처리하려는 앱을 선택할 수 있습니다.

- JSON 파일은 https 서버에 업로드해야 합니다.

- 지원하려는 경로를 변경해야 하는 경우 앱을 다시 게시하지 않고 JSON 파일을 다시 게시할 수 있습니다. 사용자는 1-8일 내에 변경 사항을 확인할 수 있습니다.

- AppUriHandlers를 사용하여 테스트용으로 로드된 모든 앱에는 설치 시 호스트에 대해 유효성이 검사된 링크가 있습니다. 기능을 테스트하기 위해 JSON 파일을 업로드할 필요는 없습니다.

- 이 기능은 [LaunchUriAsync](https://msdn.microsoft.com/en-us/library/windows/apps/hh701480.aspx)를 사용하여 UWP 앱을 시작하거나 [ShellExecuteEx](https://msdn.microsoft.com/en-us/library/windows/desktop/bb762154(v=vs.85).aspx)를 사용하여 Windows 데스크톱 앱을 시작할 때마다 작동합니다. 등록된 앱 URI 처리기에 해당하는 URL의 경우 브라우저 대신 앱이 시작됩니다.

## <a name="see-also"></a>참고 항목

[windows.protocol 등록](https://msdn.microsoft.com/en-us/library/windows/apps/br211458.aspx)

[URI 활성화 처리](https://msdn.microsoft.com/en-us/windows/uwp/launch-resume/handle-uri-activation)

[연결 시작 샘플](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/AssociationLaunching)에서는 LaunchUriAsync() API를 사용하는 방법을 보여 줍니다.



<!--HONumber=Dec16_HO1-->


