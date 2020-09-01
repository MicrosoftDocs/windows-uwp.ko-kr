---
title: 질문과 대답
description: 작업이 예상 대로 작동 하지 않는 경우 Xbox에서 UWP에 대 한 질문과 대답 페이지를 참조 하세요.
ms.date: 03/29/2017
ms.topic: article
keywords: windows 10, uwp
ms.assetid: 265fe827-bd4a-48d4-b362-8793b9b25705
ms.localizationpriority: medium
ms.openlocfilehash: 7da3b6a6507f2652eb2357ab80897d9ba35d7484
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89170737"
---
# <a name="frequently-asked-questions"></a>질문과 대답

예상 대로 작동 하지 않는 것은 무엇 인가요? 자주 묻는 질문과 대답 페이지를 살펴봅니다. 또한 [알려진 문제](known-issues.md) 항목 및 [유니버설 Windows 앱 개발](https://social.msdn.microsoft.com/Forums/windowsapps/en-US/home?forum=wpdevelop) 포럼을 확인 하세요. 

### <a name="why-arent-my-games-and-apps-working"></a>게임 및 앱이 작동 하지 않는 이유는 무엇 인가요?

게임과 앱이 작동 하지 않거나 스토어 또는 라이브 서비스에 액세스할 수 없는 경우 개발자 모드에서 실행 되 고 있는 것입니다. 현재 모드를 확인 하려면 컨트롤러에서 **홈** 단추를 누릅니다. 소매 집 환경 대신 개발자 홈으로 이동 하는 경우 개발자 모드입니다. 게임을 재생 하려는 경우 개발 홈을 열고 **개발자 모드 유지** 단추를 사용 하 여 일반 정품 모드로 전환할 수 있습니다.

### <a name="why-cant-i-connect-to-my-xbox-one-using-visual-studio"></a>Visual Studio를 사용 하 여 Xbox One에 연결할 수 없는 이유는 무엇 인가요?

먼저 개발자 모드에서 실행 중이 고 소매 모드가 아니라 실행 중인지 확인 합니다. Xbox가 정품 모드에 있으면 Xbox One에 연결할 수 없습니다. 현재 모드를 확인 하려면 컨트롤러에서 **홈** 단추를 누릅니다. 개발자 홈 대신 골드/라이브 콘텐츠가 표시 되는 경우 소매 모드 이며 개발자 모드로 전환 하려면 개발자 모드 정품 인증 앱을 실행 해야 합니다.

> [!NOTE]
> 앱을 배포 하려면 사용자가 로그인 해야 합니다.

자세한 내용은이 페이지의 뒷부분에 나오는 [배포 오류 해결](#fixing-deployment-failures) 을 참조 하십시오.

### <a name="how-do-i-switch-between-retail-mode-and-developer-mode"></a>어떻게 할까요? 소매 모드와 개발자 모드 사이에서 전환 하나요?

[Xbox One Developer Mode 정품 인증](devkit-activation.md) 지침에 따라 이러한 상태에 대해 자세히 알아보세요.

### <a name="how-do-i-know-if-i-am-in-retail-mode-or-developer-mode"></a>어떻게 할까요? 소매 모드 또는 개발자 모드 인지 여부를 알 수 있나요?

[Xbox One Developer Mode 정품 인증](devkit-activation.md) 지침에 따라 이러한 상태에 대해 자세히 알아보세요. 

현재 모드를 확인 하려면 컨트롤러에서 **홈** 단추를 누릅니다. 
- 개발자 홈으로 이동 하면 개발자 모드를 사용 하 게 됩니다.
- 골드/라이브 콘텐츠가 표시 되는 경우 소매 모드입니다.

### <a name="will-my-games-and-apps-still-work-if-i-activate-developer-mode"></a>개발자 모드를 활성화 하는 경우 내 게임과 앱이 계속 작동 하나요?

예, 개발자 모드에서 정품 모드로 전환 하 여 게임을 재생할 수 있습니다. 자세한 내용은 [Xbox One Developer Mode 정품 인증](devkit-activation.md) 페이지를 참조 하세요. 

### <a name="can-i-develop-and-publish-x86-apps-for-xbox"></a>Xbox 용 x86 앱을 개발 하 고 게시할 수 있나요?
Xbox는 더 이상 x86 앱 개발 또는 x86 앱 제출을 저장소에 대해 지원 하지 않습니다. 

### <a name="will-i-lose-my-games-and-apps-or-saved-changes"></a>게임 및 앱 또는 저장 된 변경 내용이 손실 됩니까?

개발자 프로그램을 떠나는 경우 설치 된 게임과 앱은 손실 되지 않습니다. 또한 온라인 상태를 재생 하는 동안에는 저장 된 게임이 모두 실시간 계정 클라우드 프로필에 저장 되므로 분실 해도 됩니다.

### <a name="how-do-i-leave-the-developer-program"></a>개발자 프로그램을 그대로 어떻게 할까요? 시겠습니까?

개발자 프로그램을 종료 하는 방법에 대 한 자세한 내용은 [Xbox One Developer Mode 비활성화](devkit-deactivation.md) 항목을 참조 하세요.

### <a name="i-sold-my-xbox-one-and-left-it-in-developer-mode-how-do-i-deactivate-developer-mode"></a>Xbox One을 판매 하 고 개발자 모드에서 나갔습니다. 어떻게 할까요? 개발자 모드를 비활성화 하 시겠습니까?

Xbox One에 더 이상 액세스할 수 없는 경우 Windows 파트너 센터에서이를 비활성화할 수 있습니다. 자세한 내용은 [Xbox One Developer Mode 비활성화](devkit-deactivation.md#deactivate-your-console-using-partner-center) 항목에서 **파트너 센터를 사용 하 여 콘솔 비활성화** 섹션을 참조 하세요. 

### <a name="i-left-the-developer-program-using-partner-center-but-im-in-still-developer-mode-what-do-i-do"></a>파트너 센터를 사용 하 여 개발자 프로그램을 사용 했지만 여전히 개발자 모드입니다. 어떻게 해야 합니까?

개발자 홈을 시작 하 고 **개발자 모드 유지** 단추를 선택 합니다. 그러면 콘솔이 정품 모드로 다시 시작 됩니다. 

### <a name="can-i-publish-my-app"></a>내 앱을 게시할 수 있나요?

[개발자 계정이](https://developer.microsoft.com/store/register)있는 경우 파트너 센터를 통해 [앱을 게시할](../publish/index.md) 수 있습니다. 소매 Xbox one 콘솔에서 만들고 테스트 한 UWP 앱은 현재 Windows에서 수행 하는 것과 동일한 수집, 검토 및 게시 프로세스를 통해 오늘날의 Xbox One 표준을 충족 하는 추가 검토를 수행 합니다.

### <a name="can-i-publish-my-game"></a>게임을 게시할 수 있나요?

개발자 모드에서 UWP 및 Xbox One을 사용 하 여 Xbox One에서 게임을 빌드하고 테스트할 수 있습니다. UWP 게임을 게시 하려면에 등록 [ID@XBOX](https://www.xbox.com/Developers/id) 하거나 [Xbox Live 크리에이터 프로그램](https://developer.microsoft.com/games/xbox/xboxlive/creator)의 일부 여야 합니다. 자세한 내용은 [개발자 프로그램 개요](https://developer.microsoft.com/games/xbox/docs/xboxlive/get-started/developer-program-overview.html)를 참조 하세요.

### <a name="will-the-standard-game-engines-work"></a>표준 게임 엔진은 작동 하나요?

이 릴리스에 대 한 [알려진 문제](known-issues.md) 페이지를 확인 하세요.

### <a name="what-capabilities-and-system-resources-are-available-to-uwp-games-on-xbox-one"></a>Xbox One의 UWP 게임에 사용할 수 있는 기능 및 시스템 리소스는 무엇 인가요? 

자세한 내용은 참조는 [UWP 앱에 대 한 시스템 리소스 및 Xbox one 게임](system-resource-allocation.md)입니다.

### <a name="if-i-create-a-directx-12-uwp-game-will-it-run-on-my-xbox-one-in-developer-mode"></a>DirectX 12 UWP 게임을 만드는 경우 Xbox One에서 개발자 모드로 실행 되나요?

자세한 내용은 참조는 [UWP 앱에 대 한 시스템 리소스 및 Xbox one 게임](system-resource-allocation.md)입니다.

### <a name="will-the-entire-uwp-api-surface-be-available-on-xbox"></a>전체 UWP API 화면을 Xbox에서 사용할 수 있나요?

이 릴리스에 대 한 [알려진 문제](known-issues.md) 페이지를 확인 하세요.

### <a name="fixing-deployment-failures"></a>배포 오류 해결

Visual Studio에서 앱을 배포할 수 없는 경우 이러한 단계를 통해 문제를 해결할 수 있습니다. 문제가 발생 하는 경우 포럼에서 도움을 요청 하세요.

> [!NOTE]
> 앱을 배포 하려면 사용자가 로그인 해야 합니다. 0x87e10008 오류 메시지가 표시 되 면 사용자에 게 로그인 했는지 확인 한 후 다시 시도 하십시오.

Visual Studio가 Xbox One에 연결할 수 없는 경우:

1. 개발자 모드에 있는지 확인 합니다 (이 페이지의 앞부분에서 설명).
2. 개발 PC를 올바르게 설정 했는지 확인 합니다. [Xbox One에서 UWP 앱 개발 시작](getting-started.md)에 있는 지침을 *모두* 따랐나요? 

3. 아직 하지 않은 경우 [개발 환경 설정](development-environment-setup.md) 항목 및 [Xbox one 도구 소개](introduction-to-xbox-tools.md) 항목을 참조 하세요.

4. 개발 PC에서 콘솔 IP 주소를 "ping" 할 수 있는지 확인 합니다.
  > [!NOTE]
  > 최상의 배포 성능을 얻기 위해서는 콘솔에 대 한 유선 연결을 사용 하는 것이 좋습니다.

5. **디버그** 탭의 인증 드롭다운 목록에서 범용 (암호화 되지 않은 프로토콜)을 사용 하 고 있는지 확인 합니다. 자세한 내용은 [개발 환경 설정](development-environment-setup.md)을 참조 하세요.


### <a name="if-im-building-an-app-using-htmljavascript-how-do-i-enable-gamepad-navigation"></a>HTML/JavaScript를 사용 하 여 앱을 빌드하는 경우 게임 패드 탐색을 사용 하려면 어떻게 해야 하나요?

TVHelpers JavaScript 및 c #에서 뛰어난 Xbox One 및 TV 환경을 빌드하는 데 도움이 되는 JavaScript 및 XAML/c # 샘플과 라이브러리 집합입니다. TVJS는 Xbox One 용 프리미엄 UWP 앱을 빌드하는 데 도움이 되는 라이브러리입니다. TVJS는 자동 컨트롤러 탐색, 리치 미디어 재생, 검색 등을 지원 합니다. Windows 런타임 Api에 대 한 모든 권한을 가진 패키지 된 웹 UWP 앱에서와 마찬가지로, 호스트 된 웹 앱과 함께 TVJS를 사용할 수 있습니다.

자세한 내용은 [Tvhelpers](https://github.com/Microsoft/TVHelpers) 프로젝트 및 프로젝트 [wiki](https://github.com/Microsoft/TVHelpers/wiki)를 참조 하세요.

## <a name="see-also"></a>참고 항목
- [Xbox One에서 UWP의 알려진 문제](known-issues.md)
- [Xbox One의 UWP](index.md)
- [Xbox One의 UWP](index.md)
