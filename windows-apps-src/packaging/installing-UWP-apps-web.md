---
title: 웹 페이지에서 UWP 앱 설치
description: 이 섹션에서는 사용자가 웹 페이지에서 직접 앱을 설치할 수 있도록 하기 위해 필요한 단계를 검토합니다.
ms.date: 11/16/2017
ms.topic: article
keywords: Windows 10, uwp 앱 설치 관리자, AppInstaller, 사이드로드, 관련 집합, 선택적 패키지
ms.localizationpriority: medium
ms.openlocfilehash: 515beebd55049ecb4d0c6747fa7d37e76577ef7f
ms.sourcegitcommit: 681c70f964210ab49ac5d06357ae96505bb78741
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/26/2018
ms.locfileid: "7708123"
---
# <a name="installing-uwp-apps-from-a-web-page"></a>웹 페이지에서 UWP 앱 설치

일반적으로 앱은 앱 설치 관리자를 사용하여 설치하기 전에 장치에서 로컬로 사용할 수 있어야 합니다. 웹 시나리오의 경우 사용자가 웹 서버에서 앱 패키지를 다운로드해야 하며 그 후에 앱 설치 관리자를 사용하여 설치할 수 있음을 의미합니다. 이는 비효율적이며 디스크 공간을 낭비하며, 앱 설치 관리자가 이제 프로세스를 가속화하는 기능을 내장한 이유입니다.

앱 설치 관리자는 웹 서버에서 직접 앱을 설치할 수 있습니다. 사용자가 호스팅된 웹앱 패키지를 클릭하면 앱 설치 관리자가 자동으로 호출됩니다. 사용자는 앱 설치 관리자의 앱 정보 보기로 이동한 다음 클릭 한 번으로 앱을 직접 실행하게 됩니다. 

직접 앱 설치는 Windows 10 Fall Creators Update 이상에서만 가능합니다. 이전 버전의 Windows(Windows 10 1주년 업데이트로 돌아가기)는 [이전 버전의 Windows 10 웹 설치 환경](#web-install-experience)에서 지원됩니다. 이 환경은 직접 앱 설치만큼 유동적이지는 않지만 기존 앱 설치 절차를 크게 개선합니다.
  
> [!NOTE]
> 이 기능을 지원하려면 앱 설치 관리자 버전이 1.0.12271.0 이상이어야 합니다.

## <a name="protocol-activation-scheme"></a>프로토콜 활성화 스키마
이 메커니즘에서 앱 설치 관리자는 프로토콜 활성화 스키마에 대해 운영 체제에 등록됩니다. 사용자가 웹 링크를 클릭하면 브라우저는 OS와 함께 해당 웹 링크에 등록된 앱을 확인합니다. 스키마가 앱 설치 관리자에 지정된 프로토콜 활성화 스키마와 일치하면 앱 설치 관리자가 호출됩니다. 이 메커니즘은 브라우저에 독립적이라는 점에 유의해야 합니다. 예를 들어 웹 페이지에 통합하는 동안 웹 브라우저의 차이점을 고려할 필요가 없기 때문에 사이트 관리자에게 유용합니다. 

### <a name="requirements-for-protocol-activation-scheme"></a>프로토콜 활성화 스키마 요구 사항

1. 웹 서버를 바이트 범위 요청 (HTTP/1.1)를 지원 해야 합니다.
    - HTTP/1.1 프로토콜을 지 원하는 서버 바이트 범위 요청에 대 한 지원이 있어야 합니다. 
2. 웹 서버를 Windows 10 앱 패키지 콘텐츠 유형에 대해 알아야
    - 다음은 [웹 구성 파일](web-install-IIS.md#step-7---configure-the-web-app-for-app-package-mime-types) 의 일부로 새 콘텐츠 형식을 선언 하는 방법

### <a name="how-to-enable-this-on-a-webpage"></a>웹 페이지에서 이를 활성화하는 방법 
웹 사이트에서 앱 패키지를 호스팅하려는 앱 개발자는 다음 단계를 따라야 합니다.

웹 페이지에서 참조할 때 앱 설치 관리자가 등록된 활성화 스키마 `'ms-appinstaller:?source='`로 앱 패키지 URI에 접두사를 지정합니다. 자세한 내용은 **MyApp 웹 페이지**의 예제를 참조하세요. 
``` html
<html>
    <body>
        <h1> MyApp Web Page </h1>
        <a href="ms-appinstaller:?source=http://mywebservice.azureedge.net/HubApp.appx"> Install app package </a>
        <a href="ms-appinstaller:?source=http://mywebservice.azureedge.net/HubAppBundle.appxbundle"> Install app bundle  </a>
        <a href="ms-appinstaller:?source=http://mywebservice.azureedge.net/HubAppSet.appinstaller"> Install related set </a>
    </body>
</html>
```

## <a name="signing-the-app-package"></a>앱 패키지에 서명
앱을 설치하는 사용자는 신뢰할 수 있는 인증서를 사용하여 앱 패키지에 서명해야 합니다. 신뢰할 수 있는 인증 기관의 타사 유료 인증서를 사용하여 앱 패키지에 서명할 수 있습니다. 제3자 인증서를 사용하는 경우 사용자는 앱을 설치 및 실행하기 위해 디바이스를 사이드로드 또는 개발자 모드로 두어야 합니다.

기업 내 직원에게 앱을 배포하는 경우 기업이 발급한 인증서를 사용하여 앱에 서명할 수 있습니다. 엔터프라이즈 인증서는 앱이 설치될 모든 디바이스에 배포해야 합니다. 엔터프라이즈 앱 배포에 대한 자세한 내용은 [엔터프라이즈 앱 관리](https://docs.microsoft.com/windows/client-management/mdm/enterprise-app-management)를 참조하세요.

## 이전 버전 Windows 10에서의 웹 설치 환경<a name="web-install-experience"></a>

브라우저에서 앱 설치 관리자를 호출하면 앱 설치 관리자를 사용할 수 있는 모든 버전의 Windows 10(Windows 10 1주년 업데이트)에서 지원됩니다. 하지만 패키지를 먼저 다운로드할 필요 없이 웹에서 직접 설치하는 기능은 Windows 10 Fall Creators Update에서만 사용할 수 있습니다.  

이전 버전의 Windows 10 사용자(앱 설치 관리자 사용 가능)는 앱 설치 관리자를 통해 UWP 앱의 웹 설치를 이용할 수도 있지만 사용자 환경은 다를 수 있습니다. 이 사용자가 웹 링크를 클릭하면 앱 설치 관리자는 **설치** 대신 패키지를 **다운로드**하라는 메시지를 표시합니다. 다운로드가 끝나면 앱 설치 관리자는 다운로드한 패키지를 자동으로 시작합니다. 앱 패키지가 웹에서 다운로드되기 때문에 보안 검사를 위해 이 파일에 Microsoft SmartScreen이 진행됩니다. 사용자가 계속할 수 있는 권한을 부여한 다음 **설치**를 한 번 더 클릭하면 앱을 사용할 수 있습니다.

이 흐름은 Windows 10 Fall Creators Update의 직접 설치만큼 원활하지는 않지만 사용자는 여전히 앱을 빠르게 실행할 수 있습니다. 또한 이 흐름을 통해 사용자는 불필요하게 드라이브에서 공간을 차지하는 앱 패키지 파일에 대해 걱정할 필요가 없습니다. 앱 설치 관리자는 앱 데이터 폴더에 패키지를 다운로드하고 더 이상 필요 없는 패키지를 지워서 공간을 효율적으로 관리합니다. 

다음은 앱 설치 관리자의 Windows 10 Fall Creators Update 버전과 이전 버전을 빠르게 비교한 것입니다.

| 앱 설치 관리자, 최신 버전 | 앱 설치 관리자, 이전 버전 |
|------------------------------|----------------------------------|
| 앱 설치 관리자는 다운로드가 시작되기 전에 앱 정보를 표시합니다. | 브라우저가 사용자에게 다운로드하도록 선택하라는 메시지를 표시합니다.  |
| 앱 설치 관리자가 다운로드를 수행합니다. | 사용자가 수동으로 앱 패키지 실행을 시작해야 합니다. |
| 패키지 다운로드 후 앱 설치 관리자가 자동으로 앱 패키지를 실행합니다. | 사용자는 **설치**를 클릭하고 수동으로 앱 패키지를 실행해야 합니다. |
| 앱 설치 관리자는 다운로드한 패키지의 폐기를 처리합니다. | 사용자는 다운로드한 파일을 수동으로 삭제해야 합니다. |

## <a name="web-install-experience-on-previous-versions-of-windows-10"></a>이전 버전 Windows 10에서의 웹 설치 환경
Windows 10 Fall Creators Update 이전 버전에서는 앱 설치 관리자가 웹에서 직접 앱을 설치할 수 없습니다. 이 버전에서 앱 설치 관리자는 로컬에서 사용할 수 있는 앱 패키지만 설치할 수 있습니다. 대신 앱 설치 관리자는 패키지를 다운로드하고 사용자가 다운로드한 패키지를 두 번 클릭하여 설치해야 합니다.


## <a name="microsoft-smartscreen-integration"></a>Microsoft SmartScreen 통합

Microsoft SmartScreen은 앱 설치 관리자를 통해 앱을 설치하기 위한 설치 프로세스의 일부입니다. SmartScreen은 장치로 이동할 수 있는 잘못된 콘텐츠로부터 사용자를 보호합니다. 앱 설치 관리자의 최신 업데이트를 통해 SmartScreen 통합은 알 수 없는 앱을 설치하고 장치가 손상되지 않게 보호할 때 더 원활하고 강력한 성능으로 경고를 보냅니다. 
