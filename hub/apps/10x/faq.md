---
Description: Windows 10 배에 대 한 몇 가지 기본적인 개발자 질문에 대 한 답변을 알아보세요.
title: Windows 10 배 개발자 FAQ
ms.topic: article
ms.date: 06/02/2020
ms.localizationpriority: medium
ms.author: quradic
author: QuinnRadich
ms.openlocfilehash: 3ba14e33c098d3515522a9a5907065751fafba87
ms.sourcegitcommit: 13bda6040988461a61b1b5561fde2f7a54835ccd
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 06/03/2020
ms.locfileid: "84318239"
---
# <a name="windows-10x-developer-faq"></a>Windows 10 배 개발자 FAQ

> [!IMPORTANT]
> 최근 Windows 10 및 Windows 10 배의 우선 순위에 대 한 일부 변경 내용을 발표 했습니다.
> 이러한 공지에는 Windows 10 배 폼 팩터 우선 순위에 대 한 변경 내용이 포함 됩니다. [자세한 내용은 여기를 참조 하세요.](https://blogs.windows.com/windowsexperience/2020/05/04/accelerating-innovation-in-windows-10-to-meet-customers-where-they-are/)

Windows 10 배은 이중 화면 장치에서 사용 하도록 최적화 된 Windows 제품군의 제품 라인입니다. 개발자는 Windows 10 배 응용 프로그램을 최적화 하 고, 모바일 및 이중 화면 대상 그룹에 대 한 새로운 기능을 활용 하 고, 동일한 범위의 Windows 10 기능 및 풍부한 데스크톱 지원을 계속 제공 하 여 더 광범위 한 대상 그룹에 연결할 수 있습니다. Microsoft는 [2019 년 말에 Windows 10 배을 발표](https://blogs.windows.com/windowsexperience/2019/10/02/introducing-windows-10x-enabling-dual-screen-pcs-in-2020/#6qxkItE2XMPu24uw.97)했으며, 년 2020 말에 출시 하려고 노력 하 고 있습니다.

![Windows 10 배를 실행 하는 장치](images/windows-10x-devices.png)
 
*[시험판 제품이 표시 되 고, 시뮬레이션 된 화면이 변경 될 수 있습니다.]*

이중 화면 환경 및 Windows 10 배를 구축 하는 방법에 대 한 자세한 내용은 [Microsoft 365 개발 일](https://developer.microsoft.com/microsoft-365/virtual-events)또는 [이중 화면 개발자 문서](https://docs.microsoft.com/dual-screen/)에서 가상 세션을 확인 하세요. 요약 정보는 다음과 같은 몇 가지 질문에 대 한 답을 얻을 수 있습니다.

### <a name="how-is-this-different-from-developing-for-windows-10"></a>이는 Windows 10에 대 한 개발과 어떻게 다른 가요?

대다수 응용 프로그램의 경우에는 전혀 다르지 않습니다. Windows 10 배 apps 작성은 Windows 10 SDK를 통해 지원 됩니다. Windows 10의 식으로 Windows 10 배는 WinRT (Windows 런타임) Api를 지원 하 고 네이티브 컨테이너를 통해 Win32 앱을 실행 합니다. 새 또는 기존 UWP 또는 Win32 앱에서 이중 화면 장치용으로 특별히 설계 된 새 Api를 호출 하 여이 새 플랫폼의 기능과 이점에 액세스할 수 있습니다.

### <a name="does-this-replace-desktop-windows-10"></a>데스크톱 Windows 10을 대체 하나요?

아니요. Windows 10 배는 Windows 10의 데스크톱 릴리스와 병렬로 출시 됩니다. Windows 10의 데스크톱 릴리스는 최신 데스크톱 응용 프로그램 스토리의 향상 된 기능과 향상 된 기능을 계속 제공 합니다. Windows 10 배는 이중 화면 플랫폼을 지원 하도록 최적화 된 또 다른 플랫폼입니다.

### <a name="when-will-windows-10x-be-released"></a>Windows 10 배 언제 출시 되나요?

Windows 10 배는 Surface Neo 및 기타 타사 이중 화면 장치와 함께 제공 되는 2020에 출시 됩니다.

### <a name="when-can-i-start-development-for-windows-10x"></a>Windows 10 배 개발을 시작할 수 있는 시기는 언제 인가요?

지금 [Microsoft 에뮬레이터 및 Windows 10 배 에뮬레이터 이미지](https://docs.microsoft.com/dual-screen/windows/get-dev-tools) 를 다운로드할 수 있습니다. Microsoft는이 에뮬레이터를 계속 개선 하 고 다른 Windows 10 배 사용 장치를 지 원하는 기능을 보완 합니다. Windows SDK 시험판 버전과 결합 된 이러한 에뮬레이터를 사용 하면 첫 번째 이중 화면 장치를 공개적으로 출시 하기 전에 Windows 10 배을 개발할 수 있습니다.

### <a name="will-my-universal-windows-platform-uwp-apps-run-on-windows-10x"></a>UWP (내 유니버설 Windows 플랫폼) 앱이 Windows 10 배에서 실행 되나요?

대부분의 UWP 앱은 Windows 10 배에서 완벽 하 게 지원 되며 Windows 10 배를 실행 하는 장치에서 기능을 변경 하지 않고 작동 합니다. 모든 WinRT Api는 UWP 앱에서 액세스할 수 있는 대부분의 다른 기능을 지원 합니다. 시험판 개발이 계속 되 면 지원 되지 않는 다른 기능을 자세히 설명 하는 설명서를 출시할 예정입니다.

### <a name="will-my-win32-apps-run-on-windows-10x"></a>내 Win32 앱이 Windows 10 배에서 실행 되나요?

Windows 10 배는 포함 된 환경에서 Win32 앱을 실행할 수 있는 기본 지원을 제공 합니다. 대부분의 Win32 앱은 인시던트 없이 Windows 10 배 장치에서 실행 및 디버그할 수 있으며, 새 Win32 Api를 사용 하 여 앱에 이중 화면 지원을 추가할 수도 있습니다.

### <a name="are-there-any-features-of-my-app-that-wont-work-on-windows-10x"></a>Windows 10 배에서 작동 하지 않는 내 앱의 기능이 있나요?

Windows 10 배 시험판 개발을 계속 진행 하면서 특정 제한 사항을 강조 표시 하는 설명서를 출시할 예정입니다. 그러나 Win32 앱을 실행 하는 데 사용 되는 포함 된 환경에는 Windows 셸이 포함 되지 않으므로 셸 확장과 이와 유사한 기능이 지원 되지 않습니다. 마찬가지로, Windows 10 배 장치는 전원 사용 옵션과 같은 특정 시스템 설정과 관련 된 Api를 지원 하지 않습니다.

### <a name="if-i-enhance-my-app-with-windows-10x-features-will-it-still-run-on-devices-running-desktop-windows-10"></a>Windows 10 배 기능을 사용 하 여 앱을 개선 하는 경우 데스크톱 Windows 10을 실행 하는 장치에서 계속 실행 되나요?

Windows 10 배 용으로 설계 된 앱은 데스크톱 버전의 Windows 10을 실행 하는 장치에서 계속 작동 하지만 이러한 새 Windows Api는 다음 주 버전 업데이트까지 Windows 10의 데스크톱 버전에 추가 되지 않습니다. 여러 버전의 데스크톱 Windows 10에서 지원 되는 앱을 개발 하는 것 처럼, [적응 코딩 모범 사례](https://docs.microsoft.com/windows/uwp/debug-test-perf/version-adaptive-code) 에 따라 앱이 10 배 및 desktop windows 10 모두에서 정상적으로 작동 하는지 확인 합니다. 
