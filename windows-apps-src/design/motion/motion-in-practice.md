---
author: jwmsft
Description: Learn how Fluent motion fundamentals come together in your app.
title: 동작 중인 움직임 - UWP 앱의 애니메이션
label: Motion in practice
template: detail.hbs
ms.author: jimwalk
ms.date: 10/02/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
pm-contact: stmoy
design-contact: jeffarn
doc-status: Draft
ms.localizationpriority: medium
ms.openlocfilehash: 6001f955b3ab6a60446eb84296dc3bc52ad3a99e
ms.sourcegitcommit: 1938851dc132c60348f9722daf994b86f2ead09e
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/02/2018
ms.locfileid: "4260018"
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

## <a name="implicit-animations"></a>암시적 애니메이션

> **미리 보기**: 암시적 애니메이션은 [최신 Windows 10 Insider Preview 빌드 및 SDK](https://insider.windows.com/for-developers/)필요 합니다.

암시적 애니메이션은 자동으로 매개 변수를 변경 하는 동안 이전 및 새 값 사이의 보간 하 여 Fluent 움직임을 달성 하는 간단한 방법입니다.

다음 속성 변경에 암시적으로 애니메이션할 수 있습니다.

- [UIElement](/uwp/api/windows.ui.xaml.uielement)
  - **불투명도**
  - **회전**
  - **배율**
  - **Translation**

- [테두리](/uwp/api/windows.ui.xaml.controls.border), [ContentPresenter](/uwp/api/windows.ui.xaml.controls.contentpresenter)또는 [패널](/uwp/api/windows.ui.xaml.controls.panel)
  - **Background**

각 속성을 변경 암시적으로 애니메이션 효과 줄 수 있는 해당 _전환_ 속성이 있습니다. 속성에 애니메이션을 전환 유형을 해당 _전환_ 속성에 할당 합니다. 이 표에서 각각에 대해 사용할 전환 종류와 _전환_ 속성을 보여줍니다.

| 애니메이션 효과 준된 속성 | 전환 속성 | 암시적 전환 유형 |
| -- | -- | -- |
| [UIElement.Opacity](/uwp/api/windows.ui.xaml.uielement.opacity) | [OpacityTransition](/uwp/api/windows.ui.xaml.uielement.opacitytransition) | [ScalarTransition](/uwp/api/windows.ui.xaml.scalartransition) |
| [UIElement.Rotation](/uwp/api/windows.ui.xaml.uielement.rotation) | [RotationTransition](/uwp/api/windows.ui.xaml.uielement.rotationtransition) | [ScalarTransition](/uwp/api/windows.ui.xaml.scalartransition) |
| [UIElement.Scale](/uwp/api/windows.ui.xaml.uielement.scale) | [ScaleTransition](/uwp/api/windows.ui.xaml.uielement.scaletransition) | [Vector3Transition](/uwp/api/windows.ui.xaml.uielement.vector3transition) |
| [UIElement.Translation](/uwp/api/windows.ui.xaml.uielement.scale) | [TranslationTransition](/uwp/api/windows.ui.xaml.uielement.translationtransition) | [Vector3Transition](/uwp/api/windows.ui.xaml.uielement.vector3transition) |
| [Border.Background](/uwp/api/windows.ui.xaml.controls.border.background) | [BackgroundTransition](/uwp/api/windows.ui.xaml.controls.border.backgroundtransition) | [BrushTransition](//uwp/api/windows.ui.xaml.uielement.brushtransition) |
| [ContentPresenter.Background](/uwp/api/windows.ui.xaml.controls.contentpresenter.background) | [BackgroundTransition](/uwp/api/windows.ui.xaml.controls.contentpresenter.backgroundtransition) | [BrushTransition](//uwp/api/windows.ui.xaml.uielement.brushtransition) |
| [Panel.Background](/uwp/api/windows.ui.xaml.controls.panel.background) | [BackgroundTransition](/uwp/api/windows.ui.xaml.controls.panel.backgroundtransition)  | [BrushTransition](//uwp/api/windows.ui.xaml.uielement.brushtransition) |

이 예제에서는 불투명도 속성 및 전환 컨트롤을 사용 하는 페이드 인 / 페이드 아웃 비활성화 된 경우 단추를 사용 하 여 하는 방법을 보여 줍니다.

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

- [움직임 개요](index.md)
- [타이밍 및 감속](timing-and-easing.md)
- [방향 및 무게](directionality-and-gravity.md)