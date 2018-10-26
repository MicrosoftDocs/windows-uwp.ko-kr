---
author: jwmsft
description: Template settings 클래스
title: Template settings 클래스
ms.assetid: CAE933C6-EF13-465A-9831-AB003AF23907
ms.author: jimwalk
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 4d7b08138ab22d4cf2cbf4fb5273759f000a7c94
ms.sourcegitcommit: d0e836dfc937ebf7dfa9c424620f93f3c8e0a7e8
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/26/2018
ms.locfileid: "5666619"
---
# <a name="template-settings-classes"></a>Template settings 클래스


## <a name="prerequisites"></a>필수 조건

컨트롤을 UI에 추가하고 속성을 설정하고 이벤트 처리기를 연결할 수 있다고 가정합니다. 컨트롤을 앱에 추가하는 방법에 대해서는 [컨트롤 추가 및 이벤트 처리](https://msdn.microsoft.com/library/windows/apps/mt228345)를 참조하세요. 또한 기본 템플릿을 복사한 후 편집하여 컨트롤에 대한 사용자 지정 템플릿을 정의하는 방법에 대한 기본 사항을 알고 있다고 가정합니다. 자세한 내용은 [빠른 시작: 컨트롤 템플릿](https://msdn.microsoft.com/library/windows/apps/xaml/hh465374)을 참조하세요.

## <a name="the-scenario-for-templatesettings-classes"></a>**TemplateSettings** 클래스에 대한 시나리오

**TemplateSettings** 클래스는 컨트롤에 대한 새 컨트롤 템플릿을 정의할 때 사용되는 속성 집합을 제공합니다. 속성은 값(예제: 특정 UI 요소의 크기에 대한 픽셀 측정값)이 있습니다. 일반적으로 재정의하거나 액세스하기 쉽지 않은 컨트롤 논리에서 계산된 값을 가지는 경우도 있습니다. 일부 속성은 파트의 전환 및 애니메이션을 제어하는 **From** 및 **To** 값으로 사용되므로 관련 **TemplateSettings** 속성이 쌍으로 제공됩니다.

여러 가지 **TemplateSettings** 클래스가 있습니다. 이러한 모든 클래스는 [**Windows.UI.Xaml.Controls.Primitives**](https://msdn.microsoft.com/library/windows/apps/br209818) 네임스페이스에 있습니다. 클래스의 목록과 관련 컨트롤의 **TemplateSettings** 속성에 대한 링크는 다음과 같습니다. 이 **TemplateSettings** 속성은 컨트롤의 **TemplateSettings** 값에 액세스하는 데 사용되며, 속성에 대한 템플릿 바인딩을 설정할 수 있습니다.

-   [**ComboBoxTemplateSettings**](https://msdn.microsoft.com/library/windows/apps/br227752): [**ComboBox.TemplateSettings**](https://msdn.microsoft.com/library/windows/apps/br209364)의 값
-   [**GridViewItemTemplateSettings**](https://msdn.microsoft.com/library/windows/apps/hh738499): [**GridViewItem.TemplateSettings**](https://msdn.microsoft.com/library/windows/apps/hh738503)의 값
-   [**ListViewItemTemplateSettings**](https://msdn.microsoft.com/library/windows/apps/hh701948): [**ListViewItem.TemplateSettings**](https://msdn.microsoft.com/library/windows/apps/br242923)의 값
-   [**ProgressBarTemplateSettings**](https://msdn.microsoft.com/library/windows/apps/br227856): [**ProgressBar.TemplateSettings**](https://msdn.microsoft.com/library/windows/apps/br227537)의 값
-   [**ProgressRingTemplateSettings**](https://msdn.microsoft.com/library/windows/apps/hh702248): [**ProgressRing.TemplateSettings**](https://msdn.microsoft.com/library/windows/apps/hh702581)의 값
-   [**SettingsFlyoutTemplateSettings**](https://msdn.microsoft.com/library/windows/apps/dn298721): [**SettingsFlyout.TemplateSettings**](https://msdn.microsoft.com/library/windows/apps/dn252826)의 값
-   [**ToggleSwitchTemplateSettings**](https://msdn.microsoft.com/library/windows/apps/br209804): [**ToggleSwitch.TemplateSettings**](https://msdn.microsoft.com/library/windows/apps/br209731)의 값
-   [**ToolTipTemplateSettings**](https://msdn.microsoft.com/library/windows/apps/br209813): [**ToolTip.TemplateSettings**](https://msdn.microsoft.com/library/windows/apps/br227629)의 값

**TemplateSettings** 속성은 항상 XAML에서 사용되며, 코드에서 사용할 수 없습니다. 이러한 속성은 부모 컨트롤에 대한 읽기 전용 **TemplateSettings** 속성의 읽기 전용 하위 속성입니다. 새로운 [**Control**](https://msdn.microsoft.com/library/windows/apps/br209390) 기반 클래스를 만들어서 컨트롤 논리에 영향을 줄 수 있는 고급 사용자 지정 컨트롤 시나리오의 경우 컨트롤 템플릿을 다시 작성하려는 모든 사용자에게 유용한 정보를 전달하기 위해 컨트롤에 대한 사용자 지정 **TemplateSettings** 속성을 정의할 것을 고려하세요. 해당 속성의 읽기 전용 값으로, 템플릿 측정, 애니메이션 위치 등과 관련된 각 정보 항목에 대한 읽기 전용 속성을 갖는 새 **TemplateSettings** 클래스를 정의하고, 호출자에게 컨트롤 논리를 사용하여 초기화되는 해당 클래스의 런타임 인스턴스를 제공합니다. **TemplateSettings** 클래스는 [**DependencyObject**](https://msdn.microsoft.com/library/windows/apps/br242356)에서 파생되므로 속성에서 속성 변경 콜백에 대한 종속성 속성 시스템을 사용할 수 있습니다. 하지만 **TemplateSettings** 속성은 호출자에게 읽기 전용이므로 속성에 대한 종속성 속성 식별자가 공용 API로 노출되지 않습니다.

## <a name="how-to-use-templatesettings-in-a-control-template"></a>컨트롤 템플릿에서 **TemplateSettings**을 사용하는 방법

시작하는 기본 XAML 컨트롤 템플릿의 예는 다음과 같습니다. 이 예는 [**ProgressRing**](https://msdn.microsoft.com/library/windows/apps/br227538)의 기본 템플릿을 기반으로 합니다.

```xml
<Ellipse
    x:Name="E1"
    Style="{StaticResource ProgressRingEllipseStyle}"
    Width="{Binding RelativeSource={RelativeSource TemplatedParent}, 
        Path=TemplateSettings.EllipseDiameter}"
    Height="{Binding RelativeSource={RelativeSource TemplatedParent}, 
        Path=TemplateSettings.EllipseDiameter}"
    Margin="{Binding RelativeSource={RelativeSource TemplatedParent}, 
        Path=TemplateSettings.EllipseOffset}"
    Fill="{TemplateBinding Foreground}"/>
```

[**ProgressRing**](https://msdn.microsoft.com/library/windows/apps/br227538) 템플릿의 전체 XAML은 수백 줄에 달하므로 여기서는 일부를 발췌했습니다. 이 XAML은 확정되지 않은 진행률에 대한 회전 애니메이션을 나타내는 6개의 [**Ellipse**](/uwp/api/Windows.UI.Xaml.Shapes.Ellipse) 컨트롤 중 하나인 컨트롤 파트를 정의합니다. 개발자는 원을 싫어할 수도 있으므로 다른 그래픽을 과 같은 다른 그래픽 기본 요소 또는 다른 기본 모양을 사용하여 애니케이션의 진행률을 나타낼 수 있습니다. 예를 들어 사각형으로 정렬된 [**Rectangle**](/uwp/api/Windows.UI.Xaml.Shapes.Rectangle) 요소 집합을 사용하는 **ProgressRing**을 구성할 수 있습니다. 그럴 경우 새 템플릿의 각 **Rectangle** 구성 요소는 다음과 같이 표시될 수 있습니다.

```xml
<Rectangle
    x:Name="R1"
    Width="{Binding RelativeSource={RelativeSource TemplatedParent}, 
        Path=TemplateSettings.EllipseDiameter}"
    Height="{Binding RelativeSource={RelativeSource TemplatedParent}, 
        Path=TemplateSettings.EllipseDiameter}"
    Margin="{Binding RelativeSource={RelativeSource TemplatedParent}, 
        Path=TemplateSettings.EllipseOffset}"
    Fill="{TemplateBinding Foreground}"/>
```

여기서 **TemplateSettings** 속성이 유용한 이유는 [**ProgressRing**](https://msdn.microsoft.com/library/windows/apps/br227538)의 기본 컨트롤 논리에서 계산된 값을 가져오기 때문입니다. 계산에서는 **ProgressRing**의 전체 [**ActualWidth**](https://msdn.microsoft.com/library/windows/apps/br208709) 및 [**ActualHeight**](https://msdn.microsoft.com/library/windows/apps/br208707)를 분할하고 템플릿 파트의 크기가 콘텐츠에 맞도록 템플릿의 각 동작 요소에 대해 계산된 측정값을 할당합니다.

다음은 애니메이션의 **From** 및 **To**에 해당하는 속성 집합 중 하나를 보여주는 기본 XAML 컨트롤 템플릿의 다른 사용 예입니다. 이 예제는 다음 [**ComboBox**](https://msdn.microsoft.com/library/windows/apps/br209348) 기본 템플릿을 기반으로 합니다.

```xml
<VisualStateGroup x:Name="DropDownStates">
    <VisualState x:Name="Opened">
        <Storyboard>
            <SplitOpenThemeAnimation
               OpenedTargetName="PopupBorder"
               ContentTargetName="ScrollViewer"
               ClosedTargetName="ContentPresenter"
               ContentTranslationOffset="0"
               OffsetFromCenter="{Binding RelativeSource={RelativeSource TemplatedParent}, 
                 Path=TemplateSettings.DropDownOffset}"
               OpenedLength="{Binding RelativeSource={RelativeSource TemplatedParent}, 
                 Path=TemplateSettings.DropDownOpenedHeight}"
               ClosedLength="{Binding RelativeSource={RelativeSource TemplatedParent},
                 Path=TemplateSettings.DropDownClosedHeight}" />
        </Storyboard>
   </VisualState>
...
</VisualStateGroup>
```

이 템플릿에는 많은 XAML이 있으므로 일부만 발췌했습니다. 또한 각각 동일한 [**ComboBoxTemplateSettings**](https://msdn.microsoft.com/library/windows/apps/br227752) 속성을 사용하는 여러 상태 및 테마 애니메이션 중 하나에 불과합니다. [**ComboBox**](https://msdn.microsoft.com/library/windows/apps/br209348)의 경우 바인딩을 통해 **ComboBoxTemplateSettings** 값을 사용하여 공유 값을 기반으로 해당 위치에서 템플릿의 관련 애니메이션을 중지 및 시작하므로 부드럽게 전환됩니다.

**참고**  **TemplateSettings** 값을 사용 하 여 컨트롤 템플릿의 일부로 수행을 값의 형식과 일치 하는 속성을 설정 하 고 있는지 확인 합니다. 그렇지 않으면 바인딩의 대상 형식이 **TemplateSettings** 값의 다른 소스 형식으로부터 변환될 수 있도록 바인딩에 대한 값 변환기를 만들어야 합니다. 자세한 내용은 [**IValueConverter**](https://msdn.microsoft.com/library/windows/apps/br209903)를 참조하세요.

## <a name="related-topics"></a>관련 항목

* [빠른 시작: 컨트롤 템플릿](https://msdn.microsoft.com/library/windows/apps/xaml/hh465374)

