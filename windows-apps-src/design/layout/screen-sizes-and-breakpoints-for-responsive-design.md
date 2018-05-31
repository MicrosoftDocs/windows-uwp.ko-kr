---
author: mijacobs
title: 반응형 디자인에 대한 화면 크기 및 중단점
description: Windows 10 에코시스템을 구성하는 다수의 장치에 맞게 UI를 최적화하기보다는 중단점이라고 불리는 몇 가지 주요 너비 카테고리에 맞게 설계하는 더욱 좋다고 권장하였습니다.
ms.author: mijacobs
ms.date: 08/30/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 0d84530c1a7c3795c566495c1eae121691b0766a
ms.sourcegitcommit: 6618517dc0a4e4100af06e6d27fac133d317e545
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/28/2018
ms.locfileid: "1688939"
---
#  <a name="screen-sizes-and-breakpoints"></a>화면 크기 및 중단점

UWP 앱은 휴대폰, 태블릿, 데스크톱, TV 등 Windows 10 기반 장치라면 어디에서든지 실행할 수 있습니다. Windows 10 에코시스템은 구성하는 장치 및 화면 크기가 매우 많기 때문에 각 장치마다 UI를 최적화하기보다는 몇 가지 주요 너비 카테고리(중단점)에 맞게 디자인하는 것이 좋다고 권장하였습니다. 
- 소형(640px 이하)
- 중간(641px ~ 1007px)
- 대형(1008px 이상)

> [!TIP]
> 특정 중단점에 맞게 디자인할 때는 화면 크기가 아닌, 앱에서 사용할 수 있는 화면 공간(앱의 창) 크기를 고려해서 디자인하세요. 앱이 전체 화면으로 실행되면 앱의 창과 화면 크기가 동일하지만 앱이 전체 화면이 아닐 때는 창이 화면보다 작습니다.

## <a name="breakpoints"></a>중단점
이 표는 다양한 크기 클래스와 중단점을 설명한 것입니다.

![반응형 디자인 중단점](images/rsp-design/rspd-breakpoints.png)

<table>
<thead>
<tr class="header">
<th align="left">크기 클래스</th>
<th align="left">중단점</th>
<th align="left">화면 크기(대각선)</th>
<th align="left">장치</th>
<th align="left">창 크기</th>
</tr>
</thead>
<tbody>
<tr class="even">
<td style="vertical-align:top;">소형</td>
<td style="vertical-align:top;">640px 이하</td>
<td style="vertical-align:top;">4&quot; ~ 6&quot;</td>
<td style="vertical-align:top;">휴대폰</td>
<td style="vertical-align:top;">320x569, 360x640, 480x854</td>
</tr>
<tr class="odd">
<td style="vertical-align:top;">중간</td>
<td style="vertical-align:top;">641px ~ 1007px</td>
<td style="vertical-align:top;">7&quot; ~ 12&quot;</td>
<td style="vertical-align:top;">패블릿, 태블릿, TV</td>
<td style="vertical-align:top;">960 x 540</td>
</tr>
<tr class="even">
<td style="vertical-align:top;">대형</td>
<td style="vertical-align:top;">1008px 이상</td>
<td style="vertical-align:top;">13&quot; 이상</td>
<td style="vertical-align:top;">PC, 노트북, Surface Hub</td>
<td style="vertical-align:top;">1024 x 640, 1366 x 768, 1920 x 1080</td>
</tr>
</tbody>
</table>

## <a name="general-recommendations"></a>일반 권장 사항

### <a name="small"></a>소형
- 왼쪽 및 오른쪽 창 여백을 12px로 설정하고 앱 창의 왼쪽 및 오른쪽 가장자리 간에 시각적 구분을 만듭니다.
- 접근성 향상을 위해 창의 맨 아래에 [앱 바](../controls-and-patterns/app-bars.md)를 도킹합니다.
- 한 번에 하나의 열/영역을 사용합니다.
- 아이콘을 사용하여 검색을 나타냅니다(검색 상자를 표시하지 않음).
- [탐색 창](../controls-and-patterns/navigationview.md)을 오버레이 모드로 전환하여 화면 공간 절약
- [마스터 세부 정보 패턴](../controls-and-patterns/master-details.md)을 사용하는 경우 누적된 프레젠테이션 모드를 사용하여 화면 공간을 절약할 수 있습니다.

### <a name="medium"></a>중간
- 왼쪽 및 오른쪽 창 여백을 24px로 설정하고 앱 창의 왼쪽 및 오른쪽 가장자리 간에 시각적 구분을 만듭니다.
- [앱 바](../controls-and-patterns/app-bars.md) 등의 명령 요소를 앱 창의 맨 위에 배치합니다.
- 열/영역은 최대 2개까지 사용합니다.
- 검색 상자를 표시합니다.
- 좁은 아이콘 스트립이 항상 표시되도록 [탐색 창](../controls-and-patterns/navigationview.md)을 작은 모드로 전환합니다.
- [TV 환경](http://go.microsoft.com/fwlink/?LinkId=760736)에 맞는 추가 조정을 고려합니다.

### <a name="large"></a>대형
- 왼쪽 및 오른쪽 창 여백을 24px로 설정하고 앱 창의 왼쪽 및 오른쪽 가장자리 간에 시각적 구분을 만듭니다.
- [앱 바](../controls-and-patterns/app-bars.md) 등의 명령 요소를 앱 창의 맨 위에 배치합니다.
- 열/영역은 최대 3개까지 사용합니다.
- 검색 상자를 표시합니다.
- 항상 표시되도록 [탐색 창](../controls-and-patterns/navigationview.md)을 도킹 모드로 전환합니다.

>[!TIP] 
> [**휴대폰용 Continuum**](http://go.microsoft.com/fwlink/p/?LinkID=699431)을 통해 사용자는 호환되는 Windows 10 Mobile 장치를 모니터, 마우스 및 키보드에 연결하여 휴대폰을 노트북처럼 사용할 수 있습니다. 특정 중단점에 대해 디자인할 때 이 새로운 기능에 유의하세요. 휴대폰이 항상 크기 클래스로 유지되지는 않습니다.

## <a name="effective-pixels-and-scale-factor"></a>유효 픽셀 및 배율

UWP 앱은 모든 Windows 10 장치에서 앱을 쉽게 알아볼 수 있도록 UI 크기를 자동으로 조정합니다. Windows는 장치의 가시거리와 DPI(인치당 도트 수)를 기준으로 각 디스플레이에 맞게 크기를 자동으로 조정합니다. 사용자는 **설정** > **디스플레이** > **배율 및 레이아웃** 설정 페이지에서 기본 값을 재정의할 수 있습니다. 
