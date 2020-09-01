---
title: ARM 기반 Windows 10
description: 이 문서에서는 ARM에서 경험과 앱이 실행 되는 방법, 제한 사항 및 자세히 알아볼 수 있는 위치에 대 한 개요를 제공 합니다.
ms.date: 05/22/2020
ms.topic: article
keywords: windows 10 s, 항상 연결 됨, ARM, ARM64, x86 에뮬레이션
ms.localizationpriority: medium
ms.openlocfilehash: 39ff5b2aa6c72feaeaea0a7a61100196c109257c
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89162297"
---
# <a name="windows-10-on-arm"></a>ARM 기반 Windows 10
원래 Windows 10 (Windows 10 Mobile과 구분 됨)은 x86 및 x64 프로세서로 구동 된 Pc 에서만 실행 될 수 있습니다. 이제 Windows 10 desktop은 ARM64 프로세서에서 구동 하는 컴퓨터에서이 컴퓨터에서 실행 될 수 있습니다. ARM CPU 아키텍처의 전력 절약 특성을 통해 이러한 Pc는 모든 배터리 수명 및 모바일 데이터 네트워크에 대 한 지원을 받을 수 있습니다. 이러한 Pc는 뛰어난 응용 프로그램 호환성을 제공 하며 기존 x86 win32 응용 프로그램을 수정 하지 않고 실행할 수 있습니다. 자세한 내용 또는 데모는 [채널 9 비디오에서 항상 연결 된 PC](https://channel9.msdn.com/Events/Build/2017/P4171)를 살펴보세요.

여기서 *ARM* 이라는 용어는 ARM64 (일반적으로 *AArch64*) 프로세서의 Windows 10 데스크톱 버전을 실행 하는 pc에 대 한 약어로 사용 됩니다.  여기서 *ARM32* 이라는 용어는 32 비트 arm 아키텍처 (일반적으로 다른 설명서에서 *ARM* 이라고 함)에 대 한 약어로 사용 됩니다.

## <a name="apps-and-experiences-on-arm"></a>ARM의 앱 및 환경

### <a name="built-in-windows-10-experiences-apps-and-drivers"></a>기본 제공 Windows 10 환경, 앱 및 드라이버
Edge, Cortana, 시작 메뉴 및 탐색기와 같은 기본 제공 Windows 10 환경은 모두 기본 제공 되 고 ARM64로 실행 됩니다. 여기에는 그래픽, 네트워킹, 하드 디스크 등의 모든 장치 드라이버도 포함 됩니다. 이렇게 하면 Qualcomm Snapdragon 프로세서의 전체 기본 속도로 실행 되는 장치에서 최적의 사용자 환경 및 배터리 수명을 얻을 수 있습니다.

### <a name="universal-windows-platform-uwp-apps"></a>UWP (유니버설 Windows 플랫폼) 앱
ARM의 Windows 10은 Microsoft Store에서 모든 x86, ARM32 및 ARM64 [UWP 앱](../get-started/universal-application-platform-guide.md) 을 실행 합니다. ARM32 및 ARM64 앱은 에뮬레이션 없이 기본적으로 실행 되지만 x86 앱은 에뮬레이션 하에서 실행 됩니다. UWP 개발자 인 경우에는 장치에 대 한 최상의 사용자 환경을 제공 하므로 앱에 대 한 ARM 패키지를 제출 해야 합니다. 자세한 내용은 [앱 패키지 아키텍처](/windows/msix/package/device-architecture)를 참조 하세요.

>[!NOTE]
> ARM64 플랫폼을 기본적으로 대상으로 하는 UWP 응용 프로그램을 빌드하려면 Visual Studio 2017 버전 15.9 이상 또는 Visual Studio 2019이 있어야 합니다. 자세한 내용은 [이 블로그 게시물](https://blogs.windows.com/buildingapps/2018/11/15/official-support-for-windows-10-on-arm-development)을 참조하세요.


>[!IMPORTANT]
> ARM의 Windows 10은 ARM64 장치의 저장소에서 x86, ARM32 및 ARM64 UWP 앱을 지원 합니다. 사용자가 ARM64 장치에서 UWP 앱을 다운로드 하면 OS는 사용 가능한 최적의 앱 버전을 자동으로 설치 합니다. 앱의 x86, ARM32 및 ARM64 버전을 스토어에 제출 하면 OS가 자동으로 앱의 ARM64 버전을 설치 합니다. X86 및 ARM32 버전의 앱만 제출 하는 경우 OS는 ARM32 버전을 설치 합니다. 응용 프로그램의 x86 버전을 제출 하는 경우 OS는 해당 버전을 설치 하 고 에뮬레이션을 통해 실행 합니다. 아키텍처에 대 한 자세한 내용은 [앱 패키지 아키텍처](/windows/msix/package/device-architecture)를 참조 하세요.

### <a name="win32-apps"></a>Win32 앱
UWP 앱 외에도 ARM의 Windows 10은 모든 PC와 마찬가지로 뛰어난 성능과 원활한 사용자 환경을 사용 하 여 x86 Win32 앱을 수정 하지 않고 실행할 수 있습니다. 이러한 x86 Win32 앱은 ARM을 위해 다시 컴파일하지 않아도 되며 ARM 프로세서에서 실행 되는 것을 인식 하지 못합니다. 64 비트 x64 Win32 앱은 지원 되지 않지만 대다수의 앱은 x86 버전을 사용할 수 있습니다.  앱 아키텍처가 선택 된 경우 ARM PC의 Windows 10에서 앱을 실행 하려면 32 비트 x86 버전을 선택 하면 됩니다.

## <a name="downloads"></a>다운로드

Visual Studio 2019는 ARM의 Windows 10에 대 한 몇 가지 도구 다운로드를 제공 합니다. Visual Studio 2017을 사용 하는 사용자 stil은 설치 관리자를 사용 하 여 유사한 도구와 패키지를 찾고 설치할 수 있습니다. 이러한 단계를 수행 하려면 Visual Studio 2019을 사용 해야 합니다.

### <a name="visual-c-redistributable"></a>Visual C++ 재배포 가능 패키지

ARM 앱에는 Visual C++ Redist 패키지를 사용할 수 있습니다. [Visual studio 다운로드 페이지](https://visualstudio.microsoft.com/downloads/) 를 방문 하 여 **모든 다운로드**까지 아래로 스크롤하고 **다른 도구 및 프레임 워크**를 연 다음 **Visual Studio 2019에 대 한 Microsoft Visual C++ 재배포 가능** 항목으로 이동 합니다. **ARM64** 라디오 단추를 선택한 다음 **다운로드**를 선택 합니다.

### <a name="remote-tools"></a>원격 도구

ARM 앱에는 Visual Studio용 원격 도구를 사용할 수 있습니다. [Visual studio 다운로드 페이지](https://visualstudio.microsoft.com/downloads/) 를 방문 하 여 **모든 다운로드**로 스크롤하고 **visual studio 2019 용 도구**를 연 다음 **Visual Studio용 원격 도구 2019** 항목으로 이동 합니다. **ARM64* 라디오 단추를 선택한 다음 **다운로드**를 선택 합니다.


## <a name="in-this-section"></a>섹션 내용
|항목 | Description |
|-----|-----|
|[ARM에서 x86 에뮬레이션이 작동하는 방식](apps-on-arm-x86-emulation.md)|X86 앱이 ARM에서 에뮬레이트하는 방식을 자세히 설명 합니다.|
|[ARM 기반 x86 앱의 문제 해결](apps-on-arm-troubleshooting-x86.md)|ARM에서 실행 하는 경우 x86 앱에 대 한 일반적인 문제 및 해결 방법 |
|[ARM에서 ARM 앱 문제 해결](apps-on-arm-troubleshooting-arm32.md)|ARM에서 실행 될 때 ARM32 및 ARM64 앱에 대 한 일반적인 문제 및 해결 방법 |
|[ARM의 프로그램 호환성 문제 해결사](apps-on-arm-program-compat-troubleshooter.md)|앱이 ARM에서 제대로 작동 하지 않는 경우 호환성 설정을 조정 하는 방법에 대 한 지침입니다. |

## <a name="related-topics"></a>관련 항목
|항목 | Description |
|-----|-----|
|[WDK를 사용하여 ARM64 드라이버 빌드](/windows-hardware/drivers/develop/building-arm64-drivers)|ARM64 드라이버를 구축 하는 방법에 대 한 지침입니다. |
| [ARM에서 x86 앱 디버그](/windows-hardware/drivers/debugger/debugging-arm64) | ARM에서 x86 앱을 디버깅 하는 방법에 대 한 지침입니다. |