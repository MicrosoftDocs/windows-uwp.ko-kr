---
title: X86 데스크톱 응용 프로그램 문제 해결
description: 드라이버, 셸 확장 및 디버깅에 대 한 정보를 포함 하 여 ARM64에서 실행 되는 x86 데스크톱 앱과 관련 된 일반적인 문제를 해결 하는 방법에 대해 알아봅니다.
ms.date: 05/09/2018
ms.topic: article
keywords: windows 10 s, 항상 연결 됨, ARM의 x86 에뮬레이션, 문제 해결
ms.localizationpriority: medium
ms.openlocfilehash: 91e142eedc54e6c05f4bbb51e49eb8e516411b48
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89155417"
---
# <a name="troubleshooting-x86-desktop-apps"></a>X86 데스크톱 응용 프로그램 문제 해결
>[!IMPORTANT]
> ARM64 SDK는 이제 Visual Studio 15.8 Preview 1의 일부로 제공 됩니다. 앱이 전체 기본 속도로 실행 되도록 앱을 ARM64에 다시 컴파일하는 것이 좋습니다. 자세한 내용은 [ARM 개발의 Windows 10에 대 한 Visual Studio 지원의 초기 미리 보기](https://blogs.windows.com/buildingapps/2018/05/08/visual-studio-support-for-windows-10-on-arm-development/) 블로그 게시물을 참조 하세요.

X86 데스크톱 앱이 x86 컴퓨터에서 수행 하는 방식으로 작동 하지 않는 경우 문제를 해결 하는 데 도움이 되는 몇 가지 지침은 다음과 같습니다.

|문제|해결 방법|
|-----|--------|
| 앱은 ARM 용으로 설계 되지 않은 드라이버를 사용 합니다. | ARM64에 x86 드라이버를 다시 컴파일합니다. [WDK를 사용 하 여 ARM64 드라이버 빌드](/windows-hardware/drivers/develop/building-arm64-drivers)를 참조 하세요. |
| 앱은 x 64에만 사용할 수 있습니다. | Microsoft Store 용으로 개발 하는 경우 ARM 버전의 앱을 제출 합니다. 자세한 내용은 [앱 패키지 아키텍처](/windows/msix/package/device-architecture)를 참조 하세요. Win32 개발자 인 경우 ARM64에 앱을 다시 컴파일하는 것이 좋습니다. 자세한 내용은 [ARM 개발의 Windows 10에 대 한 Visual Studio 지원의 초기 미리 보기](https://blogs.windows.com/buildingapps/2018/05/08/visual-studio-support-for-windows-10-on-arm-development/)를 참조 하세요. |
| 앱이 1.1 이후의 OpenGL 버전을 사용 하거나 하드웨어 가속 OpenGL을 요구 합니다. | 앱의 DirectX 모드를 사용 합니다 (사용 가능한 경우). DirectX 9, DirectX 10, DirectX 11 및 DirectX 12를 사용 하는 x86 앱은 ARM에서 작동 합니다. 자세한 내용은 [DirectX Graphics And 게이밍](/windows/desktop/directx)를 참조 하세요. |
| X86 앱이 예상 대로 작동 하지 않습니다. | [ARM의 프로그램 호환성 문제 해결사](apps-on-arm-program-compat-troubleshooter.md)에서 지침에 따라 호환성 문제 해결사를 사용해 보세요. 다른 문제 해결 단계는 [ARM의 x86 앱 문제 해결](apps-on-arm-troubleshooting-x86.md) 문서를 참조 하세요. |

## <a name="best-practices-for-wow"></a>WOW 모범 사례
앱에서 WOW를 실행 하는 것을 검색 한 후 x64 시스템에 있다고 가정 하는 일반적인 문제 중 하나가 발생 합니다. 이러한 가정이 적용 되 면 앱에서 다음을 수행할 수 있습니다.

- ARM에서 지원 되지 않는 자체의 x64 버전을 설치 해 보세요.
- 네이티브 레지스트리 보기에서 다른 소프트웨어를 확인 합니다.
- 64 비트 .NET framework를 사용할 수 있다고 가정 합니다.

일반적으로 앱은 WOW에서 실행 되도록 결정 되 면 호스트 시스템에 대 한 가정을 하지 않아야 합니다. 가능한 한 OS의 네이티브 구성 요소와 상호 작용 하지 않도록 합니다.

앱은 기본 레지스트리 보기 아래에 레지스트리 키를 추가 하거나 WOW 유무에 따라 함수를 수행할 수 있습니다. 원래 **IsWow64Process**  는 응용 프로그램이 x64 컴퓨터에서 실행 되 고 있는지만 나타냅니다. 앱은 이제 [IsWow64Process2](/windows/desktop/api/wow64apiset/nf-wow64apiset-iswow64process2) 을 사용 하 여 WOW를 지 원하는 시스템에서 실행 되 고 있는지 확인 해야 합니다. 

## <a name="drivers"></a>드라이버 
모든 커널 모드 드라이버, [UMDF (사용자 모드 드라이버 프레임 워크)](/windows-hardware/drivers/wdf/overview-of-the-umdf) 드라이버 및 인쇄 드라이버는 OS 아키텍처와 일치 하도록 컴파일해야 합니다. X86 앱에 드라이버가 있는 경우 ARM64에 대해 해당 드라이버를 다시 컴파일해야 합니다. X86 앱은 에뮬레이션에서 제대로 실행 될 수 있으며, ARM64에 대해 드라이버를 다시 컴파일해야 하며, 드라이버에 종속 된 앱 환경을 사용할 수 없게 됩니다. ARM64 드라이버를 컴파일하는 방법에 대 한 자세한 내용은 [WDK를 사용 하 여 ARM64 드라이버 빌드](/windows-hardware/drivers/develop/building-arm64-drivers)를 참조 하세요.

## <a name="shell-extensions"></a>셸 확장 
Windows 구성 요소를 후크 하거나 해당 Dll을 Windows 프로세스로 로드 하려는 앱은 시스템의 아키텍처와 일치 하도록 해당 Dll을 다시 컴파일해야 합니다. 예: ARM64. 일반적으로 이러한 메서드는 Ime (입력기), 보조 기술 및 셸 확장 앱에서 사용 됩니다. 예를 들어, 탐색기에 클라우드 저장소 아이콘을 표시 하거나 상황에 맞는 메뉴를 마우스 오른쪽 단추로 클릭 합니다. ARM64에 앱 또는 Dll을 다시 컴파일하는 방법에 대 한 자세한 내용은 [ARM 개발의 Windows 10에 대 한 Visual Studio 지원의 초기 미리 보기](https://blogs.windows.com/buildingapps/2018/05/08/visual-studio-support-for-windows-10-on-arm-development/) 블로그 게시물을 참조 하세요. 

## <a name="debugging"></a>디버깅
응용 프로그램의 동작을 더 자세히 조사 하려면 arm에서 [디버깅](/windows-hardware/drivers/debugger/debugging-arm64) 을 참조 하 여 arm에서 디버깅 하기 위한 도구 및 전략에 대해 자세히 알아보세요.

## <a name="virtual-machines"></a>Virtual Machines
Qualcomm Snapdragon 835 Mobile PC 플랫폼에서는 Windows 하이퍼바이저 플랫폼이 지원 되지 않습니다. 따라서 Hyper-v를 사용 하 여 실행 중인 가상 컴퓨터는 작동 하지 않습니다. 향후 Qualcomm 칩셋에는 이러한 기술에 계속 투자 하 고 있습니다. 

## <a name="dynamic-code-generation"></a>동적 코드 생성
X86 데스크톱 앱은 런타임에 ARM64 명령을 생성 하는 시스템에 의해 ARM64에서 에뮬레이션 됩니다. 즉, x86 데스크톱 앱이 동적 코드를 생성 하거나 프로세스에서 수정할 수 없도록 하는 경우 ARM64에서 x 86으로 실행 되도록 해당 앱을 지원할 수 없습니다. 

이는 보안을 완화 하기 위한 것입니다. 일부 앱은 플래그가 있는 [SetProcessMitigationPolicy](/windows/desktop/api/processthreadsapi/nf-processthreadsapi-setprocessmitigationpolicy) API를 사용 하 여 프로세스에서 사용 하도록 설정 `ProcessDynamicCodePolicy` 합니다. X86 프로세스로 ARM64에서 성공적으로 실행 하려면이 완화 정책을 사용 하지 않도록 설정 해야 합니다.