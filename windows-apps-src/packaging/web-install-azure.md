---
author: c-don
title: Azure 웹 서버에서 UWP 앱 설치
description: 이 자습서에서는 Azure 웹 서버를 설정하고, 웹 앱이 앱 패키지를 호스트할 수 있는지 확인하고, 효과적으로 앱 설치 관리자를 호출 및 사용하는 방법에 대해 설명합니다.
ms.author: cdon
ms.date: 09/30/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: Windows 10, uwp 앱 설치 관리자, AppInstaller, 사이드로드, 관련 집합, 선택적 패키지, Azure 웹 서버
ms.localizationpriority: medium
ms.openlocfilehash: b98ca6316f733210dbdbc5201178b3a89a2b5982
ms.sourcegitcommit: 72835733ec429a5deb6a11da4112336746e5e9cf
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/19/2018
ms.locfileid: "5162489"
---
# <a name="install-a-uwp-app-from-an-azure-web-app"></a>Azure 웹앱에서 UWP 앱 설치

앱 설치 관리자 앱을 사용하여 개발자 및 IT 전문가가 자신의 CDN(콘텐츠 배달 네트워크)에서 Windows 10 앱을 호스트하여 배포할 수 있습니다. Microsoft Store에 자신의 앱을 게시하고 싶지 않거나 게시할 필요가 없지만 여전히 Windows 10 패키지 및 배포 플랫폼을 활용하고자 하는 기업에 유용합니다.

이 항목에서는 UWP 앱 패키지를 호스트하기 위해 Azure 웹 서버를 구성하는 단계 및 앱 패키지를 설치하기 위해 앱 설치 관리자 앱을 사용하는 방법을 설명합니다.

이 자습서에서는 IIS 서버를 설정하여 로컬로 웹 응용 프로그램이 앱 패키지를 제대로 호스트하고 앱 설치 관리자 앱을 효율적으로 호출 및 사용할 수 있는지 확인하는 작업에 대해 다룹니다. 앱 설치 관리자 웹 설치 요구 사항에 맞는지 확인하기 위해 필드(Azure 및 AWS)의 인기 있는 클라우드 웹 서비스에서 제대로 웹 응용 프로그램을 호스트하는 자습서도 있습니다. 이 단계별 자습서에는 전문 지식이 필요하지 않으며 매우 쉽게 따라 할 수 있습니다. 

## <a name="setup"></a>설정

이 자습서를 수행하려면 다음이 필요합니다.
 
1. Microsoft Azure 구독 
2. UWP 앱 패키지 - 배포할 앱 패키지

옵션: GitHub의 [Starter 프로젝트](https://github.com/AppInstaller/MySampleWebApp) 작업할 앱 패키지 또는 웹 페이지가 없지만 여전히 이 기능을 사용하는 방법을 알아보고자 하는 경우 유용합니다.

### <a name="step-1---get-an-azure-subscription"></a>1단계 - Azure 구독 가져오기
Azure 구독을 가져오려면 [Azure 계정 페이지](https://azure.microsoft.com/free/)를 방문합니다. 이 자습서의 목적을 위해 무료 멤버십을 사용할 수 있습니다.

### <a name="step-2---create-an-azure-web-app"></a>2단계 - Azure 웹앱 만들기 
Azure Portal 페이지에서 **+ 리소스 만들기** 버튼을 클릭한 다음 **웹앱**을 선택합니다.

![만들기](images/azure-create-app.png)

고유한 **앱 이름**을 만들고 필드의 나머지 부분을 기본값으로 둡니다. **만들기**를 클릭하여 웹앱 만들기 마법사를 마칩니다. 

![웹앱 만들기](images/azure-create-app-2.png)

### <a name="step-3---hosting-the-app-package-and-the-web-page"></a>3단계 - 앱 패키지 및 웹 페이지 호스트 
웹앱을 만든 후에 Azure Portal 대시보드에서 웹앱에 액세스할 수 있습니다. 이 단계에서 Azure Portal의 GUI로 간단한 웹 페이지를 만들겠습니다.

대시보드에서 새로 만든 웹앱을 선택한 후 검색 필드를 사용하여 **앱 서비스 편집기**를 찾아 엽니다. 

편집기에 기본 `hostingstart.html` 파일이 있습니다. 파일 탐색기 패널의 빈 공간을 마우스 오른쪽 단추로 클릭하고 **파일 업로드**를 선택하여 앱 패키지 업로드를 시작합니다.

> [!NOTE]
> 사용할 수 있는 앱 패키지가 없는 경우 GitHub에서 제공된 [Starter 프로젝트](https://github.com/AppInstaller/MySampleWebApp) 리포지토리의 일부인 앱 패키지를 사용할 수 있습니다. 패키지를 서명한 인증서(MySampleApp.cer)는 GitHub의 샘플도 서명합니다. 앱을 설치하기 전에 디바이스에 인증서가 설치되어 있어야 합니다.

![업로드](images/azure-upload-file.png)

파일 탐색기 패널의 빈 공간을 마우스 오른쪽 단추로 클릭하고 **새 파일**을 새 파일을 만듭니다. 파일 이름을 `default.html`로 지정합니다.

[Starter 프로젝트](https://github.com/AppInstaller/MySampleWebApp)에서 제공된 앱 패키지를 사용하는 경우, 다음 HTML 코드를 복사하여 `default.html` 웹 페이지를 새로 만듭니다. 자체 앱 패키지를 사용하는 경우 앱 서비스 URL을 수정합니다(`source=`이후 URL). Azure Portal에서 앱의 개요 페이지에서 앱 서비스 URL을 가져올 수 있습니다.

```html
<html>
<head>
    <meta charset="utf-8" />
    <title> Install My Sample App</title>
</head>
<body>
    <a href="ms-appinstaller:?source=https://appinstaller-azure-demo.azurewebsites.net/MySampleApp.appxbundle"> Install My Sample App</a>
</body>
</html>
```

### <a name="step-4---configure-the-web-app-for-app-package-mime-types"></a>4단계 - 앱 패키지 MIME 형식에 대한 웹앱 구성

웹앱에 `Web.config`라는 이름의 새 파일을 추가합니다. 탐색기에서 `Web.config` 파일을 열고 다음 줄을 추가합니다. 

```xml
<?xml version="1.0" encoding="utf-8"?>
<configuration>
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
</configuration>
```

### <a name="step-5---run-and-test"></a>5단계 - 실행 및 테스트

사용자가 만든 웹 페이지를 시작하려면 `/default.html`이 따라오는 3단계의 URL을 브라우저에 붙여 넣습니다. 

![Edge](images/edge.png)

"내 샘플 앱 설치"를 클릭하여 앱 설치 관리자를 실행하고 앱 패키지를 설치합니다. 

## <a name="troubleshooting-issues"></a>문제 해결

### <a name="app-installer-app-fails-to-install"></a>앱 설치 관리자 앱이 설치에 실패 
앱 패키지가 서명된 인증서가 디바이스에 설치되어 있지 않으면 앱이 설치되지 않습니다. 이 문제를 해결하려면 앱을 설치하기 전에 인증서를 설치해야 합니다. 공용 배포를 위해 앱 패키지를 호스팅할 경우 인증 기관에서 발급한 인증서를 사용하여 앱 패키지에 서명하는 것이 좋습니다. 

![앱 인증서](images/aws-app-cert.png)

### <a name="nothing-happens-when-you-click-the-link"></a>링크를 클릭해도 아무 작업도 실행되지 않음 
앱 설치 관리자 앱이 설치되어 있는지 확인합니다. **설정** -> **앱 및 기능**으로 이동하여 설치된 앱 목록에서 앱 설치 관리자를 찾습니다. 

