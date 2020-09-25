---
Description: 서로 다른 운영 체제를 대상으로 하는 패키지를 제공한 경우 다른 대상 운영 체제에 대 한 매장 목록의 일부를 사용자 지정할 수 있습니다.
title: 플랫폼별 스토어 목록 만들기
ms.assetid: 5BE66BE2-669C-49E0-8915-60F1027EF94A
ms.date: 09/24/2020
ms.topic: article
keywords: windows 10, uwp, 사용자 지정, 목록, 설명, 이전
ms.localizationpriority: medium
ms.openlocfilehash: b0008095ac027a1ef1d17b655610a3ad50a6596b
ms.sourcegitcommit: eda7bbe9caa9d61126e11f0f1a98b12183df794d
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 09/24/2020
ms.locfileid: "91220506"
---
# <a name="create-platform-specific-store-listings"></a>플랫폼별 스토어 목록 만들기


이전에 게시 된 앱에 서로 다른 운영 체제를 대상으로 하는 패키지가 있는 경우 이전 OS 버전 (Windows 8.x 또는 이전 및/또는 Windows Phone .x 이전 버전)에서 고객을 위해 스토어의 일부를 사용자 지정 하는 옵션이 있습니다. 

Windows 10 (Xbox 포함)의 고객에 게는 항상 기본 [매장 목록이](create-app-store-listings.md)표시 됩니다. 하나 이상의 이전 OS 버전을 지 원하는 패키지를 사용 하 여 앱을 이미 게시 한 경우를 제외 하 고 플랫폼별 저장소 목록을 만드는 옵션은 표시 되지 않습니다. 

> [!IMPORTANT]
> Windows Phone .x SDK를 사용 하 여 빌드된 새 XAP 패키지는 더 이상 업로드할 수 없습니다. 이미 XAP 패키지를 사용 하 여 저장 된 앱은 Windows 10 Mobile 장치에서 계속 작동 합니다. 자세한 내용은이 [블로그 게시물](https://blogs.windows.com/windowsdeveloper/2018/08/20/important-dates-regarding-apps-with-windows-phone-8-x-and-earlier-and-windows-8-8-1-packages-submitted-to-microsoft-store)을 참조 하세요.

특정 OS 버전에만 표시 되는 기능을 언급 하거나 특정 OS와 관련 된 스크린샷 (장치 유형과 무관)을 제공 하려는 경우 플랫폼별 저장소 목록이 유용할 수 있습니다.

> [!NOTE]
> 플랫폼별 저장소 목록을 한 언어로 만들면 앱이 지 원하는 다른 언어의 플랫폼별 저장소가 생성 되지 않습니다. 플랫폼별 저장소는 각 언어에 대해 개별적으로 나열 해야 합니다. 또한 플랫폼별 목록에 대 한 [저장소 목록 데이터를 가져오거나 내보낼](import-and-export-store-listings.md) 수 없습니다.


## <a name="creating-a-platform-specific-store-listing"></a>플랫폼별 저장소 목록 만들기

이전에 게시 된 앱에 이전 OS 버전을 지 원하는 패키지 ((Windows 8.x 또는 이전 버전 및/또는 Windows Phone .x 또는 이전 버전)가 포함 된 경우 **스토어 목록** 페이지의 맨 위에 있는 **플랫폼 특정 앱 스토어 목록 만들기**를 선택할 수 있습니다. 이 옵션을 선택 하면 전송에서 지 원하는 대상 OS 버전을 선택 하 라는 메시지가 표시 됩니다. 앱이 대상으로 하는 모든 이전 OS 버전에 대해 플랫폼별 저장소 목록을 이미 만든 경우에는 다른 항목을 선택할 수 없습니다.

기본 (Windows 10) 저장소 목록을 시작 점으로 사용 하 여 기본 저장소 목록에 대해 입력 한 해당 텍스트 및 이미지를 가져올 수 있습니다. 그런 다음 저장 하기 전에 원하는 대로 변경할 수 있습니다. 원한다 면 완전히 빈 상점 목록에서 시작할 수도 있습니다.

**계속**을 클릭 하면 **스토어 목록** 페이지에 방금 만든 플랫폼별 매장 목록에 대 한 섹션이 포함 됩니다. 이 섹션에는 **설명** (필수)의 고유한 필드 집합, **이 버전의 새로운 기능**, **스크린샷**, **앱 타일 아이콘**, **앱 기능**및 **추가 시스템 요구 사항이**포함 됩니다. 사용자 지정 매장 목록에서 정보를 표시 하려는 각 필드에 정보를 입력 해야 합니다 .이 정보는 기본 매장 목록에서와 동일한 정보를 포함 하 고 있는 경우에도 마찬가지입니다. 이러한 필드 중 하나를 비워 두면 사용자 지정 매장 목록에 해당 필드에 대 한 정보가 표시 되지 않습니다.

> [!IMPORTANT]
> 스토어 목록의 [추가 정보](create-app-store-listings.md#additional-information) 섹션에 있는 필드는 다른 OS 버전에 맞게 사용자 지정할 수 없습니다.
> 
> 또한 기본 [저장소 목록](create-app-store-listings.md) 페이지의 일부 필드는 Windows 10의 고객 에게만 적용 되므로 플랫폼별 저장소 목록을 만들 때 동일한 옵션이 모두 표시 되지 않습니다. 예를 들어 트레일러는 Windows 10, 버전 1607 이상에 있는 고객만 표시 되기 때문에 플랫폼별 저장소 목록에 트레일러를 추가할 수 없습니다. 

특정 OS 버전에서 고객을 변경 하기 위해 필요에 따라 플랫폼별 목록을 계속 편집할 수 있습니다.


## <a name="removing-a-platform-specific-store-listing"></a>플랫폼별 저장소 목록 제거

플랫폼별 저장소 목록을 만들고 나중에 해당 운영 체제의 고객에 게 기본 매장 목록을 표시 하는 경우 해당 목록 옆에 있는 **삭제** 링크를 선택 합니다.

기본 상점 목록에서 해당 고객을 표시 하 시겠습니까를 확인 한 후 **확인**을 선택 합니다. 플랫폼별 저장소 목록이 제거 되 고 (해당 하는 모든 언어에 대해) 해당 OS 버전의 고객은 이제 기본 매장 목록을 볼 수 있습니다. 마음이 바뀌면 위에 나열 된 단계를 수행 하 여 해당 운영 체제에 대 한 다른 플랫폼별 저장소 목록을 만들 수 있습니다.
 

 




