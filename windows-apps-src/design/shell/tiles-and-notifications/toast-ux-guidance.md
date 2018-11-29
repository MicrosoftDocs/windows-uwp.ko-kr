---
Description: Learn how to create effective and user-focused notifications that make your users productive and happy.
title: 알림 UX 지침
label: Toast UX Guidance
template: detail.hbs
ms.date: 05/18/2018
ms.topic: article
keywords: windows 10, uwp, 알림, 컬렉션, 그룹, ux, ux 지침, 지침, 동작, 알림, 알림 센터, noninterruptive, 유효 알림, nonintrusive 알림, 실행 가능한, 관리, 구성
ms.localizationpriority: medium
ms.openlocfilehash: 17861760ea5640eca01d3e69c1ed36a023c8f9d3
ms.sourcegitcommit: b5c9c18e70625ab770946b8243f3465ee1013184
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/29/2018
ms.locfileid: "7989927"
---
# <a name="toast-notification-ux-guidance"></a>알림 알림 UX 지침
알림은 부분이 최신 수명; 사용자의 생산성을 하 고 모든 업데이트를 사용 하 여 계속 현재 뿐만 아니라 앱 및 웹 사이트, 약속 된 데 도움이 됩니다. 그러나 알림 overbearing에 유용 하 고 사용자 중심 방식으로 설계 되지 않은 경우 주입 식에서 신속 하 게 끌 수 있습니다. 알림은에서 해제 되 고 하나의 마우스 오른쪽 단추로 클릭 하 고 것 같지 않은 해제 되 면, 것은 다시 설정 합니다.  따라서 인지 확인 알림을 사용자의 화면 공간 및 시간의 정중 하므로이 참여 채널을 열어 둘 수 있습니다.

> **중요 Api**: [Windows 커뮤니티 도구 키트 알림 nuget 패키지](https://www.nuget.org/packages/Microsoft.Toolkit.Uwp.Notifications/)

우리가 우리의 Windows 원격 분석 뿐만 아니라 다른 첫 번째 및 제 3 자 사례 연구 멋진 알림 스토리를 개성을 주위 4 개의 규칙을 분석 했습니다.  우리는 이러한 규칙 플랫폼에 관계 없이 전체적으로 적용 되 고 사용자에 게 긍정적인 영향을 미칠 알림을 데 도움이 됩니다.

## <a name="1-actionable-notifications"></a>1. 실행 가능한 알림
실행 가능한 알림 사용자가 앱을 열지 않고도 생산성을 허용 합니다.  멋진 앱을 시작, 성공의 유일한 측정 아닙니다 및 작은 작업을 수행할 수 있도록 사용자를 안내 상태일 때 작업 앱을 벗어나지 않고 사용자에 게 delighting에 매우 강력한 도구 될 수 있습니다.

![텍스트 입력된 상자 및 미리 알림을 설정 하 여 알림에 응답 하는 단추를 사용 하 여 실행 가능한 알림](images/actionable-notification-example01.png)

위의 작업을 활용 하는 알림의 예로 나와 있습니다. 마무리 작업의 보편적 양수 느낌 이며의 실행 가능한 콘텐츠에 포함 된 알림을 전송 하 여 앱 또는 웹 사이트에 해당 느낌을 가져올 수 있습니다. 실행 가능한 알림 생산성을 높일 수 있습니다, 그리고 작업 사용자에 게 시간을 단축 하 여 엔터프라이즈 및 소비자 시나리오에서 모두 아닌지 작은 이러한 작업을 수행 합니다. 사용자가 정기적으로 수행할 작업 또는 할 사용자를 교육 하는 것을 포함 하는 것이 좋습니다.  예를 들면 다음과 같습니다.
* 원하는 대로 favoriting, 플래그, 또는 콘텐츠 주연
* 승인 또는 거부 경비 보고서, 권한 등 해제 시간입니다.
* 인라인 메시지에 회신, 메일, 그룹 채팅, 설명, 등.
* [보류 중인 업데이트](toast-pending-update.md) 를 사용 하 여 순서를 완료 합니다.
* 일정에서 시간을 예약 하는 잠재적으로 뿐만 아니라 다른 시간에 대 한 경고 또는 미리 알림 설정

ctionable 알림은 사용자가 앱 또는 웹 사이트를 사용 하 여 멋진 고 효율적인 환경을 있고. 그래야 듣는 사람도 생산성, 작업을 수행 하는 데 매우 강력한 도구입니다.  여기에 기회 많습니다. 아이디어 브레인스토밍 도움말을 원한다 면 자유롭게 windows 알림 팀에 문의 하십시오.  본인 

## <a name="2-timing-and-urgency"></a>2. 타이밍 및 긴급
종종 생각 알림에 대 한, 반대로 실시간으로 최상의 되지는 않습니다. 것이 좋습니다 개발자를가 사용자에 대 한 대기 하 고 알림을 보내고 있기 긴급 정보 인지 여부. 사용자가 쉽게 너무 많은 정보를 사용 하 여 오버 로드 될 수 하 고 포커스를 동안 중단 되는 경우 당황 받으세요. Windows는 알림 보내는의 실행 시기를 고려 하는 방법에 대 한 몇 가지 옵션이 있습니다.

**원시 알림:** [원시 알림을](raw-notification-overview.md) 사용 하 여 유용할 수 있습니다 여러 가지 이유로 사용자에 게 중단을 최소화 하는 동안 기다립니다 살펴봤듯이 특히.  원시 알림 보내기를 절전 앱은 백그라운드에서 알림을 타당 앱의 컨텍스트에서 즉시 제공할 수 있는지 여부를 평가할 수 있도록 합니다. 느껴질 것을 표시할 사용자에 게 즉시 여기에서 [로컬 알림 메시지](send-local-toast.md) 를 표시할 수 있는 경우.  이라면 사용자가 볼 필요가 지금은, 나중에 발생 하는 [예약 된 알림 메시지](https://blogs.msdn.microsoft.com/tiles_and_toasts/2016/09/30/quickstart-sending-an-alarm-in-windows-10/) 를 만들 수 있습니다.

**알림 고스트:** 되 고 오른쪽 아래 모서리 화면에서 팝업을 건너뛰고 대신 알림을 알림 센터에 직접 보낼 알림도 발생할 수 있습니다. [SuppressPopup 속성](https://docs.microsoft.com/en-us/uwp/api/windows.ui.notifications.toastnotification.suppresspopup) 을 true로 설정 하 여 수행 됩니다. 2 확인 하지 알림을 알림 센터 외부 팝 주변 일부 대처 있을 수 있지만-3 회 높은 참여를 통해 관리 센터에서 라이브 알림의 알림 팝 합니다.  사용자는 응답성 알림을 받을 준비가 되 고 때 이러한 중단 되 면 때문에 사용자에 게 알리는 간접적에 대 한 훨씬 더 효과적일 수 있는 알림 센터에서 콘텐츠를 제어할 수 있습니다.

## <a name="3-clear-out-the-clutter"></a>3. 복잡 한 지우기
알림은 시간이 오래 (기본값 3 일)에 대 한 알림 센터에서 유지할 수 있습니다.  여기 있는 콘텐츠는 최신 상태로 유지 하 고 관련 된 사용자가 알림 센터를 열 때마다 되는지 확인 해야 합니다. 사용자의 화면 공간을 낭비 되며 더 최신 용도로 사용 될 수 있는 슬롯을 차지 합니다.  사용자가 메일 관리 앱을 설치 하 고 10 메일과 함께 이러한 메일 10 알림을 받는 가정해 보겠습니다.  원하는 환경에 따라 사용자가 해당 메일 읽기 나 알림 센터에서 이전 혼란을 제거 하는 방법으로 앱을 연 경우 이러한 알림을 취소를 고려할 수 있습니다.

우리 [ToastNotificationHistory](https://docs.microsoft.com/en-us/uwp/api/windows.ui.notifications.toastnotificationhistory) 관리 센터에는 콘텐츠가 볼 수 있게 해 주는 Api는 뿐만 아니라 이러한 알림을 관리 합니다. 사용자의 화면 공간을 정중 되며 사용자에 게 현재 및 관련 콘텐츠를 표시만 하는 주의 합니다.

## <a name="4-keeping-organized"></a>4. 유지 구성
앞서 언급 했 듯이, 3 일에 대 한 알림 센터에서 콘텐츠가 유지 됩니다.  사용자가 찾으려는 신속 하 게 정보를 선택 하려면 [머리글이](https://docs.microsoft.com/en-us/windows/uwp/design/shell/tiles-and-notifications/toast-headers) 나 [컬렉션](https://docs.microsoft.com/en-us/uwp/api/windows.ui.notifications.toastcollection)을 사용 하 여 알림 센터에서 알림을 구성 합니다. 아래 헤더의 예를 볼 수 있습니다.

![헤더를 사용 하 여 알림 예제에는 'Camping!!' 레이블이 지정](images/toast-headers-action-center.png)

두 가지 방식으로 관련 콘텐츠를 함께 유지 되므로 gorup 알림은 (즉, 다른 스포츠 리그 스포츠 앱에서는 좌우 또는 그룹 채팅 메시지를 정렬할 생각). 헤더는 보다 섬세 하지만 심사 하 고 알림을 더 빠르게 골라낼 수 있도록 모두 컬렉션은 알림 그룹화 하는 보다 명확 하 게 합니다. 

## <a name="other-resources"></a>다른 리소스
이러한 4 개의 점을 위의 지침 및 첫 번째 및 제 3 자 실험 했으므로 분석의 원격 분석을 통해 유효 발견 했습니다. 하지만 유의는 이러한 지침은: 지침.  우리는 확신이 사용자 중심을 통한 교육 및 학습 고유한 데이터를 대체할 수 아무것도 하지만이 규칙 이러한 참여 및 알림, 생산성을 향상 하는 데 도움이 됩니다.  

UWP 앱에 현재 알림을 보내는 경우 [파트너 센터](https://partner.microsoft.com/dashboard)에서 알림을 변경 사항에 분석을 볼 수 있습니다! 이 데이터는 [Store Services SDK](https://marketplace.visualstudio.com/items?itemName=AdMediator.MicrosoftStoreServicesSDK) 또는 [WNS Api](https://docs.microsoft.com/en-us/windows/uwp/design/shell/tiles-and-notifications/windows-push-notification-services--wns--overview)를 사용 하는 경우 무료 제공 됩니다. 이러한 메트릭은 보다 잘 이해할 windows 플랫폼에서 알림을 처리에 사용자가 알림을와 상호 작용 하는 방법. 왼쪽 참여에 메뉴로 이동 하 여이 데이터에 액세스 > 알림, 알림 페이지 내에서 "분석" 탭을 클릭 합니다.  이 파트너 센터에서 알림을 전송으로 동일한 위치에 있습니다.

## <a name="related-topics"></a>관련 항목

* [알림 콘텐츠](adaptive-interactive-toasts.md)
* [원시 알림](raw-notification-overview.md)
* [보류 중인 업데이트](toast-pending-update.md)
* [GitHub의 알림 라이브러리(Windows 커뮤니티 도구 키트의 일부)](https://github.com/Microsoft/UWPCommunityToolkit/tree/master/Microsoft.Toolkit.Uwp.Notifications)
