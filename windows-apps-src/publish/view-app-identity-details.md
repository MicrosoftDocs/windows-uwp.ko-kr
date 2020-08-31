---
Description: Microsoft Store에서 앱에 할당 한 고유 id와 관련 된 세부 정보를 확인 하 고 앱의 스토어 목록에 대 한 링크를 가져옵니다.
title: 앱 ID 세부 정보 보기
ms.assetid: 86F05A79-EFBC-4705-9A71-3A056323AC65
ms.date: 10/02/2018
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 9232acbf83659c661e1b1f3c35a7fb7ad546e819
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89157927"
---
# <a name="view-app-identity-details"></a>앱 ID 세부 정보 보기


앱 **id** 페이지에서 Microsoft Store 하 여 앱에 할당 된 고유 id와 관련 된 세부 정보를 볼 수 있습니다. 또한이 페이지에서 앱의 스토어 목록에 대 한 링크를 가져올 수 있습니다.

이 정보를 찾으려면 앱 중 하나로 이동한 다음 왼쪽 탐색 메뉴에서 **앱 관리** 를 확장 합니다. 이러한 세부 정보를 보려면 **앱 id** 를 선택 합니다.


## <a name="values-to-include-in-your-app-package-manifest"></a>앱 패키지 매니페스트에 포함할 값

패키지 매니페스트에는 다음 값이 포함 되어야 합니다. [Microsoft Visual Studio를 사용](/windows/msix/package/packaging-uwp-apps)하 여 패키지를 빌드하고 개발자 계정에 연결 된 것과 동일한 Microsoft 계정로 로그인 한 경우 이러한 세부 정보가 자동으로 포함 됩니다. 패키지를 수동으로 작성 하는 경우 다음 항목을 추가 해야 합니다.

-   **패키지/Id/이름**
-   **패키지/i d/게시자**
-   **패키지/속성/Publisherdisplayname 문자열과**

자세한 내용은 [패키지 매니페스트 스키마 참조](/uwp/schemas/appxpackage/uapmanifestschema/schema-root)에서 [**id**](/uwp/schemas/appxpackage/uapmanifestschema/element-identity) 를 참조 하세요.

이러한 요소는 모두 응용 프로그램의 id를 선언 하 여 모든 패키지가 속하는 "패키지 패밀리"를 설정 합니다. 개별 패키지에는 아키텍처 및 버전과 같은 추가 정보가 포함 됩니다.


## <a name="additional-values-for-package-family"></a>패키지 패밀리의 추가 값

다음 값은 앱의 패키지 패밀리를 참조 하지만 매니페스트에는 포함 되지 않는 추가 값입니다.

-   **PFN (패키지 패밀리 이름)**:이 값은 특정 Windows api와 함께 사용 됩니다.
-   **패키지 SID**: 응용 프로그램에 WNS 알림을 보내려면이 값이 필요 합니다. 자세한 내용은 [WNS (Windows 푸시 Notification Services) 개요](../design/shell/tiles-and-notifications/windows-push-notification-services--wns--overview.md)를 참조 하세요.


## <a name="link-to-your-apps-listing"></a>앱 목록에 대 한 링크

앱 페이지에 대 한 직접 링크를 공유 하 여 고객이 스토어에서 앱을 찾을 수 있도록 할 수 있습니다. 이 링크는 형식입니다 **`https://www.microsoft.com/store/apps/<your app's Store ID>`** . 고객이이 링크를 클릭 하면 앱에 대 한 웹 기반 목록 페이지가 열립니다. Windows 장치에서 스토어 앱이 시작 되 고 앱의 목록도 표시 됩니다.

앱의 **스토어 ID** 도이 섹션에 나와 있습니다. 이 저장소 ID를 사용 하 여 [매장 배지를 생성](https://developer.microsoft.com/store/badges) 하거나 다른 방법으로 앱을 식별할 수 있습니다.

**저장소 프로토콜 링크** 를 사용 하 여 앱 내에서 연결 하는 경우와 같이 브라우저를 열지 않고 스토어의 앱에 직접 연결할 수 있습니다. 자세한 내용은 [앱에 연결](link-to-your-app.md)을 참조 하세요.



 

 