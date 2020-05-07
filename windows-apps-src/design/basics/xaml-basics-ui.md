---
title: 사용자 인터페이스 만들기 자습서
description: 이 문서에서는 XAML의 사용자 인터페이스 빌드 기본 사항에 대해 설명합니다.
keywords: XAML, UWP, 시작
ms.date: 08/30/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: c23a9539d0fc3902f715917b380e8b6b3e132c15
ms.sourcegitcommit: 76e8b4fb3f76cc162aab80982a441bfc18507fb4
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/29/2020
ms.locfileid: "68974456"
---
# <a name="tutorial-create-a-user-interface"></a>자습서: 사용자 인터페이스 만들기

이 자습서에서는 이미지 편집 프로그램의 기본 UI를 만드는 방법을 알아봅니다. 

+ Visual Studio에서 XAML 도구(예: XAML 디자이너, 도구 상자, XAML 편집기, **속성** 패널 및 문서 개요)를 사용하여 컨트롤과 콘텐츠를 UI에 추가합니다.
+ 가장 일반적인 XAML 레이아웃 패널(예: **RelativePanel**, **Grid** 및 **StackPanel**)을 사용합니다.

이미지 편집 프로그램에는 다음과 같은 두 페이지가 있습니다. *기본 페이지*는 각 이미지 파일에 대한 일부 정보와 함께 사진 갤러리 보기를 표시합니다.

![기본 페이지](images/xaml-basics/mainpage.png)

*세부 정보 페이지*는 선택한 단일 사진을 표시합니다. 플라이아웃 편집 메뉴를 사용하면 사진을 수정하고, 이름을 변경하고, 저장할 수 있습니다.

![세부 정보 페이지](images/xaml-basics/detailpage.png)


## <a name="prerequisites"></a>필수 구성 요소

* Visual Studio 2019: [Visual Studio 2019 Community(무료) 다운로드](https://www.visualstudio.com/thank-you-downloading-visual-studio/?sku=Community&rel=15&campaign=WinDevCenter&ocid=wdgcx-windevcenter-community-download) 
* Windows 10 SDK(10.0.15063.468 이상):  [최신 Windows SDK(무료)](https://developer.microsoft.com/windows/downloads/windows-10-sdk)

## <a name="part-0-get-the-starter-code-from-github"></a>0부: GitHub에서 시작 코드 가져오기

이 자습서에서는 PhotoLab 샘플의 간소화된 버전부터 시작합니다. 

1. [샘플의 GitHub 페이지](https://github.com/Microsoft/Windows-appsample-photo-lab)로 이동합니다. 
2. 다음으로, 샘플을 복제 또는 다운로드해야 합니다. **복제 또는 다운로드** 단추를 선택합니다. 하위 메뉴가 표시됩니다.
    ![PhotoLab 샘플의 GitHub 페이지에 있는 복제 또는 다운로드 메뉴](images/xaml-basics/clone-repo.png)
    
    **GitHub에 익숙하지 않은 경우:**
    
    a. **ZIP 다운로드**를 선택하고, 파일을 로컬로 선택합니다. 이 단계에서는 필요한 프로젝트 파일이 모두 포함된 .zip 파일을 다운로드합니다.

    b. 파일의 압축을 풉니다. 파일 탐색기를 사용하여 방금 다운로드한 .zip 파일을 찾아 마우스 오른쪽 단추로 클릭하고, **압축 풀기**를 선택합니다.

    c. 샘플의 로컬 복사본을 찾고, `Windows-appsample-photo-lab-master\xaml-basics-starting-points\user-interface` 디렉터리로 이동합니다.    

    **GitHub에 익숙한 경우:**

    a. 리포지토리의 마스터 분기를 로컬에 복제합니다.

    b. `Windows-appsample-photo-lab\xaml-basics-starting-points\user-interface` 디렉터리로 이동합니다.

3. **Photolab.sln**을 선택하여 프로젝트를 엽니다.

## <a name="part-1-add-a-textblock-control-by-using-xaml-designer"></a>1부: XAML 디자이너를 사용하여 TextBlock 컨트롤 추가

Visual Studio에서는 XAML UI를 쉽게 만들 수 있는 여러 가지 도구를 제공합니다. XAML 디자이너를 사용하여 컨트롤을 디자인 화면으로 끌고, 앱을 실행하기 전에 해당 컨트롤의 모양을 확인합니다. **속성** 패널을 사용하면 디자이너에서 활성화된 컨트롤의 모든 속성을 보고 설정할 수 있습니다. 문서 개요는 UI에 대한 XAML 시각적 트리의 부모/자식 구조를 보여 줍니다. XAML 편집기를 사용하여 XAML 태그를 직접 입력하고 수정합니다.

다음은 Visual Studio UI의 레이블이 있는 도구입니다.

![Visual Studio 레이아웃](images/xaml-basics/visual-studio-tools.png)

이러한 각 도구를 사용하면 UI를 쉽게 만들 수 있으므로 이 자습서에서는 이러한 도구를 모두 사용합니다. 먼저 XAML 디자이너를 사용하여 컨트롤을 추가합니다. 

XAML 디자이너를 사용하여 컨트롤을 추가하려면 다음을 수행합니다.

1. 솔루션 탐색기에서 **MainPage.xaml**을 두 번 클릭하여 엽니다. 이 단계에서는 UI 요소가 추가되지 않은 앱의 기본 페이지를 보여 줍니다.

2. 계속 진행하기 전에 Visual Studio를 다음과 같이 약간 조정해야 합니다.

    - 솔루션 플랫폼이 ARM이 아니라 x86 또는 x64로 설정되어 있는지 확인합니다.
    - 13.3인치 Desktop 미리 보기를 표시하도록 XAML 디자이너의 기본 페이지를 설정합니다.

    여기에 표시된 것처럼 창 위쪽에 두 설정이 모두 표시되어야 합니다.

    ![Visual Studio 설정](images/xaml-basics/layout-vs-settings.png)

    지금 앱을 실행할 수는 있지만, 볼 수 있는 것이 별로 없습니다. 좀 더 재미있는 UI 요소를 추가해 보겠습니다.

3. 도구 상자에서 **공용 XAML 컨트롤**을 확장하고 [TextBlock](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.textblock) 컨트롤을 찾습니다. **TextBlock** 컨트롤을 페이지의 왼쪽 위 모서리 근처에 있는 디자인 화면으로 끕니다.

    **TextBlock** 컨트롤이 페이지에 추가되고, 디자이너에서는 원하는 레이아웃에서 가장 적절한 추측에 따라 일부 속성을 설정합니다. **TextBlock** 컨트롤 주위에 파란색으로 강조 표시되어 현재 활성 개체임을 나타냅니다. 디자이너에서 추가한 여백 및 기타 설정을 확인합니다. 
    
    XAML은 다음과 같습니다. 형식이 이처럼 정확히 지정되지 않았더라도 걱정하지 마세요. 여기서는 쉽게 읽을 수 있도록 간략하게 줄였습니다.

    ```xaml
    <TextBlock x:Name="textBlock"
               HorizontalAlignment="Left"
               Margin="351,44,0,0"
               TextWrapping="Wrap"
               Text="TextBlock"
               VerticalAlignment="Top"/>
    ```

    다음 단계에서 이 값을 업데이트하겠습니다.

4. **속성** 패널에서 **TextBlock** 컨트롤의 **Name** 값을 **textBlock**에서 **TitleTextBlock**으로 변경합니다. (**TextBlock** 컨트롤이 여전히 활성 개체인지 확인하세요.)

5. **공용** 아래에서 **Text** 값을 **Collection**으로 변경합니다.

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

6. **TextBlock** 컨트롤의 위치를 지정하려면 먼저 Visual Studio에서 추가한 속성 값을 제거해야 합니다. [문서 개요]에서 마우스 오른쪽 단추로 **TitleTextBlock**을 클릭한 다음, **레이아웃** > **모두 다시 설정**을 차례로 선택합니다.

    ![문서 개요](images/xaml-basics/doc-outline-reset.png)

7. **속성** 패널의 검색 상자에서 **margin**을 입력하여 **Margin** 속성을 쉽게 찾습니다. 왼쪽 여백과 아래쪽 여백을 24로 설정합니다.

    ![TextBlock 여백](images/xaml-basics/margins.png)

    여백은 페이지에서 요소의 가장 기본적인 위치 지정을 제공합니다. 이는 레이아웃을 미세 조정하는 데 유용하지만, Visual Studio에서 추가한 것과 같은 큰 여백 값은 사용하지 않아야 합니다. 이렇게 하면 UI가 다양한 화면 크기에 맞게 조정하기가 어려워집니다.

    자세한 내용은 [맞춤, 여백 및 안쪽 여백](../layout/alignment-margin-padding.md)을 참조하세요.

8. [문서 개요]에서 마우스 오른쪽 단추로 **TitleTextBlock**을 클릭한 다음, **스타일 편집** > **리소스 적용** > **TitleTextBlockStyle**을 차례로 선택합니다. 이 단계에서는 시스템 정의 스타일이 제목 텍스트에 적용됩니다.

    ```xaml
    <TextBlock x:Name="TitleTextBlock"
               TextWrapping="Wrap"
               Text="Collection"
               Margin="24,0,0,24"
               Style="{StaticResource TitleTextBlockStyle}"/>
    ```

9. **속성** 패널의 검색 상자에서 **textwrapping**을 입력하여 **TextWrapping** 속성을 찾습니다. **TextWrapping** 속성에 대한 _속성 마커_를 선택하여 해당 메뉴를 엽니다. (속성 마커는 각 속성 값의 오른쪽에 있는 작은 상자 모양 기호입니다. 속성 마커가 검은색이면 해당 속성이 기본값이 아닌 값으로 설정되었음을 나타냅니다.) **속성** 메뉴에서 **다시 설정**을 선택하여 **TextWrapping** 속성을 다시 설정합니다.

    Visual Studio가 이 속성을 추가하지만, 여러분이 적용한 스타일로 이미 설정되어 있으므로 여기서는 필요 없습니다.

UI의 첫 번째 부분을 앱에 추가했습니다. 이제 앱을 실행하여 어떤 모양인지 확인합니다.

XAML 디자이너에서 앱이 검은색 배경의 흰색 텍스트를 표시했음을 알 수 있습니다. 앱을 실행하면 흰색 배경의 검은색 텍스트가 표시되었습니다. Windows에는 어두운 테마와 밝은 테마가 모두 있고 기본 테마는 디바이스마다 다르기 때문입니다. PC에서 기본 테마는 [밝게]입니다. XAML 디자이너에서 앱이 PC와 동일하게 보이도록 하려면, XAML 디자이너 위쪽에서 기어 아이콘을 선택하여 [디바이스 미리 보기 설정]을 열고 테마를 [밝게]로 변경합니다.

> [!NOTE]
> 자습서의 이 부분에서는 컨트롤을 끌어서 추가했습니다. 도구 상자에서 컨트롤을 두 번 클릭하여 컨트롤을 추가할 수도 있습니다. 시도해 보고 Visual Studio에서 생성하는 XAML의 차이점을 확인해 보세요.

## <a name="part-2-add-a-gridview-control-by-using-the-xaml-editor"></a>2부: XAML 편집기를 사용하여 GridView 컨트롤 추가

1부에서는 XAML 디자이너 및 Visual Studio에서 제공하는 다른 도구를 사용했습니다. 여기서는 XAML 편집기를 사용하여 XAML 태그를 직접 작업하겠습니다. XAML에 익숙해지면 이 방법이 더 효율적으로 작동할 수 있습니다.

먼저 루트 [Grid](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.grid) 레이아웃을 [RelativePanel](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.relativepanel)로 바꿉니다. **RelativePanel**을 사용하면 패널 또는 다른 UI 요소를 기준으로 UI 청크를 쉽게 다시 정렬할 수 있습니다. 이에 대한 유용성은 [XAML 적응형 레이아웃](xaml-basics-adaptive-layout.md) 자습서에서 확인할 수 있습니다. 

다음으로, [GridView](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.gridview) 컨트롤을 추가하여 데이터를 표시합니다.

XAML 편집기를 사용하여 컨트롤을 추가하려면 다음을 수행합니다.

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

2. **ImageGridView**라는 **GridView** 컨트롤을 **TextBlock** 요소 아래에 추가합니다. **RelativePanel** _연결된 속성_을 설정하여 컨트롤을 제목 텍스트 아래에 배치하고 화면의 전체 너비에 맞게 늘립니다.

    **이 XAML 추가**

    ```xaml
    <GridView x:Name="ImageGridView"
              Margin="0,0,0,8"
              RelativePanel.AlignLeftWithPanel="True"
              RelativePanel.AlignRightWithPanel="True"
              RelativePanel.Below="TitleTextBlock"/>
    ```

    **TextBlock 뒤**
    ```xaml
    <RelativePanel Background="{ThemeResource ApplicationPageBackgroundThemeBrush}">
        <TextBlock x:Name="TitleTextBlock"
                   Text="Collection"
                   Margin="24,0,0,24"
                   Style="{StaticResource TitleTextBlockStyle}"/>
        
        <!-- Add the GridView control here. -->

    </RelativePanel>
    ```

    패널이 연결된 속성에 대한 자세한 내용은 [레이아웃 패널](https://docs.microsoft.com/windows/uwp/layout/layout-panels)을 참조하세요.

3. **GridView** 컨트롤에서 항목을 표시하려면 표시할 데이터 컬렉션을 제공해야 합니다. MainPage.xaml.cs를 열고 **GetItemsAsync** 메서드를 찾습니다. 이 메서드는 **MainPage**에 추가한 속성인 **Images**라는 컬렉션을 채웁니다.

    **GetItemsAsync**의 **foreach** 루프 이후에 이 코드 행을 추가합니다.

    ```csharp
    ImageGridView.ItemsSource = Images;
    ```

    이 단계에서는 **GridView** 컨트롤의 **ItemsSource** 속성을 앱의 **Images** 컬렉션으로 설정합니다. 또한 표시할 항목을 **GridView** 컨트롤에 제공합니다.

앱을 실행하고 모든 것이 제대로 작동하는지 확인할 수 있는 좋은 장소입니다. 이제 다음과 비슷한 모양입니다.

![앱 UI 검사점 1](images/xaml-basics/layout-0.png)

앱에 아직 이미지가 표시되지 않는 것을 알 수 있습니다. 기본적으로 앱은 컬렉션에 있는 데이터 형식의 **ToString** 값을 표시합니다. 다음으로, 데이터 템플릿을 만들어 데이터 표시 방법을 정의합니다.

> [!NOTE]
> **RelativePanel**을 사용하는 레이아웃은 [레이아웃 패널](https://docs.microsoft.com/windows/uwp/layout/layout-panels#relativepanel) 문서에서 자세히 알아볼 수 있습니다. 이 문서를 살펴본 다음, **TextBlock** 및 **GridView**에서 **RelativePanel** 연결된 속성을 설정하여 다른 레이아웃을 실험해 보세요.

## <a name="part-3-add-a-datatemplate-object-to-display-your-data"></a>3부: 데이터를 표시하는 DataTemplate 개체 추가

이제 데이터를 표시하는 방법을 **GridView**에 지시하는 [DataTemplate](https://docs.microsoft.com/uwp/api/windows.ui.xaml.datatemplate) 개체를 만듭니다. 데이터 템플릿의 전체 설명은 [항목 컨테이너 및 템플릿](../controls-and-patterns/item-containers-templates.md)을 참조하세요.

지금은 원하는 레이아웃을 만드는 데 도움이 되는 자리 표시자만 추가합니다. [XAML 데이터 바인딩](../../data-binding/xaml-basics-data-binding.md) 자습서에서는 이러한 자리 표시자를 **ImageFileInfo** 클래스의 실제 데이터로 바꿉니다. 데이터 개체의 모습을 보려면 ImageFileInfo.cs 파일을 열면 됩니다.

데이터 템플릿을 그리드 보기에 추가하려면 다음을 수행합니다.

1. MainPage.xaml을 엽니다.

2. 등급을 표시하려면 [UWP용 Telerik UI](https://github.com/telerik/UI-For-UWP) NuGet 패키지의 **RadRating** 컨트롤을 사용합니다. Telerik 컨트롤의 네임스페이스를 지정하는 XAML 네임스페이스 참조를 추가합니다. 이 참조를 다른 `xmlns:` 항목 바로 뒤의 여는 **Page**태그에 배치합니다.

    **이 XAML 추가**

    ```xaml
    xmlns:telerikInput="using:Telerik.UI.Xaml.Controls.Input"
    ```

    **마지막 `xmlns:` 항목 뒤**

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

3. 문서 개요에서 **ImageGridView**를 마우스 오른쪽 단추로 클릭합니다. 바로 가기 메뉴에서 **추가 템플릿 편집** > **생성된 항목 편집(ItemTemplate)**  > **빈 항목 만들기**를 차례로 선택합니다. **리소스 만들기** 대화 상자가 열립니다.

4. 대화 상자에서 **Name (key)** 값을 **ImageGridView_DefaultItemTemplate**으로 변경한 다음, **확인**을 선택합니다.

    **확인**을 선택하면 다음과 같은 몇 가지 작업이 수행됩니다.

    - **DataTemplate** 개체가 MainPage.xaml의 `Page.Resources` 섹션에 추가됩니다.

        ```xaml
        <Page.Resources>
            <DataTemplate x:Key="ImageGridView_DefaultItemTemplate">
                <Grid/>
            </DataTemplate>
        </Page.Resources>
        ```

    - [문서 개요]의 범위가 이 **DataTemplate** 개체로 설정됩니다.

        데이터 템플릿 만들기가 완료되면 [문서 개요]의 왼쪽 위 모서리에서 화살표를 선택하여 페이지 범위로 돌아갈 수 있습니다.

    - **GridView** 컨트롤의 **ItemTemplate** 속성은 **DataTemplate** 리소스로 설정됩니다.

       ```xaml
           <GridView x:Name="ImageGridView"
                     Margin="0,0,0,8"
                     RelativePanel.AlignLeftWithPanel="True"
                     RelativePanel.AlignRightWithPanel="True"
                     RelativePanel.Below="TitleTextBlock"
                     ItemTemplate="{StaticResource ImageGridView_DefaultItemTemplate}"/>
       ```

5. **ImageGridView_DefaultItemTemplate** 리소스에서 루트 **Grid**의 높이와 너비를 **300**으로, 여백을 **8**로 설정합니다. 그런 다음, 두 개의 행을 추가하고 두 번째 행의 높이를 **Auto**로 설정합니다.

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

    **Grid** 레이아웃에 대한 자세한 내용은 [레이아웃 패널](https://docs.microsoft.com/windows/uwp/layout/layout-panels#grid)을 참조하세요.

6. 컨트롤을 **Grid** 레이아웃에 추가합니다.

    a. 첫 번째 그리드 행에 **Image** 컨트롤을 추가합니다. 이는 이미지가 표시되는 위치입니다. 그러나 지금은 앱의 스토어 로고를 자리 표시자로 사용합니다.

    b. **TextBlock** 컨트롤을 추가하여 이미지의 이름, 파일 유형 및 크기를 표시합니다. 이를 위해 **StackPanel** 컨트롤을 사용하여 텍스트 블록을 정렬합니다.

    **StackPanel** 레이아웃에 대한 자세한 내용은 [레이아웃 패널](https://docs.microsoft.com/windows/uwp/layout/layout-panels#stackpanel)을 참조하세요.

    c. **RadRating** 컨트롤을 외부(세로) **StackPanel** 컨트롤에 추가합니다. 이 컨트롤을 내부(가로) **StackPanel** 컨트롤 뒤에 배치합니다.

    **최종 템플릿**

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

이제 앱을 실행하여 방금 만든 항목 템플릿이 있는 **GridView** 컨트롤을 확인합니다. 하지만 흰색 배경에 흰색 별이 있기 때문에 평점 컨트롤이 표시되지 않을 수 있습니다. 다음으로 배경색을 변경하겠습니다.

![앱 UI 검사점 2](images/xaml-basics/layout-1.png)

## <a name="part-4-modify-the-item-container-style"></a>4부: 항목 컨테이너 스타일 수정

항목의 컨트롤 템플릿에는 선택 항목, 포인터 가리키기 및 포커스와 같은 상태를 표시하는 시각적 개체가 포함됩니다. 이러한 시각적 개체는 데이터 템플릿의 위 또는 아래에 렌더링됩니다. 여기서는 컨트롤 템플릿의 **Background** 및 **Margin** 속성을 수정하여 **GridView** 항목에 회색 배경을 부여합니다.

항목 컨테이너를 수정하려면 다음을 수행합니다.

1. 문서 개요에서 **ImageGridView**를 마우스 오른쪽 단추로 클릭합니다. 바로 가기 메뉴에서 **추가 템플릿 편집** > **생성된 항목 컨테이너 편집(ItemContainerStyle)**  > **복사본 편집**을 차례로 선택합니다. **리소스 만들기** 대화 상자가 열립니다.

2. 대화 상자에서 **Name (key)** 값을 **ImageGridView_DefaultItemContainerStyle**로 변경한 다음, **확인**을 선택합니다.

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

    **GridViewItem** 기본 스타일은 많은 속성을 설정합니다. 항상 기본 스타일의 복사본으로 시작하고 필요한 속성만 수정해야 합니다. 그러지 않으면 일부 속성이 올바르게 설정되지 않으므로 시각적 개체가 예상대로 표시되지 않을 수 있습니다.

    이전 단계와 같이 **GridView** 컨트롤의 **ItemContainerStyle** 속성이 새 **Style** 리소스로 설정됩니다.

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

앱을 실행하고 현재의 모습을 확인합니다. 앱 창 크기를 조정합니다. **GridView** 컨트롤에서 이미지를 다시 정렬하는 작업을 수행하지만, 일부 너비의 경우 앱 창의 오른쪽에 많은 공간이 있습니다. 이미지가 중심에 있다면 더 보기 좋을 것입니다. 이 부분은 다음에 처리하겠습니다.

![앱 UI 검사점 3](images/xaml-basics/layout-2.png)

> [!Note]
> 실험하려면 **Background** 및 **Margin** 속성을 다른 값으로 설정하여 해당 효과를 확인해 보세요.

## <a name="part-5-apply-some-final-adjustments-to-the-layout"></a>5부: 레이아웃에 최종 조정 사항 적용

페이지에서 이미지를 가운데에 맞추려면 페이지에서 **Grid** 컨트롤의 맞춤을 조정해야 합니다. 또는 **GridView**에서 이미지 맞춤을 조정해야 하나요? 효과가 있을까요? 확인해 보겠습니다.

맞춤에 대한 자세한 내용은 [맞춤, 여백 및 안쪽 여백](../layout/alignment-margin-padding.md)을 참조하세요.

(이 단계에서 **GridView**의 **Background** 속성을 원하는 색으로 설정해 볼 수 있습니다. 레이아웃을 통해 어떤 일이 일어나는지 좀 더 분명하게 확인할 수 있습니다.)

이미지 맞춤을 수정하려면 다음을 수행합니다.

1. **GridView**에서 **HorizontalAlignment** 속성을 **Center**로 설정합니다.

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

2. 앱을 실행하고 창의 크기를 조정합니다. 아래로 스크롤하여 더 많은 이미지를 봅니다.

    이미지가 중앙에 위치해서 보기가 더 좋습니다. 그러나 스크롤 막대는 창의 가장자리 대신 **GridView** 컨트롤의 가장자리에 맞춰 정렬됩니다. 이 문제를 해결하려면 **GridView**를 페이지의 가운데에 맞추는 대신 이미지를 **GridView**의 가운데에 맞춥니다. 약간의 작업이 더 필요하지만 최종적인 모습은 더 보기 좋게 됩니다.

3. 이전 단계의 **HorizontalAlignment** 설정을 제거합니다.

4. 문서 개요에서 **ImageGridView**를 마우스 오른쪽 단추로 클릭합니다. 바로 가기 메뉴에서 **추가 템플릿 편집** > **항목 레이아웃 편집(ItemsPanel)**  > **복사본 편집**을 차례로 선택합니다. **리소스 만들기** 대화 상자가 열립니다.

5. 대화 상자에서 **Name (key)** 값을 **ImageGridView_ItemsPanelTemplate**으로 변경한 다음, **확인**을 선택합니다.

    기본 **ItemsPanelTemplate**이 XAML의 **Page.Resources** 섹션에 추가됩니다. (이전과 마찬가지로 이 리소스를 참조하도록 **GridView**가 업데이트됩니다.)

    ```xaml
    <ItemsPanelTemplate x:Key="ImageGridView_ItemsPanelTemplate">
        <ItemsWrapGrid Orientation="Horizontal" />
    </ItemsPanelTemplate>
    ```

    다양한 패널을 사용하여 앱의 컨트롤에 대한 레이아웃을 처리하는 것처럼 **GridView**에는 항목의 레이아웃을 관리하는 내부 패널이 있습니다. 이제 이 패널(**ItemsWrapGrid**)에 액세스할 수 있으므로 해당 속성을 수정하여 **GridView** 컨트롤 내의 항목 레이아웃을 변경할 수 있습니다.

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

7. 앱을 실행하고 창의 크기를 다시 조정합니다. 아래로 스크롤하여 더 많은 이미지를 봅니다.

![앱 UI 검사점 4](images/xaml-basics/layout-3.png)

이제 스크롤 막대가 창의 가장자리에 맞춰 정렬됩니다. 축하합니다. 앱의 기본 UI를 만들었습니다.

## <a name="go-further"></a>더 나아가기

이제 기본 UI를 만들었으므로 다른 자습서를 살펴보세요. 이러한 자습서도 PhotoLab 샘플을 기반으로 합니다. 

* [XAML 데이터 바인딩 자습서](../../data-binding/xaml-basics-data-binding.md)에 실제 이미지와 데이터를 추가합니다.
* [XAML 적응 레이아웃 자습서](xaml-basics-adaptive-layout.md)에서 여러 화면 크기에 맞게 조정되는 적응형 UI를 만듭니다.


## <a name="get-the-final-version-of-the-photolab-sample"></a>PhotoLab 샘플의 최종 버전 다운로드

이 자습서에서는 완전한 사진 편집 앱으로 빌드하지 않습니다. 따라서 [최종 버전](https://github.com/Microsoft/Windows-appsample-photo-lab)을 확인하여 사용자 지정 애니메이션 및 휴대폰 지원과 같은 다른 기능을 확인해야 합니다.

