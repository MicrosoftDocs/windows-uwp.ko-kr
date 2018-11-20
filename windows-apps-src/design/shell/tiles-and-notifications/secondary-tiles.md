---
author: andrewleader
Description: Secondary tiles allow users to pin specific content and deep links from your app onto their Start menu, providing easy future access to the content within your app.
title: 보조 타일
label: Secondary tiles
template: detail.hbs
ms.author: wdg-dev-content
ms.date: 05/25/2017
ms.topic: article
keywords: windows 10, uwp, 보조 타일
ms.localizationpriority: medium
ms.openlocfilehash: e27786701fa2ae9ac00a7eab57e840ec9a0dc811
ms.sourcegitcommit: ed0304b8a214c03b8aab74b8ef12c9f82b8e3c5f
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/19/2018
ms.locfileid: "7297997"
---
# <a name="secondary-tiles"></a>보조 타일


사용자가 보조 타일을 통해 앱의 특정 콘텐츠 및 딥 링크를 시작 메뉴에 고정해 두면 나중에 앱 내에서 해당 콘텐츠에 간편하게 액세스할 수 있습니다.

![보조 타일의 스크린샷](images/secondarytiles.png)

예를 들어 사용자는 수많은 특정 위치의 날씨를 시작 메뉴에 고정할 수 있으며, (1) Live Tile 덕분에 현재 날씨에 대한 정보를 한 눈에 볼 수 있는 실시간 정보, 그리고 (2) 사용자가 중요하게 생각하는 특정 도시의 날씨로 신속하게 이동하는 진입점이 제공됩니다. 또한 사용자는 특정 주식, 뉴스 기사 등 자신에게 중요한 기타 항목을 고정할 수 있습니다.

앱에 보조 타일을 추가하면 사용자가 신속하고 효율적으로 앱을 사용할 수 있으며, 보조 타일을 통해 간편하게 액세스할 수 있기 때문에 좀 더 자주 앱으로 돌아옵니다.

**오직 사용자만이 보조 타일을 고정할 수 있으며, 앱은 사용자 동의 없이는 프로그래밍 방식으로 보조 타일을 고정할 수 없습니다**. 사용자가 앱 내에서 "고정" 단추를 명시적으로 클릭해야 하며, 클릭하는 순간 개발자는 API를 사용하여 보조 타일을 만들라고 요청합니다. 그러면 시스템에서 타일을 고정할 것인지 사용자에게 확인을 요청하는 대화 상자를 표시합니다.

## <a name="quick-links"></a>빠른 연결

| 문서 | 설명 |
| --- | --- |
| [보조 타일에 대한 지침](secondary-tiles-guidance.md) | 보조 타일을 언제 어디에 사용해야 하는지 알아보세요. |
| [보조 타일 고정](secondary-tiles-pinning.md) | 보조 타일을 고정하는 방법을 알아보세요. |
| [데스크톱 응용 프로그램에서 고정](secondary-tiles-desktop-pinning.md) | Windows 데스크톱 응용 프로그램은 데스크톱 브리지 덕분에 보조 타일을 고정할 수 있습니다! |


## <a name="secondary-tiles-in-relation-to-primary-tiles"></a>기본 타일과 관련된 보조 타일

보조 타일은 단일 상위 앱과 연결됩니다. 보조 타일은 시작 메뉴에 고정되어 사용자에게 상위 앱에서 자주 사용하는 영역을 바로 시작할 수 있는 일관적이고 효율적인 방법을 제공합니다. 이것은 자주 업데이트되는 콘텐츠가 포함된 상위 앱의 일반적인 하위 섹션일 수도 있고 앱의 특정 영역에 대한 딥 링크일 수도 있습니다.

다음은 보조 타일 시나리오의 예입니다.

* 날씨 앱의 특정 도시에 대한 날씨 업데이트
* 일정 앱의 예정된 이벤트 요약
* 소셜 앱의 중요한 연락처 상태 및 업데이트
* RSS 수집기의 특정 피드
* 음악 재생 목록
* 블로그

사용자가 모니터링하려는 자주 변경되는 콘텐츠는 보조 타일의 좋은 후보입니다. 보조 타일을 고정한 후 사용자는 타일을 통해 한 눈에 보는 업데이트를 수신하여 상위 앱을 바로 시작하는 데 사용할 수 있습니다.

보조 타일은 여러 가지 면에서 기본 타일과 유사합니다.

* 타일 알림을 사용하여 풍부한 콘텐츠를 표시됩니다.
* 기본 타일 콘텐츠에 대한 150 x 150 픽셀 로고를 포함해야 합니다.
* 필요에 따라 더 큰 타일 크기를 사용하도록 다른 로고 크기를 포함할 수 있습니다.
* 알림과 배지를 표시할 수 있습니다.
* 시작 메뉴에 다시 정렬할 수 있습니다.
* 앱이 제거되면 자동으로 삭제됩니다.
* 배지 및 잠금 자세한 상태 텍스트를 잠금에 표시할 수 있습니다.

그러나 보조 타일은 몇 가지 눈에 띄는 방면에서 기본 타일과 차이가 있습니다.

* 사용자가 언제든지 상위 앱을 삭제하지 않고도 보조 타일을 삭제할 수 있습니다.
* 보조 타일은 런타임에 만들 수 있습니다. 앱 타일은 설치 중에만 만들 수 있습니다.
* 플라이아웃은 보조 타일을 추가하기 전에 사용자에게 확인을 요청합니다.
* 사용자 요청을 통해 프로그래밍 방식으로 잠금 화면에 대해 선택할 수 없습니다. 사용자가 PC 설정의 개인 설정 페이지를 통해 보조 타일을 수동으로 추가 해야 합니다.

알림 보내기의 경우 보조 타일에 사용되는 타일 및 배지 업데이트와 푸시 알림 채널에 대한 특정 메서드가 제공됩니다. 이는 기본 타일에 사용되는 버전과 비슷합니다. CreateBadgeUpdaterForApplication과 CreateBadgeUpdaterForSecondaryTile을 예로 들 수 있습니다.


## <a name="guidance-on-secondary-tiles"></a>보조 타일에 대한 지침
보조 타일을 언제 어디에 사용해야 하는지, 기타 사용 지침으로는 어떤 것들이 있는지 알아보려면 [보조 타일에 대한 지침](secondary-tiles-guidance.md)을 참조하세요.


## <a name="pinning-secondary-tiles"></a>보조 타일 고정
보조 타일을 고정하는 방법을 알아보려면 [보조 타일 고정](secondary-tiles-pinning.md)을 참조하세요.


## <a name="desktop-applications-and-secondary-tiles"></a>데스크톱 응용 프로그램 및 보조 타일
데스크톱 브리지를 통해 데스크톱 응용 프로그램에서 보조 타일을 사용하는 방법을 알아보려면 [데스크톱 응용 프로그램에서 보조 타일 고정](secondary-tiles-desktop-pinning.md)을 참조하세요.
