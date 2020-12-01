---
title: 앱 URI 처리기를 사용 하 여 웹 사이트에 앱 사용
description: Websites 용 앱 기능을 지원 하 여 사용자의 앱 참여를 유도 합니다.
keywords: Windows 심층 연결
ms.date: 08/25/2017
ms.topic: article
ms.assetid: 260cf387-88be-4a3d-93bc-7e4560f90abc
ms.localizationpriority: medium
ms.openlocfilehash: 493a9936fa5d97ef2ac8d8d67bec15ce2f4df3c2
ms.sourcegitcommit: 25063560ff0a37fb404bc50e3b6e66759ee1051d
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 12/01/2020
ms.locfileid: "96420372"
---
# <a name="enable-apps-for-websites-using-app-uri-handlers"></a>앱 URI 처리기를 사용 하 여 웹 사이트에 앱 사용

웹 사이트용 앱은 웹 사이트에 앱을 연결 하 여 누군가가 웹 사이트에 대 한 링크를 열 때 브라우저를 여는 대신 응용 프로그램을 시작 합니다. 앱이 설치 되지 않은 경우 웹 사이트는 평소와 같이 브라우저에서 열립니다. 확인 된 콘텐츠 소유자만 링크를 등록할 수 있으므로 사용자는이 환경을 신뢰할 수 있습니다. 사용자는 웹 사이트용 앱 > 앱 > 설정으로 이동 하 여 등록 된 모든 웹 앱 링크를 확인할 수 있습니다.

웹-앱 링크를 사용 하도록 설정 하려면 다음을 수행 해야 합니다.
- 앱이 매니페스트 파일에서 처리할 Uri를 식별 합니다.
- 앱과 웹 사이트 간의 연결을 정의 하는 JSON 파일입니다. 응용 프로그램 패키지 제품군 이름을 응용 프로그램 매니페스트 선언과 동일한 호스트 루트에 사용 합니다.
- 앱에서 활성화를 처리 합니다.

> [!Note]
> Windows 10 크리에이터 업데이트부터 Microsoft Edge에서 지원 되는 링크를 클릭 하면 해당 앱이 시작 됩니다. 다른 브라우저 (예: Internet Explorer 등)에서 지원 되는 링크를 클릭 하면 검색 환경을 유지할 수 있습니다.

## <a name="register-to-handle-http-and-https-links-in-the-app-manifest"></a>앱 매니페스트에서 http 및 https 링크를 처리 하려면 등록 합니다.

앱은 처리할 웹 사이트의 Uri를 식별 해야 합니다. 이렇게 하려면 응용 프로그램의 매니페스트 파일 appxmanifest.xml에 **Windows. appUriHandler** 확장 등록을 추가 합니다 **.**

예를 들어 웹 사이트의 주소가 "msn.com" 인 경우 응용 프로그램의 매니페스트에 다음과 같이 입력 합니다.

```xml
<Applications>
  <Application ... >
      ...
      <Extensions>
         <uap3:Extension Category="windows.appUriHandler">
          <uap3:AppUriHandler>
            <uap3:Host Name="msn.com" />
          </uap3:AppUriHandler>
        </uap3:Extension>
      </Extensions>
  </Application>
</Applications>
```

위의 선언은 지정 된 호스트의 링크를 처리 하도록 앱을 등록 합니다. 웹 사이트에 주소가 여러 개 있는 경우 (예: m.example.com, www \. example.com 및 example.com) `<uap3:Host Name=... />` `<uap3:AppUriHandler>` 각 주소에 대해 내부에 별도의 항목을 추가 합니다.

## <a name="associate-your-app-and-website-with-a-json-file"></a>앱 및 웹 사이트를 JSON 파일과 연결

앱만 웹 사이트의 콘텐츠를 열 수 있도록 하려면 웹 서버 루트 또는 도메인의 잘 알려진 디렉터리에 있는 JSON 파일에 앱의 패키지 패밀리 이름을 포함 합니다. 이는 웹 사이트에서 사이트의 콘텐츠를 열기 위해 나열 된 앱에 대 한 동의를 제공 한다는 것을 의미 합니다. 패키지 제품군 이름은 앱 매니페스트 디자이너의 패키지 섹션에서 찾을 수 있습니다.

>[!Important]
> JSON 파일에는. json 파일 접미사가 없어야 합니다.

**Windows-app-v-link** 라는 json 파일 (. json 파일 확장명 제외)을 만들고 앱의 패키지 패밀리 이름을 제공 합니다. 예를 들면 다음과 같습니다.

``` JSON
[{
  "packageFamilyName" : "Your app's package family name, e.g MyApp_9jmtgj1pbbz6e",
  "paths" : [ "*" ],
  "excludePaths" : [ "/news/*", "/blog/*" ]
 }]
```

Windows에서 웹 사이트에 대 한 https 연결을 설정 하면 웹 서버에서 해당 하는 JSON 파일을 찾습니다.

### <a name="wildcards"></a>와일드카드

위의 JSON 파일 예제에서는 와일드 카드를 사용 하는 방법을 보여 줍니다. 와일드 카드를 사용 하면 코드 줄 수가 작은 다양 한 링크를 지원할 수 있습니다. 웹 앱 연결은 JSON 파일에서 두 가지 유형의 와일드 카드를 지원 합니다.

| **와일드카드** | **설명**               |
|--------------|-------------------------------|
| **\** _       | 부분 문자열을 나타냅니다.      |
| _ *?**        | 단일 문자를 나타냅니다. |

예를 들어 `"excludePaths" : [ "/news/*", "/blog/*" ]` 위의 예제에서 응용 프로그램은 및의 경우를 **제외** 하 고 웹 사이트의 주소 (예: msn.com)로 시작 하는 모든 경로를 지원 합니다 `/news/` `/blog/` . **msn.com/weather.html** 은 지원 되지만 **msn.com/news/topnews.html** 은 지원 되지 않습니다.

### <a name="multiple-apps"></a>여러 앱

웹 사이트에 연결 하려는 앱이 두 개 있는 경우 **windows-앱-웹 링크** JSON 파일에 응용 프로그램 패키지 제품군 이름을 모두 나열 합니다. 두 앱 모두 지원 될 수 있습니다. 둘 다 설치 된 경우 기본 링크를 선택 하 여 사용자에 게 표시 됩니다. 나중에 기본 링크를 변경 하려는 경우에는 **웹 사이트에 대 한 설정 > 앱** 에서 변경할 수 있습니다. 개발자는 언제 든 지 JSON 파일을 변경 하 고 업데이트 후 8 일 이상 지난 후에 변경 내용을 볼 수도 있습니다.

``` JSON
[{
  "packageFamilyName": "Your apps's package family name, e.g MyApp_9jmtgj1pbbz6e",
  "paths": [ "*" ],
  "excludePaths" : [ "/news/*", "/blog/*" ]
 },
 {
  "packageFamilyName": "Your second app's package family name, for example, MyApp2_8jmtgj2pbbz6e",
  "paths": [ "/example/*", "/links/*" ]
 }]
```

사용자에 게 최상의 환경을 제공 하려면 제외 경로를 사용 하 여 온라인 전용 콘텐츠가 JSON 파일의 지원 되는 경로에서 제외 되는지 확인 합니다.

제외 경로를 먼저 확인 하 고 일치 하는 항목이 있으면 지정 된 앱이 아닌 브라우저에서 해당 페이지가 열립니다. 위의 예제에서 '/news/'는 \* 해당 경로 아래에 있는 모든 페이지를 포함 하는 반면 '/뉴스 ' (슬래시 없음)에는 ' \* \* newslocal/', ' newsinternational/' 등과 같은 ' news ' 아래의 경로가 포함 됩니다.

## <a name="handle-links-on-activation-to-link-to-content"></a>콘텐츠 링크에 대 한 활성화 링크를 처리 합니다.

앱의 Visual Studio 솔루션 및 **Onactivated 됨 ()** 에서 **App.xaml.cs** 으로 이동 하 여 연결 된 콘텐츠에 대 한 처리를 추가 합니다. 다음 예제에서는 앱에서 열리는 페이지가 URI 경로에 따라 달라 집니다.

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

**중요** `if (rootFrame.Content == null)` 위의 예제와 같이 최종 논리를로 바꾸어야 합니다 `rootFrame.Navigate(deepLinkPageType, e);` .

## <a name="test-it-out-local-validation-tool"></a>테스트 테스트: 로컬 유효성 검사 도구

에서 사용할 수 있는 앱 호스트 등록 검증 도구를 실행 하 여 앱 및 웹 사이트의 구성을 테스트할 수 있습니다.

% windir% \\ system32 \\ **AppHostRegistrationVerifier.exe**

다음 매개 변수를 사용 하 여이 도구를 실행 하 여 앱 및 웹 사이트의 구성을 테스트 합니다.

**AppHostRegistrationVerifier.exe** *hostname packagefamilyname filepath*

-   호스트 이름: 웹 사이트 (예: microsoft.com)
-   PFN (패키지 패밀리 이름): 앱의 PFN
-   파일 경로: 로컬 유효성 검사를 위한 JSON 파일 (예: C: \\ SomeFolder \\ )

도구가 아무것도 반환 하지 않는 경우에는 업로드할 때 해당 파일에 대해 유효성 검사가 수행 됩니다. 오류 코드가 있으면 작동 하지 않습니다.

다음 레지스트리 키를 사용 하도록 설정 하 여 로컬 유효성 검사의 일부로 테스트용으로 로드 된 앱에 대 한 경로 일치를 강제 적용할 수 있습니다.

`HKCU\Software\Classes\LocalSettings\Software\Microsoft\Windows\CurrentVersion\
AppModel\SystemAppData\YourApp\AppUriHandlers`

Keyname: `ForceValidation` 값: `1`

## <a name="test-it-web-validation"></a>테스트: 웹 유효성 검사

응용 프로그램을 닫고 링크를 클릭할 때 앱이 활성화 되었는지 확인 합니다. 그런 다음 웹 사이트에서 지원 되는 경로 중 하나의 주소를 복사 합니다. 예를 들어 웹 사이트의 주소가 "msn.com"이 고 지원 경로 중 하나가 "path1" 인 경우 다음을 사용 합니다. `http://msn.com/path1`

앱이 닫혀 있는지 확인합니다. **Windows 키 + R** 을 눌러 **실행** 대화 상자를 열고 창에 링크를 붙여 넣습니다. 웹 브라우저 대신 앱이 시작 됩니다.

또한 [LaunchUriAsync](/uwp/api/windows.system.launcher.launchuriasync) API를 사용 하 여 다른 앱에서 앱을 시작 하 여 테스트할 수 있습니다. 이 API를 사용 하 여 휴대폰 에서도 테스트할 수 있습니다.

프로토콜 활성화 논리를 따르려면 **Onactivated** 된 이벤트 처리기에서 중단점을 설정 합니다.

## <a name="appurihandlers-tips"></a>AppUriHandlers 팁:

- 앱에서 처리할 수 있는 링크만 지정 해야 합니다.
- 지원할 호스트를 모두 나열 합니다.  Www \. example.com와 example.com는 서로 다른 호스트입니다.
- 사용자는 설정에서 웹 사이트를 처리 하는 데 선호 하는 앱을 선택할 수 있습니다.
- JSON 파일을 https 서버에 업로드 해야 합니다.
- 지원 하려는 경로를 변경 해야 하는 경우 앱을 다시 게시 하지 않고 JSON 파일을 다시 게시할 수 있습니다. 사용자에 게 1-8 일의 변경 내용이 표시 됩니다.
- AppUriHandlers 있는 모든 테스트용으로 로드 앱은 설치 시 호스트의 유효성을 검사 한 링크를 갖게 됩니다. 기능을 테스트 하기 위해 JSON 파일을 업로드 하지 않아도 됩니다.
- 이 기능은 앱이  [LaunchUriAsync](/uwp/api/windows.system.launcher.launchuriasync) 으로 시작 된 UWP 앱 또는  [shellexecuteex](/windows/desktop/api/shellapi/nf-shellapi-shellexecuteexa)로 시작 된 Windows 데스크톱 앱 인 경우에만 작동 합니다. URL이 등록 된 앱 URI 처리기에 해당 하는 경우 브라우저 대신 앱이 시작 됩니다.

## <a name="see-also"></a>추가 정보

[웹 앱 예제 프로젝트](https://github.com/project-rome/AppUriHandlers/tree/master/NarwhalFacts) 
 [windows. 프로토콜 등록](/uwp/schemas/appxpackage/appxmanifestschema/element-protocol) 
 [URI 활성화 처리](./handle-uri-activation.md) 
 [연결 시작 샘플](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/AssociationLaunching) 에서는 LaunchUriAsync () API를 사용 하는 방법을 보여 줍니다.
