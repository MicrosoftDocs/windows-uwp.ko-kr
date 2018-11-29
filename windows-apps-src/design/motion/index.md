---
Description: Purposeful, well-designed motion brings your app to life and makes the experience feel crafted and polished. Help users understand context changes, and tie experiences together with visual transitions.
title: UWP 앱의 동작 및 애니메이션
ms.assetid: 21AA1335-765E-433A-85D8-560B340AE966
label: Motion
template: detail.hbs
ms.date: 05/19/2017
ms.topic: article
keywords: Windows 10, uwp
pm-contact: stmoy
design-contact: jeffarn
doc-status: Published
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: 2701844ccefdc5a535fa8fc20086c550cb7bc29e
ms.sourcegitcommit: b5c9c18e70625ab770946b8243f3465ee1013184
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/29/2018
ms.locfileid: "7992095"
---
# <a name="motion-for-uwp-apps"></a>UWP 앱의 동작

![영웅 이미지](images/header-motion2.svg)

Fluent 움직임은 앱에서 목적에 맞게 작동합니다. 사용자의 동작을 기반으로 지능형 피드백을 제공하고 UI를 살아있는 느낌이 들도록 유지하고, 앱에 대한 사용자의 탐색을 안내합니다. Fluent 움직임은 사용자와 디지털 환경 간에 정신적 연결을 끌어냅니다. 사용자가 실제 세계에서 이미 이해하는 자연적임 움직임을 기반으로 빌드하고 거기에서 시스템을 확장합니다.

## <a name="fluent-motion-principles"></a>Fluent 움직임 원칙

### <a name="physical"></a>물리적

움직이는 개체는 실제 세계에서 개체의 동작을 나타냅니다. 유연하고 신속한 동작은 환경에 자연스러운 느낌을 주어 감정적인 연결을 만들어내고 퍼스낼리티를 더합니다.

![물리적 움직임의 UI 예](images/Physical.gif)
> 터치를 통해 UI와 상호 작용할 때 UI의 움직임은 상호 작용의 속도와 직접적인 관련이 있습니다. 이 터치는 직접적인 조작이므로 상호 작용하는 개체는 주위의 개체에 영향을 미칩니다.

### <a name="functional"></a>기능적

움직임은 목적에 대해 수행되며 확신을 가집니다. 복잡성을 통해 사용자를 안내하고 계층 설정을 돕습니다. 움직임은 향상된 성능의 느낌을 주고 인지된 지연을 숨겨 사용자 환경을 최적화합니다.

![기능적 움직임의 UI 예](images/functional.gif)
> 페이지 전환은 목적을 위해 구현되었습니다. 페이지가 서로 어떻게 관련되었는지에 대한 힌트를 제공합니다. 성능이 최적이 아닌 경우에도 빠르게 인식되는 방식으로 이동합니다.

### <a name="continuous"></a>지속적

지점 사이의 유연한 움직임은 자연스럽게 시선을 이끌고 사용자를 안내합니다. 세련되게 사용자의 작업을 이어 붙여 더욱 소모적이며 친근한 느낌을 줍니다.

![지속적인 움직임의 UI 예](images/continuous3.gif)
> 개체는 장면 사이를 이동하거나 장면 내에서 모핑되어 연속성을 제공하고 사용자 컨텍스트 유지를 돕습니다.

### <a name="contextual"></a>상황에 맞는

지능형 움직임은 UI을 조작하는 방법에 맞게 조정되어 사용자에게 피드백을 제공합니다. 상호 작용은 사용자를 중심으로 이루어집니다. 움직임은 폼 팩터에 적절한 느낌을 주며 시나리오에 맞게 설계됩니다. 각 사용자에게 편안해야 합니다.

![상황에 맞는 움직임의 UI 예](images/Contextual.gif)
> 애니메이션은 사용자 상호 작용에 다시 연결해야 합니다. 상황에 맞는 메뉴는 사용자가 활성화한 지점에서 배포됩니다. 

## <a name="motion-articles"></a>움직임 문서

:::row:::
    :::column:::
        ### [Timing and easing](timing-and-easing.md)
        Timing and easing are important elements that make motion feel natural for objects entering, exiting, or moving within the UI.
    :::column-end:::
    :::column:::
        ### [Directionality and gravity](directionality-and-gravity.md)
        Directional signals help provide a solid mental model of the journey a user takes across experiences. Directional movement is subject to forces like gravity, which reinforces the natural feel of the movement.
    :::column-end:::
:::row-end:::
:::row:::
    :::column:::
        ### [Page transitions](page-transitions.md)
        Page transitions navigate users between pages in an app, providing feedback about the relationship between pages. They help users understand where they are in the navigation hierarchy.
    :::column-end:::
    :::column:::
        ### [Connected animation](connected-animation.md)
        Connected animations let you create a dynamic and compelling navigation experience by animating the transition of an element between two different views.
    :::column-end:::
:::row-end:::
