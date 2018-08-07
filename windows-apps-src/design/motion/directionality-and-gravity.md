---
author: jwmsft
Description: Learn how Fluent motion uses directionality and gravity.
title: 방향 및 무게- UWP 앱의 애니메이션
label: Directionality and gravity
template: detail.hbs
ms.author: jimwalk
ms.date: 05/19/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
pm-contact: stmoy
design-contact: jeffarn
doc-status: Draft
ms.localizationpriority: medium
ms.openlocfilehash: a5216e81bc556a2e761e88b071e988bf6e4f457e
ms.sourcegitcommit: 517c83baffd344d4c705bc644d7c6d2b1a4c7e1a
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 05/07/2018
ms.locfileid: "1843892"
---
# <a name="directionality-and-gravity"></a>방향 및 무게

> [!IMPORTANT]
> 이 문서에서는 아직 출시되지 않아 상업적으로 출시하기 전에 크게 수정될 수 있는 기능에 대해 설명합니다. Microsoft는 여기에 제공된 정보에 대해 명시적 또는 묵시적 보증을 하지 않습니다.

방향 신호는 환경 전반에서 사용자의 멘탈 모델 여정을 구체화하는 데 도움이 됩니다. 모든 동작의 방향이 공간의 연속성 및 공간에서 개체의 무결성을 모두 지원하는지 여부는 중요합니다.

동작 방향은 무게 등의 힘의 영향을 받습니다. 동작에 이러한 힘을 적용하면 동작의 느낌이 보다 자연스러워집니다.

## <a name="direction-of-movement"></a>동작 방향

:::row::: :::column::: 동작 방향이 물리적 움직임에 해당합니다. 자연에서처럼 개체는 세계 축 X, Y, Z의 어디든지 이동할 수 있습니다. 다음은 화면에서 개체의 동작에 대한 생각입니다.

        When you move objects, avoid unnatural collisions. Keep in mind where objects come from and go to, and alway support higher level constructs that may be used in the scene, such as scroll direction or layout hierarchy.
    :::column-end:::
    :::column:::
        ![direction backward in](images/Direction.gif)
    :::column-end:::
:::row-end:::

## <a name="direction-of-navigation"></a>이동 방향

앱에서 장면 간에 이동 방향은 개념적입니다. 사용자는 앞과 및 뒤로 이동합니다. 장면은 보기의 안과 밖으로 이동합니다. 이러한 개념은 사용자를 안내하기 위해 물리적 동작과 결합되어 있습니다.

이동으로 인해 개체가 이전 장면에서 새 장면으로 이동하면, 개체는 화면에서 단순하게 A-B로 이동합니다. 동작이 더욱 실제적인 느낌을 주도록 표준 감속뿐만 아니라 무게의 느낌 또한 추가됩니다.

뒤로 이동의 경우 움직임이 반전됩니다(B-A). 사용자가 뒤로 이동하면 즉시 이전 상태로 되돌아가게 됩니다. 타이밍은 더 빠르고 더 직접적이며 감속을 사용합니다.

여기에서 앞으로 및 뒤로 이동하는 동안 선택한 항목이 화면에 유지될 때 이러한 원리가 적용됩니다.

![지속적인 움직임의 UI 예](images/continuous3.gif)

이동으로 인해 화면의 항목이 교체될 때, 기존 장면이 어디로 이동되었는지와 새 장면이 어디에서 왔는지 표시하는 것이 중요합니다.

여기에 몇 가지 이점이 있습니다.

- 공간에 대한 사용자의 멘탈 모델을 구체화합니다.
- 기존 장면의 기간은 들어오는 화면에서 애니메이션을 준비할 더 많은 시간을 제공합니다.
- 앱에 대한 인식된 성능이 향상됩니다.

고려할 이동 방향은 4가지가 있습니다.

:::row::: :::column::: **앞-안**

        Celebrate content entering the scene in a manner that does not collide with outgoing content. Content decelerates into the scene.
    :::column-end:::
    :::column:::
        ![direction forward in](images/forwardIN.gif)
    :::column-end:::
:::row-end::: :::row::: :::column::: **앞-밖**

        Content exits quickly. Objects accelerate off screen.
    :::column-end:::
    :::column:::
        ![direction forward out](images/forwardOUT.gif)
    :::column-end:::
:::row-end::: :::row::: :::column::: **뒤-안**

        Same as Forward-In, but reversed.
    :::column-end:::
    :::column:::
        ![direction backward in](images/backwardIN.gif)
    :::column-end:::
:::row-end::: :::row::: :::column::: **뒤-밖**

        Same as Forward-Out, but reversed.
    :::column-end:::
    :::column:::
        ![direction backward out](images/backwardOUT.gif)
    :::column-end:::
:::row-end:::

## <a name="gravity"></a>무게

무게는 사용자 환경의 모습을 더욱 자연스럽게 만듭니다. Z축에서 이동하고 화면 어포던스에 의해 화면에 고정하지 못한 개체는 무게에 영향을 받을 수도 있습니다. 개체가 장면에서 벗어나 이스케이프 속도에 도달하기 전에 무게는 개체를 끌어 내리며 개체가 이동할 때 보다 자연스러운 곡선을 그리게 만듭니다.

무게는 개체가 한 장면에서 다른 장면으로 이동해야 하는 경우 일반적으로 나타납니다. 이로 인해 연결된 애니메이션은 무게의 개념을 사용합니다.

여기에서 그리드의 맨 위 행에 있는 요소는 무게에 영향을 받으므로 장소를 떠나 앞으로 이동할 때 약간 아래로 낮아집니다.

![뒤로 방향](images/continuity-photos.gif)

## <a name="related-articles"></a>관련 문서

- [움직임 개요](index.md)
- [타이밍 및 감속](timing-and-easing.md)