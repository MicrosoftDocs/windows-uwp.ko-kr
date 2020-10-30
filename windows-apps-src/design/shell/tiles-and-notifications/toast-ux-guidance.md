---
description: 사용자의 생산성과 만족을 높여 주는 효과적이 고 사용자 중심의 알림을 만드는 방법을 알아봅니다.
title: 알림 UX 지침
label: Toast UX Guidance
template: detail.hbs
ms.date: 05/18/2018
ms.topic: article
keywords: windows 10, uwp, 알림, 수집, 그룹, ux, ux 지침, 지침, 작업, 알림, 동작 센터, noninterruptive, 유효 알림, 비 침입 알림, 실행 가능, 관리, 구성
ms.localizationpriority: medium
ms.openlocfilehash: d6ad253ccfa744864caa8d0229d09ba40d066771
ms.sourcegitcommit: a3bbd3dd13be5d2f8a2793717adf4276840ee17d
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/30/2020
ms.locfileid: "93033046"
---
# <a name="toast-notification-ux-guidance"></a>알림 메시지 UX 지침
알림은 최신 수명에서 필요한 부분입니다. 사용자는 앱 및 웹 사이트를 사용 하 여 생산성을 높이고 최신 업데이트를 유지 하는 데 도움이 됩니다. 그러나 알림은 사용자 중심 방식으로 설계 되지 않은 경우에는 더 유용한 방법으로 신속 하 게 전환할 수 있습니다. 알림은 마우스 오른쪽 단추를 끌 때 발생 하는 것 이며, 꺼져 있는 경우에는 다시 설정 됩니다.  따라서 사용자의 화면 공간과 시간에 대 한 알림이 respectful이 engagement 채널을 열어 둘 수 있습니다.

> **중요 한 api** : [Windows Community Toolkit notification nuget 패키지](https://www.nuget.org/packages/Microsoft.Toolkit.Uwp.Notifications/)

Microsoft는 Windows 원격 분석과 기타 첫 번째와 타사 사례 연구를 분석 하 여 훌륭한 알림 스토리를 만드는 방법에 대해 네 가지 규칙을 제공 합니다.  이러한 규칙은 플랫폼에 관계 없이 보편적으로 적용할 수 있으며, 알림이 사용자에 게 긍정적인 영향을 줄 수 있도록 지원 합니다.

## <a name="1-actionable-notifications"></a>1. 실행 가능한 알림
실행 가능한 알림을 통해 사용자는 앱을 열지 않고도 생산적으로 작업할 수 있습니다.  앱이 시작 되는 것은 좋지만이는 성공의 유일한 측정값이 아니라 사용자가 앱으로 이동 하지 않고도 작은 작업을 수행할 수 있도록 하는 것은 사용자에 게 매우 강력한 도구 만족 수 있습니다.

![입력 텍스트 상자 및 단추를 사용 하 여 미리 알림을 설정 하 고 알림에 응답 하는 조치 가능한 알림](images/actionable-notification-example01.png)

다음은 작업을 활용 하는 알림의 예입니다. 작업을 완료 하는 데는 전체적으로 긍정적인 느낌이 필요 하며, 실행 가능한 콘텐츠가 있는 알림을 보내 앱 또는 웹 사이트에 이러한 느낌을 제공할 수 있습니다. 작업 가능한 알림은 사용자가 이러한 작은 작업을 수행 하는 데 걸리는 시간을 단축 함으로써 엔터프라이즈 및 소비자 시나리오에서 생산성을 높일 수도 있습니다. 사용자가 정기적으로 수행 하는 작업 또는 사용자가 수행 하도록 학습 하려는 작업을 포함 하는 것이 좋습니다.  일부 사례:
* Favoriting, 플래그 또는 주연 콘텐츠
* 비용 보고서, 휴가, 사용 권한 등을 승인 하거나 거부 합니다.
* 메시지, 전자 메일, 그룹 채팅, 의견 등에 인라인 회신
* [보류 중인 업데이트](toast-pending-update.md) 를 사용 하 여 주문 완료
* 다른 시간에 대 한 경고 또는 미리 알림 설정 및 일정에 대 한 일정 예약 시간 설정

실행 가능한 알림은 사용자가 생산성을 유지 하 고, 작업을 수행 하 고, 앱 또는 웹 사이트에서 효율적이 고 효율적인 환경을 제공 하기 위한 매우 강력한 도구입니다.  여기에는 많은 기회가 있습니다. 아이디어를 브레인스토밍 하는 데 도움이 필요한 경우에는 windows 알림 팀에 자유롭게 연락 하세요.

## <a name="2-timing-and-urgency"></a>2. 타이밍 및 긴급도
알림을 자주 생각 하는 것과 달리 실시간이 반드시 필요 하지는 않습니다. 개발자는 사용자에 대해 생각 하 고 보내는 알림이 긴급 정보 인지 여부를 생각해 볼 수 있습니다. 사용자는 너무 많은 정보를 사용 하 여 쉽게 오버 로드 될 수 있으며, 포커스를 시도 하는 동안 중단 되는 경우 불편을 얻을 수 있습니다. Windows에서는 보내는 알림의 개입 수준을 고려 하는 방법에 대 한 몇 가지 옵션을 제공 합니다.

**원시 알림:** 특히 사용자의 중단을 최소화 하는 등의 다양 한 이유로 [원시 알림을](raw-notification-overview.md) 사용 하는 것이 유용할 수 있습니다.  원시 알림을 보내는 작업은 백그라운드에서 응용 프로그램의 절전 모드를 해제 하므로 알림이 앱의 컨텍스트에서 즉시 배달 될 수 있는지 여부를 평가할 수 있습니다. 사용자에 게 사용자에 게 표시 되어야 하는 사항이 있으면 여기에서 [로컬 알림 메시지](send-local-toast.md) 를 표시할 수 있습니다.  사용자가 지금 바로 볼 필요가 없는 경우 나중에 발생 하는 [예약 된 알림 메시지](/archive/blogs/tiles_and_toasts/quickstart-sending-an-alarm-in-windows-10) 를 만들 수 있습니다.


**고스트 알림:** 화면의 오른쪽 아래 모서리에서 팝 하는 알림을 실행 하 고 대신 알림 메시지를 알림 센터에 직접 보낼 수도 있습니다. 이 작업은 [Su보도 Spopup 속성](/uwp/api/windows.ui.notifications.toastnotification.suppresspopup) 을 True로 설정 하 여 수행 합니다. 액션 센터 외부에서 회의가 알림을 팝 하지 않을 수 있지만, 팝 된 알림을 통해 작업 센터에 살고 있는 알림을에 대 한 2-3 배 높은 참여를 확인할 수 있습니다.  사용자는 알림을 받을 준비가 되 고 중단 된 시간을 제어할 수 있는 경우 보다 응답성이 향상 됩니다. 즉, 사용자에 게 알릴 때 noninvasively에 더 효과적일 수 있습니다.

## <a name="3-clear-out-the-clutter"></a>3. 복잡 한 내용을 지웁니다.
알림은 매우 긴 시간 동안 (기본값은 3 일) 작업 센터에서 지속 될 수 있습니다.  여기에 있는 콘텐츠는 사용자가 작업 센터를 열 때마다 최신이 고 관련이 있는지 확인 해야 합니다. 사용자의 화면 공간을 낭비 하 고 최신 정보에 사용할 수 있는 슬롯을 만드는 것입니다.  사용자가 전자 메일 관리 앱을 설치 하 고 해당 전자 메일을 통해 10 개의 메일과 10 개의 알림을 받는 경우를 가정해 보겠습니다.  원하는 환경에 따라 사용자가 해당 전자 메일을 읽거나 작업 센터에서 이전 작업을 제거 하는 방법으로 앱을 여는 경우 해당 알림을 지울 수 있습니다.

작업 센터에서 콘텐츠를 볼 수 있을 뿐만 아니라 이러한 알림을 관리 하는 일련의 [To Notificationhistory](/uwp/api/windows.ui.notifications.toastnotificationhistory) api가 있습니다. 사용자의 화면 공간을 respectful 하 고 사용자에 게 관련 된 현재 콘텐츠만 표시 됩니다.

## <a name="4-keeping-organized"></a>4. 구성 유지
앞서 언급 했 듯이, 관리 센터의 콘텐츠는 3 일 동안 지속 됩니다.  사용자가 빠르게 찾고 있는 정보를 선택 하는 데 도움이 되도록, [헤더](./toast-headers.md) 또는 [컬렉션](/uwp/api/windows.ui.notifications.toastcollection)을 사용 하 여 알림 센터에서 알림을 구성 합니다. 아래 헤더의 예제를 볼 수 있습니다.

![헤더가 ' 캠핑!! ' 인 알림 예제](images/toast-headers-action-center.png)

관련 콘텐츠가 함께 유지 되도록 이러한 알림을 그룹화 합니다. 즉, 스포츠 앱에서 다른 스포츠 leagues를 분리 하거나 그룹 채팅을 통해 메시지를 정렬 하는 것을 고려해 야 합니다. 컬렉션은 알림을 그룹화 하는 보다 분명 한 방법 이지만 헤더는 더 미묘한 반면, 둘 다 사용자가 알림을 더 빠르게 심사 하 고 선택할 수 있습니다.



위의 4 개 요소는 원격 분석을 자체적으로 분석 하 고 첫 번째와 타사 실험을 통해 효과적으로 발견 한 지침입니다. 그러나 이러한 지침에 대해서는 지침을 염두에 두어야 합니다.  이러한 규칙을 통해 알림의 참여 및 생산성을 높일 수 있지만 사용자 중심 생각을 대체 하 고 사용자 고유의 데이터를 학습할 수 없습니다.  

## <a name="related-topics"></a>관련 항목

* [알림 콘텐츠](adaptive-interactive-toasts.md)
* [원시 알림](raw-notification-overview.md)
* [보류 중인 업데이트](toast-pending-update.md)
* [GitHub의 알림 라이브러리 (Windows 커뮤니티 도구 키트의 일부)](https://github.com/windows-toolkit/WindowsCommunityToolkit/tree/master/Microsoft.Toolkit.Uwp.Notifications)
