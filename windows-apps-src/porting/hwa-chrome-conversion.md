---
author: seksenov
title: "호스트된 웹앱 - 유니버설 Windows 플랫폼 앱으로 Chrome 앱 변환"
description: "Chrome 앱 또는 Chrome 확장을 Windows 스토어용 UWP(유니버설 Windows 플랫폼) 앱으로 변환합니다."
kw: Package Chrome Extension for Windows Store tutorial, Port Chrome Extension to Windows 10, How to convert Chrome App to Windows, How to add Chrome Extension to Windows Store, hwa-cli, Hosted Web Apps Command Line Interface CLI Tool, Install Chrome Extension on Windows 10 Device, convert .crx to .AppX
ms.author: wdg-dev-content
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: "Windows용 Chrome 확장, Windows용 Chrome 앱, hwa-cli, .crx를 .AppX로 변환"
ms.assetid: 04f37333-48ba-441b-875e-246fbc3e1a4d
ms.openlocfilehash: b2168242d5464dbf41f12c777aa5672753a4ae6e
ms.sourcegitcommit: 909d859a0f11981a8d1beac0da35f779786a6889
translationtype: HT
---
# <a name="convert-your-existing-chrome-app-to-a-uwp-app"></a>기존 Chrome 앱을 UWP 앱으로 변환

기존의 Chrome 호스트된 앱을 UWP(유니버설 Windows 플랫폼)에서 실행되는 앱으로 쉽게 변환할 수 있습니다. Chrome 앱을 변환하는 방법은 다음과 같이 두 가지가 있습니다.

- 옵션 #1: [ManifoldJS](http://manifoldjs.com/)는 이제 Chrome 매니페스트를 입력 형식으로 허용합니다. 

- 옵션 #2: 기존 `.zip` 또는 `.crx` 파일에서 `.appx` 패키지를 생성하는 [CLI 도구](https://github.com/MicrosoftEdge/hwa-cli)를 개발했습니다.

## <a name="convert-your-existing-chrome-app-using-the-command-line-interface"></a>명령줄 인터페이스를 사용하여 기존 Chrome 앱 변환

1. [NodeJS](https://nodejs.org/en/)와 패키지 관리자인 [npm](https://www.npmjs.com/)을 설치합니다. 


2. 명령 프롬프트 창을 열고 선택한 디렉터리로 이동합니다.


3. 명령줄에서 다음을 입력하여 HWA(호스트된 웹앱) CLI(명령줄 인터페이스)를 설치합니다. `npm i -g hwa-cli`

4. 명령줄에서 다음을 입력하여 Chrome 패키지(`.crx` 및 `.zip`은 지원되는 패키지 형식임)를 변환합니다. `hwa convert path/to/chrome/app.crx` 또는 `hwa convert path/to/chrome/app.zip`

    **`path/to/chrome/app`을 Chrome 앱의 경로 정보로 바꿉니다.*
    
5. 생성된 `.appx`는 Chrome 패키지와 동일한 폴더에 표시됩니다. 이제 Windows 스토어에 앱을 업로드할 준비가 되었습니다. 

## <a name="uploading-your-app-to-the-windows-store"></a>Windows 스토어에 앱 업로드

앱을 업로드하려면 [Windows 개발자 센터](https://developer.microsoft.com/windows)의 대시보드를 방문합니다. "[새 앱 만들기](https://developer.microsoft.com/dashboard/Application/New)"를 클릭하고 앱 이름을 예약합니다.
![Windows 개발자 센터 대시보드에서 이름 예약](images/hwa-to-uwp/reserve_a_name.png)


제출 섹션의 "패키지" 페이지로 이동하여 `AppX` 패키지를 업로드합니다.

Windows 스토어에 표시되는 메시지에 정보를 입력합니다.

    During the conversion process, you will be prompted for an Identity Name, Publisher Identity, and Publisher Display Name. To retrieve these values, visit the Dashboard in the [Windows Dev Center](https://developer.microsoft.com/windows).
    - Click on "[Create a new app](https://developer.microsoft.com/dashboard/Application/New)" and reserve your app name.
![Windows 개발자 센터 대시보드에서 이름 예약](images/hwa-to-uwp/reserve_a_name.png)
    - 그런 다음, "앱 관리" 섹션 왼쪽 아래 메뉴에서 "앱 ID"를 클릭합니다.
    ![Windows 개발자 센터 대시보드 앱 ID](images/hwa-to-uwp/app_identity.png)
    - 페이지에서 입력하도록 요청한 세 가지 값이 나열됩니다. 
        1. ID 이름: `Package/Identity/Name`
        2. 게시자 ID: `Package/Identity/Publisher`
        3. 게시자 표시 이름: `Package/Properties/PublisherDisplayName`


## <a name="guide-for-migrating-your-hosted-web-app"></a>호스트된 웹앱 마이그레이션 가이드

Windows 스토어용으로 웹앱을 패키징한 후 PC, 태블릿, 휴대폰, HoloLens, Surface Hub, Xbox 및 Raspberry Pi를 비롯한 모든 Windows 기반 디바이스에서 원활하게 작동하도록 사용자 지정합니다.

### <a name="application-content-uri-rules"></a>응용 프로그램 콘텐츠 URI 규칙

[ACUR(응용 프로그램 콘텐츠 URI 규칙)](./hwa-access-features.md) 또는 콘텐츠 URI는 앱 패키지 매니페스트의 URL 허용 목록을 통해 호스트된 웹앱의 범위를 정의합니다. 원격 콘텐츠를 주고받는 통신을 제어하려면 이 목록에 포함 및/또는 이 목록에서 제외할 URL을 정의해야 합니다. 사용자가 명시적으로 포함되지 않은 URL을 클릭하는 경우 Windows는 기본 브라우저에서 대상 경로를 엽니다. ACUR을 사용하면 [유니버설 Windows API](https://msdn.microsoft.com/library/windows/apps/br211377.aspx)에 대한 액세스 권한을 URL에 부여할 수도 있습니다.

최소한 앱의 시작 페이지를 포함해야 합니다. 변환 도구에서는 시작 페이지 및 해당 도메인에 따라 ACUR 집합을 자동으로 만듭니다. 그러나 서버 또는 클라이언트에 프로그래밍 방식의 리디렉션이 있는 경우 이러한 대상은 허용 목록에 추가해야 합니다.

*참고: ACUR은 페이지 탐색에만 적용됩니다. 이미지, JavaScript 라이브러리 및 기타 유사한 자산은 이러한 제한의 영향을 받지 않습니다.*

여러 앱이 로그인 흐름에 Facebook, Google 등 타사 사이트를 사용합니다. 변환 도구에서는 가장 인기 있는 사이트에 따라 ACUR 집합을 자동으로 만듭니다. 인증 방법이 해당 목록에 포함되지 않았으며 리디렉션 흐름인 경우 해당 경로를 ACUR로 추가해야 합니다. 또한 [웹 인증 브로커](./hwa-access-features.md)의 사용도 고려할 수 있습니다.

### <a name="flash"></a>Flash

플래시는 Windows 10 앱에서 허용되지 않습니다. 플래시를 사용할 수 없으면 앱 환경이 영향을 받지 않는지 확인해야 합니다.

광고의 경우 광고 공급자에 HTML5 옵션이 있는지 확인해야 합니다. [Bing Ads](https://bingads.microsoft.com/) 및 [Microsoft 광고 라이브러리](../monetize/display-ads-in-your-app.md)를 참조할 수 있습니다. 

[`<iframe>` embed 메서드](https://developers.google.com/youtube/iframe_api_reference)를 사용하는 한, YouTube 동영상은 이제 [기본적으로 HTML5 `<video>`,](http://youtube-eng.blogspot.com/2015/01/youtube-now-defaults-to-html5_27.html)로 설정되므로 계속 작동합니다. 앱에서 여전히 플래시 API를 사용하는 경우 앞에서 언급한 embed 스타일로 전환해야 합니다.

### <a name="image-assets"></a>이미지 자산

Chrome 웹 스토어에서는 이미 앱 패키지에 [128x128 앱 아이콘 이미지](https://developer.chrome.com/webstore/images)를 포함하도록 요구합니다. Windows 10 앱의 경우 최소한 44x44, 50x50, 150x150 및 600x350 앱 아이콘 이미지를 제공해야 합니다. 변환 도구에서는 128x128 이미지를 기반으로 하여 이러한 이미지를 자동으로 만듭니다. 더욱 세련되고 풍부한 앱 환경을 위해 고유한 이미지 파일을 직접 만드는 것이 좋습니다. [타일 및 아이콘 자산에 대한 지침](https://msdn.microsoft.com/library/windows/apps/mt412102.aspx)을 참조하세요.

### <a name="capabilities"></a>접근 권한 값

특정 API 및 리소스에 액세스하려면 앱 접근 권한 값을 패키지 매니페스트에서 [선언](https://msdn.microsoft.com/windows/uwp/packaging/app-capability-declarations)해야 합니다. 변환 도구에서는 세 가지 일반적인 디바이스 기능 즉, 위치, 마이크 및 웹캠을 자동으로 사용할 수 있습니다. 이전과 마찬가지로 시스템은 액세스 권한을 부여하기 전에 먼저 사용 권한을 확인하는 메시지를 표시합니다.

*참고: 사용자는 앱에서 선언하는 모든 접근 권한 값에 대해 알림을 받습니다. 앱에 필요하지 않은 접근 권한 값을 모두 제거하는 것이 좋습니다.*

### <a name="file-downloads"></a>파일 다운로드

브라우저에 표시되는 것 같은 기존 파일 다운로드는 현재 지원되지 않습니다.

### <a name="chrome-platform-apis"></a>Chrome 플랫폼 API

Chrome은 백그라운드 스크립트로 실행할 수 있는 [특별한 용도의 API](https://developer.chrome.com/apps/api_index)를 앱에 제공합니다. 이러한 API는 지원되지 않습니다. 이에 해당하는 기능과 훨씬 더 많은 기능을 [Windows 런타임 API](https://msdn.microsoft.com/library/windows/apps/br211377.aspx)에서 찾을 수 있습니다.

## <a name="related-topics"></a>관련 항목

- [UWP(유니버설 Windows 플랫폼) 기능에 액세스하여 웹앱 향상](./hwa-access-features.md)
- [UWP(유니버설 Windows 플랫폼) 앱 지침](http://go.microsoft.com/fwlink/p/?LinkID=397871)
- [Windows 스토어 앱용 디자인 자산 다운로드](https://msdn.microsoft.com/library/windows/apps/xaml/bg125377.aspx)
