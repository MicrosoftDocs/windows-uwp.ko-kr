---
title: Xbox Live 파트너에 대 한 단계별 가이드
description: 관리 되는 파트너를 위한 Xbox Live를 통합 하는 단계에 대 한 지침을 제공 합니다.
ms.assetid: f0c9db8f-f492-4955-8622-e1736f0a4509
ms.date: 04/04/2017
ms.topic: article
keywords: xbox live, xbox, 게임, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 9a2d0b9e7d97adfb02281e7bae34ed51afd44f7f
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57660848"
---
# <a name="step-by-step-guide-to-integrate-xbox-live-for-managed-partners-and-idxbox-members"></a>관리 되는 파트너를 위한 Xbox Live를 통합 하 단계별 가이드 및 ID@Xbox 멤버

이 섹션은 Xbox Live를 사용한 시작 및 실행 하는 데 도움이 됩니다.

## <a name="1-choose-a-platform"></a>1. 플랫폼 선택
Xbox 개발 키트 (XDK), 유니버설 Windows 플랫폼 (UWP) 게임 간 진행 하면서 결정 합니다.

- XDK은 Xbox One 콘솔 에서만 실행 하는 게임 기반
- UWP 게임 Windows PC, Windows Phone 또는 Xbox One와 같은 모든 Windows 플랫폼을 대상 수 있습니다.
  - Xbox One에 대 한 참조 [Xbox One에 UWP](https://msdn.microsoft.com/en-us/windows/uwp/xbox-apps/index) 및 특히 [Xbox One에서 UWP 앱과 게임에 대 한 시스템 리소스](https://msdn.microsoft.com/en-us/windows/uwp/xbox-apps/system-resource-allocation)
- 교차 play 게임은 일반적으로 게임 Xbox One 및 XDK와 UWP 모두 경로 사용 하 여 Windows PC를 대상으로 하는입니다.

## <a name="2-ensure-you-have-a-title-created-in-partner-center-or-xdp"></a>2. 파트너 센터 또는 XDP에서 만든 제목을 확인합니다
Xbox Live 제목 모든 로그인 및 Xbox Live 서비스를 호출 하는 일을 할 수 있습니다 파트너 센터 또는 Xbox 개발자 포털 (XDP)에 정의 되어야 합니다.  [새 제목을 만들](create-a-new-title.md) 이 작업을 수행 하는 방법을 표시 됩니다.

## <a name="3-follow-the-appropriate-guide-to-setup-your-ide-or-game-engine"></a>3. 프로그램 IDE 또는 게임 엔진을 설정 하려면 적절 한 가이드
플랫폼 및 엔진에 대해 적절 한 시작 가이드를 따르세요를 진행 하면서 Xbox Live의 기본 사항을 알아봅니다.

* [Visual Studio를 사용 하 여 UWP 게임을 위한 시작](get-started-with-visual-studio-and-uwp.md) 파트너 센터에서 Xbox Live 구성 사용 하 여 Visual Studio 프로젝트를 연결 하는 방법을 표시 됩니다.

* [Unity를 사용 하 여 UWP 게임을 위한 시작](partner-add-xbox-live-to-unity-uwp.md) 새 Xbox Live 만들려면 Unity title을 사용 하도록 설정 하는 방법을 제목에 순위표와 같은 기능을 추가 하는 네이티브 Visual Studio 프로젝트 생성에 표시 됩니다.

* [게임 기반 XDK 용 Visual Studio를 사용 하 여 시작](xdk-developers.md) 는 XDK를 사용 하 여 Xbox One title을 하는 경우 Visual Studio 프로젝트 설정을 가져오는 방법 표시 됩니다.

* [가져오기 시작 게임을 만들면 간 play](get-started-with-cross-play-games.md) 제품 Xbox One 및 기반 UWP는 XDK 기반 게임을 Windows 10 PC에 대 한 게임을 확인 하는 방법에 설명 합니다.

## <a name="4-xbox-live-concepts"></a>4. Xbox Live 개념
타이틀을 만들었으면 타이틀 개발 경험에 영향을 주는 Xbox Live 개념에 대해 알아 두어야 합니다.

- [Xbox Live 샌드박스](../xbox-live-sandboxes.md)
- [Xbox Live 테스트 계정](../xbox-live-test-accounts.md)
- [Xbox Live Api 소개](../introduction-to-xbox-live-apis.md)

## <a name="5-add-xbox-live-features"></a>5. Xbox Live 기능 추가

게임에 Xbox Live 기능을 추가 합니다.

- [Xbox Live 친구 소셜 플랫폼-프로필 상태](../social-platform/social-platform.md)
- [Xbox Live 성과 데이터 플랫폼-Stats 순위표](../data-platform/data-platform.md)
- [Xbox Live 멀티 플레이 플랫폼-매 치메이 킹, 초대 토너먼트](../multiplayer/multiplayer-intro.md)
- [Xbox Live 플랫폼-연결 된 저장소 제목 저장소](../storage-platform/storage-platform.md)
- [상황별 검색](../contextual-search/introduction-to-contextual-search.md)

## <a name="6-release-your-title"></a>6. 제목 릴리스

ID@Xbox 제목 게임 게시자가 출시 되는 모든이 릴리스 전에 인증 프로세스를 통해 Microsoft 파트너를 이동 해야 합니다.  이는 이러한 제목 멀티 플레이 게임, 성과, 그리고 Rich 현재 상태와 같은 추가 기능에 대 한 액세스를 갖기 때문입니다.  한 번을 제목 릴리스, 제출 및 릴리스 프로세스에 대 한 자세한 내용은 Microsoft 담당자에 게 문의 하는 일을 준비가 됩니다.
