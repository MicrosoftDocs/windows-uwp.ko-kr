---
title: Xbox One의 UWP에 대한 새로운 기능
description: Xbox One의 UWP 앱에 대한 새로운 기능을 강조하여 설명합니다.
ms.date: 03/29/2017
ms.topic: article
keywords: windows 10, uwp
ms.assetid: fe63c527-8f06-43a5-868f-de909f5664b3
ms.localizationpriority: medium
ms.openlocfilehash: aff65e5f1b4771cbb33bc8b8219224042b7bf7e2
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57660688"
---
# <a name="whats-new-for-developers-in-the-latest-update-of-uwp-on-xbox-one"></a>개발자를 위한 Xbox One의 UWP 최신 업데이트의 새로운 기능

Xbox One의 UWP(유니버설 Windows 플랫폼) 최신 업데이트에는 다음과 같은 새로운 기능, 기존 기능의 업데이트 및 버그 수정이 포함되어 있습니다.

## <a name="x86-apps-and-games-are-no-longer-supported-on-xbox"></a>x86 앱과 게임은 더 이상 Xbox에서 지원되지 않음  
Xbox는 이제 x86 앱 개발이나 x86 Microsoft Store로의 앱 전송을 지원하지 않습니다.

## <a name="apps-can-now-support-navigating-back-to-the-previous-app"></a>이제 앱에서 이전 앱 탐색을 지원할 수 있음 
Xbox One 앱의 UWP는 이제 이전 앱 탐색을 지원할 수 있습니다. 이렇게 하려면 [**Windows.UI.Core.SystemNavigationManager.BackRequested**](https://msdn.microsoft.com/library/windows/apps/dn893595) 이벤트를 구독하고 이벤트 처리기에서 **Handled** 속성을 **false**로 설정합니다.

> [!NOTE]
> 호환성 때문에 이 기능은 Xbox One의 가장 최신 UWP 릴리스에 기본 제공되는 앱에서만 사용할 수 있습니다. 

## <a name="dev-home-is-now-the-default-home-experience-on-development-consoles"></a>이제부터 개발자 홈이 개발 콘솔의 기본 홈 환경
이제 개발 콘솔은 개발자 홈을 기본 홈 환경으로 실행합니다. 따라서 정품 홈 화면에서 클릭하지 않고 바로 작업으로 이동할 수 있습니다. 이제 개발자 홈에는 정품 홈 화면을 시작하는 바로 가기가 있습니다. 또한 정품 홈을 기본 환경으로 만드는 새로운 설정이 있습니다. 

## <a name="new-dev-home-user-interface"></a>새로운 개발자 홈 사용자 인터페이스
이제 개발 홈 사용자 인터페이스에는 같은 생산성 향상 기능이 포함 됩니다.
 - 이제 가시성을 위해 IP 주소 및 복구 버전 같은 중요한 데이터가 화면 맨 위에 표시됩니다. 
 - 이제 개발자 홈에는 빠른 탐색이 가능하도록 도구를 논리 집합으로 그룹화하는 탭 UI가 있습니다.
 - 개발자 홈의 첫 번째 탭에 있는 바로 가기 단추를 통해 가장 일반적으로 사용되는 작업에 신속하게 액세스할 수 있습니다. 

## <a name="wdp-for-xbox-enhancements"></a>향상된 Xbox WDP
이제 WDP(Windows Device Portal)에는 콘솔 설정에 대한 추가 지원이 포함되어 있습니다. 

## <a name="you-can-now-switch-the-type-of-your-uwp-title-between-app-and-game"></a>이제 UWP 타이틀 유형을 "앱"과 "게임" 사이에서 전환할 수 있습니다.
UWP 타이틀 유형을 "앱"과 "게임" 사이에서 전환하여 게임 시나리오를 스토어에 게시하지 않고도 테스트할 수 있습니다. 개발자 홈의 **게임 및 앱** 창에서 앱을 선택하고, 컨트롤러에서 보기 단추를 누르고, **앱 세부 정보**를 선택한 다음 유형을 "앱" 또는 "게임"으로 변경합니다.

## <a name="see-also"></a>참고 항목
- [알려진된 문제](known-issues.md)
- [Xbox One에서 UWP](index.md)
 - 이제 콘솔의 스크린샷을 캡처할 수 있습니다. 스크린샷 작성에 대한 자세한 내용은 [/ext/screenshot](wdp-media-capture-api.md) 참조 항목을 참조하세요.
 - 도구에서 앱의 느슨한 파일 빌드를 배포할 수 있습니다. 느슨한 파일 빌드에 대한 자세한 내용은 [/api/app/packagemanager/register](wdp-loose-folder-register-api.md) 참조 항목을 참조하세요.
 - 개발 PC의 파일 탐색기에서 콘솔의 개발자 파일에 액세스할 수 있습니다. 파일 탐색기를 통해 파일에 액세스하는 방법에 대한 자세한 내용은 [/ext/smb/developerfolder](wdp-smb-api.md) 참조 항목을 참조하세요.

## <a name="see-also"></a>참고 항목
- [알려진된 문제](known-issues.md)
- [Xbox One에서 UWP](index.md)
