---
author: jnHs
Description: Review this list to help avoid issues that frequently prevent apps from getting certified, or that might be identified during a spot check after the app is published.
title: 일반적인 인증 실패 방지
ms.assetid: 9E9E3841-2F9B-42D4-B5F8-4C7C31E42E3D
ms.author: wdg-dev-content
ms.date: 06/19/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 283856ad163d2e67078c61559f6f8ec667e92b87
ms.sourcegitcommit: 753dfcd0f9fdfc963579dd0b217b445c4b110a18
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/27/2018
ms.locfileid: "2861862"
---
# <a name="avoid-common-certification-failures"></a>일반적인 인증 실패 방지


이 목록을 검토하여 빈번하게 앱이 인증되지 않도록 하거나, 앱이 게시된 후 임의 추출 검사 중 식별할 수 있는 문제를 방지하세요.

> [!NOTE]
> 나열 된 요구 사항을 모두 충족 하는 앱을 확인 하는 [Microsoft 저장소 정책](https://docs.microsoft.com/legal/windows/agreements/store-policies) 을 검토 해야 합니다.

-   완료된 앱만 제출합니다. 앱 설명을 사용하여 제공 예정인 기능을 언급할 수 있지만 앱에 불완전한 조항, 생성 중인 웹 페이지 링크 또는 앱이 불완전하다는 인상을 고객에게 주는 다른 사항이 앱에 포함되지 않도록 하세요.

-   앱을 제출하기 전에 [Windows 앱 인증 키트로 앱을 테스트합니다](../debug-test-perf/windows-app-certification-kit.md).

-   여러 다른 구성에서 앱을 테스트하여 안정적인지 확인합니다.

-   네트워크에 연결되지 않아도 앱 크래시가 발생하지 않도록 합니다. 실제로 앱을 사용하는 데 연결이 필요한 경우에도 연결되지 않았을 때 적절하게 작동해야 합니다.

-   앱을 사용하기 위해 사용자가 서비스에 로그인해야 하는 경우 테스트 계정의 사용자 이름 및 암호, 숨겨진 기능 또는 잠긴 기능을 액세스하는 데 필요한 단계 등 앱을 사용하는 데 [필요한 정보를 제공](notes-for-certification.md)합니다.

-   앱이 요구하는 경우(예를 들어 앱이 어떤 방식으로든 개인 정보를 액세스하는 경우나 법률에서 요구하는 경우) [개인 정보 취급 방침](create-app-store-listings.md#privacy-policy)을 포함시킵니다. 앱 개인정보 보호 정책이 필요로 하는 경우를 확인 하려면 [응용 프로그램 개발자 계약](https://docs.microsoft.com/legal/windows/agreements/app-developer-agreement) 및 [Microsoft 저장소 정책을](https://docs.microsoft.com/legal/windows/agreements/store-policies)검토 합니다.

-   앱이 수행하는 역할을 앱 설명이 명확하게 나타내는지 확인합니다. 도움이 필요하면 [유용한 앱 설명 작성](write-a-great-app-description.md)에 있는 지침을 참조하세요.

-   [연령별 등급](age-ratings.md) 섹션의 모든 질문에 대해 완전하고 정확한 답변을 제시해야 합니다.

-   앱을 특별히 엔지니어링하고 접근성 시나리오를 테스트하지 않은 경우 [앱을 접근성 있음으로 선언](app-declarations.md#this-app-has-been-tested-to-meet-accessibility-guidelines)하지 마세요.

-   앱이 [**Windows.ApplicationModel.Store**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Store) 네임스페이스의 상거래 API를 사용하는 경우 앱을 테스트하고 일반적인 예외가 처리되는지 확인합니다. 또한 앱이 테스트용으로만 사용되는 [**CurrentAppSimulator**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Store.CurrentAppSimulator) 클래스가 아니라 [**CurrentApp**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Store.CurrentApp) 클래스를 사용하는지 확인합니다. 앱이 Windows10 버전 1607 이상을 대상으로 하는 경우 Windows.ApplicationModel.Store 네임스페이스 대신 [Windows.Services.Store](https://docs.microsoft.com/uwp/api/windows.services.store) 네임스페이스의 멤버를 사용하는 것이 좋습니다.


 

 




