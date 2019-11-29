---
Description: Microsoft Store에서 앱에 할당 한 고유 id와 관련 된 세부 정보를 확인 하 고 앱의 스토어 목록에 대 한 링크를 가져옵니다.
title: 앱 ID 세부 정보 보기
ms.assetid: 86F05A79-EFBC-4705-9A71-3A056323AC65
ms.date: 10/02/2018
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 07c2d3308d204d37e246a9a56c0a7203a1340dc0
ms.sourcegitcommit: b52ddecccb9e68dbb71695af3078005a2eb78af1
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/20/2019
ms.locfileid: "74259886"
---
# <a name="view-app-identity-details"></a>앱 ID 세부 정보 보기


앱 **id** 페이지에서 Microsoft Store 하 여 앱에 할당 된 고유 id와 관련 된 세부 정보를 볼 수 있습니다. 또한이 페이지에서 앱의 스토어 목록에 대 한 링크를 가져올 수 있습니다.

이 정보를 찾으려면 앱 중 하나로 이동하여 왼쪽 탐색 메뉴에서 **앱 관리**를 확장합니다. 그리고 **앱 ID**를 선택하여 이들 세부 정보를 확인합니다.


## <a name="values-to-include-in-your-app-package-manifest"></a>앱 패키지 매니페스트에 포함시킬 값

패키지 매니페스트에는 다음 값이 포함 되어야 합니다. [Microsoft Visual Studio를 사용하여 패키지를 빌드](/windows/msix/package/packaging-uwp-apps)하고 개발자 계정에 연결된 동일한 Microsoft 계정으로 로그인을 하면 이들 세부 정보가 자동으로 포함됩니다. 패키지를 수동으로 빌드할 경우에는 이들 항목을 반드시 추가해야 합니다.

-   **패키지/Id/이름**
-   **패키지/i d/게시자**
-   **패키지/속성/Publisherdisplayname 문자열과**

자세한 내용은 [패키지 매니페스트 스키마 참조**의** ](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/element-identity)ID[](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/schema-root) 섹션을 참조하세요.

이들 요소는 앱의 ID를 선언하여 패키지가 속한 "패키지 패밀리"를 설정합니다. 개별 패키지에는 아키텍처 및 버전과 같은 추가 세부 정보가 포함됩니다.


## <a name="additional-values-for-package-family"></a>패키지 패밀리에 대한 추가 값

다음 값은 앱의 패키지 패밀리를 나타내는 추가적인 값이지만 매니페스트에 포함되지 않습니다.

-   **PFN(패키지 패밀리 이름)** : 이 값은 특정 Windows API와 함께 사용합니다.
-   **패키지 SID**: WNS 알림을 앱에 보내려면 이 값이 필요합니다. 자세한 내용은 [WNS(Windows 푸시 알림 서비스) 개요](../design/shell/tiles-and-notifications/windows-push-notification-services--wns--overview.md)를 참조하세요.


## <a name="link-to-your-apps-listing"></a>앱 목록 링크

앱 페이지에 대한 직접 링크를 공유하면 고객이 스토어에서 앱을 찾는 데 도움이 됩니다. 이 링크는 **`https://www.microsoft.com/store/apps/<your app's Store ID>`** 형식으로 되어 있습니다. 고객이 이 링크를 클릭하면 앱에 대한 웹 기반 목록 페이지가 열립니다. 또한 Windows 장치에서 스토어 앱이 시작되고 앱 목록을 표시합니다.

앱의 **스토어 ID**도 이 섹션에 표시됩니다. 이 스토어 ID는 [스토어 배지를 생성](https://developer.microsoft.com/store/badges)하거나 그렇지 않으면 앱을 식별하는 데 사용할 수 있습니다.

**스토어 프로토콜 링크**는 앱 내에서 연결을 할 때처럼 브라우저를 열지 않고도 스토어에서 앱을 직접 연결하는 데 사용됩니다. 자세한 내용은 [앱에 대한 링크](link-to-your-app.md)를 참조하세요.



 

 




