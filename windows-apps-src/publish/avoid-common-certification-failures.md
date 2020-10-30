---
description: 앱이 인증 되지 않도록 자주 발생 하거나 앱이 게시 된 후에도 확인 하는 동안 확인 될 수 있는 문제를 방지 하려면이 목록을 검토 하세요.
title: 일반적인 인증 실패 방지
ms.assetid: 9E9E3841-2F9B-42D4-B5F8-4C7C31E42E3D
ms.date: 10/31/2018
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 672da214582fb6b206d7e16e1e776be40caeec90
ms.sourcegitcommit: a3bbd3dd13be5d2f8a2793717adf4276840ee17d
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/30/2020
ms.locfileid: "93031226"
---
# <a name="avoid-common-certification-failures"></a>일반적인 인증 실패 방지


앱이 인증 되지 않도록 자주 발생 하거나 앱이 게시 된 후에도 확인 하는 동안 확인 될 수 있는 문제를 방지 하려면이 목록을 검토 하세요.

> [!NOTE]
> [Microsoft Store 정책을](store-policies.md) 검토 하 여 앱이 여기에 나열 된 모든 요구 사항을 충족 하는지 확인 해야 합니다.

-   작업이 완료 된 경우에만 앱을 제출 합니다. 앱의 설명에 따라 예정 된 기능을 언급 하 고, 앱에 불완전 한 섹션, 제작 중인 웹 페이지에 대 한 링크 또는 앱이 완전 하지 않은 느낌을 줄 수 있는 기타 항목을 포함 하지 않는지 확인 하는 것을 환영 합니다.

-   앱을 제출 하기 전에 [Windows 앱 인증 키트를 사용 하 여 앱을 테스트](../debug-test-perf/windows-app-certification-kit.md) 합니다.

-   여러 다른 구성에서 앱을 테스트 하 여 최대한 안정적으로 작동 하는지 확인 합니다.

-   네트워크 연결 없이 앱이 작동 중단 되지 않도록 합니다. 실제로 앱을 사용 하는 데 연결이 필요한 경우에도 연결이 없는 경우 적절 하 게 수행 해야 합니다.

-   앱에서 사용자에 게 서비스에 로그인 해야 하는 경우 테스트 계정의 사용자 이름 및 암호와 같은 응용 프로그램을 사용 하는 데 필요한 [정보를 제공](notes-for-certification.md) 하거나, 숨겨진 기능 또는 잠긴 기능에 액세스 하는 데 필요한 모든 단계를 제공 합니다.

-   앱에 필요한 경우 [개인 정보 취급 방침 URL](enter-app-properties.md#privacy-policy-url) 을 포함 합니다. 예를 들어 앱에서 어떤 방식으로든 모든 종류의 개인 정보에 액세스 하는 경우 또는 법률에서 요구 하는 경우입니다. 앱에 개인 정보 취급 방침이 필요한 지 여부를 확인 하려면 [앱 개발자 계약](/legal/windows/agreements/app-developer-agreement) 및 [Microsoft Store 정책을](store-policies.md)검토 하세요.

-   앱의 설명이 앱에서 수행 하는 작업을 명확 하 게 나타냅니다. 도움이 필요 하면 [유용한 앱 설명 작성](write-a-great-app-description.md)에 대 한 지침을 참조 하세요.

-   [연령별 등급](age-ratings.md) 섹션의 모든 질문에 대 한 완전 하 고 정확한 응답을 제공 합니다.

-   내게 필요한 옵션 시나리오에 대해 특별히 엔지니어링 하 고 테스트 한 경우를 제외 하 고 [응용 프로그램을 액세스 가능으로 선언](product-declarations.md#this-app-has-been-tested-to-meet-accessibility-guidelines) 하지 마세요.

-   앱이 [**Windows. ApplicationModel. Store**](/uwp/api/Windows.ApplicationModel.Store) 네임 스페이스의 상거래 api를 사용 하는 경우 앱을 테스트 하 고 일반적인 예외를 처리 하는지 확인 해야 합니다. 또한 앱이 [**Currentapp시뮬레이터**](/uwp/api/Windows.ApplicationModel.Store.CurrentAppSimulator) 클래스가 아니라 [**currentapp**](/uwp/api/Windows.ApplicationModel.Store.CurrentApp) 클래스를 사용 하 고 있는지 확인 합니다 .이는 테스트 목적 으로만 사용 됩니다. 앱이 Windows 10 버전 1607 이상을 대상으로 하는 경우에는 Windows. ApplicationModel. Store 네임 스페이스 대신에 [windows. store](/uwp/api/windows.services.store) 네임 스페이스의 멤버를 사용 하는 것이 좋습니다.


 

 
