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
ms.openlocfilehash: ce639faac66e93b65a398e6d9cdc700546fc68ab
ms.sourcegitcommit: d2517e522cacc5240f7dffd5bc1eaa278e3f7768
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/30/2018
ms.locfileid: "8332787"
---
# <a name="connected-animation-for-uwp-apps"></a>UWP 앱에 대한 연결된 애니메이션

연결된 애니메이션을 사용하면 두 가지 보기 간에 전환되는 동작에 애니메이션 효과를 적용하여 역동적이고 매력적인 탐색 환경을 만들 수 있습니다. 이렇게 하면 사용자가 컨텍스트를 유지하는 데 도움이 될 뿐 아니라 보기 간에 연속성이 보장됩니다.

연결된 된 애니메이션에서 요소가 UI 콘텐츠를 새 보기의 대상 원본 보기의 위치에서 화면을 가로질러 비행 변화 하는 동안 두 보기 간에 "계속"를 표시 합니다. 이 보기 간의 공통 콘텐츠가 강조 하 고 전환의 일부로 아름 답 고 역동적인 효과 만듭니다.

> **중요 Api**: [ConnectedAnimation 클래스](/uwp/api/windows.ui.xaml.media.animation.connectedanimation), [ConnectedAnimationService 클래스](/uwp/api/windows.ui.xaml.media.animation.connectedanimationservice)

## <a name="see-it-in-action"></a>실제 장면 보기

이 짧은 동영상 앱 "계속 해 서" 다음 페이지의 머리글이 되도록 항목 이미지에 애니메이션 효과 연결된 된 애니메이션을 사용 합니다. 이 효과는 전환 시 사용자 컨텍스트를 유지하는 데 도움이 됩니다.

![연결된 애니메이션](images/connected-animations/example.gif)

<!-- 
<iframe width=640 height=360 src='https://microsoft.sharepoint.com/portals/hub/_layouts/15/VideoEmbedHost.aspx?chId=552c725c%2De353%2D4118%2Dbd2b%2Dc2d0584c9848&amp;vId=b2daa5ee%2Dbe15%2D4503%2Db541%2D1328a6587c36&amp;width=640&amp;height=360&amp;autoPlay=false&amp;showInfo=true' allowfullscreen></iframe>
-->

## <a name="video-summary"></a>비디오 요약

> [!VIDEO https://channel9.msdn.com/Events/Windows/Windows-Developer-Day-Fall-Creators-Update/WinDev005/player]

## <a name="connected-animation-and-the-fluent-design-system"></a>연결된 애니메이션 및 Fluent 디자인 시스템

 Fluent 디자인 시스템을 사용하면 조명, 깊이, 움직임, 재질 및 배율이 통합된 선명한 현대식 UI를 만들 수 있습니다. 연결된 애니메이션은 앱에 동작을 추가하는 Fluent 디자인 시스템 구성 요소입니다. 자세히 알아보려면 [UWP용 Fluent 디자인 개요](../fluent-design-system/index.md)를 확인하십시오.

## <a name="why-connected-animation"></a>연결된 애니메이션을 사용하는 이유

페이지를 탐색할 때 사용자는 탐색 후 어떤 새 콘텐츠가 제공되며 탐색할 때 해당 콘텐츠가 사용자의 의도와 어떤 관계가 있는지 이해할 수 있어야 합니다. 연결된 애니메이션은 두 보기 간에 공유되는 콘텐츠에 사용자의 포커스를 그려서 두 보기 간의 관계를 강조함으로써 강력한 시각적 비유를 제공합니다. 뿐만 아니라 연결된 애니메이션은 페이지 탐색에 앱의 동작 디자인을 차별화하는 시각적 흥미와 세련미를 더해 줍니다.

## <a name="when-to-use-connected-animation"></a>연결된 애니메이션을 사용하는 시기

연결된 애니메이션은 UI의 콘텐츠를 변경하고 사용자로 하여금 컨텍스트를 유지하게 하려는 모든 환경에 적용할 수 있지만 일반적으로 페이지를 변경할 때 사용됩니다. 원본 보기와 대상 보기 사이에 공유되는 이미지 또는 기타 UI 부분이 있을 때에는 [드릴인 탐색 전환](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.media.animation.navigationthemetransition.aspx) 대신 연결된 애니메이션을 사용하는 방안을 고려해야 합니다.

## <a name="configure-connected-animation"></a>연결 된 애니메이션을 구성 합니다.

> [!IMPORTANT]
> 이 기능을 사용 하려면 앱의 대상 버전 Windows 10, 버전 1809 ([SDK 17763](https://developer.microsoft.com/windows/downloads/windows-10-sdk)) 것 이상입니다. 구성 속성을 이전 Sdk에서 사용할 수 없는 경우 17763 SDK 최소 버전을 대상으로 지정할 수 적응 코드나 조건부 XAML을 사용 하 여 합니다. 자세한 내용은 [버전 적응 앱](/debug-test-perf/version-adaptive-apps)을 참조 하세요.

Windows 10, 버전 1809부터 연결 된 애니메이션 추가 구체화 Fluent 디자인 애니메이션을 제공 하 여 구성을 맞춤식 앞으로 및 뒤로 탐색 페이지.

ConnectedAnimation에서 구성 속성을 설정 하 여 애니메이션 구성을 지정 합니다. (살펴보겠습니다이 예제는 다음 섹션에서.)

이 표에 사용 가능한 구성 되어 있습니다. 이러한 애니메이션에 적용 된 움직임 원칙에 대 한 자세한 내용은 [방향 및 무게를](index.md)참조 하세요.

| [GravityConnectedAnimationConfiguration]() |
| - |
| 기본 구성 되며 앞으로 탐색에 대 한 것이 좋습니다. |
사용자가 앱 (A)에서 앞으로 이동, 연결 된 요소를 물리적으로 "페이지" 표시 됩니다. 그렇게 요소 z-공간에서 앞으로 이동할 나타나고 보류 라인 무게의 효과로 약간 삭제 합니다. 무게의 효과 해결 하려면 요소 속도 획득 하 고 최종 위치에 가속화 합니다. 결과 "배율 및 dip" 애니메이션입니다. |

| [DirectConnectedAnimationConfiguration]() |
| - |
| 사용자가 앱 (B A로)에서 뒤로 탐색, 애니메이션 직접적입니다. 연결 된 요소를 사용 하 여 직접적 입방 형 3 차원 감속/가속 함수에서 B 선형으로 변환 합니다. 뒤로 시각적 어포던스 반환 사용자를 이전 상태로 화면은 가능한 한 빠르게 탐색 흐름의 컨텍스트를 유지 하면서 합니다. |

| [BasicConnectedAnimationConfiguration]() |
| - |
| 이것이 기본값 (및만) Windows 10, 버전 1809 ([SDK 17763](https://developer.microsoft.com/windows/downloads/windows-10-sdk)) 이전 버전에서 사용 되는 애니메이션. |

### <a name="connectedanimationservice-configuration"></a>ConnectedAnimationService 구성

[ConnectedAnimationService](/uwp/api/windows.ui.xaml.media.animation.connectedanimationservice) 클래스에 전체 서비스 보다는 개별 애니메이션을 적용할 수 있는 두 가지 속성이 있습니다.

- [DefaultDuration](/uwp/api/windows.ui.xaml.media.animation.connectedanimationservice.defaultduration)
- [DefaultEasingFunction](/uwp/api/windows.ui.xaml.media.animation.connectedanimationservice.defaulteasingfunction)

다양 한 효과 구현 하려면 일부 구성 ConnectedAnimationService에서 이러한 속성을 무시 하 고이 테이블에 설명 된 대로 대신 고유한 값을 사용 합니다.

| 구성 | 측면 DefaultDuration? | 측면 DefaultEasingFunction? |
| - | - | - |
| 무게 | 예 | 예* <br/> **B A에서 기본 번역이 감속/가속 함수를 사용 하 여 되었으나 "무게 dip" 자체 감속/가속 함수입니다.*  |
| 직접 | 아니요 <br/> *150ms 넘는 애니메이션을 적용 합니다.*| 아니요 <br/> *감속/가속 함수 감속을 사용 합니다.* |
| 기본 | 예 | 예 |

## <a name="how-to-implement-connected-animation"></a>연결 된 애니메이션을 구현 하는 방법

연결된 애니메이션을 설정하려면 다음 두 단계를 거쳐야 합니다.

1. *준비* 원본 요소는 연결 된 애니메이션에 참여 하는 시스템을 나타내는 원본 페이지의 애니메이션 개체입니다.
1. *시작* 대상 페이지에 있는 애니메이션 대상 요소에 대 한 참조를 전달 합니다.

원본 페이지를 탐색할 때 [ConnectedAnimationService.GetForCurrentView](/uwp/api/windows.ui.xaml.media.animation.connectedanimationservice.getforcurrentview) ConnectedAnimationService의 인스턴스를 호출 합니다. 애니메이션을 준비 하려면이 인스턴스에서 [PrepareToAnimate](/uwp/api/windows.ui.xaml.media.animation.connectedanimationservice.preparetoanimate) 를 호출 하 고 고유 키 및 전환에 사용 하려는 UI 요소에 전달 합니다. 고유 키를 사용 하면 애니메이션 대상 페이지에서 나중에 검색할 수 있습니다.

```csharp
ConnectedAnimationService.GetForCurrentView()
    .PrepareToAnimate("forwardAnimation", SourceImage);
```

탐색 발생할 때 대상 페이지에서 애니메이션을 시작 합니다. 애니메이션을 시작하려면 [ConnectedAnimation.TryStart](/uwp/api/windows.ui.xaml.media.animation.connectedanimation.trystart)를 호출합니다. 애니메이션을 만들 때 제공한 고유 키로 [ConnectedAnimationService.GetAnimation](/uwp/api/windows.ui.xaml.media.animation.connectedanimationservice.getanimation)을 호출하여 올바른 애니메이션 인스턴스를 검색할 수 있습니다.

```csharp
ConnectedAnimation animation =
    ConnectedAnimationService.GetForCurrentView().GetAnimation("forwardAnimation");
if (animation != null)
{
    animation.TryStart(DestinationImage);
}
```

### <a name="forward-navigation"></a>앞으로 탐색

이 예제에서는 앞으로 탐색 (Page_B Page_A) 두 페이지 간에 전환을 만드는 데 ConnectedAnimationService를 사용 하는 방법을 보여 줍니다.

앞으로 탐색에 대 한 권장된 애니메이션 구성은 [GravityConnectedAnimationConfiguration입니다](). 이것이 기본값, 하므로 다른 구성을 지정 하려는 경우가 아니면 [구성](/uwp/api/windows.ui.xaml.media.animation.connectedanimation.configuration) 속성을 설정할 필요는 없습니다.

원본 페이지의 애니메이션을 설정 합니다.

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

동일한 단계를 따를 뒤로 탐색 (Page_A Page_B), 하지만 원본 및 대상 페이지 취소 됩니다.

사용자가 뒤로 이동 하면 이전 상태로 반환 되는 가능한 한 빨리 앱을 기대 합니다. 따라서 권장 구성은 [DirectConnectedAnimationConfiguration입니다](). 이 애니메이션 빠르게, 더 직접적 이며 감속/가속 감속을 사용 하 여.

원본 페이지의 애니메이션을 설정 합니다.

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

시간이 애니메이션 설정 되 고 시작 될 때 원본 요소는 앱의 다른 UI 위에 고정 나타납니다. 이렇게 하면 다른 전환 애니메이션을 동시에 수행할 수 있습니다. 이러한 이유로 원본 요소의 존재가 방해 요소로 작동할 수 있으므로 두 단계 사이 이상 시간이 250 밀리초를 기다립니다 해서는 안 됩니다. 애니메이션을 준비하고 3초 이내에 시작하지 않으면 시스템에서 애니메이션을 삭제하고 후속 [TryStart](/uwp/api/windows.ui.xaml.media.animation.connectedanimation.trystart) 호출이 실패합니다.

## <a name="connected-animation-in-list-and-grid-experiences"></a>목록 및 그리드 환경의 연결된 애니메이션

목록 또는 그리드 컨트롤에서/로 연결된 애니메이션을 만들고 싶은 경우가 종종 있을 것입니다. [ListView](/uwp/api/windows.ui.xaml.controls.listview) 및 [GridView](/uwp/api/windows.ui.xaml.controls.gridview), [PrepareConnectedAnimation](/uwp/api/windows.ui.xaml.controls.listviewbase.prepareconnectedanimation) [TryStartConnectedAnimationAsync](/uwp/api/windows.ui.xaml.controls.listviewbase.trystartconnectedanimationasync)이 프로세스를 간소화 하기 위해 두 가지 메서드를 사용할 수 있습니다.

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

지정 된 목록 항목에 해당 하는 타원을 사용 하 여 연결된 된 애니메이션을 준비 하려면 고유 키, 해당 항목 및 이름 "PortraitEllipse"를 사용 하 여 [PrepareConnectedAnimation](/uwp/api/windows.ui.xaml.controls.listviewbase.prepareconnectedanimation) 메서드를 호출 합니다.

```csharp
void PrepareAnimationWithItem(ContactsItem item)
{
     ContactsListView.PrepareConnectedAnimation("portrait", item, "PortraitEllipse");
}
```

세부 정보 보기에서 뒤로 탐색 때 등의 대상으로이 요소를 사용 하 여 애니메이션을 시작 하려면 [TryStartConnectedAnimationAsync](/uwp/api/windows.ui.xaml.controls.listviewbase.trystartconnectedanimationasync)를 사용 합니다. ListView에 대한 데이터 원본을 방금 로드한 경우 TryStartConnectedAnimationAsync는 해당 항목 컨테이너가 생성될 때까지 기다렸다가 애니메이션을 시작합니다.

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

*조정 된 애니메이션* 은 화면을 넘어갈 때 연결 된 애니메이션 요소와 함께에서 애니메이션을 적용 하는 연결 된 애니메이션 대상이 함께 요소가 표시는 특수 한 종류의 입구 애니메이션. 조정된 애니메이션은 전환에 더 많은 시각 효과를 추가하여 원본 보기와 대상 보기 사이에 공유되는 상황에 대해 사용자의 관심을 더 많이 끌 수 있습니다. 다음 그림에서 항목의 UI는 조정된 애니메이션을 사용하여 애니메이션 효과를 적용합니다.

조정 된 애니메이션을 중력 구성에서는 중력 연결 된 애니메이션 요소와 조정 된 요소에 적용 됩니다. 조정 된 요소는 "급강하" 연결 된 요소와 함께 해야만 요소 진정으로 조정 된 유지 합니다.

**TryStart**의 2-매개 변수 오버로드를 사용하여 연결된 애니메이션에 조정된 요소를 추가합니다. 이 예제에서는 "CoverImage" 라는 연결 된 애니메이션 요소와 동시에 입력 하는 "DescriptionRoot" 라는 그리드 레이아웃이의 조정 된 애니메이션을 보여 줍니다.

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
- 앞으로 탐색을 위해 [GravityConnectedAnimationConfiguration]() 를 사용 합니다.
- 뒤로 탐색에 대 한 [DirectConnectedAnimationConfiguration]() 를 사용 합니다.
- 네트워크 요청 또는 다른 장기 실행 비동기 작업을 기다리지 준비 하 고 연결된 된 애니메이션을 시작에서 대기 하지 마세요. 필요한 정보를 미리 로드하여 전환을 미리 실행하거나, 대상 보기에 고해상도 이미지가 로드되는 동안 저해상도 자리 표시자 이미지를 사용해야 할 수도 있습니다.
- [SuppressNavigationTransitionInfo](/uwp/api/windows.ui.xaml.media.animation.suppressnavigationtransitioninfo) 를 사용 하 여 연결 된 애니메이션 기본 탐색을 사용 하 여 동시에 사용 하도록 설계 되지 이후 **ConnectedAnimationService**사용 하는 경우 **프레임** 에서 전환 애니메이션을 차단 합니다. 전환 합니다. 탐색 전환을 사용하는 방법에 대한 자세한 내용은 [NavigationThemeTransition](/uwp/api/Windows.UI.Xaml.Media.Animation.NavigationThemeTransition)을 참조하세요.

## <a name="download-the-code-samples"></a>코드 샘플 다운로드

[WindowsUIDevLabs](https://github.com/Microsoft/WindowsUIDevLabs) 샘플 갤러리의 [연결된 애니메이션 샘플](https://github.com/Microsoft/WindowsUIDevLabs/tree/master/SampleGallery/Samples/SDK%2014393/ConnectedAnimationSample)을 참조하세요.

## <a name="related-articles"></a>관련 문서

[ConnectedAnimation](/uwp/api/windows.ui.xaml.media.animation.connectedanimation)

[ConnectedAnimationService](/uwp/api/windows.ui.xaml.media.animation.connectedanimationservice)

[NavigationThemeTransition](/uwp/api/Windows.UI.Xaml.Media.Animation.NavigationThemeTransition)
