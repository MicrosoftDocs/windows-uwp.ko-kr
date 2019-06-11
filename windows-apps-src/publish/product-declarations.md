---
Description: 제품 선언 되도록 앱 Microsoft Store 적절 하 게 표시 되 고 고객의 올바른 집합을 제공 합니다.
title: 제품 선언
ms.assetid: 3AF618F3-2B47-4A57-B7E8-1DF979D4A82C
ms.date: 10/31/2018
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 47011a22353f26361a392690d857bde1fc180c03
ms.sourcegitcommit: 978df7dfd3813de51609b6a44aedcd402083a5fd
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 06/11/2019
ms.locfileid: "66826108"
---
# <a name="product-declarations"></a>제품 선언

합니다 **제품 선언** 섹션을 [속성](enter-app-properties.md) 페이지를 [제출 프로세스](app-submissions.md) 앱 적절 하 게 표시 되 고 올바른 집합을 제공 하도록 해야 고객을 도와의 앱 사용 방법을 이해 하 합니다.

다음 섹션에서는 선언 및 각 선언 앱에 적용 되는지 여부를 결정할 때 고려해 야 하는 내용 중 일부를 설명 합니다. Note는 이러한 선언 두 기본적으로 선택 됩니다 (아래 설명 된 대로.) 제품의 범주에 따라 추가 선언이 나타날 수도 있습니다. 선언의 모든 검토 하 고 제출을 정확히 반영 되도록 해야 합니다.

## <a name="this-app-allows-users-to-make-purchases-but-does-not-use-the-microsoft-store-commerce-system"></a>이 앱을 구매할 수 있습니다 하지만 Microsoft Store 상거래 시스템을 사용 하지 않습니다.

거의 모든 서브 미션이 상자를 선택 취소 되어 있음 두어야 구입 하는 기회를 제공 하는 앱부터 됩니다 또는 가져오거나 수 있는 사용 앱 내에서 사용 되는 항목 사용 해야 합니다 Microsoft Store 앱에서 바로 구매 API 만들기 및 추가 기능을 제출 합니다. 당 합니다 [앱 개발자 계약](https://docs.microsoft.com/legal/windows/agreements/app-developer-agreement), 앱 만들고 2015 년 6 월 29 일 전에 전송 된 구매 기능으로 한 Microsoft의 상용 엔진을 사용 하지 않고 앱 내 구매 기능을 제공 합니다. 계속 될 수 있습니다 준수 하는 [Microsoft Store 정책](store-policies.md#108-financial-transactions)합니다. 이 경우 이 상자를 선택해야 합니다. 그렇지 않으면 선택되지 않은 상태로 그대로 둡니다.

## <a name="this-app-has-been-tested-to-meet-accessibility-guidelines"></a>이 앱은 접근성 준수하도록 테스트되었습니다.

이 상자를 선택하면 스토어에서 접근성 있는 앱을 찾고 있는 고객이 앱을 검색할 수 있습니다.

다음 항목을 모두 완료했으면 이 상자만 선택해야 합니다.

-   액세스 가능한 이름 등 UI 요소에 관련된 모든 접근성 정보를 설정합니다.
-   계정 탭 순서, 키보드 활성화, 화살표 키 탐색, 바로 가기를 고려하여 키보드 탐색과 작업을 구현합니다.
-   4.5:1 텍스트 대비 비율 등을 포함하여 접근성 있는 시각적 환경을 구현하고 색상에만 의존하여 사용자에게 정보를 전달하지 않습니다.
-   Inspect 또는 AccChecker 등의 접근성 테스트 도구를 사용하여 앱을 확인하고 해당 도구에서 검색된 높은 우선 순위 오류를 모두 해결합니다.
-   내레이터, 돋보기, 화상 키보드, 고대비, 높은 DPI 등의 기능과 도구를 사용하여 앱의 종단 간 주요 시나리오를 확인합니다.

앱을 접근성 있음으로 선언하면 장애가 있는 사용자를 비롯하여 모든 고객이 앱에 접근할 수 있다는 것에 동의하는 것입니다. 예를 들어 고대비 모드에서 앱을 테스트하고 화면 읽기 프로그램을 사용하여 앱을 테스트했음을 의미합니다. 또한 사용자 인터페이스가 키보드, 돋보기 및 기타 접근성 도구에서 올바르게 작동함을 검증했습니다.

자세한 내용은 [접근성](../design/accessibility/accessibility.md), [접근성 테스트](../design/accessibility/accessibility-testing.md) 및 [스토어에서 접근성](../design/accessibility/accessibility-in-the-store.md)을 참조하세요.

> [!IMPORTANT]
> 특히 엔지니어링 있고 해당 목적을 위해 테스트 하지 않는 한 목록으로 액세스할 수 있도록 앱을 없습니다. 앱이 접근성 있음으로 선언되었지만 실제로 접근성을 지원하지 않을 경우 커뮤니티로부터 부정적인 피드백을 받을 수 있습니다.

## <a name="customers-can-install-this-app-to-alternate-drives-or-removable-storage"></a>고객은 이 앱을 설치하여 드라이브 또는 이동식 저장소를 대체할 수 있습니다.

이 확인란이 기본적으로 고객 외부 또는 이동식 저장소 미디어 SD 카드, 같은 또는 비 시스템 볼륨에 외부 드라이브와 같은 드라이브에 앱을 설치할 수 있도록 합니다.

앱 대체 드라이브 또는 이동식 저장소에 설치 되지 않도록 및만 해당 장치의 내부 하드 드라이브에 설치를 허용 하려는 경우에이 확인란의 선택을 취소 합니다. (앱 수 있도록 설치를 제한할 수 있는 옵션이 있다는 점에 주의 *만* 이동식 저장소 미디어에 설치할 수 있습니다.)


## <a name="windows-can-include-this-apps-data-in-automatic-backups-to-onedrive"></a>Windows에서는 이 앱의 데이터를 OneDrive에 대한 자동 백업에 포함할 수 있습니다.

고객이 Windows에서 OneDrive에 자동으로 백업하도록 선택하면 앱 데이터를 포함할 수 있도록 이 상자가 기본적으로 선택됩니다.

앱 데이터가 자동화된 백업에 포함되지 않게 하려면 이 상자를 선택 취소합니다.


## <a name="this-app-sends-kinect-data-to-external-services"></a>이 앱은 Kinect 데이터를 외부 서비스로 전송합니다. 

앱이 kinect 데이터를 사용하고, 이를 외부 서비스로 전송한다면 이 상자를 선택해야 합니다.



 

 

 




