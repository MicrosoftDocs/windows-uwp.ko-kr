---
description: 연결된 애니메이션을 사용하면 두 가지 보기 간에 전환되는 동작에 애니메이션 효과를 적용하여 역동적이고 매력적인 탐색 환경을 만들 수 있습니다.
title: 연결된 애니메이션
template: detail.hbs
ms.date: 10/04/2018
ms.topic: article
keywords: windows 10, uwp
pm-contact: stmoy
design-contact: conrwi
doc-status: Published
ms.localizationpriority: medium
ms.openlocfilehash: 21e7c026d336507b1a82badba770ac3bb50e19f8
ms.sourcegitcommit: f0f933d5cf0be734373a7b03e338e65000cc3d80
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 05/21/2019
ms.locfileid: "65984125"
---
# <a name="connected-animation-for-uwp-apps"></a>UWP 앱에 대한 연결된 애니메이션

연결된 애니메이션을 사용하면 두 가지 보기 간에 전환되는 동작에 애니메이션 효과를 적용하여 역동적이고 매력적인 탐색 환경을 만들 수 있습니다. 이렇게 하면 사용자가 컨텍스트를 유지하는 데 도움이 될 뿐 아니라 보기 간에 연속성이 보장됩니다.

연결 된 애니메이션 요소 "계속" 화면에서 새 보기에서 해당 대상에 소스 뷰에서 해당 위치에서 비행 UI 콘텐츠를 변경 하는 동안 두 뷰 간에 표시 됩니다. 이 뷰 간에 공통 콘텐츠를 강조 하 고 전환의 일부로 멋진 및 동적 효과 만듭니다.

> **중요 API**:  [ConnectedAnimation 클래스](/uwp/api/windows.ui.xaml.media.animation.connectedanimation), [ConnectedAnimationService 클래스](/uwp/api/windows.ui.xaml.media.animation.connectedanimationservice)


## <a name="examples"></a>예

<table>
<th align="left">XAML 컨트롤 갤러리<th>
<tr>
<td><img src="images/xaml-controls-gallery-app-icon.png" alt="XAML controls gallery" width="168"></img></td>
<td>
    <p>있는 경우는 <strong style="font-weight: semi-bold">XAML 컨트롤 갤러리</strong> 앱을 설치 하려면 여기를 클릭 <a href="xamlcontrolsgallery:/item/ConnectedAnimation">앱을 열고 작업에 연결 된 애니메이션을 참조 하세요.</a>합니다.</p>
    <ul>
    <li><a href="https://www.microsoft.com/store/productId/9MSVH128X2ZT">XAML Controls Gallery 앱 가져오기(Microsoft Store)</a></li>
    <li><a href="https://github.com/Microsoft/Xaml-Controls-Gallery">소스 코드 가져오기(GitHub)</a></li>
    </ul>
</td>
</tr>
</table>

이 짧은 비디오에서는 앱의 다음 페이지 헤더의 일부로 "계속" 항목 이미지를 애니메이션 효과를 주는 연결 된 애니메이션을 사용 합니다. 이 효과는 전환 시 사용자 컨텍스트를 유지하는 데 도움이 됩니다.

![연결된 애니메이션](images/connected-animations/example.gif)

<!-- 
<iframe width=640 height=360 src='https://microsoft.sharepoint.com/portals/hub/_layouts/15/VideoEmbedHost.aspx?chId=552c725c%2De353%2D4118%2Dbd2b%2Dc2d0584c9848&amp;vId=b2daa5ee%2Dbe15%2D4503%2Db541%2D1328a6587c36&amp;width=640&amp;height=360&amp;autoPlay=false&amp;showInfo=true' allowfullscreen></iframe>
-->

## <a name="video-summary"></a>비디오 요약

> [!VIDEO https://channel9.msdn.com/Events/Windows/Windows-Developer-Day-Fall-Creators-Update/WinDev005/player]

## <a name="connected-animation-and-the-fluent-design-system"></a>연결된 애니메이션 및 Fluent 디자인 시스템

 Fluent 디자인 시스템을 사용하면 조명, 깊이, 움직임, 재질 및 배율이 통합된 선명한 현대식 UI를 만들 수 있습니다. 연결된 애니메이션은 앱에 동작을 추가하는 Fluent 디자인 시스템 구성 요소입니다. 자세히 알아보려면 [UWP용 Fluent 디자인 개요](/windows/apps/fluent-design-system)를 확인하십시오.

## <a name="why-connected-animation"></a>연결된 애니메이션을 사용하는 이유

페이지를 탐색할 때 사용자는 탐색 후 어떤 새 콘텐츠가 제공되며 탐색할 때 해당 콘텐츠가 사용자의 의도와 어떤 관계가 있는지 이해할 수 있어야 합니다. 연결된 애니메이션은 두 보기 간에 공유되는 콘텐츠에 사용자의 포커스를 그려서 두 보기 간의 관계를 강조함으로써 강력한 시각적 비유를 제공합니다. 뿐만 아니라 연결된 애니메이션은 페이지 탐색에 앱의 동작 디자인을 차별화하는 시각적 흥미와 세련미를 더해 줍니다.

## <a name="when-to-use-connected-animation"></a>연결된 애니메이션을 사용하는 시기

연결된 애니메이션은 UI의 콘텐츠를 변경하고 사용자로 하여금 컨텍스트를 유지하게 하려는 모든 환경에 적용할 수 있지만 일반적으로 페이지를 변경할 때 사용됩니다. 원본 보기와 대상 보기 사이에 공유되는 이미지 또는 기타 UI 부분이 있을 때에는 [드릴인 탐색 전환](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.media.animation.navigationthemetransition.aspx) 대신 연결된 애니메이션을 사용하는 방안을 고려해야 합니다.

## <a name="configure-connected-animation"></a>연결 된 애니메이션을 구성 합니다.

> [!IMPORTANT]
> 이 기능을 사용 하려면 앱의 대상 버전 되도록 Windows 10 버전 1809 ([17763 SDK](https://developer.microsoft.com/windows/downloads/windows-10-sdk)) 이상. 구성 속성을 이전 Sdk에서 사용할 수 없는 경우 SDK 17763 보다 작은 최소 버전을 대상으로 지정할 수 있습니다 적응 코드 또는 조건부 XAML을 사용 하 여 합니다. 자세한 내용은 참조 하세요. [적응 응용 프로그램 버전](/windows/uwp/debug-test-perf/version-adaptive-apps)합니다.

Windows 10 1809, 버전부터 연결 된 애니메이션 추가 규제가 Fluent 디자인 애니메이션을 제공 하 여 구성을 맞는 전달에 맞게 및 이전 버전과 페이지 탐색 합니다.

ConnectedAnimation에서 구성 속성을 설정 하 여 애니메이션 구성을 지정 합니다. (살펴봅니다이 예가 다음 섹션에서.)

이 표에서 사용 가능한 구성 합니다. 이러한 애니메이션에 적용 하는 동작 원칙에 대 한 자세한 내용은 참조 하세요. [방향 및 중력](index.md)합니다.

| [GravityConnectedAnimationConfiguration](/uwp/api/windows.ui.xaml.media.animation.gravityconnectedanimationconfiguration) |
| - |
| 기본 구성 이며 앞으로 탐색을 위한 것이 좋습니다. |
사용자가 (A B로) 앱에서 앞으로 탐색, 연결 된 요소를 물리적으로 "페이지" 표시 됩니다. 이렇게 요소 z 공간에서 앞으로 이동 하려면 표시 되 고 잠시 대기를 수행 하는 중력의 영향으로 삭제 합니다. 을 극복 하기 위해 중력의 영향 요소 개발 속도 향상 및 최종 위치로 가속화 합니다. 결과 "확장 및 dip" 애니메이션 합니다. |

| [DirectConnectedAnimationConfiguration](/uwp/api/windows.ui.xaml.media.animation.directconnectedanimationconfiguration) |
| - |
| 사용자 앱 (A B)에서 뒤로 탐색을 애니메이션 보다 직접적인입니다. 연결 된 요소가 사용 하 여 감속 입방 형 3 차원 곡선 감속/가속 함수 B에서 선형적으로 변환 합니다. 이전 버전과 visual 유도성 반환 사용자를 이전 상태로 최대한 빠르게 탐색 흐름의 컨텍스트를 유지 하면서 합니다. |

| [BasicConnectedAnimationConfiguration](/uwp/api/windows.ui.xaml.media.animation.basicconnectedanimationconfiguration) |
| - |
| 이것이 기본 (및 유일한) Windows 10 버전 1809 이전 버전에 사용 되는 애니메이션 ([17763 SDK](https://developer.microsoft.com/windows/downloads/windows-10-sdk)). |

### <a name="connectedanimationservice-configuration"></a>ConnectedAnimationService 구성

합니다 [ConnectedAnimationService](/uwp/api/windows.ui.xaml.media.animation.connectedanimationservice) 클래스에는 전체 서비스가 아닌 개별 애니메이션에 적용 되는 두 가지 속성이 있습니다.

- [DefaultDuration](/uwp/api/windows.ui.xaml.media.animation.connectedanimationservice.defaultduration)
- [DefaultEasingFunction](/uwp/api/windows.ui.xaml.media.animation.connectedanimationservice.defaulteasingfunction)

다양 한 효과 달성 하려면 일부 구성 ConnectedAnimationService에서 이러한 속성을 무시 하 고 고유한 값을이 테이블에 설명 된 대로 사용 합니다.

| Configuration | 측면 DefaultDuration? | 측면 DefaultEasingFunction? |
| - | - | - |
| 무게 | 예 | 예* <br/> **A에서 B에 대 한 기본 변환을이 감속/가속 함수를 사용 하지만 "중력 dip" 자체 감속/가속 함수입니다.*  |
| 직접 | 아니요 <br/> *개 150ms 애니메이션 효과 줍니다.*| 아니오 <br/> *감속/가속 함수 감속을 사용 합니다.* |
| Basic | 예 | 예 |

## <a name="how-to-implement-connected-animation"></a>연결 된 애니메이션을 구현 하는 방법

연결된 애니메이션을 설정하려면 다음 두 단계를 거쳐야 합니다.

1. *준비* 소스 요소에 연결 된 애니메이션 참여할 시스템을 나타내는 원본 페이지에서 애니메이션 개체입니다.
1. *시작* 애니메이션 대상 페이지에서 대상 요소에 대 한 참조를 전달 합니다.

원본 페이지를 탐색 하는 경우 호출 [ConnectedAnimationService.GetForCurrentView](/uwp/api/windows.ui.xaml.media.animation.connectedanimationservice.getforcurrentview) ConnectedAnimationService의 인스턴스를 가져옵니다. 애니메이션을 준비 하려면 호출 [PrepareToAnimate](/uwp/api/windows.ui.xaml.media.animation.connectedanimationservice.preparetoanimate) 이 인스턴스를 고유 키의 전환에 사용 하려는 UI 요소에 전달 합니다. 고유 키를 사용 하면 애니메이션 대상 페이지에서 나중에 검색할 수 있습니다.

```csharp
ConnectedAnimationService.GetForCurrentView()
    .PrepareToAnimate("forwardAnimation", SourceImage);
```

탐색이 발생할 때, 대상 페이지에 애니메이션을 시작 합니다. 애니메이션을 시작하려면 [ConnectedAnimation.TryStart](/uwp/api/windows.ui.xaml.media.animation.connectedanimation.trystart)를 호출합니다. 애니메이션을 만들 때 제공한 고유 키로 [ConnectedAnimationService.GetAnimation](/uwp/api/windows.ui.xaml.media.animation.connectedanimationservice.getanimation)을 호출하여 올바른 애니메이션 인스턴스를 검색할 수 있습니다.

```csharp
ConnectedAnimation animation =
    ConnectedAnimationService.GetForCurrentView().GetAnimation("forwardAnimation");
if (animation != null)
{
    animation.TryStart(DestinationImage);
}
```

### <a name="forward-navigation"></a>앞으로 탐색

이 예제에서는 두 페이지 (Page_B Page_A) 사이 탐색 전달에 대 한 전환을 만들려면 ConnectedAnimationService를 사용 하는 방법을 보여 줍니다.

앞으로 탐색에 대 한 권장 되는 애니메이션 구성은 [GravityConnectedAnimationConfiguration](/uwp/api/windows.ui.xaml.media.animation.gravityconnectedanimationconfiguration)합니다. 설정할 필요가 없습니다 기본값 이므로 합니다 [구성](/uwp/api/windows.ui.xaml.media.animation.connectedanimation.configuration) 속성 다른 구성을 지정 하려는 경우가 아니면 합니다.

원본 페이지에서 애니메이션을 설정 합니다.

```xaml
<!-- Page_A.xaml -->

<Image x:Name="SourceImage"
       HorizontalAlignment="Left" VerticalAlignment="Top"
       Width="200" Height="200"
       Stretch="Fill"
       Source="Assets/StoreLogo.png"
       PointerPressed="SourceImage_PointerPressed"/>
```

```csharp
// Page_A.xaml.cs

private void SourceImage_PointerPressed(object sender, PointerRoutedEventArgs e)
{
    // Navigate to detail page.
    // Suppress the default animation to avoid conflict with the connected animation.
    Frame.Navigate(typeof(Page_B), null, new SuppressNavigationTransitionInfo());
}

protected override void OnNavigatingFrom(NavigatingCancelEventArgs e)
{
    ConnectedAnimationService.GetForCurrentView()
        .PrepareToAnimate("forwardAnimation", SourceImage);
    // You don't need to explicitly set the Configuration property because
    // the recommended Gravity configuration is default.
    // For custom animation, use:
    // animation.Configuration = new BasicConnectedAnimationConfiguration();
}
```

대상 페이지에 애니메이션을 시작 합니다.

```xaml
<!-- Page_B.xaml -->

<Image x:Name="DestinationImage"
       Width="400" Height="400"
       Stretch="Fill"
       Source="Assets/StoreLogo.png" />
```

```csharp
// Page_B.xaml.cs

protected override void OnNavigatedTo(NavigationEventArgs e)
{
    base.OnNavigatedTo(e);

    ConnectedAnimation animation =
        ConnectedAnimationService.GetForCurrentView().GetAnimation("forwardAnimation");
    if (animation != null)
    {
        animation.TryStart(DestinationImage);
    }
}
```

### <a name="back-navigation"></a>뒤로 탐색

뒤로 탐색 (Page_A Page_B)을 위한 동일한 단계를 수행할 수 있지만 소스 및 대상 페이지 반대가 됩니다.

사용자가 다시 탐색를 이전 상태로 반환 되는 가능한 한 빨리 앱을 기대 합니다. 따라서 권장 구성은 [DirectConnectedAnimationConfiguration](/uwp/api/windows.ui.xaml.media.animation.directconnectedanimationconfiguration)합니다. 이 애니메이션 더 빠르게, 더 지원 하며 감속/가속 감속을 사용 합니다.

원본 페이지에서 애니메이션을 설정 합니다.

```csharp
// Page_B.xaml.cs

protected override void OnNavigatingFrom(NavigatingCancelEventArgs e)
{
    if (e.NavigationMode == NavigationMode.Back)
    {
        ConnectedAnimationService.GetForCurrentView()
            .PrepareToAnimate("backAnimation", DestinationImage);

        // Use the recommended configuration for back animation.
        animation.Configuration = new DirectConnectedAnimationConfiguration();
    }
}
```

대상 페이지에 애니메이션을 시작 합니다.

```csharp
// Page_A.xaml.cs

protected override void OnNavigatedTo(NavigationEventArgs e)
{
    base.OnNavigatedTo(e);

    ConnectedAnimation animation =
        ConnectedAnimationService.GetForCurrentView().GetAnimation("backAnimation");
    if (animation != null)
    {
        animation.TryStart(SourceImage);
    }
}
```

애니메이션은 설정 및 시작 될 때 시간으로 사이의 소스 요소를 앱의 다른 UI 위에 고정 나타납니다. 이렇게 하면 다른 전환 애니메이션을 동시에 수행할 수 있습니다. 이러한 이유로 기다릴 수는 없습니다 ~ 250 개 시간 (밀리초)의 두 단계 사이 하므로 원본 요소가 방해가 될 수 있습니다. 애니메이션을 준비하고 3초 이내에 시작하지 않으면 시스템에서 애니메이션을 삭제하고 후속 [TryStart](/uwp/api/windows.ui.xaml.media.animation.connectedanimation.trystart) 호출이 실패합니다.

## <a name="connected-animation-in-list-and-grid-experiences"></a>목록 및 그리드 환경의 연결된 애니메이션

목록 또는 그리드 컨트롤에서/로 연결된 애니메이션을 만들고 싶은 경우가 종종 있을 것입니다. 두 메서드를 사용할 수 있습니다 [ListView](/uwp/api/windows.ui.xaml.controls.listview) 하 고 [GridView](/uwp/api/windows.ui.xaml.controls.gridview)를 [PrepareConnectedAnimation](/uwp/api/windows.ui.xaml.controls.listviewbase.prepareconnectedanimation) 고 [TryStartConnectedAnimationAsync](/uwp/api/windows.ui.xaml.controls.listviewbase.trystartconnectedanimationasync), 이 프로세스를 간소화 하 합니다.

예를 들어 데이터 템플릿에 "PortraitEllipse"라는 이름의 요소가 들어 있는 **ListView**가 있다고 가정해 봅시다.

```xaml
<ListView x:Name="ContactsListView" Loaded="ContactsListView_Loaded">
    <ListView.ItemTemplate>
        <DataTemplate x:DataType="vm:ContactsItem">
            <Grid>
                …
                <Ellipse x:Name="PortraitEllipse" … />
            </Grid>
        </DataTemplate>
    </ListView.ItemTemplate>
</ListView>
```

타원을 지정 된 목록 항목에 해당 하는 연결 된 애니메이션을 준비 하려면 다음을 호출 합니다 [PrepareConnectedAnimation](/uwp/api/windows.ui.xaml.controls.listviewbase.prepareconnectedanimation) 메서드 이름을 사용 하 여 고유 키, 항목을 "PortraitEllipse"입니다.

```csharp
void PrepareAnimationWithItem(ContactsItem item)
{
     ContactsListView.PrepareConnectedAnimation("portrait", item, "PortraitEllipse");
}
```

세부 정보 뷰에서 경우 뒤로 탐색 등을 대상으로이 요소를 사용 하 여 애니메이션을 시작 하려면 [TryStartConnectedAnimationAsync](/uwp/api/windows.ui.xaml.controls.listviewbase.trystartconnectedanimationasync)합니다. ListView에 대 한 바로 데이터 소스를 로드 하는 경우 해당 항목 컨테이너에 생성 될 때까지 애니메이션을 시작 하려면 TryStartConnectedAnimationAsync 대기 합니다.

```csharp
private void ContactsListView_Loaded(object sender, RoutedEventArgs e)
{
    ContactsItem item = GetPersistedItem(); // Get persisted item
    if (item != null)
    {
        ContactsListView.ScrollIntoView(item);
        ConnectedAnimation animation =
            ConnectedAnimationService.GetForCurrentView().GetAnimation("portrait");
        if (animation != null)
        {
            await ContactsListView.TryStartConnectedAnimationAsync(
                animation, item, "PortraitEllipse");
        }
    }
}
```

## <a name="coordinated-animation"></a>조정된 애니메이션

![조정된 애니메이션](images/connected-animations/coordinated_example.gif)

<!--
<iframe width=640 height=360 src='https://microsoft.sharepoint.com/portals/hub/_layouts/15/VideoEmbedHost.aspx?chId=552c725c%2De353%2D4118%2Dbd2b%2Dc2d0584c9848&amp;vId=9066bbbe%2Dcf58%2D4ab4%2Db274%2D595616f5d0a0&amp;width=640&amp;height=360&amp;autoPlay=false&amp;showInfo=true' allowfullscreen></iframe>
-->

A *애니메이션 조정* 요소 화면으로 이동 될 때 연결 된 애니메이션 요소를 사용 하 여 동시에 애니메이션 효과 주는 연결 된 애니메이션 대상에 함께 표시 되는 특별 한 유형의 나타내기 애니메이션 합니다. 조정된 애니메이션은 전환에 더 많은 시각 효과를 추가하여 원본 보기와 대상 보기 사이에 공유되는 상황에 대해 사용자의 관심을 더 많이 끌 수 있습니다. 다음 그림에서 항목의 UI는 조정된 애니메이션을 사용하여 애니메이션 효과를 적용합니다.

조정 된 애니메이션을 중력 구성에서는 중력 조정 된 요소와 연결 된 애니메이션 요소에 적용 됩니다. 조정 된 요소는 "급강하" 연결 된 요소와 함께 요소를 실제로 조정 상태를 유지 하도록 합니다.

**TryStart**의 2-매개 변수 오버로드를 사용하여 연결된 애니메이션에 조정된 요소를 추가합니다. 이 예제에서는 "CoverImage" 라는 연결 된 애니메이션 요소를 사용 하 여 동시에 들어가는 "DescriptionRoot" 명명 된 표 형태 레이아웃의 조정 된 애니메이션을 보여 줍니다.

```xaml
<!-- DestinationPage.xaml -->
<Grid>
    <Image x:Name="CoverImage" />
    <Grid x:Name="DescriptionRoot" />
</Grid>
```

```csharp
// DestinationPage.xaml.cs
void OnNavigatedTo(NavigationEventArgs e)
{
    var animationService = ConnectedAnimationService.GetForCurrentView();
    var animation = animationService.GetAnimation("coverImage");

    if (animation != null)
    {
        // Don’t need to capture the return value as we are not scheduling any subsequent
        // animations
        animation.TryStart(CoverImage, new UIElement[] { DescriptionRoot });
     }
}
```

## <a name="dos-and-donts"></a>권장 사항 및 금지 사항

- 원본 페이지와 대상 페이지 간에 요소가 공유되는 페이지 전환에는 연결된 애니메이션을 사용합니다.
- 사용 하 여 [GravityConnectedAnimationConfiguration](/uwp/api/windows.ui.xaml.media.animation.gravityconnectedanimationconfiguration) 앞으로 탐색 합니다.
- 사용 하 여 [DirectConnectedAnimationConfiguration](/uwp/api/windows.ui.xaml.media.animation.directconnectedanimationconfiguration) 다시 탐색 합니다.
- 네트워크 요청 또는 다른 장기 실행 비동기 작업 준비 및 연결 된 애니메이션을 시작 사이 대기 하지 마십시오. 필요한 정보를 미리 로드하여 전환을 미리 실행하거나, 대상 보기에 고해상도 이미지가 로드되는 동안 저해상도 자리 표시자 이미지를 사용해야 할 수도 있습니다.
- 사용 하 여 [SuppressNavigationTransitionInfo](/uwp/api/windows.ui.xaml.media.animation.suppressnavigationtransitioninfo) 에서 전환 애니메이션을 방지 하기 위해를 **프레임** 사용 하는 경우 **ConnectedAnimationService**, 연결 된 애니메이션 이후 기본 탐색 전환을 사용 하 여 동시에 사용할 수 하려는 되지 않습니다. 탐색 전환을 사용하는 방법에 대한 자세한 내용은 [NavigationThemeTransition](/uwp/api/Windows.UI.Xaml.Media.Animation.NavigationThemeTransition)을 참조하세요.

## <a name="related-articles"></a>관련 문서

[ConnectedAnimation](/uwp/api/windows.ui.xaml.media.animation.connectedanimation)

[ConnectedAnimationService](/uwp/api/windows.ui.xaml.media.animation.connectedanimationservice)

[NavigationThemeTransition](/uwp/api/Windows.UI.Xaml.Media.Animation.NavigationThemeTransition)
