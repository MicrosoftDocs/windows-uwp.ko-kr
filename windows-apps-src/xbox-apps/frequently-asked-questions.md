---
title: 질문과 대답
description: Xbox의 UWP에 대한 FAQ입니다.
ms.date: 03/29/2017
ms.topic: article
keywords: windows 10, uwp
ms.assetid: 265fe827-bd4a-48d4-b362-8793b9b25705
ms.localizationpriority: medium
ms.openlocfilehash: 036c3c1832bbb3e27a93671f399a9a97c7afaba3
ms.sourcegitcommit: b4c502d69a13340f6e3c887aa3c26ef2aeee9cee
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 12/03/2018
ms.locfileid: "8458484"
---
# <a name="frequently-asked-questions"></a>질문과 대답

예상하는 대로 작동하지 않나요? 질문과 대답 페이지를 검토합니다. 또한 [알려진 문제](known-issues.md) 항목 및 [유니버설 Windows 앱 개발](https://go.microsoft.com/fwlink/?linkid=839446) 포럼을 확인합니다. 

### <a name="why-arent-my-games-and-apps-working"></a>게임과 앱이 작동하지 않는 이유는 무엇인가요?

게임과 앱이 작동하지 않거나 Microsoft Store나 Live 서비스에 액세스할 수 없는 경우 개발자 모드에서 실행 중일 수 있습니다. 현재 실행 중인 모드를 확인하려면 컨트롤러의 **Home** 단추를 누릅니다. 정품 홈 환경 대신 개발자 홈으로 이동되면 현재 개발자 모드에 있는 것입니다. 게임을 플레이하려는 경우 개발자 홈을 열고 **Leave developer mode(개발자 모드 나가기)** 단추를 사용하여 정품 모드로 다시 전환할 수 있습니다.

### <a name="why-cant-i-connect-to-my-xbox-one-using-visual-studio"></a>Visual Studio를 사용하여 Xbox One에 연결할 수 없는 이유는 무엇인가요?

정품 모드가 아니고 개발자 모드에서 실행 중인지 먼저 확인합니다. 정품 모드에 있을 경우에는 Xbox One에 연결할 수 없습니다. 현재 실행 중인 모드를 확인하려면 컨트롤러의 **Home** 단추를 누릅니다. 개발자 홈 대신 골드/라이브 콘텐츠가 표시되면 현재 정품 모드에 있는 것이며 개발자 모드 활성화 앱을 실행하여 개발자 모드로 전환해야 합니다.

> [!NOTE]
> 앱을 배포하려면 사용자가 로그인되어 있어야 합니다.

자세한 내용은 이 페이지의 뒷부분에 있는 [배포 오류 수정](#fixing-deployment-failures)을 참조하세요.

### <a name="how-do-i-switch-between-retail-mode-and-developer-mode"></a>정품 모드와 개발자 모드 간에 전환하는 방법은 무엇인가요?

[Xbox One 개발자 모드 활성화](devkit-activation.md) 지침을 따라 이러한 상태에 대해 자세히 파악합니다.

### <a name="how-do-i-know-if-i-am-in-retail-mode-or-developer-mode"></a>정품 모드 또는 개발자 모드에 있는지 어떻게 알 수 있나요?

[Xbox One 개발자 모드 활성화](devkit-activation.md) 지침을 따라 이러한 상태에 대해 자세히 파악합니다. 

현재 실행 중인 모드를 확인하려면 컨트롤러의 **Home** 단추를 누릅니다. 
- 개발자 홈으로 이동되면 현재 개발자 모드에 있는 것입니다.
- 골드/라이브 콘텐츠가 표시되면 현재 정품 모드에 있는 것입니다.

### <a name="will-my-games-and-apps-still-work-if-i-activate-developer-mode"></a>개발자 모드를 활성화해도 게임과 앱이 계속 작동하나요?

예, 개발자 모드에서 게임을 플레이할 수 있는 정품 모드로 전환할 수 있습니다. 자세한 내용은 [Xbox One 개발자 모드 활성화](devkit-activation.md) 페이지를 참조하세요. 

### <a name="can-i-develop-and-publish-x86-apps-for-xbox"></a>Xbox용 x86 앱을 개발하고 게시할 수 있나요?
Xbox는 이제 x86 앱 개발이나 x86 Microsoft Store로의 앱 전송을 지원하지 않습니다. 

### <a name="will-i-lose-my-games-and-apps-or-saved-changes"></a>게임과 앱 또는 저장된 변경 내용이 손실되나요?

개발자 프로그램을 종료할 경우 설치된 게임과 앱은 손실되지 않습니다. 또한 온라인 상태에서 플레이한 경우에는 저장된 게임이 Live 계정 클라우드 프로필에 모두 저장되므로 손실되지 않습니다.

### <a name="how-do-i-leave-the-developer-program"></a>개발자 프로그램은 어떻게 종료하나요?

개발자 프로그램을 종료하는 방법은 [Xbox One 개발자 모드 비활성화](devkit-deactivation.md) 항목을 참조하세요.

### <a name="i-sold-my-xbox-one-and-left-it-in-developer-mode-how-do-i-deactivate-developer-mode"></a>Xbox One을 팔고 개발자 모드에서 종료했습니다. 개발자 모드를 비활성화하는 방법은 무엇인가요?

Xbox one에 대 한 액세스 이상 있는 경우 Windows 파트너 센터에서 비활성화할 수 없습니다. 자세한 내용은 [Xbox One 개발자 모드 비활성화](devkit-deactivation.md#deactivate-your-console-using-partner-center) 항목의 **파트너 센터를 사용 하 여 콘솔 비활성화** 섹션을 참조 하세요. 

### <a name="i-left-the-developer-program-using-partner-center-but-im-in-still-developer-mode-what-do-i-do"></a>파트너 센터를 사용 하 여 개발자 프로그램 퇴사 이지만 개발자 모드에 계속 합니다. 어떻게 해야 하나요?

개발자 홈을 시작하고 **Leave developer mode(개발자 모드 나가기)** 단추를 선택합니다. 이렇게 하면 정품 모드로 콘솔이 다시 시작됩니다. 

### <a name="can-i-publish-my-app"></a>내 앱을 게시할 수 있나요?

[개발자 계정이](https://developer.microsoft.com/store/register)있는 경우 파트너 센터를 통해 [앱을 게시할](../publish/index.md) 수 있습니다. 정품 Xbox One 본체에서 만들고 테스트한 UWP 앱은 현재 Windows가 수행하는 동일한 수집, 검토 및 게시 프로세스를 진행하며 최신 Xbox One 표준을 충족하는지 추가적으로 검토합니다.

### <a name="can-i-publish-my-game"></a>내 게임을 게시할 수 있나요?

개발자 모드에서 UWP와 Xbox One을 사용하여 Xbox One에서 게임을 빌드하고 테스트할 수 있습니다. UWP 게임을 게시하려면 [ID@XBOX](http://www.xbox.com/Developers/id) 또는 [Xbox Live Creators 프로그램](https://developer.microsoft.com/games/xbox/xboxlive/creator)의 일부로 등록해야 합니다. 자세한 내용은 [개발자 프로그램 개요](https://developer.microsoft.com/games/xbox/docs/xboxlive/get-started/developer-program-overview.html)를 참조하세요.

### <a name="will-the-standard-game-engines-work"></a>표준 게임 엔진이 작동하나요?

이 릴리스에 대해 [알려진 문제](known-issues.md) 페이지를 확인합니다.

### <a name="what-capabilities-and-system-resources-are-available-to-uwp-games-on-xbox-one"></a>Xbox One에서 UWP 게임에 사용할 수 있는 기능 및 시스템 리소스는 무엇인가요? 

자세한 내용은 [Xbox One의 UWP 앱 및 게임에 대한 시스템 리소스](system-resource-allocation.md)를 참조하세요.

### <a name="if-i-create-a-directx-12-uwp-game-will-it-run-on-my-xbox-one-in-developer-mode"></a>DirectX 12 UWP 게임을 만들면 Xbox One에서 개발자 모드로 실행되나요?

자세한 내용은 [Xbox One의 UWP 앱 및 게임에 대한 시스템 리소스](system-resource-allocation.md)를 참조하세요.

### <a name="will-the-entire-uwp-api-surface-be-available-on-xbox"></a>전체 UWP API 화면을 Xbox에서 사용할 수 있나요?

이 릴리스에 대해 [알려진 문제](known-issues.md) 페이지를 확인합니다.

### <a name="fixing-deployment-failures"></a>배포 오류 수정

Visual Studio에서 앱을 배포할 수 없는 경우 다음 단계가 문제 해결에 도움이 될 수 있습니다. 문제 해결이 안 되는 경우 포럼에 도움을 요청합니다.

> [!NOTE]
> 앱을 배포하려면 사용자가 로그인되어 있어야 합니다. 0x87e10008 오류 메시지가 표시되면 사용자가 로그인되어 있는지 확인하고 다시 시도하세요.

Visual Studio에서 Xbox One에 연결할 수 없는 경우에는 다음을 확인합니다.

1. 개발자 모드에 있는지 확인합니다(이 페이지의 앞에서 설명함).
2. 개발 PC를 올바르게 설정했는지 확인합니다. [Xbox One에서 UWP 앱 개발 시작](getting-started.md)에 있는 지침을 *모두* 따랐나요? 

3. 아직 따르지 않았다면 [개발 환경 설정](development-environment-setup.md) 항목 및 [Xbox One 도구 소개](introduction-to-xbox-tools.md) 항목을 참조하세요.

4. 개발 PC에서 콘솔 IP 주소에 "ping"할 수 있는지 확인합니다.
  > [!NOTE]
  > 최상의 배포 성능을 얻기 위해서는 콘솔에 유선으로 연결하는 것이 좋습니다.

5. **디버그** 탭의 인증 드롭다운 목록에서 유니버설(암호화되지 않은 프로토콜)을 사용하고 있는지 확인합니다. 자세한 내용은 [개발 환경 설정](development-environment-setup.md)을 참조하세요.


### <a name="if-im-building-an-app-using-htmljavascript-how-do-i-enable-gamepad-navigation"></a>HTML/JavaScript를 사용하여 앱을 빌드하는 경우 게임 패드 탐색을 사용하도록 설정하는 방법은 무엇인가요?

TVHelpers는 JavaScript 및 XAML/C#의 샘플 및 라이브러리 집합으로서 JavaScript 및 C#에서 좋은 Xbox One 및 TV 환경을 빌드하는 데 도움이 됩니다. TVJS는 Xbox One용 프리미엄 UWP 앱을 빌드할 수 있는 라이브러리입니다. TVJS에는 자동 컨트롤러 탐색, 리치 미디어 재생, 검색 등에 대한 지원이 포함되어 있습니다. Windows 런타임 API에 대한 모든 권한으로 패키지에 포함된 웹 UWP 앱과 같이 쉽게 호스트된 웹앱과 함께 TVJS를 사용할 수 있습니다.

자세한 내용은 [TVHelpers](https://github.com/Microsoft/TVHelpers) 프로젝트 및 프로젝트 [wiki](https://github.com/Microsoft/TVHelpers/wiki)를 참조하세요.

## <a name="see-also"></a>참고 항목
- [Xbox One의 UWP에 대해 알려진 문제](known-issues.md)
- [Xbox One의 UWP](index.md)
- [Xbox One의 UWP](index.md)
