---
author: muhsinking
title: 적응형 레이아웃 만들기 자습서
description: 이 문서는 XAML의 적응형 레이아웃의 기본 사항에 대해 설명합니다.
keywords: XAML, UWP, 시작
ms.author: mukin
ms.date: 08/30/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 000aa2d8f3684aa813b85076d9124a87a71b6a8c
ms.sourcegitcommit: 71e8eae5c077a7740e5606298951bb78fc42b22c
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/14/2018
ms.locfileid: "6673478"
---
# <a name="tutorial-create-adaptive-layouts"></a>자습서: 적응형 레이아웃 만들기

이 자습서에서는 XAML의 적응형 및 맞춤형 레이아웃 기능을 사용하여 모든 장치에 앱을 만드는 기본 사항을 다룹니다. VisualStateManager 및 AdaptiveTrigger 요소를 사용하여 새 DataTemplate을 만들고, 창 끌기 지점을 추가하고, 앱의 레이아웃을 조정하는 방법을 알아봅니다. 이러한 도구를 사용하여 소형 장치 화면에 맞는 이미지 편집 프로그램을 최적화합니다. 

사용할 이미지 편집 프로그램은 페이지/화면이 아래와 같이 2가지입니다.

**메인 페이지**, 각 이미지 파일에 대한 정보와 함께 사진 갤러리 보기를 표시합니다.

![MainPage](../basics/images/xaml-basics/mainpage.png)

**세부 정보 페이지**, 하나의 사진을 선택한 후에 표시합니다. 플라이아웃 편집 메뉴를 사용하면 사진을 수정하고, 이름을 변경하고, 저장할 수 있습니다.

![DetailPage](../basics/images/xaml-basics/detailpage.png)

## <a name="prerequisites"></a>필수 구성 요소

* Visual Studio 2017: [Visual Studio 2017 Community 다운로드(무료)](https://www.visualstudio.com/thank-you-downloading-visual-studio/?sku=Community&rel=15&campaign=WinDevCenter&ocid=wdgcx-windevcenter-community-download) 
* Windows 10 SDK(10.0.15063.468 이상): [최신 Windows SDK 다운로드(무료)](https://developer.microsoft.com/windows/downloads/windows-10-sdk)
* Windows Mobile 에뮬레이터: [Windows 10 Mobile 에뮬레이터 다운로드(무료)](https://developer.microsoft.com/en-us/windows/downloads/sdk-archive)

## <a name="part-0-get-the-starter-code-from-github"></a>파트 0: github에서 시작 코드 다운로드

이번 자습서에서는 PhotoLab 샘플의 간소화된 버전부터 시작합니다. 

1. 이동 [https://github.com/Microsoft/Windows-appsample-photo-lab](https://github.com/Microsoft/Windows-appsample-photo-lab). 샘플을 볼 수 있는 GitHub 페이지로 이동하게 됩니다. 
2. 그런 다음 샘플을 복제 또는 다운로드해야 합니다. **복제 또는 다운로드** 버튼을 클릭합니다. 하위 메뉴가 나타납니다.
    <figure>
        <img src="../basics/images/xaml-basics/clone-repo.png" alt="The Clone or download menu on GitHub">
        <figcaption>Photo lab 샘플의 GitHub 페이지에 있는 <b>복제 또는 다운로드</b> 메뉴</figcaption>
    </figure>

    **GitHub가 익숙하지 않은 경우:**
    
    a. **ZIP 다운로드**를 클릭하고 파일을 로컬에 저장합니다. 그러면 필요한 프로젝트 파일이 모두 포함된 .zip 파일이 다운로드됩니다.
    b. 파일의 압축을 풉니다. 파일 탐색기를 사용해 앞에서 다운로드한 .zip 파일로 이동하여 마우스 오른쪽 버튼을 클릭한 후 **모두 압축 풀기**를 선택합니다. c. 샘플의 로컬 사본으로 탐색한 후 `Windows-appsample-photo-lab-master\xaml-basics-starting-points\adaptive-layout` 디렉토리로 이동합니다.    

    **GitHub가 익숙한 경우:**

    a. 리포지토리의 마스터 분기를 로컬에 복제합니다.
    b. `Windows-appsample-photo-lab\xaml-basics-starting-points\adaptive-layout` 디렉토리로 이동합니다.

3. `Photolab.sln`을 클릭하여 프로젝트를 엽니다.

## <a name="part-1-run-the-mobile-emulator"></a>파트 1: 모바일 에뮬레이터 실행

Visual Studio 툴바에서 솔루션 플랫폼이 ARM이 아닌 x86 또는 x64로 설정되어 있는지 확인한 다음 대상 장치를 로컬 컴퓨터에서 설치한 모바일 에뮬레이터 중 하나로 변경합니다(예: Mobile Emulator 10.0.15063 WVGA 5인치 1GB). **F5**를 눌러 선택한 모바일 에뮬레이터에서 사진 갤러리 앱을 실행합니다.

앱이 시작되자마자, 앱이 작동하는 동안 작은 뷰포트에서 보기에 좋지 않다는 점을 느낄 수 있습니다. 유연한 그리드 요소는 표시되는 열 수를 줄임으로써 제한된 화면 영역을 수용하려고 시도하지만, 이렇게 작은 뷰포트에 대해 영감을 주지 않고 부적합한 레이아웃으로 남게 됩니다.

![모바일 레이아웃: 이후](../basics/images/xaml-basics/adaptive-layout-mobile-before.png)

## <a name="part-2-build-a-tailored-mobile-layout"></a>파트 2: 맞춤형 모바일 레이아웃 빌드
소형 장치에서 이 앱을 멋지게 보이게 만들려면 XAML 페이지에서 모바일 장치가 감지된 경우에만 사용되는 별도의 스타일 집합을 만들어야 합니다.

### <a name="create-a-new-datatemplate"></a>새 DataTemplate 만들기
이미지를 위한 새로운 DataTemplate을 생성하여 응용 프로그램의 갤러리 보기를 조정할 것입니다. 솔루션 탐색기에서 MainPage.xaml을 열고 **Page.Resources** 태그에 다음 코드를 추가합니다.

```XAML
<DataTemplate x:Key="ImageGridView_MobileItemTemplate"
              x:DataType="local:ImageFileInfo">

    <!-- Create image grid -->
    <Grid Height="{Binding ItemSize, ElementName=page}"
          Width="{Binding ItemSize, ElementName=page}">
        
        <!-- Place image in grid, stretching it to fill the pane-->
        <Image x:Name="ItemImage"
               Source="{x:Bind ImagePreview}"
               Stretch="UniformToFill">
        </Image>

    </Grid>
</DataTemplate>
```

이 갤러리 템플릿은 이미지 주위의 경계선을 없애고 각 미리 보기 아래 이미지 메타데이터(파일 이름, 평점 등)를 제거하여 화면 공간을 절약합니다. 대신 각 미리 보기를 간단한 사각형으로 표시합니다.

### <a name="add-metadata-to-a-tooltip"></a>도구 설명에 메타데이터 추가
사용자가 각 이미지의 메타데이터에 액세스할 수 있게 할 것이므로 각 이미지 항목에 도구 설명을 추가합니다. 방금 생성한 DataTemplate의 **이미지** 태그 내에 다음 코드를 추가합니다.

```XAML
<Image ...>

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
</Image>
```

미리 보기 위로 마우스를 올리거나 터치 스크린을 길게 누르면 이미지의 제목, 파일 유형 및 크기가 표시됩니다.

### <a name="add-a-visualstatemanager-and-statetrigger"></a>VisualStateManager 및 StateTrigger 추가

이제 데이터를 위한 새로운 레이아웃을 만들었지만, 앱은 현재 기본 스타일보다 이 레이아웃을 사용해야 하는 시점을 알지 못합니다. 이 문제를 해결하려면 **VisualStateManager**를 추가해야 합니다. 페이지의 루트 요소인 **RelativePanel**에 다음 코드를 추가합니다.

```XAML
<VisualStateManager.VisualStateGroups>
    <VisualStateGroup>

        <!-- Add a new VisualState for mobile devices -->
        <VisualState x:Key="Mobile">

            <!-- Trigger visualstate when a mobile device is detected -->
            <VisualState.StateTriggers>
                <local:MobileScreenTrigger InteractionMode="Touch" />
            </VisualState.StateTriggers>

        </VisualState>
    </VisualStateGroup>
</VisualStateManager.VisualStateGroups>
```

이는 앱이 모바일 장치에서 실행 중임을 감지하면 트리거되는 새로운 **VisualState** 및 **StateTrigger**를 추가합니다(이 작업의 논리는 PhotoLab 디렉토리에 제공되는 MobileScreenTrigger.cs에서 찾을 수 있음). **StateTrigger**가 시작될 때 앱은 **VisualState**에 할당된 레이아웃 속성을 사용합니다.

### <a name="add-visualstate-setters"></a>VisualState setter 추가
다음으로 **VisualState** setter를 사용하여 상태가 트리거될 때 적용할 속성을 **VisualStateManager**에게 알려 줍니다. 각 setter는 특정 XAML 요소의 속성 하나를 대상으로 하고 지정된 값으로 설정합니다. 이 코드를 **VisualState.StateTriggers** 요소 아래 방금 생성한 모바일 **VisualState**에 추가합니다. 

```XAML
<VisualStateManager.VisualStateGroups>
    <VisualStateGroup>

        <VisualState x:Key="Mobile">
            ...

            <!-- Add setters for mobile visualstate -->
            <VisualState.Setters>

                <!-- Move GridView about the command bar -->
                <Setter Target="ImageGridView.(RelativePanel.Above)"
                        Value="MainCommandBar" />

                <!-- Switch to mobile layout -->
                <Setter Target="ImageGridView.ItemTemplate"
                        Value="{StaticResource ImageGridView_MobileItemTemplate}" />

                <!-- Switch to mobile container styles -->
                <Setter Target="ImageGridView.ItemContainerStyle"
                        Value="{StaticResource ImageGridView_MobileItemContainerStyle}" />

                <!-- Move command bar to bottom of the screen -->
                <Setter Target="MainCommandBar.(RelativePanel.AlignBottomWithPanel)"
                        Value="True" />
                <Setter Target="MainCommandBar.(RelativePanel.AlignLeftWithPanel)"
                        Value="True" />
                <Setter Target="MainCommandBar.(RelativePanel.AlignRightWithPanel)"
                        Value="True" />

                <!-- Adjust the zoom slider to fit mobile screens -->
                <Setter Target="ZoomSlider.Minimum"
                        Value="80" />
                <Setter Target="ZoomSlider.Maximum"
                        Value="180" />
                <Setter Target="ZoomSlider.TickFrequency"
                        Value="20" />
                <Setter Target="ZoomSlider.Value"
                        Value="100" />
            </VisualState.Setters>

        </VisualState>
    </VisualStateGroup>
</VisualStateManager.VisualStateGroups>

```

이 setter는 이미지 갤러리의 **ItemTemplate**을 첫 번째 파트에서 생성한 **DataTemplate**으로 설정하고, 명령 모음과 확대/축소 슬라이더를 화면 하단에 정렬하여 휴대폰 화면에서 엄지 손가락으로 훨씬 쉽게 접근할 수 있게 해 줍니다.

### <a name="run-the-app"></a>앱 실행
이제 모바일 에뮬레이터를 사용하여 앱을 실행해 봅니다. 새 레이아웃이 성공적으로 표시됩니까? 아래 그림과 같이 작은 미리 보기 이미지가 표시됩니다. 그래도 이전 레이아웃이 표시되는 경우 **VisualStateManager** 코드에 오타가 있을 수 있습니다.

![모바일 레이아웃: 이후](../basics/images/xaml-basics/adaptive-layout-mobile-after.png)

## <a name="part-3-adapt-to-multiple-window-sizes-on-a-single-device"></a>파트 3: 하나의 장치에서 여러 개의 창 크기 조정
새로운 맞춤형 레이아웃을 만들면 모바일 장치의 반응형 디자인 문제를 해결할 수 있지만 데스크톱과 태블릿의 경우는 어떨까요? 앱이 전체 화면으로 보기에는 좋지만 사용자가 창을 축소하면 어색한 인터페이스로 표시될 수 있습니다. **VisualStateManager**를 사용하여 하나의 장치에서 여러 창 크기를 조정함으로써 최종 사용자 환경을 항상 적절한 모습과 느낌으로 유지할 수 있습니다.

![작은 창: 이전](../basics/images/xaml-basics/adaptive-layout-small-before.png)

### <a name="add-window-snap-points"></a>창 끌기 지점 추가
첫 번째 단계는 서로 다른 **VisualStates**가 트리거되는 '끌기 지점'을 정의하는 것입니다. 솔루션 탐색기에서 App.xaml을 열고 **Application** 태그 간에 다음 코드를 추가합니다.

```XAML
<Application.Resources>
    <!--  window width adaptive snap points  -->
    <x:Double x:Key="MinWindowSnapPoint">0</x:Double>
    <x:Double x:Key="MediumWindowSnapPoint">641</x:Double>
    <x:Double x:Key="LargeWindowSnapPoint">1008</x:Double>
</Application.Resources>
```

이는 3개의 끌기 지점을 제공하며, 이 끌기 지점을 사용하면 세 가지 범위의 창 크기에 대해 새로운 **VisualStates**를 만들 수 있습니다.
+ 작음(0~640 픽셀 너비)
+ 보통(641~1007 픽셀 너비)
+ 큼(> 1007 픽셀 너비)

### <a name="create-new-visualstates-and-statetriggers"></a>새 VisualStates 및 StateTriggers 만들기
다음으로, 각 끌기 지점에 해당하는 **VisualStates** 및 **StateTriggers**를 만듭니다. MainPage.xaml에서는, 방금 파트 2에서 만든 **VisualStateManager**에 다음 코드를 추가합니다.

```XAML
<VisualStateManager.VisualStateGroups>
    <VisualStateGroup>
    ...

        <!-- Large window VisualState -->
        <VisualState x:Key="LargeWindow">

            <!-- Large window trigger -->
            <VisualState.StateTriggers>
                <AdaptiveTrigger MinWindowWidth="{StaticResource LargeWindowSnapPoint}"/>
            </VisualState.StateTriggers>
     
        </VisualState>

        <!-- Medium window VisualState -->
        <VisualState x:Key="MediumWindow">

            <!-- Medium window trigger -->
            <VisualState.StateTriggers>
                <AdaptiveTrigger MinWindowWidth="{StaticResource MediumWindowSnapPoint}"/>
            </VisualState.StateTriggers>
        
        </VisualState>

        <!-- Small window VisualState -->
        <VisualState x:Key="SmallWindow">

            <!-- Small window trigger -->
            <VisualState.StateTriggers >
                <AdaptiveTrigger MinWindowWidth="{StaticResource MinWindowSnapPoint}"/>
            </VisualState.StateTriggers>

        </VisualState>

    </VisualStateGroup>
</VisualStateManager.VisualStateGroups>
```

### <a name="add-setters"></a>setter 추가
마지막으로, 이러한 setter를 추가 **SmallWindow** 상태에 추가합니다.

```XAML

<VisualState x:Key="SmallWindow">
    ...

    <!-- Small window setters -->
    <VisualState.Setters>

        <!-- Apply mobile itemtemplate and styles -->
        <Setter Target="ImageGridView.ItemTemplate"
                Value="{StaticResource ImageGridView_MobileItemTemplate}" />
        <Setter Target="ImageGridView.ItemContainerStyle"
                Value="{StaticResource ImageGridView_MobileItemContainerStyle}" />

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

</VisualState>

```

이 setter는 뷰포트의 너비가 641 픽셀보다 작을 때마다 모바일 **DataTemplate** 및 스타일을 데스크톱 앱에 적용합니다. 또한 작은 화면에 더 잘 맞도록 확대/축소 슬라이더를 조정합니다.

### <a name="run-the-app"></a>앱 실행

Visual Studio 툴바에서 대상 장치를 **로컬 컴퓨터**로 설정하고 앱을 실행합니다. 앱이 로드되면 창의 크기를 변경해 봅니다. 창을 작은 크기로 축소하면 앱이 파트 2에서 만든 모바일 레이아웃으로 전환되는 것을 볼 수 있습니다.

![작은 창: 이후](../basics/images/xaml-basics/adaptive-layout-small-after.png)

## <a name="going-further"></a>더 나아가기

이제 이 실습을 완료했으므로 독자적으로 더 많은 실습을 수행할 수 있는 충분한 적응형 레이아웃 지식을 얻게 되었습니다. 이전에 추가한 모바일 전용 도구 설명에 평점 컨트롤을 추가해 보세요. 또는 더 큰 문제를 해결하려면 큰 화면 크기에 맞게 레이아웃을 최적화하세요(TV 화면 또는 Surface Studio).

문제가 있는 경우 [XAML을 사용하여 페이지 레이아웃 정의](../layout/layouts-with-xaml.md)의 이 섹션에서 더 많은 지침을 찾을 수 있습니다.

+ [시각적 상태 및 상태 트리거](https://docs.microsoft.com/en-us/windows/uwp/layout/layouts-with-xaml#visual-states-and-state-triggers)
+ [맞춤형 레이아웃](https://docs.microsoft.com/en-us/windows/uwp/layout/layouts-with-xaml#tailored-layouts)

또는 초기 사진 편집 앱의 빌드 방법에 대해 자세히 알아보려면 XAML [사용자 인터페이스](../basics/xaml-basics-ui.md) 및 [데이터 바인딩](../../data-binding/xaml-basics-data-binding.md)의 이 자습서를 확인하세요.

## <a name="get-the-final-version-of-the-photolab-sample"></a>PhotoLab 샘플의 최종 버전 다운로드

이 자습서에서는 전체적인 사진 편집 앱을 빌드하지 않으므로 [최종 버전](https://github.com/Microsoft/Windows-appsample-photo-lab)을 확인하여 사용자 지정 애니메이션 및 휴대폰 지원과 같은 다른 기능을 살펴보세요.