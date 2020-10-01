---
title: 사용자 지정 스타일 만들기
description: 이 자습서를 따라 사용자 지정 스타일 및 슬라이더 컨트롤을 만들어 XAML 앱의 UI를 사용자 지정하는 방법을 알아봅니다.
keywords: XAML, UWP, 시작
ms.date: 09/24/2020
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 1ff5df0d56d5be6b2cbb93814d01fed48a626797
ms.sourcegitcommit: eda7bbe9caa9d61126e11f0f1a98b12183df794d
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 09/24/2020
ms.locfileid: "91219096"
---
# <a name="tutorial-create-custom-styles"></a>자습서: 사용자 지정 스타일 만들기

이 자습서는 XAML 앱의 UI를 사용자 지정하는 방법을 보여 줍니다. 경고: 이 자습서에는 유니콘이 포함될 수도 있습니다. (확인해 보세요!)

PhotoLab 샘플 앱에는 두 가지 페이지가 있습니다. _기본 페이지_는 각 이미지 파일에 대한 일부 정보와 함께 사진 갤러리 보기를 표시합니다.

![MainPage](../basics/images/xaml-basics/mainpage.png)

*세부 정보 페이지*는 선택한 단일 사진을 표시합니다. 플라이아웃 편집 메뉴를 사용하면 사진을 수정하고, 이름을 변경하고, 저장할 수 있습니다.

![DetailPage](../basics/images/xaml-basics/detailpage.png)

## <a name="prerequisites"></a>필수 구성 요소

+ Visual Studio 2019: [Visual Studio 2019 다운로드](https://visualstudio.microsoft.com/downloads/)(Community Edition은 무료입니다.)
+ Windows 10 SDK(10.0.17763.0 이상):  [최신 Windows SDK(무료)](https://developer.microsoft.com/windows/downloads/windows-10-sdk)
+ Windows 10, 버전 1809 이상

## <a name="part-0-get-the-starter-code-from-github"></a>0부: GitHub에서 시작 코드 가져오기

이 자습서에서는 PhotoLab 샘플의 간소화된 버전부터 시작합니다.

1. GitHub의 샘플 페이지([https://github.com/Microsoft/Windows-appsample-photo-lab](https://github.com/Microsoft/Windows-appsample-photo-lab))로 이동합니다.
2. 다음으로, 샘플을 복제 또는 다운로드해야 합니다. **복제 또는 다운로드** 단추를 선택합니다. 하위 메뉴가 나타납니다.
    ![PhotoLab 샘플의 GitHub 페이지에 있는 복제 또는 다운로드 메뉴](images/xaml-basics/clone-repo.png)

    **GitHub에 익숙하지 않은 경우:**

    a. **ZIP 다운로드**를 선택하고, 파일을 로컬로 선택합니다. 그러면 필요한 프로젝트 파일이 모두 포함된 .zip 파일이 다운로드됩니다.

    b. 파일의 압축을 풉니다. 파일 탐색기를 사용하여 방금 다운로드한 .zip 파일을 찾아 마우스 오른쪽 단추로 클릭하고, **압축 풀기...** 를 선택합니다.

    c. 샘플의 로컬 복사본을 찾고, `Windows-appsample-photo-lab-master\xaml-basics-starting-points\style` 디렉터리로 이동합니다.

    **GitHub에 익숙한 경우:**

    a. 리포지토리의 마스터 분기를 로컬에 복제합니다.

    b. `Windows-appsample-photo-lab\xaml-basics-starting-points\style` 디렉터리로 이동합니다.

3. `Photolab.sln`을 두 번 클릭하여 Visual Studio에서 솔루션을 엽니다.

## <a name="part-1-create-a-fancy-slider-control"></a>1부: 멋진 슬라이더 컨트롤 만들기

Windows 앱에서는 여러 가지 방법으로 앱 모양을 사용자 지정할 수 있습니다. 글꼴 및 인쇄 설정에서 색상 및 효과를 흐리게 처리하는 그라데이션에 이르기까지 많은 옵션이 있습니다.

자습서의 첫 번째 부분에서는 일부 사진 편집 컨트롤을 장식해 보겠습니다.

![기본 스타일 기능의 간단한 슬라이더.](../basics/images/xaml-basics/slider-start.png)

이 슬라이더는 뛰어납니다. 슬라이더가 해야 할 모든 작업을 수행하지만, 멋진 모습은 아닙니다. 문제를 해결해 보겠습니다.

노출 슬라이더는 이미지의 노출을 조정합니다. 즉, 왼쪽으로 밀면 이미지가 어두워지고 슬라이더를 오른쪽으로 움직이면 더 밝아집니다. 슬라이더를 검은색에서 흰색으로 바꿔서 더 멋지게 만들어 봅니다. 슬라이더를 더 멋지게 보이게 만드는 한편, 슬라이더가 제공하는 기능에 대한 시각적 단서를 제공하기도 합니다.

F5 키를 눌러 앱을 컴파일 및 실행합니다. 첫 번째 화면에는 이미지 갤러리가 표시됩니다. 이미지를 클릭하면 이미지 세부 정보 페이지로 이동합니다. 편집 단추를 클릭하면 작업할 편집 컨트롤을 볼 수 있습니다. 앱을 종료하고 Visual Studio로 돌아갑니다.

### <a name="customize-a-slider-control"></a>슬라이더 컨트롤 사용자 지정

1. 솔루션 탐색기 패널에서 DetailPage.xaml을 두 번 클릭하여 엽니다.

    ![Visual Studio 솔루션 탐색기의 DetailPage.xaml 파일입니다.](../basics/images/xaml-basics/style-detail-page-explorer.png)

1. `Polygon` 요소를 사용하여 노출 슬라이더의 배경 모양을 만듭니다.

    [Windows.UI.Xaml.Shapes 네임스페이스](/uwp/api/Windows.UI.Xaml.Shapes)는 선택할 수 있는 7가지 셰이프를 제공합니다. 타원, 직사각형 및 유니콘처럼 어떤 형태로든 만들 수 있는 패스라고 불리는 항목이 있습니다.

    ![유니콘](../basics/images/xaml-basics/unicorn.png)

    > **확인 항목:** [셰이프 그리기](../controls-and-patterns/shapes.md) 문서에서는 XAML 도형에 대해 알아야 할 모든 정보를 제공합니다.

    스테레오의 볼륨 컨트롤에서 볼 수 있는 모양과 같은 삼각형 모양의 위젯을 만들려고 합니다.

    ![볼륨 슬라이더](../basics/images/xaml-basics/style-volume-slider.png)

    `Polygon` 셰이프를 작업해야 할 것 같습니다! 다각형을 정의하려면 점 세트를 지정하고 채우기를 지정합니다. 그라데이션 채우기를 사용하여 너비가 약 200픽셀, 높이가 20픽셀인 다각형을 만듭니다.

    DetailPage.xaml에서 노출 슬라이더의 코드를 찾은 다음, 바로 앞에 다각형 요소를 만듭니다.

    * **Grid.Row**를 “2”로 설정하여 다각형을 노출 슬라이더와 같은 행에 배치합니다.
    * **Points** 속성을 “0,20 200,20 200,0”으로 설정하여 삼각형 모양을 정의합니다.
    * **Stretch** 속성을 “Fill”로 설정하고 **HorizontalAlignment** 속성을 “Stretch”로 설정합니다.
    * **Height**를 “20”으로 설정하고 **VerticalAlignment**를 “Center”로 설정합니다.
    * **Polygon**에 선형 그라데이션 채우기를 적용합니다.
    * 노출 슬라이더에서 **Foreground** 속성을 “Transparent”로 설정하여 다각형을 볼 수 있게 합니다.

    **이전**

    ```xaml
    <Slider Header="Exposure"
        Grid.Row="2"
        Value="{x:Bind item.Exposure, Mode=TwoWay}"
        Minimum="-2"
        Maximum="2" />
    ```

    **이후**

    ```xaml
    <Polygon Grid.Row="2" Stretch="Fill"
                Points="0,20 200,20 200,0" HorizontalAlignment="Stretch"
                VerticalAlignment="Center" Height="20">
        <Polygon.Fill>
            <LinearGradientBrush StartPoint="0,0.5" EndPoint="1,0.5">
                <LinearGradientBrush.GradientStops>
                    <GradientStop Offset="0" Color="Black" />
                    <GradientStop Offset="1" Color="White" />
                </LinearGradientBrush.GradientStops>
            </LinearGradientBrush>
        </Polygon.Fill>
    </Polygon>
    <Slider Header="Exposure"
        Grid.Row="2"
        Foreground="Transparent"
        Value="{x:Bind item.Exposure, Mode=TwoWay}"
        Minimum="-2"
        Maximum="2" />
    ```

    참고:
    * 주변 XAML을 보면 이 요소가 `Grid`에 있는 것을 알 수 있습니다. 노출 슬라이더(`Grid.Row="2"`)와 동일한 행에 다각형을 넣어 동일한 지점에 나타냅니다. 슬라이더가 형태 위에 렌더링되도록 슬라이드 앞에 다각형을 놓습니다.
    * 다각형에 대해 `Stretch="Fill"` 및 `HorizontalAlignment="Stretch"`를 설정하여 삼각형이 사용 가능한 공간을 채우도록 조정합니다. 슬라이더가 축소되거나 너비가 커지면 다각형은 서로 일치하도록 축소되거나 확대됩니다.

1. 앱을 컴파일하고 실행합니다. 슬라이더가 이제 멋진 모습으로 보입니다.

    ![멋진 노출 슬라이더](../basics/images/xaml-basics/style-exposure-slider-done.png)

1. 그다음, 이제 온도 슬라이더를 업그레이드해 보겠습니다. 온도 슬라이더는 이미지의 색온도를 변경합니다. 왼쪽으로 밀면 이미지가 파란색이 되고 오른쪽으로 밀면 이미지가 노란색이 됩니다.

    이 배경 셰이프에 이전과 같은 크기의 또 다른 다각형을 사용하지만, 이번에는 채우기를 흑백이 아닌 파란색-노란색 그라데이션으로 만들 것입니다.

    **이전**

    ```xaml
    <TextBlock Grid.Row="2"
                Grid.Column="1"
                 Margin="10,8,0,0" VerticalAlignment="Center" Padding="0"
                Text="{x:Bind item.Exposure.ToString('N', culture), Mode=OneWay}" />

    <Slider Header="Temperature"
            Grid.Row="3" Background="Transparent" Foreground="Transparent"
            Value="{x:Bind item.Temperature, Mode=TwoWay}"
            Minimum="-1"
            Maximum="1" />
    ```

    **이후**

    ```xaml
    <TextBlock Grid.Row="2"
                Grid.Column="1"
                Margin="10,8,0,0" VerticalAlignment="Center" Padding="0"
                Text="{x:Bind item.Exposure.ToString('N', culture), Mode=OneWay}" />

    <Polygon Grid.Row="3" Stretch="Fill"
                Points="0,20 200,20 200,0" HorizontalAlignment="Stretch"
                VerticalAlignment="Center" Height="20">
        <Polygon.Fill>
            <LinearGradientBrush StartPoint="0,0.5" EndPoint="1,0.5">
                <LinearGradientBrush.GradientStops>
                    <GradientStop Offset="0" Color="Blue" />
                    <GradientStop Offset="1" Color="Yellow" />
                </LinearGradientBrush.GradientStops>
            </LinearGradientBrush>
        </Polygon.Fill>
    </Polygon>
    <Slider Header="Temperature"
            Grid.Row="3" Background="Transparent" Foreground="Transparent"
            Value="{x:Bind item.Temperature, Mode=TwoWay}"
            Minimum="-1"
            Maximum="1" />
    ```

1. 앱을 컴파일하고 실행합니다. 다음 두 개의 멋진 슬라이더를 만들었습니다.

    ![두 개의 멋진 슬라이더](../basics/images/xaml-basics/style-2sliders-done.png)

1. **추가 크레딧**

    초록색에서 빨간색으로 그라데이션 처리된 색조 슬라이더에 대한 배경 셰이프를 추가합니다.

    ![세 개의 멋진 슬라이더](../basics/images/xaml-basics/style-3sliders-done.png)

축하합니다. 1부를 완료했습니다! 중간에 문제가 생겼거나 최종 솔루션을 확인하려는 경우 [https://github.com/Microsoft/Windows-appsample-photo-lab](https://github.com/Microsoft/Windows-appsample-photo-lab)에서 완성된 코드를 볼 수 있습니다.

## <a name="part-2-create-basic-styles"></a>2부: 기본 스타일 만들기

XAML 스타일의 장점 중 하나는 작성해야 하는 코드의 양을 대폭 줄일 수 있고 앱의 모양을 훨씬 쉽게 업데이트할 수 있다는 것입니다.

스타일을 정의하려면 스타일을 지정할 컨트롤이 포함된 요소의 [Resources](/uwp/api/windows.ui.xaml.frameworkelement.Resources) 속성에 [Style](/uwp/api/Windows.UI.Xaml.Style) 요소를 추가합니다.  `Page.Resources` 속성에 스타일을 추가하면 스타일이 전체 페이지에 액세스할 수 있습니다. App.xaml 파일의 `Application.Resources` 속성에 스타일을 추가하면 스타일이 전체 앱에 액세스할 수 있습니다.

명명된 스타일과 일반 스타일을 만들 수 있습니다. 명명된 스타일은 특정 컨트롤에 명시적으로 적용되어야 합니다. 일반 스타일은 지정된 `TargetType`과 일치하는 모든 컨트롤에 적용됩니다.

이 예제에서는 첫 번째 스타일에 `x:Key` 특성이 있고 대상 유형은 `Button`입니다. 첫 번째 단추의 `Style` 속성이 이 키로 설정되므로 이 스타일은 명명된 스타일이며 명시적으로 적용되어야 합니다. 두 번째 단추는 대상 유형이 `Button`이고 스타일에 `x:Key` 특성이 없으므로 두 번째 스타일이 암시적으로 자동 적용됩니다.

```xaml
<Page.Resources>
    <Style x:Key="PurpleStyle" TargetType="Button">
        <Setter Property="FontFamily" Value="Lucida Sans Unicode"/>
        <Setter Property="FontStyle" Value="Italic"/>
        <Setter Property="FontSize" Value="14"/>
        <Setter Property="Foreground" Value="MediumOrchid"/>
    </Style>

    <Style TargetType="Button">
        <Setter Property="Foreground" Value="Orange"/>
    </Style>
</Page.Resources>

<Grid x:Name="LayoutRoot">
    <Button Content="Button" Style="{StaticResource PurpleStyle}"/>
    <Button Content="Button" />
</Grid>
```

앱에 스타일을 추가해 보겠습니다. DetailsPage.xaml에서 노출, 온도 및 색조 슬라이더 옆에 있는 텍스트 블록을 살펴봅니다. 각 텍스트 블록에는 슬라이더의 값이 표시됩니다. 노출 슬라이더의 텍스트 블록은 다음과 같습니다. `Margin`, `VerticalAlignment` 및 `Padding` 속성이 설정된 점에 주목합니다.

```xaml
<TextBlock Grid.Row="2"
            Grid.Column="1"
            Margin="10,8,0,0" VerticalAlignment="Center" Padding="0"
            Text="{x:Bind item.Exposure.ToString('N', culture), Mode=OneWay}" />
```

다른 텍스트 블록을 살펴보면 동일한 속성이 동일한 값으로 설정되어 있습니다. 스타일에 대한 좋은 본보기가 됩니다.

### <a name="create-a-value-text-block-style"></a>값 텍스트 블록 스타일 만들기

1. DetailsPage.xaml을 엽니다.

2. **EditControlsGrid**라는 이름의 `Grid`를 찾습니다. 여기에는 슬라이더와 텍스트 상자가 있습니다. 그리드는 이미 슬라이더의 스타일을 정의한다는 점에 주목합니다.

    ```xaml
    <Grid x:Name="EditControlsGrid"
            HorizontalAlignment="Stretch"
            Margin="24,48,24,24">
        <Grid.Resources>
            <Style TargetType="Slider">
                <Setter Property="Margin"
                        Value="0,0,0,0" />
                <Setter Property="Padding"
                        Value="0" />
                <Setter Property="MinWidth"
                        Value="100" />
                <Setter Property="StepFrequency"
                        Value="0.1" />
                <Setter Property="TickFrequency"
                        Value="0.1" />
            </Style>
        </Grid.Resources>
    ```

3. **Margin**을 “10,8,0,0”으로, **VerticalAlignment**를 “Center”로, **Padding**을 “0”으로 설정하는 **TextBlock** 스타일을 만듭니다.

    **이전**

    ```xaml
        <Grid.Resources>
            <Style TargetType="Slider">
                <Setter Property="Margin"
                        Value="0,0,0,0" />
                <Setter Property="Padding"
                        Value="0" />
                <Setter Property="MinWidth"
                        Value="100" />
                <Setter Property="StepFrequency"
                        Value="0.1" />
                <Setter Property="TickFrequency"
                        Value="0.1" />
            </Style>
        </Grid.Resources>
    ```

    **이후**

    ```xaml
        <Grid.Resources>
            <Style TargetType="Slider">
                <Setter Property="Margin"
                        Value="0,0,0,0" />
                <Setter Property="Padding"
                        Value="0" />
                <Setter Property="MinWidth"
                        Value="100" />
                <Setter Property="StepFrequency"
                        Value="0.1" />
                <Setter Property="TickFrequency"
                        Value="0.1" />
            </Style>
            <Style TargetType="TextBlock">
                <Setter Property="Margin"
                        Value="10,8,0,0" />
                <Setter Property="VerticalAlignment"
                        Value="Center" />
                <Setter Property="Padding"
                        Value="0" />
            </Style>
        </Grid.Resources>
    ```

4. 이를 명명된 스타일로 만들어 `TextBlock`이 적용되는 컨트롤을 지정할 수 있습니다. 스타일의 `x:Key` 속성을 "ValueTextBlock"으로 설정합니다.

    **이전**

    ```xaml
            <Style TargetType="TextBlock">
                <Setter Property="Margin"
                        Value="10,8,0,0" />
                <Setter Property="VerticalAlignment"
                        Value="Center" />
                <Setter Property="Padding"
                        Value="0" />
            </Style>
    ```

    **이후**

    ```xaml
            <Style TargetType="TextBlock"
                   x:Key="ValueTextBlock">
                <Setter Property="Margin"
                        Value="10,8,0,0" />
                <Setter Property="VerticalAlignment"
                        Value="Center" />
                <Setter Property="Padding"
                        Value="0" />
            </Style>
    ```

5. 각 `TextBlock`에 대해 `Margin`, `VerticalAlignment`, `Padding` 속성을 제거하고 해당 `Style` 속성을 "{StaticResource ValueTextBlock}"으로 설정합니다.

    **이전**

    ```xaml
     <TextBlock Grid.Row="2"
                Grid.Column="1"
                Margin="10,8,0,0" VerticalAlignment="Center" Padding="0"
                Text="{x:Bind item.Exposure.ToString('N', culture), Mode=OneWay}" />
    ```

    **이후**

    ```xaml
     <TextBlock Grid.Row="2"
                Grid.Column="1"
                Style="{StaticResource ValueTextBlock}"
                Text="{x:Bind item.Exposure.ToString('N', culture), Mode=OneWay}" />
    ```

    슬라이더와 관련된 6개의 TextBlock 컨트롤 모두를 변경합니다.

6. 앱을 컴파일하고 실행합니다. 동일한 모습으로 보여야 합니다. 하지만 효율적이고 유지 관리하기 쉬운 코드를 작성함으로써 얻은 만족감과 성취감을 느낄 수 있습니다.

축하합니다. 2부를 완료했습니다!

## <a name="part-3-use-a-control-template-to-make-a-fancy-slider"></a>3부: 컨트롤 템플릿을 사용하여 멋진 슬라이더 만들기

1부에서 슬라이더 뒤에 형태를 추가하여 멋지게 표현한 것을 기억하세요?

작업은 완료했지만 동일한 효과를 얻는 더 좋은 방법이 있습니다. 즉, 컨트롤 템플릿을 만드는 것입니다.

1. 솔루션 탐색기 패널에서 DetailPage.xaml을 두 번 클릭합니다.

1. 그다음, 슬라이더의 기본 컨트롤 템플릿을 시작 지점으로 사용합니다. 이 XAML을 `Page.Resources` 요소에 추가합니다. (`Page.Resources` 요소는 페이지 시작 부분에 있습니다.)

    이 앱은 Windows UI 라이브러리(WinUI)를 사용하므로 WinUI GitHub 리포지토리에서 기본 템플릿을 복사할 수 있습니다. [Slider_themeresources.xaml](https://github.com/microsoft/microsoft-ui-xaml/blob/master/dev/Slider/Slider_themeresources.xaml).


    ```xaml
    <ControlTemplate x:Key="FancySliderControlTemplate" TargetType="Slider"
      xmlns:contract6Present="http://schemas.microsoft.com/winfx/2006/xaml/presentation?IsApiContractPresent(Windows.Foundation.UniversalApiContract,6)"
      xmlns:contract6NotPresent="http://schemas.microsoft.com/winfx/2006/xaml/presentation?IsApiContractNotPresent(Windows.Foundation.UniversalApiContract,6)"
      xmlns:contract7Present="http://schemas.microsoft.com/winfx/2006/xaml/presentation?IsApiContractPresent(Windows.Foundation.UniversalApiContract,7)"
      xmlns:contract7NotPresent="http://schemas.microsoft.com/winfx/2006/xaml/presentation?IsApiContractNotPresent(Windows.Foundation.UniversalApiContract,7)">
      <Grid Margin="{TemplateBinding Padding}">
        <Grid.Resources>
            <Style TargetType="Thumb" x:Key="SliderThumbStyle">
                <Setter Property="BorderThickness" Value="0" />
                <Setter Property="Background" Value="{ThemeResource SliderThumbBackground}" />
                <Setter Property="Template">
                    <Setter.Value>
                        <ControlTemplate TargetType="Thumb">
                            <Border
                                Background="{TemplateBinding Background}"
                                BorderBrush="{TemplateBinding BorderBrush}"
                                BorderThickness="{TemplateBinding BorderThickness}"
                                CornerRadius="{ThemeResource SliderThumbCornerRadius}" />
                        </ControlTemplate>
                    </Setter.Value>
                </Setter>
            </Style>
        </Grid.Resources>

        <VisualStateManager.VisualStateGroups>
            <VisualStateGroup x:Name="CommonStates">
                <VisualState x:Name="Normal" />

                <VisualState x:Name="Pressed">

                    <Storyboard>
                        <ObjectAnimationUsingKeyFrames Storyboard.TargetName="HorizontalTrackRect" Storyboard.TargetProperty="Fill">
                            <DiscreteObjectKeyFrame KeyTime="0" Value="{ThemeResource SliderTrackFillPressed}" />
                        </ObjectAnimationUsingKeyFrames>
                        <ObjectAnimationUsingKeyFrames Storyboard.TargetName="VerticalTrackRect" Storyboard.TargetProperty="Fill">
                            <DiscreteObjectKeyFrame KeyTime="0" Value="{ThemeResource SliderTrackFillPressed}" />
                        </ObjectAnimationUsingKeyFrames>
                        <ObjectAnimationUsingKeyFrames Storyboard.TargetName="HorizontalThumb" Storyboard.TargetProperty="Background">
                            <DiscreteObjectKeyFrame KeyTime="0" Value="{ThemeResource SliderThumbBackgroundPressed}" />
                        </ObjectAnimationUsingKeyFrames>
                        <ObjectAnimationUsingKeyFrames Storyboard.TargetName="VerticalThumb" Storyboard.TargetProperty="Background">
                            <DiscreteObjectKeyFrame KeyTime="0" Value="{ThemeResource SliderThumbBackgroundPressed}" />
                        </ObjectAnimationUsingKeyFrames>
                        <ObjectAnimationUsingKeyFrames Storyboard.TargetName="SliderContainer" Storyboard.TargetProperty="Background">
                            <DiscreteObjectKeyFrame KeyTime="0" Value="{ThemeResource SliderContainerBackgroundPressed}" />
                        </ObjectAnimationUsingKeyFrames>
                        <ObjectAnimationUsingKeyFrames Storyboard.TargetName="HorizontalDecreaseRect" Storyboard.TargetProperty="Fill">
                            <DiscreteObjectKeyFrame KeyTime="0" Value="{ThemeResource SliderTrackValueFillPressed}" />
                        </ObjectAnimationUsingKeyFrames>
                        <ObjectAnimationUsingKeyFrames Storyboard.TargetName="VerticalDecreaseRect" Storyboard.TargetProperty="Fill">
                            <DiscreteObjectKeyFrame KeyTime="0" Value="{ThemeResource SliderTrackValueFillPressed}" />
                        </ObjectAnimationUsingKeyFrames>
                    </Storyboard>
                </VisualState>

                <VisualState x:Name="Disabled">

                    <Storyboard>
                        <ObjectAnimationUsingKeyFrames Storyboard.TargetName="HeaderContentPresenter" Storyboard.TargetProperty="Foreground">
                            <DiscreteObjectKeyFrame KeyTime="0" Value="{ThemeResource SliderHeaderForegroundDisabled}" />
                        </ObjectAnimationUsingKeyFrames>
                        <ObjectAnimationUsingKeyFrames Storyboard.TargetName="HorizontalDecreaseRect" Storyboard.TargetProperty="Fill">
                            <DiscreteObjectKeyFrame KeyTime="0" Value="{ThemeResource SliderTrackValueFillDisabled}" />
                        </ObjectAnimationUsingKeyFrames>
                        <ObjectAnimationUsingKeyFrames Storyboard.TargetName="HorizontalTrackRect" Storyboard.TargetProperty="Fill">
                            <DiscreteObjectKeyFrame KeyTime="0" Value="{ThemeResource SliderTrackFillDisabled}" />
                        </ObjectAnimationUsingKeyFrames>
                        <ObjectAnimationUsingKeyFrames Storyboard.TargetName="VerticalDecreaseRect" Storyboard.TargetProperty="Fill">
                            <DiscreteObjectKeyFrame KeyTime="0" Value="{ThemeResource SliderTrackValueFillDisabled}" />
                        </ObjectAnimationUsingKeyFrames>
                        <ObjectAnimationUsingKeyFrames Storyboard.TargetName="VerticalTrackRect" Storyboard.TargetProperty="Fill">
                            <DiscreteObjectKeyFrame KeyTime="0" Value="{ThemeResource SliderTrackFillDisabled}" />
                        </ObjectAnimationUsingKeyFrames>
                        <ObjectAnimationUsingKeyFrames Storyboard.TargetName="HorizontalThumb" Storyboard.TargetProperty="Background">
                            <DiscreteObjectKeyFrame KeyTime="0" Value="{ThemeResource SliderThumbBackgroundDisabled}" />
                        </ObjectAnimationUsingKeyFrames>
                        <ObjectAnimationUsingKeyFrames Storyboard.TargetName="VerticalThumb" Storyboard.TargetProperty="Background">
                            <DiscreteObjectKeyFrame KeyTime="0" Value="{ThemeResource SliderThumbBackgroundDisabled}" />
                        </ObjectAnimationUsingKeyFrames>
                        <ObjectAnimationUsingKeyFrames Storyboard.TargetName="TopTickBar" Storyboard.TargetProperty="Fill">
                            <DiscreteObjectKeyFrame KeyTime="0" Value="{ThemeResource SliderTickBarFillDisabled}" />
                        </ObjectAnimationUsingKeyFrames>
                        <ObjectAnimationUsingKeyFrames Storyboard.TargetName="BottomTickBar" Storyboard.TargetProperty="Fill">
                            <DiscreteObjectKeyFrame KeyTime="0" Value="{ThemeResource SliderTickBarFillDisabled}" />
                        </ObjectAnimationUsingKeyFrames>
                        <ObjectAnimationUsingKeyFrames Storyboard.TargetName="LeftTickBar" Storyboard.TargetProperty="Fill">
                            <DiscreteObjectKeyFrame KeyTime="0" Value="{ThemeResource SliderTickBarFillDisabled}" />
                        </ObjectAnimationUsingKeyFrames>
                        <ObjectAnimationUsingKeyFrames Storyboard.TargetName="RightTickBar" Storyboard.TargetProperty="Fill">
                            <DiscreteObjectKeyFrame KeyTime="0" Value="{ThemeResource SliderTickBarFillDisabled}" />
                        </ObjectAnimationUsingKeyFrames>
                        <ObjectAnimationUsingKeyFrames Storyboard.TargetName="SliderContainer" Storyboard.TargetProperty="Background">
                            <DiscreteObjectKeyFrame KeyTime="0" Value="{ThemeResource SliderContainerBackgroundDisabled}" />
                        </ObjectAnimationUsingKeyFrames>
                    </Storyboard>
                </VisualState>

                <VisualState x:Name="PointerOver">

                    <Storyboard>
                        <ObjectAnimationUsingKeyFrames Storyboard.TargetName="HorizontalTrackRect" Storyboard.TargetProperty="Fill">
                            <DiscreteObjectKeyFrame KeyTime="0" Value="{ThemeResource SliderTrackFillPointerOver}" />
                        </ObjectAnimationUsingKeyFrames>
                        <ObjectAnimationUsingKeyFrames Storyboard.TargetName="VerticalTrackRect" Storyboard.TargetProperty="Fill">
                            <DiscreteObjectKeyFrame KeyTime="0" Value="{ThemeResource SliderTrackFillPointerOver}" />
                        </ObjectAnimationUsingKeyFrames>
                        <ObjectAnimationUsingKeyFrames Storyboard.TargetName="HorizontalThumb" Storyboard.TargetProperty="Background">
                            <DiscreteObjectKeyFrame KeyTime="0" Value="{ThemeResource SliderThumbBackgroundPointerOver}" />
                        </ObjectAnimationUsingKeyFrames>
                        <ObjectAnimationUsingKeyFrames Storyboard.TargetName="VerticalThumb" Storyboard.TargetProperty="Background">
                            <DiscreteObjectKeyFrame KeyTime="0" Value="{ThemeResource SliderThumbBackgroundPointerOver}" />
                        </ObjectAnimationUsingKeyFrames>
                        <ObjectAnimationUsingKeyFrames Storyboard.TargetName="SliderContainer" Storyboard.TargetProperty="Background">
                            <DiscreteObjectKeyFrame KeyTime="0" Value="{ThemeResource SliderContainerBackgroundPointerOver}" />
                        </ObjectAnimationUsingKeyFrames>
                        <ObjectAnimationUsingKeyFrames Storyboard.TargetName="HorizontalDecreaseRect" Storyboard.TargetProperty="Fill">
                            <DiscreteObjectKeyFrame KeyTime="0" Value="{ThemeResource SliderTrackValueFillPointerOver}" />
                        </ObjectAnimationUsingKeyFrames>
                        <ObjectAnimationUsingKeyFrames Storyboard.TargetName="VerticalDecreaseRect" Storyboard.TargetProperty="Fill">
                            <DiscreteObjectKeyFrame KeyTime="0" Value="{ThemeResource SliderTrackValueFillPointerOver}" />
                        </ObjectAnimationUsingKeyFrames>
                    </Storyboard>
                </VisualState>

            </VisualStateGroup>
            <VisualStateGroup x:Name="FocusEngagementStates">
                <VisualState x:Name="FocusDisengaged" />
                <VisualState x:Name="FocusEngagedHorizontal">

                    <Storyboard>
                        <ObjectAnimationUsingKeyFrames Storyboard.TargetName="SliderContainer" Storyboard.TargetProperty="(Control.IsTemplateFocusTarget)">
                            <DiscreteObjectKeyFrame KeyTime="0" Value="False" />
                        </ObjectAnimationUsingKeyFrames>
                        <ObjectAnimationUsingKeyFrames Storyboard.TargetName="HorizontalThumb" Storyboard.TargetProperty="(Control.IsTemplateFocusTarget)">
                            <DiscreteObjectKeyFrame KeyTime="0" Value="True" />
                        </ObjectAnimationUsingKeyFrames>
                    </Storyboard>
                </VisualState>
                <VisualState x:Name="FocusEngagedVertical">

                    <Storyboard>
                        <ObjectAnimationUsingKeyFrames Storyboard.TargetName="SliderContainer" Storyboard.TargetProperty="(Control.IsTemplateFocusTarget)">
                            <DiscreteObjectKeyFrame KeyTime="0" Value="False" />
                        </ObjectAnimationUsingKeyFrames>
                        <ObjectAnimationUsingKeyFrames Storyboard.TargetName="VerticalThumb" Storyboard.TargetProperty="(Control.IsTemplateFocusTarget)">
                            <DiscreteObjectKeyFrame KeyTime="0" Value="True" />
                        </ObjectAnimationUsingKeyFrames>
                    </Storyboard>
                </VisualState>

            </VisualStateGroup>

        </VisualStateManager.VisualStateGroups>

        <Grid.RowDefinitions>
            <RowDefinition Height="Auto" />
            <RowDefinition Height="*" />
        </Grid.RowDefinitions>

        <ContentPresenter x:Name="HeaderContentPresenter"
            Grid.Row="0"
            Content="{TemplateBinding Header}"
            ContentTemplate="{TemplateBinding HeaderTemplate}"
            FontWeight="{ThemeResource SliderHeaderThemeFontWeight}"
            Foreground="{ThemeResource SliderHeaderForeground}"
            Margin="{ThemeResource SliderTopHeaderMargin}"
            TextWrapping="Wrap"
            Visibility="Collapsed"
            x:DeferLoadStrategy="Lazy"/>
        <Grid x:Name="SliderContainer"
            Grid.Row="1"
            Background="{ThemeResource SliderContainerBackground}"
            Control.IsTemplateFocusTarget="True">
            <Grid x:Name="HorizontalTemplate" MinHeight="{ThemeResource SliderHorizontalHeight}">

                <Grid.ColumnDefinitions>
                    <ColumnDefinition Width="Auto" />
                    <ColumnDefinition Width="Auto" />
                    <ColumnDefinition Width="*" />
                </Grid.ColumnDefinitions>

                <Grid.RowDefinitions>
                    <RowDefinition Height="{ThemeResource SliderPreContentMargin}" />
                    <RowDefinition Height="Auto" />
                    <RowDefinition Height="{ThemeResource SliderPostContentMargin}" />
                </Grid.RowDefinitions>

                <Rectangle x:Name="HorizontalTrackRect"
                    Fill="{TemplateBinding Background}"
                    Height="{ThemeResource SliderTrackThemeHeight}"
                    Grid.Row="1"
                    Grid.ColumnSpan="3"
                    contract7Present:RadiusX="{Binding CornerRadius, RelativeSource={RelativeSource TemplatedParent}, Converter={StaticResource TopLeftCornerRadiusDoubleValueConverter}}"
                    contract7Present:RadiusY="{Binding CornerRadius, RelativeSource={RelativeSource TemplatedParent}, Converter={StaticResource BottomRightCornerRadiusDoubleValueConverter}}"
                    contract7NotPresent:RadiusX="{Binding Source={ThemeResource ControlCornerRadius}, Converter={StaticResource TopLeftCornerRadiusDoubleValueConverter}}"
                    contract7NotPresent:RadiusY="{Binding Source={ThemeResource ControlCornerRadius}, Converter={StaticResource BottomRightCornerRadiusDoubleValueConverter}}" />
                <Rectangle x:Name="HorizontalDecreaseRect" Fill="{TemplateBinding Foreground}" Grid.Row="1"
                    contract7Present:RadiusX="{Binding CornerRadius, RelativeSource={RelativeSource TemplatedParent}, Converter={StaticResource TopLeftCornerRadiusDoubleValueConverter}}"
                    contract7Present:RadiusY="{Binding CornerRadius, RelativeSource={RelativeSource TemplatedParent}, Converter={StaticResource BottomRightCornerRadiusDoubleValueConverter}}"
                    contract7NotPresent:RadiusX="{Binding Source={ThemeResource ControlCornerRadius}, Converter={StaticResource TopLeftCornerRadiusDoubleValueConverter}}"
                    contract7NotPresent:RadiusY="{Binding Source={ThemeResource ControlCornerRadius}, Converter={StaticResource BottomRightCornerRadiusDoubleValueConverter}}" />
                <TickBar x:Name="TopTickBar"
                    Visibility="Collapsed"
                    Fill="{ThemeResource SliderTickBarFill}"
                    Height="{ThemeResource SliderOutsideTickBarThemeHeight}"
                    VerticalAlignment="Bottom"
                    Margin="0,0,0,4"
                    Grid.ColumnSpan="3" />
                <TickBar x:Name="HorizontalInlineTickBar"
                    Visibility="Collapsed"
                    Fill="{ThemeResource SliderInlineTickBarFill}"
                    Height="{ThemeResource SliderTrackThemeHeight}"
                    Grid.Row="1"
                    Grid.ColumnSpan="3" />
                <TickBar x:Name="BottomTickBar"
                    Visibility="Collapsed"
                    Fill="{ThemeResource SliderTickBarFill}"
                    Height="{ThemeResource SliderOutsideTickBarThemeHeight}"
                    VerticalAlignment="Top"
                    Margin="0,4,0,0"
                    Grid.Row="2"
                    Grid.ColumnSpan="3" />
                <Thumb x:Name="HorizontalThumb"
                    Style="{StaticResource SliderThumbStyle}"
                    DataContext="{TemplateBinding Value}"
                    Height="{ThemeResource SliderHorizontalThumbHeight}"
                    Width="{ThemeResource SliderHorizontalThumbWidth}"
                    Grid.Row="0"
                    Grid.RowSpan="3"
                    Grid.Column="1"
                    FocusVisualMargin="-14,-6,-14,-6"
                    AutomationProperties.AccessibilityView="Raw" />
            </Grid>
            <Grid x:Name="VerticalTemplate" MinWidth="{ThemeResource SliderVerticalWidth}" Visibility="Collapsed">

                <Grid.RowDefinitions>
                    <RowDefinition Height="*" />
                    <RowDefinition Height="Auto" />
                    <RowDefinition Height="Auto" />
                </Grid.RowDefinitions>

                <Grid.ColumnDefinitions>
                    <ColumnDefinition Width="{ThemeResource SliderPreContentMargin}" />
                    <ColumnDefinition Width="Auto" />
                    <ColumnDefinition Width="{ThemeResource SliderPostContentMargin}" />
                </Grid.ColumnDefinitions>

                <Rectangle x:Name="VerticalTrackRect"
                    Fill="{TemplateBinding Background}"
                    Width="{ThemeResource SliderTrackThemeHeight}"
                    Grid.Column="1"
                    Grid.RowSpan="3"
                    contract7Present:RadiusX="{Binding CornerRadius, RelativeSource={RelativeSource TemplatedParent}, Converter={StaticResource TopLeftCornerRadiusDoubleValueConverter}}"
                    contract7Present:RadiusY="{Binding CornerRadius, RelativeSource={RelativeSource TemplatedParent}, Converter={StaticResource BottomRightCornerRadiusDoubleValueConverter}}"
                    contract7NotPresent:RadiusX="{Binding Source={ThemeResource ControlCornerRadius}, Converter={StaticResource TopLeftCornerRadiusDoubleValueConverter}}"
                    contract7NotPresent:RadiusY="{Binding Source={ThemeResource ControlCornerRadius}, Converter={StaticResource BottomRightCornerRadiusDoubleValueConverter}}" />
                <Rectangle x:Name="VerticalDecreaseRect"
                    Fill="{TemplateBinding Foreground}"
                    Grid.Column="1"
                    Grid.Row="2"
                    contract7Present:RadiusX="{Binding CornerRadius, RelativeSource={RelativeSource TemplatedParent}, Converter={StaticResource TopLeftCornerRadiusDoubleValueConverter}}"
                    contract7Present:RadiusY="{Binding CornerRadius, RelativeSource={RelativeSource TemplatedParent}, Converter={StaticResource BottomRightCornerRadiusDoubleValueConverter}}"
                    contract7NotPresent:RadiusX="{Binding Source={ThemeResource ControlCornerRadius}, Converter={StaticResource TopLeftCornerRadiusDoubleValueConverter}}"
                    contract7NotPresent:RadiusY="{Binding Source={ThemeResource ControlCornerRadius}, Converter={StaticResource BottomRightCornerRadiusDoubleValueConverter}}" />
                <TickBar x:Name="LeftTickBar"
                    Visibility="Collapsed"
                    Fill="{ThemeResource SliderTickBarFill}"
                    Width="{ThemeResource SliderOutsideTickBarThemeHeight}"
                    HorizontalAlignment="Right"
                    Margin="0,0,4,0"
                    Grid.RowSpan="3" />
                <TickBar x:Name="VerticalInlineTickBar"
                    Visibility="Collapsed"
                    Fill="{ThemeResource SliderInlineTickBarFill}"
                    Width="{ThemeResource SliderTrackThemeHeight}"
                    Grid.Column="1"
                    Grid.RowSpan="3" />
                <TickBar x:Name="RightTickBar"
                    Visibility="Collapsed"
                    Fill="{ThemeResource SliderTickBarFill}"
                    Width="{ThemeResource SliderOutsideTickBarThemeHeight}"
                    HorizontalAlignment="Left"
                    Margin="4,0,0,0"
                    Grid.Column="2"
                    Grid.RowSpan="3" />
                <Thumb x:Name="VerticalThumb"
                    Style="{StaticResource SliderThumbStyle}"
                    DataContext="{TemplateBinding Value}"
                    Width="{ThemeResource SliderVerticalThumbWidth}"
                    Height="{ThemeResource SliderVerticalThumbHeight}"
                    Grid.Row="1"
                    Grid.Column="0"
                    Grid.ColumnSpan="3"
                    FocusVisualMargin="-6,-14,-6,-14"
                    AutomationProperties.AccessibilityView="Raw" />
            </Grid>
        </Grid>
      </Grid>
    </ControlTemplate>
    ```

    XAML이 많네요! 컨트롤 템플릿은 강력한 기능이지만 매우 복잡할 수 있기 때문에 기본 템플릿에서 시작하는 것이 좋습니다.

1. 방금 추가한 `ControlTemplate` 컨트롤 내에서 `HorizontalTemplate`이라는 그리드 컨트롤을 찾습니다. 이 그리드는 변경하려는 템플릿의 부분을 정의합니다.

    ```xaml
    <Grid x:Name="HorizontalTemplate" MinHeight="{ThemeResource SliderHorizontalHeight}">
        <Grid.ColumnDefinitions>
            <ColumnDefinition Width="Auto" />
            <ColumnDefinition Width="Auto" />
            <ColumnDefinition Width="*" />
        </Grid.ColumnDefinitions>
        <Grid.RowDefinitions>
            <RowDefinition Height="{ThemeResource SliderPreContentMargin}" />
            <RowDefinition Height="Auto" />
            <RowDefinition Height="{ThemeResource SliderPostContentMargin}" />
        </Grid.RowDefinitions>
    ```

1.  1부의 노출 슬라이더용으로 만든 다각형과 동일한 다각형을 만듭니다. `Grid.RowDefinitions` 태그 뒤에 다각형을 추가합니다. `Grid.Row`를 “0”으로, `Grid.RowSpan`을 “3”으로, `Grid.ColumnSpan`을 “3”으로 설정합니다.

    **이전**

    ```xaml
    <Grid x:Name="HorizontalTemplate" MinHeight="44">
        <Grid.ColumnDefinitions>
            <ColumnDefinition Width="Auto" />
            <ColumnDefinition Width="Auto" />
            <ColumnDefinition Width="*" />
        </Grid.ColumnDefinitions>
        <Grid.RowDefinitions>
            <RowDefinition Height="18" />
            <RowDefinition Height="Auto" />
            <RowDefinition Height="18" />
        </Grid.RowDefinitions>
    ```

    **이후**

    ```xaml
    <Grid x:Name="HorizontalTemplate" MinHeight="44">
        <Grid.ColumnDefinitions>
            <ColumnDefinition Width="Auto" />
            <ColumnDefinition Width="Auto" />
            <ColumnDefinition Width="*" />
        </Grid.ColumnDefinitions>
        <Grid.RowDefinitions>
            <RowDefinition Height="18" />
            <RowDefinition Height="Auto" />
            <RowDefinition Height="18" />
        </Grid.RowDefinitions>
        <Polygon Grid.Row="0" Grid.RowSpan="3"  Grid.ColumnSpan="3" Stretch="Fill"
                    Points="0,20 200,20 200,0" HorizontalAlignment="Stretch"
                    VerticalAlignment="Center" Height="20" >
            <Polygon.Fill>
                <LinearGradientBrush StartPoint="0,0.5" EndPoint="1,0.5">
                    <LinearGradientBrush.GradientStops>
                        <GradientStop Offset="0" Color="Black" />
                        <GradientStop Offset="1" Color="White" />
                    </LinearGradientBrush.GradientStops>
                </LinearGradientBrush>
            </Polygon.Fill>
        </Polygon>
    ```

1. `Polygon.Fill` 설정을 제거합니다. `Fill`를 `"{TemplateBinding Background}"`로 설정합니다. 이렇게 하면 슬라이더의 `Background` 속성이 다각형의 `Fill` 속성으로 설정됩니다.

    **이전**

    ```xaml
        <Polygon Grid.Row="0" Grid.RowSpan="3"  Grid.ColumnSpan="3" Stretch="Fill"
                    Points="0,20 200,20 200,0" HorizontalAlignment="Stretch"
                    VerticalAlignment="Center" Height="20" >
            <Polygon.Fill>
                <LinearGradientBrush StartPoint="0,0.5" EndPoint="1,0.5">
                    <LinearGradientBrush.GradientStops>
                        <GradientStop Offset="0" Color="Black" />
                        <GradientStop Offset="1" Color="White" />
                    </LinearGradientBrush.GradientStops>
                </LinearGradientBrush>
            </Polygon.Fill>
        </Polygon>
    ```

    **이후**

    ```xaml
        <Polygon Grid.Row="0" Grid.RowSpan="3"  Grid.ColumnSpan="3" Stretch="Fill"
                    Points="0,20 200,20 200,0" HorizontalAlignment="Stretch"
                    VerticalAlignment="Center" Height="20"
                    Fill="{TemplateBinding Background}">
        </Polygon>
    ```

1. 추가한 다각형 다음에 `HorizontalTrackRect`라는 사각형이 있습니다. 사각형의 `Fill` 설정을 제거하여 사각형을 보이지 않게 하고 다각형 형태를 차단하지 않게 합니다. (컨트롤 템플릿은 마우스로 가리키기와 같은 상호 작용 시각적 이미지에도 사용되기 때문에 사각형을 완전히 제거하지는 않습니다.)

    **이전**

    ```xaml
        <Rectangle x:Name="HorizontalTrackRect"
                    Fill="{TemplateBinding Background}"
                    Height="{ThemeResource SliderTrackThemeHeight}"
                    Grid.Row="1"
                    Grid.ColumnSpan="3" />
    ```

    **이후**

    ```xaml
        <Rectangle x:Name="HorizontalTrackRect"
                    Height="{ThemeResource SliderTrackThemeHeight}"
                    Grid.Row="1"
                    Grid.ColumnSpan="3" />
    ```

    템플릿 작업을 완료했습니다! 이제는 슬라이더에 적용해야 합니다.

1. 노출 슬라이더를 업데이트해 보겠습니다.

    * 슬라이더의 `Template` 속성을 `"{StaticResource FancySliderControlTemplate}"`으로 설정합니다.
    * 슬라이더의 `Background="Transparent"` 설정을 제거합니다.
    * 슬라이더의 `Background`를 검은색에서 흰색으로 전환되는 선형 그라데이션으로 설정합니다.
    * 1부에서 만든 배경 다각형을 제거합니다.

    **이전**

    ```xaml
    <Polygon Grid.Row="2" Stretch="Fill"
                Points="0,20 200,20 200,0" HorizontalAlignment="Stretch"
                VerticalAlignment="Center" Height="20">
        <Polygon.Fill>
            <LinearGradientBrush StartPoint="0,0.5" EndPoint="1,0.5">
                <LinearGradientBrush.GradientStops>
                    <GradientStop Offset="0" Color="Black" />
                    <GradientStop Offset="1" Color="White" />
                </LinearGradientBrush.GradientStops>
            </LinearGradientBrush>
        </Polygon.Fill>
    </Polygon>
    <Slider Header="Exposure"
            Grid.Row="2" Background="Transparent" Foreground="Transparent"
            Value="{x:Bind item.Exposure, Mode=TwoWay}"
            Minimum="-2"
            Maximum="2" />
    ```

    **이후**

    ```xaml
    <Slider Header="Exposure"
            Grid.Row="2"  Foreground="Transparent"
            Value="{x:Bind item.Exposure, Mode=TwoWay}"
            Minimum="-2"
            Maximum="2"
            Template="{StaticResource FancySliderControlTemplate}">
        <Slider.Background>
            <LinearGradientBrush StartPoint="0,0.5" EndPoint="1,0.5">
                <LinearGradientBrush.GradientStops>
                    <GradientStop Offset="0" Color="Black" />
                    <GradientStop Offset="1" Color="White" />
                </LinearGradientBrush.GradientStops>
            </LinearGradientBrush>
        </Slider.Background>
    </Slider>
    ```

1. 온도 슬라이더와 동일한 업데이트를 실행합니다.

    **이전**

    ```xaml
    <Polygon Grid.Row="3" Stretch="Fill"
                Points="0,20 200,20 200,0" HorizontalAlignment="Stretch"
                VerticalAlignment="Center" Height="20">
        <Polygon.Fill>
            <LinearGradientBrush StartPoint="0,0.5" EndPoint="1,0.5">
                <LinearGradientBrush.GradientStops>
                    <GradientStop Offset="0" Color="Blue" />
                    <GradientStop Offset="1" Color="Yellow" />
                </LinearGradientBrush.GradientStops>
            </LinearGradientBrush>
        </Polygon.Fill>
    </Polygon>
    <Slider Header="Temperature"
            Grid.Row="3" Background="Transparent" Foreground="Transparent"
            Value="{x:Bind item.Temperature, Mode=TwoWay}"
            Minimum="-1"
            Maximum="1" />
    ```

    **이후**

    ```xaml
    <Slider Header="Temperature"
            Grid.Row="3" Foreground="Transparent"
            Value="{x:Bind item.Temperature, Mode=TwoWay}"
            Minimum="-1"
            Maximum="1"
            Template="{StaticResource FancySliderControlTemplate}">
        <Slider.Background>
            <LinearGradientBrush StartPoint="0,0.5" EndPoint="1,0.5">
                <LinearGradientBrush.GradientStops>
                    <GradientStop Offset="0" Color="Blue" />
                    <GradientStop Offset="1" Color="Yellow" />
                </LinearGradientBrush.GradientStops>
            </LinearGradientBrush>
        </Slider.Background>
    </Slider>
    ```

1. 색조 슬라이더와 동일한 업데이트를 실행합니다.

    **이전**

    ```xaml
    <Polygon Grid.Row="4" Stretch="Fill"
                Points="0,20 200,20 200,0" HorizontalAlignment="Stretch"
                VerticalAlignment="Center" Height="20">
        <Polygon.Fill>
            <LinearGradientBrush StartPoint="0,0.5" EndPoint="1,0.5">
                <LinearGradientBrush.GradientStops>
                    <GradientStop Offset="0" Color="Red" />
                    <GradientStop Offset="1" Color="Green" />
                </LinearGradientBrush.GradientStops>
            </LinearGradientBrush>
        </Polygon.Fill>
    </Polygon>
    <Slider Header="Tint"
            Grid.Row="4" Background="Transparent" Foreground="Transparent"
            Value="{x:Bind item.Tint, Mode=TwoWay}"
            Minimum="-1"
            Maximum="1" />
    ```

    **이후**

    ```xaml
    <Slider Header="Tint"
            Grid.Row="4" Foreground="Transparent"
            Value="{x:Bind item.Tint, Mode=TwoWay}"
            Minimum="-1"
            Maximum="1"
            Template="{StaticResource FancySliderControlTemplate}">
        <Slider.Background>
            <LinearGradientBrush StartPoint="0,0.5" EndPoint="1,0.5">
                <LinearGradientBrush.GradientStops>
                    <GradientStop Offset="0" Color="Red" />
                    <GradientStop Offset="1" Color="Green" />
                </LinearGradientBrush.GradientStops>
            </LinearGradientBrush>
        </Slider.Background>
    </Slider>
    ```

1. 앱을 컴파일하고 실행합니다.

    ![세계 최고의 슬라이더](../basics/images/xaml-basics/style-sliders-templates.png)

    보시다시피, 업데이트를 통해 다각형의 위치를 개선했습니다. 이제 다각형의 아래쪽이 슬라이더 위치 조정 컨트롤의 아래쪽에 정렬됩니다.

축하합니다. 이 자습서를 완료했습니다. 중간에 문제가 생겨서 최종 솔루션을 확인하려는 경우 [https://github.com/Microsoft/Windows-appsample-photo-lab](https://github.com/Microsoft/Windows-appsample-photo-lab)에서 전체 샘플을 볼 수 있습니다.
