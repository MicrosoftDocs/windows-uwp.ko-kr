---
title: Xbox One의 UWP에 대 한 새로운 기능
description: Xbox One에서 UWP의 최신 업데이트에서 개발자를 위한 새로운 기능, 기존 기능 업데이트 및 버그 수정을 참조 하세요.
ms.date: 03/29/2017
ms.topic: article
keywords: windows 10, uwp
ms.assetid: fe63c527-8f06-43a5-868f-de909f5664b3
ms.localizationpriority: medium
ms.openlocfilehash: 937d13c8764cda2f0a4592f6ca3a78d3b35a9529
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89168957"
---
# <a name="whats-new-for-developers-in-the-latest-update-of-uwp-on-xbox-one"></a>Xbox One에서 UWP 최신 업데이트의 개발자를 위한 새로운 기능

Xbox One의 최신 유니버설 Windows 플랫폼 업데이트에는 다음과 같은 새로운 기능, 기존 기능 업데이트 및 버그 수정이 포함 되어 있습니다.

## <a name="x86-apps-and-games-are-no-longer-supported-on-xbox"></a>x86 앱 및 게임이 Xbox에서 더 이상 지원 되지 않음  
Xbox는 더 이상 x86 앱 개발 또는 x86 앱 제출을 저장소에 대해 지원 하지 않습니다.

## <a name="apps-can-now-support-navigating-back-to-the-previous-app"></a>앱은 이제 이전 앱으로의 탐색을 지원할 수 있습니다. 
Xbox의 UWP 앱은 이제 이전 앱으로의 탐색을 지원할 수 있습니다. 이렇게 하려면 [**Windows.UI.Core.SystemNavigationManager**](/uwp/api/Windows.UI.Core.SystemNavigationManager) 이벤트를 구독 하 고 이벤트 처리기에서 **처리** 된 속성을 **false** 로 설정 합니다.

> [!NOTE]
> 호환성을 위해이 기능은 Xbox One에서 최신 버전의 UWP로 빌드된 앱에만 사용할 수 있습니다. 

## <a name="dev-home-is-now-the-default-home-experience-on-development-consoles"></a>개발자 홈은 이제 개발 콘솔의 기본 가정 환경입니다.
이제 개발 콘솔에서 개발자 홈을 기본 홈 환경으로 시작 합니다. 이렇게 하면 소매 홈 화면에서를 클릭 하지 않고도 작업을 바로 시작할 수 있습니다. 이제 개발 홈에는 일반 정품 홈 화면을 시작 하는 빠른 작업이 포함 되어 있습니다. 또한 새로운 설정을 사용 하 여 정품을 기본 환경으로 설정할 수 있습니다. 

## <a name="new-dev-home-user-interface"></a>새 개발자 홈 사용자 인터페이스
개발자 홈 사용자 인터페이스에는 이제 다음과 같은 생산성 향상 기능이 포함 되어 있습니다.
 - 이제 표시를 위해 IP 주소 및 복구 버전과 같은 중요 한 데이터가 화면 위쪽에 표시 됩니다. 
 - 이제 개발자 홈에는 빠른 탐색을 허용 하는 도구를 논리적 집합으로 그룹화 하는 탭 UI가 있습니다.
 - 개발자 홈의 첫 번째 탭에 있는 빠른 작업 단추를 사용 하면 가장 일반적으로 사용 되는 작업에 빠르게 액세스할 수 있습니다. 

## <a name="wdp-for-xbox-enhancements"></a>Xbox의 향상 된 기능에 대 한 WDP
이제 Windows 장치 포털 (WDP)에 콘솔 설정에 대 한 추가 지원이 포함 됩니다. 

## <a name="you-can-now-switch-the-type-of-your-uwp-title-between-app-and-game"></a>이제 UWP 제목의 형식을 "App"과 "Game" 사이에서 전환할 수 있습니다.
UWP 제목의 유형을 "App"과 "Game"으로 전환 하면 스토어에 게시 하지 않고도 게임 시나리오를 테스트할 수 있습니다. 개발자 홈의 **게임 & 앱** 창에서 앱을 선택 하 고 컨트롤러에서 보기 단추를 클릭 한 다음 **앱 정보** 를 선택 하 고 유형을 "App" 또는 "Game"으로 변경 합니다.

## <a name="see-also"></a>참고 항목
- [알려진 문제](known-issues.md)
- [Xbox One의 UWP](index.md)
 - 이제 콘솔의 스크린샷을 캡처할 수 있습니다. 스크린샷을 만드는 방법에 대 한 자세한 내용은 [/exti/스크린샷](wdp-media-capture-api.md) 참조 항목을 참조 하세요.
 - 이 도구는 앱의 느슨한 파일 빌드를 배포할 수 있습니다. 느슨한 파일 빌드에 대 한 자세한 내용은 [/api/app/packagemanager/register](wdp-loose-folder-register-api.md) 참조 항목을 참조 하세요.
 - 콘솔의 개발자 파일은 개발 PC의 파일 탐색기에서 액세스할 수 있습니다. 파일 탐색기를 통해 파일에 액세스 하는 방법에 대 한 자세한 내용은 [/ext/smb/developerfolder](wdp-smb-api.md) 참조 항목을 참조 하세요.

## <a name="see-also"></a>참고 항목
- [알려진 문제](known-issues.md)
- [Xbox One의 UWP](index.md)