---
author: jnHs
Description: Product declarations help make sure your app is displayed appropriately in the Microsoft Store and offered to the right set of customers.
title: 제품 선언
ms.assetid: 3AF618F3-2B47-4A57-B7E8-1DF979D4A82C
ms.author: wdg-dev-content
ms.date: 12/05/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 959e056d5edf5e1fe7a1c51a2f855c9e11512cb0
ms.sourcegitcommit: f2f4820dd2026f1b47a2b1bf2bc89d7220a79c1a
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/22/2018
ms.locfileid: "2799544"
---
# <a name="product-declarations"></a>제품 선언

[전송 프로세스](app-submissions.md) 의 [속성](enter-app-properties.md) 페이지의 **제품 선언** 구역 되었는지 쉽게 확인할 앱 적절 하 게 표시 되 고, 고객 및 앱을 사용할 수 있는 방법을 이해 하는 데 도움이 집합이 오른쪽에 게 제공 합니다.

다음 섹션에서는 선언과 각 선언 앱에 적용 될 수 있는지 여부를 결정할 때 고려 해야하는 대해 설명 합니다. Note는 이러한 선언의 2는 기본적으로 선택 (아래 설명에 따라.) 제품의 종류에 따라 추가 선언 볼 수 있습니다. 모든 선언을 검토 하 고 사용자가 전송한 항목을 정확 하 게 반영 하 확인 해야 합니다.

## <a name="this-app-allows-users-to-make-purchases-but-does-not-use-the-microsoft-store-commerce-system"></a>이 응용 프로그램을 구매 하는 사용자 수는 있지만 Microsoft 저장소 전자 상거래 시스템을 사용 하지 않습니다.

거의 모든 전송에 대 한이 상자를 선택 하지 않은 두어야 앱 구입 기회를 제공 하는 이후는 또는 하거나 될 수 있는 사용 하는 응용 프로그램 내에서 사용 되는 항목 사용 해야 합니다 Microsoft Store에서 앱 구매 API를 만들고 추가 기능을 제출 합니다. [앱 개발자 계약](https://docs.microsoft.com/legal/windows/agreements/app-developer-agreement)당 만들고 2015, 년 6 월 29 하기 전에 제출 된 앱을 구매 하는 기능을 준수는 [Microsoft의 전자 상거래 엔진을 사용 하지 않고 응용 프로그램에서 구매 기능을 제공 하려면 계속 수 있습니다. Microsoft 저장소 정책](https://docs.microsoft.com/legal/windows/agreements/store-policies#108-financial-transactions)합니다. 이 경우 이 상자를 선택해야 합니다. 그렇지 않으면 선택되지 않은 상태로 그대로 둡니다.

## <a name="this-app-has-been-tested-to-meet-accessibility-guidelines"></a>이 앱은 접근성 준수하도록 테스트되었습니다.

이 상자를 선택하면 스토어에서 접근성 있는 앱을 찾고 있는 고객이 앱을 검색할 수 있습니다.

다음 항목을 모두 완료했으면 이 상자만 선택해야 합니다.

-   액세스 가능한 이름 등 UI 요소에 관련된 모든 접근성 정보를 설정합니다.
-   계정 탭 순서, 키보드 활성화, 화살표 키 탐색, 바로 가기를 고려하여 키보드 탐색과 작업을 구현합니다.
-   4.5:1 텍스트 대비 비율 등을 포함하여 접근성 있는 시각적 환경을 구현하고 색상에만 의존하여 사용자에게 정보를 전달하지 않습니다.
-   Inspect 또는 AccChecker 등의 접근성 테스트 도구를 사용하여 앱을 확인하고 해당 도구에서 검색된 높은 우선 순위 오류를 모두 해결합니다.
-   내레이터, 돋보기, 화상 키보드, 고대비, 높은 DPI 등의 기능과 도구를 사용하여 앱의 종단 간 주요 시나리오를 확인합니다.

앱을 접근성 있음으로 선언하면 장애가 있는 사용자를 비롯하여 모든 고객이 앱에 접근할 수 있다는 것에 동의하는 것입니다. 예를 들어 고대비 모드에서 앱을 테스트하고 화면 읽기 프로그램을 사용하여 앱을 테스트했음을 의미합니다. 또한 사용자 인터페이스가 키보드, 돋보기 및 기타 접근성 도구에서 올바르게 작동함을 검증했습니다.

자세한 정보를 [내게 필요한 옵션](../design/accessibility/accessibility.md), [내게 필요한 옵션 테스트](../design/accessibility/accessibility-testing.md)및 [저장소의 내게 필요한 옵션](../design/accessibility/accessibility-in-the-store.md)을 참조 하십시오.

> [!IMPORTANT]
> 특히 엔지니어링 있고 해당 목적을 위해 테스트 하지 않으면 목록으로 액세스할 수 있는 앱을 없어도 됩니다. 앱이 접근성 있음으로 선언되었지만 실제로 접근성을 지원하지 않을 경우 커뮤니티로부터 부정적인 피드백을 받을 수 있습니다.

## <a name="customers-can-install-this-app-to-alternate-drives-or-removable-storage"></a>고객은 이 앱을 설치하여 드라이브 또는 이동식 저장소를 대체할 수 있습니다.

기본적으로 외부 드라이브와 같은 미디어 SD 카드, 예: 또는 비 시스템 볼륨을 구동 하는 외부 또는 이동식 저장소에 앱을 설치 하는 고객을 허용 하려면이 상자를 선택 합니다. (Windows Phone 8.1이 이전에 표시 된 StoreManifest.xml를 통해.)

대체 된 드라이브 또는 이동식 저장소에 설치 되 고에서 앱을 방지 하 고만 해당 장치에 내부 하드 드라이브에 설치를 허용 하려는 경우이 상자를 선택 취소 합니다.

메모는 앱 수 *만* 있도록 설치를 제한 하려면 옵션이 이동식 저장소 미디어를 설치 합니다.


## <a name="windows-can-include-this-apps-data-in-automatic-backups-to-onedrive"></a>Windows에서는 이 앱의 데이터를 OneDrive에 대한 자동 백업에 포함할 수 있습니다.

고객이 Windows에서 OneDrive에 자동으로 백업하도록 선택하면 앱 데이터를 포함할 수 있도록 이 상자가 기본적으로 선택됩니다. (Windows Phone 8.1이 이전에 표시 된 StoreManifest.xml를 통해.)

앱 데이터가 자동화된 백업에 포함되지 않게 하려면 이 상자를 선택 취소합니다.


## <a name="this-app-sends-kinect-data-to-external-services"></a>이 응용 프로그램 외부 서비스 Kinect 데이터를 보냅니다. 

앱 Kinect 데이터를 사용 하는 모든 외부 서비스에 보내는 하는 경우에이 상자를 선택 해야 합니다.



 

 

 




