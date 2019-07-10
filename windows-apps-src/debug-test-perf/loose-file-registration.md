---
title: 느슨한 파일 등록을 통해 앱 배포
description: 이 가이드에서는 느슨한 파일 레이아웃을 사용하여 Windows 10 앱을 패키지하지 않고도 유효성을 검사하고 공유하는 방법을 보여 줍니다.
ms.date: 06/01/2018
ms.topic: article
keywords: windows 10, uwp, 장치 포털, 앱 관리자, 배포, sdk
ms.localizationpriority: medium
ms.openlocfilehash: 3369f3a982efec258fb5ac2358b2962e84e6cefb
ms.sourcegitcommit: 139717a79af648a9231821bdfcaf69d8a1e6e894
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/10/2019
ms.locfileid: "67713766"
---
# <a name="deploy-an-app-through-loose-file-registration"></a>느슨한 파일 등록을 통해 앱 배포 

이 가이드에서는 느슨한 파일 레이아웃을 사용하여 Windows 10 앱을 패키지하지 않고도 유효성을 검사하고 공유하는 방법을 보여 줍니다. 사용 완화 파일 레이아웃을 등록 개발자가 패키지 및 앱을 설치할 필요 없이 앱을 빠르게 검사할 수 있습니다. 

## <a name="what-is-a-loose-file-layout"></a>사용 완화 파일 레이아웃 이란?

사용 완화 파일 레이아웃은 단순히 앱 콘텐츠 패키징 프로세스를 진행 하는 대신 폴더에 배치 작업입니다. 패키지 내용을 폴더에 "느슨한" 제공 되며 패키지 되지 않습니다. 

> [!WARNING]
> 사용 완화 파일 레이아웃을 등록 하는 개발자와 디자이너가 활성 개발 중 해당 앱을 빠르게 검사할 수입니다. 이 방법은 "개밥" 하는 데 사용할 또는 앱 비행 해서는 안 됩니다. 신뢰할 수 있는 인증서로 서명 된 패키지 된 앱에서 최종 유효성 검사를 수행 하는 것이 좋습니다. 

## <a name="advantages-of-loose-file-registration"></a>사용 완화 파일 등록의 장점

- **빠른 유효성 검사** -사용자가 신속 하 게 느슨한 파일 레이아웃을 등록 하 고 앱을 시작 앱 파일이 이미 압축 되지 않으므로 합니다. 일반 앱 처럼 사용자 설계 된 대로 앱을 사용할 수 없게 됩니다. 
- **쉽게 네트워크에 배포할** -느슨한 파일이 로컬 드라이브 대신 네트워크 공유에서 개발자가 네트워크에 액세스할 수 있는 다른 사용자를 네트워크 공유 위치를 보낼 수 있습니다 및 느슨한 파일 레이아웃을 등록 하 고 실행할 수 하는 경우는 앱입니다. 따라서 여러 사용자가 동시에 앱의 유효성을 검사 하도록 합니다. 
- **공동 작업** -느슨한 파일로 등록 개발자와 디자이너 앱이 등록 되는 동안 시각적 자산에서 계속 작업을 허용 합니다. 사용자는 앱을 실행할 때 이러한 변경 내용이 표시 됩니다. 참고가이 방식으로 정적 자산만 변경할 수 있습니다. 모든 코드 또는 동적으로 생성된 된 콘텐츠를 수정 해야 할 경우 앱을 다시 컴파일할 해야 있습니다.

## <a name="how-to-register-a-loose-file-layout"></a>사용 완화 파일 레이아웃을 등록 하는 방법

Windows는 로컬 및 원격 장치에서 느슨한 파일 레이아웃을 등록 하려면 여러 개발자 도구를 제공 합니다. 선택할 수 있습니다 `WinDeployAppCmd` (Windows SDK 도구), Windows Device Portal, PowerShell, 및 [Visual Studio](https://docs.microsoft.com/windows/uwp/debug-test-perf/deploying-and-debugging-uwp-apps#register-layout-from-network)합니다. 아래 이러한 도구를 사용 하 여 느슨한 파일을 등록 하는 방법을 살펴보겠습니다. 하지만 먼저 설치 했는지를 확인 합니다.

- 장치에서 Windows 10 크리에이터 업데이트 (빌드 14965) 이상 이어야 합니다.
- 사용 하도록 설정 해야 합니다 [개발자 모드](https://docs.microsoft.com/windows/uwp/get-started/enable-your-device-for-development) 하 고 [장치 검색](https://docs.microsoft.com/en-us/windows/uwp/get-started/enable-your-device-for-development#device-discovery) 모든 장치에서.

> [!IMPORTANT]
> 사용 완화 파일을 등록 하는 네트워크 공유 (SMB) 프로토콜을 지 원하는 장치에서 사용할 수 만입니다. 데스크톱, Xbox입니다. 

### <a name="register-with-windeployappcmd"></a>WinDeployAppCmd 등록

Windows 10 크리에이터 스 업데이트 (빌드 14965) 이상에 해당 SDK 도구를 사용 하는 경우 사용할 수 있습니다는 `WinDeployAppCmd` 명령 프롬프트에서 명령을 합니다.

```cmd
WinAppDeployCmd.exe registerfiles -remotedeploydir <Network Path> -ip <IP Address> -pin <target machine PIN>
```

**네트워크 경로** – 앱의 느슨한 파일 경로입니다.

**IP 주소** – 대상 컴퓨터의 IP 주소입니다.

**대상 컴퓨터 PIN** – PIN, 대상 장치를 사용 하 여 연결을 설정 하는 데 필요한 경우. 다시 시도 하 라는 메시지가 표시 됩니다는 `-pin` 인증이 필요한 경우 옵션입니다. 참조 [장치 검색](https://docs.microsoft.com/windows/uwp/get-started/enable-your-device-for-development#device-discovery) 에 PIN을 가져오는 방법을 알아봅니다.

### <a name="windows-device-portal"></a>Windows Device Portal

Windows Device Portal 모든 Windows 10 장치에서 사용할 수 있으며 개발자가 테스트 하 고 해당 작업의 유효성을 검사 하는 데 사용 됩니다. 모든 해당 브라우저 UX 사용 하 여 개발자 커뮤니티의 대상 그룹 및 REST 끝점에 대응 하는 것입니다. 장치 포털에 대 한 자세한 내용은 참조는 [Windows Device Portal 개요](device-portal.md)합니다.

사용 완화 파일 레이아웃 장치 포털에 등록 하려면 다음이 단계를 수행 합니다.

1. 단계를 수행 하 여 장치 포털에 연결 합니다 **설치** 섹션을 [Windows Device Portal 개요](device-portal.md)합니다.
1. Apps Manager 탭에서 선택 **네트워크 공유에서 등록**합니다.
1. 사용 완화 파일 레이아웃을 네트워크 공유 경로 입력 합니다. 
1. 호스트 장치 네트워크 공유에 액세스할 수 없으면 필요한 자격 증명을 입력 하 라는 메시지가 됩니다.
1. 등록이 완료 되 면 앱을 시작할 수 있습니다.

장치 포털의 앱 Manager 페이지에서 등록할 수도 있습니다 선택적 느슨한 파일 레이아웃 기본 앱을 선택 하 여 합니다 **선택적 패키지를 지정 하려면** 확인란을 선택 하 고 다음 네트워크를 지정 공유 선택적 앱의 경로 . 

### <a name="powershell"></a>PowerShell 

또한 Windows PowerShell을 사용 하면 로컬 장치에만 사용 완화 파일 레이아웃을 등록할 수 있습니다. 원격 장치에 레이아웃을 등록 해야 할 경우 다른 메서드 중 하나를 사용 해야 합니다. 

사용 완화 파일 레이아웃을 등록 하려면 PowerShell을 시작 하 고 다음을 입력 합니다.

```PowerShell
Add-AppxPackage -Register <path to manifest file>
```

## <a name="troubleshooting"></a>문제 해결

### <a name="mapped-network-drives"></a>연결된 네트워크 드라이브
현재 매핑된 네트워크 드라이브는 느슨한 파일 등록에 대 한 지원 되지 않습니다. 네트워크 공유 경로를 전체를 사용 하 여 매핑된 드라이브를 참조 하십시오.

### <a name="registration-failure"></a>등록 실패
파일 레이아웃에 액세스할 수 있도록 장치를 등록을 수행 해야 합니다. 파일 레이아웃을 네트워크 공유에 호스트 되는 경우에 장치에서 액세스할 수 있는지를 확인 합니다. 

### <a name="modifications-to-visual-assets-arent-being-loaded-in-the-app"></a>시각적 자산 수정 앱에서 로드 되지 않습니다. 
앱 시작 시 해당 시각적 자산을 로드 합니다. 앱을 시작한 후 시각적 자산을 수정 된, 경우 최신 변경 내용을 보려면 앱 다시 시작 해야 합니다.
