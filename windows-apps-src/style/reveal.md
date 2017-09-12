---
author: mijacobs
description: "표시는 응용 프로그램에 더 많은 포커스와 즐거움을 추가하는 데 도움이 되는 새로운 상호 작용 모델입니다."
title: "표시"
template: detail.hbs
ms.author: mijacobs
ms.date: 08/9/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
pm-contact: kisai
design-contact: conrwi
dev-contact: jevansa
doc-status: Published
ms.openlocfilehash: d50e3f47faad5fff0ef461a4b5312127a0b9ec9c
ms.sourcegitcommit: 0d5b3daddb3ae74f91178c58e35cbab33854cb7f
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/09/2017
---
# <a name="reveal"></a>표시

> [!IMPORTANT]
> 이 문서에서는 아직 출시되지 않아 상업적으로 출시하기 전에 크게 수정될 수 있는 기능에 대해 설명합니다. Microsoft는 여기에 제공된 정보에 대해 명시적 또는 묵시적 보증을 하지 않습니다.

표시는 앱의 대화형 요소에 깊이와 포커스를 더해주는 조명 효과입니다.

> **중요 API**: [RevealBrush 클래스](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.revealbrush), [RevealBackgroundBrush 클래스](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.revealbackgroundbrush), [RevealBorderBrush 클래스](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.revealborderbrush), [RevealBrushHelper 클래스](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.revealbrushhelper), [VisualState 클래스](https://docs.microsoft.com/en-us/uwp/api/Windows.UI.Xaml.VisualState)

표시 동작은 마우스 또는 포커스 사각형이 원하는 영역 위에 올 때 영웅(또는 초점) 콘텐츠 주변의 요소 조각을 표시하여 이를 수행합니다.

![Visual 표시](images/Nav_Reveal_Animation.gif)

표시는 개체 주변의 숨겨진 테두리를 노출하여 사용자가 상호 작용하는 공간을 이해하고 사용 가능한 작업을 이해하는 데 도움을 줍니다. 이는 목록 컨트롤과 백플레이트가 있는 컨트롤에 특히 중요합니다.

## <a name="reveal-and-the-fluent-design-system"></a>표시 및 Fluent 디자인 시스템

 Fluent 디자인 시스템을 사용하면 조명, 깊이, 움직임, 재질 및 배율이 통합된 선명한 현대식 UI를 만들 수 있습니다. 표시는 앱에 조명을 추가하는 Fluent 디자인 시스템 구성 요소입니다. 

## <a name="what-is-reveal"></a>표시는 무엇인가요?

표시에는 두 개의 주요 시각적 구성 요소가 있는데, 하나는 **가리켜서 표시** 동작이고 다른 하나는 **테두리 표시** 동작입니다.

![계층 표시](images/RevealLayers.png)

가리켜서 표시는 포인터 또는 포커스 입력을 통해 가리키는 콘텐츠에 직접 연결되며, 가리킨 항목 또는 포커스가 설정된 항목 주변에 은은한 후광 형상을 적용합니다.

테두리 표시는 포커스가 설정된 항목과 그 주변 항목에 적용됩니다. 이를 통해 주변 개체가 현재 포커스가 설정된 작업과 비슷한 작업을 수행할 수 있음을 보여 줍니다.

표시 제작법 분류는 다음과 같습니다.

- 테두리 표시는 모든 콘텐츠 위에 있지만 지정된 가장자리 위에 있습니다.
- 텍스트와 콘텐츠는 테두리 표시 바로 아래에 표시됩니다.
- 가리켜서 표시는 콘텐츠 및 텍스트 아래에 있습니다.
- 백플레이트(가리켜서 표시를 켜고 사용하도록 설정)
- 배경(컨트롤의 배경)

<!--
<div class=”microsoft-internal-note”>
To create your own Reveal lighting effect for static comps or prototype purposes, see the full [uni design guidance](http://uni/DesignDepot.FrontEnd/#/ProductNav/3020/1/dv/?t=Resources%7CToolkit%7CReveal&f=Neon) for this effect in illustrator.
</div>
-->

## <a name="how-to-use-it"></a>사용 방법

표시는 필요할 때 커서 주변에 기하 도형을 노출하고, 사용자가 이동하면 원활하게 기하 도형을 페이드 아웃합니다.

표시는 암묵적 경계 및 백플레이트가 있는 앱의 주 콘텐츠(영웅 콘텐츠)에서 사용하기에 가장 적합합니다. 표시는 컬렉션 또는 목록 같은 컨트롤에도 사용해야 합니다.

## <a name="controls-that-automatically-use-reveal"></a>자동으로 표시를 사용하는 컨트롤

- [**ListView**](../controls-and-patterns/lists.md)
- [**TreeView**](../controls-and-patterns/tree-view.md)
- [**NavigationView**](../controls-and-patterns/navigationview.md)
- [**AutosuggestBox**](../controls-and-patterns/auto-suggest-box.md)

## <a name="enabling-reveal-on-other-common-controls"></a>다른 일반 컨트롤에서 표시를 사용하도록 설정

표시를 적용해야 하는 시나리오가 있는 경우(이러한 컨트롤은 주 콘텐츠이며/이거나 목록 또는 컬렉션 방향에 사용됨) 이러한 상황에 표시를 사용할 수 있는 옵트인(opt in) 리소스 스타일이 제공됩니다.

이러한 컨트롤은 일반적으로 응용 프로그램 주요 포커스 지점의 도우미 컨트롤인 작은 컨트롤이기 때문에 기본적으로 표시가 없지만, 똑같은 앱은 하나도 없으며 이러한 컨트롤이 앱에서 가장 많이 사용되는 경우를 대비하여 일부 스타일이 제공되었습니다.

| 컨트롤 이름   | 리소스 이름 |
|----------|:-------------:|
| 단추 |  ButtonRevealStyle |
| ToggleButton | ToggleButtonRevealStyle |
| RepeatButton | RepeatButtonRevealStyle |
| AppBarButton | AppBarButtonRevealStyle |
| SemanticZoom | SemanticZoomRevealStyle |
| ComboBoxItem | ComboxBoxItemRevealStyle |

이러한 스타일을 적용하려면 스타일 속성을 다음과 같이 업데이트합니다.

```XAML
<Button Content="Button Content" Style="{StaticResource ButtonRevealStyle}"/>
```

> [!NOTE]
> SDK의 16190 버전은 이러한 스타일을 자동으로 사용할 수 있도록 만들지 않습니다. 스타일을 받으려면 generic.xaml에서 수동으로 앱에 복사해야 합니다. Generic.xaml은 일반적으로 C:\Program Files (x86)\Windows Kits\10\DesignTime\CommonConfiguration\Neutral\UAP\10.0.16190.0\Generic에 있습니다. 이 문제는 이후 빌드에서 해결될 예정입니다. 

## <a name="enabling-reveal-on-custom-controls"></a>사용자 지정 컨트롤에서 표시를 사용하도록 설정

사용자 지정 컨트롤 또는 다시 템플릿으로 지정된 컨트롤에서 표시를 사용하려면 해당 컨트롤의 시각적 상태에서 해당 컨트롤의 스타일로 이동하고, 루트 그리드에서 표시를 지정합니다.

```xaml
<VisualState x:Name="PointerOver">
  <VisualState.Setters>
    <Setter Target="RootGrid.(RevealBrushHelper.State)" Value="PointerOver" />
    <Setter Target="RootGrid.Background" Value="{ThemeResource ButtonRevealBackgroundPointerOver}"/>
    <Setter Target="ContentPresenter.BorderBrush" Value="{ThemeResource ButtonRevealBorderBrushPointerOver}"/>
    <Setter Target="ContentPresenter.Foreground" Value="{ThemeResource ButtonForegroundPointerOver}"/>
  </VisualState.Setters>
</VisualState>
```

표시의 모든 효과를 끌어내려면 PressedState에도 똑같은 RevealBrushHelper를 추가해야 합니다.

```xaml
<VisualState x:Name="Pressed">
  <VisualState.Setters>
    <Setter Target="RootGrid.(RevealBrushHelper.State)" Value="Pressed" />
    <Setter Target="RootGrid.Background" Value="{ThemeResource ButtonRevealBackgroundPressed}"/>
    <Setter Target="ContentPresenter.BorderBrush" Value="{ThemeResource ButtonRevealBorderBrushPressed}"/>
    <Setter Target="ContentPresenter.Foreground" Value="{ThemeResource ButtonForegroundPressed}"/>
  </VisualState.Setters>
</VisualState>
```


Microsoft의 시스템 리소스 표시를 사용한다는 것은 Microsoft가 사용자를 대신하여 테마 변경을 처리한다는 의미입니다.

## <a name="dos-and-donts"></a>권장 사항 및 금지 사항
- 사용자가 작업을 수행할 수 있는 요소에 표시 사용 - 표시가 정적 콘텐츠에 있으면 안 됨
- 데이터 목록 또는 컬렉션에 표시 사용
- 백플레이트가 있는 명확하게 포함된 콘텐츠에 표시 적용
- 상호 작용이 불가능한 정적 배경, 텍스트 또는 이미지에는 표시를 사용하지 말 것
- 관련이 없는 부동 콘텐츠에는 표시를 적용하지 말 것
- 콘텐츠 대화 또는 알림 같은 격리된 일회성 상황에는 표시를 사용하지 말 것
- 사용자에게 제공해야 하는 메시지로부터 사용자의 주의를 뺏을 수 있으므로 보안 결정에는 표시를 사용하지 말 것

## <a name="related-articles"></a>관련 문서

- [RevealBrush 클래스](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.revealbrush)
- [**아크릴**](acrylic.md)
- [**컴퍼지션 효과**](https://msdn.microsoft.com/windows/uwp/graphics/composition-effects)
