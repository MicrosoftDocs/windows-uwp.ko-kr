---
title: 방향성 및 중력-Windows 앱의 애니메이션
description: 예제를 확인 하 여 이동 방향, 탐색 방향 및 애니메이션 장면에서의 무게를 사용 하는 방법에 대해 알아봅니다.
label: Directionality and gravity
template: detail.hbs
ms.date: 09/24/2020
ms.topic: article
keywords: windows 10, uwp
pm-contact: stmoy
design-contact: jeffarn
doc-status: Draft
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: 87047e20d4513c9120c79bb329c008dad104a352
ms.sourcegitcommit: d786d084dafee5da0268ebb51cead1d8acb9b13e
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/09/2020
ms.locfileid: "91860084"
---
# <a name="directionality-and-gravity"></a>방향 및 무게

방향성 신호를 통해 사용자가 경험을 멘 탈 하는 경험의 모델을 구체화 하는 데 도움이 됩니다. 모든 동작의 방향은 공간의 연속성 뿐만 아니라 공간에 있는 개체의 무결성을 모두 지원 해야 합니다.

방향성 이동은 중력 처럼 강제로 적용 됩니다. 움직임에 강화를 적용 하면 동작의 자연 스러운 느낌을 받을 수 있습니다.

## <a name="examples"></a>예

<table>
<tr>
<td><img src="images/xaml-controls-gallery-app-icon.png" alt="XAML controls gallery" width="168"></img></td>
<td>
    <p><strong style="font-weight: semi-bold">XAML Controls Gallery</strong> 앱이 설치된 경우 여기를 클릭하여 <a href="xamlcontrolsgallery:/category/Motion">앱을 열고 작동 중인 모션을 확인</a>합니다.</p>
    <ul>
    <li><a href="https://www.microsoft.com/store/productId/9MSVH128X2ZT">XAML Controls Gallery 앱 가져오기(Microsoft Store)</a></li>
    <li><a href="https://github.com/Microsoft/Xaml-Controls-Gallery">소스 코드 가져오기(GitHub)</a></li>
    </ul>
</td>
</tr>
</table>

## <a name="direction-of-movement"></a>이동 방향

:::row:::
    :::column:::
이동 방향은 실제 동작에 해당 합니다. 본질적으로와 마찬가지로 개체는 모든 세계 축 (X, Y, Z)에서 이동할 수 있습니다. 이는 화면에서 개체의 움직임을 생각 하는 방법입니다.
개체를 이동 하는 경우에는 자연스럽 게 충돌이 발생 하지 않습니다. 개체가 들어오고 이동 하는 위치를 염두에 두고, 스크롤 방향 또는 레이아웃 계층 구조와 같이 장면에서 사용할 수 있는 더 높은 수준의 구문을 항상 지원 합니다.
    :::column-end:::
    :::column:::
        ![원을 보여 주는 짧은 비디오와 X 축, Y 축 및 Z 축이 추가 됩니다.](images/Direction.gif)
    :::column-end:::
:::row-end:::

## <a name="direction-of-navigation"></a>탐색 방향

앱에서 장면 간의 탐색 방향은 개념적입니다. 사용자가 앞으로 및 뒤로 탐색 합니다. 장면을 뷰에서 이동 합니다. 이러한 개념은 실제 이동과 결합 되어 사용자를 안내 합니다.

탐색을 통해 개체가 이전 장면에서 새 장면으로 이동 하면 개체가 화면에서 간단한 A-B 이동을 수행 합니다. 이동이 더 물리적인 지 확인 하기 위해 표준 감속 및 무게의 느낌이 추가 됩니다.

후방 탐색의 경우 이동이 반전 됩니다 (B-A). 사용자가 다시 탐색할 때 가능한 한 빨리 이전 상태로 돌아갈 것으로 예상 됩니다. 타이밍은 더 빠르고 직접적 이며 감속 감속/가속을 사용 합니다.

여기서 이러한 원칙은 앞으로 및 뒤로 탐색 하는 동안 선택한 항목이 화면에 유지 되는 경우 적용 됩니다.

![지속적인 움직임의 UI 예](images/continuous3.gif)

탐색이 화면에 있는 항목을 대체 하는 경우 해당 항목은 종료 장면이 전환 된 위치와 새 장면이 들어오는 위치를 표시 하는 데 중요 합니다.

여기에는 여러 가지 이점이 있습니다.

- 사용자의 공간에 대 한 사용자의 멘 탈 모델을 solidifies 합니다.
- 종료 되는 장면의 기간은 들어오는 장면에 대해 애니메이션을 적용할 수 있도록 준비 하는 데 더 많은 시간을 제공 합니다.
- 응용 프로그램의 인식 된 성능을 향상 시킵니다.

고려해 야 할 탐색의 네 가지 지침이 있습니다.

:::row:::
    :::column:::
**전달** 콘텐츠를 축 하 여 나가는 콘텐츠와 충돌 하지 않는 방식으로 장면을 입력 합니다. 콘텐츠를 장면으로 감속 합니다.
    :::column-end:::
    :::column:::
        ![방향 앞으로](images/forwardIN.gif)
    :::column-end:::
:::row-end:::
:::row:::
    :::column:::
**전달** 콘텐츠가 신속 하 게 종료 됩니다. 개체가 화면에서 가속 됩니다.
    :::column-end:::
    :::column:::
        ![방향 전달](images/forwardOUT.gif)
    :::column-end:::
:::row-end:::
:::row:::
    :::column:::
**이전 버전** 이후 버전과 동일 하지만 역방향입니다.
    :::column-end:::
    :::column:::
        ![프레임의 오른쪽에서 시작 하 여 프레임 중간에서 중지 하는 원을 보여 주는 짧은 비디오입니다.](images/backwardIN.gif)
    :::column-end:::
:::row-end:::
:::row:::
    :::column:::
**뒤로** 이동 정방향으로 진행 되는 것과 동일 합니다.
    :::column-end:::
    :::column:::
        ![방향 뒤로](images/backwardOUT.gif)
    :::column-end:::
:::row-end:::

## <a name="gravity"></a>중력

무게를 통해 환경을 보다 자연스럽 게 만들 수 있습니다. Z 축에서 이동 하 고 화면 affordance 장면에 고정 되지 않은 개체는 중력의 영향을 받을 가능성이 있습니다. 개체가 장면을 사용 하지 않으며 이스케이프 속도에 도달 하기 전에는 개체에서 구부리기를 설정 하 여 이동 하는 개체 궤적의 보다 자연 스러운 곡선을 만듭니다.

중력은 일반적으로 개체가 한 장면에서 다른 장면으로 이동 해야 하는 경우에 매니페스트 합니다. 따라서 연결 된 애니메이션은 중력 개념을 사용 합니다.

여기에서 표의 맨 위 행에 있는 요소는 중력의 영향을 받으며, 위치를 떠나 앞으로 이동할 때 약간 삭제 됩니다.

![모눈의 맨 위 행에 있는 사각형 요소를 보여 주는 짧은 비디오입니다.](images/continuity-photos.gif)

## <a name="related-articles"></a>관련된 문서

- [동작 개요](index.md)
- [타이밍 및 감속](timing-and-easing.md)
