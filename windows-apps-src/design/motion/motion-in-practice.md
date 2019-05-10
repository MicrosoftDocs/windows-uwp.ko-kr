---
Description: 기본 앱에서 함께 일 하는 방법을 Fluent 동작에 알아봅니다.
title: 동작 중인 움직임 - UWP 앱의 애니메이션
label: Motion in practice
template: detail.hbs
ms.date: 10/02/2018
ms.topic: article
keywords: windows 10, uwp
pm-contact: stmoy
design-contact: jeffarn
doc-status: Draft
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: 7272abca9862eaf5ed7ceb161e433849497369a2
ms.sourcegitcommit: cc0ef75f314658b14376eb60ef8e5bb4d7726e04
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 05/08/2019
ms.locfileid: "65444218"
---
# <a name="bringing-it-together"></a>통합

타이밍, 감속, 방향 및 무게는 함께 작동하여 Fluent 동작의 기초를 형성합니다. 각각은 다른 것의 컨텍스트에서 간주되고 앱의 컨텍스트에서 적절하게 적용되어야 합니다.

앱에서 Fluent 움직임을 적용하는 3가지 방법은 다음과 같습니다.

:::row:::
    :::column:::
        **Implicit animation**

        Automatic tween and timing between values in a parameter change to achieve very simple Fluent motion using the standardized values.
    :::column-end:::
    :::column:::
        **Built-in animation**

        System components, such as common controls and shared motion, are "Fluent by default". Fundamentals have been applied in a manner consistent with their implied usage.
    :::column-end:::
    :::column:::
        **Custom animation following guidance recommendations**

        There may be times when the system does not yet provide an exact motion solution for your scenario. In those cases, use the baseline fundamental recommendations as a starting point for your experiences.
    :::column-end:::
:::row-end:::

**전환 예제**

![기능적 애니메이션](images/pageRefresh.gif)

:::row:::
    :::column:::
        <b>Direction Forward Out:</b><br>
        Fade out: 150m; Easing: Default Accelerate

        <b>Direction Forward In:</b><br>
        Slide up 150px: 300ms; Easing: Default Decelerate
    :::column-end:::
    :::column:::
         <b>Direction Backward Out:</b><br>
        Slide down 150px: 150ms; Easing: Default Accelerate

        <b>Direction Backward In:</b><br>
        Fade in: 300ms; Easing: Default Decelerate
    :::column-end:::
:::row-end:::

**개체 예제**

 ![300ms 움직임](images/control.gif)

:::row:::
    :::column:::
        <b>Direction Expand:</b><br>
        Grow: 300ms; Easing: Standard
    :::column-end:::
    :::column:::
        <b>Direction Contract:</b><br>
        Grow: 150ms; Easing: Default Accelerate
    :::column-end:::
:::row-end:::

## <a name="examples"></a>예

<table>
<tr>
<td><img src="images/xaml-controls-gallery-app-icon.png" alt="XAML controls gallery" width="168"></img></td>
<td>
    <p>있는 경우는 <strong style="font-weight: semi-bold">XAML 컨트롤 갤러리</strong> 앱을 설치 하려면 여기를 클릭 <a href="xamlcontrolsgallery:/item/ImplicitTransition">앱을 열고 암시적 전환 중인 참조</a>합니다.</p>
    <ul>
    <li><a href="https://www.microsoft.com/store/productId/9MSVH128X2ZT">XAML Controls Gallery 앱 가져오기(Microsoft Store)</a></li>
    <li><a href="https://github.com/Microsoft/Xaml-Controls-Gallery">소스 코드 가져오기(GitHub)</a></li>
    </ul>
</td>
</tr>
</table>

## <a name="implicit-animations"></a>암시적 애니메이션

> 암시적 애니메이션 필요한 Windows 10 버전 1809 ([17763 SDK](https://developer.microsoft.com/windows/downloads/windows-10-sdk)) 이상.

암시적 애니메이션은 매개 변수를 변경 하는 동안 이전 및 새 값을 자동으로 보간하여 Fluent 동작을 달성 하는 간단한 방법입니다.

암시적으로 같은 속성을 변경 애니메이션을 적용할 수 있습니다.

- [UIElement](/uwp/api/windows.ui.xaml.uielement)
  - **불투명도**
  - **회전**
  - **소수 자릿수**
  - **번역**

- [테두리](/uwp/api/windows.ui.xaml.controls.border)하십시오 [ContentPresenter](/uwp/api/windows.ui.xaml.controls.contentpresenter), 또는 [패널](/uwp/api/windows.ui.xaml.controls.panel)
  - **배경**

암시적으로 애니메이션 효과가 적용 된 변경 수 있는 각 속성에는 해당 _전환_ 속성입니다. 전환 형식을 해당에 할당 하는 속성에 애니메이션 효과 _전환_ 속성입니다. 이 테이블에 표시 된 _전환_ 속성 및 각각에 대해 사용 하도록 전환 형식입니다.

| 애니메이션된 속성 | 전환 속성 | 암시적 전환 형식 |
| -- | -- | -- |
| [UIElement.Opacity](/uwp/api/windows.ui.xaml.uielement.opacity) | [OpacityTransition](/uwp/api/windows.ui.xaml.uielement.opacitytransition) | [ScalarTransition](/uwp/api/windows.ui.xaml.scalartransition) |
| [UIElement.Rotation](/uwp/api/windows.ui.xaml.uielement.rotation) | [RotationTransition](/uwp/api/windows.ui.xaml.uielement.rotationtransition) | [ScalarTransition](/uwp/api/windows.ui.xaml.scalartransition) |
| [UIElement.Scale](/uwp/api/windows.ui.xaml.uielement.scale) | [ScaleTransition](/uwp/api/windows.ui.xaml.uielement.scaletransition) | [Vector3Transition](/uwp/api/windows.ui.xaml.vector3transition) |
| [UIElement.Translation](/uwp/api/windows.ui.xaml.uielement.scale) | [TranslationTransition](/uwp/api/windows.ui.xaml.uielement.translationtransition) | [Vector3Transition](/uwp/api/windows.ui.xaml.vector3transition) |
| [Border.Background](/uwp/api/windows.ui.xaml.controls.border.background) | [BackgroundTransition](/uwp/api/windows.ui.xaml.controls.border.backgroundtransition) | [BrushTransition](//uwp/api/windows.ui.xaml.uielement.brushtransition) |
| [ContentPresenter.Background](/uwp/api/windows.ui.xaml.controls.contentpresenter.background) | [BackgroundTransition](/uwp/api/windows.ui.xaml.controls.contentpresenter.backgroundtransition) | [BrushTransition](//uwp/api/windows.ui.xaml.uielement.brushtransition) |
| [Panel.Background](/uwp/api/windows.ui.xaml.controls.panel.background) | [BackgroundTransition](/uwp/api/windows.ui.xaml.controls.panel.backgroundtransition)  | [BrushTransition](//uwp/api/windows.ui.xaml.uielement.brushtransition) |

이 예제에서는 단추 컨트롤을 사용 하는 경우 페이드 인 및 페이드 아웃 비활성화 되었을 때를 확인 하려면 Opacity 속성 및 전환 사용 방법을 보여 줍니다.

```xaml
<Button x:Name="SubmitButton"
        Content="Submit"
        Opacity="{x:Bind OpaqueIfEnabled(SubmitButton.IsEnabled), Mode=OneWay}">
    <Button.OpacityTransition>
        <ScalarTransition />
    </Button.OpacityTransition>
</Button>
```

```csharp
public double OpaqueIfEnabled(bool IsEnabled)
{
    return IsEnabled ? 1.0 : 0.2;
}
```

## <a name="related-articles"></a>관련 문서

- [동작 개요](index.md)
- [시간 및 감속/가속](timing-and-easing.md)
- [방향 및 무게](directionality-and-gravity.md)
