---
title: ARM의 앱 및 환경 제한
author: msatranjr
description: ARM에서 제대로 작동하지 않는 앱에 대한 문제 해결 단계입니다.
ms.author: misatran
ms.date: 02/15/2018
ms.topic: article
keywords: Windows 10 s, 항상 연결, 제한, ARM 기반 Windows10
ms.localizationpriority: medium
redirect_url: https://docs.microsoft.com/en-us/windows/uwp/porting/apps-on-arm-troubleshooting-x86
ms.openlocfilehash: 24afc8a876b976f21d0f4ebd5892ceef7c403018
ms.sourcegitcommit: 144f5f127fc4fbd852f2f6780ef26054192d68fc
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/02/2018
ms.locfileid: "5990146"
---
# <a name="limitations-of-apps-and-experiences-on-arm"></a>ARM의 앱 및 환경 제한
ARM 기반 Windows10은 다음과 같은 필수 제한 사항이 있습니다.

- **ARM64 드라이버만 지원됩니다**. 모든 아키텍처와 마찬가지로 커널 모드 드라이버, [사용자 모드 드라이버 프레임워크(UMDF)](https://docs.microsoft.com/en-us/windows-hardware/drivers/wdf/overview-of-the-umdf) 드라이버 및 인쇄 드라이버가 운영 체제의 아키텍처와 일치하도록 컴파일되어야 합니다. ARM OS에는 x86 사용자 모드 앱을 에뮬레이션하기 위한 기능이 있지만 다른 아키텍처(예: x64 또는 x86)에 대해 구현된 드라이버는 현재 에뮬레이트되지 않으며 따라서 이 플랫폼에서 지원되지 않습니다. 사용자 지정 드라이버를 사용하는 모든 앱은 ARM64에 포트되어야 합니다. 제한된 시나리오에서 앱은 에뮬레이션 하에 x86으로 실행될 수 있지만 앱의 드라이버 부분을 ARM64에 포트해야 합니다. ARM64에 대한 드라이버를 컴파일하는 것에 대한 자세한 내용은 [WDK를 사용하여 ARM64 드라이버 빌드](https://review.docs.microsoft.com/en-us/windows-hardware/drivers/develop/building-arm64-drivers?branch=rs4-arm64)를 참조하세요.

- **x64 앱은 지원되지 않습니다**. ARM 기반 Windows10은 x64 앱의 에뮬레이션을 지원하지 않습니다.

- **일부 게임이 작동하지 않습니다**. 1.1 이후의 OpenGL 버전을 사용하거나 하드웨어 가속 OpenGL이 필요한 게임 및 앱이 작동하지 않습니다. 또한 "치트 방지" 드라이버를 사용하는 게임이 이 플랫폼에서 지원되지 않습니다.

- **Windows 환경을 사용자 지정하는 앱이 제대로 작동하지 않을 수 있습니다**. 기본 운영 체제의 구성 요소가 비 기본 구성 요소를 로드할 수 없습니다. 일반적으로 이 작업을 수행하는 앱의 예는 일부 입력기(IME), 보조 기술, 클라우드 저장소 앱이 있습니다. IME 및 보조 기술은 종종 많은 앱의 기능을 위해 입력 스택에 연결됩니다. 일반적으로 클라우드 저장소 앱은 셸 확장을 사용합니다(예: 탐색기의 아이콘 및 마우스 오른쪽 단추로 클릭 메뉴 추가). 셸 확장에 실패할 수 있으며 오류가 적절하게 처리되지 않는 경우 앱이 전혀 작동하지 않을 수 있습니다.

- **모든 ARM 기반 디바이스가 모바일 버전의 Windows에서 실행 중임을 가정하는 앱은 제대로 작동하지 않을 수 있습니다**. 이 가정을 한 앱은 먼저 계약 가용성을 테스트하지 않고 모바일 전용 API를 호출하려고 할 때 잘못된 방향으로 나타날 수 있으며, 예기치 않게 UI 레이아웃 또는 렌더링을 표시하거나, 완전히 시작되지 않을 수 있습니다.

- **Windows 하이퍼바이저 플랫폼은 ARM에서 지원되지 않습니다**. ARM 장치에서 Hyper-V를 사용하여 가상 컴퓨터를 실행할 수 없습니다.

다음 표에서 몇 가지 일반적인 문제를 설명하고 이를 해결하는 방법에 대해 제안합니다.

|문제|해결 방법|
|-----|--------|
| 앱은 ARM용으로 고안되지 않은 드라이버를 사용합니다. | x86 드라이버를 ARM64에 다시 컴파일합니다. [WDK를 사용하여 ARM64 드라이버 빌드](https://docs.microsoft.com/en-us/windows-hardware/drivers/develop/building-arm64-drivers)를 참조하세요. |
| 앱은 x64에서만 사용할 수 있습니다. | Microsoft Store용으로 개발하는 경우 앱의 ARM 버전을 제출하세요. 자세한 내용은 [앱 패키지 아키텍처](../packaging/device-architecture.md)를 참조하세요. Win32 개발자는 경우 앱의 x86 버전을 배포합니다. |
| 앱은 1.1 이상의 OpenGL 버전을 사용하거나 하드웨어 가속 OpenGL이 필요합니다. | DirectX 9, DirectX 10, DirectX 11, DirectX 12를 사용하는 x86 앱은 ARM에서 작동합니다. 자세한 내용은 [DirectX 그래픽 및 게임](https://msdn.microsoft.com/en-us/library/windows/desktop/ee663274(v=vs.85).aspx)을 참조하세요. |
| x86 앱이 예상대로 작동하지 않습니다. | [ARM의 프로그램 호환성 문제 해결사](apps-on-arm-program-compat-troubleshooter.md)의 지침을 따라 호환성 문제 해결사 사용을 시도해 보세요. 몇 가지 문제 해결 단계는 [ARM의 x86 앱 문제 해결](apps-on-arm-troubleshooting-x86.md) 문서를 참조하세요. |
| x86 앱이 ARM에서 실행되고 있는지 감지하지 않습니다. | [IsWow64Process2](https://msdn.microsoft.com/en-us/library/windows/desktop/mt804318(v=vs.85).aspx)를 사용하여 앱이 ARM에서 실행되고 있는지 확인합니다. |
| UWP ARM32 앱이 예상대로 작동하지 않습니다. | [ARM의 ARM32 앱 문제 해결](apps-on-arm-troubleshooting-arm32.md)을 참조하여 앱이 ARM에서 제대로 작동하도록 하는 방법을 알아보세요. |
