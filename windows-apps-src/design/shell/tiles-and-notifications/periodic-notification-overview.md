---
description: 정기 알림(폴링된 알림이라고도 함)은 고정된 간격에 따라 클라우드 서비스에서 콘텐츠를 다운로드하여 타일 및 배지를 업데이트합니다.
title: 정기 알림 개요
ms.assetid: 1EB79BF6-4B94-451F-9FAB-0A1B45B4D01C
template: detail.hbs
ms.date: 05/19/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 5860c4e45a25b0d141e1b2c57421dd2640d5fbb4
ms.sourcegitcommit: a3bbd3dd13be5d2f8a2793717adf4276840ee17d
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/30/2020
ms.locfileid: "93033786"
---
# <a name="periodic-notification-overview"></a>정기 알림 개요
 


정기 알림(폴링된 알림이라고도 함)은 고정된 간격에 따라 클라우드 서비스에서 콘텐츠를 다운로드하여 타일 및 배지를 업데이트합니다. 정기 알림을 사용 하려면 클라이언트 앱 코드에서 다음 두 가지 정보를 제공 해야 합니다.

-   응용 프로그램에 대 한 타일 또는 배지 업데이트를 폴링하기 위한 Windows 웹 위치의 URI (Uniform Resource Identifier)입니다.
-   URI를 폴링 해야 하는 빈도

정기 알림을 사용 하면 앱에서 최소 클라우드 서비스 및 클라이언트 투자로 라이브 타일 업데이트를 가져올 수 있습니다. 정기적 알림은 동일한 콘텐츠를 광범위 한 사용자에 게 배포 하는 좋은 배달 방법입니다.

**참고**   Windows 8.1에 대 한 [푸시 및 정기 알림 샘플](https://github.com/microsoftarchive/msdn-code-gallery-microsoft/tree/411c271e537727d737a53fa2cbe99eaecac00cc0/Official%20Windows%20Platform%20Sample/Windows%208%20app%20samples/%5BC%23%5D-Windows%208%20app%20samples/C%23/Windows%208%20app%20samples/Push%20and%20periodic%20notifications%20client-side%20sample%20(Windows%208)) 을 다운로드 하 고 Windows 10 앱에서 소스 코드를 다시 사용 하 여 자세히 알아볼 수 있습니다.

 

## <a name="how-it-works"></a>작동 방법


정기적 알림은 앱이 클라우드 서비스를 호스트 해야 합니다. 앱이 설치 된 모든 사용자가 서비스를 주기적으로 폴링합니다. 각 폴링 간격 (예: 한 시간에 한 번)에서 Windows는 URI에 HTTP GET 요청을 보내고 요청에 대 한 응답으로 제공 된 요청 된 타일 또는 배지 콘텐츠 (XML)를 다운로드 하 고 앱의 타일에 콘텐츠를 표시 합니다.

정기적 업데이트는 알림 메시지와 함께 사용할 수 없습니다. 알림 메시지는 [예약](/previous-versions/windows/apps/hh465417(v=win.10)) 된 알림 또는 [푸시](/previous-versions/windows/apps/hh868252(v=win.10)) 알림을 통해 가장 잘 배달 됩니다.

## <a name="uri-location-and-xml-content"></a>URI 위치 및 XML 콘텐츠


유효한 모든 HTTP 또는 HTTPS 웹 주소는 폴링할 URI로 사용할 수 있습니다.

클라우드 서버의 응답은 다운로드 한 콘텐츠를 포함 합니다. URI에서 반환 된 콘텐츠는 [Tile](adaptive-tiles-schema.md) 또는 [배지](/uwp/schemas/tiles/badgeschema/schema-root) XML 스키마 사양과 일치 해야 하며 u t f-8로 인코딩해야 합니다. 정의 된 HTTP 헤더를 사용 하 여 알림에 대 한 [만료 시간](#expiration-of-tile-and-badge-notifications) 또는 태그를 지정할 수 있습니다.

## <a name="polling-behavior"></a>폴링 동작


다음 메서드 중 하나를 호출 하 여 폴링을 시작 합니다.

-   [**StartPeriodicUpdate**](/uwp/api/Windows.UI.Notifications.TileUpdater#Windows_UI_Notifications_TileUpdater_StartPeriodicUpdate_Windows_Foundation_Uri_Windows_Foundation_DateTime_Windows_UI_Notifications_PeriodicUpdateRecurrence_) (타일)
-   [**StartPeriodicUpdate**](/uwp/api/Windows.UI.Notifications.BadgeUpdater#Windows_UI_Notifications_BadgeUpdater_StartPeriodicUpdate_Windows_Foundation_Uri_Windows_Foundation_DateTime_Windows_UI_Notifications_PeriodicUpdateRecurrence_) (배지)
-   [**StartPeriodicUpdateBatch**](/uwp/api/Windows.UI.Notifications.TileUpdater#Windows_UI_Notifications_TileUpdater_StartPeriodicUpdateBatch_Windows_Foundation_Collections_IIterable_1_Windows_UI_Notifications_PeriodicUpdateRecurrence_) (타일)

이러한 메서드 중 하나를 호출 하면 URI가 즉시 폴링됩니다. 타일 또는 배지는 받은 내용으로 업데이트 됩니다. 이 초기 폴링 후에는 Windows에서 요청 된 간격으로 업데이트를 계속 제공 합니다. 폴링은 명시적으로 중지 ( [**TileUpdater**](/uwp/api/Windows.UI.Notifications.TileUpdater.StopPeriodicUpdate)사용) 하거나, 앱이 제거 되거나, 보조 타일의 경우 타일이 제거 될 때까지 계속 됩니다. 그렇지 않으면 앱을 다시 시작 하지 않아도 Windows에서 타일 또는 배지 업데이트를 계속 폴링합니다.

### <a name="the-recurrence-interval"></a>되풀이 간격

되풀이 간격을 위에 나열 된 방법의 매개 변수로 지정 합니다. Windows에서 요청 된 대로 폴링할 수 있는 최상의 노력을 주기 때문에 간격이 정확 하지 않습니다. 요청 된 폴링 간격은 Windows의 재량에 따라 최대 15 분까지 지연 될 수 있습니다.

### <a name="the-start-time"></a>시작 시간

필요에 따라 특정 시간을 지정 하 여 폴링을 시작할 수 있습니다. 응용 프로그램에서 1 일에 한 번만 타일 콘텐츠를 변경 하는 것이 좋습니다. 이 경우 클라우드 서비스를 업데이트 하는 시간에 가까운 시간을 폴링하는 것이 좋습니다. 예를 들어 일별 쇼핑 사이트에서 매일 오전 8 시에 제공 하는 제품을 게시 하는 경우 오전 8 시에 새로운 타일 콘텐츠를 폴링합니다.

시작 시간을 제공 하는 경우 메서드를 처음 호출 하면 콘텐츠를 즉시 폴링합니다. 그런 다음 일반 폴링은 제공 된 시작 시간의 15 분 이내에 시작 됩니다.

### <a name="automatic-retry-behavior"></a>자동 다시 시도 동작

이 URI는 장치가 온라인 상태인 경우에만 폴링됩니다. 네트워크를 사용할 수 있지만 어떤 이유로 든 URI에 연결할 수 없는 경우 폴링 간격의이 반복을 건너뛰고 다음 간격에 URI가 다시 폴링 됩니다. 폴링 간격에 도달 했을 때 장치가 꺼짐, 절전 또는 최대 절전 모드로 상태 이면 장치가 꺼짐 또는 절전 상태에서 반환 될 때 URI가 폴링합니다.

### <a name="handling-app-updates"></a>앱 업데이트 처리

폴링 URI를 변경 하는 앱 업데이트를 해제 하는 경우 새 URI를 사용 하 여 StartPeriodicUpdate를 호출 하는 일별 [시간 트리거 백그라운드 작업](../../../launch-resume/run-a-background-task-on-a-timer-.md) 을 추가 하 여 타일이 새 uri를 사용 하는지 확인 해야 합니다. 그렇지 않으면 사용자가 앱 업데이트를 수신 하지만 앱을 시작 하지 않는 경우 해당 타일은 여전히 이전 URI를 사용 하 고 있습니다 .이는 URI가 유효 하지 않은 경우 또는 반환 된 페이로드가 더 이상 존재 하지 않는 로컬 이미지를 참조 하는 경우 표시 되지 않을 수 있습니다.

## <a name="expiration-of-tile-and-badge-notifications"></a>타일 및 배지 알림 만료


기본적으로 정기 타일 및 배지 알림은 다운로드 된 시간부터 3 일 후에 만료 됩니다. 알림이 만료 되 면 콘텐츠는 배지, 타일 또는 큐에서 제거 되며 더 이상 사용자에 게 표시 되지 않습니다. 앱 또는 알림에 적합 한 시간을 사용 하 여 모든 주기적인 타일 및 배지 알림에 대 한 명시적 만료 시간을 설정 하는 것이 가장 좋은 방법입니다 .이를 통해 콘텐츠가 관련 된 것 보다 오래 지속 되지 않도록 할 수 있습니다. 지정 된 수명 범위의 콘텐츠에는 명시적 만료 시간이 필요 합니다. 또한 클라우드 서비스에 연결할 수 없는 경우 또는 사용자가 오랫동안 네트워크에서 연결을 해제 하는 경우에는 오래 된 콘텐츠가 제거 됩니다.

클라우드 서비스는 응답 페이로드에 X WNS-Expires HTTP 헤더를 포함 하 여 알림에 대 한 만료 날짜 및 시간을 설정 합니다. X WNS-Expires HTTP 헤더는 [http 날짜 형식을](https://www.w3.org/Protocols/rfc2616/rfc2616-sec3.html#sec3.3.1)준수 합니다. 자세한 내용은 [**StartPeriodicUpdate**](/uwp/api/Windows.UI.Notifications.TileUpdater#Windows_UI_Notifications_TileUpdater_StartPeriodicUpdate_Windows_Foundation_Uri_Windows_Foundation_DateTime_Windows_UI_Notifications_PeriodicUpdateRecurrence_) 또는 [**StartPeriodicUpdateBatch**](/uwp/api/Windows.UI.Notifications.TileUpdater#Windows_UI_Notifications_TileUpdater_StartPeriodicUpdateBatch_Windows_Foundation_Collections_IIterable_1_Windows_UI_Notifications_PeriodicUpdateRecurrence_)를 참조 하세요.

예를 들어, 주식 시장의 활성 거래 시간 중에는 주가 업데이트에 대 한 만료 시간을 폴링 간격의 두 배 (예: 1 분 마다 폴링 하는 경우 수령 후 1 시간)로 설정할 수 있습니다. 또 다른 예로, 뉴스 앱은 일일 뉴스 타일 업데이트에 대해 하루에 적절 한 만료 시간을 결정할 수 있습니다.

## <a name="periodic-notifications-in-the-notification-queue"></a>알림 큐의 정기 알림


[알림 순환을](/previous-versions/windows/apps/hh781199(v=win.10))통해 정기적으로 타일 업데이트를 사용할 수 있습니다. 기본적으로 시작 화면의 타일에는 새 알림으로 바뀔 때까지 단일 알림의 콘텐츠가 표시 됩니다. 순환을 사용 하도록 설정 하면 최대 5 개의 알림이 큐에서 유지 관리 되 고 타일이이를 순환 합니다.

큐가 5 개의 알림 용량에 도달 하면 다음 새 알림이 큐에서 가장 오래 된 알림을 대체 합니다. 그러나 알림에 태그를 설정 하 여 큐의 교체 정책에 영향을 줄 수 있습니다. 태그는 응답 페이로드의 [X WNS 태그](/previous-versions/windows/apps/hh465435(v=win.10)) HTTP 헤더에 지정 된, 최대 16 개의 영숫자 문자로 구성 된 대/소문자를 구분 하지 않는 문자열입니다. Windows는 들어오는 알림의 태그를 큐에 이미 있는 모든 알림의 태그와 비교 합니다. 일치 하는 항목이 발견 되 면 새 알림이 같은 태그로 대기 중인 알림을 대체 합니다. 일치 하는 항목이 없으면 기본 대체 규칙이 적용 되 고 새 알림이 큐에서 가장 오래 된 알림을 대체 합니다.

알림 큐 및 태그 지정을 사용 하 여 다양 한 다양 한 알림 시나리오를 구현할 수 있습니다. 예를 들어, 주식 앱은 각각 서로 다른 주식 및 주식 이름이 지정 된 각에 대 한 5 개의 알림을 보낼 수 있습니다. 이렇게 하면 큐가 동일한 주식에 대해 두 개의 알림을 포함 하는 것을 방지할 수 있습니다. 이전 버전은 최신이 아닙니다.

자세한 내용은 [알림 큐 사용](/previous-versions/windows/apps/hh781199(v=win.10))을 참조 하세요.

### <a name="enabling-the-notification-queue"></a>알림 큐 사용

알림 큐를 구현 하려면 먼저 타일에 대 한 큐를 사용 하도록 설정 합니다 ( [로컬 알림과 함께 알림 큐를 사용 하는 방법](/archive/blogs/tiles_and_toasts/quickstart-how-to-use-the-tile-notification-queue-with-local-notifications)참조). 큐를 사용 하도록 설정 하는 호출은 앱의 수명 동안 한 번만 수행 해야 하지만 앱이 시작 될 때마다 호출 하는 데에는 피해를 주지 않습니다.

### <a name="polling-for-more-than-one-notification-at-a-time"></a>한 번에 둘 이상의 알림에 대 한 폴링

타일에 대해 Windows에서 다운로드할 각 알림에 대해 고유한 URI를 제공 해야 합니다. [**StartPeriodicUpdateBatch**](/uwp/api/Windows.UI.Notifications.TileUpdater#Windows_UI_Notifications_TileUpdater_StartPeriodicUpdateBatch_Windows_Foundation_Collections_IIterable_1_Windows_UI_Notifications_PeriodicUpdateRecurrence_) 메서드를 사용 하 여 알림 큐와 함께 사용 하기 위해 한 번에 최대 5 개의 uri를 제공할 수 있습니다. 각 URI는 단일 알림 페이로드에 대해 또는 거의 동시에 폴링됩니다. 폴링 된 각 URI는 자체 만료 및 태그 값을 반환할 수 있습니다.

## <a name="related-topics"></a>관련 항목


* [정기 알림에 대 한 지침]()
* [배지에 대 한 정기 알림을 설정 하는 방법](/previous-versions/windows/apps/hh761476(v=win.10))
* [타일에 대 한 주기적인 알림을 설정 하는 방법](/previous-versions/windows/apps/hh761476(v=win.10))
 
