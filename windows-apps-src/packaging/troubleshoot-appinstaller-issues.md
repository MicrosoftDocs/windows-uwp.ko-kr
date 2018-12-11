---
title: 앱 설치 관리자 파일 관련 설치 문제 해결
description: 앱 설치 관리자 파일로 응용 프로그램을 사이드로드할 때 일반적인 문제.
ms.date: 5/2/2018
ms.topic: article
keywords: Windows 10, uwp 앱 설치 관리자, AppInstaller, 사이드로드
ms.localizationpriority: medium
ms.openlocfilehash: d4c3aa690dd45a50e6f33d664fbc6cc4503e93f8
ms.sourcegitcommit: 231065c899d0de285584d41e6335251e0c2c4048
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 12/10/2018
ms.locfileid: "8829926"
---
# <a name="troubleshoot-installation-issues-with-the-app-installer-file"></a>앱 설치 관리자 파일 관련 설치 문제 해결

앱 설치 관리자 파일에서 응용 프로그램을 설치하는 데 문제가 있는 경우 이 항목은 도움이 될 수 있는 몇 가지 문제 해결 지침을 제공합니다.

## <a name="prerequisites"></a>사전 요구 사항

Windows 10에서 앱을 사이드로드할 수 있어야 하며 사용자 장치는 다음 요구 사항을 만족해야 합니다.

- 장치는 개발자 모드 또는 사이드로드 앱을 사용할 수 있어야 합니다. 자세한 내용은 [디바이스를 개발에 사용하도록 설정](https://docs.microsoft.com/windows/uwp/get-started/enable-your-device-for-development)을 참조하세요.
- 패키지를 서명하기 위해 사용되는 인증서는 장치에서 신뢰할 수 있어야 합니다. 자세한 내용은 아래의 **신뢰할 수 있는 인증서** 섹션을 참조하십시오.
- Windows 10 버전은 `.appinstaller` 파일 스키마 및 배포 프로토콜을 지원해야 합니다.

## <a name="common-issues"></a>일반적인 문제

사용자 컴퓨터에 처음으로 응용 프로그램을 사이드로드할 때 몇 가지 일반적인 문제가 있습니다. 다음 몇 가지 섹션은 가장 자주 묻는 문제 및 해결 방법을 설명합니다.

### <a name="windows-version"></a>Windows 버전

각 Windows 10 릴리스는 사이드로드 환경을 개선하고 있으며 아래의 테이블에서 각 주요 버전에서 사용할 수 있는 기능을 찾을 수 있습니다. 자신의 Windows 10 버전에서 지원되지 않는 방법을 사용하여 앱을 사이드로드하면 배포 오류가 발생합니다.

| 버전 | 사이드로드 노트 |
|---------|----------------|
| 빌드 17134(2018년 4월 업데이트, 버전 1804)    | `.appinstaller` 파일은 UNC/공유 폴더를 통해 액세스할 수 있습니다. 구성 업데이트 확인 또한 사용할 수 있습니다. |
| 빌드 16299(Fall Creators Update, 버전 1709) | 앱에 자동 업데이트를 제공하도록 `.appinstaller` 파일을 도입했습니다. 이 버전은 HTTP 끝점만 지원합니다. 업데이트 확인은 구성할 수 없으며 24시간 마다 발생합니다. |
| 빌드 15063(크리에이터스 업데이트, 버전 1703)      | 앱 설치 관리자 앱은 Microsoft Store에서 앱 종속성(릴리스 모드로만)을 다운로드할 수 있습니다. |
| 빌드 14393(1주년 업데이트, 버전 1607)   | .appx 및 .appxbundle 파일을 설치하기 위해 앱 설치 관리자 앱을 도입했으며 .appinstaller 파일은 지원되지 않습니다. |
| 빌드 10586(11월 업데이트, 버전 1511)      | 사이드로드는 [Add-AppxPackage](https://docs.microsoft.com/powershell/module/appx/add-appxpackage?view=win10-ps) 명령을 사용하여 PowerShell을 통해서만 사용할 수 있습니다. |
| 빌드 10240(Windows 10, 버전 1507)           | 사이드로드는 [Add-AppxPackage](https://docs.microsoft.com/powershell/module/appx/add-appxpackage?view=win10-ps) 명령을 사용하여 PowerShell을 통해서만 사용할 수 있습니다. |

### <a name="trusted-certificates"></a>신뢰할 수 있는 인증서

앱 패키지는 장치에서 신뢰할 수 있는 인증서로 서명해야 합니다. 일반적인 인증 기관에서 제공되는 인증서는 기본적으로 Windows 운영 체제에서 신뢰할 수 있지만 인증서를 신뢰할 수 없는 경우 앱을 설치하기 **전에** 장치에 설치해야 합니다. 인증서를 신뢰하려면 인증서는 장치의 다음 로컬 컴퓨터 인증서 저장소 중 하나에 있어야 합니다.

- 신뢰할 수 있는 판매자
- 신뢰할 수 있는 사용자
- 신뢰할 수 있는 루트 기관(권장하지 않음)

 >[!IMPORTANT]
 > 로컬 컴퓨터 저장소에 인증서를 설치하려면 관리자 액세스 권한이 필요합니다.

### <a name="dependencies-not-installed"></a>종속성이 설치되지 않음 

UWP 응용 프로그램은 앱을 생성하는 데 사용되는 응용 프로그램 플랫폼에 따라 프레임워크 종속성을 가질 수 있습니다. C# 또는 VB를 사용하는 경우 앱에는 .NET 런타임 및 .NET 프레임워크 패키지가 필요합니다. C++ 응용 프로그램에는 VCLibs가 필요합니다.

>[!IMPORTANT] 
> 앱 패키지가 릴리스 모드 구성에서 빌드되며 프레임워크 종속성은 Microsoft Store에서 가져옵니다. 그러나 앱이 디버그 모드 구성으로 빌드된 경우 종속성은 `.appinstaller` 파일에 지정된 위치에서 가져옵니다.

### <a name="files-not-accessible"></a>파일에 액세스할 수 없음

HTTP 끝점에서 설치할 때 모든 파일이 올바른 MIME 형식으로 액세스할 수 있는지 확인하는 것이 중요합니다. Visual Studio에서 생성된 HTML 페이지에 제공되는 링크를 따라 이러한 파일을 확인하는 것이 가장 쉬운 방법입니다. 다음 파일을 확인해야 합니다.

- `.appinstaller` 다음으로 사용 가능한 파일 `application/xml`
- `.appx` 및 다음으로 사용 가능한 `.appxbundle` 파일 `application/vns.ms-appx`

## <a name="isolate-app-installer-app-issues"></a>앱 설치 관리자 앱 문제 격리

앱 설치 관리자 앱이 앱을 설치할 수 없는 경우 다음 단계는 설치 문제를 식별하는 데 도움이 됩니다.

### <a name="verify-app-package-file-installation"></a>앱 패키지 파일 설치 확인

- 로컬 폴더에 앱 패키지 파일을 다운로드 하 고 [Add-appxpackage](https://docs.microsoft.com/powershell/module/appx/add-appxpackage?view=win10-ps) PowerShell 명령을 사용 하 여이 설치 합니다.

- 로컬 폴더에 `.appinstaller` 파일을 다운로드하고 `Add-AppxPackage -Appinstaller` PowerShell 명령을 사용하여 이를 설치합니다.

## <a name="related-logs"></a>관련된 로그

앱 배포 인프라는 Windows 이벤트 뷰어에 디버깅에 대한 로그를 제공합니다. 이러한 로그는 여기에서 찾을 수 있습니다. `Application and Services Logs->Microsoft->Windows->AppxDeployment-Server`



