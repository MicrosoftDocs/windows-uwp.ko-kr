---
Description: 여러 운영 체제를 대상으로 하는 패키지를 제공한 경우 여러 대상 운영 체제에 대한 스토어 목록 일부를 사용자 지정할 수 있습니다.
title: 플랫폼별 스토어 목록 만들기
ms.assetid: 5BE66BE2-669C-49E0-8915-60F1027EF94A
ms.date: 10/31/2018
ms.topic: article
keywords: Windows 10, UWP, 사용자 지정, 목록, 설명, 이전
ms.localizationpriority: medium
ms.openlocfilehash: 2d9a9e86397ca5e85697543f99481b43f438a063
ms.sourcegitcommit: 4aef8c01ba9321401d5729a1ec6d46452ee76faf
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 06/29/2019
ms.locfileid: "67468972"
---
# <a name="create-platform-specific-store-listings"></a>플랫폼별 스토어 목록 만들기


이전 OS 버전에는 고객에 대 한 저장소 목록 부분을 사용자 지정할 수 있는 경우 이전에 게시 된 앱에 서로 다른 운영 체제를 대상으로 하는 패키지 (Windows 8.x 또는 이전 버전 및/또는 Windows Phone 8.x 또는 이전 버전). 

Windows 10 (Xbox 포함)에서 고객에 게는 항상 기본 표시 [스토어 목록](create-app-store-listings.md)합니다. 하나 이상의 이전 OS 버전을 지 원하는 패키지를 사용 하 여 앱을 이미 게시 하지 않는 한 플랫폼별 스토어 목록을 만드는 옵션을 볼 수 없습니다. 

> [!IMPORTANT]
> 2018 년 10 월 31 일부 터 새로 만든 제품 Windows 8.x/Windows를 대상으로 하는 패키지를 포함할 수 없습니다 Phone 8.x 또는 이전 버전입니다. 자세한 내용은 참조 [블로그 게시물](https://blogs.windows.com/windowsdeveloper/2018/08/20/important-dates-regarding-apps-with-windows-phone-8-x-and-earlier-and-windows-8-8-1-packages-submitted-to-microsoft-store)합니다.

플랫폼별 스토어 목록 (장치 유형에 관계 없이)는 특정 OS와 관련이 있는 스크린 샷을 제공 하려면 한 OS 버전에만 표시 되는 기능을 언급 하려고 하는 경우 유용할 수 있습니다.

> [!NOTE]
> 만들기, 플랫폼별 하나의 언어로 나열 하는 저장소 만들어지지는지 않습니다 플랫폼별 스토어 목록 앱을 지 원하는 다른 언어에서. 각 언어에 대해 플랫폼별 스토어 목록을 개별적으로 만들어야 합니다. 할 수 있는 참고 수도 [가져오기 및 내보내기 데이터를 나열 하는 저장소](import-and-export-store-listings.md) 플랫폼별 목록에 대 한 합니다.


## <a name="creating-a-platform-specific-store-listing"></a>플랫폼별 스토어 목록 만들기

맨 위 근처에 **스토어 목록** 페이지에서 이전에 게시 된 앱에는 이전 OS 버전을 지 원하는 패키지를 포함 하는 경우 ((Windows 8.x 또는 이전 버전 및/또는 Windows Phone 8.x 또는 이전 버전)을 선택할 수 있습니다 **만들기를 플랫폼별 앱 스토어 목록**합니다. 이 옵션을 선택한 후에는 제출이 지원하는 대상 OS 버전으로부터 선택하라는 메시지가 표시됩니다. 이미 만든 경우 모든 이전 OS 버전에 대 한 플랫폼별 스토어 목록 앱의 대상, 다른를 선택할 수 없습니다.

(Windows 10) 기본 저장소 목록을 나열 합니다; 해당 텍스트 및 이미지를 기본 저장소에 대해 입력 한 표시 됩니다는 시작 점으로 사용할 수 있습니다. 그런 다음 저장 하기 전에 원하는 대로 변경할 수 있습니다. 원하는 경우 완전히 빈 스토어 목록에서 시작할 수도 있습니다.

**계속**을 클릭하면, **Store 목록** 페이지에 생성한 플랫폼별 Store 목록에 해당되는 섹션이 포함됩니다. 이 섹션에는 **설명**(필수), **이 버전의 새로운 기능**, **스크린샷**, **앱 타일 아이콘**, **앱 기능** 및 **추가 시스템 요구 사항**에 대한 고유한 필드 집합이 포함되어 있습니다. 기본 스토어 목록과 정보가 동일한 경우에도 사용자 지정 스토어 목록에 표시할 정보를 각 필드에 입력해야 합니다. 이러한 필드를 비워 두면 사용자 지정 스토어 목록의 해당 필드에 아무 정보도 표시되지 않습니다.

> [!IMPORTANT]
> Store 목록의 [추가 정보](create-app-store-listings.md#additional-information) 섹션에 있는 필드들은 서로 다른 OS 버전에서 사용자 지정을 할 수 없습니다.
> 
> 뿐만 아니라 기본 [Store 목록](create-app-store-listings.md) 페이지의 일부 필드들은 Windows 10의 고객에게만 적용되기 때문에 플랫폼별 Store 목록을 만들 때 표시되는 옵션이 다를 수 있습니다. 예를 들어, 플랫폼별 Store 목록에 예고편을 추가하지 못할 수 있습니다. Windows 10 1607 이후 버전 고객에게만 예고편이 표시되기 때문입니다. 

특정 OS 버전에는 고객에 대 한 변경할 수 있도록 필요에 따라 플랫폼별 목록 편집 할 수 있습니다.


## <a name="removing-a-platform-specific-store-listing"></a>플랫폼별 스토어 목록 제거

플랫폼별 Store 목록을 만들고 나중에 해당 운영 체제의 고객에게 기본 Store 목록을 표시하려는 경우 해당 목록 옆의 **삭제** 링크를 선택합니다.

해당 고객에게 기본 Store 목록을 표시하도록 확인한 후에 **확인**을 선택합니다. 플랫폼별 Store 목록이 삭제되고(해당하는 모든 언어에 대해), 이제 해당 OS 버전 사용 고객에게 기본 Store 목록이 표시됩니다. 마음이 바뀌면 위 단계에 따라 해당 운영 체제에 대한 다른 플랫폼별 Store 목록을 만들 수 있습니다.
 

 




