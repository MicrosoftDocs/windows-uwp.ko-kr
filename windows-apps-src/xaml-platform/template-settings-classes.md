---
description: 템플릿 설정 클래스를 사용 하 여 새 컨트롤 템플릿을 정의 하는 속성 집합을 제공 하는 방법에 대해 알아봅니다.
title: Template settings 클래스
ms.assetid: CAE933C6-EF13-465A-9831-AB003AF23907
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 23418233769d893e755ee8981aa9f66aa9404758
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89154967"
---
# <a name="template-settings-classes"></a>Template settings 클래스


## <a name="prerequisites"></a>필수 구성 요소

UI에 컨트롤을 추가 하 고, 속성을 설정 하 고, 이벤트 처리기를 연결할 수 있다고 가정 합니다. 응용 프로그램에 컨트롤을 추가 하는 방법에 대 한 지침은 [컨트롤 추가 및 이벤트 처리](../design/controls-and-patterns/controls-and-events-intro.md)를 참조 하세요. 또한 기본 템플릿의 복사본을 만들고 편집 하 여 컨트롤에 대 한 사용자 지정 템플릿을 정의 하는 방법의 기본 사항을 알고 있다고 가정 합니다. 이에 대 한 자세한 내용은 [빠른 시작: 컨트롤 템플릿](/previous-versions/windows/apps/hh465374(v=win.10))을 참조 하세요.

## <a name="the-scenario-for-templatesettings-classes"></a>**서식 설정** 클래스에 대 한 시나리오

템플릿 **설정** 클래스는 컨트롤에 대 한 새 컨트롤 템플릿을 정의할 때 사용 되는 속성 집합을 제공 합니다. 속성에는 특정 UI 요소 부분의 크기에 대 한 픽셀 측정치와 같은 값이 있습니다. 값은 경우에 따라 일반적으로 쉽게 재정의 하거나 액세스할 수 없는 제어 논리에서 제공 되는 계산 된 값입니다. 일부 속성은 요소에 **대 한 전환** 및 애니메이션을 제어 하는 값의 **일부 이며,** 따라서 관련 된 템플릿 **설정** 속성은 쌍으로 제공 됩니다.

여러 **서식 설정** 클래스가 있습니다. 모든 파일은 [**Windows. .xaml**](/uwp/api/Windows.UI.Xaml.Controls.Primitives) 네임 스페이스에 있습니다. 다음은 클래스 목록과 관련 컨트롤의 템플릿 **설정** 속성에 대 한 링크입니다. 이 템플릿 **설정** 속성은 컨트롤에 대 한 템플릿 **설정** 값에 액세스 하는 방법 이며 해당 속성에 대 한 템플릿 바인딩을 설정할 수 있습니다.

-   [**ComboBoxTemplateSettings**](/uwp/api/Windows.UI.Xaml.Controls.Primitives.ComboBoxTemplateSettings): [ **ComboBox 설정** 의 값](/uwp/api/windows.ui.xaml.controls.combobox.templatesettings)
-   [**GridViewItemTemplateSettings**](/uwp/api/Windows.UI.Xaml.Controls.Primitives.GridViewItemTemplateSettings): [ **GridViewItem 설정** 의 값](/uwp/api/windows.ui.xaml.controls.gridviewitem.templatesettings)
-   [**ListViewItemTemplateSettings**](/uwp/api/Windows.UI.Xaml.Controls.Primitives.ListViewItemTemplateSettings): [ **ListViewItem 설정** 의 값](/uwp/api/windows.ui.xaml.controls.listviewitem.templatesettings)
-   [**ProgressBarTemplateSettings**](/uwp/api/Windows.UI.Xaml.Controls.Primitives.ProgressBarTemplateSettings): [ **ProgressBar 설정** 의 값](/uwp/api/windows.ui.xaml.controls.progressbar.templatesettings)
-   [**ProgressRingTemplateSettings**](/uwp/api/Windows.UI.Xaml.Controls.Primitives.ProgressRingTemplateSettings): [ **ProgressRing 설정** 의 값](/uwp/api/windows.ui.xaml.controls.progressring.templatesettings)
-   [**SettingsFlyoutTemplateSettings**](/uwp/api/Windows.UI.Xaml.Controls.Primitives.SettingsFlyoutTemplateSettings): [ **settingsflyout의 값입니다.**](/uwp/api/windows.ui.xaml.controls.settingsflyout.templatesettings)
-   [**ToggleSwitchTemplateSettings**](/uwp/api/Windows.UI.Xaml.Controls.Primitives.ToggleSwitchTemplateSettings): [ **ToggleSwitch 설정** 의 값](/uwp/api/windows.ui.xaml.controls.toggleswitch.templatesettings)
-   [**ToolTipTemplateSettings**](/uwp/api/Windows.UI.Xaml.Controls.Primitives.ToolTipTemplateSettings): [ **ToolTip 설정** 의 값](/uwp/api/windows.ui.xaml.controls.tooltip.templatesettings)

템플릿 **설정** 속성은 코드를 사용 하는 것이 아니라 XAML에서 항상 사용 됩니다. 부모 컨트롤의 읽기 전용 템플릿 **설정** 속성에 대 한 읽기 전용 하위 속성입니다. 새 [**컨트롤**](/uwp/api/Windows.UI.Xaml.Controls.Control)기반 클래스를 만들고 컨트롤 논리에 영향을 줄 수 있는 고급 사용자 지정 컨트롤 시나리오의 경우 컨트롤의 템플릿을 다시 작성 하는 모든 사용자에 게 유용할 수 있는 정보를 전달 하기 위해 컨트롤에 사용자 지정 템플릿 **설정** 속성을 정의 하는 것이 좋습니다. 해당 속성의 읽기 전용 값으로 템플릿 측정, 애니메이션 위치 지정과 관련 된 각 정보 항목에 대 한 읽기 전용 속성을 포함 하는 컨트롤에 관련 된 새 템플릿 **설정** 클래스를 정의 하 고 호출자에 게 컨트롤 논리를 사용 하 여 초기화 된 해당 클래스의 런타임 인스턴스를 부여 합니다. 템플릿 **설정** 클래스는 [**DependencyObject**](/uwp/api/Windows.UI.Xaml.DependencyObject)에서 파생 되므로 속성은 속성 변경 콜백에 대 한 종속성 속성 시스템을 사용할 수 있습니다. 그러나 템플릿 **설정** 속성이 호출자에 게 읽기 전용 이기 때문에 속성에 대 한 종속성 속성 식별자는 공용 API로 노출 되지 않습니다.

## <a name="how-to-use-templatesettings-in-a-control-template"></a>컨트롤 템플릿에서 **서식 파일 설정을** 사용 하는 방법

다음은 기본 XAML 컨트롤 템플릿 시작에서 제공 되는 예제입니다. 이 특정 항목은 [**ProgressRing**](/uwp/api/Windows.UI.Xaml.Controls.ProgressRing)의 기본 템플릿에서 가져온 것입니다.

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

[**ProgressRing**](/uwp/api/Windows.UI.Xaml.Controls.ProgressRing) 템플릿에 대 한 전체 XAML은 수백 줄 이므로이는 작은 발췌 일 뿐입니다. 이 XAML은 확정 되지 않은 진행률에 대해 회전 하는 애니메이션을 표시 된다 하는 6 개의 [**타원**](/uwp/api/Windows.UI.Xaml.Shapes.Ellipse) 요소 중 하나인 컨트롤 파트를 정의 합니다. 개발자는 원이 마음에 들지 않을 수 있으며 애니메이션이 진행 되는 방식에 다른 그래픽 기본 형식 또는 다른 기본 셰이프를 사용할 수 있습니다. 예를 들어 사각형에 정렬 된 [**사각형**](/uwp/api/Windows.UI.Xaml.Shapes.Rectangle) 요소 집합을 사용 하는 **ProgressRing** 을 구성할 수 있습니다. 그렇다면 새 템플릿의 각 개별 **사각형** 구성 요소가 다음과 같이 표시 될 수 있습니다.

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

여기에서 템플릿 **설정** 속성이 유용한 이유는 [**ProgressRing**](/uwp/api/Windows.UI.Xaml.Controls.ProgressRing)의 기본 제어 논리에서 발생 하는 계산 된 값 이기 때문입니다. 계산은 **ProgressRing**의 전체 [**system.windows.frameworkelement.actualwidth**](/uwp/api/windows.ui.xaml.frameworkelement.actualwidth) 와 [**system.windows.frameworkelement.actualheight**](/uwp/api/windows.ui.xaml.frameworkelement.actualheight) 를 나누고 템플릿 파트가 내용에 맞게 조정 될 수 있도록 템플릿의 각 동작 요소에 대해 계산 된 측정값을 allotting 합니다.

다음은 기본 XAML 컨트롤 템플릿의 다른 예제 사용법입니다. 이번에는 애니메이션의 **시작** **및 끝에 해당** 하는 속성 집합 중 하나를 표시 합니다. [**콤보 상자**](/uwp/api/Windows.UI.Xaml.Controls.ComboBox) 기본 템플릿에서 다음을 수행 합니다.

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

템플릿에 많은 XAML이 있으므로 발췌만 표시 됩니다. 그리고 각각 동일한 [**ComboBoxTemplateSettings**](/uwp/api/Windows.UI.Xaml.Controls.Primitives.ComboBoxTemplateSettings) 속성을 사용 하는 여러 상태 및 테마 애니메이션 중 하나일 뿐입니다. [**ComboBox**](/uwp/api/Windows.UI.Xaml.Controls.ComboBox)의 경우 **ComboBoxTemplateSettings** 값을 통해 바인딩을 사용 하면 템플릿의 관련 애니메이션이 공유 값을 기반으로 하는 위치에서 중지 및 시작 되므로 원활 하 게 전환할 수 있습니다.

**참고**    컨트롤 템플릿의 일부로 템플릿 **설정** 값을 사용 하는 경우 값의 형식과 일치 하는 속성을 설정 하 고 있는지 확인 합니다. 그렇지 않으면 바인딩의 대상 형식을 템플릿 **설정** 값의 다른 원본 형식에서 변환할 수 있도록 바인딩에 대 한 값 변환기를 만들어야 할 수 있습니다. 자세한 내용은 [**ivalueconverter.convert**](/uwp/api/Windows.UI.Xaml.Data.IValueConverter)를 참조 하세요.

## <a name="related-topics"></a>관련 항목

* [빠른 시작: 컨트롤 템플릿](/previous-versions/windows/apps/hh465374(v=win.10))