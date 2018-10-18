---
author: andrewleader
Description: Use chaseable tile notifications to find out what your app displayed on its Live Tile when the user clicked it.
title: 추적형 타일 알림
ms.assetid: E9AB7156-A29E-4ED7-B286-DA4A6E683638
label: Chaseable tile notifications
template: detail.hbs
ms.author: mijacobs
ms.date: 06/13/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp, 추적형 타일, 라이브 타일, 추적형 타일 알림
ms.localizationpriority: medium
ms.openlocfilehash: b6d86d8881e0027a28f0f2a737e5f3fcb46a6ab5
ms.sourcegitcommit: e16c9845b52d5bd43fc02bbe92296a9682d96926
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/18/2018
ms.locfileid: "5013136"
---
# <a name="chaseable-tile-notifications"></a>추적형 타일 알림

추적형 타일 알림을 사용하면 사용자가 타일을 클릭했을 때 앱의 라이브 타일이 어떤 타일 알림을 표시하고 있었는지 파악할 수 있습니다.  
예를 들어 뉴스 앱은 이 기능을 사용하여 사용자가 앱을 시작했을 때 라이브 타일이 어떤 뉴스 스토리를 표시했는지 확인할 수 있습니다. 해당 스토리를 눈에 띄게 표시해 사용자가 쉽게 찾도록 할 수 있습니다. 

> [!IMPORTANT]
> **1주년 업데이트 필요**: C#, C++, VB 기반 UWP 앱과 함께 추적형 타일 알림을 사용하려면 SDK 14393을 대상으로 하고 빌드 14393 이상을 실행하고 있어야 합니다. JavaScript 기반 UWP 앱의 경우 SDK 17134를 타겟팅하고 빌드 17134 이상을 실행해야 합니다. 


> **중요한 API**: [LaunchActivatedEventArgs.TileActivatedInfo 속성](https://docs.microsoft.com/uwp/api/windows.applicationmodel.activation.launchactivatedeventargs.TileActivatedInfo), [TileActivatedInfo 클래스](https://docs.microsoft.com/uwp/api/windows.applicationmodel.activation.tileactivatedinfo)


## <a name="how-it-works"></a>작동 방식

추적형 타일 알림을 사용하려면 알림 메시지 페이로드의 실행 속성과 비슷하게 타일 알림 페이로드의 **Arguments** 속성을 사용하여 타일 알림의 콘텐츠에 대한 정보를 포함시킵니다.

앱이 라이브 타일을 통해 실행되면 시스템은 현재/최근에 표시된 타일 알림에서 인수 목록을 반환합니다.


## <a name="when-to-use-chaseable-tile-notifications"></a>추적형 타일 알림을 사용해야 하는 경우

추적형 타일 알림은 일반적으로 라이브 타일에서 알림 큐를 사용할 때 사용됩니다(즉, 최대 5개의 알림 순환 사용 가능). 또한 라이브 타일의 콘텐츠가 앱의 최신 콘텐츠와 동기화되지 않을 때 유용합니다. 예를 들어, 뉴스 앱은 30분마다 라이브 타일을 새로 고칩니다. 그러나 앱이 실행되면 최신 뉴스를 로드합니다(마지막 폴링 간격 이후 타일에 있었던 항목은 포함되지 않을 수 있음). 이 경우 사용자는 라이브 타일에서 본 스토리를 찾을 수 없다는 사실에 좌절감을 느낄 수 있습니다. 이럴 때 사용자가 타일에서 본 내용을 쉽게 찾을 수 있다는 점에서 추적형 타일 알림이 도움이 될 수 있습니다.

## <a name="what-to-do-with-a-chaseable-tile-notifications"></a>추적형 타일 알림으로 수행해야 하는 작업

무엇보다 대부분의 시나리오에서 사용자가 클릭했을 때 타일에 있었던 **특정 알림으로 직접 이동해서는 안 된다**는 점을 유념해야 합니다. 라이브 타일은 응용 프로그램 진입점으로 사용됩니다. 사용자가 라이브 타일을 클릭하는 시나리오는 크게 두 가지입니다. (1) 평소대로 앱을 실행하려고 하거나, (2) 라이브 타일에 있던 특정 알림에 대한 추가 정보를 보고 싶은 경우입니다. 사용자가 원하는 동작을 명시적으로 말하도록 유도할 수는 없기 때문에, **앱을 정상적으로 실행하면서 사용자가 본 알림을 쉽게 찾을 수 있도록** 하는 방법이 가장 좋습니다.

예를 들어 MSN 뉴스 앱의 라이브 타일을 클릭하면 앱이 정상적으로 실행되고, 홈 페이지 또는 사용자가 이전에 읽고 있었던 기사가 표시됩니다. 그러나 홈 페이지에서는 앱에서 라이브 타일의 스토리를 쉽게 찾을 수 있습니다. 이렇게 하면 단순히 앱을 실행/다시 시작하려는 시나리오와 특정 스토리를 보려는 두 가지 시나리오가 모두 지원됩니다.


## <a name="how-to-include-the-arguments-property-in-your-tile-notification-payload"></a>타일 알림 페이로드에 Arguments 속성을 포함시키는 방법

알림 페이로드에서 인수 속성을 사용하면 나중에 알림을 식별하는 데 사용할 수 있는 데이터를 앱이 제공하도록 할 수 있습니다. 예를 들어, 인수에 스토리의 ID를 포함하면 실행할 때 스토리를 검색하고 표시할 수 있습니다. 이 속성은 원하는 방식으로 직렬화할 수 있는 문자열(쿼리 문자열, JSON 등)을 수락하지만, 일반적으로 쿼리 문자열 형식을 권장합니다. 가볍고 XML 인코딩이 원활하기 때문입니다.

이 속성은 **TileVisual** 및 **TileBinding** 요소 모두에서 설정할 수 있으며 아래로 계단식 배열됩니다. 모든 타일 크기에 대해 동일한 인수를 원한다면 **TileVisual**에서 인수를 설정하기만 하면 됩니다. 특정 타일 크기에 대해 특정 인수가 필요한 경우 개별 **TileBinding** 요소에서 인수를 설정할 수 있습니다.

이 예는 인수 속성을 사용하는 알림 페이로드를 만들어 나중에 알림을 식별할 수 있도록 합니다. 

```csharp
// Uses the following NuGet packages
// - Microsoft.Toolkit.Uwp.Notifications
// - QueryString.NET
 
TileContent content = new TileContent()
{
    Visual = new TileVisual()
    {
        // These arguments cascade down to Medium and Wide
        Arguments = new QueryString()
        {
            { "action", "storyClicked" },
            { "story", "201c9b1" }
        }.ToString(),
 
 
        // Medium tile
        TileMedium = new TileBinding()
        {
            Content = new TileBindingContentAdaptive()
            {
                // Omitted
            }
        },
 
 
        // Wide tile is same as Medium
        TileWide = new TileBinding() { /* Omitted */ },
 
 
        // Large tile is an aggregate of multiple stories
        // and therefore needs different arguments
        TileLarge = new TileBinding()
        {
            Arguments = new QueryString()
            {
                { "action", "storiesClicked" },
                { "story", "43f939ag" },
                { "story", "201c9b1" },
                { "story", "d9481ca" }
            }.ToString(),
 
            Content = new TileBindingContentAdaptive() { /* Omitted */ }
        }
    }
};
```


## <a name="how-to-check-for-the-arguments-property-when-your-app-launches"></a>앱이 실행될 때 인수 속성을 확인하는 방법

대부분의 앱에는 [OnLaunched](https://docs.microsoft.com/uwp/api/windows.ui.xaml.application#Windows_UI_Xaml_Application_OnLaunched_Windows_ApplicationModel_Activation_LaunchActivatedEventArgs_) 메서드에 대한 재정의가 포함된 App.xaml.cs 파일이 있습니다. 이름에서 알 수 있듯이 앱이 실행되면 이 메서드가 호출됩니다. [LaunchActivatedEventArgs](https://docs.microsoft.com/uwp/api/windows.applicationmodel.activation.launchactivatedeventargs) 객체라는 단일 인수를 사용합니다.

LaunchActivatedEventArgs 개체에는 추적형 알림을 활성화하는 [TileActivatedInfo 속성](https://docs.microsoft.com/uwp/api/windows.applicationmodel.activation.launchactivatedeventargs.TileActivatedInfo)이 있습니다. 이 속성은 [TileActivatedInfo 개체](https://docs.microsoft.com/uwp/api/windows.applicationmodel.activation.tileactivatedinfo)에 대한 액세스를 제공합니다. 사용자가 앱 목록, 검색 또는 기타 진입점 대신 타일에서 앱을 시작하면 앱이 이 속성을 초기화합니다.

[TileActivatedInfo 개체](https://docs.microsoft.com/uwp/api/windows.applicationmodel.activation.tileactivatedinfo)에는 마지막 15분 내에 타일에 표시되었던 알림 목록이 수록된 [RecentlyShownNotifications](https://docs.microsoft.com/uwp/api/windows.applicationmodel.activation.tileactivatedinfo.RecentlyShownNotifications)라는 속성이 포함되어 있습니다. 목록의 첫 번째 항목은 현재 타일에 있는 알림을 나타내며, 후속 항목은 사용자가 현재 알림 이전에 본 알림을 나타냅니다. 타일이 삭제된 경우 이 목록은 비어 있습니다.

각 ShownTileNotification에 인수 속성이 있습니다. 인숙 속성은 타일 알림 페이로드의 인수 문자열로 초기화되거나 페이로드에 인수 문자열이 포함되지 않은 경우 null로 초기화됩니다.

```csharp
protected override void OnLaunched(LaunchActivatedEventArgs args)
{
    // If the API is present (doesn't exist on 10240 and 10586)
    if (ApiInformation.IsPropertyPresent(typeof(LaunchActivatedEventArgs).FullName, nameof(LaunchActivatedEventArgs.TileActivatedInfo)))
    {
        // If clicked on from tile
        if (args.TileActivatedInfo != null)
        {
            // If tile notification(s) were present
            if (args.TileActivatedInfo.RecentlyShownNotifications.Count > 0)
            {
                // Get arguments from the notifications that were recently displayed
                string[] allArgs = args.TileActivatedInfo.RecentlyShownNotifications
                .Select(i => i.Arguments)
                .ToArray();
 
                // TODO: Highlight each story in the app
            }
        }
    }
 
    // TODO: Initialize app
}
```


## <a name="raw-xml-example"></a>원시 XML의 예

알림 라이브러리 대신 원시 XML을 사용하는 경우 여기에 XML이 있습니다.

```xml
<tile>
  <visual arguments="action=storyClicked&amp;story=201c9b1">
 
    <binding template="TileMedium">
       
      <text>Kitten learns how to drive a car...</text>
      ... (omitted)
     
    </binding>
 
    <binding template="TileWide">
      ... (same as Medium)
    </binding>
     
    <!--Large tile is an aggregate of multiple stories-->
    <binding
      template="TileLarge"
      arguments="action=storiesClicked&amp;story=43f939ag&amp;story=201c9b1&amp;story=d9481ca">
   
      <text>Can your dog understand what you're saying?</text>
      ... (another story)
      ... (one more story)
   
    </binding>
 
  </visual>
</tile>
```



## <a name="related-articles"></a>관련 문서

- [LaunchActivatedEventArgs.TileActivatedInfo 속성](https://docs.microsoft.com/uwp/api/windows.applicationmodel.activation.launchactivatedeventargs#Windows_ApplicationModel_Activation_LaunchActivatedEventArgs_TileActivatedInfo_)
- [TileActivatedInfo 클래스](https://docs.microsoft.com/uwp/api/windows.applicationmodel.activation.tileactivatedinfo)