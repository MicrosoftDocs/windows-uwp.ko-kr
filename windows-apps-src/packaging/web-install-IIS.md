---
author: laurenhughes
title: IIS 서버에서 UWP 앱 설치
description: 이 자습서에서는 웹 앱 수 있는 앱 패키지를 호스트할 호출 및 확인 효과적으로 앱 설치 관리자를 사용 하 여 IIS 서버를 설정 하는 방법에 설명 합니다.
ms.author: cdon
ms.date: 05/30/2018
ms.topic: article
keywords: windows 10, uwp, 앱 설치 관리자, AppInstaller, 사이드 로드, 관련 IIS 서버 설정, 선택적 패키지
ms.localizationpriority: medium
ms.openlocfilehash: 2898a3450f75379492bae1ade5c85581cc8e5a4e
ms.sourcegitcommit: e2fca6c79f31e521ba76f7ecf343cf8f278e6a15
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/15/2018
ms.locfileid: "6982508"
---
# <a name="install-a-uwp-app-from-an-iis-server"></a>IIS 서버에서 UWP 앱 설치

이 자습서에서는 웹 앱 수 있는 앱 패키지를 호스트할 호출 및 확인 효과적으로 앱 설치 관리자를 사용 하 여 IIS 서버를 설정 하는 방법에 설명 합니다.

앱 설치 관리자 앱을 사용하여 개발자 및 IT 전문가가 자신의 CDN(콘텐츠 배달 네트워크)에서 Windows 10 앱을 호스트하여 배포할 수 있습니다. Microsoft Store에 자신의 앱을 게시하고 싶지 않거나 게시할 필요가 없지만 여전히 Windows 10 패키지 및 배포 플랫폼을 활용하고자 하는 기업에 유용합니다. 

## <a name="setup"></a>Setup

이 자습서를 성공적으로 이동 하려면 다음이 필요 합니다.

1. Visual Studio 2017  
2. IIS 및 웹 개발 도구 
3. UWP 앱 패키지 - 배포할 앱 패키지

옵션: GitHub의 [Starter 프로젝트](https://github.com/AppInstaller/MySampleWebApp) 하지만이 기능을 사용 하는 방법을 알아보려면 여전히 작업할 앱 패키지에 없는 경우에 유용 합니다.

## <a name="step-1---install-iis-and-aspnet"></a>단계 1-IIS와 ASP.NET 설치 

[인터넷 정보 서비스](https://www.iis.net/) 는 시작 메뉴를 통해 설치할 수 있는 Windows 기능입니다. **Windows 기능**에 대 한 **시작 메뉴** 검색 합니다.

찾아 **인터넷 정보 서비스** IIS 설치를 선택 합니다.

> [!NOTE]
> 인터넷 정보 서비스에서 모든 확인란을 선택 하지 않아도 됩니다. **인터넷 정보 서비스** 를 검사할 때 선택한 이벤트만 충분 합니다.

ASP.NET 4.5 이상을 설치 해야 합니다. 설치 하기 위해, 찾습니다 **인터넷 정보 서비스->는 웹 서비스 응용 프로그램 개발 기능을->** 합니다. ASP.NET 4.5 보다 크거나 ASP.NET의 버전을 선택 합니다.

![ASP.NET 설치](images/install-asp.png)

## <a name="step-2---install-visual-studio-2017-and-web-development-tools"></a>2 단계-설치 Visual Studio 2017 및 웹 개발 도구 

[Visual Studio 2017 설치](https://docs.microsoft.com/visualstudio/install/install-visual-studio) 아직 설치 하지 않은 경우. Visual Studio 2017 이미 있는 경우 다음 워크 로드 설치 되어 있는지 확인 합니다. 워크 로드를 설치에 없는 경우 따르는 (시작 메뉴에서 찾을 수) Visual Studio 설치 관리자를 사용 하 여 합니다.  

설치 하는 동안 **ASP.NET 및 웹 개발** 및에 관심이 있는 다른 워크 로드를 선택 합니다. 

설치가 완료 되 면 Visual Studio를 시작 하 고 새 프로젝트를 만듭니다 (**파일** -> **새 프로젝트**).

## <a name="step-3---build-a-web-app"></a>3 단계-웹 앱 제작

**관리자 권한** 으로 Visual Studio 2017을 시작 하 고 **빈** 프로젝트 템플릿을 사용 하 여 새 **Visual C# 웹 응용 프로그램** 프로젝트를 만듭니다. 

![새 프로젝트](images/sample-web-app.png)

## <a name="step-4---configure-iis-with-our-web-app"></a>4 단계-IIS 웹 앱을 사용 하 여 구성 

솔루션 탐색기에서 루트 프로젝트를 마우스 오른쪽 단추로 클릭 하 고 **속성**을 선택 합니다.

웹 앱 속성에서 **웹** 탭을 선택 합니다. **서버** 섹션에서 드롭다운 메뉴에서에서 **로컬 IIS** 를 선택 하 고 **가상 디렉터리 만들기**를 클릭 합니다. 

![웹 탭](images/web-tab.png)

## <a name="step-5---add-an-app-package-to-a-web-application"></a>5 단계-앱 패키지를 웹 응용 프로그램 추가 

웹 응용 프로그램으로 배포 하려는 앱 패키지를 추가 합니다. 앱 패키지를 사용할 수 없는 경우 GitHub에서 제공 된 [starter 프로젝트 패키지](https://github.com/AppInstaller/MySampleWebApp/tree/master/MySampleWebApp/packages) 에 포함 된 앱 패키지를 사용할 수 있습니다. 패키지를 서명한 인증서(MySampleApp.cer)는 GitHub의 샘플도 서명합니다. 인증서가 (9 단계) 앱을 설치 하기 전에 장치에 설치 되어 있어야 합니다.

라는 웹 앱에 추가 된 새 폴더 starter 프로젝트 웹 응용 프로그램에서 `packages` 앱 패키지를 배포할 수 있는 합니다. Visual Studio에서 폴더를 만들고 솔루션 탐색기의 루트를 마우스 오른쪽 단추로 클릭을 선택 하는 **추가** -> **새 폴더** 이름을 `packages`. 앱 패키지 폴더를 추가 하려면 마우스 오른쪽 단추로 클릭은 `packages` 폴더와 **추가**선택 -> **기존 항목...** 및 탐색 앱 패키지 위치 합니다. 

![패키지 추가](images/add-package.png)

## <a name="step-6---create-a-web-page"></a>6 단계-웹 페이지 만들기

이 샘플 웹 응용 프로그램 간단한 HTML을 사용합니다. 사용자는 필요에 맞게 필요에 따라 웹 앱을 빌드할 수 있습니다. 

솔루션 탐색기의 루트 프로젝트를 마우스 오른쪽 단추로 클릭 한 다음 **추가**선택 합니다 -> **새 항목**을 **웹** 섹션에서 새 **HTML 페이지** 를 추가 합니다.

HTML 페이지를 만든 후 솔루션 탐색기에서 HTML 페이지에서 마우스 오른쪽 단추로 클릭 하 고 **시작 페이지로 설정**를 선택 합니다.  

코드 편집기 창에서 열려는 HTML 파일을 두 번 클릭 합니다. 이 자습서에서는 Windows 10 앱 설치를 성공적으로 앱 설치 관리자 앱을 호출 하는 웹 페이지에서 필수 요소에만 사용 됩니다. 

웹 페이지에 다음 HTML 코드를 포함 합니다. 성공적으로 앱 설치 관리자를 호출 하는 키 OS를 사용 하 여 앱 설치 관리자를 등록 하는 사용자 지정 체계를 사용 하는 것: `ms-appinstaller:?source=`. 자세한 내용은 아래 코드 예제를 참조 하세요.

> [!NOTE]
> 사용자 지정 스키마 VS 솔루션의 웹 탭에서 프로젝트 Url과 일치 하는 뒤에 지정 된 URL 경로 확인 합니다.
 
```HTML
<html>
<head>
    <meta charset="utf-8" />
    <title> Install Page </title>
</head>
<body>
    <a href="ms-appinstaller:?source=http://localhost/SampleWebApp/packages/MySampleApp.appxbundle"> Install My Sample App</a>
</body>
</html>
```

## <a name="step-7---configure-the-web-app-for-app-package-mime-types"></a>7 단계-앱 패키지 MIME 형식에 대 한 웹 앱 구성

솔루션 탐색기에서 **Web.config** 파일을 열고 내에서 다음 줄을 추가 합니다 `<configuration>` 요소입니다. 

```xml
<system.webServer>
    <!--This is to allow the web server to serve resources with the appropriate file extension-->
    <staticContent>
      <mimeMap fileExtension=".appx" mimeType="application/appx" />
      <mimeMap fileExtension=".msix" mimeType="application/msix" />
      <mimeMap fileExtension=".appxbundle" mimeType="application/appxbundle" />
      <mimeMap fileExtension=".msixbundle" mimeType="application/msixbundle" />
      <mimeMap fileExtension=".appinstaller" mimeType="application/appinstaller" />
    </staticContent>
</system.webServer>
```

## <a name="step-8---add-loopback-exemption-for-app-installer"></a>-8 단계 앱 설치 관리자에 대 한 루프백 예외를 추가 합니다.

네트워크 격리로 인해 앱 설치 관리자와 같은 UWP 앱은 같은 IP 루프백 주소를 사용 하 여 제한 된 http://localhost/. 로컬 IIS 서버를 사용할 때 루프백 제외 목록에 앱 설치 관리자를 추가 합니다. 

이렇게 하려면 **관리자** 권한으로 **명령 프롬프트** 를 열고 다음과 같이 입력: ' ' 명령줄 CheckNetIsolation.exe LoopbackExempt-a-n=microsoft.desktopappinstaller_8wekyb3d8bbwe
```

To verify that the app is added to the exempt list, use the following command to display the apps in the loopback exempt list: 
```Command Line
CheckNetIsolation.exe LoopbackExempt -s
```

찾아야 `microsoft.desktopappinstaller_8wekyb3d8bbwe` 목록에서 합니다.

앱 설치 관리자를 통해 앱 설치의 로컬 유효성 검사가 완료 되 면으로이 단계에서 추가한 루프백 예외를 제거할 수 있습니다.

' ' 명령줄 CheckNetIsolation.exe LoopbackExempt-d-n=microsoft.desktopappinstaller_8wekyb3d8bbwe
```

## Step 9 - Run the Web App 

Build and run the web application by clicking on the run button on the VS Ribbon as shown in the image below:

![run](images/run.png)

A web page will open in your browser:

![web page](images/web-page.png)

Click on the link in the web page to launch the App Installer app and install your Windows 10 app package.


## Troubleshooting issues

### Not sufficient privilege 

If running the web app in Visual Studio displays an error such as "You do not have sufficient privilege to access IIS web sites on your machine", you will need to run Visual Studio as an administrator. Close the current instance of Visual Studio and reopen it as an admin.

### Set start page 

If running the web app causes the browser to load with an HTTP 403.14 - Forbidden error, it's because the web app doesn't have a defined start page. Refer to Step 6 in this tutorial to learn how to define a start page.
