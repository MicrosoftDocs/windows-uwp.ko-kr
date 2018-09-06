---
author: jnHs
Description: If you've provided packages targeting different operating systems, you have the option to customize parts of your Store listing for different targeted operating systems.
title: 플랫폼별 Store 목록 만들기
ms.assetid: 5BE66BE2-669C-49E0-8915-60F1027EF94A
ms.author: wdg-dev-content
ms.date: 3/13/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: Windows 10, UWP, 사용자 지정, 목록, 설명, 이전
ms.localizationpriority: medium
ms.openlocfilehash: 6f30a825cc7aec1b6f7dbf5cff0ea1c17c43ffd7
ms.sourcegitcommit: 914b38559852aaefe7e9468f6f53a7465bf36e30
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 09/06/2018
ms.locfileid: "3402265"
---
# <a name="create-platform-specific-store-listings"></a>플랫폼별 Store 목록 만들기


앱에 다른 운영 체제를 대상으로 하는 패키지가 있는 경우, 이전 OS 버전(Windows 8.x 이하 및/또는 Windows Phone 8.x 이하) 고객을 위한 Store 목록 부분을 사용자 설정할 수 있습니다. 

> [!IMPORTANT]
> Windows 10 기반 고객에게는 항상 기본 [Store 목록](create-app-store-listings.md)이 표시됩니다. 하나 이상의 이전 OS 버전을 지원하는 패키지를 이미 업로드하지 않은 한, 플랫폼별 Store 목록을 만드는 옵션이 표시되지 않습니다. 

플랫폼별 스토어 목록은 모든 고객에게 동일한 스토어 목록을 표시하지 않고 하나의 OS 버전에서만 표시되는 기능을 설명하거나, 특정 OS(디바이스 유형에 독립적)에 특정한 스크린샷을 제공하려는 경우에 유용할 수 있습니다.

> [!NOTE]
> 플랫폼별 Store 목록을 한 언어로 만들면 앱에서 지원하는 다른 언어로는 플랫폼별 Store 목록이 만들어지지 않습니다. 각 언어에 대해 플랫폼별 스토어 목록을 개별적으로 만들어야 합니다. 또한 플랫폼별 목록에 대한 Store 목록을 가져오고 내보낼 수 없습니다.


## <a name="creating-a-platform-specific-store-listing"></a>플랫폼별 스토어 목록 만들기

앱이 이전 OS 버전(Windows 8.x 이하 및/또는 Windows Phone 8.x 이하)을 지원하는 경우 **Store 목록** 페이지의 위쪽에 **플랫폼별 앱 Store 목록 만들기**를 선택할 수 있습니다. 

> [!TIP]
> 팁 패키지를 업로드한 후에 플랫폼별 Store 목록을 만들 수 있습니다.

이 옵션을 선택한 후에는 제출이 지원하는 대상 OS 버전으로부터 선택하라는 메시지가 표시됩니다. 이미 앱이 대상으로 하고 있는 모든 OS 버전에 대한 플랫폼별 Store 목록을 만든 경우 선택할 수 없습니다. (Windows 10 고객에게는 항상 앱의 기본 Store 목록이 표시되므로 Windows 10은 선택 목록에 포함되어 있지 않습니다.)

기본 Store 목록을 시작점으로 사용하면 기본 Store 목록에 입력한 적용되는 모든 텍스트와 이미지를 가져옵니다. 저장하기 전에 원하는 내용을 변경할 수 있습니다. 원하는 경우 완전히 빈 스토어 목록에서 시작할 수도 있습니다.

**계속**을 클릭하면, **Store 목록** 페이지에 생성한 플랫폼별 Store 목록에 해당되는 섹션이 포함됩니다. 이 섹션에는 **설명**(필수), **이 버전의 새로운 기능**, **스크린샷**, **앱 타일 아이콘**, **앱 기능** 및 **추가 시스템 요구 사항**에 대한 고유한 필드 집합이 포함되어 있습니다. 기본 스토어 목록과 정보가 동일한 경우에도 사용자 지정 스토어 목록에 표시할 정보를 각 필드에 입력해야 합니다. 이러한 필드를 비워 두면 사용자 지정 스토어 목록의 해당 필드에 아무 정보도 표시되지 않습니다.


> [!IMPORTANT]
> Store 목록의 [추가 정보](create-app-store-listings.md#additional-information) 섹션에 있는 필드들은 서로 다른 OS 버전에서 사용자 지정을 할 수 없습니다.
> 
> 뿐만 아니라 기본 [Store 목록](create-app-store-listings.md) 페이지의 일부 필드들은 Windows 10의 고객에게만 적용되기 때문에 플랫폼별 Store 목록을 만들 때 표시되는 옵션이 다를 수 있습니다. 예를 들어, 플랫폼별 Store 목록에 예고편을 추가하지 못할 수 있습니다. Windows 10 1607 이후 버전 고객에게만 예고편이 표시되기 때문입니다. 


## <a name="removing-a-platform-specific-store-listing"></a>플랫폼별 Store 목록 제거

플랫폼별 Store 목록을 만들고 나중에 해당 운영 체제의 고객에게 기본 Store 목록을 표시하려는 경우 해당 목록 옆의 **삭제** 링크를 선택합니다.

해당 고객에게 기본 Store 목록을 표시하도록 확인한 후에 **확인**을 선택합니다. 플랫폼별 Store 목록이 삭제되고(해당하는 모든 언어에 대해), 이제 해당 OS 버전 사용 고객에게 기본 Store 목록이 표시됩니다. 마음이 바뀌면 위 단계에 따라 해당 운영 체제에 대한 다른 플랫폼별 Store 목록을 만들 수 있습니다.

 

 




