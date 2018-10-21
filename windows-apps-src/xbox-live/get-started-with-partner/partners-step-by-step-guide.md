---
title: Xbox Live 파트너에 대 한 단계별 지침
author: KevinAsgari
description: 관리 파트너에 대 한 Xbox Live를 통합 하는 단계에 대 한 지침을 제공 합니다.
ms.assetid: f0c9db8f-f492-4955-8622-e1736f0a4509
ms.author: kevinasg
ms.date: 04/04/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: xbox live, xbox, 게임, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 6c1b1741beaf1ffa2b034726a41c56339136e6f1
ms.sourcegitcommit: 72835733ec429a5deb6a11da4112336746e5e9cf
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/21/2018
ms.locfileid: "5162964"
---
# <a name="step-by-step-guide-to-integrate-xbox-live-for-managed-partners-and-idxbox-members"></a>관리 파트너에 대 한 Xbox Live를 통합 하는 단계별 가이드 및 ID@Xbox 멤버

이 섹션에서는 Xbox live를 실행 하는 데 도움이:

## <a name="1-choose-a-platform"></a>1. 플랫폼을 선택 합니다.
Xbox 개발 키트 (XDK), 유니버설 Windows 플랫폼 (UWP), 또는 크로스 플레이 게임을 만드는 중에 결정 합니다.

- XDK 기반 게임만 Xbox One 콘솔에서 실행
- UWP 게임을 Windows PC, Windows Phone 또는 Xbox One 등 모든 Windows 플랫폼 대상으로 지정할 수 있습니다.
  - Xbox One, [Xbox One의 UWP](https://msdn.microsoft.com/en-us/windows/uwp/xbox-apps/index) 및 참조 특히 [Xbox One의 UWP 앱 및 게임에 대 한 시스템 리소스](https://msdn.microsoft.com/en-us/windows/uwp/xbox-apps/system-resource-allocation)
- 크로스 플레이 게임은 일반적으로 게임 Xbox One 및 XDK와 UWP 경로 사용 하 여 Windows PC를 대상으로 하는입니다.

## <a name="2-ensure-you-have-a-title-created-on-dev-center-or-xdp"></a>2. 개발자 센터 또는 XDP에서 만든 타이틀이 있는지 확인
모든 Xbox Live 타이틀 수에 로그인 및 Xbox Live 서비스 전화를 걸기 전에 개발자 센터 또는 Xbox 개발자 포털 (XDP)에서 정의 되어야 합니다.  [새 타이틀 만들기](create-a-new-title.md) 에서는이 작업을 수행 하는 방법을 보여 줍니다.

## <a name="3-follow-the-appropriate-guide-to-setup-your-ide-or-game-engine"></a>3. IDE 또는 게임 엔진 설정 적절 한 가이드를 따라
플랫폼 및 엔진에 대 한 적절 한 시작 가이드를 따라 수 있으며 진행 하면서 Xbox Live의 기본 사항을 알아봅니다.

* [UWP 게임용 Visual Studio를 사용 하 여 시작](get-started-with-visual-studio-and-uwp.md) Visual Studio 프로젝트를 개발자 센터의 Xbox Live 구성과 연결 하는 방법을 표시 됩니다.

* [Unity UWP 게임을 사용 하 여 시작](partner-add-xbox-live-to-unity-uwp.md) 하면 새 Xbox Live를 만들려면 Unity 제목, 사용 하도록 설정 하는 방법 타이틀, 순위표 등의 기능을 추가 하 고 기본 Visual Studio 프로젝트를 생성 표시 됩니다.

* [XDK 용 Visual Studio를 사용 하 여 시작 기반 게임은](xdk-developers.md) XDK를 사용 하는 Xbox One 제목 하는 경우 Visual Studio 프로젝트 설정 하는 방법을 표시 됩니다.

* [크로스 플레이 게임 만들고 시작](get-started-with-cross-play-games.md) 을 Windows 10 PC에 대 한 제품을 기반으로 하는 XDK에 대해 Xbox One 및 기반 UWP 게임에 대 한 게임을 만드는 방법을 설명 합니다.

## <a name="4-xbox-live-concepts"></a>4. Xbox Live 개념
타이틀을 만들었으면 타이틀 개발 경험에 영향을 주는 Xbox Live 개념에 대해 알아 두어야 합니다.

- [Xbox Live 샌드박스](../xbox-live-sandboxes.md)
- [Xbox Live 테스트 계정](../xbox-live-test-accounts.md)
- [Xbox Live API 소개](../introduction-to-xbox-live-apis.md)

## <a name="5-add-xbox-live-features"></a>5. Xbox Live 기능을 추가 합니다.

게임에 Xbox Live 기능을 추가 합니다.

- [Xbox Live 소셜 플랫폼 - 프로필, 친구, 상태](../social-platform/social-platform.md)
- [Xbox Live 데이터 플랫폼-통계, 순위표, 도전 과제](../data-platform/data-platform.md)
- [Xbox Live 멀티 플레이 플랫폼-초대, 매치 메이 킹, 토너먼트](../multiplayer/multiplayer-intro.md)
- [Xbox Live 저장소 플랫폼-연결 된 저장소, 타이틀 저장소](../storage-platform/storage-platform.md)
- [상황에 맞는 검색](../contextual-search/introduction-to-contextual-search.md)

## <a name="6-release-your-title"></a>6. 타이틀을 릴리스 합니다.

ID@Xbox이 제목 및 게임 게시자가 출시 타이틀은 Microsoft 파트너 릴리스 전에 인증 프로세스를 통과 해야 합니다.  이러한 제목은 멀티 플레이어, 도전 과제 및 다양 한 상태와 같은 추가 기능에 액세스할 수 때문입니다.  타이틀을 릴리스할 준비가 되 면 제출 및 릴리스 프로세스에 대 한 자세한 내용은 Microsoft 담당자에 게 문의 합니다.
