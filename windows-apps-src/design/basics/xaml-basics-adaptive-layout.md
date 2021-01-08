---
title: 적응형 레이아웃 만들기 자습서
description: XAML에서 적응형 레이아웃 기능을 사용하여 창 크기에 적합한 앱을 만드는 방법을 알아봅니다.
keywords: XAML, UWP, 시작
ms.date: 08/20/2020
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: e9db25c822d524ec36c6e512e132e49a69a2bd7d
ms.sourcegitcommit: 7aa0e1108fd1a19ebc5632acbc9f66ea9af2b321
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 12/18/2020
ms.locfileid: "97691523"
---
# <a name="tutorial-create-adaptive-layouts"></a>자습서: 적응형 레이아웃 만들기

이 자습서에서는 XAML의 레이아웃 기능을 사용하여 어떤 크기에도 잘 어울리는 앱을 만드는 기본 사항을 다룹니다. 창 중단점을 추가하고, 새 DataTemplate을 만들고, VisualStateManager 클래스를 사용하여 앱의 레이아웃을 조정하는 방법을 알아봅니다. 이 도구를 사용하여 작은 창 크기에 맞게 이미지 편집 프로그램을 최적화합니다.

이미지 편집 프로그램에는 다음과 같은 두 페이지가 있습니다. _기본 페이지_ 는 각 이미지 파일에 대한 일부 정보와 함께 사진 갤러리 보기를 표시합니다.

![사진 랩 기본 페이지의 스크린샷.](../basics/images/xaml-basics/mainpage.png)

*세부 정보 페이지* 는 선택한 단일 사진을 표시합니다. 플라이아웃 편집 메뉴를 사용하면 사진을 수정하고, 이름을 변경하고, 저장할 수 있습니다.

![사진 랩 세부 정보 페이지의 스크린샷.](../basics/images/xaml-basics/detailpage.png)

## <a name="prerequisites"></a>필수 구성 요소

+ Visual Studio 2019: [Visual Studio 2019 다운로드](https://visualstudio.microsoft.com/downloads/)(Community Edition은 무료입니다.)
+ Windows 10 SDK(10.0.17763.0 이상):  [최신 Windows SDK(무료)](https://developer.microsoft.com/windows/downloads/windows-10-sdk)
+ Windows 10, 버전 1809 이상

## <a name="part-0-get-the-starter-code-from-github"></a>0부: github에서 시작 코드 다운로드

이 자습서에서는 PhotoLab 샘플의 간소화된 버전부터 시작합니다.

1. GitHub의 샘플 페이지([https://github.com/Microsoft/Windows-appsample-photo-lab](https://github.com/Microsoft/Windows-appsample-photo-lab))로 이동합니다.
2. 다음으로, 샘플을 복제 또는 다운로드해야 합니다. **복제 또는 다운로드** 단추를 클릭합니다. 하위 메뉴가 나타납니다.
    ![PhotoLab 샘플의 GitHub 페이지에 있는 복제 또는 다운로드 메뉴](images/xaml-basics/clone-repo.png)

    **GitHub에 익숙하지 않은 경우:**

    a. **ZIP 다운로드** 를 클릭하고 파일을 로컬에 저장합니다. 그러면 필요한 프로젝트 파일이 모두 포함된 .zip 파일이 다운로드됩니다.

    b. 파일의 압축을 풉니다. 파일 탐색기를 사용하여 방금 다운로드한 .zip 파일을 찾아 마우스 오른쪽 단추로 클릭하고, **압축 풀기...** 를 선택합니다.

    c. 샘플의 로컬 복사본을 찾고, `Windows-appsample-photo-lab-master\xaml-basics-starting-points\adaptive-layout` 디렉터리로 이동합니다.

    **GitHub에 익숙한 경우:**

    a. 리포지토리의 기본 분기를 로컬에 복제합니다.

    b. `Windows-appsample-photo-lab\xaml-basics-starting-points\adaptive-layout` 디렉터리로 이동합니다.

3. `Photolab.sln`을 두 번 클릭하여 Visual Studio에서 솔루션을 엽니다.

## <a name="part-1-define-window-breakpoints"></a>1부: 창 중단점 정의

앱을 실행합니다. 전체 화면에는 잘 어울리지만 창을 너무 작게 축소하는 경우 UI(사용자 인터페이스)가 적합하지 않습니다. [VisualStateManager](/uwp/api/windows.ui.xaml.visualstatemanager)를 사용하여 다양한 창 크기게 맞게 UI를 조정하면 최종 사용자 환경을 항상 멋진 모습과 느낌으로 유지할 수 있습니다.

![작은 창: 이전](../basics/images/xaml-basics/adaptive-layout-small-before.png)

앱 레이아웃에 대한 자세한 내용은 문서에서 [레이아웃](../layout/index.md) 섹션을 참조하세요.

### <a name="add-window-breakpoints"></a>창 중단점 추가

첫 번째 단계에서는 다양한 시각적 상태가 적용되는 중단점을 정의합니다. 소형, 중형 및 대형 화면의 중단점에 대한 자세한 내용은 [화면 크기 및 중단점](../layout/screen-sizes-and-breakpoints-for-responsive-design.md)을 참조하세요.

솔루션 탐색기에서 App.xaml을 열고, 닫는 `</ResourceDictionary>` 태그 바로 앞에 `MergedDictionaries` 뒤에 다음 코드를 추가합니다.

```xaml
    <!--  Window width adaptive breakpoints.  -->
    <x:Double x:Key="MinWindowBreakpoint">0</x:Double>
    <x:Double x:Key="MediumWindowBreakpoint">641</x:Double>
    <x:Double x:Key="LargeWindowBreakpoint">1008</x:Double>
```

이렇게 하면 중단점이 3개 생성되어, 3가지 범위의 창 크기에 대해 새로운 시각적 상태를 만들 수 있습니다.

+ 작음(0-640픽셀 너비)
+ 보통(641-1007픽셀 너비)
+ 큼(1007픽셀을 초과하는 너비)

이 예에서는 작은 창 크기에 대해서만 새로운 모양을 만듭니다. 중형 및 대형 크기는 동일한 모양을 사용합니다.

## <a name="part-2-add-a-data-template-for-small-window-sizes"></a>2부: 작은 창 크기에 대한 데이터 템플릿 추가

작은 창에서도 이 앱이 멋지게 보이도록 하기 위해, 사용자가 창을 축소하는 경우 이미지 갤러리 보기의 이미지가 표시되는 방식을 최적화하는 새 데이터 템플릿을 만들 수 있습니다.

### <a name="create-a-new-datatemplate"></a>새 DataTemplate 만들기

 솔루션 탐색기에서 MainPage.xaml을 열고 `Page.Resources` 태그에 다음 코드를 추가합니다.

```xaml
<DataTemplate x:Key="ImageGridView_SmallItemTemplate"
              x:DataType="local:ImageFileInfo">

    <!-- Create image grid -->
    <Grid Height="{Binding ItemSize, ElementName=page}"
          Width="{Binding ItemSize, ElementName=page}">

        <!-- Place image in grid, stretching it to fill the pane-->
        <Image x:Name="ItemImage"
               Source="{x:Bind ImageSource, Mode=OneWay}"
               Stretch="UniformToFill">
        </Image>

    </Grid>
</DataTemplate>
```

이 갤러리 템플릿은 이미지 주위의 경계선을 없애고 각 미리 보기 아래의 이미지 메타데이터(파일 이름, 평점 등)를 제거하여 화면 공간을 절약합니다. 대신 각 미리 보기를 간단한 사각형으로 표시합니다.

### <a name="add-metadata-to-a-tooltip"></a>도구 설명에 메타데이터 추가

사용자가 각 이미지의 메타데이터에 액세스할 수 있어야 하므로 각 이미지 항목에 도구 설명을 추가합니다. 방금 만든 DataTemplate의 `Image` 태그 내에 다음 코드를 추가합니다.

```xaml
    <!-- Add a tooltip to the image that displays metadata -->
    <ToolTipService.ToolTip>
        <ToolTip x:Name="tooltip">

            <!-- Arrange tooltip elements vertically -->
            <StackPanel Orientation="Vertical"
                        Grid.Row="1">

                <!-- Image title -->
                <TextBlock Text="{x:Bind ImageTitle, Mode=OneWay}"
                           HorizontalAlignment="Center"
                           Style="{StaticResource SubtitleTextBlockStyle}" />

                <!-- Arrange elements horizontally -->
                <StackPanel Orientation="Horizontal"
                            HorizontalAlignment="Center">

                    <!-- Image file type -->
                    <TextBlock Text="{x:Bind ImageFileType}"
                               HorizontalAlignment="Center"
                               Style="{StaticResource CaptionTextBlockStyle}" />

                    <!-- Image dimensions -->
                    <TextBlock Text="{x:Bind ImageDimensions}"
                               HorizontalAlignment="Center"
                               Style="{StaticResource CaptionTextBlockStyle}"
                               Margin="8,0,0,0" />
                </StackPanel>
            </StackPanel>
        </ToolTip>
    </ToolTipService.ToolTip>
```

마우스로 미리 보기를 가리키거나 터치 스크린을 길게 누르면 이미지의 제목, 파일 유형 및 크기가 표시됩니다.

## <a name="part-3-define-visual-states"></a>3부: 시각적 상태 정의

이제 데이터를 위한 새 레이아웃을 만들었지만, 앱은 현재 기본 스타일 대신 이 레이아웃을 사용해야 하는 시기를 알지 못합니다. 이 문제를 해결하려면 [VisualStateManager](/uwp/api/windows.ui.xaml.visualstatemanager)와 [VisualState](/uwp/api/windows.ui.xaml.visualstate) 정의를 추가해야 합니다.

### <a name="add-a-visualstatemanager"></a>VisualStateManager 추가

페이지의 루트 요소인 `RelativePanel`에 다음 코드를 추가합니다.

```xaml
<VisualStateManager.VisualStateGroups>
    <VisualStateGroup>
    ...

        <!-- Large window VisualState -->
        <VisualState>

        </VisualState>

        <!-- Medium window VisualState -->
        <VisualState>

        </VisualState>

        <!-- Small window VisualState -->
        <VisualState>

        </VisualState>

    </VisualStateGroup>
</VisualStateManager.VisualStateGroups>
```

### <a name="create-statetriggers-to-apply-the-visual-state"></a>시각적 상태를 적용하는 StateTrigger 생성

다음으로 각 맞춤 지점에 해당하는 `StateTriggers`를 만듭니다. MainPage.xaml에서 각 `VisualState`에 다음 코드를 추가합니다.

```xaml
<VisualStateManager.VisualStateGroups>
    <VisualStateGroup>
    ...

        <!-- Large window VisualState -->
        <VisualState>

            <!-- Large window trigger -->
            <VisualState.StateTriggers>
                <AdaptiveTrigger MinWindowWidth="{StaticResource LargeWindowBreakpoint}"/>
            </VisualState.StateTriggers>

        </VisualState>

        <!-- Medium window VisualState -->
        <VisualState>

            <!-- Medium window trigger -->
            <VisualState.StateTriggers>
                <AdaptiveTrigger MinWindowWidth="{StaticResource MediumWindowBreakpoint}"/>
            </VisualState.StateTriggers>

        </VisualState>

        <!-- Small window VisualState -->
        <VisualState>

            <!-- Small window trigger -->
            <VisualState.StateTriggers >
                <AdaptiveTrigger MinWindowWidth="{StaticResource MinWindowBreakpoint}"/>
            </VisualState.StateTriggers>

        </VisualState>

    </VisualStateGroup>
</VisualStateManager.VisualStateGroups>
```

각 시각적 상태가 트리거되면 앱은 활성 `VisualState`에 할당된 레이아웃 속성을 사용합니다.

### <a name="set-properties-for-each-visual-state"></a>각 시각적 상태의 속성 설정

마지막으로 상태가 트리거될 때 어떤 속성을 적용할지를 `VisualStateManager`에 알리는 각 시각적 상태에 대한 속성을 설정합니다. 각 setter는 특정 XAML 요소의 속성 하나를 대상으로 삼아 지정된 값으로 설정합니다. 다음 코드를 방금 만든 `SmallWindow` 시각적 상태에, `StateTriggers` 뒤에 추가합니다.

```xaml
    <!-- Small window setters -->
    <VisualState.Setters>

        <!-- Apply small template and styles -->
        <Setter Target="ImageGridView.ItemTemplate"
                Value="{StaticResource ImageGridView_SmallItemTemplate}" />
        <Setter Target="ImageGridView.ItemContainerStyle"
                Value="{StaticResource ImageGridView_SmallItemContainerStyle}" />

        <!-- Adjust the zoom slider to fit small windows-->
        <Setter Target="ZoomSlider.Minimum"
                Value="80" />
        <Setter Target="ZoomSlider.Maximum"
                Value="180" />
        <Setter Target="ZoomSlider.TickFrequency"
                Value="20" />
        <Setter Target="ZoomSlider.Value"
                Value="100" />
    </VisualState.Setters>
```

이러한 setter는 이미지 갤러리의 `ItemTemplate`을 이전 섹션에서 만든 새 `DataTemplate`으로 설정합니다. 또한 작은 화면에 더 잘 맞도록 확대/축소 슬라이더를 조정합니다.

### <a name="run-the-app"></a>앱 실행

앱을 실행합니다. 앱이 로드되면 창의 크기를 변경해 봅니다. 창을 작은 크기로 축소하면 앱이 2부에서 만든 작은 레이아웃으로 전환되는 것을 볼 수 있습니다.

![작은 창: 이후](../basics/images/xaml-basics/adaptive-layout-small-after.png)

## <a name="going-further"></a>더 나아가기

이제 이 실습을 완료했으므로, 여러분은 혼자 더 많은 실습을 수행할 수 있는 충분한 적응형 레이아웃 지식을 얻었습니다. 더 큰 문제를 해결하려면 Surface Hub와 같은 더 큰 화면 크기에 맞게 레이아웃을 최적화해보세요. Surface Hub 레이아웃을 테스트하려면 [Visual Studio를 사용하여 Surface Hub 앱 테스트](../../debug-test-perf/test-surface-hub-apps-using-visual-studio.md)를 참조하세요.

문제가 발생하는 경우 [XAML을 사용한 반응형 레이아웃](../layout/layouts-with-xaml.md) 섹션에서 더 많은 지침을 확인할 수 있습니다.

또는 초기 사진 편집 앱의 빌드 방법에 대해 자세히 알아보려면 XAML [사용자 인터페이스](../basics/xaml-basics-ui.md) 및 [데이터 바인딩](../../data-binding/xaml-basics-data-binding.md)의 이 자습서를 확인하세요.

## <a name="get-the-final-version-of-the-photolab-sample"></a>PhotoLab 샘플의 최종 버전 다운로드

이 자습서에서는 전체적인 사진 편집 앱을 빌드하지 않으므로 [최종 버전](https://github.com/Microsoft/Windows-appsample-photo-lab)을 확인하여 사용자 지정 애니메이션 및 스타일 같은 다른 기능을 살펴보세요.
