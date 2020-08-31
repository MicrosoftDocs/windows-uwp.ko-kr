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
ms.openlocfilehash: ff252faf4dd49929ec46c2ceaa02f94011e6b225
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89169327"
---
# <a name="connected-animation-for-windows-apps"></a>Windows 앱에 대 한 연결 된 애니메이션

연결된 애니메이션을 사용하면 두 가지 보기 간에 전환되는 동작에 애니메이션 효과를 적용하여 역동적이고 매력적인 탐색 환경을 만들 수 있습니다. 이를 통해 사용자는 컨텍스트를 유지 하 고 뷰 간에 연속성을 제공할 수 있습니다.

연결 된 애니메이션에서 요소는 UI 내용이 변경 되는 동안 두 보기 사이에 "continue"가 표시 되 고, 화면에서 소스 뷰의 위치부터 새 뷰의 대상까지 이동 합니다. 이렇게 하면 보기 간의 공통 콘텐츠를 강조 하 고 전환의 일부로 멋진 및 동적 효과를 만듭니다.

> **중요 한 api**:  [ConnectedAnimation 클래스](/uwp/api/windows.ui.xaml.media.animation.connectedanimation), [ConnectedAnimationService 클래스](/uwp/api/windows.ui.xaml.media.animation.connectedanimationservice)


## <a name="examples"></a>예

<table>
<th align="left">XAML Controls Gallery<th>
<tr>
<td><img src="images/xaml-controls-gallery-app-icon.png" alt="XAML controls gallery" width="168"></img></td>
<td>
    <p><strong style="font-weight: semi-bold">XAML 컨트롤 갤러리</strong> 앱이 설치 되어 있는 경우 여기를 클릭 하 여 <a href="xamlcontrolsgallery:/item/ConnectedAnimation">앱을 열고 동작의 연결 된 애니메이션을 확인</a>하세요.</p>
    <ul>
    <li><a href="https://www.microsoft.com/p/xaml-controls-gallery/9msvh128x2zt">XAML Controls Gallery 앱 가져오기(Microsoft Store)</a></li>
    <li><a href="https://github.com/Microsoft/Xaml-Controls-Gallery">소스 코드 가져오기(GitHub)</a></li>
    </ul>
</td>
</tr>
</table>

이 짧은 비디오에서 앱은 연결 된 애니메이션을 사용 하 여 항목 이미지에 애니메이션 효과를 주기 때문에 다음 페이지의 헤더에 포함 될 수 있습니다. 이 효과는 전환 전체에서 사용자 컨텍스트를 유지 하는 데 도움이 됩니다.

![연결 된 애니메이션](images/connected-animations/example.gif)

<!-- 
<iframe width=640 height=360 src='https://microsoft.sharepoint.com/portals/hub/_layouts/15/VideoEmbedHost.aspx?chId=552c725c%2De353%2D4118%2Dbd2b%2Dc2d0584c9848&amp;vId=b2daa5ee%2Dbe15%2D4503%2Db541%2D1328a6587c36&amp;width=640&amp;height=360&amp;autoPlay=false&amp;showInfo=true' allowfullscreen></iframe>
-->

## <a name="video-summary"></a>동영상 요약

> [!VIDEO https://channel9.msdn.com/Events/Windows/Windows-Developer-Day-Fall-Creators-Update/WinDev005/player]

## <a name="connected-animation-and-the-fluent-design-system"></a>연결 된 애니메이션 및 흐름 디자인 시스템

 Fluent 디자인 시스템을 사용하면 조명, 깊이, 움직임, 재질 및 배율이 통합된 선명한 현대식 UI를 만들 수 있습니다. 연결 된 애니메이션은 응용 프로그램에 동작을 추가 하는 흐름 디자인 시스템 구성 요소입니다. 자세한 내용은 [Fluent Design 개요](/windows/apps/fluent-design-system)를 참조하세요.

## <a name="why-connected-animation"></a>애니메이션을 연결 하는 이유

페이지 간을 탐색할 때 사용자가 탐색 후에 표시 되는 새 콘텐츠와 탐색 시의 용도에 대 한 정보를 이해 하는 것이 중요 합니다. 연결 된 애니메이션은 사용자의 포커스를 두 뷰 간에 공유 되는 내용에 그려 두 뷰 간의 관계를 강조 하는 강력한 시각적 비유를 제공 합니다. 또한 연결 된 애니메이션은 앱의 동작 디자인을 구분 하는 데 도움이 되는 시각적 관심사와 폴란드어를 페이지 탐색에 추가 합니다.

## <a name="when-to-use-connected-animation"></a>연결 된 애니메이션을 사용 하는 경우

연결 된 애니메이션은 일반적으로 페이지를 변경할 때 사용 되지만, UI에서 콘텐츠를 변경 하 고 사용자가 컨텍스트를 유지 관리 하려는 환경에도 적용 될 수 있습니다. 원본 뷰와 대상 뷰 간에 이미지 또는 다른 UI가 공유 될 때마다 [탐색에서 드릴](/uwp/api/Windows.UI.Xaml.Media.Animation.NavigationThemeTransition) 하는 대신 연결 된 애니메이션을 사용 하는 것이 좋습니다.

## <a name="configure-connected-animation"></a>연결 된 애니메이션 구성

> [!IMPORTANT]
> 이 기능을 사용 하려면 앱의 대상 버전이 Windows 10, 버전 1809 ([SDK 17763](https://developer.microsoft.com/windows/downloads/windows-10-sdk)) 이상 이어야 합니다. 이전 Sdk에서는 구성 속성을 사용할 수 없습니다. 적응 코드 또는 조건부 XAML을 사용 하 여 SDK 17763 보다 낮은 최소 버전을 대상으로 지정할 수 있습니다. 자세한 내용은 [버전 적응 앱](../../debug-test-perf/version-adaptive-apps.md)을 참조 하세요.

Windows 10, 버전 1809부터, 연결 된 애니메이션은 앞으로 및 뒤로 페이지 탐색을 위해 특별히 조정 된 애니메이션 구성을 제공 하 여 흐름 디자인을 추가로 구체화 합니다.

ConnectedAnimation에서 구성 속성을 설정 하 여 애니메이션 구성을 지정 합니다. 다음 섹션에서이에 대 한 예를 살펴보겠습니다.

다음 표에서는 사용 가능한 구성을 설명 합니다. 이러한 애니메이션에 적용 되는 동작 원칙에 대 한 자세한 내용은 [방향성 및 중력](index.md)을 참조 하세요.

| [GravityConnectedAnimationConfiguration](/uwp/api/windows.ui.xaml.media.animation.gravityconnectedanimationconfiguration) |
| - |
| 이는 기본 구성 이며 전방 탐색에 권장 됩니다. |
사용자가 앱에서 앞으로 탐색 하는 경우 (A에서 B로) 연결 된 요소는 실제로 "페이지를 끌어올 수 있습니다." 라고 표시 됩니다. 이렇게 하면 요소가 z 공간에서 앞으로 이동 하는 것 처럼 표시 되 고 무게를 길게 유지 하는 효과로 비트를 삭제 합니다. 무게의 효과를 극복 하기 위해 요소는 속도를 향상 하 고 최종 위치로 가속화 합니다. 결과는 "scale and dip" 애니메이션입니다. |

| [DirectConnectedAnimationConfiguration](/uwp/api/windows.ui.xaml.media.animation.directconnectedanimationconfiguration) |
| - |
| 사용자가 앱에서 뒤로 이동 하면 (B에서 A로) 애니메이션이 더 직접적입니다. 연결 된 요소는 감속 3 차원 감속/가속 함수를 사용 하 여 B에서로 변환 됩니다. 이전 visual affordance는 계속 해 서 탐색 흐름의 컨텍스트를 유지 하면서 최대한 빠르게 사용자를 이전 상태로 되돌립니다. |

| [BasicConnectedAnimationConfiguration](/uwp/api/windows.ui.xaml.media.animation.basicconnectedanimationconfiguration) |
| - |
| 이는 Windows 10 버전 1809 ([SDK 17763](https://developer.microsoft.com/windows/downloads/windows-10-sdk)) 이전 버전에서 사용 되는 기본 (및에만 해당) 애니메이션입니다. |

### <a name="connectedanimationservice-configuration"></a>ConnectedAnimationService 구성

[ConnectedAnimationService](/uwp/api/windows.ui.xaml.media.animation.connectedanimationservice) 클래스에는 전체 서비스가 아닌 개별 애니메이션에 적용 되는 두 개의 속성이 있습니다.

- [DefaultDuration](/uwp/api/windows.ui.xaml.media.animation.connectedanimationservice.defaultduration)
- [DefaultEasingFunction](/uwp/api/windows.ui.xaml.media.animation.connectedanimationservice.defaulteasingfunction)

다양 한 효과를 얻기 위해이 표에 설명 된 대로 일부 구성은 ConnectedAnimationService에서 이러한 속성을 무시 하 고 대신 고유한 값을 사용 합니다.

| 구성 | DefaultDuration을 고려 하나요? | DefaultEasingFunction? |
| - | - | - |
| 중력 | 예 | 예* <br/> **A에서 B로의 기본 변환은이 감속/가속 함수를 사용 하지만 "중력 dip"에는 자체 감속/가속 함수가 있습니다.*  |
| 직접 | 예 <br/> *150ms에 대 한 애니메이션을 적용 합니다.*| 예 <br/> *감속 감속/가속 함수를 사용 합니다.* |
| 기본 | 예 | 예 |

## <a name="how-to-implement-connected-animation"></a>연결 된 애니메이션을 구현 하는 방법

연결 된 애니메이션을 설정 하는 과정은 다음 두 단계로 구성 됩니다.

1. 소스 요소가 연결 된 애니메이션에 참여할 시스템을 나타내는 소스 페이지에서 애니메이션 개체를 *준비* 합니다.
1. 대상 페이지에서 애니메이션을 *시작* 하 고 대상 요소에 대 한 참조를 전달 합니다.

원본 페이지에서 탐색할 때 [ConnectedAnimationService](/uwp/api/windows.ui.xaml.media.animation.connectedanimationservice.getforcurrentview) 를 호출 하 여 ConnectedAnimationService의 인스턴스를 가져옵니다. 애니메이션을 준비 하려면이 인스턴스에서 [PrepareToAnimate](/uwp/api/windows.ui.xaml.media.animation.connectedanimationservice.preparetoanimate) 를 호출 하 고, 전환에 사용할 UI 요소 및 고유 키를 전달 합니다. 고유 키를 사용 하 여 나중에 대상 페이지에서 애니메이션을 검색할 수 있습니다.

```csharp
ConnectedAnimationService.GetForCurrentView()
    .PrepareToAnimate("forwardAnimation", SourceImage);
```

탐색이 발생 하면 대상 페이지에서 애니메이션을 시작 합니다. 애니메이션을 시작 하려면 [ConnectedAnimation](/uwp/api/windows.ui.xaml.media.animation.connectedanimation.trystart)를 호출 합니다. 애니메이션을 만들 때 제공 된 고유 키를 사용 하 여 [ConnectedAnimationService](/uwp/api/windows.ui.xaml.media.animation.connectedanimationservice.getanimation) 를 호출 하 여 올바른 애니메이션 인스턴스를 검색할 수 있습니다.

```csharp
ConnectedAnimation animation =
    ConnectedAnimationService.GetForCurrentView().GetAnimation("forwardAnimation");
if (animation != null)
{
    animation.TryStart(DestinationImage);
}
```

### <a name="forward-navigation"></a>전방 탐색

이 예제에서는 ConnectedAnimationService를 사용 하 여 두 페이지 간에 전방 탐색을 위한 전환을 만드는 방법을 보여 줍니다 (Page_A Page_B).

전방 탐색에 권장 되는 애니메이션 구성은 [GravityConnectedAnimationConfiguration](/uwp/api/windows.ui.xaml.media.animation.gravityconnectedanimationconfiguration)입니다. 이것은 기본값 이므로 다른 구성을 지정 하려는 경우가 아니면 [구성](/uwp/api/windows.ui.xaml.media.animation.connectedanimation.configuration) 속성을 설정할 필요가 없습니다.

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

대상 페이지에서 애니메이션을 시작 합니다.

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

뒤로 탐색 (Page_B Page_A)의 경우 동일한 단계를 수행 하지만 원본 및 대상 페이지가 반대로 바뀝니다.

사용자가 다시 탐색할 때 앱이 최대한 빨리 이전 상태로 반환 될 것으로 예상 합니다. 따라서 권장 구성은 [DirectConnectedAnimationConfiguration](/uwp/api/windows.ui.xaml.media.animation.directconnectedanimationconfiguration)입니다. 이 애니메이션은 더 빠르고 직접적 이며 감속 감속/가속을 사용 합니다.

원본 페이지에서 애니메이션을 설정 합니다.

```csharp
// Page_B.xaml.cs

protected override void OnNavigatingFrom(NavigatingCancelEventArgs e)
{
    if (e.NavigationMode == NavigationMode.Back)
    {
        ConnectedAnimation animation = 
            ConnectedAnimationService.GetForCurrentView().PrepareToAnimate("backAnimation", DestinationImage);

        // Use the recommended configuration for back animation.
        animation.Configuration = new DirectConnectedAnimationConfiguration();
    }
}
```

대상 페이지에서 애니메이션을 시작 합니다.

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

애니메이션이 설정 된 시간과 시작 될 때 사이에 소스 요소가 앱에서 다른 UI 위에 고정 된 상태로 표시 됩니다. 이렇게 하면 다른 모든 전환 애니메이션을 동시에 수행할 수 있습니다. 이러한 이유로 인해 소스 요소가 존재 하지 않을 수 있으므로이 두 단계 사이에서 250 밀리초를 초과 하 여 대기 하면 안 됩니다. 애니메이션을 준비 하 고 3 초 이내에 시작 하지 않는 경우 시스템에서 애니메이션을 [삭제 하 고](/uwp/api/windows.ui.xaml.media.animation.connectedanimation.trystart) 모든 후속 호출이 실패 합니다.

## <a name="connected-animation-in-list-and-grid-experiences"></a>목록 및 그리드 환경에서 연결 된 애니메이션

또는 목록 또는 그리드 컨트롤에서 연결 된 애니메이션을 만들려고 하는 경우가 많습니다. [ListView](/uwp/api/windows.ui.xaml.controls.listview) 및 [GridView](/uwp/api/windows.ui.xaml.controls.gridview)( [PrepareConnectedAnimation](/uwp/api/windows.ui.xaml.controls.listviewbase.prepareconnectedanimation) 및 [TryStartConnectedAnimationAsync](/uwp/api/windows.ui.xaml.controls.listviewbase.trystartconnectedanimationasync))의 두 가지 메서드를 사용 하 여이 프로세스를 간소화할 수 있습니다.

예를 들어 데이터 템플릿에 이름이 "PortraitEllipse" 인 요소가 포함 된 **ListView** 가 있다고 가정해 보겠습니다.

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

지정 된 목록 항목에 해당 하는 타원을 사용 하 여 연결 된 애니메이션을 준비 하려면 고유 키, 항목 및 이름 "PortraitEllipse"를 사용 하 여 [PrepareConnectedAnimation](/uwp/api/windows.ui.xaml.controls.listviewbase.prepareconnectedanimation) 메서드를 호출 합니다.

```csharp
void PrepareAnimationWithItem(ContactsItem item)
{
     ContactsListView.PrepareConnectedAnimation("portrait", item, "PortraitEllipse");
}
```

자세히 보기에서 뒤로 탐색 하는 경우와 같이이 요소를 대상으로 사용 하 여 애니메이션을 시작 하려면 [TryStartConnectedAnimationAsync](/uwp/api/windows.ui.xaml.controls.listviewbase.trystartconnectedanimationasync)를 사용 합니다. 방금 ListView에 대 한 데이터 소스를 로드 한 경우 TryStartConnectedAnimationAsync는 해당 항목 컨테이너가 생성 될 때까지 애니메이션을 시작할 때까지 기다립니다.

```csharp
private async void ContactsListView_Loaded(object sender, RoutedEventArgs e)
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

## <a name="coordinated-animation"></a>통합 애니메이션

![통합 애니메이션](images/connected-animations/coordinated_example.gif)

<!--
<iframe width=640 height=360 src='https://microsoft.sharepoint.com/portals/hub/_layouts/15/VideoEmbedHost.aspx?chId=552c725c%2De353%2D4118%2Dbd2b%2Dc2d0584c9848&amp;vId=9066bbbe%2Dcf58%2D4ab4%2Db274%2D595616f5d0a0&amp;width=640&amp;height=360&amp;autoPlay=false&amp;showInfo=true' allowfullscreen></iframe>
-->

*조정 된 애니메이션* 은 연결 된 애니메이션 대상과 함께 요소가 표시 되는 특수 한 형식의 나타내기 애니메이션으로, 화면에서 이동할 때 연결 된 애니메이션 요소와 함께 적용 됩니다. 조정 된 애니메이션은 전환에 시각적 효과를 추가 하 고 원본 및 대상 뷰 간에 공유 되는 컨텍스트에 사용자의 주의를 기울일 수 있습니다. 이러한 이미지에서 항목에 대 한 캡션 UI는 조정 된 애니메이션을 사용 하 여 애니메이션을 적용 합니다.

조정 된 애니메이션이 중력 구성을 사용 하는 경우 연결 된 애니메이션 요소와 조정 된 요소 모두에 중력이 적용 됩니다. 요소가 실제로 조정 되도록 조정 된 요소는 연결 된 요소와 함께 "swoop" 됩니다.

**Trstart** 의 두 매개 변수 오버 로드를 사용 하 여 연결 된 애니메이션에 조정 된 요소를 추가 합니다. 이 예제에서는 "CoverImage" 라는 연결 된 애니메이션 요소와 함께 입력 되는 "설명 루트" 라는 그리드 레이아웃의 통합 된 애니메이션을 보여 줍니다.

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

## <a name="dos-and-donts"></a>Do 및 일과

- 소스 및 대상 페이지 간에 요소를 공유 하는 페이지 전환에서 연결 된 애니메이션을 사용 합니다.
- 전방 탐색에 [GravityConnectedAnimationConfiguration](/uwp/api/windows.ui.xaml.media.animation.gravityconnectedanimationconfiguration) 를 사용 합니다.
- 뒤로 탐색에 [DirectConnectedAnimationConfiguration](/uwp/api/windows.ui.xaml.media.animation.directconnectedanimationconfiguration) 를 사용 합니다.
- 연결 된 애니메이션 준비 및 시작 사이에서 네트워크 요청 또는 다른 장기 실행 비동기 작업을 기다리지 마세요. 미리 전환을 실행 하는 데 필요한 정보를 미리 로드 하거나, 고해상도 이미지가 대상 보기에서 로드 되는 동안 저해상도 자리 표시자 이미지를 사용 해야 할 수도 있습니다.
- [SuppressNavigationTransitionInfo](/uwp/api/windows.ui.xaml.media.animation.suppressnavigationtransitioninfo) 를 사용 하 여 **ConnectedAnimationService**를 사용 하는 경우 **프레임** 의 전환 애니메이션을 방지 합니다. 연결 된 애니메이션은 기본 탐색 전환과 함께 동시에 사용 되지 않기 때문입니다. 탐색 전환을 사용 하는 방법에 대 한 자세한 내용은 [NavigationThemeTransition](/uwp/api/Windows.UI.Xaml.Media.Animation.NavigationThemeTransition) 를 참조 하세요.

## <a name="related-articles"></a>관련된 문서

[ConnectedAnimation](/uwp/api/windows.ui.xaml.media.animation.connectedanimation)

[ConnectedAnimationService](/uwp/api/windows.ui.xaml.media.animation.connectedanimationservice)

[NavigationThemeTransition](/uwp/api/Windows.UI.Xaml.Media.Animation.NavigationThemeTransition)