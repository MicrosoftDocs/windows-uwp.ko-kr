---
description: 이 문서에서는 C#을 사용하여 WinUI 3용 XAML 템플릿 기반 컨트롤을 만드는 과정을 안내합니다.
title: C#을 사용하는 WinUI 3 앱용 템플릿 기반 XAML 컨트롤
ms.date: 09/11/2020
ms.topic: article
keywords: Windows 10, UWP, 사용자 지정 컨트롤, 템플릿 기반 컨트롤, WinUI
ms.author: drewbat
author: drewbatgit
ms.localizationpriority: high
ms.custom: 19H1
ms.openlocfilehash: 618bfc5a9937d29c546dc9420a1cba1c25fcc0ea
ms.sourcegitcommit: aabd6f40df6cc82bb8ce3a43275e4abd568c236f
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/14/2020
ms.locfileid: "92061701"
---
# <a name="templated-xaml-controls-for-winui-3-apps-with-c"></a>C#을 사용하는 WinUI 3 앱용 템플릿 기반 XAML 컨트롤

이 문서에서는 C#을 사용하여 WinUI 3용 템플릿 기반 XAML 컨트롤을 만드는 과정을 안내합니다. 템플릿 기반 컨트롤은 **Microsoft.UI.Xaml.Controls.Control**에서 상속되며, XAML 컨트롤 템플릿을 사용하여 사용자 지정할 수 있는 시각적 구조와 시각적 동작을 포함합니다.

이 문서의 단계를 수행하기 전에 먼저 개발 환경이 WinUI 3 앱을 만들도록 구성되어 있는지 확인해야 합니다. 설정에 대한 자세한 내용은 [데스크톱 앱용 WinUI 3 시작](./get-started-winui3-for-desktop.md)을 참조하세요.

## <a name="create-a-blank-app-bglabelcontrolapp"></a>비어 있는 앱(BgLabelControlApp) 만들기

먼저 Microsoft Visual Studio에서 새 프로젝트를 만듭니다. **새 프로젝트 만들기** 대화 상자에서 **비어 있는 앱(UWP의 WinUI)** 프로젝트 템플릿을 선택하고, C++ 언어 버전을 선택해야 합니다. 파일 이름이 아래 예제의 코드와 일치하도록 프로젝트 이름을 "BgLabelControlApp"으로 설정합니다. **대상 버전**을 Windows 10, 버전 1903(빌드 18362)으로, **최소 버전**을 Windows 10, 버전 1803(빌드 17134)으로 설정합니다. 이 연습은 **빈 앱, 패키지됨(데스크톱의 WinUI)** 프로젝트 템플릿을 사용하여 만든 데스크톱 앱에서도 작동하며, **BgLabelControlApp(데스크톱)** 프로젝트의 모든 단계를 수행하면 됩니다.

![비어 있는 앱 프로젝트 템플릿](images/winui-csharp-new-project-uwp.png)

## <a name="add-a-templated-control-to-your-app"></a>앱에 템플릿 기반 컨트롤 추가

템플릿 기반 컨트롤을 추가하려면 도구 모음에서 **프로젝트** 메뉴를 클릭하거나 **솔루션 탐색기**에서 마우스 오른쪽 단추로 프로젝트를 클릭하고, **새 항목 추가**를 선택합니다. **Visual C#->WinUI** 아래에서 **사용자 지정 컨트롤(WinUI)** 템플릿을 선택합니다. 새 컨트롤의 이름을 "BgLabelControl"로 지정하고, *추가*를 클릭합니다. 그러면 두 개의 새 파일이 프로젝트에 추가됩니다. `BgLabelControl.cs`에는 컨트롤에 대한 코드 숨김이 포함되어 있습니다. 

## <a name="update-the-code-behind-file"></a>코드 숨김 파일 업데이트

BgLabelControl.xaml.cs 코드 숨김 파일에서 생성자는 컨트롤에 대한 **DefaultStyleKey** 속성을 정의합니다. 컨트롤 소비자에서 템플릿을 명시적으로 지정하지 않을 경우 이 키는 사용되는 기본 템플릿을 식별합니다. 키 값은 컨트롤의 *형식*입니다. 나중에 제네릭 템플릿 파일을 구현할 때 이 키를 사용하는 것을 볼 수 있습니다.

```csharp
public BgLabelControl()
{
    this.DefaultStyleKey = typeof(BgLabelControl);
}
```

템플릿 기반 컨트롤에는 코드, XAML 또는 데이터 바인딩을 통해 프로그래밍 방식으로 설정할 수 있는 텍스트 레이블이 있습니다. 시스템에서 컨트롤 레이블의 텍스트를 최신 상태로 유지하려면 [DependencyPropety](/uwp/api/Windows.UI.Xaml.DependencyProperty)로 구현해야 합니다. 이렇게 하려면 먼저 문자열 속성을 선언하고, **Label**이라고 합니다. 지원 변수를 사용하는 대신 [GetValue](/uwp/api/windows.ui.xaml.dependencyobject.getvalue) 및 [SetValue](/uwp/api/windows.ui.xaml.dependencyobject.setvalue)를 호출하여 종속성 속성의 값을 설정하고 가져옵니다. 이러한 메서드는 **Microsoft.UI.Xaml.Controls.Control**에서 상속하는 [DependencyObject](/uwp/api/windows.ui.xaml.dependencyobject)에서 제공합니다.

```csharp
public string Label
{
    get => (string)GetValue(LabelProperty);
    set => SetValue(LabelProperty, value);
}
```
그런 다음, 종속성 속성을 선언하고, [DependencyProperty.Register](/uwp/api/windows.ui.xaml.dependencyproperty.register)를 호출하여 시스템에 등록합니다. 이 메서드는 **Label** 속성의 이름과 형식, 속성 소유자의 형식, **BgLabelControl** 클래스 및 속성의 기본값을 지정합니다.

```csharp
DependencyProperty LabelProperty = DependencyProperty.Register(
    nameof(Label), 
    typeof(string),
    typeof(BgLabelControl), 
    new PropertyMetadata(default(string)));
```

이러한 두 단계는 종속성 속성을 구현하는 데 필요한 모든 것이지만, 다음 예제에서는 **OnLabelChanged** 이벤트에 대한 선택적 처리기를 추가합니다. 속성 값이 업데이트될 때마다 시스템에서 이 이벤트가 발생합니다. 이 경우 새 레이블 텍스트가 빈 문자열인지 확인하고, 이에 따라 클래스 변수를 업데이트합니다.

```csharp
public bool HasLabelValue { get; set; }

private static void OnLabelChanged(DependencyObject d, DependencyPropertyChangedEventArgs e)
{
    {
        BgLabelControl labelControl = d as BgLabelControl; //null checks omitted
        String s = e.NewValue as String; //null checks omitted
        if (s == String.Empty)
        {
            labelControl.HasLabelValue = false;
        }
        else
        {
            labelControl.HasLabelValue = true;
        }
    }
}
```
종속성 속성의 작동 방식에 대한 자세한 내용은 [종속성 속성 개요](/windows/uwp/xaml-platform/dependency-properties-overview)를 참조하세요.

## <a name="define-the-default-style-for-bglabelcontrol"></a>BgLabelControl의 기본 스타일 정의
컨트롤 사용자가 스타일을 명시적으로 설정하지 않은 경우 템플릿 기반 컨트롤에서 사용되는 기본 스타일 템플릿을 제공해야 합니다. 이 단계에서 컨트롤에 대한 제네릭 템플릿 파일을 만듭니다.

**모든 파일 표시**가 아직 설정되어 있는지 확인합니다(**솔루션 탐색기**에서). 프로젝트 노드에서 새 폴더를 만들고 이름을 “Themes”로 지정합니다. `Themes` 아래에서 **Visual C# > WinUI > 리소스 사전(WinUI)** 형식의 새 항목을 추가하고, 이름을 "Generic.xaml"로 지정합니다. XAML 프레임워크에서 템플릿 기반 컨트롤의 기본 스타일을 찾을 수 있도록 폴더 및 파일 이름이 이와 비슷해야 합니다. Generic.xaml의 기본 내용을 삭제하고, 아래 태그를 붙여넣습니다.



```xaml
<!-- \Themes\Generic.xaml -->
<ResourceDictionary
    xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
    xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
    xmlns:local="using:BgLabelControlApp">

    <Style TargetType="local:BgLabelControl" >
        <Setter Property="Template">
            <Setter.Value>
                <ControlTemplate TargetType="local:BgLabelControl">
                    <Grid Width="100" Height="100" Background="{TemplateBinding Background}">
                        <TextBlock HorizontalAlignment="Center" VerticalAlignment="Center" Text="{TemplateBinding Label}"/>
                    </Grid>
                </ControlTemplate>
            </Setter.Value>
        </Setter>
    </Style>
</ResourceDictionary>
```

이 예제에서는 **Style** 요소의 **TargetType** 특성이 **BgLabelControlApp** 네임스페이스 내에서 **BgLabelControl** 형식으로 설정되어 있음을 확인할 수 있습니다. 이 형식은 컨트롤의 기본 스타일로 식별하는 컨트롤 생성자의 **DefaultStyleKey** 속성에 대해 위에서 지정한 것과 동일한 값입니다.

컨트롤 템플릿에 있는 **TextBlock**의 **Text** 속성은 컨트롤의 **Label** 종속성 속성과 바인딩됩니다. 이 속성은 [TemplateBinding](/windows/uwp/xaml-platform/templatebinding-markup-extension) 태그 확장을 사용하여 바인딩됩니다. 또한 이 예제에서는 **Grid** 배경을 **Control** 클래스에서 상속된 **Background** 종속성 속성에 바인딩합니다.

## <a name="add-an-instance-of-bglabelcontrol-to-the-main-ui-page"></a>주 UI 페이지에 BgLabelControl 인스턴스 추가

`MainPage.xaml`을 엽니다. 여기에는 기본 UI 페이지에 사용할 XAML 태그가 포함되어 있습니다. **StackPanel** 내부에서 **Button** 요소 바로 뒤에 다음 태그를 추가합니다.

```xaml
<local:BgLabelControl Background="Red" Label="Hello, World!"/>
```

앱을 만들고 실행하면 지정된 배경색과 레이블이 있는 템플릿 기반 컨트롤이 표시됩니다.

![템플릿 기반 컨트롤 결과](images/winui-templated-control-result.png)


