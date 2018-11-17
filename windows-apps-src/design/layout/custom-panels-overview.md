---
author: muhsinking
Description: You can define custom panels for XAML layout by deriving a custom class from the Panel class.
MS-HAID: dev\_ctrl\_layout\_txt.xaml\_custom\_panels\_overview
MSHAttr: PreferredLib:/library/windows/apps
Search.Product: eADQiWindows 10XVcnh
title: XAML 사용자 지정 패널 개요
ms.assetid: 0CD395CD-E2AB-429D-BB49-56A71C5CC35D
label: XAML custom panels overview (Windows apps)
template: detail.hbs
op-migration-status: ready
ms.author: mukin
ms.date: 05/19/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: d9856d564ffd36226a841c38eba65df0b62ee306
ms.sourcegitcommit: 3257416aebb5a7b1515e107866806f8bd57845a8
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/16/2018
ms.locfileid: "7172504"
---
# <a name="xaml-custom-panels-overview"></a>XAML 사용자 지정 패널 개요

 

*패널*은 XAML(Extensible Application Markup Language) 레이아웃 시스템이 실행되고 앱 UI가 렌더링될 때 패널에 포함된 자식 요소에 대한 레이아웃 동작을 제공하는 개체입니다. 


> **중요 API**: [**Panel**](https://msdn.microsoft.com/library/windows/apps/br227511), [**ArrangeOverride**](https://msdn.microsoft.com/library/windows/apps/br208711), [**MeasureOverride**](https://msdn.microsoft.com/library/windows/apps/br208730)

[**Panel**](https://msdn.microsoft.com/library/windows/apps/br227511) 클래스에서 사용자 지정 클래스를 파생시켜 XAML 레이아웃에 대한 사용자 지정 패널을 정의할 수 있습니다. [**MeasureOverride**](https://msdn.microsoft.com/library/windows/apps/br208730) 및 [**ArrangeOverride**](https://msdn.microsoft.com/library/windows/apps/br208711)를 재정의하고 자식 요소를 측정 및 정렬하는 논리를 제공하여 패널에 대한 동작을 제공합니다.

## <a name="the-panel-base-class"></a>**Panel** 기본 클래스


사용자 지정 패널 클래스를 정의하려면 [**Panel**](https://msdn.microsoft.com/library/windows/apps/br227511) 클래스에서 직접 파생시키거나 봉인되지 않은 실용적인 패널 클래스(예: [**Grid**](https://msdn.microsoft.com/library/windows/apps/br242704) 또는 [**StackPanel**](https://msdn.microsoft.com/library/windows/apps/br209635)) 중 하나에서 파생시킬 수 있습니다. 이미 레이아웃 동작이 있는 패널의 기존 레이아웃 논리를 처리하는 것은 어려울 수 있으므로 **Panel**에서 파생시키기가 더 쉽습니다. 또한 동작을 포함하는 패널에는 패널의 레이아웃 기능과 관련이 없는 기존 속성이 있을 수도 있습니다.

사용자 지정 패널은 [**Panel**](https://msdn.microsoft.com/library/windows/apps/br227511)에서 다음과 같은 API를 상속합니다.

-   [**Children**](https://msdn.microsoft.com/library/windows/apps/br227514) 속성
-   [**Background**](https://msdn.microsoft.com/library/windows/apps/br227512), [**ChildrenTransitions**](https://msdn.microsoft.com/library/windows/apps/br227515) 및 [**IsItemsHost**](https://msdn.microsoft.com/library/windows/apps/br227517) 속성과 종속성 속성 식별자. 이러한 속성은 모두 가상이 아니므로 일반적으로 재정의하거나 바꾸지 않습니다. 일반적으로 사용자 지정 패널 시나리오에서는 값을 읽기 위한 경우에도 이러한 속성이 필요하지 않습니다.
-   레이아웃은 [**MeasureOverride**](https://msdn.microsoft.com/library/windows/apps/br208730) 및 [**ArrangeOverride**](https://msdn.microsoft.com/library/windows/apps/br208711) 메서드를 재정의합니다. 두 메서드는 원래 [**FrameworkElement**](https://msdn.microsoft.com/library/windows/apps/br208706)에서 정의되었습니다. 기본 [**Panel**](https://msdn.microsoft.com/library/windows/apps/br227511) 클래스는 이러한 메서드를 재정의하지 않지만, [**Grid**](https://msdn.microsoft.com/library/windows/apps/br242704) 등의 실용적인 패널에는 네이티브 코드로 구현되고 시스템에서 실행되는 재정의 구현이 있습니다. **ArrangeOverride** 및 **MeasureOverride**에 대한 새로운(또는 가산적) 구현을 제공하는 경우 사용자 지정 패널을 정의하는 데 많은 노력이 필요합니다.
-   [**FrameworkElement**](https://msdn.microsoft.com/library/windows/apps/br208706), [**UIElement**](https://msdn.microsoft.com/library/windows/apps/br208911) 및 [**DependencyObject**](https://msdn.microsoft.com/library/windows/apps/br242356)의 기타 모든 API(예: [**Height**](/uwp/api/Windows.UI.Xaml.FrameworkElement.Height), [**Visibility**](https://msdn.microsoft.com/library/windows/apps/br208992) 등). 때로는 레이아웃 재정의에서 이러한 속성 값을 참조하지만 가상이 아니므로 일반적으로 재정의하거나 바꾸지 않습니다.

레이아웃에서 가능하거나 필요한 사용자 지정 패널 동작의 모든 가능성을 고려할 수 있도록 여기서는 XAML 레이아웃 개념에 대해 중점적으로 설명합니다. 예제 사용자 지정 패널 구현을 바로 확인하려면 [BoxPanel, 예제 사용자 지정 패널](boxpanel-example-custom-panel.md)을 참조하세요.

## <a name="the-children-property"></a>**Children** 속성


[**Panel**](https://msdn.microsoft.com/library/windows/apps/br227514)에서 파생된 모든 클래스는 **Children** 속성을 사용하여 포함된 자식 요소를 컬렉션으로 저장하기 때문에 [**Children**](https://msdn.microsoft.com/library/windows/apps/br227511) 속성은 사용자 지정 패널과 관련이 있습니다. **Children**은 **Panel** 클래스에 대한 XAML 콘텐츠 속성으로 지정되며, **Panel**에서 파생된 모든 클래스는 XAML 콘텐츠 속성 동작을 상속할 수 있습니다. 속성이 XAML 콘텐츠 속성으로 지정된 경우 태그에 해당 속성을 지정할 때 XAML 태그에서 속성 요소를 생략할 수 있으며, 값이 태그 직계 자식("콘텐츠")으로 설정됩니다. 예를 들어 **Panel**에서 새 동작을 정의하지 않는 **CustomPanel** 클래스를 파생시키는 경우에도 다음 태그를 사용할 수 있습니다.

```XAML
<local:CustomPanel>
  <Button Name="button1"/>
  <Button Name="button2"/>
</local:CustomPanel>
```

XAML 파서가 이 태그를 읽을 때 [**Children**](https://msdn.microsoft.com/library/windows/apps/br227514)이 모든 [**Panel**](https://msdn.microsoft.com/library/windows/apps/br227511) 파생 형식에 대한 XAML 콘텐츠 속성으로 알려지므로 파서에서 두 개의 [**Button**](https://msdn.microsoft.com/library/windows/apps/br209265) 요소를 **Children** 속성의 [**UIElementCollection**](https://msdn.microsoft.com/library/windows/apps/br227633) 값에 추가합니다. XAML 콘텐츠 속성은 UI 정의에 대한 XAML 태그에서 부모-자식 관계를 간소화합니다. XAML 콘텐츠 속성 및 XAML을 구문 분석할 때 컬렉션 속성이 채워지는 방법에 대한 자세한 내용은 [XAML 구문 가이드](https://msdn.microsoft.com/library/windows/apps/mt185596)를 참조하세요.

[**Children**](https://msdn.microsoft.com/library/windows/apps/br227514) 속성 값을 유지 관리하는 컬렉션 형식은 [**UIElementCollection**](https://msdn.microsoft.com/library/windows/apps/br227633) 클래스입니다. **UIElementCollection**은 [**UIElement**](https://msdn.microsoft.com/library/windows/apps/br208911)를 강제 항목 종류로 사용하는 강력한 형식의 컬렉션입니다. **UIElement**는 수백 개의 실용적인 UI 요소 형식에 상속되는 기본 형식이므로 여기서는 의도적으로 느슨한 형식 적용을 사용합니다. 그러나 [**Brush**](/uwp/api/Windows.UI.Xaml.Media.Brush)를 [**Panel**](https://msdn.microsoft.com/library/windows/apps/br227511)의 직계 자식으로 사용할 수 없도록 강제하며, 일반적으로 UI에 표시되고 레이아웃에 사용되는 요소만 **Panel**에서 자식 요소로 발견됩니다.

일반적으로 사용자 지정 패널은 [**Children**](https://msdn.microsoft.com/library/windows/apps/br208911) 속성의 특성을 현재 상태대로 사용하여 XAML 정의에 의한 [**UIElement**](https://msdn.microsoft.com/library/windows/apps/br227514) 자식 요소를 모두 수락합니다. 고급 시나리오로, 레이아웃 재정의에서 컬렉션을 반복할 때 자식 요소의 추가 형식 검사를 지원할 수 있습니다.

재정의에서 [**Children**](https://msdn.microsoft.com/library/windows/apps/br227514) 컬렉션을 반복하는 것 외에도 패널 논리는 `Children.Count`의 영향을 받을 수 있습니다. 원하는 크기와 개별 항목의 기타 특성 대신 적어도 부분적으로 항목 수에 따라 공간을 할당하는 논리를 사용할 수도 있습니다.

## <a name="overriding-the-layout-methods"></a>레이아웃 재정의 메서드


레이아웃 재정의 메서드([**MeasureOverride**](https://msdn.microsoft.com/library/windows/apps/br208730) 및 [**ArrangeOverride**](https://msdn.microsoft.com/library/windows/apps/br208711))의 기본 모델은 모든 자식을 반복하고 각 자식 요소의 특정 레이아웃 메서드를 호출해야 한다는 것입니다. 첫 번째 레이아웃 주기는 XAML 레이아웃 시스템에서 루트 창에 대한 화면 효과를 설정할 때 시작됩니다. 각 부모가 자식에 대해 레이아웃을 호출하기 때문에 레이아웃 메서드 호출이 레이아웃에 포함된 가능한 모든 UI 요소에 전파됩니다. XAML 레이아웃에는 측정과 정렬의 두 단계가 있습니다.

기본 [**Panel**](https://msdn.microsoft.com/library/windows/apps/br208730) 클래스에서 [**MeasureOverride**](https://msdn.microsoft.com/library/windows/apps/br208711) 및 [**ArrangeOverride**](https://msdn.microsoft.com/library/windows/apps/br227511)에 대한 기본 제공 레이아웃 메서드 동작을 가져오지 않습니다. [**Children**](https://msdn.microsoft.com/library/windows/apps/br227514)의 항목은 XAML 시각적 트리의 일부로 자동으로 렌더링되지 않습니다. **MeasureOverride** 및 **ArrangeOverride** 구현 내에서 레이아웃 단계를 통해 **Children**에서 찾은 각 항목에 대해 레이아웃 메서드를 호출하여 사용자가 레이아웃 프로세스에 항목이 알려지도록 해야 합니다.

고유한 상속이 없는 경우 레이아웃 재정의에서 기본 구현을 호출할 이유는 없습니다. 호출 여부에 관계없이 레이아웃 동작(있는 경우)에 대한 네이티브 메서드가 실행되며, 재정의에서 기본 구현을 호출하지 않아도 기본 동작이 수행됩니다.

측정 단계에서 레이아웃 논리는 해당 자식 요소에 대해 [**Measure**](https://msdn.microsoft.com/library/windows/apps/br208952) 메서드를 호출하여 각 자식 요소에 원하는 크기를 쿼리합니다. **Measure** 메서드를 호출하면 [**DesiredSize**](https://msdn.microsoft.com/library/windows/apps/br208921) 속성 값이 설정됩니다. [**MeasureOverride**](https://msdn.microsoft.com/library/windows/apps/br208730) 반환 값은 패널 자체에 원하는 크기입니다.

정렬 단계에서 자식 요소의 위치와 크기가 x-y 공간으로 결정되며 렌더링을 위해 레이아웃 컴퍼지션이 준비됩니다. 레이아웃 시스템에서 요소가 레이아웃에 속하는 것을 감지하도록 코드에서 [**Children**](https://msdn.microsoft.com/library/windows/apps/br208914)의 각 자식 요소에 대해 [**Arrange**](https://msdn.microsoft.com/library/windows/apps/br227514)를 호출해야 합니다. **Arrange** 호출은 컴퍼지션 및 렌더링 전에 수행되며, 컴퍼지션이 렌더링을 위해 제출될 때 레이아웃 시스템에 해당 요소의 목적지를 알립니다.

많은 속성과 값이 레이아웃 논리의 런타임 작동 방식에 영향을 줍니다. 레이아웃 프로세스를 고려하는 한 가지 방법은 자식이 없는 요소(일반적으로 UI의 가장 안쪽에 중첩된 요소)가 먼저 측정을 완료할 수 있도록 하는 것입니다. 해당 요소에는 원하는 크기에 영향을 주는 자식 요소에 대한 종속성이 없습니다. 원하는 크기를 임의로 가질 수 있으며, 레이아웃이 실제로 수행될 때까지 크기 제안으로 사용됩니다. 그런 다음 루트 요소에 측정이 있고 모든 측정을 완료할 수 있을 때까지 측정 단계가 계속 시각적 트리 위로 진행됩니다.

후보 레이아웃이 현재 앱 창에 맞아야 하며, 그렇지 않으면 UI의 일부가 잘립니다. 패널에서 클리핑 논리가 결정되는 경우가 많습니다. 패널 논리에 따라 [**MeasureOverride**](https://msdn.microsoft.com/library/windows/apps/br208730) 구현 내에서 사용 가능한 크기를 결정할 수 있으며, 모든 요소가 잘 맞도록 크기 제한을 자식에 적용하고 자식 간에 공간을 나누어야 할 수도 있습니다. 이상적인 레이아웃 결과는 모든 레이아웃 요소의 다양한 속성을 사용하는 동시에 앱 창에 잘 맞는 레이아웃입니다. 이렇게 하려면 패널의 레이아웃 논리에 대한 적절한 구현과 해당 패널을 사용하여 UI를 빌드하는 앱 코드 부분에서 신중한 UI 디자인이 필요합니다. 전체 UI 디자인에 포함된 자식 요소 수가 앱에 들어갈 수 있는 개수보다 많으면 패널 디자인이 멋지게 표시되지 않습니다.

레이아웃 시스템 작동의 핵심 부분은 [**FrameworkElement**](https://msdn.microsoft.com/library/windows/apps/br208706)를 기반으로 하는 요소가 컨테이너에서 자식으로 작동할 경우 이미 내재된 동작이 있다는 것입니다. 예를 들어 레이아웃 동작을 알리거나 레이아웃 작동에 필요한 **FrameworkElement**의 여러 API가 있습니다. 다음이 포함됩니다.

-   [**DesiredSize**](https://msdn.microsoft.com/library/windows/apps/br208921)(실제로 [**UIElement**](https://msdn.microsoft.com/library/windows/apps/br208911) 속성)
-   [**ActualHeight**](https://msdn.microsoft.com/library/windows/apps/br208707) 및 [**ActualWidth**](https://msdn.microsoft.com/library/windows/apps/br208709)
-   [**Height**](/uwp/api/Windows.UI.Xaml.FrameworkElement.Height) 및 [**Width**](/uwp/api/Windows.UI.Xaml.FrameworkElement.Width)
-   [**여백**](https://msdn.microsoft.com/library/windows/apps/br208724)
-   [**LayoutUpdated**](https://msdn.microsoft.com/library/windows/apps/br208722) 이벤트
-   [**HorizontalAlignment**](https://msdn.microsoft.com/library/windows/apps/br208720) 및 [**VerticalAlignment**](https://msdn.microsoft.com/library/windows/apps/br208749)
-   [**ArrangeOverride**](https://msdn.microsoft.com/library/windows/apps/br208711) 및 [**MeasureOverride**](https://msdn.microsoft.com/library/windows/apps/br208730) 메서드
-   [**Arrange**](https://msdn.microsoft.com/library/windows/apps/br208914) 및 [**Measure**](https://msdn.microsoft.com/library/windows/apps/br208952) 메서드: [**FrameworkElement**](https://msdn.microsoft.com/library/windows/apps/br208706) 수준에서 요소 수준 레이아웃 작업을 처리하는 기본 구현이 정의되어 있습니다.

## **<a name="measureoverride"></a>MeasureOverride**


레이아웃에서 부모가 패널에 대해 [**Measure**](https://msdn.microsoft.com/library/windows/apps/br208730) 메서드를 호출할 때 [**MeasureOverride**](https://msdn.microsoft.com/library/windows/apps/br208921) 메서드의 반환 값은 레이아웃 시스템에서 패널 자체의 시작 [**DesiredSize**](https://msdn.microsoft.com/library/windows/apps/br208952)로 사용됩니다. 메서드 내의 논리 선택은 반환 값만큼 중요하며, 논리가 반환 값에 영향을 주는 경우가 많습니다.

모든 [**MeasureOverride**](https://msdn.microsoft.com/library/windows/apps/br208730) 구현에서 [**Children**](https://msdn.microsoft.com/library/windows/apps/br227514)을 반복하고 각 자식 요소에 대해 [**Measure**](https://msdn.microsoft.com/library/windows/apps/br208952) 메서드를 호출해야 합니다. **Measure** 메서드를 호출하면 [**DesiredSize**](https://msdn.microsoft.com/library/windows/apps/br208921) 속성 값이 설정됩니다. 이 값은 패널 자체에 필요한 공간 크기뿐 아니라 요소 간에 공간을 나누는 방법이나 특정 자식 요소에 대해 공간 크기를 지정하는 방법을 알려줄 수 있습니다.

다음은 [**MeasureOverride**](https://msdn.microsoft.com/library/windows/apps/br208730) 메서드의 기본 골격입니다.

```CSharp
protected override Size MeasureOverride(Size availableSize)
{
    Size returnSize; //TODO might return availableSize, might do something else
     
    //loop through each Child, call Measure on each
    foreach (UIElement child in Children)
    {
        child.Measure(new Size()); // TODO determine how much space the panel allots for this child, that's what you pass to Measure
        Size childDesiredSize = child.DesiredSize; //TODO determine how the returned Size is influenced by each child's DesiredSize
        //TODO, logic if passed-in Size and net DesiredSize are different, does that matter?
    }
    return returnSize;
}
```

레이아웃에 사용할 준비가 될 때쯤에는 대체로 요소에 기본 크기가 있습니다. 측정 단계 후 [**Measure**](https://msdn.microsoft.com/library/windows/apps/br208921)에 대해 전달한 *availableSize*가 더 작은 경우 [**DesiredSize**](https://msdn.microsoft.com/library/windows/apps/br208952)가 기본 크기를 나타낼 수도 있습니다. 기본 크기가 **Measure**에 대해 전달한 *availableSize*보다 크면 **DesiredSize**가 *availableSize*로 제약됩니다. **Measure**의 내부 구현은 이런 방식으로 동작하며, 레이아웃 재정의에서 이 동작을 고려해야 합니다.

일부 요소는 [**Height**](/uwp/api/Windows.UI.Xaml.FrameworkElement.Height) 및 [**Width**](/uwp/api/Windows.UI.Xaml.FrameworkElement.Width) 값이 **Auto**이므로 기본 크기가 없습니다. 이러한 요소는 **Auto** 값이 나타내는 대로 전체 *availableSize*를 사용합니다. *availableSize*로 [**Measure**](https://msdn.microsoft.com/library/windows/apps/br208952)를 호출하여 요소 크기를 레이아웃 직계 부모가 전달하는 사용 가능한 최대 크기로 조정합니다. 실제로 최상위 창인 경우에도 항상 UI 크기가 조정되는 몇 가지 측정이 있습니다. 궁극적으로 측정 단계에서는 모든 **Auto** 값을 부모 제약 조건으로 확인하며, 모든 **Auto** 값 요소에 실제 측정이 있습니다. 레이아웃이 완료된 후 [**ActualWidth**](https://msdn.microsoft.com/library/windows/apps/br208709) 및 [**ActualHeight**](https://msdn.microsoft.com/library/windows/apps/br208707)를 검사하여 측정을 가져올 수 있습니다.

무한 차원이 하나 이상 있는 [**Measure**](https://msdn.microsoft.com/library/windows/apps/br208952)에 크기를 전달하여 패널이 포함된 콘텐츠의 측정값에 맞게 크기를 조정할 수 있음을 나타내는 것이 좋습니다. 측정되고 있는 각 자식 요소는 기본 크기를 사용하여 [**DesiredSize**](https://msdn.microsoft.com/library/windows/apps/br208921) 값을 설정합니다. 그런 다음 일반적으로 정렬 단계에서 패널은 해당 크기를 사용하여 정렬됩니다.

[**TextBlock**](https://msdn.microsoft.com/library/windows/apps/br209652) 등의 텍스트 요소에는 [**Height**](https://msdn.microsoft.com/library/windows/apps/br208709) 또는 [**Width**](https://msdn.microsoft.com/library/windows/apps/br208707) 값이 설정되지 않은 경우에도 텍스트 문자열과 텍스트 속성에 따라 계산된 [**ActualWidth**](/uwp/api/Windows.UI.Xaml.FrameworkElement.Height) 및 [**ActualHeight**](/uwp/api/Windows.UI.Xaml.FrameworkElement.Width)가 있으며, 패널 논리에 이러한 차원이 반영되어야 합니다. 텍스트 클리핑은 특히 잘못된 UI 환경입니다.

구현에서 원하는 크기 측정을 사용하지 않는 경우에도 각 자식 요소에 대해 [**Measure**](https://msdn.microsoft.com/library/windows/apps/br208952) 메서드를 호출하는 것이 가장 좋습니다. 호출되는 **Measure**에서 트리거하는 내부 및 기본 동작이 있기 때문입니다. 요소가 레이아웃에 사용되려면 측정 단계에서 각 자식 요소에 대해 **Measure**가 호출되고 정렬 단계에서 [**Arrange**](https://msdn.microsoft.com/library/windows/apps/br208914) 메서드가 호출되어야 합니다. 이러한 메서드를 호출하면 개체에 내부 플래그가 설정되며 시스템 레이아웃 논리에서 시각적 트리를 빌드하고 UI를 렌더링할 때 필요한 값(예: [**DesiredSize**](https://msdn.microsoft.com/library/windows/apps/br208921) 속성)이 채워집니다.

[**MeasureOverride**](https://msdn.microsoft.com/library/windows/apps/br208730) 반환 값은 [**Measure**](https://msdn.microsoft.com/library/windows/apps/br208921)가 호출될 때 [**Children**](https://msdn.microsoft.com/library/windows/apps/br227514)의 각 자식 요소에 대한 [**DesiredSize**](https://msdn.microsoft.com/library/windows/apps/br208952) 또는 기타 크기 고려 사항을 해석하는 패널 논리를 기반으로 합니다. 자식의 **DesiredSize** 값을 처리하는 방법 및 **MeasureOverride** 반환 값에서 해당 값을 사용하는 방법은 고유한 논리의 해석에 따라 달라집니다. **MeasureOverride**의 입력은 패널의 부모가 제안한 고정된 사용 가능 크기인 경우가 많기 때문에 일반적으로 수정 없이 값을 더하지 않습니다. 해당 크기를 초과하면 패널 자체가 잘릴 수 있습니다. 일반적으로 자식의 총 크기를 패널의 사용 가능한 크기와 비교하고 필요에 따라 조정합니다.

### <a name="tips-and-guidance"></a>팁과 지침

-   이상적인 사용자 지정 패널은 [**Page**](https://msdn.microsoft.com/library/windows/apps/br227503), [**UserControl**](https://msdn.microsoft.com/library/windows/apps/br227647) 또는 XAML 페이지 루트인 다른 요소의 바로 아래 수준에서 UI 컴퍼지션의 첫 번째 실제 화면 효과가 되기에 적합해야 합니다. [**MeasureOverride**](https://msdn.microsoft.com/library/windows/apps/br208730) 구현에서 값을 검사하지 않고 입력 [**Size**](https://msdn.microsoft.com/library/windows/apps/br225995)를 자주 반환하지 마세요. 반환 **Size**에 **Infinity** 값이 있는 경우 이로 인해 런타임 레이아웃 논리에서 예외가 발생할 수 있습니다. **Infinity** 값은 스크롤 가능한 메인 앱 창에서 가져올 수 있으므로 최대 높이가 없습니다. 스크롤 가능한 다른 콘텐츠에도 같은 동작이 있을 수 있습니다.
-   [**MeasureOverride**](https://msdn.microsoft.com/library/windows/apps/br208730) 구현의 다른 일반적인 실수는 새 기본 [**Size**](https://msdn.microsoft.com/library/windows/apps/br225995)를 반환하는 것입니다(높이 및 너비 값이 0임). 해당 값으로 시작할 수 있으며, 패널에서 자식이 렌더링되지 않도록 결정하는 경우 올바른 값일 수도 있습니다. 그러나 기본 **Size**를 사용하면 호스트에서 패널 크기를 올바르게 조정하지 않습니다. UI 공간을 요청하지 않으므로 공간을 얻지 못하며 렌더링하지 않습니다. 또는 모든 패널 코드가 제대로 작동하지만 높이 0과 너비 0으로 작성되는 경우 패널이나 패널의 콘텐츠가 표시되지 않습니다.
-   재정의 내에서 자식 요소를 [**FrameworkElement**](https://msdn.microsoft.com/library/windows/apps/br208706)로 캐스팅하고 레이아웃 결과로 계산된 속성, 특히 [**ActualWidth**](https://msdn.microsoft.com/library/windows/apps/br208709) 및 [**ActualHeight**](https://msdn.microsoft.com/library/windows/apps/br208707)를 사용하려고 시도하지 마세요. 대부분의 일반적인 시나리오에서는 자식의 [**DesiredSize**](https://msdn.microsoft.com/library/windows/apps/br208921) 값을 기준으로 논리를 작성할 수 있으며 자식 요소의 [**Height**](/uwp/api/Windows.UI.Xaml.FrameworkElement.Height) 또는 [**Width**](/uwp/api/Windows.UI.Xaml.FrameworkElement.Width) 관련 속성이 필요하지 않습니다. 요소 형식을 알고 있고 이미지 파일 기본 크기 등의 추가 정보가 있는 특수한 경우에서는 레이아웃 시스템에서 자주 변경되는 값이 아니므로 요소의 특수한 정보를 사용해도 됩니다. 레이아웃에서 계산된 속성을 레이아웃 논리의 일부로 포함하면 의도하지 않은 레이아웃 루프가 정의될 위험이 훨씬 증가합니다. 이러한 루프는 유효한 레이아웃을 만들 수 없는 상태를 발생시키며, 루프를 복구할 수 없는 경우 시스템에서 [**LayoutCycleException**](https://msdn.microsoft.com/library/windows/apps/hh673799)이 발생할 수 있습니다.
-   일반적으로 패널은 정확히 공간을 나누는 방법은 다르지만 사용 가능한 공간을 여러 자식 요소 간에 나눕니다. 예를 들어 [**Grid**](https://msdn.microsoft.com/library/windows/apps/br242704)는 해당 [**RowDefinition**](https://msdn.microsoft.com/library/windows/apps/br227606) 및 [**ColumnDefinition**](https://msdn.microsoft.com/library/windows/apps/br209324) 값을 사용하여 공간을 **Grid** 셀로 나누고 배율 크기 조정과 픽셀 값을 모두 지원하는 레이아웃 논리를 구현합니다. 픽셀 값인 경우 각 하위에 사용 가능한 크기가 이미 알려져 있으므로 해당 값이 그리드 스타일 [**Measure**](https://msdn.microsoft.com/library/windows/apps/br208952)에 대한 입력 값으로 전달됩니다.
-   패널 자체에서 항목 사이의 안쪽 여백으로 예약된 공간을 적용할 수 있습니다. 이 경우 [**Margin**](https://msdn.microsoft.com/library/windows/apps/br208724) 또는 **Padding** 속성과 다른 속성으로 측정을 노출해야 합니다.
-   이전 레이아웃 단계를 기준으로 요소의 [**ActualWidth**](https://msdn.microsoft.com/library/windows/apps/br208709) 및 [**ActualHeight**](https://msdn.microsoft.com/library/windows/apps/br208707) 속성 값이 지정될 수 있습니다. 값이 변경되면 앱 UI 코드에서 실행할 특수 논리가 있는 경우 요소에 [**LayoutUpdated**](https://msdn.microsoft.com/library/windows/apps/br208722)의 처리기를 배치할 수 있지만, 일반적으로 패널 논리에서 이벤트 처리를 사용하여 변경 사항을 확인할 필요는 없습니다. 레이아웃 관련 속성 값이 변경되었으며 해당 상황에서 패널의 [**MeasureOverride**](https://msdn.microsoft.com/library/windows/apps/br208730) 또는 [**ArrangeOverride**](https://msdn.microsoft.com/library/windows/apps/br208711)가 자동으로 호출되기 때문에 레이아웃 시스템에서 레이아웃을 다시 실행할 시기를 이미 결정합니다.

## **<a name="arrangeoverride"></a>ArrangeOverride**


레이아웃에서 부모가 패널에 대해 [**Arrange**](https://msdn.microsoft.com/library/windows/apps/br208711) 메서드를 호출할 때 [**ArrangeOverride**](https://msdn.microsoft.com/library/windows/apps/br225995) 메서드의 [**Size**](https://msdn.microsoft.com/library/windows/apps/br208914) 반환 값은 레이아웃 시스템에서 패널 자체의 렌더링 시 사용됩니다. 일반적으로 입력 *finalSize*와 **Size**에서 반환된 **ArrangeOverride**는 같습니다. 다른 경우 패널에서 레이아웃의 다른 요소가 사용 가능하다고 주장하는 크기와 다른 크기를 설정하려고 하는 것입니다. 최종 크기는 패널 코드를 통해 이전에 실행한 레이아웃 측정 단계를 기반으로 하며, 이런 이유 때문에 다른 크기를 반환하는 것은 일반적이지 않습니다. 의도적으로 측정 논리를 무시하고 있는 것입니다.

[**Size**](https://msdn.microsoft.com/library/windows/apps/br225995)를 **Infinity** 구성 요소와 함께 반환하지 않습니다. **Size**를 사용하면 내부 레이아웃에서 예외가 발생합니다.

모든 [**ArrangeOverride**](https://msdn.microsoft.com/library/windows/apps/br208711) 구현에서 [**Children**](https://msdn.microsoft.com/library/windows/apps/br227514)을 반복하고 각 자식 요소에 대해 [**Arrange**](https://msdn.microsoft.com/library/windows/apps/br208914) 메서드를 호출해야 합니다. [**Measure**](https://msdn.microsoft.com/library/windows/apps/br208952)와 마찬가지로 **Arrange**에는 반환 값이 없습니다. 그러나 **Measure**와 달리 계산된 속성이 결과로 설정되지 않습니다. 하지만 해당 요소에서 [**LayoutUpdated**](https://msdn.microsoft.com/library/windows/apps/br208722) 이벤트가 발생합니다.

다음은 [**ArrangeOverride**](https://msdn.microsoft.com/library/windows/apps/br208711) 메서드의 기본 골격입니다.

```CSharp
protected override Size ArrangeOverride(Size finalSize)
{
    //loop through each Child, call Arrange on each
    foreach (UIElement child in Children)
    {
        Point anchorPoint = new Point(); //TODO more logic for topleft corner placement in your panel
       // for this child, and based on finalSize or other internal state of your panel
        child.Arrange(new Rect(anchorPoint, child.DesiredSize)); //OR, set a different Size 
    }
    return finalSize; //OR, return a different Size, but that's rare
}
```

측정 단계가 먼저 수행되지 않고 레이아웃 정렬 단계가 발생할 수도 있습니다. 그러나 이는 레이아웃 시스템에서 이전 측정에 영향을 준 속성이 변경되지 않았음을 확인한 경우에만 발생합니다. 예를 들어 맞춤이 변경되는 경우 맞춤 선택 변경 시 해당 [**DesiredSize**](https://msdn.microsoft.com/library/windows/apps/br208921)가 변경되지 않으므로 특정 요소를 다시 측정할 필요가 없습니다. 반면, 레이아웃의 요소에서 [**ActualHeight**](https://msdn.microsoft.com/library/windows/apps/br208707)가 변경되는 경우 새 측정 단계가 필요합니다. 레이아웃 시스템에서 진정한 측정 변경 사항을 자동으로 검색하고 측정 단계를 다시 호출한 다음 다른 정렬 단계를 실행합니다.

[**Arrange**](https://msdn.microsoft.com/library/windows/apps/br208914)에 대한 입력은 [**Rect**](https://msdn.microsoft.com/library/windows/apps/br225994) 값을 사용합니다. 이 **Rect**를 생성하는 가장 일반적인 방법은 [**Point**](https://msdn.microsoft.com/library/windows/apps/br225870) 입력과 [**Size**](https://msdn.microsoft.com/library/windows/apps/br225995) 입력이 포함된 생성자를 사용하는 것입니다. **Point**는 요소에 대한 경계 상자의 왼쪽 위 모서리를 배치할 지점입니다. **Size**는 특정 요소를 렌더링하는 데 사용되는 차원입니다. 레이아웃에 포함된 모든 요소에 대해 **DesiredSize**를 설정하는 것이 레이아웃 측정 단계의 목적이기 때문에 대체로 해당 요소의 [**DesiredSize**](https://msdn.microsoft.com/library/windows/apps/br208921)를 이 **Size** 값으로 사용합니다. 레이아웃 시스템이 정렬 단계에 도달한 후 요소의 배치 방법을 최적화할 수 있도록 측정 단계는 반복적인 방식으로 요소의 전체 크기 조정을 결정합니다.

일반적으로 [**ArrangeOverride**](https://msdn.microsoft.com/library/windows/apps/br208711) 구현 간의 차이점은 패널에서 각 자식을 정렬하는 방법의 [**Point**](https://msdn.microsoft.com/library/windows/apps/br225870)를 결정하는 논리입니다. [**Canvas**](https://msdn.microsoft.com/library/windows/apps/br209267) 등의 절대 위치 패널은 [**Canvas.Left**](https://msdn.microsoft.com/library/windows/apps/hh759771) 및 [**Canvas.Top**](https://msdn.microsoft.com/library/windows/apps/hh759772) 값을 통해 각 요소에서 가져오는 명시적 배치 정보를 사용합니다. [**Grid**](https://msdn.microsoft.com/library/windows/apps/br242704) 등의 공간 분할 패널에는 사용 가능한 공간을 셀로 나눈 수학적 연산이 있으며, 각 셀에 콘텐츠가 배치 및 정렬되어야 하는 x-y 값이 있습니다. [**StackPanel**](https://msdn.microsoft.com/library/windows/apps/br209635) 등의 적응 패널은 방향 차원에서 콘텐츠에 맞게 확장될 수도 있습니다.

직접 제어하고 [**Arrange**](https://msdn.microsoft.com/library/windows/apps/br208914)에 전달하는 값 외에 레이아웃의 요소 위치에 대한 추가적인 영향도 있습니다. 이러한 영향은 모든 [**FrameworkElement**](https://msdn.microsoft.com/library/windows/apps/br208706) 파생 형식에 공통적으로 적용되고 텍스트 요소 등의 다른 형식에 의해 확장되는 **Arrange**의 내부 기본 구현에서 비롯됩니다. 예를 들어 요소에 여백과 맞춤이 있을 수 있으며, 안쪽 여백이 지정된 요소도 있습니다. 대체로 이러한 속성은 상호 작용합니다. 자세한 내용은 [맞춤, 여백 및 안쪽 여백](alignment-margin-padding.md)을 참조하세요.

## <a name="panels-and-controls"></a>패널 및 컨트롤


사용자 지정 컨트롤로 빌드해야 하는 기능을 사용자 지정 패널에 배치하지 마세요. 패널의 역할은 패널 내에 있는 모든 자식 요소 콘텐츠를 자동으로 발생하는 레이아웃의 기능으로 표시하는 것입니다. [**Border**](https://msdn.microsoft.com/library/windows/apps/br209250)에서 표시하는 요소 주위에 테두리를 추가하는 방법과 마찬가지로 패널에서 콘텐츠에 데코레이션을 추가하거나 안쪽 여백 등의 다른 레이아웃 관련 조정을 수행할 수도 있습니다. 그러나 자식의 정보 보고 및 사용 이외에 시각적 트리 출력을 확장할 때는 이것이 한계입니다.

사용자가 액세스할 수 있는 상호 작용이 있는 경우 패널이 아니라 사용자 지정 컨트롤을 작성해야 합니다. 예를 들어 스크롤 막대, 미리 보기 등은 대화형 컨트롤 요소이므로 클리핑을 방지하기 위한 경우에도 패널에서 표시하는 콘텐츠에 스크롤 뷰포트를 추가하면 안 됩니다. (콘텐츠에 결국 스크롤 막대가 포함될 수도 있지만 자식의 논리에 따라 결정되게 해야 합니다. 스크롤을 레이아웃 작업으로 추가하여 강제로 적용하지 마세요.) 컨트롤을 만들고, 해당 컨트롤에 콘텐츠를 표시하는 경우 컨트롤의 시각적 트리에서 중요한 역할을 하는 사용자 지정 패널을 작성할 수도 있습니다. 그러나 컨트롤과 패널은 별개의 코드 개체여야 합니다.

컨트롤과 패널의 구분이 중요한 이유는 Microsoft UI 자동화 및 접근성 때문입니다. 패널은 논리적 동작이 아니라 시각적 레이아웃 동작을 제공합니다. UI 요소를 어떻게 시각적으로 표시하는지는 일반적으로 접근성 시나리오에 중요한 UI의 측면이 아닙니다. 접근성은 논리적으로 UI 인식에 중요한 앱 구성 요소를 노출하는 것입니다. 상호 작용이 필요한 경우 컨트롤이 UI 자동화 인프라에 상호 작용 기능을 노출해야 합니다. 자세한 내용은 [사용자 지정 자동화 피어](https://msdn.microsoft.com/library/windows/apps/mt297667)를 참조하세요.

## <a name="other-layout-api"></a>기타 레이아웃 API


레이아웃 시스템에 포함되지만 [**Panel**](https://msdn.microsoft.com/library/windows/apps/br227511)에서 선언되지 않는 다른 몇 가지 API도 있습니다. 이러한 API는 패널 구현이나 패널을 사용하는 사용자 지정 컨트롤에서 사용될 수 있습니다.

-   [**UpdateLayout**](https://msdn.microsoft.com/library/windows/apps/br208989), [**InvalidateMeasure**](https://msdn.microsoft.com/library/windows/apps/br208930) 및 [**InvalidateArrange**](https://msdn.microsoft.com/library/windows/apps/br208929)는 레이아웃 단계를 시작하는 메서드입니다. **InvalidateArrange**는 측정 단계를 트리거하지 않을 수도 있지만 다른 두 메서드는 트리거합니다. 거의 항상 레이아웃 루프가 발생하므로 레이아웃 메서드 재정의 내에서 이러한 메서드를 호출하지 마세요. 일반적으로 제어 코드에서 해당 메서드를 호출할 필요도 없습니다. 대부분의 레이아웃 측면은 프레임워크에서 정의된 [**Width**](/uwp/api/Windows.UI.Xaml.FrameworkElement.Width) 등의 레이아웃 속성에 대한 변경 사항을 검색하여 자동으로 트리거됩니다.
-   [**LayoutUpdated**](https://msdn.microsoft.com/library/windows/apps/br208722)는 요소 레이아웃의 일부 측면이 변경된 경우에 발생하는 이벤트입니다. 패널과 관련이 없으며 [**FrameworkElement**](https://msdn.microsoft.com/library/windows/apps/br208706)에서 이벤트를 정의합니다.
-   [**SizeChanged**](https://msdn.microsoft.com/library/windows/apps/br208742)는 레이아웃 단계가 완료된 후에만 발생하는 이벤트이며, 그 결과로 [**ActualHeight**](https://msdn.microsoft.com/library/windows/apps/br208707) 또는 [**ActualWidth**](https://msdn.microsoft.com/library/windows/apps/br208709)가 변경되었음을 나타냅니다. 이것은 다른 [**FrameworkElement**](https://msdn.microsoft.com/library/windows/apps/br208706) 이벤트입니다. [**LayoutUpdated**](https://msdn.microsoft.com/library/windows/apps/br208722)가 발생하지만 **SizeChanged**가 발생하지 않는 경우가 있습니다. 예를 들어 내부 콘텐츠는 다시 정렬되지만 요소 크기는 변경되지 않습니다.


## <a name="related-topics"></a>관련 항목

**참조**
* [**FrameworkElement.ArrangeOverride**](https://msdn.microsoft.com/library/windows/apps/br208711)
* [**FrameworkElement.MeasureOverride**](https://msdn.microsoft.com/library/windows/apps/br208730)
* [**패널**](https://msdn.microsoft.com/library/windows/apps/br227511)

**개념**
* [맞춤, 여백 및 안쪽 여백](alignment-margin-padding.md)
