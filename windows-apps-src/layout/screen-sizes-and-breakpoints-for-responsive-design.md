---
author: mijacobs
title: "반응형 디자인에 대한 화면 크기 및 중단점"
description: .
ms.assetid: BF42E810-CDC8-47D2-9C30-BAA19DCBE2DA
label: Screen sizes and break points
template: detail.hbs
op-migration-status: ready
ms.author: mijacobs
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
translationtype: Human Translation
ms.sourcegitcommit: c6b64cff1bbebc8ba69bc6e03d34b69f85e798fc
ms.openlocfilehash: 7c992aa651069f6876aa920da88ada659480132e
ms.lasthandoff: 02/07/2017

---

#  <a name="screen-sizes-and-break-points-for-responsive-design"></a>반응형 디자인에 대한 화면 크기 및 중단점

<link rel="stylesheet" href="https://az835927.vo.msecnd.net/sites/uwp/Resources/css/custom.css">

Windows 10 에코시스템에서는 장치 대상 및 화면 크기가 너무 다양해서 각각에 맞게 UI를 최적화하는 것에 대해 걱정할 수조차 없습니다. 대신 360, 640, 1024, 1366epx 등의 몇 가지 주요 너비("중단점"이라고도 함)에 대해 디자인하는 것이 좋습니다.

> [!TIP]
> 특정 중단점에 대해 디자인할 때 앱에서 사용할 수 있는 화면 공간(앱의 창) 크기를 고려해서 디자인합니다. 앱이 전체 화면에서 실행되는 경우에는 앱 창이 화면 크기와 같지만 다른 경우에는 더 작습니다.
 

다음 표에는 다양한 크기 클래스가 설명되어 있으며 해당 크기 클래스에 맞추기 위한 일반 권장 사항이 나와 있습니다.

![반응형 디자인 중단점](images/rsp-design/rspd-breakpoints.png)

<table>
<colgroup>
<col width="25%" />
<col width="25%" />
<col width="25%" />
<col width="25%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">크기 클래스</th>
<th align="left">작음</th>
<th align="left">보통</th>
<th align="left">큼</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td style="vertical-align:top;">일반적인 화면 크기(대각선)</td>
<td style="vertical-align:top;">4&quot;~6&quot;</td>
<td style="vertical-align:top;">7&quot;~12&quot; 또는 TV</td>
<td style="vertical-align:top;">13&quot; 이상</td>
</tr>
<tr class="even">
<td style="vertical-align:top;">일반 장치</td>
<td style="vertical-align:top;">휴대폰</td>
<td style="vertical-align:top;">패블릿, 태블릿, TV</td>
<td style="vertical-align:top;">PC, 노트북, Surface Hub</td>
</tr>
<tr class="odd">
<td style="vertical-align:top;">일반적인 창 크기(유효 픽셀)</td>
<td style="vertical-align:top;">320x569, 360x640, 480x854</td>
<td style="vertical-align:top;">960x540, 1024x640</td>
<td style="vertical-align:top;">1366x768, 1920x1080</td>
</tr>
<tr class="even">
<td style="vertical-align:top;">창 너비 중단점(유효 픽셀)</td>
<td style="vertical-align:top;">640px 이하</td>
<td style="vertical-align:top;">641px - 1007px</td>
<td style="vertical-align:top;">1008px 이상</td>
</tr>
<tr class="odd">
<td style="vertical-align:top;">일반 권장 사항</td>
<td style="vertical-align:top;"><ul>
<li>탭 요소를 가운데 정렬합니다.</li>
<li>왼쪽 및 오른쪽 창 여백을 12픽셀로 설정하고 앱 창의 왼쪽 및 오른쪽 가장자리 간에 시각적 구분을 만듭니다.</li>
<li>접근성 향상을 위해 창의 맨 아래에 [앱 바](../controls-and-patterns/app-bars.md)를 도킹합니다.</li>
<li>한 번에 하나의 열/영역을 사용합니다.</li>
<li>아이콘을 사용하여 검색을 나타냅니다(검색 상자를 표시하지 않음).</li>
<li>[탐색 창](../controls-and-patterns/nav-pane.md)을 오버레이 모드로 전환하여 화면 공간 절약</li>
<li>[마스터 세부 정보 패턴](../controls-and-patterns/master-details.md)을 사용하는 경우 누적된 프레젠테이션 모드를 사용하여 화면 공간을 절약할 수 있습니다.</li>
</ul></td>
<td style="vertical-align:top;"><ul>
<li>탭 요소를 왼쪽에 맞춥니다.</li>
<li>왼쪽 및 오른쪽 창 여백을 24픽셀로 설정하고 앱 창의 왼쪽 및 오른쪽 가장자리 간에 시각적 구분을 만듭니다.</li>
<li>[앱 바](../controls-and-patterns/app-bars.md) 등의 명령 요소를 앱 창의 맨 위에 배치합니다.</li>
<li>최대 2개의 열/영역</li>
<li>검색 상자를 표시합니다.</li>
<li>좁은 아이콘 스트립이 항상 표시되도록 [탐색 창](../controls-and-patterns/nav-pane.md)을 작은 모드로 전환합니다.</li>
<li>[TV 환경](http://go.microsoft.com/fwlink/?LinkId=760736)에 맞는 추가 조정을 고려합니다.</li>
</ul></td>
<td style="vertical-align:top;"><ul>
<li>탭 요소를 왼쪽에 맞춥니다.</li>
<li>왼쪽 및 오른쪽 창 여백을 24픽셀로 설정하고 앱 창의 왼쪽 및 오른쪽 가장자리 간에 시각적 구분을 만듭니다.</li>
<li>[앱 바](../controls-and-patterns/app-bars.md) 등의 명령 요소를 앱 창의 맨 위에 배치합니다.</li>
<li>최대 3개의 열/영역</li>
<li>검색 상자를 표시합니다.</li>
<li>항상 표시되도록 [탐색 창](../controls-and-patterns/nav-pane.md)을 도킹 모드로 전환합니다.</li>
</ul></td>
</tr>
</tbody>
</table>

호환되는 Windows 10 Mobile 장치를 위한 새 환경인 [**휴대폰용 Continuum**](http://go.microsoft.com/fwlink/p/?LinkID=699431)을 사용하면 사용자가 휴대폰을 모니터, 마우스 및 키보드에 연결하여 노트북처럼 사용할 수 있습니다. 특정 중단점에 대해 디자인할 때 이 새로운 기능에 유의하세요. 휴대폰이 항상 작은 크기 클래스로 유지되지는 않습니다.
 

