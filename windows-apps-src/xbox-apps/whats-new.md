---
author: v-angraf
title: Xbox One의 UWP에 대한 새로운 기능
description: Xbox One의 UWP 앱에 대한 새로운 기능을 강조하여 설명합니다.
ms.author: v-angraf
ms.date: 03/29/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
ms.assetid: fe63c527-8f06-43a5-868f-de909f5664b3
ms.localizationpriority: medium
ms.openlocfilehash: cbabe9d31b5b9762320df8e4a92d19ae4e33497d
ms.sourcegitcommit: 897a111e8fc5d38d483800288ad01c523e924ef4
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/13/2018
ms.locfileid: "301457"
---
# <a name="whats-new-for-developers-in-the-latest-update-of-uwp-on-xbox-one"></a>개발자를 위한 Xbox One의 UWP 최신 업데이트의 새로운 기능

최신 업데이트의 유니버설 Windows 플랫폼 (UWP) Xbox 하나에서 다음과 같은 새로운 기능, 기존 기능 및 버그 수정 프로그램에 대 한 업데이트를 포함합니다.

## <a name="x86-apps-and-games-are-no-longer-supported-on-xbox"></a>x86 앱 및 게임 Xbox에서 더이상 지원  
Xbox는 이제 x86 앱 개발이나 x86 Microsoft Store로의 앱 전송을 지원하지 않습니다.

## <a name="apps-can-now-support-navigating-back-to-the-previous-app"></a>앱 수 이제 이전 응용 프로그램을 다시 탐색 지원 
Xbox 하나의 앱에 UWP 이전 응용 프로그램을 다시 탐색 하는 것을 지원할 이제 수 있습니다. 이 작업을 수행 [**Windows.UI.Core.SystemNavigationManager.BackRequested**](https://msdn.microsoft.com/library/windows/apps/dn893595) 이벤트에 등록 하 고 **false 이면** 이벤트 처리기에서 **Handled** 속성을 설정 합니다.

> [!NOTE]
> 호환성을 위해이 기능은 Xbox 하나에 UWP의 최신 버전을 사용 하 여 작성 된 응용 프로그램에만 사용할 수 있습니다. 

## <a name="dev-home-is-now-the-default-home-experience-on-development-consoles"></a>개발자 홈 개발 콘솔에서 기본 홈 경험 되었습니다.
이제 개발 콘솔 기본 홈 환경으로 개발 홈을 실행 합니다. 이렇게 하면 곧바로 작업을 통해 소매 홈 화면에서를 클릭 하지 않아도 수 있습니다. 이제 개발 홈 소매 홈 화면을 실행 하는 빠른 동작을 포함 합니다. 또한 새 설정을 사용 하면 소매품 홈을 기본 환경 확인 수 있습니다. 

## <a name="new-dev-home-user-interface"></a>새 개발 홈 사용자 인터페이스
개발 홈 사용자 인터페이스에는 이제는 다음과 같은 생산성 향상 된 기능이 포함 됩니다.
 - 중요 한 데이터 같은 IP 주소 및 복구 버전의 표시 여부에 대 한 화면 위쪽에 표시 됩니다. 
 - 개발자 홈 탭된 UI 도구 논리 집합으로 그룹화 하는 빠른 탐색을 허용 되었습니다.
 - 개발자 홈의 첫번째 탭에서 빠른 실행 단추 가장 일반적으로 사용 되는 작업에 대 한 빠른 액세스를 허용 합니다. 

## <a name="wdp-for-xbox-enhancements"></a>WDP Xbox 향상 된 기능에 대 한
이제 Windows 장치 포털 (WDP)에 콘솔 설정에 대 한 추가 지원이 포함 됩니다. 

## <a name="you-can-now-switch-the-type-of-your-uwp-title-between-app-and-game"></a>이제 "App" 및 "게임" 사이 UWP 제목의 종류를 전환할 수 있습니다.
"App" 및 "게임" 사이 UWP 제목 유형의 전환를 사용 하면 저장소에 게시 하지 않고 게임 시나리오를 테스트할 수 있습니다. 개발 집에서 **게임 및 응용 프로그램** 창에서 응용 프로그램을 선택, 컨트롤러에서 보기 단추를 눌러, **앱 세부 정보** 를 선택 하 고 형식을 "App" 또는 "게임"로 변경 합니다.

## <a name="see-also"></a>기타 참조
- [알려진 문제](known-issues.md)
- [Xbox One의 UWP](index.md)
 - 이제 콘솔의 스크린샷을 캡처할 수 있습니다. 스크린샷 작성에 대한 자세한 내용은 [/ext/screenshot](wdp-media-capture-api.md) 참조 항목을 참조하세요.
 - 도구에서 앱의 느슨한 파일 빌드를 배포할 수 있습니다. 느슨한 파일 빌드에 대한 자세한 내용은 [/api/app/packagemanager/register](wdp-loose-folder-register-api.md) 참조 항목을 참조하세요.
 - 개발 PC의 파일 탐색기에서 콘솔의 개발자 파일에 액세스할 수 있습니다. 파일 탐색기를 통해 파일에 액세스하는 방법에 대한 자세한 내용은 [/ext/smb/developerfolder](wdp-smb-api.md) 참조 항목을 참조하세요.

## <a name="see-also"></a>참고 항목
- [알려진 문제](known-issues.md)
- [Xbox One의 UWP](index.md)
