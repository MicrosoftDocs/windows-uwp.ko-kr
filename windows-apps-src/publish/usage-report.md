---
author: jnHs
Description: "Windows 개발자 센터 대시보드의 사용 보고서에서는 고객이 앱을 사용하는 방식을 확인할 수 있습니다."
title: "사용 보고서"
ms.assetid: 5F0E7F94-D121-4AD3-A6E5-9C0DEC437BD3
ms.author: wdg-dev-content
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
translationtype: Human Translation
ms.sourcegitcommit: c6b64cff1bbebc8ba69bc6e03d34b69f85e798fc
ms.openlocfilehash: b3c225316b028baa9a499e81841cdb939be9588e
ms.lasthandoff: 02/07/2017

---

# <a name="usage-report"></a>사용 보고서


Windows 개발자 센터 대시보드의 **사용** 보고서에서는 Windows 10의 고객이 앱을 사용하는 방식을 확인하고 정의한 사용자 지정 이벤트에 대한 정보를 얻을 수 있습니다. 대시보드에서 이 데이터를 보거나 [보고서를 다운로드](download-analytic-reports.md)하여 오프라인으로 볼 수 있습니다.

> **참고** 이전에는 앱에서 Visual Studio Application Insights SDK를 활성화한 경우에만 **사용** 보고서에서 데이터를 제공했습니다. 업데이트된 **사용** 보고서에서는 이 작업이 더 이상 필요하지 않습니다.

## <a name="apply-filters"></a>필터 적용


페이지 위쪽에서 **필터 적용**을 확장하여 날짜 범위 및/또는 제품 그룹(관련 OS 버전)을 기준으로 이 페이지의 모든 데이터를 필터링할 수 있습니다.

-   **날짜**: 기본 필터는 **지난 30일**이지만 **지난 12개월**까지 이 필터를 확장할 수 있습니다.
-   **패키지 버전**: 기본 설정은 **모두**입니다. 앱에 패키지가 두 개 이상 포함되어 있을 경우 여기서 특정 버전을 선택할 수 있습니다.
-   **디바이스 유형**: 기본 설정은 **모두**이지만 하나의 특정 디바이스 유형에 대한 데이터만 표시하도록 선택할 수 있습니다.

아래에 나열된 모든 차트의 정보는 **필터 적용**에서 선택한 기간을 반영합니다. **필터 적용** 섹션을 사용하여 하나만 필터링하지 않는 한 기본적으로 여기에는 모든 패키지 버전 및 지원되는 디바이스 유형에 대한 데이터가 포함됩니다.

> **참고** Windows 10의 고객이 제공하는 사용 현황 데이터만 이 보고서에 포함됩니다.

## <a name="total-user-sessions"></a>총 사용자 세션

**총 사용자 세션** 차트는 선택한 기간의 일일 사용자 세션 수를 보여 줍니다.

각 사용자 세션은 고객이 앱을 조작한 고유 기간을 나타냅니다. 각 사용자 세션은 일정 기간의 비활성 상태 후 종료된 것으로 간주되므로 단일 고객이 동일한 날에 여러 사용자 세션을 사용할 수 있습니다. 이 차트는 고유한 앱 사용자를 추적하지 않습니다.

## <a name="active-users"></a>활성 사용자

**활성 사용자** 차트는 선택한 기간의 특정 날짜에 앱을 사용한 고객 수를 보여 줍니다.

각 활성 사용자는 해당 날짜에 앱을 사용한 고객을 나타냅니다. 이 차트는 고유한 사용자 세션을 추적하지 않습니다. 즉, 이 차트에서 고객은 해당 날짜에 앱을 한 번만 또는 여러 번 사용했는지를 나타냅니다.

## <a name="custom-events"></a>사용자 지정 이벤트

**사용자 지정 이벤트** 차트는 앱에 대해 정의한 사용자 지정 이벤트의 총 발생 횟수를 보여 줍니다. 여기에는 같은 고객에 대한 발생이 여러 번 포함될 수 있습니다.

사용자 지정 이벤트는 [Microsoft Store Services SDK](../monetize/microsoft-store-services-sdk.md)의 [StoreServicesCustomEventLogger.Log](https://msdn.microsoft.com/library/windows/apps/microsoft.services.store.engagement.storeservicescustomeventlogger.log.aspx) 메서드를 사용하여 구현됩니다.



 

