---
ms.assetid: 333f67f5-f012-4981-917f-c6fd271267c6
description: 서 점에서 제공 된 정보를 기반으로 하는이 사례 연구는 데이터를 작성 하는 데 사용 되는 Windows Phone Silverlight 앱에서 시작 합니다.
title: Windows Phone Silverlight에서 UWP 사례 연구, Bookstore2
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 48603651c8e12ed452d8d3b136bfd3e9b8788316
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89172937"
---
# <a name="windowsphone-silverlight-to-uwp-case-study-bookstore2"></a>Windows Phone Silverlight에서 UWP 사례 연구: Bookstore2


[Bookstore1](wpsl-to-uwp-case-study-bookstore1.md)에 제공 된 정보를 기반으로 하는이 사례 연구는 데이터를 작성 하는 데 **Windows Phone 사용 됩니다.** 뷰 모델에서 클래스 **작성자** 의 각 인스턴스는 해당 저자가 작성 한 책의 그룹을 나타내고,이를 **선택**하면 author 별로 그룹화 된 책 목록을 볼 수 있습니다. 또는 축소 하 여 저자의 점프 목록을 볼 수 있습니다. 점프 목록은 책 목록을 스크롤할 때보다 훨씬 더 빠른 탐색이 가능케 합니다. 앱을 UWP (Windows 10 유니버설 Windows 플랫폼) 앱으로 이식 하는 단계를 안내 합니다.

**참고**    \_Visual studio에서 Bookstore2Universal 10을 열 때 "Visual studio 업데이트 필요" 메시지가 표시 되 면 [Targetplatformversion](w8x-to-uwp-troubleshooting.md)에서 대상 플랫폼 버전을 설정 하는 단계를 따릅니다.

## <a name="downloads"></a>다운로드

[Bookstore2WPSL8 Windows Phone Silverlight 앱을 다운로드](https://codeload.github.com/MicrosoftDocs/windows-topic-specific-samples/zip/Bookstore2WPSL8)합니다.

[Bookstore2Universal \_ 10 Windows 10 앱을 다운로드](https://codeload.github.com/MicrosoftDocs/windows-topic-specific-samples/zip/Bookstore2Universal_10)합니다.

##  <a name="the-windowsphone-silverlight-app"></a>Windows Phone Silverlight 앱

아래 그림은 Bookstore2WPSL8 (이식 가능한 앱)를 보여 줍니다. 작성자 별로 그룹화 된 책의 세로 스크롤 열을 **선택** 합니다. 점프 목록을 축소 하 고 원하는 그룹으로 다시 이동할 수 있습니다. 이 앱에는 그룹화 된 데이터 원본을 제공 하는 뷰 모델과 해당 뷰 모델에 바인딩되는 사용자 인터페이스가 있습니다. 여기에서 볼 수 있듯이 이러한 두 부분 모두 Windows Phone Silverlight 기술을 UWP (유니버설 Windows 플랫폼)로 쉽게 이식할 수 있습니다.

![bookstore2wpsl8 모양](images/wpsl-to-uwp-case-studies/c02-01-wpsl-how-the-app-looks.png)

##  <a name="porting-to-a-windows10-project"></a>Windows 10 프로젝트로 포팅

Visual Studio에서 새 프로젝트를 만들고, Bookstore2WPSL8에서 파일에 파일을 복사 하 고, 복사한 파일을 새 프로젝트에 포함 하는 것이 빠른 작업입니다. 새 빈 응용 프로그램 (Windows 유니버설) 프로젝트를 만들어 시작 합니다. 이름을 Bookstore2Universal \_ 10으로 합니다. Bookstore2WPSL8에서 Bookstore2Universal 10으로 복사할 파일 \_ 입니다.

-   책 표지 이미지를 포함 하는 폴더를 복사 합니다. PNG 파일 \\ ( \\ CoverImages 폴더)입니다. 폴더를 복사한 후 **솔루션 탐색기**에서 **모든 파일 표시** 가 설정 되어 있는지 확인 합니다. 복사한 폴더를 마우스 오른쪽 단추로 클릭 하 고 **프로젝트에 포함**을 클릭 합니다. 이 명령은 프로젝트의 파일 또는 폴더를 "포함" 하 여 의미 합니다. 파일이 나 폴더를 복사할 때마다 **솔루션 탐색기** 에서 **새로 고침** 을 클릭 한 다음 프로젝트에 파일 또는 폴더를 포함 합니다. 대상에서 대체 하는 파일에 대해서는이 작업을 수행할 필요가 없습니다.
-   뷰 모델 원본 파일이 포함 된 폴더를 복사 \\ 합니다 (ViewModel).
-   MainPage을 복사 하 고 대상의 파일을 바꿉니다.

Windows 10 프로젝트에서 Visual Studio가 생성 한 App.xaml.cs 및 Visual Studio를 유지할 수 있습니다.

방금 복사한 소스 코드 및 태그 파일을 편집 하 고 Bookstore2WPSL8 네임 스페이스에 대 한 참조를 Bookstore2Universal 10으로 변경 \_ 합니다. 이 작업을 수행 하는 빠른 방법은 **파일에서 바꾸기** 기능을 사용 하는 것입니다. 뷰 모델 소스 파일의 명령적 코드에서 이러한 이식 변경이 필요 합니다.

-   `System.ComponentModel.DesignerProperties`을로 변경 하 `DesignMode` 고이에 대해 **Resolve** 명령을 사용 합니다. 속성을 삭제 하 `IsInDesignTool` 고 IntelliSense를 사용 하 여 올바른 속성 이름를 추가 `DesignModeEnabled` 합니다.
-   에서 **Resolve** 명령을 사용 `ImageSource` 합니다.
-   에서 **Resolve** 명령을 사용 `BitmapImage` 합니다.
-   `using System.Windows.Media;`및를 삭제 `using System.Windows.Media.Imaging;` 합니다.
-   **Bookstore2Universal \_ ** 속성에서 반환 하는 값을 "BOOKSTORE2WPSL8"에서 "Bookstore2Universal"로 변경 합니다.
-   [Bookstore1](wpsl-to-uwp-case-study-bookstore1.md)와 마찬가지로 **CoverImage** 속성의 구현을 업데이트 합니다 ( [이미지를 뷰 모델에 바인딩](wpsl-to-uwp-case-study-bookstore1.md)참조).

MainPage .xaml에서 이러한 초기 포팅 변경이 필요 합니다.

-   `phone:PhoneApplicationPage`를로 변경 `Page` 합니다 (속성 요소 구문에 발생 하는 경우 포함).
-   `phone`및 `shell` 네임 스페이스 접두사 선언을 삭제 합니다.
-   나머지 네임 스페이스 접두사 선언에서 "clr-namespace"를 "using"으로 변경 합니다.
-   `SupportedOrientations="Portrait"` `Orientation="Portrait"` 새 프로젝트에서 앱 패키지 매니페스트의 **세로** 를 삭제 하 고, 및 구성 합니다.
-   `shell:SystemTray.IsVisible="True"`를 삭제합니다.
-   태그에 리소스로 있는 점프 목록 항목 변환기의 형식이 [**Windows.**](/uwp/api/Windows.UI.Xaml.Controls.Primitives) x m l. x m l. x m l 네임 스페이스로 이동 되었습니다. 따라서 네임 스페이스 접두사 선언 Windows \_ UI \_ Xaml \_ 컨트롤 \_ 기본 형식을 추가 하 고이를 **windows.** x m l. x m l. x m l에 매핑합니다. 점프 목록 항목 변환기 리소스에서 접두사를에서 `phone:` 로 변경 `Windows_UI_Xaml_Controls_Primitives:` 합니다.
-   [Bookstore1](wpsl-to-uwp-case-study-bookstore1.md)와 마찬가지로 TextBlock 스타일에 대 한 모든 참조를에 대 한 참조로 바꾸고,을로 바꾸고,을로 바꿉니다 `PhoneTextExtraLargeStyle`  **TextBlock** `SubtitleTextBlockStyle` `PhoneTextSubtleStyle` `SubtitleTextBlockStyle` `PhoneTextNormalStyle` `CaptionTextBlockStyle` `PhoneTextTitle1Style` `HeaderTextBlockStyle` .
-   에는 한 가지 예외가 있습니다 `BookTemplate` . 두 번째 **TextBlock** 의 스타일은를 참조 해야 합니다 `CaptionTextBlockStyle` .
-   안쪽 **TextBlock** 에서 FontFamily 특성을 제거 `AuthorGroupHeaderTemplate` 하 고 대신 **테두리** 의 배경을 참조로 설정 `SystemControlBackgroundAccentBrush` `PhoneAccentBrush` 합니다.
-   [뷰 픽셀과 관련 된 변경 사항](wpsl-to-uwp-porting-xaml-and-ui.md)으로 인해 태그를 살펴보고 고정 크기 치수 (여백, 너비, 높이 등)를 0.8으로 곱합니다.

## <a name="replacing-the-longlistselector"></a>가 나 Listselector 바꾸기


SemanticZoom 컨트롤 **을 사용 하 여** [**SemanticZoom**](/uwp/api/Windows.UI.Xaml.Controls.SemanticZoom) 컨트롤을 대체 하는 경우에는이 작업을 시작 하겠습니다. **SemanticZoom** **listselector** 는 그룹화 된 데이터 원본에 직접 바인딩되므로 [**collectionviewsource**](/uwp/api/Windows.UI.Xaml.Data.CollectionViewSource) 어댑터를 통해 데이터에 간접적으로 바인딩하는 [**ListView**](/uwp/api/Windows.UI.Xaml.Controls.ListView) 또는 [**GridView**](/uwp/api/Windows.UI.Xaml.Controls.GridView) 컨트롤을 포함 합니다. **Collectionviewsource** 는 리소스로 태그에 있어야 하므로, 내에서이를 mainpage의 태그에 추가 하 여 시작 `<Page.Resources>` 하겠습니다.

```xml
    <CollectionViewSource
        x:Name="AuthorHasACollectionOfBookSku"
        Source="{Binding Authors}"
        IsSourceGrouped="true"/>
```

System.windows.controls.itemscontrol.itemssource에 대 한 바인딩은 **collectionviewsource**의 값이 되 고 **IsGroupingEnabled** 는 **Collectionviewsource. issourcegrouped**가 됩니다. **LongListSelector.ItemsSource** **Collectionviewsource** 에는 이름 (참고: 필요할 수 있음)이 있으므로 바인딩할 수 있습니다.

그런 다음를 `phone:LongListSelector` 이 태그로 바꿔서 사용할 예비 **SemanticZoom** 제공 합니다.

```xml
    <SemanticZoom>
        <SemanticZoom.ZoomedInView>
            <ListView
                ItemsSource="{Binding Source={StaticResource AuthorHasACollectionOfBookSku}}"
                ItemTemplate="{StaticResource BookTemplate}">
                <ListView.GroupStyle>
                    <GroupStyle
                        HeaderTemplate="{StaticResource AuthorGroupHeaderTemplate}"
                        HidesIfEmpty="True"/>
                </ListView.GroupStyle>
            </ListView>
        </SemanticZoom.ZoomedInView>
        <SemanticZoom.ZoomedOutView>
            <ListView
                ItemsSource="{Binding CollectionGroups, Source={StaticResource AuthorHasACollectionOfBookSku}}"
                ItemTemplate="{StaticResource ZoomedOutAuthorTemplate}"/>
        </SemanticZoom.ZoomedOutView>
    </SemanticZoom>
```

기본 목록 및 점프 목록 모드의 **SemanticZoom** **개념은** 확대 된 보기와 축소 된 보기의 개념에 대 한 답변입니다. 확대 된 뷰는 속성 이며 해당 속성을 **ListView**의 인스턴스로 설정 합니다. 이 경우에는 축소 된 보기도 **listview**로 설정 되 고 두 **listview** 컨트롤은 모두 **collectionviewsource**에 바인딩됩니다. 확대 뷰에서는 동일한 항목 템플릿, 그룹 헤더 템플릿 및 **HideEmptyGroups** 설정 (현재 명명 된 HidesIfEmpty)을 사용 하 여 **HidesIfEmpty**의 **기본**목록으로 사용 합니다. 그리고 축소 된 보기는 항목 템플릿을 사용 하 **는 것과**매우 유사한 항목 템플릿을 사용 `AuthorNameJumpListStyle` 합니다. 또한 축소 된 보기가 **collectiongroups**라는 **collectionviewsource** 의 특수 속성에 바인딩됩니다 .이 속성은 항목이 아닌 그룹이 포함 된 컬렉션입니다.

더 이상 필요 하지 않습니다 `AuthorNameJumpListStyle` . 이 앱의 작성자 인 그룹에 대 한 데이터 템플릿만 축소 된 보기에서 필요 합니다. 따라서 스타일을 삭제 `AuthorNameJumpListStyle` 하 고이 데이터 템플릿으로 바꿉니다.

```xml
   <DataTemplate x:Key="ZoomedOutAuthorTemplate">
        <Border Margin="9.6,0.8" Background="{Binding Converter={StaticResource JumpListItemBackgroundConverter}}">
            <TextBlock Margin="9.6,0,9.6,4.8" Text="{Binding Group.Name}" Style="{StaticResource SubtitleTextBlockStyle}"
            Foreground="{Binding Converter={StaticResource JumpListItemForegroundConverter}}" VerticalAlignment="Bottom"/>
        </Border>
    </DataTemplate>
```

이 데이터 템플릿의 데이터 컨텍스트는 항목이 아닌 그룹 이므로 **그룹**이라는 특수 한 속성에 바인딩합니다.

이제 앱을 빌드하고 실행할 수 있습니다. 모바일 에뮬레이터에서 표시 되는 방법은 다음과 같습니다.

![초기 소스 코드가 변경 된 모바일의 uwp 앱](images/wpsl-to-uwp-case-studies/c02-02-mob10-initial-source-code-changes.png)

뷰 모델과 확대/축소 뷰 및 확대 뷰가 제대로 작동 하 고 있습니다. 단, 스타일 및 템플릿 작업을 조금 더 수행 해야 하는 문제가 있습니다. 예를 들어 올바른 스타일 및 브러시는 아직 사용 되지 않으므로 클릭 하 여 축소할 수 있는 그룹 머리글에 텍스트가 표시 되지 않습니다. 데스크톱 장치에서 앱을 실행 하는 경우 두 번째 문제가 표시 됩니다 .이는 앱에서 사용자 인터페이스를 아직 조정 하지 않아 windows가 모바일 장치 화면 보다 훨씬 더 커질 수 있는 큰 장치에서 최상의 환경을 제공 하 고 공간을 사용 하는 것입니다. 따라서 다음 몇 개의 섹션 ([초기 스타일 및 템플릿](#initial-styling-and-templating), [적응 UI](#adaptive-ui)및 [최종 스타일](#final-styling)지정)에서는 이러한 문제를 해결 합니다.

## <a name="initial-styling-and-templating"></a>초기 스타일 지정 및 템플릿 작업

그룹 머리글을 적절 하 게 확장 하려면 `AuthorGroupHeaderTemplate` 테두리의 **여백을** 편집 하 고 설정 `"0,0,0,9.6"` 합니다 **Border**.

책 항목을 깔끔하게 공간을 확보 하려면을 편집 하 `BookTemplate` 고 **Margin** `"9.6,0"` 두 개의 **TextBlock**에 대해 여백을로 설정 합니다.

내에서 앱 이름 및 페이지 제목의 레이아웃을 약간 개선 하려면 `TitlePanel` 값을로 설정 하 여 두 번째 **TextBlock** 의 위쪽 **여백을** 제거 합니다 `"7.2,0,0,0"` . 그리고 `TitlePanel` 그 자체로는 여백을로 설정 `0` (또는 사용자에 게 유용한 값)

`LayoutRoot`배경을로 변경 `"{ThemeResource ApplicationPageBackgroundThemeBrush}"` 합니다.

## <a name="adaptive-ui"></a>적응 UI

휴대폰 앱에서 시작 했기 때문에,이 단계에서 프로세스의이 단계에서 이식 된 앱의 UI 레이아웃은 작은 장치 및 좁은 windows에만 적합 합니다. 하지만 앱이 광범위 한 창에서 실행 되는 경우 (큰 화면을 사용 하는 장치 에서만 가능) UI 레이아웃을 사용 하 고 공간을 더 효율적으로 사용 하는 것이 좋습니다 .이는 앱의 창이 좁아서 (작은 장치에서 발생 하며, 대규모 장치 에서도 발생할 수 있음) UI를 사용 하는 것이 좋습니다.

적응 시각적 상태 관리자 기능을 사용 하 여이를 달성할 수 있습니다. 기본적으로 UI가 현재 사용 하 고 있는 템플릿을 사용 하 여 좁은 상태로 레이아웃 되도록 시각적 요소에 대 한 속성을 설정 합니다. 그런 다음 앱의 창이 특정 크기 ( [유효 픽셀](wpsl-to-uwp-porting-xaml-and-ui.md)단위로 측정 됨) 보다 크거나 같은 경우를 검색 하 고,이에 대 한 응답으로 시각적 요소의 속성을 변경 하 여 더 크고 더 넓은 레이아웃을 가져옵니다. 이러한 속성 변경 내용을 시각적 상태로 설정 하 고, 적응 트리거를 사용 하 여 해당 시각적 상태를 적용 하는지 여부를 지속적으로 모니터링 하 고 해당 시각적 상태를 적용 하는지 여부를 결정 합니다. 이 경우 창 너비를 트리거하기는 하지만 창 높이를 트리거할 수도 있습니다.

최소 창 너비 548 window.epx.codesnippet는이 사용 사례에 적합 합니다. 가장 작은 장치의 크기 이므로 넓은 레이아웃을 표시 하고자 합니다. 휴대폰은 일반적으로 548 epx 보다 작으므로,이 처럼 작은 장치에서는 기본 좁은 레이아웃으로 유지 됩니다. PC에서 창은 기본적으로 전체를 시작 하 여 와이드 상태로 전환 되며 250x250 크기의 항목이 표시 됩니다. 여기에서 250 x 250 항목의 최소 두 열을 표시 하는 데 충분 한 범위를 끌 수 있습니다. 이를 제외 하 고 트리거를 비활성화 하면 와이드 시각적 상태가 제거 되 고 기본 좁은 레이아웃이 적용 됩니다.

적응 시각적 상태 관리자 피스를 주요 당면 전에 먼저 광범위 한 상태를 디자인 하 고 태그에 몇 가지 새로운 시각적 요소와 템플릿을 추가 해야 합니다. 이러한 단계는이 작업을 수행 하는 방법을 설명 합니다. 시각적 요소 및 템플릿에 대 한 명명 규칙을 통해 전체 상태에 해당 하는 요소 또는 템플릿의 이름에 "wide" 라는 단어를 포함 합니다. 요소 또는 템플릿에 "wide" 라는 단어가 포함 되지 않은 경우에는 기본 상태이 고 해당 속성 값이 페이지의 시각적 요소에 대 한 로컬 값으로 설정 되는 좁은 상태에 대 한 것으로 간주할 수 있습니다. 전체 상태에 대 한 속성 값만 태그의 실제 시각적 상태를 통해 설정 됩니다.

-   태그에서 [**SemanticZoom**](/uwp/api/Windows.UI.Xaml.Controls.SemanticZoom) 컨트롤의 복사본을 만들고 `x:Name="narrowSeZo"` 복사본에 대해를 설정 합니다. 원본에서 `x:Name="wideSeZo"`를 설정하고 기본적으로 넓은 보기가 표시되지 않도록 `Visibility="Collapsed"`도 설정합니다.
-   에서 `wideSeZo` **ListView**s를 확대 뷰와 축소 된 보기 모두에서 **GridView**로 변경 합니다.
-   이러한 세 리소스의 복사본을 만들고 `AuthorGroupHeaderTemplate` `ZoomedOutAuthorTemplate` `BookTemplate` `Wide` 복사본의 키에 단어를 추가 합니다. 또한 `wideSeZo` 이러한 새 리소스의 키를 참조 하도록 업데이트 합니다.
-   의 내용을로 바꿉니다 `AuthorGroupHeaderTemplateWide` `<TextBlock Style="{StaticResource SubheaderTextBlockStyle}" Text="{Binding Name}"/>` .
-   `ZoomedOutAuthorTemplateWide`의 내용을 다음으로 바꿉니다.

```xml
    <Grid HorizontalAlignment="Left" Width="250" Height="250" >
        <Border Background="{StaticResource ListViewItemPlaceholderBackgroundThemeBrush}"/>
        <StackPanel VerticalAlignment="Bottom" Background="{StaticResource ListViewItemOverlayBackgroundThemeBrush}">
          <TextBlock Foreground="{StaticResource ListViewItemOverlayForegroundThemeBrush}"
              Style="{StaticResource SubtitleTextBlockStyle}"
            Height="80" Margin="15,0" Text="{Binding Group.Name}"/>
        </StackPanel>
    </Grid>
```

-   `BookTemplateWide`의 내용을 다음으로 바꿉니다.

```xml
    <Grid HorizontalAlignment="Left" Width="250" Height="250">
        <Border Background="{StaticResource ListViewItemPlaceholderBackgroundThemeBrush}"/>
        <Image Source="{Binding CoverImage}" Stretch="UniformToFill"/>
        <StackPanel VerticalAlignment="Bottom" Background="{StaticResource ListViewItemOverlayBackgroundThemeBrush}">
            <TextBlock Style="{StaticResource SubtitleTextBlockStyle}"
                Foreground="{StaticResource ListViewItemOverlaySecondaryForegroundThemeBrush}"
                TextWrapping="NoWrap" TextTrimming="CharacterEllipsis"
                Margin="12,0,24,0" Text="{Binding Title}"/>
            <TextBlock Style="{StaticResource CaptionTextBlockStyle}" Text="{Binding Author.Name}"
                Foreground="{StaticResource ListViewItemOverlaySecondaryForegroundThemeBrush}" TextWrapping="NoWrap"
                TextTrimming="CharacterEllipsis" Margin="12,0,12,12"/>
        </StackPanel>
    </Grid>
```

-   넓은 상태의 경우에는 확대 뷰의 그룹에 더 많은 수직 보냅니다 공간이 필요 합니다. 항목 패널 템플릿을 만들고 참조 하면 원하는 결과를 얻을 수 있습니다. 태그 모양은 다음과 같습니다.

```xml
   <ItemsPanelTemplate x:Key="ZoomedInItemsPanelTemplate">
        <ItemsWrapGrid Orientation="Horizontal" GroupPadding="0,0,0,20"/>
    </ItemsPanelTemplate>
    ...

    <SemanticZoom x:Name="wideSeZo" ... >
        <SemanticZoom.ZoomedInView>
            <GridView
            ...
            ItemsPanel="{StaticResource ZoomedInItemsPanelTemplate}">
            ...
```

-   마지막으로 적절 한 시각적 상태 관리자 태그를의 첫 번째 자식으로 추가 `LayoutRoot` 합니다.

```xml
    <Grid x:Name="LayoutRoot" ... >
        <VisualStateManager.VisualStateGroups>
            <VisualStateGroup>
                <VisualState x:Name="WideState">
                    <VisualState.StateTriggers>
                        <AdaptiveTrigger MinWindowWidth="548"/>
                    </VisualState.StateTriggers>
                    <VisualState.Setters>
                        <Setter Target="wideSeZo.Visibility" Value="Visible"/>
                        <Setter Target="narrowSeZo.Visibility" Value="Collapsed"/>
                    </VisualState.Setters>
                </VisualState>
            </VisualStateGroup>
        </VisualStateManager.VisualStateGroups>

    ...
```

## <a name="final-styling"></a>최종 스타일 지정

그 중 일부는 최종 스타일 조정의 일부입니다.

-   에서 `AuthorGroupHeaderTemplate` , `Foreground="White"` 모바일 장치 제품군에서 실행 될 때 올바르게 보이도록 **TextBlock** 에 설정 합니다.
-   `FontWeight="SemiBold"`및의 **TextBlock** 에를 추가 `AuthorGroupHeaderTemplate` `ZoomedOutAuthorTemplate` 합니다.
-   에서 `narrowSeZo` 축소 된 보기의 그룹 머리글과 작성자는 확장 된 대신 왼쪽 맞춤 되므로 작업을 수행해 보겠습니다. [**System.windows.controls.control.horizontalcontentalignment**](/uwp/api/windows.ui.xaml.controls.control.horizontalcontentalignment) 를로 설정 하 여 확대 된 보기에 대 한 [**HeaderContainerStyle**](/uwp/api/windows.ui.xaml.controls.groupstyle.headercontainerstyle) 를 만듭니다 `Stretch` . 동일한 [**Setter**](/uwp/api/Windows.UI.Xaml.Setter)를 포함 하는 축소 된 뷰에 대해 [**system.windows.controls.itemscontrol.itemcontainerstyle**](/uwp/api/windows.ui.xaml.controls.itemscontrol.itemcontainerstyle) 를 만듭니다. 모양은 다음과 같습니다.

```xml
   <Style x:Key="AuthorGroupHeaderContainerStyle" TargetType="ListViewHeaderItem">
        <Setter Property="HorizontalContentAlignment" Value="Stretch"/>
    </Style>

    <Style x:Key="ZoomedOutAuthorItemContainerStyle" TargetType="ListViewItem">
        <Setter Property="HorizontalContentAlignment" Value="Stretch"/>
    </Style>

    ...

    <SemanticZoom x:Name="narrowSeZo" ... >
        <SemanticZoom.ZoomedInView>
            <ListView
            ...
                <ListView.GroupStyle>
                    <GroupStyle
                    ...
                    HeaderContainerStyle="{StaticResource AuthorGroupHeaderContainerStyle}"
                    ...
        <SemanticZoom.ZoomedOutView>
            <ListView
                ...
                ItemContainerStyle="{StaticResource ZoomedOutAuthorItemContainerStyle}"
                ...
```

스타일 지정 작업의 마지막 시퀀스는 앱을 다음과 같이 그대로 둡니다.

![데스크톱 장치에서 실행 되는 이식 된 windows 10 앱, 확대 된 보기, 2 가지 크기의 창](images/w8x-to-uwp-case-studies/c02-07-desk10-zi-ported.png)

데스크톱 장치에서 실행 되는 이식 된 Windows 10 앱, 확대 된 보기,  
 ![ 데스크톱 장치에서 실행 되는 이식 된 windows 10 앱의 2 가지 크기, 축소 된 보기, 2 가지 크기의 창](images/w8x-to-uwp-case-studies/c02-08-desk10-zo-ported.png)

데스크톱 장치에서 실행 되는 이식 된 Windows 10 앱, 축소 된 보기, 2 가지 크기의 창

![모바일 장치에서 실행 되는 이식 된 windows 10 앱, 확대 된 보기](images/w8x-to-uwp-case-studies/c02-09-mob10-zi-ported.png)

모바일 장치에서 실행 되는 이식 된 Windows 10 앱, 확대 된 보기

![모바일 장치에서 실행 되는 이식 된 windows 10 앱, 축소 된 보기](images/w8x-to-uwp-case-studies/c02-10-mob10-zo-ported.png)

모바일 장치에서 실행 되는 이식 된 Windows 10 앱, 축소 된 보기

## <a name="making-the-view-model-more-flexible"></a>뷰 모델을 보다 유연 하 게 만들기

이 섹션에는 UWP를 사용 하도록 앱을 이동 하 여 최대 microsoft에서 열리는 기능의 예가 포함 되어 있습니다. 여기서는 **Collectionviewsource**를 통해 액세스 하는 경우 뷰 모델을 보다 유연 하 게 만들기 위해 수행할 수 있는 선택적 단계에 대해 설명 합니다. \\Windows Phone Silverlight 앱 Bookstore2WPSL8에서 이식 한 뷰 모델 (ViewModel BookstoreViewModel.cs에 포함 됨)은 **목록 &lt; &gt; t**에서 파생 되는 Author 라는 클래스를 포함 합니다. 여기서 **t** 는 booksku입니다. 즉, Author 클래스는 BookSku *그룹입니다.*

**Collectionviewsource. 원본을** 작성자에 게 바인딩하는 경우 작성자의 각 작성자가 *특정 항목*의 그룹 일 뿐입니다. 이를 **Collectionviewsource** 에 남겨 두어 작성자가 (이 경우 booksku 그룹)를 확인 합니다. 이는 작동 하지만 유연 하지 않습니다. 작성자가 BookSku 그룹이 *고* 작성자가 존속 한 주소 그룹을 *모두* 포함 하려면 어떻게 해야 하나요? 작성자는 이러한 그룹 중 *하나일* 수 없습니다. 그러나 작성자에 게는 원하는 수의 그룹이 *있을* 수 있습니다. 그리고 해결 방법: 현재 사용 하 고 있는- *a* 그룹 패턴 외에도 대신 *그룹* 패턴을 사용 합니다. 방법은 다음과 같습니다.

-   만든이가 **목록 &lt; T &gt; **에서 더 이상 파생 되지 않도록 변경 합니다.
-   이 필드를에 추가 합니다. 
-   이 속성을에 추가 합니다. 
-   물론 위의 두 단계를 반복 하 여 필요한 만큼 작성할 그룹을 추가할 수 있습니다.
-   AddBookSku 메서드의 구현을로 변경 `this.BookSkus.Add(bookSku);` 합니다.
-   이제 '작성자'가 하나 이상의 그룹을 *가졌으므로*, **CollectionViewSource**과(와) 통신해야 합니다. 이렇게 하려면 **Collectionviewsource**에 다음 속성을 추가 합니다. `ItemsPath="BookSkus"`

이러한 변경으로 인해이 앱은 변경 되지 않고 그대로 유지 되지만, 이제 작성자와 **Collectionviewsource**를 확장 하는 방법을 알고 있어야 합니다. **ItemsPath**를 지정 *하지 않고* 사용 하는 경우 선택 항목의 기본 그룹이 사용 되도록 작성자에 게 마지막으로 변경 내용을 적용 합니다.

```csharp
    public class Author : IEnumerable<BookSku>
    {
        ...

        public IEnumerator<BookSku> GetEnumerator()
        {
            return this.BookSkus.GetEnumerator();
        }
        System.Collections.IEnumerator System.Collections.IEnumerable.GetEnumerator()
        {
            return this.BookSkus.GetEnumerator();
        }
    }
```

이제 원하는 경우 제거 하도록 선택할 수 있으며 `ItemsPath="BookSkus"` 앱은 여전히 동일한 방식으로 동작 합니다.

## <a name="conclusion"></a>결론

이 사례 연구는 이전 보다 더 원대한 사용자 인터페이스를 포함 합니다. Windows Phone Silverlight 이상 **목록 선택기**등의 모든 기능 및 개념은 UWP 앱에서 **SemanticZoom**, **ListView**, **GridView**및 **collectionviewsource**형식으로 사용할 수 있습니다. UWP 앱에서 명령적 코드 및 태그 모두를 다시 사용 하거나 복사 및 편집 하 여 가장 작은 규모의 Windows 장치 폼 팩터 및 모든 크기에 맞게 조정 된 기능, UI 및 상호 작용을 구현 하는 방법을 살펴보았습니다.