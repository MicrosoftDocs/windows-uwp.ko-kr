---
Description: 체계적으로 잘 디자인된 동작은 앱에 생명을 불어넣고 환경을 전문적이고 돋보이게 합니다. 사용자가 컨텍스트 변경을 이해하고 시각적 전환에 맞게 작업할 수 있도록 합니다.
title: Windows 앱의 동작
ms.assetid: 21AA1335-765E-433A-85D8-560B340AE966
label: Motion
template: detail.hbs
ms.date: 05/19/2017
ms.topic: article
keywords: windows 10, uwp
pm-contact: stmoy
design-contact: jeffarn
doc-status: Published
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: 49fc31729bc8f195bacf1d743c570aa5293b33de
ms.sourcegitcommit: 0dee502484df798a0595ac1fe7fb7d0f5a982821
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 05/08/2020
ms.locfileid: "82970338"
---
# <a name="motion-for-windows-apps"></a>Windows 앱의 동작

![동작 아이콘](../images/motion-2x.png)

Fluent 움직임은 앱에서 목적에 맞게 작동합니다. 사용자의 동작을 기반으로 인텔리전트 피드백을 제공하고, UI를 살아있는 느낌이 들도록 유지하고, 사용자의 앱 탐색을 안내합니다. Fluent 움직임은 사용자와 디지털 환경 간의 정신적 연결을 끌어냅니다. 사용자가 실제 세계에서 이미 이해하고 있는 자연의 움직임을 기반으로 빌드하고, 거기에서 시스템을 확장합니다.

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

## <a name="fluent-motion-principles"></a>Fluent 움직임 원칙

### <a name="physical"></a>물리적

움직이는 개체는 실제 세계의 개체 동작을 보여줍니다. 유연하고 신속한 동작은 자연스러운 환경을 연출하여 감정적 연결을 만들어 내고 퍼스낼리티를 더합니다.

![물리적 움직임의 UI 예](images/Physical.gif)
> 터치를 통해 UI와 상호 작용할 때 UI의 움직임은 상호 작용의 속도와 직접적인 관련이 있습니다. 이 터치는 직접적인 조작이므로 상호 작용하는 개체는 주위의 개체에 영향을 미칩니다.

### <a name="functional"></a>기능적

움직임은 목적을 수행하며 그 안에는 신념이 담겨 있습니다. 움직임은 복잡성을 통해 사용자를 안내하고 계층 설정을 도와줍니다. 움직임은 성능이 향상된 느낌을 주고, 인지되는 대기 시간을 숨겨 사용자 환경을 최적화합니다.

![기능적 움직임의 UI 예](images/functional.gif)
> 페이지 전환은 특별한 목적을 위해 구현되었습니다. 페이지 전환은 페이지가 서로 어떻게 관련되었는지에 대한 힌트를 제공합니다. 성능이 최적화되지 않은 경우에도 빠르다고 인식되는 방식으로 움직입니다.

### <a name="continuous"></a>계속

지점 사이의 유연한 움직임은 자연스럽게 시선을 끌고 사용자를 안내합니다. 세련되게 사용자의 작업을 이어 붙여 더욱 사용하기 편하고 친근한 느낌을 주게 만듭니다.

![지속적인 움직임의 UI 예](images/continuous3.gif)
> 개체는 장면 사이를 이동하거나 장면 내에서 모핑되어 연속성을 제공하고 사용자 컨텍스트 유지를 도와줍니다.

### <a name="contextual"></a>상황에 맞는

인텔리전트한 움직임은 사용자의 UI 조작 방식에 맞는 방법으로 사용자에게 피드백을 제공합니다. 상호 작용은 사용자를 중심으로 이루어집니다. 움직임이 폼 팩터와 잘 어울리고 시나리오에 맞게 설계된 듯한 느낌을 줍니다. 움직임은 각 사용자에게 편안해야 합니다.

![상황에 맞는 움직임의 UI 예](images/Contextual.gif)
> 애니메이션은 사용자 상호 작용에 다시 연결해야 합니다. 상황에 맞는 메뉴는 사용자가 활성화한 지점에서 배포됩니다.

## <a name="motion-articles"></a>움직임 문서

:::row:::
    :::column:::
### <a name="timing-and-easing"></a>[타이밍 및 감속](timing-and-easing.md)
타이밍 및 감속은 UI 내에서 개체가 출입하거나 이동하는 움직임이 자연스럽게 느껴지도록 만드는 중요한 요소입니다.
    :::column-end:::
    :::column:::
### <a name="directionality-and-gravity"></a>[방향 및 무게](directionality-and-gravity.md)
방향 신호는 환경 전반에서 사용자에게 구체적인 멘탈 모델을 제공하는 데 도움이 됩니다. 방향 이동에는 이동의 자연스러운 느낌을 강화하는 중력 같은 힘이 강제로 적용됩니다.
    :::column-end:::
:::row-end:::
:::row:::
    :::column:::
### <a name="page-transitions"></a>[페이지 전환](page-transitions.md)
페이지 전환은 앱 페이지 사이에서 사용자를 탐색하면서 페이지 사이의 관계에 대한 피드백을 제공합니다. 사용자가 탐색 계층 구조상의 위치를 이해하는 데 도움이 됩니다.
    :::column-end:::
    :::column:::
### <a name="connected-animation"></a>[연결된 애니메이션](connected-animation.md)
연결된 애니메이션을 사용하면 두 가지 보기 간에 전환되는 동작에 애니메이션 효과를 적용하여 역동적이고 매력적인 탐색 환경을 만들 수 있습니다.
    :::column-end:::
:::row-end:::
