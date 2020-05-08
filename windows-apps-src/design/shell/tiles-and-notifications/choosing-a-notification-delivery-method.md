---
Description: 이 문서에서는 타일 및 배지 업데이트 및 \#toast 알림 콘텐츠를 제공 하는 8212; 로컬, \#예약, 정기 및 푸시&8212의 네 가지 알림&옵션을 설명 합니다.
title: 알림 전달 방법 선택
ms.assetid: FDB43EDE-C5F2-493F-952C-55401EC5172B
label: Choose a notification delivery method
template: detail.hbs
ms.date: 05/19/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: f1df2048ea54b3ffc7c62270841b2be650bb90ea
ms.sourcegitcommit: 0dee502484df798a0595ac1fe7fb7d0f5a982821
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 05/08/2020
ms.locfileid: "82970828"
---
# <a name="choose-a-notification-delivery-method"></a>알림 전달 방법 선택

 


이 문서에서는 타일 및 배지 업데이트와 알림 메시지 콘텐츠를 제공하는 네 가지 알림 옵션(로컬, 예약, 정기 및 푸시)에 대해 설명합니다. 타일 또는 알림 메시지는 사용자가 앱에 직접 관여 하지 않는 경우에도 사용자에 게 정보를 가져올 수 있습니다. 앱의 특성과 콘텐츠와 제공 하려는 정보는 사용자의 시나리오에 가장 적합 한 알림 방법 또는 방법을 결정 하는 데 도움이 될 수 있습니다.

## <a name="notification-delivery-methods-overview"></a>알림 배달 방법 개요


앱에서 알림을 배달 하는 데 사용할 수 있는 메커니즘에는 다음 네 가지가 있습니다.

-   **로컬**
-   **예약**
-   **정기적인**
-   **푸시**

다음 표에서는 알림 배달 유형을 요약 합니다.

<table>
<colgroup>
<col width="25%" />
<col width="25%" />
<col width="25%" />
<col width="25%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">배달 방법</th>
<th align="left">사용 방법</th>
<th align="left">설명</th>
<th align="left">예제</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left">로컬</td>
<td align="left">타일, 배지, 알림</td>
<td align="left">앱이 실행 되는 동안 알림을 보내거나, 타일 또는 배지를 직접 업데이트 하거나, 알림 메시지를 보내는 API 호출 집합입니다.</td>
<td align="left"><ul>
<li>음악 앱은 타일을 업데이트 하 여 &quot;현재 재생&quot;중인 항목을 표시 합니다.</li>
<li>게임 앱은 사용자가 게임을 떠날 때 사용자의 높은 점수를 사용 하 여 타일을 업데이트 합니다.</li>
<li>응용 프로그램이 활성화 될 때 앱이 제거 되는 새 정보가 있음을 나타내는 문자 모양의 배지입니다.</li>
</ul></td>
</tr>
<tr class="even">
<td align="left">예약</td>
<td align="left">타일, 알림</td>
<td align="left">지정 된 시간에 업데이트 하기 위해 미리 알림을 예약 하는 API 호출 집합입니다.</td>
<td align="left"><ul>
<li>일정 앱은 예정 된 모임에 대 한 알림 메시지 알림을 설정 합니다.</li>
</ul></td>
</tr>
<tr class="odd">
<td align="left">정기적인</td>
<td align="left">타일, 배지</td>
<td align="left">새 콘텐츠에 대 한 클라우드 서비스를 폴링하여 고정 된 시간 간격으로 타일 및 배지를 정기적으로 업데이트 하는 알림입니다.</td>
<td align="left"><ul>
<li>날씨 앱은 예측을 30 분 간격으로 보여 주는 타일을 업데이트 합니다.</li>
<li>일일 &quot;거래&quot; 사이트는 매일 아침에 업데이트를 업데이트 합니다.</li>
<li>이벤트가 매일 자정에 표시 된 카운트다운을 업데이트할 때 까지의 기간 (일)을 표시 하는 타일입니다.</li>
</ul></td>
</tr>
<tr class="even">
<td align="left">밀어넣기</td>
<td align="left">타일, 배지, 알림, Raw</td>
<td align="left">앱이 실행 되 고 있지 않은 경우에도 클라우드 서버에서 전송 되는 알림</td>
<td align="left"><ul>
<li>쇼핑 앱은 사용자가 시청 중인 항목에 대 한 판매를 알려 주는 알림 메시지를 보냅니다.</li>
<li>뉴스 앱은 문제가 발생 하는 경우에는 뉴스를 분리 하 여 타일을 업데이트 합니다.</li>
<li>스포츠 앱은 진행 중인 게임 동안 타일을 최신 상태로 유지 합니다.</li>
<li>통신 앱은 들어오는 메시지 또는 전화 통화에 대 한 경고를 제공 합니다.</li>
</ul></td>
</tr>
</tbody>
</table>

 

## <a name="local-notifications"></a>로컬 알림


앱이 실행 되는 동안 앱 타일 또는 배지를 업데이트 하거나 알림 메시지를 발생 시키는 것은 알림 배달 메커니즘의 가장 간단한 방법입니다. 로컬 API 호출만 필요 합니다. 사용자가 앱을 시작 하 고 앱과 상호 작용 하는 경우에만 해당 콘텐츠가 변경 되는 경우에도 모든 앱은 타일에 표시 되는 유용한 정보나 흥미로운 정보를 포함할 수 있습니다. 또한 로컬 알림은 다른 알림 메커니즘 중 하나를 사용 하는 경우에도 앱 타일을 최신 상태로 유지 하는 좋은 방법입니다. 예를 들어 사진 앱 타일은 최근에 추가 된 앨범에서 사진을 표시할 수 있습니다.

앱이 처음 시작할 때 타일을 로컬로 업데이트 하거나 사용자가 즉시 타일에 반영 되는 앱의 변경을 수행 하는 즉시 앱을 업데이트 하는 것이 좋습니다. 사용자가 앱을 떠날 때까지 해당 업데이트가 표시 되지 않지만, 앱을 사용 하는 동안 변경 하면 사용자가 분리 때 타일이 이미 최신 상태를 유지 하 게 됩니다.

API 호출이 로컬 인 동안 알림은 웹 이미지를 참조할 수 있습니다. 웹 이미지를 다운로드할 수 없거나, 손상 되었거나, 이미지 사양을 충족 하지 않는 경우 타일과 알림이 다르게 응답 합니다.

-   타일: 업데이트가 표시 되지 않습니다.
-   알림: 알림이 표시 되지만 이미지는 삭제 됩니다.

기본적으로 로컬 알림 메시지는 3 일 후에 만료 되 고 로컬 타일 알림은 만료 되지 않습니다. 이러한 기본값을 알림에 적합 한 명시적인 만료 시간으로 재정의 하는 것이 좋습니다 (알림을는 최대 3 일). 

자세한 내용은 다음 항목을 참조하세요.

-   [로컬 타일 알림 보내기](sending-a-local-tile-notification.md)
-   [로컬 알림 메시지 보내기](send-local-toast.md)
-   [Windows 앱 알림 코드 샘플](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/Notifications)

## <a name="scheduled-notifications"></a>예약 된 알림


예약 된 알림은 타일을 업데이트 하거나 알림 메시지를 표시 해야 하는 정확한 시간을 지정할 수 있는 로컬 알림의 하위 집합입니다. 예약 된 알림은 모임 초대와 같이 업데이트할 콘텐츠를 미리 알 수 있는 경우에 적합 합니다. 알림 콘텐츠에 대 한 고급 지식이 없는 경우 푸시 또는 정기 알림을 사용 해야 합니다.

예약 된 알림은 배지 알림에 사용할 수 없습니다. 배지 알림은 로컬, 정기 또는 푸시 알림에 의해 가장 효과적으로 처리 됩니다.

기본적으로 예약 된 알림은 배달 된 시간부터 3 일 후에 만료 됩니다. 예약 된 타일 알림에서이 기본 만료 시간을 재정의할 수 있지만 예약 된 알림을 만료 시간을 재정의할 수는 없습니다.

자세한 내용은 다음 항목을 참조하세요.

-   [알림 메시지 예약](scheduled-toast.md)
-   [Windows 앱 알림 코드 샘플](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/Notifications)

## <a name="periodic-notifications"></a>정기 알림


정기 알림은 최소 클라우드 서비스 및 클라이언트 투자를 통해 라이브 타일 업데이트를 제공 합니다. 또한 동일한 콘텐츠를 광범위 한 사용자에 게 배포 하는 훌륭한 방법입니다. 클라이언트 코드는 Windows에서 타일 또는 배지 업데이트를 폴링하는 클라우드 위치의 URL과 위치를 폴링하는 빈도를 지정 합니다. 각 폴링 간격 마다 Windows는 URL에 연결 하 여 지정 된 XML 콘텐츠를 다운로드 하 고 타일에 표시 합니다.

정기적 알림은 앱이 클라우드 서비스를 호스트 하는 데 필요 하며이 서비스는 앱이 설치 된 모든 사용자가 지정 된 간격으로 폴링됩니다. 정기 업데이트는 알림 메시지에 사용할 수 없습니다. 알림 메시지는 예약 된 알림이나 푸시 알림에 의해 가장 효과적으로 처리 됩니다.

기본적으로 정기 알림은 폴링을 수행 하는 시간부터 3 일 후에 만료 됩니다. 필요한 경우 명시적 만료 시간을 사용 하 여이 기본값을 재정의할 수 있습니다.

자세한 내용은 다음 항목을 참조하세요.

-   [정기 알림 개요](periodic-notification-overview.md)
-   [Windows 앱 알림 코드 샘플](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/Notifications)

## <a name="push-notifications"></a>푸시 알림


푸시 알림은 사용자에 맞게 개인 설정 된 데이터 또는 실시간 데이터를 전달 하는 데 적합 합니다. 푸시 알림은 주요 뉴스, 소셜 네트워크 업데이트 또는 인스턴트 메시지와 같이 예측할 수 없는 시간에 생성 되는 콘텐츠에 사용 됩니다. 푸시 알림은 게임 중 스포츠 점수와 같은 주기적인 알림에 맞지 않는 방식으로 데이터가 시간을 구분 하는 경우에도 유용 합니다.

푸시 알림에는 푸시 알림 채널을 관리 하 고 알림을 보낼 시기 및 시기를 선택 하는 클라우드 서비스가 필요 합니다.

기본적으로 푸시 알림은 장치에서 받은 시간부터 3 일 후에 만료 됩니다. 필요한 경우 명시적 만료 시간으로이 기본값을 재정의할 수 있습니다 (알림을는 최대 3 일).

자세한 내용은 다음을 참조하세요.

-   [WNS(Windows 푸시 알림 서비스) 개요](windows-push-notification-services--wns--overview.md)
-   [푸시 알림에 대 한 지침](https://docs.microsoft.com/windows/uwp/controls-and-patterns/tiles-and-notifications-windows-push-notification-services--wns--overview)
-   [Windows 앱 알림 코드 샘플](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/Notifications)


## <a name="related-topics"></a>관련 항목


* [로컬 타일 알림 보내기](sending-a-local-tile-notification.md)
* [로컬 알림 메시지 보내기](send-local-toast.md)
* [푸시 알림에 대 한 지침](https://docs.microsoft.com/windows/uwp/controls-and-patterns/tiles-and-notifications-windows-push-notification-services--wns--overview)
* [알림 메시지에 대한 지침](https://docs.microsoft.com/windows/uwp/controls-and-patterns/tiles-badges-notifications)
* [정기 알림 개요](periodic-notification-overview.md)
* [WNS(Windows 푸시 알림 서비스) 개요](windows-push-notification-services--wns--overview.md)
* [GitHub의 Windows 앱 알림 코드 샘플](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/Notifications)
 

 




