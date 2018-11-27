---
title: Xbox One의 UWP에 대한 새로운 기능
description: Xbox One의 UWP 앱에 대한 새로운 기능을 강조하여 설명합니다.
ms.date: 03/29/2017
ms.topic: article
keywords: windows 10, uwp
ms.assetid: fe63c527-8f06-43a5-868f-de909f5664b3
ms.localizationpriority: medium
ms.openlocfilehash: aff65e5f1b4771cbb33bc8b8219224042b7bf7e2
ms.sourcegitcommit: 681c70f964210ab49ac5d06357ae96505bb78741
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/26/2018
ms.locfileid: "7709806"
---
# <a name="whats-new-for-developers-in-the-latest-update-of-uwp-on-xbox-one"></a>개발자를 위한 Xbox One의 UWP 최신 업데이트의 새로운 기능

최신 업데이트의 유니버설 Windows 플랫폼 (UWP) Xbox One에서 다음과 같은 새로운 기능, 기존 기능 및 버그 수정에 대 한 업데이트를 포함합니다.

## <a name="x86-apps-and-games-are-no-longer-supported-on-xbox"></a>x86 앱 및 게임은 더 이상 Xbox에서 지원  
Xbox는 이제 x86 앱 개발이나 x86 Microsoft Store로의 앱 전송을 지원하지 않습니다.

## <a name="apps-can-now-support-navigating-back-to-the-previous-app"></a>앱 수 이제 이전 앱으로 다시 탐색 지원 
UWP 앱을 Xbox One에서 이전 앱으로 다시 탐색 지원할 이제 수 있습니다. 이렇게 하려면 [**Windows.UI.Core.SystemNavigationManager.BackRequested**](https://msdn.microsoft.com/library/windows/apps/dn893595) 이벤트를 구독 하 고 **false** 이벤트 처리기에서 **Handled** 속성을 설정 합니다.

> [!NOTE]
> 호환성을 위해이 기능을 Xbox One의 UWP의 최신 릴리스로 작성 된 앱만 사용할 수 있습니다. 

## <a name="dev-home-is-now-the-default-home-experience-on-development-consoles"></a>개발자 홈은 이제 개발 콘솔에서 기본 홈 경험
이제 개발 콘솔 기본 홈 환경으로 개발자 홈을 시작합니다. 이를 클릭 하 여 정품 홈 화면에서 필요 없이 작동 하려면 오른쪽 얻을 수 있습니다. 개발자 홈 실행 정품 홈 화면에 바로 가기가 포함 됩니다. 또한 새 설정의 사용 하면 기본 환경을 소매점 홈 만들 수 있습니다. 

## <a name="new-dev-home-user-interface"></a>새 개발자 홈 사용자 인터페이스
이제 개발자 홈 사용자 인터페이스는 생산성 향상 포함 됩니다.
 - 중요 한 데이터 같은 IP 주소 및 표시 여부에 대 한 화면 맨 위에 있는 복구 버전이 표시 됩니다. 
 - 개발자 홈 탭된 UI 도구 논리 집합으로 그룹화 하는 빠른 탐색 허용 되었습니다.
 - 개발자 홈의 첫 번째 탭에 대 한 빠른 작업 단추 가장 일반적으로 사용 되는 작업에 대 한 빠른 액세스를 허용 합니다. 

## <a name="wdp-for-xbox-enhancements"></a>Xbox의 향상 된 기능에 대 한 WDP
Windows 장치 포털 (WDP)에 이제 콘솔 설정에 대 한 추가 지원이 포함 됩니다. 

## <a name="you-can-now-switch-the-type-of-your-uwp-title-between-app-and-game"></a>이제 UWP 타이틀 "앱"과 "Game" 간의 유형의 전환할 수 있습니다.
"앱"과 "게임" 간의 UWP 타이틀 유형의 전환를 사용 하면 스토어에 게시 하지 않고 게임 시나리오를 테스트할 수 있습니다. 개발자 홈에서 **게임 및 앱** 창에서 앱을 선택 보기 컨트롤러에서 단추, **앱 세부 정보** 선택한 다음 형식을 "앱" 또는 "Game"로 변경 합니다.

## <a name="see-also"></a>참고 항목
- [알려진 문제](known-issues.md)
- [Xbox One의 UWP](index.md)
 - 이제 콘솔의 스크린샷을 캡처할 수 있습니다. 스크린샷 작성에 대한 자세한 내용은 [/ext/screenshot](wdp-media-capture-api.md) 참조 항목을 참조하세요.
 - 도구에서 앱의 느슨한 파일 빌드를 배포할 수 있습니다. 느슨한 파일 빌드에 대한 자세한 내용은 [/api/app/packagemanager/register](wdp-loose-folder-register-api.md) 참조 항목을 참조하세요.
 - 개발 PC의 파일 탐색기에서 콘솔의 개발자 파일에 액세스할 수 있습니다. 파일 탐색기를 통해 파일에 액세스하는 방법에 대한 자세한 내용은 [/ext/smb/developerfolder](wdp-smb-api.md) 참조 항목을 참조하세요.

## <a name="see-also"></a>참고 항목
- [알려진 문제](known-issues.md)
- [Xbox One의 UWP](index.md)
