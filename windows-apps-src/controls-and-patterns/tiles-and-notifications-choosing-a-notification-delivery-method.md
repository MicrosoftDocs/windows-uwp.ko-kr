---
author: mijacobs
Description: "이 문서에서는 타일 및 배지 업데이트와 알림 메시지 콘텐츠를 제공하는 네 가지 알림 옵션(로컬, 예약, 정기 및 푸시)에 대해 설명합니다."
title: "알림 전달 방법 선택"
ms.assetid: FDB43EDE-C5F2-493F-952C-55401EC5172B
label: Choose a notification delivery method
template: detail.hbs
translationtype: Human Translation
ms.sourcegitcommit: d51aacb31f41cbd9c065b013ffb95b83a6edaaf4
ms.openlocfilehash: 71b1255c25adcb4a99d082ba5e83af60b316abe1

---
# <a name="choose-a-notification-delivery-method"></a>알림 전달 방법 선택

<link rel="stylesheet" href="https://az835927.vo.msecnd.net/sites/uwp/Resources/css/custom.css"> 


이 문서에서는 타일 및 배지 업데이트와 알림 메시지 콘텐츠를 제공하는 네 가지 알림 옵션(로컬, 예약, 정기 및 푸시)에 대해 설명합니다. 타일 또는 알림 메시지를 사용하면 앱에 직접 연결하지 않은 사용자에게도 정보를 제공할 수 있습니다. 전달하려는 정보와 앱의 특징 및 콘텐츠에 따라 시나리오에 가장 적합한 알림 방법을 결정할 수 있습니다.

## <a name="notification-delivery-methods-overview"></a>알림 전달 방법 개요


앱에서 알림을 전달하는 데 사용할 수 있는 네 가지 메커니즘이 있습니다.

-   **로컬**
-   **예약**
-   **주기**
-   **푸시**

다음 표에는 알림 전달 유형이 요약되어 있습니다.

<table>
<colgroup>
<col width="25%" />
<col width="25%" />
<col width="25%" />
<col width="25%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">전달 방법</th>
<th align="left">사용 대상</th>
<th align="left">설명</th>
<th align="left">예제</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left">로컬</td>
<td align="left">타일, 배지, 알림</td>
<td align="left">앱이 실행 중이거나, 타일 또는 배지를 업데이트하거나, 알림 메시지를 보내는 동안에 알림을 보내는 API 호출 집합입니다.</td>
<td align="left"><ul>
<li>음악 앱에서는 타일을 업데이트하여 &quot;지금 재생&quot;되는 항목을 표시합니다.</li>
<li>게임 앱에서는 사용자가 게임을 종료할 때 사용자의 최고 점수로 타일을 업데이트합니다.</li>
<li>앱이 활성화되면 해당 문자 모양이 앱에 새로운 정보가 있음을 나타내는 배지가 지워집니다.</li>
</ul></td>
</tr>
<tr class="even">
<td align="left">예약</td>
<td align="left">타일, 알림</td>
<td align="left">지정한 시간에 업데이트하도록 알림을 미리 예약하는 API 호출 집합입니다.</td>
<td align="left"><ul>
<li>일정 앱은 예정된 모임에 대한 알림 메시지 미리 알림을 설정합니다.</li>
</ul></td>
</tr>
<tr class="odd">
<td align="left">주기</td>
<td align="left">타일, 배지</td>
<td align="left">새 콘텐츠에 대한 클라우드 서비스를 폴링하여 타일과 배지를 고정된 간격에 따라 주기적으로 업데이트하는 알림입니다.</td>
<td align="left"><ul>
<li>날씨 앱에서는 예보를 보여 주는 타일을 30분 간격으로 업데이트합니다.</li>
<li>&quot;일일 거래&quot; 사이트에서는 매일 아침 오늘의 행사 상품을 업데이트합니다.</li>
<li>이벤트가 매일 자정에 표시된 카운트 다운을 업데이트할 때까지 기간(일)을 표시하는 타일입니다.</li>
</ul></td>
</tr>
<tr class="even">
<td align="left">푸시</td>
<td align="left">타일, 배지, 알림, 푸시</td>
<td align="left">클라우드 서버에서 보내는 알림이며 앱을 실행하고 있지 않은 동안에도 전송됩니다.</td>
<td align="left"><ul>
<li>쇼핑 앱에서는 표시된 품목의 판매와 관련한 알림 메시지를 보냅니다.</li>
<li>뉴스 앱에서는 실시간 뉴스 속보로 타일을 업데이트합니다.</li>
<li>스포츠 앱에서는 타일에 최신 게임 진행 정보를 표시합니다.</li>
<li>커뮤니케이션 앱에서는 들어오는 메시지나 걸려오는 전화를 알려줍니다.</li>
</ul></td>
</tr>
</tbody>
</table>

 

## <a name="local-notifications"></a>로컬 알림


앱 실행 중에 앱 타일 또는 배지를 업데이트하거나 알림 메시지를 발생시키는 방법으로 가장 쉽게 알림을 전달할 수 있습니다. 로컬 API만 호출하면 됩니다. 모든 앱에는 사용자가 앱을 실행하고 상호 작용한 후에만 해당 콘텐츠가 변경되는 경우에도 타일에 표시할 유용하거나 흥미로운 정보가 있을 수 있습니다. 로컬 알림은 다른 알림 메커니즘 중 하나를 사용하는 경우에도 앱 타일을 최신의 상태로 유지하는 적합한 방법입니다. 예를 들어 사진 앱 타일에는 최근에 추가된 앨범에 있는 사진을 표시할 수 있습니다.

앱을 처음 실행할 때 로컬로 타일을 업데이트하거나, 적어도 사용자가 변경을 수행한 후 바로 업데이트하여 변경 사항이 타일에 적용되도록 하는 것이 좋습니다. 해당 업데이트는 사용자가 앱을 종료한 다음에야 표시되지만 앱이 사용되는 동안 해당 변경을 수행하여 사용자가 종료할 때 타일이 이미 최신 상태가 되도록 사용할 수 있습니다.

API 호출이 로컬이더라도 알림에서는 웹 이미지를 참조할 수 있습니다. 웹 이미지가 다운로드할 수 없거나, 손상되거나, 이미지 사양을 충족하지 않는 경우 타일 및 알림이 다르게 응답합니다.

-   타일: 업데이트가 표시되지 않음
-   알림: 알림이 표시되지만 이미지는 삭제됨

기본적으로 로컬 알림 메시지는 3일 내에 만료되고 로컬 타일 알림은 만료되지 않습니다. 이 기본값을 알림에 적합한 명시적 만료 시간으로 재정의하는 것이 좋습니다(알림의 최대값은 3일). 

자세한 내용은 아래 항목을 참조하세요.

-   [로컬 타일 알림 보내기](tiles-and-notifications-sending-a-local-tile-notification.md)
-   [로컬 알림 메시지 보내기](https://blogs.msdn.microsoft.com/tiles_and_toasts/2015/07/08/quickstart-sending-a-local-toast-notification-and-handling-activations-from-it-windows-10/)
-   [UWP(유니버설 Windows 플랫폼) 알림 코드 샘플](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/Notifications)

## <a name="scheduled-notifications"></a>예약된 알림


예약된 알림은 타일을 업데이트하거나 알림 메시지를 표시할 시간을 정확하게 지정할 수 있는 로컬 알림의 하위 집합입니다. 예약된 알림은 업데이트할 콘텐츠를 미리 알고 있는 경우(예: 모임 초대)에 적합합니다. 알림 콘텐츠를 미리 알 수 없는 경우 푸시 또는 정기 알림을 사용해야 합니다.

배지 알림에는 예약된 알림을 사용할 수 없습니다. 배지 알림은 로컬 알림, 정기 알림 또는 푸시 알림에서 가장 잘 처리됩니다.

기본적으로 예약 알림은 전달된 시간부터 3일 내에 만료됩니다. 예약된 타일 알림에서는 이 기본 만료 시간을 재정의할 수 있지만 예약된 알림의 만료 시간은 재정의할 수 없습니다.

자세한 내용은 아래 항목을 참조하세요.

-   [UWP(유니버설 Windows 플랫폼) 알림 코드 샘플](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/Notifications)

## <a name="periodic-notifications"></a>정기 알림


정기 알림을 사용하면 최소한의 클라우드 서비스와 클라이언트 투자로 라이브 타일 업데이트를 제공받을 수 있습니다. 또한 동일한 콘텐츠를 다양한 대상에게 배포하는 데 좋은 방법입니다. 클라이언트 코드를 통해 Windows에서 타일 또는 배지 업데이트에 대해 폴링할 클라우드 위치의 URL과 이 위치를 폴링하는 빈도를 지정합니다. 각 폴링 간격에 Windows에서는 URL에 연결하여 지정된 XML 콘텐츠를 다운로드한 후 타일에 표시합니다.

정기 알림을 사용하려면 앱에서 클라우드 서비스를 호스트해야 합니다. 그러면 앱을 설치한 모든 사용자에 의해 이 서비스가 지정된 간격으로 폴링됩니다. 알림 메시지에는 정기 업데이트를 사용할 수 없습니다. 알림 메시지는 예약된 알림 또는 푸시 알림에서 가장 잘 처리됩니다.

기본적으로 정기 알림은 폴링이 발생하는 시간부터 3일 내에 만료됩니다. 필요한 경우 명시적인 만료 시간으로 이 기본값을 재정의할 수 있습니다.

자세한 내용은 아래 항목을 참조하세요.

-   [정기 알림 개요](tiles-and-notifications-periodic-notification-overview.md)
-   [UWP(유니버설 Windows 플랫폼) 알림 코드 샘플](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/Notifications)

## <a name="push-notifications"></a>푸시 알림


푸시 알림은 실시간 데이터나 사용자에 맞게 개인 설정된 데이터를 전달하는 데 적합합니다. 푸시 알림은 콘텐츠가 생성되는 시간을 예측할 수 없는 경우(예: 뉴스 속보, 소셜 네트워크 업데이트 또는 인스턴트 메시지)에 사용됩니다. 푸시 알림은 데이터의 시간이 중요하여 정기 알림에는 적합하지 않은 경우(예: 게임 중 스포츠 점수)에도 유용합니다.

푸시 알림에는 푸시 알림 채널을 관리하고 알림을 보낼 시간과 대상을 선택하는 클라우드 서비스가 필요합니다.

기본적으로 푸시 알림은 디바이스에 수신된 시간부터 3일 내에 만료됩니다. 필요한 경우 이 기본값을 명시적 만료 시간으로 재정의할 수 있습니다(알림의 최대값은 3일).

자세한 내용은 다음을 참조하세요.

-   [WNS(Windows 푸시 알림 서비스) 개요](tiles-and-notifications-windows-push-notification-services--wns--overview.md)
-   [푸시 알림에 대한 지침](https://msdn.microsoft.com/library/windows/apps/hh761462)
-   [UWP(유니버설 Windows 플랫폼) 알림 코드 샘플](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/Notifications)


## <a name="related-topics"></a>관련 항목


* [로컬 타일 알림 보내기](tiles-and-notifications-sending-a-local-tile-notification.md)
* [로컬 알림 메시지 보내기](https://blogs.msdn.microsoft.com/tiles_and_toasts/2015/07/08/quickstart-sending-a-local-toast-notification-and-handling-activations-from-it-windows-10/)
* [푸시 알림에 대한 지침](https://msdn.microsoft.com/library/windows/apps/hh761462)
* [알림 메시지에 대한 지침](https://msdn.microsoft.com/library/windows/apps/hh465391)
* [정기 알림 개요](tiles-and-notifications-periodic-notification-overview.md)
* [WNS(Windows 푸시 알림 서비스) 개요](tiles-and-notifications-windows-push-notification-services--wns--overview.md)
* [GitHub의 UWP(유니버설 Windows 플랫폼) 알림 코드 샘플](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/Notifications)
 

 







<!--HONumber=Dec16_HO1-->


