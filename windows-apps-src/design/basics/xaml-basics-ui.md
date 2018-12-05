---
title: 사용자 인터페이스 만들기 자습서
description: 이 문서는 XAML의 사용자 인터페이스 빌드 기본 사항에 대해 설명합니다.
keywords: XAML, UWP, 시작
ms.date: 08/30/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 1bae8455f1062b3ad62aeac3807c6c58ae274a1b
ms.sourcegitcommit: c01c29cd97f1cbf050950526e18e15823b6a12a0
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 12/05/2018
ms.locfileid: "8688920"
---
# <a name="tutorial-create-a-user-interface"></a>자습서: 사용자 인터페이스 만들기

이 자습서에서는 이미지 편집 프로그램의 기본 UI를 만드는 방법을 알아봅니다. 

+ XAML 디자이너, 도구 상자, XAML 편집기, 속성 패널 및 문서 개요 등의 Visual Studio XAML 도구를 사용하여 UI에 컨트롤 및 콘텐츠 추가
+ RelativePanel, Grid 및 StackPanel과 같은 가장 일반적인 XAML 레이아웃 패널 일부를 활용합니다.

이미지 편집 프로그램은 2페이지/화면을 갖고 있습니다.

**메인 페이지**, 각 이미지 파일에 대한 정보와 함께 사진 갤러리 보기를 표시합니다.

![MainPage](images/xaml-basics/mainpage.png)

**세부 정보 페이지**, 하나의 사진을 선택한 후에 표시합니다. 플라이아웃 편집 메뉴를 사용하면 사진을 수정하고, 이름을 변경하고, 저장할 수 있습니다.

![DetailPage](images/xaml-basics/detailpage.png)


## <a name="prerequisites"></a>필수 구성 요소

* Visual Studio 2017: [Visual Studio 2017 Community 다운로드(무료)](https://www.visualstudio.com/thank-you-downloading-visual-studio/?sku=Community&rel=15&campaign=WinDevCenter&ocid=wdgcx-windevcenter-community-download) 
* Windows 10 SDK(10.0.15063.468 이상): [최신 Windows SDK 다운로드(무료)](https://developer.microsoft.com/windows/downloads/windows-10-sdk)

## <a name="part-0-get-the-starter-code-from-github"></a>파트 0: github에서 시작 코드 다운로드

이번 자습서에서는 PhotoLab 샘플의 간소화된 버전부터 시작합니다. 

1. 이동 [https://github.com/Microsoft/Windows-appsample-photo-lab](https://github.com/Microsoft/Windows-appsample-photo-lab). 샘플을 볼 수 있는 GitHub 페이지로 이동하게 됩니다. 
2. 그런 다음 샘플을 복제 또는 다운로드해야 합니다. **복제 또는 다운로드** 버튼을 클릭합니다. 하위 메뉴가 나타납니다.
    <figure>
        <img src="images/xaml-basics/clone-repo.png" alt="The Clone or download menu on GitHub">
        <figcaption>Photo lab 샘플의 GitHub 페이지에 있는 <b>복제 또는 다운로드</b> 메뉴</figcaption>
    </figure>

    **GitHub가 익숙하지 않은 경우:**
    
    a. **ZIP 다운로드**를 클릭하고 파일을 로컬에 저장합니다. 그러면 필요한 프로젝트 파일이 모두 포함된 .zip 파일이 다운로드됩니다.
    b. 파일의 압축을 풉니다. 파일 탐색기를 사용해 앞에서 다운로드한 .zip 파일로 이동하여 마우스 오른쪽 버튼을 클릭한 후 **모두 압축 풀기**를 선택합니다. c. 샘플의 로컬 사본으로 탐색한 후 `Windows-appsample-photo-lab-master\xaml-basics-starting-points\user-interface` 디렉토리로 이동합니다.    

    **GitHub가 익숙한 경우:**

    a. 리포지토리의 마스터 분기를 로컬에 복제합니다.
    b. `Windows-appsample-photo-lab\xaml-basics-starting-points\user-interface` 디렉토리로 이동합니다.

3. `Photolab.sln`을 클릭하여 프로젝트를 엽니다.

## <a name="part-1-add-a-textblock-using-xaml-designer"></a>파트 1: XAML 디자이너를 사용하여 TextBlock 추가

Visual Studio에서는 XAML UI를 쉽게 만들 수 있는 여러 가지 도구를 제공합니다. XAML 디자이너를 사용하면 컨트롤을 디자인 화면으로 드래그하여 앱을 실행하기 전에 어떤 모습인지 볼 수 있습니다. 속성 패널을 사용하면 디자이너에서 활성화된 컨트롤의 모든 속성을 보고 설정할 수 있습니다. 문서 개요는 UI에 대한 XAML 시각적 트리의 부모-자식 구조를 보여 줍니다. XAML 편집기를 사용하면 XAML 태그를 직접 입력하고 수정할 수 있습니다.

다음은 Visual Studio UI의 레이블이 있는 도구입니다.

![Visual Studio 레이아웃](images/xaml-basics/visual-studio-tools.png)

이러한 각 도구로 UI를 쉽게 만들 수 있으므로 이 자습서에서는 이 도구를 모두 사용합니다. 먼저 XAML 디자이너를 사용하여 컨트롤을 추가합니다. 

**XAML 디자이너를 사용하여 컨트롤 추가:**

1. 솔루션 탐색기에서 **MainPage.xaml**을 두 번 클릭해 엽니다. 이는 UI 요소가 추가되지 않은 상태에서 앱의 메인 페이지를 보여 줍니다.

2. 더 진행하기 전에 Visual Studio를 약간 조정해야 합니다.

    - 솔루션 플랫폼이 ARM이 아닌 x86 또는 x64로 설정되었는지 확인합니다.
    - 13.3인치 데스크톱 미리 보기를 표시하도록 기본 페이지 XAML 디자이너를 설정합니다.

    여기에 표시된 것처럼 창 위쪽에 두 설정이 모두 표시되어야 합니다.

    ![VS 설정](images/xaml-basics/layout-vs-settings.png)

    지금 앱을 실행할 수는 있지만 많이 볼 수는 없습니다. 좀 더 재미있는 UI 요소를 추가해 보겠습니다.

3. 도구 상자에서 **공용 XAML 컨트롤**을 확장하고 [TextBlock](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.textblock) 컨트롤을 찾습니다. TextBlock을 페이지의 왼쪽 위 모서리 근처의 디자인 화면으로 드래그합니다.

    TextBlock이 페이지에 추가되고 디자이너는 원하는 레이아웃에서 최상의 추측을 기반으로 일부 속성을 설정합니다. TextBlock 주위에 파란색 강조 표시가 나타나 현재 TextBlock이 활성 객체임을 나타냅니다. 디자이너가 추가한 여백 및 기타 설정을 확인합니다. XAML은 다음과 같이 보입니다. 이 형식이 정확하게 지정되지 않아도 걱정할 필요가 없습니다. 읽기 쉽도록 이 부분을 축약했습니다.

    ```xaml
    <TextBlock x:Name="textBlock"
               HorizontalAlignment="Left"
               Margin="351,44,0,0"
               TextWrapping="Wrap"
               Text="TextBlock"
               VerticalAlignment="Top"/>
    ```

    다음 단계에서 이 값을 업데이트합니다.

4. 속성 패널에서 TextBlock의 이름 값을 **textBlock**에서 **TitleTextBlock**으로 변경합니다. (TextBlock이 여전히 활성 객체인지 확인합니다.)

5. **공용** 아래에서 텍스트 값을 **컬렉션**으로 변경합니다.

    ![TextBlock 속성](images/xaml-basics/text-block-properties.png)

    XAML 편집기에서 XAML은 이제 다음과 같이 표시됩니다.

    ```xaml
    <TextBlock x:Name="TitleTextBlock"
               HorizontalAlignment="Left"
               Margin="351,44,0,0"
               TextWrapping="Wrap"
               Text="Collection"
               VerticalAlignment="Top"/>
    ```

6. TextBlock의 위치를 지정하려면 먼저 Visual Studio에서 추가한 속성 값을 제거해야 합니다. 문서 개요에서 **TitleTextBlock**을 마우스 오른쪽 단추로 클릭한 다음 **레이아웃 > 모두 다시 설정**을 선택합니다.

![문서 개요](images/xaml-basics/doc-outline-reset.png)

7. 속성 패널에서 검색 상자에 **margin**을 입력하여 **Margin** 속성을 쉽게 찾을 수 있게 합니다. 왼쪽 여백과 아래쪽 여백을 24로 설정합니다.

    ![TextBlock 여백](images/xaml-basics/margins.png)

    여백은 페이지에서 요소의 가장 기본적인 위치 지정을 제공합니다. 레이아웃을 미세하게 조정하는 데 유용하지만, Visual Studio에서 추가한 것과 같은 큰 여백 값을 사용하면 UI가 다양한 화면 크기에 적응하기 어렵기 때문에 피해야 합니다.

    자세한 내용은 [맞춤, 여백 및 안쪽 여백](../layout/alignment-margin-padding.md)을 참조하세요.

8. 문서 개요 패널에서 **TitleTextBlock**를 마우스 오른쪽 단추로 클릭한 후 **Edit Style > Apply Resource > TitleTextBlockStyle**를 선택하세요. 이렇게 하면 제목 텍스트에 시스템 정의 스타일이 적용됩니다.

    ```xaml
    <TextBlock x:Name="TitleTextBlock"
               TextWrapping="Wrap"
               Text="Collection"
               Margin="24,0,0,24"
               Style="{StaticResource TitleTextBlockStyle}"/>
    ```

9. 속성 패널에서 검색 상자에 **textwrapping**을 입력하여 **TextWrapping** 속성을 쉽게 찾을 수 있게 합니다. **TextWrapping** 속성에 대한 _속성 마커_를 클릭하여 메뉴를 엽니다. _속성 마커_는 각 속성 값의 오른쪽에 있는 작은 상자 모양 기호입니다. _속성 마커_는 속성이 기본값이 아닌 값으로 설정되었음을 나타내는 검은색입니다.) **속성** 메뉴에서 **다시 설정**을 선택하여 TextWrapping 설정을 다시 설정합니다.

    Visual Studio는 이 속성을 추가하지만 적용한 스타일로 이미 설정되어 있으므로 여기서는 필요하지 않습니다.

UI의 첫 번째 부분을 앱에 추가했습니다! 이제 앱을 실행하여 현재의 모습을 확인합니다.

XAML 디자이너에서 검은색 바탕에 흰색 텍스트가 표시되었지만, 실행했을 때 검은색 텍스트가 흰색 배경에 나타났다는 점을 인식했을 수도 있습니다. Windows에는 어두운 테마와 밝은 테마가 모두 있고 기본 테마는 디바이스마다 다르기 때문입니다. PC에서 기본 테마는 '밝게'입니다. XAML 디자이너의 위쪽에 있는 톱니 바퀴 아이콘을 클릭하여 디바이스 미리 보기 설정을 열고 테마를 '밝게'로 변경하면 XAML 디자이너의 앱이 PC에서와 같은 모습이 됩니다.

> [!NOTE]
> 이 자습서의 이번 파트에서는 드래그 앤 드롭으로 컨트롤을 추가했습니다. 도구 상자에서 에서 컨트롤을 두 번 클릭하여 컨트롤을 추가할 수도 있습니다. 시도해 보고 Visual Studio에서 생성되는 XAML의 차이점을 확인해 보세요.

## <a name="part-2-add-a-gridview-control-using-the-xaml-editor"></a>파트 2: XAML 편집기를 사용하여 GridView 컨트롤 추가

파트 1에서는 Visual Studio에서 제공하는 XAML 디자이너 및 기타 도구를 사용했습니다. 여기에서는 XAML 편집기를 사용하여 XAML 태그를 직접 사용해 봅니다. XAML에 익숙해지면 이 방법이 더 효율적으로 작동할 수 있습니다.

먼저, 루트 레이아웃 [Grid](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.grid)를 [**RelativePanel**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.relativepanel)로 바꿉니다. RelativePanel을 사용하면 패널이나 다른 UI와 비교하여 UI 청크를 쉽게 재배열할 수 있습니다. [XAML 적응형 레이아웃](xaml-basics-adaptive-layout.md) 자습서에서 이러한 유용함을 확인할 수 있습니다. 

그 다음 [GridView](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.gridview) 컨트롤을 추가하여 데이터를 표시합니다.

**XAML 편집기를 사용하여 컨트롤 추가**

1. XAML 편집기에서 루트 **Grid**를 **RelativePanel**로 변경합니다.

    **이전**
    ```xaml
    <Grid Background="{ThemeResource ApplicationPageBackgroundThemeBrush}">
          <TextBlock x:Name="TitleTextBlock"
                     Text="Collection"
                     Margin="24,0,0,24"
                     Style="{StaticResource TitleTextBlockStyle}"/>
    </Grid>
    ```

    **이후**
    ```xaml
    <RelativePanel Background="{ThemeResource ApplicationPageBackgroundThemeBrush}">
        <TextBlock x:Name="TitleTextBlock"
                   Text="Collection"
                   Margin="24,0,0,24"
                   Style="{StaticResource TitleTextBlockStyle}"/>
    </RelativePanel>
    ```

    **RelativePanel**을 사용하는 레이아웃에 대한 자세한 내용은 [레이아웃 패널](https://docs.microsoft.com/windows/uwp/layout/layout-panels#relativepanel)을 참조하세요.

2. **TextBlock** 요소 아래에서 'ImageGridView'라는 **GridView 컨트롤**을 추가합니다. **RelativePanel** _연결된 속성_을 설정하여 컨트롤을 제목 텍스트 아래에 배치하고 화면의 전체 너비에 걸쳐 늘립니다.

    **이 XAML 추가**

    ```xaml
    <GridView x:Name="ImageGridView"
              Margin="0,0,0,8"
              RelativePanel.AlignLeftWithPanel="True"
              RelativePanel.AlignRightWithPanel="True"
              RelativePanel.Below="TitleTextBlock"/>
    ```

    **TextBlock 이후**
    ```xaml
    <RelativePanel Background="{ThemeResource ApplicationPageBackgroundThemeBrush}">
        <TextBlock x:Name="TitleTextBlock"
                   Text="Collection"
                   Margin="24,0,0,24"
                   Style="{StaticResource TitleTextBlockStyle}"/>
        
        <!-- Add the GridView here. -->

    </RelativePanel>
    ```

    패널 연결된 속성에 대한 자세한 내용은 [레이아웃 패널](https://docs.microsoft.com/windows/uwp/layout/layout-panels)을 참조하세요.

3. **GridView**에 무엇이든 표시하려면 표시할 데이터 모음을 제공해야 합니다. MainPage.xaml.cs를 열고 **GetItemsAsync** 메서드를 찾습니다. 이 메서드는 Images라는 컬렉션을 채우는데, 이 컬렉션은 MainPage에 추가한 속성입니다.

    **GetItemsAsync**의 **foreach** 루프 이후에 이 코드 행을 추가합니다.

    ```csharp
    ImageGridView.ItemsSource = Images;
    ```

    이는 GridView의 **ItemsSource** 속성을 앱의 **Images** 컬렉션으로 설정하고 **GridView**에 표시할 항목을 제공합니다.

앱을 실행하고 모든 것이 제대로 작동하는지 확인할 수 있는 좋은 장소입니다. 이 도구는 다음과 같습니다.

![앱 UI 검사점 1](images/xaml-basics/layout-0.png)

앱에서 아직 이미지가 표시되지 않는 것을 알 수 있습니다. 기본적으로 이는 컬렉션에 있는 데이터 형식의 ToString 값을 표시합니다. 그런 다음 데이터 템플릿을 만들어 데이터 표시 방법을 정의합니다.

> [!NOTE]
> **RelativePanel**을 사용하는 레이아웃은 [레이아웃 패널](https://docs.microsoft.com/windows/uwp/layout/layout-panels#relativepanel) 문서에서 자세히 알아볼 수 있습니다. **TextBlock** 및 **GridView**에 대해 RelativePanel 연결된 속성을 설정하여 다른 레이아웃을 실험해 봅니다.

## <a name="part-3-add-a-datatemplate-to-display-your-data"></a>파트 3: 데이터를 표시하는 DataTemplate 추가

이제 데이터를 표시하는 방법을 GridView에 알려 주는 [DataTemplate](https://docs.microsoft.com/uwp/api/windows.ui.xaml.datatemplate)을 만듭니다. 데이터 템플릿의 전체 설명은 [항목 컨테이너 및 템플릿](../controls-and-patterns/item-containers-templates.md)을 참조하세요.

지금은 자리 표시자를 추가하여 원하는 레이아웃을 쉽게 만들 수 있습니다. [XAML 데이터 바인딩](../../data-binding/xaml-basics-data-binding.md) 자습서에서 이러한 자리 표시자를 **ImageFileInfo** 클래스의 실제 데이터로 바꿉니다. 데이터 개체의 모습을 보려면 ImageFileInfo.cs 파일을 열 수 있습니다.

**그리드 보기에 데이터 템플릿 추가**

1. MainPage.xaml을 엽니다.

2. 평점을 보려면 [UWP용 Telerik UI](https://github.com/telerik/UI-For-UWP) NuGet 패키지의 **RadRating** 컨트롤을 사용합니다. Telerik 컨트롤의 네임스페이스를 지정하는 XAML 네임스페이스 참조를 추가합니다. 이를 다른 'xmlns:' 항목 바로 뒤에 여는 **Page**태그에 배치합니다.

    **이 XAML 추가**

    ```xaml
    xmlns:telerikInput="using:Telerik.UI.Xaml.Controls.Input"
    ```

    **마지막 'xmlns:' 항목 이후**

    ```xaml
    <Page x:Name="page"
      x:Class="PhotoLab.MainPage"
      xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
      xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
      xmlns:local="using:PhotoLab"
      xmlns:d="http://schemas.microsoft.com/expression/blend/2008"
      xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006"
      xmlns:telerikInput="using:Telerik.UI.Xaml.Controls.Input"
      mc:Ignorable="d"
      NavigationCacheMode="Enabled">
    ```

    XAML 네임스페이스에 대한 자세한 내용은 [XAML 네임스페이스 및 네임스페이스 매핑](https://docs.microsoft.com/windows/uwp/xaml-platform/xaml-namespaces-and-namespace-mapping)을 참조하세요.

3. 문서 개요에서 **ImageGridView**를 마우스 오른쪽 단추로 클릭합니다. 컨텍스트 메뉴에서 **추가 템플릿 편집 > 생성된 항목 편집(ItemTemplate) > 빈 역할 만들기**를 선택합니다. **리소스 만들기** 대화 상자가 열립니다.

4. 대화 상자에서 이름(키) 값을 **ImageGridView_DefaultItemTemplate**으로 변경하고 **확인**을 클릭합니다.

    **확인**을 클릭하면 몇 가지 상황이 발생합니다.

    - **DataTemplate**이 MainPage.xaml의 Page.Resources 섹션에 추가됩니다.

        ```xaml
        <Page.Resources>
            <DataTemplate x:Key="ImageGridView_DefaultItemTemplate">
                <Grid/>
            </DataTemplate>
        </Page.Resources>
        ```

    - 문서 개요 범위는 이 **DataTemplate**으로 설정됩니다.

        데이터 템플릿을 만들었으면 문서 개요 왼쪽 상단의 위쪽 화살표를 클릭하여 페이지 범위로 돌아갈 수 있습니다.

    - GridView의 **ItemTemplate** 속성은 **DataTemplate** 리소스로 설정됩니다.

    ```xaml
        <GridView x:Name="ImageGridView"
                  Margin="0,0,0,8"
                  RelativePanel.AlignLeftWithPanel="True"
                  RelativePanel.AlignRightWithPanel="True"
                  RelativePanel.Below="TitleTextBlock"
                  ItemTemplate="{StaticResource ImageGridView_DefaultItemTemplate}"/>
    ```

5. **ImageGridView_DefaultItemTemplate** 리소스에서 루트 **Grid**의 Height와 Width를 **300**으로, Margin을 **8**로 설정합니다. 그런 다음 두 개의 행을 추가하고 두 번째 행의 Height를 **Auto**로 설정합니다.

    **이전**
    ```xaml
    <Grid/>
    ```

    **이후**
    ```xaml
    <Grid Height="300"
          Width="300"
          Margin="8">
        <Grid.RowDefinitions>
            <RowDefinition />
            <RowDefinition Height="Auto" />
        </Grid.RowDefinitions>
    </Grid>
    ```

    그리드 레이아웃에 대한 자세한 내용은 [레이아웃 패널](https://docs.microsoft.com/windows/uwp/layout/layout-panels#grid)을 참조하세요.

6. 그리드에 컨트롤을 추가합니다.

    a. 첫 번째 그리드 행에 **Image** 컨트롤을 추가합니다. 여기에 이미지가 표시되지만, 지금은 앱의 스토어 로고를 자리 표시자로 사용합니다.

    b. **TextBlock** 컨트롤을 추가하여 이미지의 이름, 파일 유형 및 크기를 표시합니다. 이를 위해 **StackPanel** 컨트롤을 사용하여 텍스트 블록을 정렬합니다.

    **StackPanel** 레이아웃에 대한 자세한 내용은 [레이아웃 패널](https://docs.microsoft.com/windows/uwp/layout/layout-panels#stackpanel)을 참조하세요.

    c. **RadRating** 컨트롤을 외부(수직) **StackPanel**에 추가합니다. 이를 내부(수평) **StackPanel** 다음에 배치합니다.

    **마지막 템플릿**

    ```xaml
    <Grid Height="300"
          Width="300"
          Margin="8">
        <Grid.RowDefinitions>
            <RowDefinition />
            <RowDefinition Height="Auto" />
        </Grid.RowDefinitions>

        <Image x:Name="ItemImage"
               Source="Assets/StoreLogo.png"
               Stretch="Uniform" />

        <StackPanel Orientation="Vertical"
                    Grid.Row="1">
            <TextBlock Text="ImageTitle"
                       HorizontalAlignment="Center"
                       Style="{StaticResource SubtitleTextBlockStyle}" />
            <StackPanel Orientation="Horizontal"
                        HorizontalAlignment="Center">
                <TextBlock Text="ImageFileType"
                           HorizontalAlignment="Center"
                           Style="{StaticResource CaptionTextBlockStyle}" />
                <TextBlock Text="ImageDimensions"
                           HorizontalAlignment="Center"
                           Style="{StaticResource CaptionTextBlockStyle}"
                           Margin="8,0,0,0" />
            </StackPanel>

            <telerikInput:RadRating Value="3"
                                    IsReadOnly="True">
                <telerikInput:RadRating.FilledIconContentTemplate>
                    <DataTemplate>
                        <SymbolIcon Symbol="SolidStar"
                                    Foreground="White" />
                    </DataTemplate>
                </telerikInput:RadRating.FilledIconContentTemplate>
                <telerikInput:RadRating.EmptyIconContentTemplate>
                    <DataTemplate>
                        <SymbolIcon Symbol="OutlineStar"
                                    Foreground="White" />
                    </DataTemplate>
                </telerikInput:RadRating.EmptyIconContentTemplate>
            </telerikInput:RadRating>

        </StackPanel>
    </Grid>
    ```

이제 앱을 실행해서 방금 만든 항목 템플릿이 있는 **GridView**를 확인합니다. 하지만 흰색 배경에 흰색 별이 있기 때문에 평점 컨트롤이 표시되지 않을 수 있습니다. 다음에는 배경색을 바꿔 봅니다.

![앱 UI 검사점 2](images/xaml-basics/layout-1.png)

## <a name="part-4-modify-the-item-container-style"></a>파트 4: 항목 컨테이너 스타일 수정

항목의 컨트롤 템플릿에는 선택 항목, 포인터 가리키기 및 포커스와 같은 상태를 표시하는 시각적 개체가 포함됩니다. 이러한 시각적 개체는 데이터 템플릿의 위 또는 아래에 렌더링됩니다. 여기에서는 컨트롤 템플릿의 **Background** 및 **Margin** 속성을 수정하여 **GridView** 항목에 회색 배경을 부여합니다.

**항목 컨테이너 수정**

1. 문서 개요에서 **ImageGridView**를 마우스 오른쪽 단추로 클릭합니다. 컨텍스트 메뉴에서 **추가 템플릿 편집 > 생성된 항목 컨테이너 편집(ItemContainerStyle) > 복사본 편집**을 선택합니다. **리소스 만들기** 대화 상자가 열립니다.

2. 대화 상자에서 이름(키) 값을 **ImageGridView_DefaultItemContainerStyle**로 변경하고 **확인**을 클릭합니다.

    기본 스타일의 복사본이 XAML의 **Page.Resources** 섹션에 추가됩니다.

    ```xaml
    <Style x:Key="ImageGridView_DefaultItemContainerStyle" TargetType="GridViewItem">
        <Setter Property="FontFamily" Value="{ThemeResource ContentControlThemeFontFamily}"/>
        <Setter Property="FontSize" Value="{ThemeResource ControlContentThemeFontSize}"/>
        <Setter Property="Background" Value="{ThemeResource GridViewItemBackground}"/>
        <Setter Property="Foreground" Value="{ThemeResource GridViewItemForeground}"/>
        <Setter Property="TabNavigation" Value="Local"/>
        <Setter Property="IsHoldingEnabled" Value="True"/>
        <Setter Property="HorizontalContentAlignment" Value="Center"/>
        <Setter Property="VerticalContentAlignment" Value="Center"/>
        <Setter Property="Margin" Value="0,0,4,4"/>
        <Setter Property="MinWidth" Value="{ThemeResource GridViewItemMinWidth}"/>
        <Setter Property="MinHeight" Value="{ThemeResource GridViewItemMinHeight}"/>
        <Setter Property="AllowDrop" Value="False"/>
        <Setter Property="UseSystemFocusVisuals" Value="True"/>
        <Setter Property="FocusVisualMargin" Value="-2"/>
        <Setter Property="FocusVisualPrimaryBrush" Value="{ThemeResource GridViewItemFocusVisualPrimaryBrush}"/>
        <Setter Property="FocusVisualPrimaryThickness" Value="2"/>
        <Setter Property="FocusVisualSecondaryBrush" Value="{ThemeResource GridViewItemFocusVisualSecondaryBrush}"/>
        <Setter Property="FocusVisualSecondaryThickness" Value="1"/>
        <Setter Property="Template">
            <Setter.Value>
                <ControlTemplate TargetType="GridViewItem">
                <!-- XAML removed for clarity
                    <ListViewItemPresenter ... />
                -->   
                </ControlTemplate>
            </Setter.Value>
        </Setter>
    </Style>
    ```

    **GridViewItem** 기본 스타일은 많은 속성을 설정합니다. 항상 기본 스타일의 복사본을 사용하여 시작한 다음 필요한 속성만 수정해야 합니다. 그러지 않으면 일부 속성이 올바르게 설정되지 않으므로 시각적 개체가 예상대로 표시되지 않을 수 있습니다.

    이전 단계와 같이 GridView의 **ItemContainerStyle** 속성을 **Style** 리소스로 설정합니다.

    ```xaml
        <GridView x:Name="ImageGridView"
                  Margin="0,0,0,8"
                  RelativePanel.AlignLeftWithPanel="True"
                  RelativePanel.AlignRightWithPanel="True"
                  RelativePanel.Below="TitleTextBlock"
                  ItemTemplate="{StaticResource ImageGridView_DefaultItemTemplate}"
                  ItemContainerStyle="{StaticResource ImageGridView_DefaultItemContainerStyle}"/>
    ```

3. **Background** 속성의 값을 **Gray**로 변경합니다.

    **이전**
    ```xaml
        <Setter Property="Background" Value="{ThemeResource GridViewItemBackground}"/>
    ```

    **이후**
    ```xaml
        <Setter Property="Background" Value="Gray"/>
    ```

4. **Margin** 속성의 값을 **8**로 변경합니다.

    **이전**
    ```xaml
        <Setter Property="Margin" Value="0,0,4,4"/>
    ```

    **이후**
    ```xaml
        <Setter Property="Margin" Value="8"/>
    ```

앱을 실행하고 현재의 모습을 확인합니다. 앱 창 크기를 조정합니다. **GridView**는 이미지를 재정렬해 주지만, 일부 너비의 경우 앱 창 오른쪽에 공간이 많이 있습니다. 이미지가 중심에 있다면 더 보기 좋을 것입니다. 다음에 살펴보겠습니다.

![앱 UI 검사점 3](images/xaml-basics/layout-2.png)

> [!Note]
> 실험해 보고 싶다면 배경 및 여백 속성을 다른 값으로 설정하고 효과를 확인합니다.

## <a name="part-5-apply-some-final-adjustments-to-the-layout"></a>파트 5: 레이아웃에 최종 조정 사항 적용

페이지에서 이미지의 중심을 맞추려면 페이지에서 그리드의 정렬을 조정해야 합니다. 아니면 **GridView**에서 이미지 정렬을 조정해야 할까요? 효과가 있을까요? 확인해 보겠습니다.

맞춤에 대한 자세한 내용은 [맞춤, 여백 및 안쪽 여백](../layout/alignment-margin-padding.md)을 참조하세요.

(**GridView**의 **Background**를 이 단계에서 원하는 색상으로 설정해 보세요. 레이아웃을 통해 어떤 일이 일어나는지 더 분명하게 확인할 수 있습니다.)

**이미지 정렬 수정**

1. **Gridview**에서 **HorizontalAlignment** 속성을 **Center**로 설정합니다.

    **이전**
    ```xaml
        <GridView x:Name="ImageGridView"
                  Margin="0,0,0,8"
                  RelativePanel.AlignLeftWithPanel="True"
                  RelativePanel.AlignRightWithPanel="True"
                  RelativePanel.Below="TitleTextBlock"
                  ItemTemplate="{StaticResource ImageGridView_DefaultItemTemplate}"
                  ItemContainerStyle="{StaticResource ImageGridView_DefaultItemContainerStyle}"/>
    ```

    **이후**
    ```xaml
        <GridView x:Name="ImageGridView"
                  Margin="0,0,0,8"
                  RelativePanel.AlignLeftWithPanel="True"
                  RelativePanel.AlignRightWithPanel="True"
                  RelativePanel.Below="TitleTextBlock"
                  ItemTemplate="{StaticResource ImageGridView_DefaultItemTemplate}"
                  ItemContainerStyle="{StaticResource ImageGridView_DefaultItemContainerStyle}" 
                  HorizontalAlignment="Center"/>
    ```

2. 앱을 실행하고 창의 크기를 조정합니다. 아래로 스크롤해서 더 많은 이미지를 봅니다.

    이미지가 중앙에 위치해서 보기가 더 좋습니다. 스크롤 막대는 창 가장자리 대신 **GridView** 가장자리에 맞춰집니다. 이 문제를 해결하려면 **GridView**를 페이지 중앙에 배치하는 대신 **GridView**에 이미지의 중심을 맞춥니다. 약간의 작업이 더 필요하지만 최종적인 모습은 더 보기 좋게 됩니다.

3. 이전 단계의 **HorizontalAlignment** 설정을 제거합니다.

4. 문서 개요에서 **ImageGridView**를 마우스 오른쪽 단추로 클릭합니다. 컨텍스트 메뉴에서 **추가 템플릿 편집 > 항목 레이아웃 편집(ItemsPanel) > 복사본 편집**을 선택합니다. **리소스 만들기** 대화 상자가 열립니다.

5. 대화 상자에서 이름(키) 값을 **ImageGridView_ItemsPanelTemplate**으로 변경하고 **확인**을 클릭합니다.

    기본 **ItemsPanelTemplate**이 XAML의 **Page.Resources** 섹션에 추가됩니다. (이전과 마찬가지로 **GridView**는 이 리소스를 참조하도록 업데이트되었습니다.)

    ```xaml
    <ItemsPanelTemplate x:Key="ImageGridView_ItemsPanelTemplate">
        <ItemsWrapGrid Orientation="Horizontal" />
    </ItemsPanelTemplate>
    ```

    다양한 패널을 사용하여 앱의 컨트롤을 레이아웃 처리하는 것처럼 **GridView**에는 항목의 레이아웃을 관리하는 내부 패널이 있습니다. 이 패널(**ItemsWrapGrid**)에 액세스했으므로 속성을 수정하여 **GridView** 내부의 항목 레이아웃을 변경할 수 있습니다.

6. **ItemsWrapGrid**에서 **HorizontalAlignment** 속성을 **Center**로 설정합니다.

    **이전**
    ```xaml
    <ItemsPanelTemplate x:Key="ImageGridView_ItemsPanelTemplate">
        <ItemsWrapGrid Orientation="Horizontal" />
    </ItemsPanelTemplate>
    ```

    **이후**
    ```xaml
    <ItemsPanelTemplate x:Key="ImageGridView_ItemsPanelTemplate">
        <ItemsWrapGrid Orientation="Horizontal"
                       HorizontalAlignment="Center"/>
    </ItemsPanelTemplate>
    ```

7. 앱을 실행하고 창의 크기를 다시 조정합니다. 아래로 스크롤해서 더 많은 이미지를 봅니다.

![앱 UI 검사점 4](images/xaml-basics/layout-3.png)

이제 스크롤 막대가 창의 가장자리와 정렬됩니다. 축하합니다. 앱의 기본 UI를 만들었습니다.

## <a name="going-further"></a>더 나아가기

기본 UI를 만들었습니다. 이제 역시 PhotoLab 샘플에 기반을 둔 다음의 다른 자습서들을 확인해 보세요. 

* [XAML 데이터 바인딩 자습서](../../data-binding/xaml-basics-data-binding.md)에 실제 이미지와 데이터를 추가합니다.
* [XAML 적응 레이아웃 자습서](xaml-basics-adaptive-layout.md)에서 여러 화면 크기에 맞춰지는 적응형 UI를 만듭니다.


## <a name="get-the-final-version-of-the-photolab-sample"></a>PhotoLab 샘플의 최종 버전 다운로드

이 자습서에서는 전체적인 사진 편집 앱을 빌드하지 않으므로 [최종 버전](https://github.com/Microsoft/Windows-appsample-photo-lab)을 확인하여 사용자 지정 애니메이션 및 휴대폰 지원과 같은 다른 기능을 살펴보세요.

