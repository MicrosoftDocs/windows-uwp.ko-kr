---
author: mijacobs
description: 연결된 애니메이션을 사용하면 두 가지 보기 간에 전환되는 동작에 애니메이션 효과를 적용하여 역동적이고 매력적인 탐색 환경을 만들 수 있습니다.
title: 연결된 애니메이션
template: detail.hbs
ms.author: jimwalk
ms.date: 10/25/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
pm-contact: stmoy
design-contact: conrwi
doc-status: Published
ms.localizationpriority: medium
ms.openlocfilehash: a789a8f082192b79b3e96990827f9a4f6a0eacbc
ms.sourcegitcommit: 4f6dc806229a8226894c55ceb6d6eab391ec8ab6
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 09/20/2018
ms.locfileid: "4083058"
---
# <a name="connected-animation-for-uwp-apps"></a>UWP 앱에 대한 연결된 애니메이션

## <a name="what-is-connected-animation"></a>연결된 애니메이션이란 무엇인가요?

연결된 애니메이션을 사용하면 두 가지 보기 간에 전환되는 동작에 애니메이션 효과를 적용하여 역동적이고 매력적인 탐색 환경을 만들 수 있습니다. 이렇게 하면 사용자가 컨텍스트를 유지하는 데 도움이 될 뿐 아니라 보기 간에 연속성이 보장됩니다.
연결된 애니메이션에서는 UI 콘텐츠가 바뀌는 동안 요소가 두 보기 간에 "계속 있는" 것처럼 보이고, 원본 보기의 원래 위치에서 새 보기의 대상 위치로 화면을 가로질러 날아갑니다. 이렇게 하면 보기 간의 공통 콘텐츠가 강조되며 전환 시 아름답고 역동적인 효과를 얻을 수 있습니다.

## <a name="see-it-in-action"></a>실제 장면 보기

이 짧은 동영상에 나오는 앱에서는 항목 이미지가 "계속해서" 다음 페이지의 머리글이 되도록 연결된 애니메이션을 사용하여 항목 이미지에 애니메이션 효과를 줍니다. 이 효과는 전환 시 사용자 컨텍스트를 유지하는 데 도움이 됩니다.

![지속적인 움직임의 UI 예](images/continuous3.gif)

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

## <a name="how-to-implement"></a>구현 방법

연결된 애니메이션을 설정하려면 다음 두 단계를 거쳐야 합니다.

1.  연결된 애니메이션에서 원본 요소가 참여하는 시스템을 나타내는 원본 페이지의 애니메이션 개체를 *준비*합니다. 
2.  대상 페이지에서 애니메이션을 *시작*하고 대상 요소에 참조를 전달합니다.

이러한 두 단계 사이에서 원본 요소는 앱의 다른 UI 위에 고정되어 표시되므로 다른 전환 애니메이션을 동시에 수행할 수 있습니다. 이러한 이유로, 원본 요소의 존재가 방해 요소로 작동할 수 있으므로 두 단계 사이에 기다리는 시간이 250밀리초를 넘으면 안 됩니다. 애니메이션을 준비하고 3초 이내에 시작하지 않으면 시스템에서 애니메이션을 삭제하고 후속 **TryStart** 호출이 실패합니다.

**ConnectedAnimationService.GetForCurrentView**를 호출하여 현재 ConnectedAnimationService 인스턴스에 액세스할 수 있습니다. 애니메이션을 준비하려면 이 인스턴스에서 **PrepareToAnimate**를 호출하고, 전환에 사용할 고유 키 및 UI 요소 참조를 전달합니다. 고유 키를 사용하여 나중에 애니메이션을 검색할 수 있습니다(예: 다른 페이지에서 검색).

```csharp
ConnectedAnimationService.GetForCurrentView().PrepareToAnimate("image", SourceImage);
```

애니메이션을 시작하려면 **ConnectedAnimation.TryStart**를 호출합니다. 애니메이션을 만들 때 제공한 고유 키로 **ConnectedAnimationService.GetAnimation**을 호출하여 올바른 애니메이션 인스턴스를 검색할 수 있습니다.

```csharp
ConnectedAnimation imageAnimation = 
    ConnectedAnimationService.GetForCurrentView().GetAnimation("image");
if (imageAnimation != null)
{
    imageAnimation.TryStart(DestinationImage);
}
```

다음은 ConnectedAnimationService를 사용하여 두 페이지 간에 전환을 만드는 완전한 예제입니다.

*SourcePage.xaml*

```xaml
<Image x:Name="SourceImage"
       Width="200"
       Height="200"
       Stretch="Fill"
       Source="Assets/StoreLogo.png" />
```

*SourcePage.xaml.cs*

```csharp
private void NavigateToDestinationPage()
{
    ConnectedAnimationService.GetForCurrentView().PrepareToAnimate("image", SourceImage);
    Frame.Navigate(typeof(DestinationPage));
}
```

*DestinationPage.xaml*

```xaml
<Image x:Name="DestinationImage"
       Width="400"
       Height="400"
       Stretch="Fill"
       Source="Assets/StoreLogo.png" />
```

*DestinationPage.xaml.cs*

```csharp
protected override void OnNavigatedTo(NavigationEventArgs e)
{
    base.OnNavigatedTo(e);

    ConnectedAnimation imageAnimation = 
        ConnectedAnimationService.GetForCurrentView().GetAnimation("image");
    if (imageAnimation != null)
    {
        imageAnimation.TryStart(DestinationImage);
    }
}
```

## <a name="connected-animation-in-list-and-grid-experiences"></a>목록 및 그리드 환경의 연결된 애니메이션

목록 또는 그리드 컨트롤에서/로 연결된 애니메이션을 만들고 싶은 경우가 종종 있을 것입니다. 새로운 두 가지 메서드 **ListView** 및 **GridView**와 **PrepareConnectedAnimation** 및 **TryStartConnectedAnimationAsync**를 사용하여 이 프로세스를 간소화할 수 있습니다.
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

지정된 목록 항목에 해당하는 타원을 사용하여 연결된 애니메이션을 준비하려면 고유 키, 해당 항목 및 이름 “PortraitEllipse”를 사용하여 **PrepareConnectedAnimation** 메서드를 호출합니다.

```csharp
void PrepareAnimationWithItem(ContactsItem item)
{
     ContactsListView.PrepareConnectedAnimation("portrait", item, "PortraitEllipse");
}
```

또는 세부 정보 보기에서 뒤로 탐색하는 경우처럼 이 요소를 대상으로 사용하여 애니메이션을 시작하려면 **TryStartConnectedAnimationAsync**를 사용합니다. **ListView**에 대한 데이터 원본을 방금 로드한 경우 **TryStartConnectedAnimationAsync**는 해당 항목 컨테이너가 생성될 때까지 기다렸다가 애니메이션을 시작합니다.

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

*조정된 애니메이션*은 연결된 애니메이션 대상과 함께 요소가 표시되는 특수한 종류의 입구 애니메이션으로, 화면을 넘어갈 때 연결된 애니메이션 요소와 함께 애니메이션 효과를 줍니다. 조정된 애니메이션은 전환에 더 많은 시각 효과를 추가하여 원본 보기와 대상 보기 사이에 공유되는 상황에 대해 사용자의 관심을 더 많이 끌 수 있습니다. 다음 그림에서 항목의 UI는 조정된 애니메이션을 사용하여 애니메이션 효과를 적용합니다.

**TryStart**의 2-매개 변수 오버로드를 사용하여 연결된 애니메이션에 조정된 요소를 추가합니다. 이 예에서는 "CoverImage"라는 연결된 애니메이션 요소와 동시에 입장하는 "DescriptionRoot"라는 그리드 레이아웃이의 조정된 애니메이션을 보여 줍니다.

*DestinationPage.xaml*

```xaml
<Grid>
    <Image x:Name="CoverImage" />
    <Grid x:Name="DescriptionRoot" />
</Grid>
```

*DestinationPage.xaml.cs*

```csharp
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
- 연결된 애니메이션을 준비하고 시작하는 사이에 네트워크 요청 또는 오래 실행되는 다른 비동기 작업을 기다리지 마세요. 필요한 정보를 미리 로드하여 전환을 미리 실행하거나, 대상 보기에 고해상도 이미지가 로드되는 동안 저해상도 자리 표시자 이미지를 사용해야 할 수도 있습니다.
- 연결된 애니메이션은 기본 탐색 전환과 동시에 사용할 수 없으므로 **ConnectedAnimationService**를 사용하는 경우 [SuppressNavigationTransitionInfo](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.xaml.media.animation.suppressnavigationtransitioninfo)를 사용하여 **프레임**에서 전환 애니메이션을 차단해야 합니다. 탐색 전환을 사용하는 방법에 대한 자세한 내용은 [NavigationThemeTransition](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.media.animation.navigationthemetransition.aspx)을 참조하세요.


## <a name="download-the-code-samples"></a>코드 샘플 다운로드

[WindowsUIDevLabs](https://github.com/Microsoft/WindowsUIDevLabs) 샘플 갤러리의 [연결된 애니메이션 샘플](https://github.com/Microsoft/WindowsUIDevLabs/tree/master/SampleGallery/Samples/SDK%2014393/ConnectedAnimationSample)을 참조하세요.

## <a name="related-articles"></a>관련 문서

- [ConnectedAnimation](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.media.animation.connectedanimation)
- [ConnectedAnimationService](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.xaml.media.animation.connectedanimation.aspx)
- [NavigationThemeTransition](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.media.animation.navigationthemetransition.aspx)
