---
title: Xbox Live 크리에이터스 프로그램 시작
author: KevinAsgari
description: Xbox Live 크리에이터스 프로그램을 시작하는 데 도움이 되는 링크를 제공합니다.
ms.assetid: 2a744405-7ee4-42b4-8f36-9916e8c3a530
ms.author: kevinasg
ms.date: 12/13/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: xbox live, xbox, 게임, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: e79846ecb0fce63d371b9ade7efa01ca0561b952
ms.sourcegitcommit: 9354909f9351b9635bee9bb2dc62db60d2d70107
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/16/2018
ms.locfileid: "4686285"
---
# <a name="get-started-with-the-xbox-live-creators-program"></a>Xbox Live 크리에이터스 프로그램 시작
 
Xbox Live 크리에이터스 프로그램은 개념 승인 없이 간단한 인증 프로세스를 통해 Xbox One 및 Windows 10에 게임을 신속하게 게시할 수 있는 프로그램입니다. 귀하의 게임이 Xbox Live와 통합되고 [표준 Store 정책](https://msdn.microsoft.com/en-us/library/windows/apps/dn764944.aspx)을 준수하면 게시할 준비가 된 것입니다. 이 문서는 게임을 설정하고 Xbox Live 통합을 사용하여 실행하는 데 필요한 단계 간략하게 설명합니다. 

Xbox Live 크리에이터스 프로그램 게임은 유니버설 Windows 플랫폼(UWP) 응용 프로그램이어야 합니다. Xbox One의 경우 [Xbox One의 UWP](https://msdn.microsoft.com/en-us/windows/uwp/xbox-apps/index) 그리고 특히 [Xbox One의 UWP 앱 및 게임에 대한 시스템 리소스](https://msdn.microsoft.com/en-us/windows/uwp/xbox-apps/system-resource-allocation)를 참조하세요. Xbox Live 크리에이터스 프로그램을 통해 게시된 게임은 도전 과제 또는 온라인 멀티 플레이어 서비스에 액세스할 수 없습니다. 지원되는 서비스의 전체 목록은 [개발자 프로그램 개요 기능 테이블](https://docs.microsoft.com/en-us/windows/uwp/xbox-live/developer-program-overview#feature-table)을 참조하세요.

## <a name="1-ensure-you-have-a-title-created-on-dev-center"></a>1. 개발자 센터에서 만든 타이틀이 있는지 확인
모든 Xbox Live 타이틀은 로그인하고 Xbox Live 서비스 전화를 걸기 전에 개발자 센터에서 정의되어야 합니다.  [새 크리에이터스 타이틀 만들기](create-and-test-a-new-creators-title.md)에서 이 작업을 수행하는 방법을 설명합니다.

## <a name="2-follow-the-appropriate-guide-to-setup-your-ide-or-game-engine"></a>2. 적절한 가이드를 따라 IDE 또는 게임 엔진 설정
플랫폼 및 엔진에 대한 적절한 "시작 가이드"를 따라 진행하면서 Xbox Live의 기본 사항을 알아봅니다.

* [Visual Studio로 크리에이터스 타이틀 개발](develop-creators-title-with-visual-studio.md)은 Visual Studio 프로젝트를 개발자 센터의 Xbox Live 구성과 연결하는 방법을 설명합니다.
* [Unity로 크리에이터스 타이틀 개발](develop-creators-title-with-unity.md)은 새 Xbox Live 지원 Unity 게임을 만들고, 단일 사용자와 여러 사용자 로그인을 처리하고, 순위표 및 통계 같은 기능을 추가하고, 기본 Visual Studio 프로젝트를 생성하는 방법을 설명합니다.

Unity는 설명서를 제공하는 유일한 타사 게임 엔진이며 [Construct(2 & 3)](https://www.scirra.com/construct2) 및 [Game Maker Studio](https://www.yoyogames.com/gamemaker) 게임 엔진 또한 Xbox Live를 각각 Construct 또는 Game Maker Studio 게임에 통합하는 것을 돕는 설명서를 제공합니다.

* [이제 Game Maker Studio 2 UWP가 Xbox Live 크리에이터스 프로그램 지원](https://www.yoyogames.com/gamemaker/xblc)은 Xbox One 및 Windows 10 PC에서 플레이하기 위해 Game Maker Studio 프로젝트를 내보내는 방법을 보여줍니다.
* [UWP 앱에서 Xbox Live 사용 - Construct](https://www.scirra.com/tutorials/9540/using-xbox-live-in-uwp-apps)는 Construct 2와 3 게임에서 Xbox Live를 사용하는 방법을 설명합니다.

문서화된 Xbox Live 통합 없는 다른 게임 개발의 경우 여전히 Xbox Live API를 사용하여 타이틀에 Xbox Live를 추가할 수 있습니다. 프로젝트에서 Xbox Live API를 사용하려면 NuGet 패키지가 있는 바이너리에 참조를 추가하거나 API 소스를 추가합니다. NuGet 패키지를 추가하면 쉽게 컴파일하고 소스를 추가하면 쉽게 디버깅할 수 있습니다.

Unity가 아닌 타가 게임 엔진을 사용하는 Xbox Live 서비스 사용에 대한 지원은 귀하의 요청에 답변할 적절한 게임 엔진 담당자에게 문의하세요.

## <a name="3-xbox-live-concepts--testing"></a>3. Xbox Live 개념 및 테스트
타이틀을 만들었으면 타이틀 개발 경험에 영향을 주는 Xbox Live 개념에 대해 알아 두어야 합니다. 게임이 예상대로 작동하는지 확인하기 위해 이를 지원하는 모든 플랫폼에서 게임을 테스트하는 것도 중요합니다.

- [크리에이터스 프로그램에 대한 Xbox Live 서비스 구성](xbox-live-service-configuration-creators.md)
- [Xbox Live 테스트 환경](../xbox-live-sandboxes.md)
- [Xbox Live 계정에 권한을 부여](authorize-xbox-live-accounts.md)

## <a name="4-enable-xbox-live-sign-in"></a>4. Xbox Live 로그인 활성화
모든 Xbox Live 크리에이터스 프로그램 게임은 Xbox Live 로그인을 통합하고 사용자 ID를 표시해야 합니다(게이머태그, 게이머 사진 등). 자동으로 사용자가 로그인되도록 하거나 버튼을 눌러 시작하도록 선택할 수 있습니다. 로그인하면 플레이어가 적절한 프로필을 사용하는 것을 확인하기 위해 게이머태그가 표시되어야 합니다.

- [Xbox Live 소셜 플랫폼 - 프로필, 친구, 상태](../social-platform/social-platform.md)

## <a name="5-add-optional-xbox-live-features"></a>5. 선택적 Xbox Live 기능 추가

Xbox Live 크리에이터스 프로그램은 귀하의 게임을 홍보하고 플레이어의 참여를 유지하는 데 도움이 되도록 설계된 일련의 기능을 제공합니다.

- [Xbox Live 데이터 플랫폼 - 통계, 순위표](../data-platform/data-platform.md)로 게이머가 친구들과 경쟁하고 높은 순위로 이동하게 하여 게임의 참여를 높일 수 있습니다.
- [Xbox Live 저장소 플랫폼 - 연결된 저장소, 타이틀 저장소](../storage-platform/storage-platform.md)는 기기 간 무료 저장 게임 로밍을 지원하여 게이머들이 쉽게 Xbox One 및 Windows PC에서 게임을 진행할 수 있습니다.
- [Xbox Live 소셜 플랫폼 - 프로필, 친구, 상태](../social-platform/social-platform.md)를 통해 게이머는 친구들과 소통하고 게임에 대해 이야기할 수 있습니다.

Xbox Live 크리에이터스 프로그램은 온라인 멀티 플레이어, 도전 과제 또는 게이머 점수를 지원하지 않습니다.

## <a name="6-release-your-game"></a>6. 게임 릴리스

Xbox Live 크리에이터스 프로그램을 사용하는 경우 프로세스는 다른 UWP 응용 프로그램 릴리스와 다르지 않습니다.

- [게임 및 Xbox 정책](https://msdn.microsoft.com/en-us/library/windows/apps/dn764944.aspx#pol_10_13) 포함 [Microsoft Store 정책](https://msdn.microsoft.com/en-us/library/windows/apps/dn764944.aspx)
- [Windows 앱 게시](https://developer.microsoft.com/en-us/store/publish-apps)