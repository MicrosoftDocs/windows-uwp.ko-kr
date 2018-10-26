---
title: x86 데스크톱 앱의 문제 해결
description: ARM을 기반으로 실행되는 경우 x86 앱 관련 일반적인 문제 및 이를 해결하는 방법.
ms.author: misatran
ms.date: 05/09/2018
ms.topic: article
keywords: windows 10 s, 항상 연결, x86 ARM 기반 에뮬레이션, 문제 해결
ms.localizationpriority: medium
ms.openlocfilehash: 01ef13f6f27b45a4cc41244e4ebed0a54804fc8e
ms.sourcegitcommit: 6cc275f2151f78db40c11ace381ee2d35f0155f9
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/26/2018
ms.locfileid: "5553210"
---
# <a name="troubleshooting-x86-desktop-apps"></a>x86 데스크톱 앱의 문제 해결
>[!IMPORTANT]
> 이제 ARM64 SDK를 Visual Studio 15.8 미리 보기 1의 일부로 사용할 수 있습니다. 앱이 최대 기본 속도로 실행되도록 ARM64에 앱을 다시 컴파일하는 것이 좋습니다. 자세한 내용은 [ARM 개발에서 Windows 10에 대한 Visual Studio 지원 초기 미리 보기](https://blogs.windows.com/buildingapps/2018/05/08/visual-studio-support-for-windows-10-on-arm-development/) 블로그 게시물을 참조하세요.

x86 데스크톱 앱 x86 컴퓨터에서와 다르게 작동하는 경우 문제 해결에 도움이 되는 몇 가지 지침은 다음과 같습니다.

|문제|해결 방법|
|-----|--------|
| 앱은 ARM용으로 고안되지 않은 드라이버를 사용합니다. | x86 드라이버를 ARM64에 다시 컴파일합니다. [WDK를 사용하여 ARM64 드라이버 빌드](https://docs.microsoft.com/en-us/windows-hardware/drivers/develop/building-arm64-drivers)를 참조하세요. |
| 앱은 x64에서만 사용할 수 있습니다. | Microsoft Store용으로 개발하는 경우 앱의 ARM 버전을 제출하세요. 자세한 내용은 [앱 패키지 아키텍처](../packaging/device-architecture.md)를 참조하세요. Win32 개발자는 앱을 ARM64에 다시 컴파일하는 것이 좋습니다. 자세한 내용은 [ARM 개발에서 Windows 10에 대한 Visual Studio 지원 초기 미리 보기](https://blogs.windows.com/buildingapps/2018/05/08/visual-studio-support-for-windows-10-on-arm-development/)를 참조하세요. |
| 앱은 1.1 이상의 OpenGL 버전을 사용하거나 하드웨어 가속 OpenGL이 필요합니다. | 사용 가능한 경우 앱의 DirectX 모드를 사용합니다. DirectX 9, DirectX 10, DirectX 11, DirectX 12를 사용하는 x86 앱은 ARM에서 작동합니다. 자세한 내용은 [DirectX 그래픽 및 게임](https://msdn.microsoft.com/en-us/library/windows/desktop/ee663274(v=vs.85).aspx)을 참조하세요. |
| x86 앱이 예상대로 작동하지 않습니다. | [ARM의 프로그램 호환성 문제 해결사](apps-on-arm-program-compat-troubleshooter.md)의 지침을 따라 호환성 문제 해결사 사용을 시도해 보세요. 몇 가지 문제 해결 단계는 [ARM의 x86 앱 문제 해결](apps-on-arm-troubleshooting-x86.md) 문서를 참조하세요. |

## <a name="best-practices-for-wow"></a>WOW 모범 사례
앱이 WOW에서 실행되는 것을 발견하고 x64 시스템에 있는 것으로 가정할 때 한 가지 일반적인 문제점이 발생합니다. 이렇게 가정했으면 앱은 다음을 수행할 수 있습니다.

- ARM에서 지원되지 않는 자체의 x64 버전을 설치하려고 시도합니다.
- 기본 레지스트리 보기에서 다른 소프트웨어를 확인합니다.
- 64비트 .NET Framework를 사용할 수 있는 것으로 가정합니다.

일반적으로 앱은 WOW에서 실행되는지 결정할 때 호스트 시스템에 대해 가정하지 않아야 합니다. 가능한 한 운영 체제의 기본 구성 요소와 상호 작용하지 않도록 합니다.

앱은 기본 레지스트리 보기에서 레지스트리 키를 배치하거나 WOW의 존재 여부에 따라 기능을 수행할 수 있습니다. 원래 **IsWow64Process**는 앱이 x64 컴퓨터에서 실행되고 있는지 여부만 나타냅니다. 앱은 이제 [IsWow64Process2](https://msdn.microsoft.com/en-us/library/windows/desktop/mt804318(v=vs.85).aspx)를 사용하여 WOW를 지원하는 시스템에서 실행 중인지 여부를 확인해야 합니다. 

## <a name="drivers"></a>드라이버 
모든 커널 모드 드라이버, [사용자 모드 드라이버 프레임워크(UMDF)](https://docs.microsoft.com/windows-hardware/drivers/wdf/overview-of-the-umdf) 드라이버 및 인쇄 드라이버가 운영 체제의 아키텍처와 일치하도록 컴파일되어야 합니다. x86 앱에 드라이버가 있는 경우 그 드라이버를 ARM64에 대해 다시 컴파일해야 합니다. x86 앱은 에뮬레이션 하에 잘 실행될지도 모르지만 그 드라이버는 ARM64에 대해 다시 컴파일해야 하며 해당 드라이버를 사용하는 모든 앱 환경을 사용할 수 없게 됩니다. ARM64에 대한 드라이버를 컴파일하는 것에 대한 자세한 내용은 [WDK를 사용하여 ARM64 드라이버 빌드](https://docs.microsoft.com/windows-hardware/drivers/develop/building-arm64-drivers)를 참조하세요.

## <a name="shell-extensions"></a>셸 확장 
Windows 구성 요소를 연결하거나 DLL을 Windows 프로세스에 로드하려고 시도하는 앱은 시스템의 아키텍처, 즉, ARM64와 일치하도록 DLL을 다시 컴파일해야 합니다. 일반적으로 입력기(IME), 보조 기술 및 셸 확장 앱(예: 탐색기에서 클라우드 저장소 아이콘 표시 또는 컨텍스트 메뉴 오른쪽 마우스로 클릭)에 의해 사용됩니다. 앱 또는 DLL을 ARM64에 다시 컴파일하는 방법을 알아보려면 [ARM 개발에서 Windows 10에 대한 Visual Studio 지원 초기 미리 보기](https://blogs.windows.com/buildingapps/2018/05/08/visual-studio-support-for-windows-10-on-arm-development/) 블로그 게시물을 참조하세요. 

## <a name="debugging"></a>디버깅
앱의 동작에 대해 더 깊이 탐구하려면 [ARM 기반 디버깅](https://docs.microsoft.com/en-us/windows-hardware/drivers/debugger/debugging-arm64)을 참조하여 ARM 기반 디버깅을 위한 도구 및 전략에 대해 자세한 정보를 알아보세요.

## <a name="virtual-machines"></a>가상 컴퓨터
Windows 하이퍼바이저 플랫폼은 Qualcomm Snapdragon 835 모바일 PC 플랫폼에서 지원되지 않습니다. 따라서 Hyper-V를 사용하여 가상 컴퓨터를 실행할 수 없습니다. 현재 추후 Qualcomm 칩셋에 대한 기술에 계속 투자하고 있습니다. 