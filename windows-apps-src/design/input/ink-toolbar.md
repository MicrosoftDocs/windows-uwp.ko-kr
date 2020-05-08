---
Description: Windows 앱 잉크 앱에 기본 InkToolbar를 추가 하 고, InkToolbar에 사용자 지정 펜 단추를 추가 하 고, 사용자 지정 펜 단추를 사용자 지정 펜 정의에 바인딩합니다.
title: Windows 앱 앱에 InkToolbar 추가
label: Add an InkToolbar to a Windows app app
template: detail.hbs
keywords: Windows Ink, Windows Ink, DirectInk, InkPresenter, InkCanvas, InkToolbar, 유니버설 Windows 플랫폼, UWP, 사용자 조작, 입력
ms.date: 02/08/2017
ms.topic: article
ms.assetid: d888f75f-c2a0-4134-81db-907b5e24fcc5
ms.localizationpriority: medium
ms.openlocfilehash: e8076c9a9022cedbd66991ddf5d5b6bab1d57cc7
ms.sourcegitcommit: 0dee502484df798a0595ac1fe7fb7d0f5a982821
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 05/08/2020
ms.locfileid: "82968102"
---
# <a name="add-an-inktoolbar-to-a-windows-app-app"></a>Windows 앱 앱에 InkToolbar 추가



Windows 앱 앱에서 잉크를 활용 하는 두 가지 컨트롤 ( [**InkCanvas**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.inkcanvas) 및 [**inktoolbar**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.inktoolbar))이 있습니다.

[**InkCanvas**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.inkcanvas) 컨트롤은 기본 Windows Ink 기능을 제공 합니다. 펜 입력을 잉크 스트로크 (색 및 두께의 기본 설정 사용) 또는 지우기 스트로크로 렌더링 하는 데 사용 합니다.

> InkCanvas 구현에 대 한 자세한 내용은 [Windows 앱의 펜 및 스타일러스 상호 작용](pen-and-stylus-interactions.md)을 참조 하세요.

완전히 투명 한 오버레이로 InkCanvas는 잉크 스트로크 속성을 설정 하기 위한 기본 제공 UI를 제공 하지 않습니다. 기본 잉크 사용 환경을 변경 하려는 경우 사용자가 잉크 스트로크 속성을 설정 하 고 다른 사용자 지정 잉크 기능을 지원 하려면 다음 두 가지 옵션을 사용할 수 있습니다.

- 코드 숨김으로 InkCanvas에 바인딩된 기본 [**InkPresenter**](https://docs.microsoft.com/uwp/api/windows.ui.input.inking.inkpresenter) 개체를 사용 합니다.

  InkPresenter Api는 잉크 환경의 광범위 한 사용자 지정을 지원 합니다. 자세한 내용은 [Windows 앱의 펜 및 스타일러스 상호 작용](pen-and-stylus-interactions.md)을 참조 하세요.

- [**Inktoolbar**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.inktoolbar) 를 InkCanvas에 바인딩합니다. 기본적으로 InkToolbar는 스트로크 크기, 잉크 색 및 펜 팁과 같은 잉크 관련 기능을 활성화 하는 데 사용할 수 있는 사용자 지정 가능 하 고 확장 가능한 단추 모음을 제공 합니다.

  이 항목의 InkToolbar 모음에 대해 설명 합니다.

> **중요 한 api**: [**InkCanvas 클래스**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.inkcanvas), [**inktoolbar 클래스**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.inktoolbar), [**InkPresenter 클래스**](https://docs.microsoft.com/uwp/api/windows.ui.input.inking.inkpresenter) [**,**](https://docs.microsoft.com/uwp/api/Windows.UI.Input.Inking)

## <a name="default-inktoolbar"></a>기본 InkToolbar

기본적으로 [**Inktoolbar**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.inktoolbar) 에는 스텐실 그리기, 지우기, 강조 표시 및 표시를 위한 단추 (눈금자 또는 protractor)가 포함 되어 있습니다. 기능에 따라 잉크 색, 스트로크 두께, 모든 잉크 지우기와 같은 기타 설정 및 명령이 플라이 아웃에 제공 됩니다.

![InkToolbar](./images/ink/ink-tools-invoked-toolbar-small.png)  
*기본 Windows Ink 도구 모음*

잉크 앱에 기본 [**Inktoolbar**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.inktoolbar) 를 추가 하려면 [**InkCanvas**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.inkcanvas) 와 동일한 페이지에 놓고 두 컨트롤을 연결 하면 됩니다.

1. MainPage에서 잉크 표면의 컨테이너 개체를 선언 합니다 .이 예제에서는 Grid 컨트롤을 사용 합니다.
2. InkCanvas 개체를 컨테이너의 자식으로 선언 합니다. InkCanvas 크기는 컨테이너에서 상속 됩니다.
3. InkToolbar를 선언 하 고 TargetInkCanvas 특성을 사용 하 여 InkCanvas에 바인딩합니다.

> [!NOTE]
> InkCanvas 후에 InkToolbar가 선언 되었는지 확인 합니다. 그렇지 않으면 InkCanvas 오버레이가 InkToolbar를 액세스할 수 없게 렌더링 합니다.

```xaml
<Grid Background="{ThemeResource ApplicationPageBackgroundThemeBrush}">
    <Grid.RowDefinitions>
        <RowDefinition Height="Auto"/>
        <RowDefinition Height="*"/>
    </Grid.RowDefinitions>
    <StackPanel x:Name="HeaderPanel" Orientation="Horizontal" Grid.Row="0">
        <TextBlock x:Name="Header"
                   Text="Basic ink sample"
                   Style="{ThemeResource HeaderTextBlockStyle}"
                   Margin="10,0,0,0" />
    </StackPanel>
    <Grid Grid.Row="1">
        <Image Source="Assets\StoreLogo.png" />
        <InkCanvas x:Name="inkCanvas" />
        <InkToolbar x:Name="inkToolbar"
          VerticalAlignment="Top"
          TargetInkCanvas="{x:Bind inkCanvas}" />
    </Grid>
</Grid>
```

## <a name="basic-customization"></a>기본 사용자 지정

이 섹션에서는 몇 가지 기본적인 Windows Ink 도구 모음 사용자 지정 시나리오를 다룹니다.

### <a name="specify-location-and-orientation"></a>위치 및 방향 지정

앱에 잉크 도구 모음을 추가 하는 경우 도구 모음의 기본 위치 및 orientaion 적용 하거나 앱 또는 사용자가 요구 하는 대로 설정할 수 있습니다.

**XAML**

[VerticalAlignment](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.VerticalAlignment), [HorizontalAlignment](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.HorizontalAlignment)및 [orientation](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.inktoolbar?branch=rs3.Orientation) 속성을 통해 도구 모음의 위치와 방향을 명시적으로 지정 합니다.

| 기본 | Explicit |
| --- | --- |
| ![기본 잉크 도구 모음 위치 및 방향](./images/ink/location-default-small.png) | ![명시적 잉크 도구 모음 위치 및 방향](./images/ink/location-explicit-small.png) |
| *Windows 잉크 도구 모음 기본 위치 및 방향* | *Windows Ink 도구 모음 명시적 위치 및 방향* |

XAML에서 잉크 도구 모음의 위치와 방향을 명시적으로 설정 하는 코드는 다음과 같습니다.
```xaml
<InkToolbar x:Name="inkToolbar" 
    VerticalAlignment="Center" 
    HorizontalAlignment="Right" 
    Orientation="Vertical" 
    TargetInkCanvas="{x:Bind inkCanvas}" />
```

**사용자 기본 설정 또는 장치 상태에 따라 초기화**

경우에 따라 사용자 기본 설정 또는 장치 상태에 따라 잉크 도구 모음의 위치와 방향을 설정 하는 것이 좋습니다. 다음 예제에서는 **설정 > 장치 > 펜 & Windows ink > > 펜**을 통해 지정 된 왼쪽 또는 오른쪽 쓰기 기본 설정에 따라 잉크 도구 모음의 위치와 방향을 설정 하는 방법을 보여 줍니다 .이를 사용 하 여 작성할 손을 선택할 수 있습니다.

![기준 설정](./images/ink/location-handedness-setting.png)  
*기준 설정*

이 설정은 HorizontalAlignment 관리의 수동 기본 설정 속성을 사용 하 여 쿼리하고 반환 된 값을 기준으로 하 여 [HorizontalAlignment](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.HorizontalAlignment) 를 설정할 수 있습니다. 이 예제에서는 왼손 사용자를 위해 앱의 왼쪽에 있는 도구 모음을 찾고 오른쪽에는 오른손 사용자를 위한 도구 모음을 찾습니다.

**[잉크 도구 모음 위치 및 방향 샘플](https://github.com/MicrosoftDocs/windows-topic-specific-samples/archive/uwp-ink-toolbar-handedness.zip) 에서이 샘플 다운로드 (기본)**

```csharp
public MainPage()
{
    this.InitializeComponent();

    Windows.UI.ViewManagement.UISettings settings = 
        new Windows.UI.ViewManagement.UISettings();
    HorizontalAlignment alignment = 
        (settings.HandPreference == 
            Windows.UI.ViewManagement.HandPreference.LeftHanded) ? 
            HorizontalAlignment.Left : HorizontalAlignment.Right;
    inkToolbar.HorizontalAlignment = alignment;
}
```

**사용자 또는 장치 상태를 동적으로 조정**

바인딩을 사용 하 여 사용자 기본 설정, 장치 설정 또는 장치 상태에 대 한 변경 내용에 따라 UI 업데이트 후를 확인할 수도 있습니다. 다음 예제에서는 이전 예제를 확장 하 고 바인딩, ViewMOdel 개체 및 [INotifyPropertyChanged](https://docs.microsoft.com/uwp/api/windows.ui.xaml.data.inotifypropertychanged) 인터페이스를 사용 하 여 장치 방향에 따라 잉크 도구 모음을 동적으로 배치 하는 방법을 보여 줍니다. 

**[잉크 도구 모음 위치 및 방향 샘플 (동적)](https://github.com/MicrosoftDocs/windows-topic-specific-samples/archive/uwp-ink-toolbar-handedness-dynamic.zip) 에서이 샘플 다운로드**

1. 먼저 ViewModel을 추가 해 보겠습니다.
    1. 프로젝트에 새 폴더를 추가 하 고이 폴더에 **Viewmodels**를 호출 합니다.
    1. ViewModels 폴더에 새 클래스를 추가 합니다 (이 예에서는 **InkToolbarSnippetHostViewModel.cs**이라고 함).
        > [!NOTE] 
        > 응용 프로그램 수명 동안이 형식의 개체가 하나만 필요 하므로 [Singleton 패턴](https://docs.microsoft.com/previous-versions/msp-n-p/ff650849(v=pandp.10)) 을 사용 했습니다.

    1. 파일 `using System.ComponentModel` 에 네임 스페이스를 추가 합니다.
    1. **Instance**라는 정적 멤버 변수 **및 명명 된**정적 읽기 전용 속성을 추가 합니다. 생성자를 private으로 설정 하 여이 클래스를 Instance 속성을 통해서만 액세스할 수 있도록 합니다.   
        > [!NOTE] 
        > 이 클래스는 클라이언트 (일반적으로 클라이언트 바인딩)에 속성 값이 변경 되었음을 알리는 데 사용 되는 [INotifyPropertyChanged](https://docs.microsoft.com/uwp/api/windows.ui.xaml.data.inotifypropertychanged) 인터페이스에서 상속 됩니다. 이를 사용 하 여 장치 방향에 대 한 변경 내용을 처리 합니다 .이 코드를 확장 하 고 이후 단계에서 추가로 설명 합니다.  

        ```csharp
        using System.ComponentModel;

        namespace locationandorientation.ViewModels
        {
            public class InkToolbarSnippetHostViewModel : INotifyPropertyChanged
            {
                private static InkToolbarSnippetHostViewModel instance;
                
                public static InkToolbarSnippetHostViewModel Instance
                {
                    get
                    {
                        if (null == instance)
                        {
                            instance = new InkToolbarSnippetHostViewModel();
                        }
                        return instance;
                    }
                }
            }

            private InkToolbarSnippetHostViewModel() { }
        }
        ```

    1. InkToolbarSnippetHostViewModel 클래스에 두 개의 bool 속성을 추가 합니다. **LeftHandedLayout** (이전 XAML 전용 예제와 동일한 기능) 및 **PortraitLayout** (장치의 방향).
        >[!NOTE] 
        > PortraitLayout 속성은 설정할 수 있으며 [PropertyChanged](https://docs.microsoft.com/uwp/api/windows.ui.xaml.data.inotifypropertychanged.PropertyChanged) 이벤트에 대 한 정의를 포함 합니다.

        ```csharp
        public bool LeftHandedLayout
        {
            get
            {
                bool leftHandedLayout = false;
                Windows.UI.ViewManagement.UISettings settings =
                    new Windows.UI.ViewManagement.UISettings();
                leftHandedLayout = (settings.HandPreference ==
                    Windows.UI.ViewManagement.HandPreference.LeftHanded);
                return leftHandedLayout;
            }
        }

        public bool portraitLayout = false;
        public bool PortraitLayout
        {
            get
            {
                Windows.UI.ViewManagement.ApplicationViewOrientation winOrientation = 
                    Windows.UI.ViewManagement.ApplicationView.GetForCurrentView().Orientation;
                portraitLayout = 
                    (winOrientation == 
                        Windows.UI.ViewManagement.ApplicationViewOrientation.Portrait);
                return portraitLayout;
            }
            set
            {
                if (value.Equals(portraitLayout)) return;
                portraitLayout = value;
                PropertyChanged?.Invoke(this, new PropertyChangedEventArgs("PortraitLayout"));
            }
        }
        ```

1. 이제 프로젝트에 몇 가지 변환기 클래스를 추가 해 보겠습니다. 각 클래스에는 맞춤 값 ( [HorizontalAlignment](https://docs.microsoft.com/uwp/api/windows.ui.xaml.horizontalalignment) 또는 [VerticalAlignment](https://docs.microsoft.com/uwp/api/windows.ui.xaml.verticalalignment))을 반환 하는 Convert 개체가 포함 되어 있습니다.
    1. 프로젝트에 새 폴더를 추가 하 고 **변환기**를 호출 합니다.
    1. 변환기 폴더에 두 개의 새 클래스를 추가 합니다 .이 예제에서는 **HorizontalAlignmentFromHandednessConverter.cs** 및 **VerticalAlignmentFromAppViewConverter.cs**를 호출 합니다.
    1. 및 `using Windows.UI.Xaml` `using Windows.UI.Xaml.Data` 네임 스페이스를 각 파일에 추가 합니다.
    1. 각 클래스를로 `public` 변경 하 고 [ivalueconverter.convert](https://docs.microsoft.com/uwp/api/windows.ui.xaml.data.ivalueconverter) 인터페이스를 구현 하도록 지정 합니다.
    1. 여기에 표시 된 대로 [Convert](https://docs.microsoft.com/uwp/api/windows.ui.xaml.data.ivalueconverter.convert) 및 [convertback](https://docs.microsoft.com/uwp/api/windows.ui.xaml.data.ivalueconverter.convertback) 메서드를 각 파일에 추가 합니다 (convertback 메서드는 구현 되지 않음).
        - HorizontalAlignmentFromHandednessConverter 오른쪽 사용자에 대 한 앱의 오른쪽 및 왼손 사용자를 위한 앱의 왼쪽에 잉크 도구 모음을 배치 합니다.
        ```csharp
        using System;

        using Windows.UI.Xaml;
        using Windows.UI.Xaml.Data;

        namespace locationandorientation.Converters
        {
            public class HorizontalAlignmentFromHandednessConverter : IValueConverter
            {
                public object Convert(object value, Type targetType,
                    object parameter, string language)
                {
                    bool leftHanded = (bool)value;
                    HorizontalAlignment alignment = HorizontalAlignment.Right;
                    if (leftHanded)
                    {
                        alignment = HorizontalAlignment.Left;
                    }
                    return alignment;
                }

                public object ConvertBack(object value, Type targetType,
                    object parameter, string language)
                {
                    throw new NotImplementedException();
                }
            }
        }
        ```

        - VerticalAlignmentFromAppViewConverter 세로 방향으로 앱의 중앙에 잉크 도구 모음을 배치 하 고 가로 방향으로 앱의 위쪽에 배치 합니다. 즉, 유용성을 향상 시키기 위한 것 이며,이는 데모용 으로만 선택할 수 있습니다.
        ```csharp
        using System;

        using Windows.UI.Xaml;
        using Windows.UI.Xaml.Data;

        namespace locationandorientation.Converters
        {
            public class VerticalAlignmentFromAppViewConverter : IValueConverter
            {
                public object Convert(object value, Type targetType,
                    object parameter, string language)
                {
                    bool portraitOrientation = (bool)value;
                    VerticalAlignment alignment = VerticalAlignment.Top;
                    if (portraitOrientation)
                    {
                        alignment = VerticalAlignment.Center;
                    }
                    return alignment;
                }

                public object ConvertBack(object value, Type targetType,
                    object parameter, string language)
                {
                    throw new NotImplementedException();
                }
            }
        }
        ```

1. 이제 MainPage.xaml.cs 파일을 엽니다.
    1. ViewModel `using using locationandorientation.ViewModels` 에 연결할 네임 스페이스 목록에를 추가 합니다.
    1. 장치 `using Windows.UI.ViewManagement` 방향에 대 한 변경 내용을 수신할 수 있도록 네임 스페이스 목록에를 추가 합니다.
    1. [WindowSizeChangedEventHandler](https://docs.microsoft.com/uwp/api/windows.ui.xaml.windowsizechangedeventhandler) 코드를 추가 합니다.
    1. 뷰의 [DataContext](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.FrameworkElement.DataContext) 를 InkToolbarSnippetHostViewModel 클래스의 singleton 인스턴스로 설정 합니다. 
    ```csharp
    using Windows.UI.Xaml;
    using Windows.UI.Xaml.Controls;

    using locationandorientation.ViewModels;
    using Windows.UI.ViewManagement;

    namespace locationandorientation
    {
        public sealed partial class MainPage : Page
        {
            public MainPage()
            {
                this.InitializeComponent();

                Window.Current.SizeChanged += (sender, args) =>
                {
                    ApplicationView currentView = ApplicationView.GetForCurrentView();

                    if (currentView.Orientation == ApplicationViewOrientation.Landscape)
                    {
                        InkToolbarSnippetHostViewModel.Instance.PortraitLayout = false;
                    }
                    else if (currentView.Orientation == ApplicationViewOrientation.Portrait)
                    {
                        InkToolbarSnippetHostViewModel.Instance.PortraitLayout = true;
                    }
                };

                DataContext = InkToolbarSnippetHostViewModel.Instance;
            }
        }
    }
    ```

1. 다음으로 MainPage .xaml 파일을 엽니다.
    1. 변환기 `xmlns:converters="using:locationandorientation.Converters"` 에 바인딩하기 `Page` 위해 요소에를 추가 합니다.
        ```xaml
        <Page
        x:Class="locationandorientation.MainPage"
        xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
        xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
        xmlns:local="using:locationandorientation"
        xmlns:converters="using:locationandorientation.Converters"
        xmlns:d="http://schemas.microsoft.com/expression/blend/2008"
        xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006"
        mc:Ignorable="d">
        ```

    1. 요소를 `PageResources` 추가 하 고 변환기에 대 한 참조를 지정 합니다.
        ```xaml
        <Page.Resources>
            <converters:HorizontalAlignmentFromHandednessConverter x:Key="HorizontalAlignmentConverter"/>
            <converters:VerticalAlignmentFromAppViewConverter x:Key="VerticalAlignmentConverter"/>
        </Page.Resources>
        ```

    1. InkCanvas 및 InkToolbar 요소를 추가 하 고 InkToolbar의 VerticalAlignment 및 HorizontalAlignment 속성을 바인딩합니다.
        ```xaml
        <InkCanvas x:Name="inkCanvas" />
        <InkToolbar x:Name="inkToolbar" 
                    VerticalAlignment="{Binding PortraitLayout, Converter={StaticResource VerticalAlignmentConverter} }" 
                    HorizontalAlignment="{Binding LeftHandedLayout, Converter={StaticResource HorizontalAlignmentConverter} }" 
                    Orientation="Vertical" 
                    TargetInkCanvas="{x:Bind inkCanvas}" />
        ```

1. InkToolbarSnippetHostViewModel.cs 파일로 돌아와서 속성 값이 변경 될 `PortraitLayout` 때 `LeftHandedLayout` 다시 바인딩 `PortraitLayout` 지원과 함께 `InkToolbarSnippetHostViewModel` 및 bool 속성을 클래스에 추가 합니다. 
    ```csharp
    public bool LeftHandedLayout
    {
        get
        {
            bool leftHandedLayout = false;
            Windows.UI.ViewManagement.UISettings settings =
                new Windows.UI.ViewManagement.UISettings();
            leftHandedLayout = (settings.HandPreference ==
                Windows.UI.ViewManagement.HandPreference.LeftHanded);
            return leftHandedLayout;
        }
    }

    public bool portraitLayout = false;
    public bool PortraitLayout
    {
        get
        {
            Windows.UI.ViewManagement.ApplicationViewOrientation winOrientation = 
                Windows.UI.ViewManagement.ApplicationView.GetForCurrentView().Orientation;
            portraitLayout = 
                (winOrientation == 
                    Windows.UI.ViewManagement.ApplicationViewOrientation.Portrait);
            return portraitLayout;
        }
        set
        {
            if (value.Equals(portraitLayout)) return;
            portraitLayout = value;
            PropertyChanged?.Invoke(this, new PropertyChangedEventArgs("PortraitLayout"));
        }
    }

    #region INotifyPropertyChanged Members

    public event PropertyChangedEventHandler PropertyChanged;

    protected void OnPropertyChanged(string property)
    {
        PropertyChanged?.Invoke(this, new PropertyChangedEventArgs(property));
    }

    #endregion
    ```

이제 사용자의 주요 기본 설정에 맞게 조정 되 고 사용자 장치의 방향에 동적으로 응답 하는 잉크 앱이 있습니다.

### <a name="specify-the-selected-button"></a>선택한 단추 지정  
![초기화 시 연필 단추가 선택 됨](./images/ink/ink-tools-default-toolbar.png)  
*초기화할 때 연필 단추가 선택 된 Windows Ink 도구 모음*

기본적으로 첫 번째 (또는 가장 왼쪽) 단추는 앱이 시작 되 고 도구 모음이 초기화 될 때 선택 됩니다. 기본 Windows 잉크 도구 모음에서 볼펜 단추를 클릭 합니다.

프레임 워크는 기본 제공 단추의 순서를 정의 하기 때문에 첫 번째 단추는 기본적으로 활성화 하려는 펜 또는 도구가 아닐 수 있습니다.

이 기본 동작을 재정의 하 고 도구 모음에서 선택한 단추를 지정할 수 있습니다.

이 예에서는 연필 단추가 선택 되 고, 볼펜이 아닌 연필이 활성화 된 상태로 기본 도구 모음을 초기화 합니다.

1. 이전 예제에서 InkCanvas 및 InkToolbar 모음에 대 한 XAML 선언을 사용 합니다.
2. 코드 숨김으로 [Inktoolbar](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.inktoolbar) 개체의 [로드](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.loaded) 된 이벤트에 대 한 처리기를 설정 합니다.

  ```csharp
  /// <summary>
  /// An empty page that can be used on its own or navigated to within a Frame.
  /// Here, we set up InkToolbar event listeners.
  /// </summary>
  public MainPage_CodeBehind()
  {
      this.InitializeComponent();
      // Add handlers for InkToolbar events.
      inkToolbar.Loaded += inkToolbar_Loaded;
  }
  ```

3. [로드](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.loaded) 된 이벤트에 대 한 처리기에서 다음을 수행 합니다.
    1. 기본 제공 [InkToolbarPencilButton](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.inktoolbarpencilbutton)에 대 한 참조를 가져옵니다.

    [Ink도구를 전달 합니다.](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.inktoolbartool) [gettoolbutton](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.inktoolbar.gettoolbutton) 메서드의 연필 개체는 [InkToolbarPencilButton](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.inktoolbarpencilbutton)에 대 한 [inktooltoolbutton](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.inktoolbartoolbutton) 개체를 반환 합니다.

    2. [ActiveTool](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.inktoolbar.activetool) 을 이전 단계에서 반환 된 개체로 설정 합니다.

```CSharp
/// <summary>
/// Handle the Loaded event of the InkToolbar.
/// By default, the active tool is set to the first tool on the toolbar.
/// Here, we set the active tool to the pencil button.
/// </summary>
/// <param name="sender"></param>
/// <param name="e"></param>
private void inkToolbar_Loaded(object sender, RoutedEventArgs e)
{
    InkToolbarToolButton pencilButton = inkToolbar.GetToolButton(InkToolbarTool.Pencil);
    inkToolbar.ActiveTool = pencilButton;
}
```

### <a name="specify-the-built-in-buttons"></a>기본 제공 단추 지정

![초기화 시 포함 되는 특정 단추](./images/ink/ink-tools-specific.png)  
*초기화 시 포함 되는 특정 단추*

앞에서 설명한 것 처럼 Windows Ink 도구 모음에는 기본 제공 단추 모음이 포함 되어 있습니다. 이러한 단추는 다음 순서 (왼쪽에서 오른쪽으로)로 표시 됩니다.

- [InkToolbarBallpointPenButton](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.inktoolbarballpointpenbutton)
- [InkToolbarPencilButton](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.inktoolbarpencilbutton)
- [InkToolbarHighlighterButton](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.inktoolbarhighlighterbutton)
- [InkToolbarEraserButton](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.inktoolbareraserbutton)
- [InkToolbarRulerButton](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.inktoolbarrulerbutton)

이 예제에서는 기본 제공 볼펜, 연필 및 지우개 단추만 포함 하는 도구 모음을 초기화 합니다.

XAML 또는 코드 숨김으로 사용 하 여이 작업을 수행할 수 있습니다.

**XAML**

첫 번째 예제에서 InkCanvas 및 InkToolbar의 XAML 선언을 수정 합니다.
- [Initialcontrols](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.inktoolbar.initialcontrols) 특성을 추가 하 고 해당 값을 "[None](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.inktoolbarinitialcontrols)"으로 설정 합니다. 기본 제공 단추의 기본 컬렉션이 지워집니다.
- 앱에 필요한 특정 InkToolbar 단추를 추가 합니다. 여기에서 [InkToolbarBallpointPenButton](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.inktoolbarballpointpenbutton), [InkToolbarPencilButton](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.inktoolbarpencilbutton)및 [InkToolbarEraserButton](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.inktoolbareraserbutton) only를 추가 합니다.
> [!NOTE]
> 단추는 프레임 워크에 정의 된 순서 대로 도구 모음에 추가 되며 여기에 지정 된 순서는 아닙니다.

```xaml
<Grid Background="{ThemeResource ApplicationPageBackgroundThemeBrush}">
    <Grid.RowDefinitions>
        <RowDefinition Height="Auto"/>
        <RowDefinition Height="*"/>
    </Grid.RowDefinitions>
    <StackPanel x:Name="HeaderPanel" Orientation="Horizontal" Grid.Row="0">
        <TextBlock x:Name="Header"
                   Text="Basic ink sample"
                   Style="{ThemeResource HeaderTextBlockStyle}"
                   Margin="10,0,0,0" />
    </StackPanel>
    <Grid Grid.Row="1">
        <Image Source="Assets\StoreLogo.png" />
        <!-- Clear the default InkToolbar buttons by setting InitialControls to None. -->
        <!-- Set the active tool to the pencil button. -->
        <InkCanvas x:Name="inkCanvas" />
        <InkToolbar x:Name="inkToolbar"
                    VerticalAlignment="Top"
                    TargetInkCanvas="{x:Bind inkCanvas}"
                    InitialControls="None">
            <!--
             Add only the ballpoint pen, pencil, and eraser.
             Note that the buttons are added to the toolbar in the order
             defined by the framework, not the order we specify here.
            -->
            <InkToolbarEraserButton />
            <InkToolbarBallpointPenButton />
            <InkToolbarPencilButton/>
        </InkToolbar>
    </Grid>
</Grid>
```

**코드 숨김**
1. 첫 번째 예제의 InkCanvas 및 InkToolbar 모음에 대 한 XAML 선언을 사용 합니다.

  ```xaml
  <Grid Background="{ThemeResource ApplicationPageBackgroundThemeBrush}">
      <Grid.RowDefinitions>
          <RowDefinition Height="Auto"/>
          <RowDefinition Height="*"/>
      </Grid.RowDefinitions>
      <StackPanel x:Name="HeaderPanel" Orientation="Horizontal" Grid.Row="0">
          <TextBlock x:Name="Header"
                     Text="Basic ink sample"
                     Style="{ThemeResource HeaderTextBlockStyle}"
                     Margin="10,0,0,0" />
      </StackPanel>
      <Grid Grid.Row="1">
          <Image Source="Assets\StoreLogo.png" />
          <InkCanvas x:Name="inkCanvas" />
          <InkToolbar x:Name="inkToolbar"
          VerticalAlignment="Top"
          TargetInkCanvas="{x:Bind inkCanvas}" />
      </Grid>
  </Grid>
  ```

2. 코드 숨김으로 [Inktoolbar](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.inktoolbar) 개체의 [로드](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.loading) 이벤트에 대 한 처리기를 설정 합니다.

  ```csharp
  /// <summary>
  /// An empty page that can be used on its own or navigated to within a Frame.
  /// Here, we set up InkToolbar event listeners.
  /// </summary>
  public MainPage_CodeBehind()
  {
      this.InitializeComponent();
      // Add handlers for InkToolbar events.
      inkToolbar.Loading += inkToolbar_Loading;
  }
  ```

3. [Initialcontrols](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.inktoolbar.initialcontrols) 를 "[None](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.inktoolbarinitialcontrols)"으로 설정 합니다.
4. 앱에 필요한 단추에 대 한 개체 참조를 만듭니다. 여기에서 [InkToolbarBallpointPenButton](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.inktoolbarballpointpenbutton), [InkToolbarPencilButton](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.inktoolbarpencilbutton)및 [InkToolbarEraserButton](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.inktoolbareraserbutton) only를 추가 합니다.
  > [!NOTE]
  > 단추는 프레임 워크에 정의 된 순서 대로 도구 모음에 추가 되며 여기에 지정 된 순서는 아닙니다.

5. 단추를 InkToolbar에 [추가](https://docs.microsoft.com/uwp/api/windows.ui.xaml.dependencyobjectcollection.add) 합니다.

  ```csharp
  /// <summary>
  /// Handles the Loading event of the InkToolbar.
  /// Here, we identify the buttons to include on the InkToolbar.
  /// </summary>
  /// <param name="sender">The InkToolbar</param>
  /// <param name="args">The InkToolbar event data.
  /// If there is no event data, this parameter is null</param>
  private void inkToolbar_Loading(FrameworkElement sender, object args)
  {
      // Clear all built-in buttons from the InkToolbar.
      inkToolbar.InitialControls = InkToolbarInitialControls.None;

      // Add only the ballpoint pen, pencil, and eraser.
      // Note that the buttons are added to the toolbar in the order
      // defined by the framework, not the order we specify here.
      InkToolbarBallpointPenButton ballpoint = new InkToolbarBallpointPenButton();
      InkToolbarPencilButton pencil = new InkToolbarPencilButton();
      InkToolbarEraserButton eraser = new InkToolbarEraserButton();
      inkToolbar.Children.Add(eraser);
      inkToolbar.Children.Add(ballpoint);
      inkToolbar.Children.Add(pencil);
  }
  ```

<!--
### Support touch input
By default, the InkToolbar supports both pen and mouse input, you have to enable support for touch input.
-->

## <a name="custom-buttons-and-inking-features"></a>사용자 지정 단추 및 잉크 기능

InkToolbar 모음을 통해 제공 되는 단추 및 연결 된 잉크 기능 모음을 사용자 지정 하 고 확장할 수 있습니다.

InkToolbar는 두 개의 고유한 단추 유형 그룹으로 구성 됩니다.

1. 기본 제공 그리기, 지우기 및 강조 표시 단추를 포함 하는 "도구" 단추 그룹입니다. 사용자 지정 펜 및 도구가 여기에 추가 됩니다.
> **Note**&nbsp;참고&nbsp;기능 선택은 함께 사용할 수 없습니다.

2. 기본 제공 눈금자 단추를 포함 하는 "토글" 단추 그룹입니다. 사용자 지정 토글이 여기에 추가 됩니다.
> **Note**&nbsp;참고&nbsp;기능은 함께 사용할 수 없으며 다른 활성 도구와 동시에 사용할 수 있습니다.

필요한 응용 프로그램 및 잉크 기능에 따라 다음과 같은 단추 (사용자 지정 잉크 기능에 바인딩)를 InkToolbar 모음에 추가할 수 있습니다.

- 사용자 지정 펜 – 잉크 색 색상표 및 펜 팁 속성 (예: 셰이프, 회전 및 크기)이 호스트 앱에서 정의 되는 펜입니다.
- 사용자 지정 도구 – 호스트 앱에서 정의 하는 비 펜 도구입니다.
- 사용자 지정 토글 – 앱 정의 기능의 상태를 설정 또는 해제로 설정 합니다. 이 기능을 켜면 활성 도구와 함께 작동 합니다.

> **Note**&nbsp;참고&nbsp;기본 제공 단추의 표시 순서는 변경할 수 없습니다. 기본 표시 순서는 볼펜, 연필, 형광펜, 지우개 및 눈금자입니다. 사용자 지정 펜이 마지막 기본 펜에 추가 되 고, 사용자 지정 도구 단추가 마지막 펜 단추 사이에 추가 되 고, 지우개 단추와 사용자 지정 토글 단추가 눈금자 단추 뒤에 추가 됩니다. 사용자 지정 단추는 지정 된 순서 대로 추가 됩니다.

### <a name="custom-pen"></a>사용자 지정 펜

잉크 색상표와 펜 팁 속성 (예: 모양, 회전 및 크기)을 정의 하는 사용자 지정 펜 (사용자 지정 펜 단추를 통해 활성화)을 만들 수 있습니다.

![사용자 지정 붓글씨 펜 단추](./images/ink/ink-tools-custompen.png)  
*사용자 지정 붓글씨 펜 단추*

이 예제에서는 기본 붓글씨 잉크 스트로크를 사용 하는 광범위 한 팁을 사용 하 여 사용자 지정 펜을 정의 합니다. 또한 단추 플라이 아웃에 표시 된 색상표에서 브러시의 컬렉션을 사용자 지정 합니다.

**코드 숨김**

먼저 사용자 지정 펜을 정의 하 고 코드 숨김으로 그리기 특성을 지정 합니다. XAML에서 나중에이 사용자 지정 펜을 참조 합니다.

1. 솔루션 탐색기에서 프로젝트를 마우스 오른쪽 단추로 클릭 하 고 추가-> 새 항목을 선택 합니다.
2. Visual c #-> 코드에서 새 클래스 파일을 추가 하 고 CalligraphicPen.cs를 호출 합니다.
3. Calligraphic.cs에서 기본 using 블록을 다음으로 바꿉니다.
```csharp
using System.Numerics;
using Windows.UI;
using Windows.UI.Input.Inking;
using Windows.UI.Xaml.Controls;
using Windows.UI.Xaml.Media;
```

4. CalligraphicPen 클래스가 [InkToolbarCustomPen](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.inktoolbarcustompen)에서 파생 되도록 지정 합니다.
```csharp
class CalligraphicPen : InkToolbarCustomPen
{
}
```

5. [CreateInkDrawingAttributesCore](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.inktoolbarcustompen.createinkdrawingattributescore) 를 재정의 하 여 고유한 브러시와 스트로크 크기를 지정 합니다.
```csharp
class CalligraphicPen : InkToolbarCustomPen
{
    protected override InkDrawingAttributes
      CreateInkDrawingAttributesCore(Brush brush, double strokeWidth)
    {
    }
}
```

6. [InkDrawingAttributes](https://docs.microsoft.com/uwp/api/windows.ui.input.inking.inkdrawingattributes) 개체를 만들고 [펜 팁 셰이프](https://docs.microsoft.com/uwp/api/windows.ui.input.inking.inkdrawingattributes.pentip), [팁 회전](https://docs.microsoft.com/uwp/api/windows.ui.input.inking.inkdrawingattributes.pentiptransform), [스트로크 크기](https://docs.microsoft.com/uwp/api/windows.ui.input.inking.inkdrawingattributes.size)및 [잉크 색](https://docs.microsoft.com/uwp/api/windows.ui.input.inking.inkdrawingattributes.color)을 설정 합니다.
```csharp
class CalligraphicPen : InkToolbarCustomPen
{
    protected override InkDrawingAttributes
      CreateInkDrawingAttributesCore(Brush brush, double strokeWidth)
    {
        InkDrawingAttributes inkDrawingAttributes =
          new InkDrawingAttributes();
        inkDrawingAttributes.PenTip = PenTipShape.Circle;
        inkDrawingAttributes.Size =
          new Windows.Foundation.Size(strokeWidth, strokeWidth * 20);
        SolidColorBrush solidColorBrush = brush as SolidColorBrush;
        if (solidColorBrush != null)
        {
            inkDrawingAttributes.Color = solidColorBrush.Color;
        }
        else
        {
            inkDrawingAttributes.Color = Colors.Black;
        }

        Matrix3x2 matrix = Matrix3x2.CreateRotation(45);
        inkDrawingAttributes.PenTipTransform = matrix;

        return inkDrawingAttributes;
    }
}
```

**XAML**

다음으로는 사용자 지정 펜에 필요한 참조를 MainPage에 추가 합니다.

1. CalligraphicPen.cs에 정의 된 사용자 지정 펜 (`CalligraphicPen`)에 대 한 참조를 만드는 로컬 페이지 리소스 사전을 선언 하 고 사용자 지정 펜 (`CalligraphicPenPalette`)에서 지원 되는 [브러시 컬렉션](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.BrushCollection) 을 선언 합니다.
```xaml
<Page.Resources>
    <!-- Add the custom CalligraphicPen to the page resources. -->
    <local:CalligraphicPen x:Key="CalligraphicPen" />
    <!-- Specify the colors for the palette of the custom pen. -->
    <BrushCollection x:Key="CalligraphicPenPalette">
        <SolidColorBrush Color="Blue" />
        <SolidColorBrush Color="Red" />
    </BrushCollection>
</Page.Resources>
```

2. 그런 다음 자식 [InkToolbarCustomPenButton](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.inktoolbarcustompenbutton) 요소를 사용 하 여 inktoolbar를 추가 합니다.

  사용자 지정 펜 단추에는 페이지 리소스에 선언 된 두 개의 정적 리소스 참조 `CalligraphicPen` 인 `CalligraphicPenPalette`및가 포함 됩니다.

  또한 스트로크 크기 슬라이더 ([MinStrokeWidth](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.inktoolbarpenbutton.minstrokewidth), [MaxStrokeWidth](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.inktoolbarpenbutton.maxstrokewidth)및 [SelectedStrokeWidth](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.inktoolbarpenbutton.selectedstrokewidthproperty)), 선택한 브러시 ([SelectedBrushIndex](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.inktoolbarpenbutton.selectedbrushindex)) 및 사용자 지정 펜 단추 ([기호 아이콘](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.symbolicon))의 아이콘에 대 한 범위를 지정 합니다.
```xaml
<Grid Grid.Row="1">
    <InkCanvas x:Name="inkCanvas" />
    <InkToolbar x:Name="inkToolbar"
                VerticalAlignment="Top"
                TargetInkCanvas="{x:Bind inkCanvas}">
        <InkToolbarCustomPenButton
            CustomPen="{StaticResource CalligraphicPen}"
            Palette="{StaticResource CalligraphicPenPalette}"
            MinStrokeWidth="1" MaxStrokeWidth="3" SelectedStrokeWidth="2"
            SelectedBrushIndex ="1">
            <SymbolIcon Symbol="Favorite" />
            <InkToolbarCustomPenButton.ConfigurationContent>
                <InkToolbarPenConfigurationControl />
            </InkToolbarCustomPenButton.ConfigurationContent>
        </InkToolbarCustomPenButton>
    </InkToolbar>
</Grid>
```

### <a name="custom-toggle"></a>사용자 지정 토글

사용자 지정 토글 (사용자 지정 토글 단추를 통해 활성화)을 만들어 앱 정의 기능의 상태를 on 또는 off로 설정할 수 있습니다. 이 기능을 켜면 활성 도구와 함께 작동 합니다.

이 예제에서는 터치 입력으로 잉크를 사용 하도록 설정 하는 사용자 지정 토글 단추를 정의 합니다 (기본적으로 터치 잉크는 사용 되지 않음).

> [!NOTE]  
> 터치를 사용 하 여 잉크를 지원 해야 하는 경우에는이 예제에 지정 된 아이콘 및 도구 설명을 사용 하 여 CustomToggleButton을 사용 하도록 설정 하는 것이 좋습니다.

일반적으로 터치식 입력은 개체 또는 앱 UI를 직접 조작 하는 데 사용 됩니다. 터치 잉크를 사용 하는 경우 동작의 차이점을 보여 주기 위해 ScrollViewer 컨테이너 내에 InkCanvas를 넣고 ScrollViewer의 차원을 InkCanvas 보다 작게 설정 합니다. 

앱이 시작 되 면 잉크 화면을 이동 하거나 확대/축소 하는 데 펜 입력만 지원 되 고 터치가 사용 됩니다. 터치 잉크를 사용 하는 경우 터치 입력을 통해 잉크 화면을 따라 이동 하거나 확대/축소할 수 없습니다.

> [!NOTE]
> [**InkCanvas**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.InkCanvas) 및 [**inktoolbar 모음**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.InkToolbar) UX 지침 모두에 대 한 [잉크 컨트롤](../controls-and-patterns/inking-controls.md) 을 참조 하세요. 다음은이 예제와 관련 된 권장 사항입니다.
> - [**Inktoolbar 모음**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.InkToolbar)및 일반적인 잉크 입력은 활성 펜을 통해 잘 경험 합니다. 그러나 앱에 필요한 경우 마우스 및 터치를 사용 하 여 잉크를 지원할 수 있습니다. 
> - 터치 입력을 사용 하 여 잉크를 지 원하는 경우 "터치 쓰기" 도구 설명을 사용 하 여 설정/해제 단추에 대 한 "Segoe MLD2 자산" 글꼴의 "ED5F" 아이콘을 사용 하는 것이 좋습니다. 

**XAML**

1. 먼저 이벤트 처리기 (Toggle_Custom)를 지정 하는 클릭 이벤트 수신기를 사용 하 여 [**Inktogglebutton customtogglebutton**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.InkToolbarCustomToggleButton) 요소 (toggleButton)를 선언 합니다.

```xaml 
<Grid Background="{ThemeResource ApplicationPageBackgroundThemeBrush}">
    <Grid.RowDefinitions>
        <RowDefinition Height="Auto"/>
        <RowDefinition Height="*"/>
    </Grid.RowDefinitions>

    <StackPanel Grid.Row="0" 
                x:Name="HeaderPanel" 
                Orientation="Horizontal">
        <TextBlock x:Name="Header" 
                   Text="Basic ink sample" 
                   Style="{ThemeResource HeaderTextBlockStyle}" 
                   Margin="10" />
    </StackPanel>

    <ScrollViewer Grid.Row="1" 
                  HorizontalScrollBarVisibility="Auto" 
                  VerticalScrollBarVisibility="Auto">
        
        <Grid HorizontalAlignment="Left" VerticalAlignment="Top">
            <Grid.RowDefinitions>
                <RowDefinition Height="Auto"/>
                <RowDefinition Height="*"/>
            </Grid.RowDefinitions>
            
            <InkToolbar Grid.Row="0" 
                        Margin="10"
                        x:Name="inkToolbar" 
                        VerticalAlignment="Top"
                        TargetInkCanvas="{x:Bind inkCanvas}">
                <InkToolbarCustomToggleButton 
                x:Name="toggleButton" 
                Click="CustomToggle_Click" 
                ToolTipService.ToolTip="Touch Writing">
                    <SymbolIcon Symbol="{x:Bind TouchWritingIcon}"/>
                </InkToolbarCustomToggleButton>
            </InkToolbar>
            
            <ScrollViewer Grid.Row="1" 
                          Height="500"
                          Width="500"
                          x:Name="scrollViewer" 
                          ZoomMode="Enabled" 
                          MinZoomFactor=".1" 
                          VerticalScrollMode="Enabled" 
                          VerticalScrollBarVisibility="Auto" 
                          HorizontalScrollMode="Enabled" 
                          HorizontalScrollBarVisibility="Auto">
                
                <Grid x:Name="outputGrid" 
                      Height="1000"
                      Width="1000"
                      Background="{ThemeResource SystemControlBackgroundChromeWhiteBrush}">
                    <InkCanvas x:Name="inkCanvas"/>
                </Grid>
                
            </ScrollViewer>
        </Grid>
    </ScrollViewer>
</Grid>
```

**코드 숨김**

2. 위의 코드 조각에서는 터치 필기 (toggleButton)의 사용자 지정 토글 단추에서 클릭 이벤트 수신기 및 처리기 (Toggle_Custom)를 선언 했습니다. 이 처리기는 InkPresenter의 InputDeviceTypes 속성을 통해 CoreInputDeviceTypes에 대 한 지원을 단순히 설정/해제 합니다.

   또한 기호 아이콘 요소를 사용 하 여 단추에 대 한 아이콘을 지정 하 고 코드 파일 (TouchWritingIcon)에 정의 된 필드에 바인딩하는 {x:Bind} 태그 확장을 지정 했습니다.

   다음 코드 조각에는 Click 이벤트 처리기와 TouchWritingIcon의 정의가 모두 포함 되어 있습니다.

```csharp 
namespace Ink_Basic_InkToolbar
{
    /// <summary>
    /// An empty page that can be used on its own or navigated to within a Frame.
    /// </summary>
    public sealed partial class MainPage_AddCustomToggle : Page
    {
        Symbol TouchWritingIcon = (Symbol)0xED5F;

        public MainPage_AddCustomToggle()
        {
            this.InitializeComponent();
        }

        // Handler for the custom toggle button that enables touch inking.
        private void CustomToggle_Click(object sender, RoutedEventArgs e)
        {
            if (toggleButton.IsChecked == true)
            {
                inkCanvas.InkPresenter.InputDeviceTypes |= CoreInputDeviceTypes.Touch;
            }
            else
            {
                inkCanvas.InkPresenter.InputDeviceTypes &= ~CoreInputDeviceTypes.Touch;
            }
        }
    }
}
```

### <a name="custom-tool"></a>사용자 지정 도구

사용자 지정 도구 단추를 만들어 앱에서 정의한 비 펜 도구를 호출할 수 있습니다.

기본적으로 [**InkPresenter**](https://docs.microsoft.com/uwp/api/Windows.UI.Input.Inking.InkPresenter) 는 모든 입력을 잉크 스트로크 또는 지우기 스트로크로 처리 합니다. 여기에는 펜 배럴 단추, 오른쪽 마우스 단추 또는 이와 유사한 affordance 보조 하드웨어에 의해 수정 된 입력이 포함 됩니다. 그러나 특정 입력이 처리 되지 않은 상태로 유지 되도록 [**InkPresenter**](https://docs.microsoft.com/uwp/api/Windows.UI.Input.Inking.InkPresenter) 를 구성 하 여 사용자 지정 처리를 위해 앱에 전달할 수 있습니다.

이 예제에서는 사용자 지정 도구 단추를 정의 합니다 .이 단추를 선택 하면 다음 스트로크가 잉크 대신 선택 올가미 (파선)로 처리 되 고 렌더링 됩니다. 선택 영역의 범위 내에 있는 모든 잉크 스트로크가 [**선택**](https://docs.microsoft.com/uwp/api/windows.ui.input.inking.inkstroke.selected)됨으로 설정 됩니다.

> [!NOTE]
> InkCanvas 및 InkToolbar 모음 UX 지침 모두에 대 한 잉크 컨트롤을 참조 하세요. 다음은이 예제와 관련 된 권장 사항입니다.
> - 스트로크 선택 항목을 제공 하는 경우 "선택 도구" 도구 설명을 사용 하 여 도구 단추에 대 한 "Segoe MLD2 자산" 글꼴의 "EF20" 아이콘을 사용 하는 것이 좋습니다. 
 
**XAML**

1. 먼저 스트로크 선택이 구성 된 이벤트 처리기 (customToolButton_Click)를 지정 하는 클릭 이벤트 수신기를 사용 하 여 [**Inktoolbarcustomtoolbutton**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.InkToolbarCustomToolButton) 요소 (customtoolbutton)를 선언 합니다. (스트로크 선택을 복사, 잘라내기 및 붙여넣기 위한 단추 집합도 추가 했습니다.)

2. 또한 선택 스트로크를 그리기 위한 Canvas 요소를 추가 합니다. 별도의 계층을 사용 하 여 선택 스트로크를 그리면 [**InkCanvas**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.InkCanvas) 및 해당 콘텐츠가 그대로 유지 됩니다. 

```xaml
<Grid Background="{ThemeResource ApplicationPageBackgroundThemeBrush}">
    <Grid.RowDefinitions>
        <RowDefinition Height="Auto"/>
        <RowDefinition Height="Auto"/>
        <RowDefinition Height="*"/>
    </Grid.RowDefinitions>
    <StackPanel x:Name="HeaderPanel" Orientation="Horizontal" Grid.Row="0">
        <TextBlock x:Name="Header" 
                   Text="Basic ink sample" 
                   Style="{ThemeResource HeaderTextBlockStyle}" 
                   Margin="10,0,0,0" />
    </StackPanel>
    <StackPanel x:Name="ToolPanel" Orientation="Horizontal" Grid.Row="1">
        <InkToolbar x:Name="inkToolbar" 
                    VerticalAlignment="Top" 
                    TargetInkCanvas="{x:Bind inkCanvas}">
            <InkToolbarCustomToolButton 
                x:Name="customToolButton" 
                Click="customToolButton_Click" 
                ToolTipService.ToolTip="Selection tool">
                <SymbolIcon Symbol="{x:Bind SelectIcon}"/>
            </InkToolbarCustomToolButton>
        </InkToolbar>
        <Button x:Name="cutButton" 
                Content="Cut" 
                Click="cutButton_Click"
                Width="100"
                Margin="5,0,0,0"/>
        <Button x:Name="copyButton" 
                Content="Copy"  
                Click="copyButton_Click"
                Width="100"
                Margin="5,0,0,0"/>
        <Button x:Name="pasteButton" 
                Content="Paste"  
                Click="pasteButton_Click"
                Width="100"
                Margin="5,0,0,0"/>
    </StackPanel>
    <Grid Grid.Row="2" x:Name="outputGrid" 
              Background="{ThemeResource SystemControlBackgroundChromeWhiteBrush}" 
              Height="Auto">
        <!-- Canvas for displaying selection UI. -->
        <Canvas x:Name="selectionCanvas"/>
        <!-- Canvas for displaying ink. -->
        <InkCanvas x:Name="inkCanvas" />
    </Grid>
</Grid>
```

**코드 숨김**

2. 그런 다음 MainPage.xaml.cs 코드 숨김으로 파일의 [**Inktoolbarcustomtoolbutton 단추**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.InkToolbarCustomToolButton) 에 대 한 클릭 이벤트를 처리 합니다.

   이 처리기는 처리 되지 않은 입력을 앱으로 전달 하도록 [**InkPresenter**](https://docs.microsoft.com/uwp/api/Windows.UI.Input.Inking.InkPresenter) 를 구성 합니다. 

   이 코드의 자세한 단계는 [windows 앱에서 펜 상호 작용 및 Windows Ink](pen-and-stylus-interactions.md)의 고급 처리를 위한 통과 입력 섹션을 참조 하세요.

   또한 기호 아이콘 요소를 사용 하 여 단추에 대 한 아이콘을 지정 하 고 코드 파일 (SelectIcon)에 정의 된 필드에 바인딩하는 {x:Bind} 태그 확장을 지정 했습니다.

   다음 코드 조각에는 Click 이벤트 처리기와 SelectIcon의 정의가 모두 포함 되어 있습니다.

```csharp
namespace Ink_Basic_InkToolbar
{
    /// <summary>
    /// An empty page that can be used on its own or navigated to within a Frame.
    /// </summary>
    public sealed partial class MainPage_AddCustomTool : Page
    {
        // Icon for custom selection tool button.
        Symbol SelectIcon = (Symbol)0xEF20;

        // Stroke selection tool.
        private Polyline lasso;
        // Stroke selection area.
        private Rect boundingRect;

        public MainPage_AddCustomTool()
        {
            this.InitializeComponent();

            // Listen for new ink or erase strokes to clean up selection UI.
            inkCanvas.InkPresenter.StrokeInput.StrokeStarted +=
                StrokeInput_StrokeStarted;
            inkCanvas.InkPresenter.StrokesErased +=
                InkPresenter_StrokesErased;
        }

        private void customToolButton_Click(object sender, RoutedEventArgs e)
        {
            // By default, the InkPresenter processes input modified by 
            // a secondary affordance (pen barrel button, right mouse 
            // button, or similar) as ink.
            // To pass through modified input to the app for custom processing 
            // on the app UI thread instead of the background ink thread, set 
            // InputProcessingConfiguration.RightDragAction to LeaveUnprocessed.
            inkCanvas.InkPresenter.InputProcessingConfiguration.RightDragAction =
                InkInputRightDragAction.LeaveUnprocessed;

            // Listen for unprocessed pointer events from modified input.
            // The input is used to provide selection functionality.
            inkCanvas.InkPresenter.UnprocessedInput.PointerPressed +=
                UnprocessedInput_PointerPressed;
            inkCanvas.InkPresenter.UnprocessedInput.PointerMoved +=
                UnprocessedInput_PointerMoved;
            inkCanvas.InkPresenter.UnprocessedInput.PointerReleased +=
                UnprocessedInput_PointerReleased;
        }

        // Handle new ink or erase strokes to clean up selection UI.
        private void StrokeInput_StrokeStarted(
            InkStrokeInput sender, Windows.UI.Core.PointerEventArgs args)
        {
            ClearSelection();
        }

        private void InkPresenter_StrokesErased(
            InkPresenter sender, InkStrokesErasedEventArgs args)
        {
            ClearSelection();
        }

        private void cutButton_Click(object sender, RoutedEventArgs e)
        {
            inkCanvas.InkPresenter.StrokeContainer.CopySelectedToClipboard();
            inkCanvas.InkPresenter.StrokeContainer.DeleteSelected();
            ClearSelection();
        }

        private void copyButton_Click(object sender, RoutedEventArgs e)
        {
            inkCanvas.InkPresenter.StrokeContainer.CopySelectedToClipboard();
        }

        private void pasteButton_Click(object sender, RoutedEventArgs e)
        {
            if (inkCanvas.InkPresenter.StrokeContainer.CanPasteFromClipboard())
            {
                inkCanvas.InkPresenter.StrokeContainer.PasteFromClipboard(
                    new Point(0, 0));
            }
            else
            {
                // Cannot paste from clipboard.
            }
        }

        // Clean up selection UI.
        private void ClearSelection()
        {
            var strokes = inkCanvas.InkPresenter.StrokeContainer.GetStrokes();
            foreach (var stroke in strokes)
            {
                stroke.Selected = false;
            }
            ClearBoundingRect();
        }

        private void ClearBoundingRect()
        {
            if (selectionCanvas.Children.Any())
            {
                selectionCanvas.Children.Clear();
                boundingRect = Rect.Empty;
            }
        }

        // Handle unprocessed pointer events from modifed input.
        // The input is used to provide selection functionality.
        // Selection UI is drawn on a canvas under the InkCanvas.
        private void UnprocessedInput_PointerPressed(
            InkUnprocessedInput sender, PointerEventArgs args)
        {
            // Initialize a selection lasso.
            lasso = new Polyline()
            {
                Stroke = new SolidColorBrush(Windows.UI.Colors.Blue),
                StrokeThickness = 1,
                StrokeDashArray = new DoubleCollection() { 5, 2 },
            };

            lasso.Points.Add(args.CurrentPoint.RawPosition);

            selectionCanvas.Children.Add(lasso);
        }

        private void UnprocessedInput_PointerMoved(
            InkUnprocessedInput sender, PointerEventArgs args)
        {
            // Add a point to the lasso Polyline object.
            lasso.Points.Add(args.CurrentPoint.RawPosition);
        }

        private void UnprocessedInput_PointerReleased(
            InkUnprocessedInput sender, PointerEventArgs args)
        {
            // Add the final point to the Polyline object and 
            // select strokes within the lasso area.
            // Draw a bounding box on the selection canvas 
            // around the selected ink strokes.
            lasso.Points.Add(args.CurrentPoint.RawPosition);

            boundingRect =
                inkCanvas.InkPresenter.StrokeContainer.SelectWithPolyLine(
                    lasso.Points);

            DrawBoundingRect();
        }

        // Draw a bounding rectangle, on the selection canvas, encompassing 
        // all ink strokes within the lasso area.
        private void DrawBoundingRect()
        {
            // Clear all existing content from the selection canvas.
            selectionCanvas.Children.Clear();

            // Draw a bounding rectangle only if there are ink strokes 
            // within the lasso area.
            if (!((boundingRect.Width == 0) ||
                (boundingRect.Height == 0) ||
                boundingRect.IsEmpty))
            {
                var rectangle = new Rectangle()
                {
                    Stroke = new SolidColorBrush(Windows.UI.Colors.Blue),
                    StrokeThickness = 1,
                    StrokeDashArray = new DoubleCollection() { 5, 2 },
                    Width = boundingRect.Width,
                    Height = boundingRect.Height
                };

                Canvas.SetLeft(rectangle, boundingRect.X);
                Canvas.SetTop(rectangle, boundingRect.Y);

                selectionCanvas.Children.Add(rectangle);
            }
        }
    }
}
```



### <a name="custom-ink-rendering"></a>사용자 지정 잉크 렌더링

기본적으로 잉크 입력은 대기 시간이 짧은 백그라운드 스레드에서 처리 되며 그릴 때 "그대로" 렌더링 됩니다. 스트로크를 완료 하는 경우 (펜 또는 손가락 리프트 또는 마우스 단추를 눌렀다 놓으면) 스트로크가 UI 스레드에서 처리 되 고 [**InkCanvas**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.InkCanvas) 계층으로 "마른"로 렌더링 됩니다.

잉크 플랫폼을 사용 하면이 동작을 재정의 하 고 사용자 지정 drying 잉크 입력을 통해 잉크 환경을 완벽 하 게 사용자 지정할 수 있습니다.

사용자 지정 drying에 대 한 자세한 내용은 [windows 앱의 펜 상호 작용 및 Windows Ink](https://docs.microsoft.com/windows/uwp/design/input/pen-and-stylus-interactions#custom-ink-rendering)를 참조 하세요.

> [!NOTE]
> 사용자 지정 drying 및 [ **inktoolbar**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.inktoolbar)  
> 앱이 사용자 지정 drying 구현을 사용 하 여 [**InkPresenter**](https://docs.microsoft.com/uwp/api/Windows.UI.Input.Inking.InkPresenter) 의 기본 잉크 렌더링 동작을 재정의 하는 경우에는 렌더링 된 잉크 스트로크를 InkToolbar 모음에서 더 이상 사용할 수 없으며, inktoolbar의 기본 제공 지우기 명령이 제대로 작동 하지 않습니다. 지우기 기능을 제공 하려면 모든 포인터 이벤트를 처리 하 고, 각 스트로크에 대해 적중 테스트를 수행 하 고, 기본 제공 되는 "모든 잉크 지우기" 명령을 재정의 해야 합니다.

## <a name="related-articles"></a>관련된 문서

- [펜 및 스타일러스 상호 작용](pen-and-stylus-interactions.md)

### <a name="topic-samples"></a>토픽 샘플

- [잉크 도구 모음 위치 및 방향 샘플 (기본)](https://github.com/MicrosoftDocs/windows-topic-specific-samples/archive/uwp-ink-toolbar-handedness.zip)
- [잉크 도구 모음 위치 및 방향 샘플 (동적)](https://github.com/MicrosoftDocs/windows-topic-specific-samples/archive/uwp-ink-toolbar-handedness-dynamic.zip)

### <a name="other-samples"></a>다른 샘플

- [단순 잉크 샘플 (c #/C + +)](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/SimpleInk)
- [복합 잉크 샘플 (c + +)](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/ComplexInk)
- [Ink 샘플 (JavaScript)](https://github.com/microsoftarchive/msdn-code-gallery-microsoft/tree/411c271e537727d737a53fa2cbe99eaecac00cc0/Official%20Windows%20Platform%20Sample/Windows%208%20app%20samples/%5BJavaScript%5D-Windows%208%20app%20samples/JavaScript/Windows%208%20app%20samples/Input%20Ink%20sample%20(Windows%208))
- [자습서 시작: Windows 앱에서 잉크 지원](https://github.com/Microsoft/Windows-tutorials-inputs-and-devices/tree/master/GettingStarted-Ink)
- [서적 샘플 강조](https://github.com/Microsoft/Windows-appsample-coloringbook)
- [제품군 정보 샘플](https://github.com/Microsoft/Windows-appsample-familynotes)
