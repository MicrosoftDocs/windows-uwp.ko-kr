---
Description: Learn how to use tiles, badges, toasts, and notifications to provide entry points into your app and keep users up-to-date.
title: 타일, 배지 및 알림
ms.assetid: 48ee4328-7999-40c2-9354-7ea7d488c538
label: Tiles, badges, and notifications
template: detail.hbs
ms.date: 05/19/2017
ms.topic: article
keywords: Windows 10 uwp
ms.localizationpriority: medium
ms.openlocfilehash: 4ea005dd33bbb5461921fa17eded8430d7648c87
ms.sourcegitcommit: d7613c791107f74b6a3dc12a372d9de916c0454b
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 12/05/2018
ms.locfileid: "8739451"
---
# <a name="tiles-badges-and-notifications-for-uwp-apps"></a>UWP 앱에 대한 타일, 배지 및 알림
 

타일, 배지 및 알림을 사용하여 앱에 대한 진입점을 제공하고 사용자를 최신 상태로 유지하는 방법에 대해 알아봅니다.

> **중요 API**: [UWP 커뮤니티 도구 키트 알림 NuGet 패키지](https://www.nuget.org/packages/Microsoft.Toolkit.Uwp.Notifications/)

<p><img style="float: left; margin: 0px 15px 15px 0px;" src="images/tile-and-live-tile.png" />
타일은 시작 메뉴에 표시되는 앱의 표시를 말합니다. 모든 UWP 앱에는 타일이 있습니다. 다양한 타일 크기(작음, 중간, 넓음 및 큼)를 사용하도록 설정할 수 있습니다.</p>

<p>뉴스 헤드라인 또는 읽지 않은 가장 최근 메시지의 제목과 같은 새 정보를 사용자에게 전달하기 위해 <em>타일 알림</em>을 사용하여 타일을 업데이트할 수 있습니다.</p>

<p><em>배지</em>를 사용하여 시스템 제공 문자 모양 또는 1-99의 숫자 형태로 상태 또는 요약 정보를 제공할 수 있습니다. 배지는 앱에 대한 작업 표시줄 아이콘에도 나타납니다. </p>

<p><em>알림 메시지</em>는 앱에서 <em>알림</em>(또는 <em>배너</em>)이라는 팝업 UI 요소를 통해 사용자에게 보내는 알림입니다. 사용자가 앱에 있는지 여부에 상관없이 알림을 볼 수 있습니다.</p>
<p><em>푸시 알림</em> 또는 <em>원시 알림</em>은 WNS(Windows 푸시 알림 서비스) 또는 백그라운드 작업을 통해 앱에 전송되는 알림입니다. 앱은 관심 가질 만한 사항이 발생했음을 사용자에게 알려(배지 업데이트, 타일 업데이트 또는 알림을 통해) 이러한 알림에 응답하거나 개발자가 선택한 방식으로 응답할 수 있습니다.</p>

 
## <a name="tiles"></a>타일
| 문서 | 설명 |
| --- | --- |
| [타일 만들기](creating-tiles.md) | 앱의 기본 타일을 사용자 지정하고 다양한 화면 크기에 대한 자산을 제공합니다. |
| [앱 아이콘 자산](app-assets.md) | Windows 10 운영 체제에서 다양한 형식으로 나타나는 앱 아이콘 자산은 UWP(유니버설 Windows 플랫폼) 앱에 대한 호출 카드입니다. 다음 지침에서는 시스템에서 앱 아이콘 자산을 표시하는 위치에 대해 자세히 설명하고 가장 완성도 높은 아이콘을 만드는 방법에 대한 자세한 디자인 팁을 제공합니다. |
| [기본 타일 API](primary-tile-apis.md) | 앱의 기본 타일을 고정하도록 요청하고 기본 타일이 현재 고정되어 있는지 확인합니다. |
| [타일 콘텐츠](create-adaptive-tiles.md) | 타일 알림 콘텐츠를 사용 하 여 적응형 지정, Windows10의 새로운 기능 디자인할 수 있도록 고유한 타일 알림 콘텐츠를 다양 한 화면 밀도에 맞게 조정 되는 단순 하 고 유연한 태그 언어를 사용 하 여 합니다. 이 문서에서는 UWP(유니버설 Windows 플랫폼) 앱의 적응형 라이브 타일을 만드는 방법을 설명합니다. |
| [타일 콘텐츠 스키마](../tiles-and-notifications/tile-schema.md) | 다음은 적응형 타일을 만드는 데 사용되는 요소 및 특성입니다. |
| [특수 타일 템플릿](special-tile-templates-catalog.md) | 특수 타일 템플릿은 애니메이션 효과가 추가되었거나 적응형 타일에서 불가능한 작업을 수행할 수 있도록 하는 고유한 템플릿입니다. |
| [로컬 타일 알림 보내기](sending-a-local-tile-notification.md) | 로컬 타일 알림을 보내서 라이브 타일에 다양한 동적 콘텐츠를 추가하는 방법을 알아봅니다. |


## <a name="notifications"></a>알림

| 문서 | 설명 |
| --- | --- |
| [알림 메시지](adaptive-interactive-toasts.md) | 적응형 및 대화형 알림 메시지를 사용하면 더 많은 콘텐츠, 선택적 인라인 이미지 및 선택적 사용자 조작이 포함된 유연한 팝업 알림을 만들 수 있습니다. |
| [로컬 알림 메시지 보내기](send-local-toast.md) | 대화형 알림 메시지를 보내는 방법을 알아봅니다. |
| [알림 시각화 도우미](notifications-visualizer.md) | 라이브 타일을 Windows10에 대 한 알림 시각화 도우미는 개발자가 적응형 디자인에 도움이 되는 [스토어](https://www.microsoft.com/store/apps/notifications-visualizer/9nblggh5xsl1) 에서 새 유니버설 Windows 플랫폼 (UWP) 앱. |
| [알림 전달 방법 선택](choosing-a-notification-delivery-method.md) | 이 문서에서는 타일 및 배지 업데이트와 알림 메시지 콘텐츠를 제공하는 네 가지 알림 옵션(로컬, 예약, 정기 및 푸시)에 대해 설명합니다. |
| [정기 알림 개요](periodic-notification-overview.md) | 정기 알림(폴링된 알림이라고도 함)은 고정된 간격에 따라 클라우드 서비스에서 콘텐츠를 다운로드하여 타일 및 배지를 업데이트합니다. |
| [WNS(Windows 푸시 알림 서비스) 개요](windows-push-notification-services--wns--overview.md) | 타사 개발자는 WNS(Windows 푸시 알림 서비스)를 사용하여 클라우드 서비스에서 알림, 타일, 배지 및 원시 업데이트를 보낼 수 있습니다. WNS는 에너지 효율적이며 신뢰할 수 있는 방법으로 사용자에게 새 업데이트를 전달하는 메커니즘을 제공합니다. |
| [푸시 알림 마법사에서 생성된 코드](the-code-generated-by-the-push-notification-wizard.md) | Visual Studio의 마법사를 사용하여 Azure Mobile Services와 함께 만든 모바일 서비스에서 푸시 알림을 생성할 수 있습니다. Visual Studio 마법사는 시작하는 데 유용한 코드를 생성합니다. 이 항목에서는 마법사에서 프로젝트를 수정하는 방법, 생성된 코드가 수행하는 작업, 이 코드를 사용하는 방법 및 푸시 알림을 최대한 활용하기 위해 다음에 수행할 수 있는 작업에 대해 설명합니다. [WNS(Windows 푸시 알림 서비스) 개요](windows-push-notification-services--wns--overview.md)를 참조하세요. |
| [원시 알림 개요](raw-notification-overview.md) | 원시 알림은 짧고 일반적인 목적의 알림으로, 오로지 사용 안내만 하며 UI 구성 요소는 포함하지 않습니다. 다른 푸시 알림과 마찬가지로, WNS 기능은 클라우드 서비스에서 앱으로 원시 알림을 전달합니다. |
