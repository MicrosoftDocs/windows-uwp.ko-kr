---
Description: 제품 선언을 사용 하면 앱이 Microsoft Store에서 적절 하 게 표시 되 고 올바른 고객 집합에 제공 되는지 확인할 수 있습니다.
title: 제품 선언
ms.assetid: 3AF618F3-2B47-4A57-B7E8-1DF979D4A82C
ms.date: 10/31/2018
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 9c4e8677a27128e6a33a844f5a887e921ca9ced3
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89167367"
---
# <a name="product-declarations"></a>제품 선언

[제출 프로세스](app-submissions.md) 의 [속성](enter-app-properties.md) 페이지에 있는 **제품 선언** 섹션을 사용 하 여 앱이 적절 하 게 표시 되 고 올바른 고객 집합에 제공 되는지 확인 하 고, 앱을 사용 하는 방법을 이해 하는 데 도움을 받을 수 있습니다.

다음 섹션에서는 각 선언이 앱에 적용 되는지 여부를 결정할 때 고려해 야 하는 몇 가지 선언과 선언에 대해 설명 합니다. 아래에서 설명 하는 것 처럼 이러한 선언 중 두 개가 기본적으로 선택 됩니다. 제품 범주에 따라 추가 선언이 표시 될 수도 있습니다. 모든 선언을 검토 하 고 제출을 정확 하 게 반영 해야 합니다.

## <a name="this-app-allows-users-to-make-purchases-but-does-not-use-the-microsoft-store-commerce-system"></a>이 앱을 통해 사용자는 구매할 수 있지만 Microsoft Store 상거래 시스템은 사용 하지 않습니다.

거의 모든 제출에 대해이 확인란을 선택 하지 않은 상태로 둡니다. 앱 내에서 또는 사용 하거나 사용할 수 있는 항목을 구입 하는 기회를 제공 하는 앱은 앱 내 구매 API를 Microsoft Store 사용 하 여 추가 기능을 만들고 제출 해야 하기 때문에이 확인란을 선택 하지 않은 상태로 두어야 합니다. [앱 개발자 계약](/legal/windows/agreements/app-developer-agreement)에 따라 2015 년 6 월 29 일 이전에 생성 및 제출 된 앱은 Microsoft의 상거래 엔진을 사용 하지 않고도 앱 내 구매 기능을 계속 제공할 수 있습니다. 따라서 구매 기능이 [Microsoft Store 정책을](store-policies.md#108-financial-transactions)준수 하기만 하면 됩니다. 앱에 적용 되는 경우이 확인란을 선택 해야 합니다. 그렇지 않으면 선택 하지 않은 상태로 둡니다.

## <a name="this-app-has-been-tested-to-meet-accessibility-guidelines"></a>이 앱은 내게 필요한 옵션 지침을 충족 하도록 테스트 되었습니다.

이 확인란을 선택 하면 앱이 스토어에서 액세스할 수 있는 앱을 구체적으로 찾는 고객을 검색할 수 있습니다.

다음 항목을 모두 완료 한 경우에만이 확인란을 선택 해야 합니다.

-   액세스 가능한 이름과 같은 UI 요소에 대 한 모든 관련 액세스 가능성 정보를 설정 합니다.
-   탭 순서, 키보드 활성화, 화살표 키 탐색, 바로 가기를 고려 하 여 키보드 탐색 및 작업을 구현 했습니다.
-   4.5:1 텍스트 대비 비율로 이러한 항목을 포함 하 여 액세스 가능한 시각적 효과를 보장 하 고 사용자에 게 정보를 전달 하는 데만 색을 사용 하지 마세요.
-   검사 또는 AccChecker와 같은 내게 필요한 옵션 테스트 도구를 사용 하 여 앱을 확인 하 고 해당 도구에서 검색 된 높은 우선 순위 오류를 모두 해결 합니다.
-   화면 키보드, 고대비 및 높은 DPI와 같은 기능 및 도구를 사용 하 여 응용 프로그램의 주요 시나리오를 최종에서 끝으로 확인 했습니다.

앱을 액세스할 수 있는 것으로 선언 하면 장애가 있는 사용자를 포함 하 여 모든 고객이 앱에 액세스할 수 있다는 것에 동의 하는 것입니다. 예를 들어, 고대비 모드 및 화면 판독기를 사용 하 여 앱을 테스트 했음을 의미 합니다. 또한 키보드, 돋보기 및 기타 접근성 도구를 사용 하 여 사용자 인터페이스가 제대로 작동 하는지 확인 했습니다.

자세한 내용은 [저장소의](../design/accessibility/accessibility-in-the-store.md)접근성, [접근성 테스트](../design/accessibility/accessibility-testing.md)및 접근성 [을 참조 하십시오](../design/accessibility/accessibility.md).

> [!IMPORTANT]
> 이러한 목적을 위해 특별히 엔지니어링 하 고 테스트 한 경우를 제외 하 고 액세스할 수 있는 앱을 나열 하지 마세요. 앱이 액세스할 수 있는 것으로 선언 되었지만 실제로 액세스 가능성을 지원 하지 않는 경우 커뮤니티에서 부정적인 피드백을 받을 가능성이 높습니다.

## <a name="customers-can-install-this-app-to-alternate-drives-or-removable-storage"></a>고객은 대체 드라이브나 이동식 저장소에이 앱을 설치할 수 있습니다.

이 상자는 고객이 SD 카드와 같은 외부 또는 이동식 저장소 미디어 나 외부 드라이브와 같은 비시스템 볼륨 드라이브에 앱을 설치할 수 있도록 기본적으로 선택 되어 있습니다.

앱을 대체 드라이브나 이동식 저장소에 설치 하지 않고 장치에서 내부 하드 드라이브에만 설치할 수 있도록 하려면이 확인란의 선택을 취소 합니다. (이동식 저장소 미디어에 *만* 앱을 설치할 수 있도록 설치를 제한 하는 옵션은 없습니다.)


## <a name="windows-can-include-this-apps-data-in-automatic-backups-to-onedrive"></a>Windows는이 앱의 데이터를 OneDrive에 자동 백업에 포함할 수 있습니다.

이 확인란은 고객이 Windows에서 자동으로 OneDrive에 백업을 만들도록 선택할 때 앱의 데이터가 포함 될 수 있도록 허용 하기 위해 기본적으로 선택 되어 있습니다.

응용 프로그램의 데이터가 자동화 된 백업에 포함 되지 않도록 하려면이 확인란의 선택을 취소 합니다.


## <a name="this-app-sends-kinect-data-to-external-services"></a>이 앱은 Kinect 데이터를 외부 서비스로 보냅니다. 

앱이 Kinect 데이터를 사용 하 여 외부 서비스로 보내는 경우이 확인란을 선택 해야 합니다.



 

 

 