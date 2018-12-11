---
title: 느슨한 파일 등록을 통해 앱 배포
description: 이 가이드의 유효성을 검사 하 여 패키징합니다 필요 없이 Windows 10 앱을 공유 느슨한 파일 레이아웃을 사용 하는 방법을 보여 줍니다.
ms.date: 6/1/2018
ms.topic: article
keywords: windows 10, uwp, 장치 포털, 앱 관리자, 배포, sdk
ms.localizationpriority: medium
ms.openlocfilehash: 928c07bd23228f0fefd78be6019a0d116b2e6e4b
ms.sourcegitcommit: 8921a9cc0dd3e5665345ae8eca7ab7aeb83ccc6f
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 12/10/2018
ms.locfileid: "8878660"
---
# <a name="deploy-an-app-through-loose-file-registration"></a>느슨한 파일 등록을 통해 앱 배포 

이 가이드의 유효성을 검사 하 여 패키징합니다 필요 없이 Windows 10 앱을 공유 느슨한 파일 레이아웃을 사용 하는 방법을 보여 줍니다. 느슨한 파일 레이아웃을 등록 하 여 앱 패키지 및 앱을 설치 하지 않고도 신속 하 게 확인 하기 위한 개발자가 있습니다. 

## <a name="what-is-a-loose-file-layout"></a>느슨한 파일 레이아웃 이란?

느슨한 파일 레이아웃은 패키징 프로세스를 수행 하는 대신 폴더의 앱 콘텐츠를 배치 하는 작업 하기만 하면 됩니다. 패키지 내용이 폴더에 "느슨하게" 사용 가능 하 고 패키지 되지 않습니다. 

> [!WARNING]
> 개발자와 디자이너가 신속 하 게 적극적으로 개발 하는 동안 앱을 확인 하기 위한 느슨한 파일 레이아웃 등록이 됩니다. 이 방법은 "평가"를 사용할 또는 앱 플라이트 해서는 안 됩니다. 신뢰할 수 있는 인증서로 서명 된 패키지 앱에서 최종 유효성 검사 수행 하는 것이 좋습니다. 

## <a name="advantages-of-loose-file-registration"></a>느슨한 파일 등록의 이점

- **빠른 유효성 검사** -사용자가 신속 하 게 느슨한 파일 레이아웃을 등록 하 고 응용 프로그램을 실행 앱 파일은 이미 압축 된 되지 않으므로 합니다. 일반 응용 프로그램에서와 마찬가지로 사용자 설계 된 앱을 사용할 수 없게 됩니다. 
- **네트워크에서 배포를 쉽게** -느슨한 파일은 로컬 드라이브 대신 네트워크 공유에 있는 경우 개발자가 네트워크에 액세스할 수 있는 다른 사용자에 게 네트워크 공유 위치를 보낼 수 있으며 느슨한 파일 레이아웃을 등록 하 고 앱을 실행할 수 있습니다. 따라서 여러 사용자가 동시에 앱의 유효성을 검사를 수 있습니다. 
- **공동 작업** -느슨한 파일 등록에는 개발자와 디자이너가 앱 등록 되는 동안 시각적 자산에 작업을 계속할 수 있습니다. 사용자가 앱을 시작할 때 이러한 변경 내용을 표시 됩니다. 참고가 방식으로 정적 자산만 변경할 수 있습니다. 모든 코드 또는 동적으로 생성된 된 콘텐츠를 수정 해야 하는 경우 앱 다시 컴파일할 해야 합니다.

## <a name="how-to-register-a-loose-file-layout"></a>느슨한 파일 레이아웃을 등록 하는 방법

Windows는 로컬 및 원격 장치에서 느슨한 파일 레이아웃을 등록 하는 여러 개발자 도구를 제공 합니다. 중에서 선택할 수 있습니다 `WinDeployAppCmd` (Windows SDK 도구), Windows Device Portal, PowerShell 및 [Visual Studio](https://docs.microsoft.com/windows/uwp/debug-test-perf/deploying-and-debugging-uwp-apps#register-layout-from-network)합니다. 아래에서는 이러한 도구를 사용 하 여 느슨한 파일 등록 하는 방법을 위로 이동 합니다. 그러나 먼저 설치 있는지 확인 합니다.

- 장치는 Windows 10 크리에이터 스 업데이트 (빌드 14965) 이상 이어야 합니다.
- [개발자 모드](https://msdn.microsoft.com/windows/uwp/get-started/enable-your-device-for-development) 및 모든 장치에서 [장치 검색](https://docs.microsoft.com/en-us/windows/uwp/get-started/enable-your-device-for-development#device-discovery) 사용 하도록 설정 해야 합니다.

> [!IMPORTANT]
> 느슨한 파일 등록은 네트워크 공유 (SMB) 프로토콜을 지 원하는 장치에서 사용할 수만: 데스크톱 및 Xbox 합니다. 

### <a name="register-with-windeployappcmd"></a>WinDeployAppCmd를 사용 하 여 등록

Windows 10 크리에이터 스 업데이트 (빌드 14965) 이상에 해당 SDK 도구를 사용 하는 경우 사용할 수 있습니다는 `WinDeployAppCmd` 명령 프롬프트에서 명령 합니다.

```cmd
WinAppDeployCmd.exe registerfiles -remotedeploydir <Network Path> -ip <IP Address> -pin <target machine PIN>
```

**네트워크 경로** -앱의 느슨한 파일의 경로입니다.

**IP 주소** -대상 컴퓨터의 IP 주소입니다.

**대상 컴퓨터 PIN** -A PIN, 대상 장치에 연결을 설정 하는 데 필요한 경우. 사용 하 여 재시도 하 라는 메시지가 표시 되는 `-pin` 인증이 필요한 경우 옵션입니다. PIN을 다운로드 하는 방법을 알아보려면 [장치 검색](https://docs.microsoft.com/windows/uwp/get-started/enable-your-device-for-development#device-discovery) 참조 하세요.

### <a name="windows-device-portal"></a>Windows Device Portal

Windows Device Portal은 모든 Windows 10 장치에서 사용할 수 하 고 테스트 하 고 작업의 유효성을 검사 하려면 개발자가 사용 됩니다. 모든 해당 브라우저 UX 사용 하 여 개발자 커뮤니티의 대상 그룹 및 REST 끝점에는 것입니다. 장치 포털에 대 한 자세한 내용은 [Windows Device Portal 개요](device-portal.md)를 참조 하세요.

장치 포털에서 느슨한 파일 레이아웃을 등록 하려면 다음이 단계를 따르세요.

1. [Windows Device Portal 개요](device-portal.md)의 **설치** 섹션의 단계를 수행 하 여 디바이스 포털에 연결 합니다.
1. 앱 관리자 탭에서 **네트워크 공유에서 등록**을 선택 합니다.
1. 느슨한 파일 레이아웃을 네트워크 공유 경로 입력 합니다. 
1. 호스트 장치에 네트워크 공유에 액세스할 수 없으면 필요한 자격 증명을 입력 하 라는 메시지가 됩니다.
1. 등록 완료 되 면 앱을 시작할 수 있습니다.

장치 포털의 앱 관리자 페이지에서 **선택적 패키지를 지정 하 고** 확인란을 선택 하 고 다음 선택적 앱의 네트워크 공유 경로 지정 하 여 메인 앱에 대 한 선택적 느슨한 파일 레이아웃을도 등록할 수 있습니다. 

### <a name="powershell"></a>PowerShell 

또한 Windows PowerShell을 사용 하면 로컬 장치에만 느슨한 파일 레이아웃을 등록할 수 있습니다. 원격 장치에 레이아웃을 등록 해야 하는 경우 다른 메서드 중 하나를 사용 해야 합니다. 

느슨한 파일 레이아웃을 등록 하려면 PowerShell을 시작 하 고 다음을 입력 합니다.

```PowerShell
Add-AppxPackage -Register <path to manifest file>
```

## <a name="troubleshooting"></a>문제 해결

### <a name="mapped-network-drives"></a>매핑된 네트워크 드라이브
현재 매핑된 네트워크 드라이브 느슨한 파일 등록에 대 한 지원 되지 않습니다. 네트워크 공유 경로 전체를 사용 하 여 매핑된 드라이브를 참조 하세요.

### <a name="registration-failure"></a>등록 오류
장치는 등록 끌어서 놓기 파일 레이아웃에 대 한 액세스 해야 합니다. 파일 레이아웃, 네트워크 공유에서 호스팅되는 경우 장치에 대 한 액세스 되었는지 확인 합니다. 

### <a name="modifications-to-visual-assets-arent-being-loaded-in-the-app"></a>앱의 시각적 자산에 대 한 수정 되지 로드할 
앱이 시작 시 해당 시각적 자산을 로드 합니다. 앱을 시작한 후 시각적 자산을 수정 된, 최신 변경 내용을 보려면 앱 다시 시작 해야 합니다.