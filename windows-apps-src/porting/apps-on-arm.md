---
title: ARM 기반 Windows 10
description: 이 문서에서는 환경 및 앱 ARM에서 실행되는 방식과 제한 사항이 무엇인지, 자세히 알아볼 수 있는 위치에 대한 개요를 제공합니다.
ms.date: 02/15/2018
ms.topic: article
keywords: windows 10 s, 항상 연결, ARM, ARM64, x86 에뮬레이션
ms.localizationpriority: medium
ms.openlocfilehash: 7450b469f77fec4288ad6dff01ee7673affc8dd9
ms.sourcegitcommit: f3c1a81b50f4a372a15996ac71b3f408a8ee1409
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/10/2019
ms.locfileid: "72237528"
---
# <a name="windows-10-on-arm"></a>ARM 기반 Windows 10
원래 Windows 10 Mobile과 달리 원래 Windows 10은 x86 및 x64 프로세서 기반 PC에서만 실행할 수 있었습니다. 이제 Windows 10 desktop은 ARM64 프로세서에서 구동 하는 컴퓨터에서이 컴퓨터에서 실행 될 수 있습니다. ARM CPU 아키텍처의 절전 특징은 하루 종일 배터리 사용 시간을 유지하고 모바일 데이터 네트워크를 지원할 수 있게 합니다. 이러한 PC는 훌륭한 응용 프로그램의 호환성을 제공하고 기존 x86 win32 응용 프로그램을 수정하지 않고 실행할 수 있도록 합니다. 자세한 내용 또는 데모는 [채널 9 비디오에서 항상 연결 된 PC](https://channel9.msdn.com/Events/Build/2017/P4171)를 살펴보세요.

여기에서는 데스크톱 버전 ARM64(흔히 *AArch64*라고도 함) 프로세서 기반 Windows 10을 실행하는 PC를 짧게 칭하여 *ARM*이라고 부릅니다.  여기에서는 32비트 ARM 아키텍처(다른 설명서에서 일반적으로 *ARM*이라고 함)를 줄여 *ARM32*라는 용어를 사용합니다.

## <a name="apps-and-experiences-on-arm"></a>ARM 기반 앱 및 환경

### <a name="built-in-windows-10-experiences-apps-and-drivers"></a>기본 제공 Windows 10 환경, 앱 및 드라이버
Edge, Cortana, 시작 메뉴 및 탐색기와 같은 기본 제공 Windows 10 환경은 모두 기본 제공 되 고 ARM64로 실행 됩니다. 여기에는 그래픽, 네트워킹, 하드 디스크 등의 모든 장치 드라이버도 포함 됩니다. 이렇게 하면 Qualcomm Snapdragon 프로세서의 전체 기본 속도로 실행 되는 장치에서 최적의 사용자 환경 및 배터리 수명을 얻을 수 있습니다.

### <a name="universal-windows-platform-uwp-apps"></a>UWP(유니버설 Windows 플랫폼) 앱
ARM의 Windows 10은 Microsoft Store에서 모든 x86, ARM32 및 ARM64 [UWP 앱](../get-started/universal-application-platform-guide.md) 을 실행 합니다. ARM32 및 ARM64 앱은 에뮬레이션 없이 기본적으로 실행 되지만 x86 앱은 에뮬레이션 하에서 실행 됩니다. UWP 개발자인 경우 이 디바이스에 대한 최적의 사용자 환경을 제공하므로 앱에 대한 ARM 패키지를 제출할 수 있는지 확인하십시오. 자세한 내용은 [앱 패키지 아키텍처](/windows/msix/package/device-architecture)를 참조하세요.

>[!NOTE]
> ARM64 플랫폼을 기본적으로 대상으로 하는 UWP 응용 프로그램을 빌드하려면 Visual Studio 2017 버전 15.9 이상 또는 Visual Studio 2019이 있어야 합니다. 자세한 내용은 [이 블로그 게시물](https://blogs.windows.com/buildingapps/2018/11/15/official-support-for-windows-10-on-arm-development)을 참조 하세요.


>[!IMPORTANT]
> x86 버전 이외도 사용할 수 있는 경우 사용자가 Microsoft Store에서 UWP 앱을 다운로드하면 ARM64 장치에 ARM32 버전이 설치됩니다. 아키텍처에 대한 자세한 내용은 [앱 패키지 아키텍처](/windows/msix/package/device-architecture)를 참조하세요.

### <a name="win32-apps"></a>Win32 앱
UWP 앱 외에도 ARM의 Windows 10은 모든 PC와 마찬가지로 뛰어난 성능과 원활한 사용자 환경을 사용 하 여 x86 Win32 앱을 수정 하지 않고 실행할 수 있습니다. 이러한 x86 Win32 앱은 ARM을 위해 다시 컴파일하지 않아도 되며 ARM 프로세서에서 실행 되는 것을 인식 하지 못합니다. 64 비트 x64 Win32 앱은 지원 되지 않지만 대다수의 앱은 x86 버전을 사용할 수 있습니다.  앱 아키텍처가 선택 된 경우 ARM PC의 Windows 10에서 앱을 실행 하려면 32 비트 x86 버전을 선택 하면 됩니다.

## <a name="in-this-section"></a>단원 내용
|항목 | 설명 |
|-----|-----|
|[ARM에서 x86 에뮬레이션이 작동하는 방식](apps-on-arm-x86-emulation.md)|x86 앱이 ARM에셔 에뮬레이션되는 방식을 설명하는 개요.|
|[ARM 기반 x86 앱의 문제 해결](apps-on-arm-troubleshooting-x86.md)|ARM을 기반으로 실행되는 경우 x86 앱 관련 일반적인 문제 및 이를 해결하는 방법. |
|[ARM에서 ARM 앱 문제 해결](apps-on-arm-troubleshooting-arm32.md)|ARM에서 실행 될 때 ARM32 및 ARM64 앱에 대 한 일반적인 문제 및 해결 방법 |
|[ARM의 프로그램 호환성 문제 해결사](apps-on-arm-program-compat-troubleshooter.md)|앱이 ARM에서 제대로 작동하지 않는 경우 호환성 설정 조정에 대한 지침. |

## <a name="related-topics"></a>관련 항목
|항목 | 설명 |
|-----|-----|
|[WDK를 사용하여 ARM64 드라이버 빌드](https://docs.microsoft.com/en-us/windows-hardware/drivers/develop/building-arm64-drivers)|ARM64 드라이버를 구축하기 위한 지침. |
| [ARM에서 x86 앱 디버그](https://docs.microsoft.com/en-us/windows-hardware/drivers/debugger/debugging-arm64) | ARM 기반 x86 앱을 디버깅하기 위한 지침. |
