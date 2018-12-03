---
Description: Periodic notifications, which are also called polled notifications, update tiles and badges at a fixed interval by downloading content from a cloud service.
title: 정기 알림 개요
ms.assetid: 1EB79BF6-4B94-451F-9FAB-0A1B45B4D01C
template: detail.hbs
ms.date: 05/19/2017
ms.topic: article
keywords: Windows 10 uwp
ms.localizationpriority: medium
ms.openlocfilehash: f7a5054fde1a1a24945b193f578b8389519dc2d5
ms.sourcegitcommit: b4c502d69a13340f6e3c887aa3c26ef2aeee9cee
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 12/03/2018
ms.locfileid: "8461299"
---
# <a name="periodic-notification-overview"></a>정기 알림 개요
 


정기 알림(폴링된 알림이라고도 함)은 고정된 간격에 따라 클라우드 서비스에서 콘텐츠를 다운로드하여 타일 및 배지를 업데이트합니다. 정기 알림을 사용하려면 클라이언트 앱 코드에서 다음과 같은 두 가지 정보를 제공해야 합니다.

-   Windows에서 앱의 타일 또는 배지 업데이트를 폴링하는 웹 위치의 URI(Uniform Resource Identifier)
-   URI를 폴링할 빈도

정기 알림을 사용하면 최소한의 클라우드 서비스와 클라이언트 투자로 앱에서 라이브 타일 업데이트를 받을 수 있습니다. 정기 알림은 동일한 콘텐츠를 다양한 대상에게 배포하는 데 좋은 전달 방법입니다.

**참고**  Windows8.1에 대 한는 [푸시 및 정기 알림 샘플을](http://go.microsoft.com/fwlink/p/?linkid=231476) 다운로드 하 고 Windows10 앱에서 해당 소스 코드를 다시 사용 하 여 자세히 알아볼 수 있습니다.

 

## <a name="how-it-works"></a>작동 방식


정기 알림을 사용하기 위해서는 앱이 클라우드 서비스를 호스트해야 합니다. 이 서비스는 앱을 설치한 모든 사용자가 정기적으로 폴링합니다. 각 폴링 간격(예: 한 시간에 한 번)에 Windows는 URI에 HTTP GET 요청을 보내고, 요청에 응답하여 제공되는 요청된 타일 또는 배지 콘텐츠를 XML로 다운로드하고, 콘텐츠를 앱의 타일에 표시합니다.

알림 메시지에는 정기 업데이트를 사용할 수 없습니다. 알림은 [예약](https://msdn.microsoft.com/library/windows/apps/hh465417) 또는 [푸시](https://msdn.microsoft.com/library/windows/apps/xaml/hh868252) 알림을 통해 가장 잘 전달됩니다.

## <a name="uri-location-and-xml-content"></a>URI 위치 및 XML 콘텐츠


모든 유효한 HTTP 또는 HTTPS 웹 주소를 폴링할 URI로 사용할 수 있습니다.

클라우드 서버의 응답에는 다운로드된 콘텐츠가 포함됩니다. URI에서 반환된 콘텐츠는 [타일](adaptive-tiles-schema.md) 또는 [배지](https://msdn.microsoft.com/library/windows/apps/br212851) XML 스키마 사양에 맞아야 하며 UTF-8로 인코딩되어야 합니다. 정의된 HTTP 헤더를 사용하여 알림에 대한 [만료 시간](#expiration-of-tile-and-badge-notifications) 또는 태그를 지정할 수 있습니다.

## <a name="polling-behavior"></a>폴링 동작


다음 메서드 중 하나를 호출하여 폴링을 시작합니다.

-   [**StartPeriodicUpdate**](https://docs.microsoft.com/uwp/api/Windows.UI.Notifications.TileUpdater#Windows_UI_Notifications_TileUpdater_StartPeriodicUpdate_Windows_Foundation_Uri_Windows_Foundation_DateTime_Windows_UI_Notifications_PeriodicUpdateRecurrence_)(타일)
-   [**StartPeriodicUpdate**](https://docs.microsoft.com/uwp/api/Windows.UI.Notifications.BadgeUpdater#Windows_UI_Notifications_BadgeUpdater_StartPeriodicUpdate_Windows_Foundation_Uri_Windows_Foundation_DateTime_Windows_UI_Notifications_PeriodicUpdateRecurrence_)(배지)
-   [**StartPeriodicUpdateBatch**](https://docs.microsoft.com/uwp/api/Windows.UI.Notifications.TileUpdater#Windows_UI_Notifications_TileUpdater_StartPeriodicUpdateBatch_Windows_Foundation_Collections_IIterable_1_Windows_UI_Notifications_PeriodicUpdateRecurrence_)(타일)

이러한 메서드 중 하나를 호출하면 URI가 즉시 폴링되고 타일 또는 배지가 수신된 콘텐츠로 업데이트됩니다. 이 초기 폴링 후 Windows는 요청된 간격에 따라 업데이트를 계속 제공합니다. 폴링은 명시적으로 중지([**TileUpdater.StopPeriodicUpdate**](https://docs.microsoft.com/uwp/api/Windows.UI.Notifications.TileUpdater.StopPeriodicUpdate) 사용)하거나 앱을 제거할 때까지 계속되며 보조 타일의 경우 타일이 제거될 때가지 계속됩니다. 그렇지 않으면 앱이 다시 시작되지 않는 경우에도 Windows에서 타일 또는 배지에 대한 업데이트를 계속 폴링합니다.

### <a name="the-recurrence-interval"></a>되풀이 간격

되풀이 간격을 위에 열거한 메서드의 매개 변수로 지정합니다. Windows에서 요청에 따라 폴링하려고 최선을 다하지만 간격이 정확하지 않습니다. 요청된 폴링 간격은 Windows에 따라 최대 15분까지 지연될 수 있습니다.

### <a name="the-start-time"></a>시작 시간

필요에 따라 폴링을 시작할 특정 시간을 지정할 수 있습니다. 앱에서 타일 콘텐츠를 하루에 한 번만 변경한다고 가정합니다. 이러한 경우 클라우드 서비스를 업데이트하는 시간에 가깝게 폴링하는 것이 좋습니다. 예를 들어 매일 쇼핑 사이트에서 오전 8시에 오늘의 제품을 게시하는 경우 오전 8시 이후 바로 새 타일 콘텐츠를 폴링합니다.

시작 시간을 제공하는 경우 메서드를 처음 호출할 때 콘텐츠가 바로 폴링됩니다. 그런 다음 정기 폴링이 제공된 시작 시간의 15분 내에 시작됩니다.

### <a name="automatic-retry-behavior"></a>자동 다시 시도 동작

URI는 장치가 온라인 상태인 경우에만 폴링됩니다. 네트워크를 사용할 수 있지만 특정 이유로 URI에 연결할 수 없으면 이번 폴링 간격 반복을 건너뛰고 다음 간격에 URI가 다시 폴링됩니다. 폴링 간격에 도달했을 때 컴퓨터가 꺼짐, 절전 또는 최대 절전 상태에 있으면 URI는 장치가 꺼짐 또는 절전 상태에서 돌아올 때 폴링됩니다.

### <a name="handling-app-updates"></a>앱 업데이트 처리

폴링 URI를 변경하여 앱 업데이트를 배포하는 경우에는 새로운 URI로 StartPeriodicUpdate를 호출하는 일별 [시간 트리거 백그라운드 작업](../../../launch-resume/run-a-background-task-on-a-timer-.md)을 추가하여 타일이 새로운 URI를 사용할 수 있도록 해야 합니다. 그렇지 않고 사용자가 앱 업데이트를 받고도 앱을 시작하지 않으면 타일이 계속해서 이전 URI를 사용하여 URI가 잘못되었는지, 혹은 반환된 페이로드가 존재하지 않는 로컬 이미지를 참조하는지 표시하지 못할 수도 있습니다.

## <a name="expiration-of-tile-and-badge-notifications"></a>타일 및 배지 알림의 만료


기본적으로 정기 타일 및 배지 알림은 다운로드한 시간으로부터 3일 내에 만료됩니다. 알림이 만료되면 콘텐츠가 배지, 타일 또는 큐에서 제거되고 더 이상 사용자에게 표시되지 않습니다. 앱 또는 알림에 적합한 시간을 사용하여 모든 정기 타일 및 배지 알림에 대한 명시적 만료 시간을 설정함으로써 콘텐츠를 관련이 있을 때까지만 유지하는 것이 좋습니다. 명시적 만료 시간은 수명이 정의되어 있는 콘텐츠에 필수적입니다. 또한 클라우드 서비스에 연결할 수 없게 되는 경우나 사용자의 네트워크 연결이 장기간 끊긴 경우에 부실 콘텐츠 제거도 수행합니다.

클라우드 서비스는 응답 페이로드에 X-WNS-Expires HTTP 헤더를 포함하여 알림의 만료 날짜 및 시간을 설정합니다. X-WNS-Expires HTTP 헤더는 [HTTP 날짜 형식](http://go.microsoft.com/fwlink/p/?linkid=253706)에 맞아야 합니다. 자세한 내용은 [**StartPeriodicUpdate**](https://docs.microsoft.com/uwp/api/Windows.UI.Notifications.TileUpdater#Windows_UI_Notifications_TileUpdater_StartPeriodicUpdate_Windows_Foundation_Uri_Windows_Foundation_DateTime_Windows_UI_Notifications_PeriodicUpdateRecurrence_) 또는 [**StartPeriodicUpdateBatch**](https://docs.microsoft.com/uwp/api/Windows.UI.Notifications.TileUpdater#Windows_UI_Notifications_TileUpdater_StartPeriodicUpdateBatch_Windows_Foundation_Collections_IIterable_1_Windows_UI_Notifications_PeriodicUpdateRecurrence_)를 참조하세요.

예를 들어 주식 시장의 활황 거래일 동안 주가 업데이트에 대한 만료를 폴링 간격의 두 배(예: 30분마다 폴링할 경우 접수 1시간 후)로 설정할 수 있습니다. 다른 예로 뉴스 앱에서는 일간 뉴스 타일 업데이트에 적절한 만료 시간을 1일로 결정할 수 있습니다.

## <a name="periodic-notifications-in-the-notification-queue"></a>알림 큐의 정기 알림


[알림 순환](https://msdn.microsoft.com/library/windows/apps/hh781199)에서 정기 타일 업데이트를 사용할 수 있습니다. 기본적으로 시작 화면의 타일은 새 알림이 현재 알림을 대체할 때까지 단일 알림 콘텐츠를 표시합니다. 순환을 사용하도록 설정하면 최대 5개의 알림이 큐에서 유지되고 타일에서 알림이 순환됩니다.

큐에서 5개 알림 용량에 도달하면 큐에 새로 들어오는 다음 알림이 가장 오래된 알림을 대체합니다. 알림에 태그를 설정하여 큐의 대체 정책에 영향을 줄 수 있습니다. 태그는 응답 페이로드의 [X-WNS-Tag](https://msdn.microsoft.com/library/windows/apps/hh465435.aspx#pncodes_x_wns_tag) HTTP 헤더에 지정된 대/소문자를 구분하지 않는 최대 16개 영숫자 문자로 이루어진 문자열로, 앱과 관련이 있습니다. Windows는 들어오는 알림의 태그를 큐에 이미 있는 모든 알림의 태그와 비교합니다. 일치하는 항목이 발견되면 새 알림이 태그가 동일한 대기 중인 태그를 대체합니다. 일치하는 항목이 없으면 기본 대체 규칙이 적용되어 새 알림이 큐에서 가장 오래된 알림을 대체합니다.

알림 큐와 태그 지정을 사용하여 다양한 알림 시나리오를 구현할 수 있습니다. 예를 들어 주식 앱에서 각각 서로 다른 주식과 관련 있고 주식 이름 태그가 지정된 5개의 알림을 보낼 수 있습니다. 이렇게 하면 큐에서 오래된 알림을 제거하여 동일한 주식에 대한 알림이 두 번 포함되는 것을 방지할 수 있습니다.

자세한 내용은 [알림 큐 사용](https://msdn.microsoft.com/library/windows/apps/hh781199)을 참조하세요.

### <a name="enabling-the-notification-queue"></a>알림 큐 사용

알림 큐를 구현하려면 먼저 타일에 대한 큐를 설정합니다([로컬 알림에서 알림 큐를 사용 하는 방법](https://blogs.msdn.microsoft.com/tiles_and_toasts/2016/01/05/quickstart-how-to-use-the-tile-notification-queue-with-local-notifications/) 참조). 큐를 사용하도록 설정하는 호출은 앱의 수명 동안 한 번만 호출하면 되지만 앱을 실행할 때마다 호출해도 문제가 되지 않습니다.

### <a name="polling-for-more-than-one-notification-at-a-time"></a>한 번에 둘 이상의 알림 폴링

Windows가 타일에 대해 다운로드할 각 알림의 고유한 URI를 제공해야 합니다. [**StartPeriodicUpdateBatch**](https://docs.microsoft.com/uwp/api/Windows.UI.Notifications.TileUpdater#Windows_UI_Notifications_TileUpdater_StartPeriodicUpdateBatch_Windows_Foundation_Collections_IIterable_1_Windows_UI_Notifications_PeriodicUpdateRecurrence_) 메서드를 사용하여 알림 큐에 사용하도록 한 번에 최대 5개의 URI를 제공할 수 있습니다. 각 URI는 거의 동일한 시간에 단일 알림 페이로드에 대해 폴링됩니다. 폴링된 각 URI는 고유한 만료 및 태그 값을 반환할 수 있습니다.

## <a name="related-topics"></a>관련 항목


* [정기적 알림에 대한 지침](https://msdn.microsoft.com/library/windows/apps/hh761461)
* [배지에 대해 정기 알림을 설정하는 방법](https://msdn.microsoft.com/library/windows/apps/hh761476)
* [타일에 대해 정기 알림을 설정하는 방법](https://msdn.microsoft.com/library/windows/apps/hh761476)
 
