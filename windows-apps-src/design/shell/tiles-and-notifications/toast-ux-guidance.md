---
Description: 생산성을 사용자에 게는 효과적이 고 사용자에 초점을 맞춘 알림을 만드는 방법에 알아봅니다.
title: 알림 UX 지침
label: Toast UX Guidance
template: detail.hbs
ms.date: 05/18/2018
ms.topic: article
keywords: windows 10, uwp, 알림, 컬렉션, 그룹, ux, ux 지침 지침, 작업, 알림, 관리 센터, noninterruptive, 효과적인 알림, 비침입적 알림, 실행 가능한, 관리, 구성
ms.localizationpriority: medium
ms.openlocfilehash: 878df85db9ab0e33db06a86ddb726f07dc28f013
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57615768"
---
# <a name="toast-notification-ux-guidance"></a>토스트 알림 UX 지침
알림은 최신 수명;의 필수 구성 사용자 생산성과 모든 업데이트를 사용 하 여 최신 뿐만 아니라 앱 및 웹 사이트를 사용 하 여 참여 데 도움이 됩니다. 그러나 알림 overbearing를 유용 하 고 사용자 중심 방식으로 설계 되지 않은 경우 주입에서 신속 하 게 설정할 수 있습니다. 알림은 해제 중에서 하나의 마우스 오른쪽 단추로 클릭 하 고 그럴 가능성은 해제 되 면, 이러한 켜 집니다 다시 합니다.  따라서 사용자 알림은 사용자의 화면 공간 시간과 정직 수 있도록이 engagement 채널 열기 해야 합니다.

> **중요 한 Api**: [Windows 커뮤니티 도구 키트 알림 nuget 패키지](https://www.nuget.org/packages/Microsoft.Toolkit.Uwp.Notifications/)

Windows 원격 분석을 뿐만 아니라 다른 첫 번째 및 제 3 자 사례 연구, 유용한 알림 스토리 점에서 주위 4 개의 규칙을 사용 하 여를 분석 한 것입니다.  이러한 규칙은 플랫폼에 관계 없이 보편적으로 적용할 수 있습니다 및 알림을 사용자에 게 긍정적인 영향을 미칠 수 있도록 도와줍니다 확신 합니다.

## <a name="1-actionable-notifications"></a>1. 실행 가능한 알림
실행 가능한 알림을 통해 사용자가 앱을 열지 않고 생산성을 높일 수 있습니다.  훌 륭 앱을 시작 하 고 이것이 유일한 성공 측정 작은 수행 되려면 사용 하도록 설정 하면 사용자가 앱으로 이동 하지 않고도 작업 에너지가 사용자의 매우 강력한 도구를 수 있습니다.

![입력된 텍스트 상자 및 미리 알림을 설정 및 알림에 응답 하는 단추를 사용 하 여 실행 가능한 알림](images/actionable-notification-example01.png)

위에서 작업을 활용 하는 알림의 예시입니다. 완료 작업의 느낌을 보편적으로 양의 느낌 이며 못한 앱 또는 웹 사이트에 실행 가능한 콘텐츠를 포함 하는 알림을 전송 하 여 가져올 수 있습니다. 실행 가능한 알림 생산성을 높이 데도 도움이 수, 더 작은 작업을 수행 하려면 통과 하는 작업 사용자에 게는 시간이 줄어들어 기업 및 소비자 시나리오에서 둘 다. 사용자가 정기적으로 수행, 하는 작업 또는 작업을 수행 하 여 사용자를 교육 하려고 하는 것을 포함 하는 것이 좋습니다.  예를 들면 다음과 같습니다.
* 원하는 대로, 플래그, 또는 콘텐츠 주연 즐겨찾기에 추가
* 경비 보고서를 승인 또는 거부, 휴가 권한, 등입니다.
* 인라인 메시지에 회신, 메일 그룹 채팅, 의견 등입니다.
* 사용 하 여 주문을 완료 [업데이트 보류 중](toast-pending-update.md)
* 일정의 시간을 예약 하는 잠재적으로 뿐만 아니라 다른 시간에 대 한 경고 또는 알림 설정

실행 가능한 알림은 사용자가 생산성 것으로 생각 될 작업을 수행 하 고 앱 이나 웹 사이트는 훌륭한 고 효율적인 환경을 제공 하는 데 강력한 도구입니다.  많이 여기 기회! 아이디어 브레인스토밍 도움말을 하려는 경우 자유롭게 windows 알림 팀에 문의 하십시오.

## <a name="2-timing-and-urgency"></a>2. 타이밍 및 긴급도
에서는 종종에 대해 어떻게 생각해 알림, 달리 실시간 반드시 최상의 아닙니다. 여부 사용자 고려해 야 하는 경우 알림을 전송 하는 긴급 한 정보를 개발자가 좋습니다. 사용자가 너무 많은 정보를 사용 하 여 쉽게 오버 로드할 수 있습니다 하 고 포커스를 시도 하는 동안 중단 되는 경우 당황 스 러 합니다. Windows는 보내는 알림의 실행 시기를 고려해 야 하는 방법에 대 한 몇 가지 옵션을 제공 합니다.

**원시 알림:** 사용 하 여 [원시 알림](raw-notification-overview.md) ot 사용자에 게는 중단을 최소화 하더라도 여러 가지 이유로 유용할 수 있습니다.  원시 알림 보내기 시작 앱 백그라운드에서 알림을 통해 앱의 컨텍스트에서 즉시 제공 하는 것이 있는지 여부를 평가할 수 있도록 합니다. 것으로 생각 될 것 표시할지 사용자에 게 즉시 팝 할 수 있으면 한 [로컬 알림](send-local-toast.md) 거기서에서 합니다.  사용자를 보려면 필요가 없습니다 지금, 만들 수는 [알림 예약](https://blogs.msdn.microsoft.com/tiles_and_toasts/2016/09/30/quickstart-sending-an-alarm-in-windows-10/) 는 나중에 실행 됩니다.


**알림 메시지를 삭제할:** 오른쪽 아래 화면에 팝업를 건너뛰어 대신 관리 센터에 직접 알림을 전송 하는 알림이 발생할 수도 있습니다. 설정 하 여 이렇게 합니다 [SupressPopup 속성](https://docs.microsoft.com/en-us/uwp/api/windows.ui.notifications.toastnotification.suppresspopup) True로 합니다. 2 관리 센터 외부에서 알림을 팝 하지 주위 일부 회의적인 있을 수 있습니다, 있지만 볼-3 배 높은 engagement를 통해 관리 센터에서 작동 하는 알림 메시지 표시에 대 한 알림 메시지를 팝 합니다.  사용자는 알림을 받을 준비가 되 고 때 이러한 중단 되 면 이기 때문에 간접적 사용자에 게 알리기 위한 훨씬 더 효과적일 수 있는 작업 센터에서 콘텐츠를 제어할 수 응답성이 향상 됩니다.

## <a name="3-clear-out-the-clutter"></a>3. 혼잡 함을 지웁니다
알림 관리 센터에서 비교적 오랜 시간 (기본값 3 일) 동안 유지할 수 있습니다.  여기에 상주 하는 콘텐츠는 최신 및 관련 사용자가 관리 센터를 열 때마다 없는지 확인 하는 반드시 합니다. 사용자의 화면 공간을 낭비 하 최신 용도로 사용할 수 있는 슬롯을 차지 합니다.  사용자 전자 메일 관리 앱을 설치 하 고 10 개의 전자 메일 및 전자 메일와 함께 10 알림을 받는 경우를 가정해 보십시오.  원하는 환경을 따라 사용자가 해당 전자 메일 읽기 나 관리 센터에서 이전 혼란을 제거 하는 방법으로 앱을 열 경우 이러한 알림을 지우는 것을 고려할 수 있습니다.

일련의 있다고 [ToastNotificationHistory](https://docs.microsoft.com/en-us/uwp/api/windows.ui.notifications.toastnotificationhistory) Api는 이러한 알림을 관리할 수 있을 뿐만 아니라 관리 센터에서 어떤 콘텐츠를 볼 수 있습니다. 사용자의 화면 공간의 정직 있고 사용자에 게 전문적이 고 현재 콘텐츠 표시는 주의 합니다.

## <a name="4-keeping-organized"></a>4. 구성 유지
이전에 설명한 대로 관리 센터의 콘텐츠는 3 일 동안 유지 됩니다.  신속 하 게 찾고 있는 정보를 선택 하는 사용자를 위해 사용 하 여 작업 센터에서 알림을 구성할 [헤더](https://docs.microsoft.com/en-us/windows/uwp/design/shell/tiles-and-notifications/toast-headers) 하거나 [컬렉션](https://docs.microsoft.com/en-us/uwp/api/windows.ui.notifications.toastcollection)합니다. 아래 헤더의 예제를 볼 수 있습니다.

![헤더를 사용 하 여 알림 예제 레이블이 'Camping!!'](images/toast-headers-action-center.png)

관련 콘텐츠를 함께 유지 되도록 방식으로 이러한 알림을 그룹 (즉, 스포츠 앱에서 다양 한 스포츠 leagues 분리 또는 그룹 채팅으로 메시지 정렬 생각). 컬렉션은 헤더는 미묘한 하지만 둘 다에 심사 하 고 보다 신속 하 게 알림을 선택 할 수 있습니다. 반면 그룹 알림을 보다 명확 하 게 방법입니다.

## <a name="other-resources"></a>다른 리소스
이러한 네 가지 위의 지점은 첫 번째 및 세 번째 파티 실험 및 고유한 분석 원격 분석을 통해 효과적인 개는 지침입니다. 그러나 염두에서에 둡니다,는 이러한 지침 여기에 해당 합니다: 지침입니다.  이러한 규칙 참여 및 알림의 생산성 향상에 도움이 될 하지만 사용자 중심 생각을 배우고 자신의 데이터에서 대체할 수 nothing 확신 합니다.  

UWP 앱에 현재 알림을 보내는 경우의 알림 내용에 분석을 볼 수 있습니다 [파트너 센터](https://partner.microsoft.com/dashboard)! 사용 하는 경우이 데이터 무료 제공 합니다 [저장소 서비스 SDK](https://marketplace.visualstudio.com/items?itemName=AdMediator.MicrosoftStoreServicesSDK) 또는 [WNS Api](https://docs.microsoft.com/en-us/windows/uwp/design/shell/tiles-and-notifications/windows-push-notification-services--wns--overview)합니다. 이러한 메트릭을 제공 합니다 되나요 windows 플랫폼에서 알림을 확인할도 사용자가 알림과 상호 작용 하는 방법과. 왼쪽 참여에서 메뉴로 이동 하 여이 데이터에 액세스 > 알림, 알림 페이지 내에서 "분석" 탭을 클릭 합니다.  이 파트너 센터에서 알림을 보내는 이동 해야 하 고 동일한 위치에 있습니다.

## <a name="related-topics"></a>관련 항목

* [알림 콘텐츠](adaptive-interactive-toasts.md)
* [원시 알림](raw-notification-overview.md)
* [보류 중인 업데이트](toast-pending-update.md)
* [GitHub (Windows 커뮤니티 도구 키트의 일부)에 대 한 알림 라이브러리](https://github.com/Microsoft/UWPCommunityToolkit/tree/master/Microsoft.Toolkit.Uwp.Notifications)
