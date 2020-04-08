---
title: 느슨한 파일 등록을 통해 앱 배포
description: 이 가이드에서는 느슨한 파일 레이아웃을 사용하여 Windows 10 앱을 패키지하지 않고도 유효성을 검사하고 공유하는 방법을 보여 줍니다.
ms.date: 06/01/2018
ms.topic: article
keywords: windows 10, uwp, device portal, 앱 관리자, 배포, sdk
ms.localizationpriority: medium
ms.openlocfilehash: 7bf3dab97be67a3b97aca4b3132bd9fe18691d15
ms.sourcegitcommit: 26bb75084b9d2d2b4a76d4aa131066e8da716679
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 01/06/2020
ms.locfileid: "75681934"
---
# <a name="deploy-an-app-through-loose-file-registration"></a>느슨한 파일 등록을 통해 앱 배포 

이 가이드에서는 느슨한 파일 레이아웃을 사용하여 Windows 10 앱을 패키지하지 않고도 유효성을 검사하고 공유하는 방법을 보여 줍니다. 느슨한 파일 레이아웃을 등록하면 개발자가 앱을 패키지 및 설치할 필요 없이 신속하게 앱의 유효성을 검사할 수 있습니다. 

## <a name="what-is-a-loose-file-layout"></a>느슨한 파일 레이아웃이란?

느슨한 파일 레이아웃은 패키지 프로세스를 진행하는 대신, 폴더에 앱 콘텐츠를 배치하는 작업입니다. 패키지 콘텐츠는 폴더에서 "느슨하게" 사용할 수 있으며 패키지되지 않습니다. 

> [!WARNING]
> 느슨한 파일 레이아웃 등록은 개발자와 디자이너가 활성 개발 중에 앱의 유효성을 신속하게 검사할 수 있도록 하기 위한 것입니다. 이 접근 방식은 앱을 "시험 사용"하거나 플라이트하는 데 사용할 수 없습니다. 최종 유효성 검사는 신뢰할 수 있는 인증서로 서명된 패키지된 앱에서 수행하는 것이 좋습니다. 

## <a name="advantages-of-loose-file-registration"></a>느슨한 파일 등록의 이점

- **빠른 유효성 검사** - 앱 파일이 이미 압축 해제되었으므로 사용자는 느슨한 파일 레이아웃을 신속하게 등록하고 앱을 시작할 수 있습니다. 일반 앱과 마찬가지로 사용자는 디자인된 앱을 사용할 수 있습니다. 
- **손쉬운 네트워크 내 배포** - 느슨한 파일이 로컬 드라이브가 아닌 네트워크 공유에 있는 경우 개발자는 네트워크 공유 위치를 네트워크에 액세스할 수 있는 다른 사용자에게 보낼 수 있으며 느슨한 파일 레이아웃을 등록하고 앱을 실행할 수 있습니다. 이를 통해 여러 사용자가 동시에 앱의 유효성을 검사할 수 있습니다. 
- **공동 작업** - 느슨한 파일 등록을 사용하면 개발자와 디자이너가 앱을 등록하는 동안 시각적 자산에 대한 작업을 계속할 수 있습니다. 사용자는 앱을 시작할 때 이러한 변경 내용을 볼 수 있습니다. 이러한 방식으로 정적 자산만 변경할 수 있습니다. 코드 또는 동적으로 만든 콘텐츠를 수정해야 하는 경우 앱을 다시 컴파일해야 합니다.

## <a name="how-to-register-a-loose-file-layout"></a>느슨한 파일 레이아웃을 등록하는 방법

Windows는 로컬 및 원격 디바이스에서 느슨한 파일 레이아웃을 등록하기 위한 여러 개발자 도구를 제공합니다. `WinDeployAppCmd`(Windows SDK 도구), Windows Device Portal, PowerShell 및 [Visual Studio](https://docs.microsoft.com/windows/uwp/debug-test-perf/deploying-and-debugging-uwp-apps#register-layout-from-network) 중에서 선택할 수 있습니다. 다음에서는 이러한 도구를 사용하여 느슨한 파일을 등록하는 방법을 설명합니다. 그러나 먼저 다음 설치를 수행했는지 확인합니다.

- 디바이스가 Windows 10 크리에이터 업데이트(빌드 14965) 이상에 있어야 합니다.
- 모든 디바이스에서 [개발자 모드](https://docs.microsoft.com/windows/uwp/get-started/enable-your-device-for-development) 및 [디바이스 검색](https://docs.microsoft.com/windows/uwp/get-started/enable-your-device-for-development#device-discovery)을 사용하도록 설정해야 합니다.

> [!IMPORTANT]
> 느슨한 파일 등록은 네트워크 공유(SMB) 프로토콜을 지원하는 디바이스인 데스크톱 및 Xbox에서만 사용할 수 있습니다. 

### <a name="register-with-windeployappcmd"></a>WinDeployAppCmd에 등록

Windows 10 크리에이터 업데이트(빌드 14965) 이상에 해당하는 SDK 도구를 사용하는 경우 명령 프롬프트에서 `WinDeployAppCmd` 명령을 사용할 수 있습니다.

```cmd
WinAppDeployCmd.exe registerfiles -remotedeploydir <Network Path> -ip <IP Address> -pin <target machine PIN>
```

**네트워크 경로** – 앱의 느슨한 파일 경로

**IP 주소** – 대상 머신의 IP 주소

**대상 머신 PIN** - 대상 디바이스에 대한 연결을 설정하는 데 필요한 PIN입니다. 인증이 필요한 경우 `-pin` 옵션을 사용하여 재시도하라는 메시지가 나타납니다. PIN을 가져오는 방법에 대한 자세한 내용은 [디바이스 검색](https://docs.microsoft.com/windows/uwp/get-started/enable-your-device-for-development#device-discovery)을 참조하세요.

### <a name="windows-device-portal"></a>Windows Device Portal

Windows Device Portal은 모든 Windows 10 디바이스에서 사용할 수 있으며 개발자가 작업을 테스트하고 유효성을 검사하는 데 사용합니다. 이 포털은 브라우저 UX 및 REST 엔드포인트를 사용하여 개발자 커뮤니티의 모든 대상에게 적절한 환경을 제공합니다. Device Portal에 대한 자세한 내용은 [Windows Device Portal 개요](device-portal.md)를 참조하세요.

Device Portal에서 느슨한 파일 레이아웃을 등록하려면 다음 단계를 수행합니다.

1. [Windows Device Portal 개요](device-portal.md)의 **설정** 섹션에 나오는 단계를 수행하여 Device Portal에 연결합니다.
1. 앱 관리자 탭에서 **네트워크 공유에서 등록**을 선택합니다.
1. 느슨한 파일 레이아웃의 네트워크 공유 경로를 입력합니다. 
1. 호스트 디바이스에서 네트워크 공유에 액세스할 수 없는 경우 필요한 자격 증명을 입력하라는 메시지가 표시됩니다.
1. 등록이 완료되면 앱을 시작할 수 있습니다.

Device Portal의 앱 관리자 페이지에서 **선택적 패키지 지정** 확인란을 선택하고 선택적 앱의 네트워크 공유 경로를 지정하여 기본 앱에 대한 선택적 느슨한 파일 레이아웃을 등록할 수도 있습니다. 

### <a name="powershell"></a>PowerShell 

Windows PowerShell을 사용하여 느슨한 파일 레이아웃을 등록할 수도 있지만 로컬 디바이스에만 등록할 수 있습니다. 원격 디바이스에 레이아웃을 등록해야 하는 경우에는 다른 방법 중 하나를 사용해야 합니다. 

느슨한 파일 레이아웃을 등록하려면 PowerShell을 시작하고 다음을 입력합니다.

```PowerShell
Add-AppxPackage -Register <path to manifest file>
```

## <a name="troubleshooting"></a>문제 해결

### <a name="mapped-network-drives"></a>연결된 네트워크 드라이브
현재, 매핑된 네트워크 드라이브는 느슨한 파일 등록이 지원되지 않습니다. 전체 네트워크 공유 경로를 사용하여 매핑된 드라이브를 참조합니다.

### <a name="registration-failure"></a>등록 오류
등록을 수행하는 디바이스에는 파일 레이아웃에 대한 액세스 권한이 있어야 합니다. 파일 레이아웃이 네트워크 공유에서 호스트되는 경우 디바이스에서 해당 위치에 액세스할 수 있는지 확인합니다. 

### <a name="modifications-to-visual-assets-arent-being-loaded-in-the-app"></a>시각적 자산에 대한 수정 내용이 앱에 로드되지 않음 
앱은 시작 시 시각적 자산을 로드합니다. 앱을 시작한 후 시각적 자산이 수정된 경우 최신 변경 내용을 보려면 앱을 다시 시작해야 합니다.
