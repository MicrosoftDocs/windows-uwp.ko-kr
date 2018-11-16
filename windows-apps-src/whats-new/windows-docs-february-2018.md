---
author: QuinnRadich
title: 2018년 2월 Windows 문서의 새로운 내용 - UWP 앱 개발
description: 2018년 2월 Windows 10 개발자 설명서에 추가된 새로운 기능, 동영상 및 개발자 지침
keywords: 새로운 기능, 업데이트, 기능, 개발자 지침, Windows 10, 2월
ms.author: quradic
ms.date: 2/5/2018
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: d6eb9b95ad7c5f7c63bca5b11fbbbf9018b75fcf
ms.sourcegitcommit: e2fca6c79f31e521ba76f7ecf343cf8f278e6a15
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/15/2018
ms.locfileid: "6981676"
---
# <a name="whats-new-in-the-windows-developer-docs-in-february-2018"></a>2018년 2월 Windows 개발자 문서의 새로운 내용

Windows 개발자 설명서는 Windows 플랫폼 전체에서 개발자가 사용할 수 있는 새로운 기능에 대한 정보로 계속 업데이트되고 있습니다. 다음과 같은 기능 개요, 개발자 지침 및 동영상이 Windows 개발자를 위한 새 정보와 업데이트된 정보를 포함하여 1월에 추가되었습니다.

Windows 10에 [도구 및 SDK를 설치](http://go.microsoft.com/fwlink/?LinkId=821431)하면 [새로운 유니버설 Windows 앱을 생성](../get-started/create-uwp-apps.md)하거나 [Windows의 기존 앱 코드](../porting/index.md)를 사용하는 방법을 알아볼 수 있습니다.


## <a name="features"></a>기능

### <a name="adaptive-and-interactive-toast-notifications"></a>적응형 및 대화형 알림 메시지

적응형 및 대화형 알림으로 앱을 개선합니다. [업데이트된 알림 메시지에 대한 지침](../design/shell/tiles-and-notifications/adaptive-interactive-toasts.md)으로 시작하고, 이미지 크기 제한, 진행률 표시줄, 그리고 입력 옵션 추가에 대한 새로운 정보를 살펴보세요.

[사용자 지정 타임스탬프](../design/shell/tiles-and-notifications/custom-timestamps-on-toasts.md), [사용자 지정 오디오](../design/shell/tiles-and-notifications/custom-audio-on-toasts.md), 또는 [진행률 표시줄](../design/shell/tiles-and-notifications/toast-progress-bar.md)을 사용하여 알림 메시지를 사용자 지정합니다.

[알림 미러링](../design/shell/tiles-and-notifications/notification-mirroring.md) 및 [유니버설 해제](../design/shell/tiles-and-notifications/universal-dismiss.md)를 사용하여 사용자의 디바이스로 알림을 확장합니다.

[알림 머리글](../design/shell/tiles-and-notifications/toast-headers.md) 및 [보류 중인 업데이트 정품 인증](../design/shell/tiles-and-notifications/toast-pending-update.md)을 사용하여 알림을 그룹화 및 구성합니다.

![실행 중인 보류 중인 업데이트 정품 인증](../design/shell/tiles-and-notifications/images/toast-pendingupdate.gif)

### <a name="page-layouts-with-xaml"></a>XAML을 사용하는 페이지 레이아웃

[XAML 페이지 레이아웃](../design/layout/layouts-with-xaml.md) 문서가 유연한 레이아웃 및 시각적 상태에 대한 새로운 정보로 업데이트되었습니다. 이러한 새로운 기능을 사용하며 귀하의 앱의 여러 요소의 위치를 지정하는 방법과 사용 가능한 시각적 공간에 맞추는 방법에 대해 보다 더 잘 제어할 수 있습니다.

![XAML 페이지 레이아웃의 여백 및 안쪽 여백](../design/layout/images/xaml-layout-margins-padding.png)

### <a name="subscription-add-ons-are-now-available-to-all-developers"></a>이제 모든 개발자가 구독 추가를 사용할 수 있습니다.

구독 추가를 만들고 게시하여 자동화된 반복 청구 기간으로 귀하의 앱 및 게임에서 디지털 제품(앱 기능 또는 디지털 콘텐츠)을 판매할 수 있습니다. 자세한 내용은 [앱에 구독 추가 기능을 사용하도록 설정](../monetize/enable-subscription-add-ons-for-your-app.md)을 참조하세요.

## <a name="developer-guidance"></a>개발자 지침

### <a name="design-basics"></a>디자인 기본 사항

[UWP 앱 디자인 소개](../design/basics/design-and-ui-intro.md)가 새로운 시각적 예시들로 향상되었습니다. 모든 UWP의 유니버설 디자인 기능 개요가 이제 Fluent 디자인 시스템의 기능을 실행하는 방법 강조합니다.

앱에서 여러 유형의 콘텐츠를 표시하는 방법에 대한 여러 예시를 제공하는 [콘텐츠 디자인 기본 사항](../design/basics/content-basics.md)에 일반적인 페이지 패턴에 대한 소개가 추가되었습니다.

![허브 페이지 패턴의 일러스트레이션](../design/basics/images/hub.png)

### <a name="writing-style"></a>쓰기 스타일

[쓰기 스타일 지침](../design/style/writing-style.md)에 음성 및 톤과 이를 변환하는 내용이 업그레이드 및 확장되었습니다. 이 새로운 정보는 앱에서 효과적인 텍스트를 만들기 위한 원칙을 제공하며 오류 메시지 또는 대화 상자 등의 컨트롤 쓰기에 대한 모범 사례를 제안합니다.

### <a name="getting-started-for-game-development"></a>게임 개발 시작

Windows10 게임 개발에 흥미가 있으신가요? 새로운 [게임 개발 시작](../gaming/getting-started.md) 페이지는 앱과 게임을 스스로 설정 및 등록하고 제출하기 위해 수행해야 할 작업에 대한 전체 개요를 제공 합니다.

## <a name="videos"></a>비디오

### <a name="xbox-live-creators-program"></a>Xbox Live 크리에이터스 프로그램

Xbox Live 크리에이터스 프로그램을 사용하면 개발자는 UWP 게임을 신속하게 Xbox One 및 Windows 10에 게시할 수 있습니다. [이 동영상을 시청](https://www.youtube.com/watch?v=zpFfHHBkVq4)하여 프로그램에 대해 학습한 다음 [이 페이지를 확인](https://www.xbox.com/developers/creators-program)하여 시작합니다.

### <a name="creating-3d-app-launchers-for-windows-mixed-reality"></a>Windows Mixed Reality용 3D 앱 시작 관리자 만들기

3D 시작 관리자는 사용자가 그들의 Mixed Reality 홈 환경에서 앱의 진정한 현실감 표현을 배치할 수 있는 독특한 방법을 제공합니다. [동영상을 시청](https://www.youtube.com/watch?v=TxIslHsEXno)하여 3D 모델을 준비하고 이를 앱에 대한 시작 관리자로 지정하는 방법을 알아보고 자세한 내용은 [개발자 문서를 읽거나](https://developer.microsoft.com/windows/mixed-reality/implementing_3d_app_launchers) [디자인 지침을 확인](https://developer.microsoft.com/windows/mixed-reality/3d_app_launcher_design_guidance)하세요.

### <a name="motion-controller-tracking"></a>모션 컨트롤러 추적

모션 컨트롤러는 Windows Mixed Reality에서 사용자의 손을 나타냅니다. [동영상을 시청](https://www.youtube.com/watch?v=rkDpRllbLII)하여 Mixed Reality 헤드셋의 보기 필드 안과 밖에서 모션 컨트롤러가 어떻게 작동하는지 알아보고 [여기에서 컨트롤러 추적에 대해 자세한 내용을 읽어보십시오.](https://developer.microsoft.com/windows/mixed-reality/motion_controllers#controller_tracking_state%E2%80%9D)