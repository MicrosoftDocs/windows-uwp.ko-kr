---
Description: Chaseable 타일 알림을 사용 하 여 사용자가 앱을 클릭 했을 때 라이브 타일에 표시 되는 앱을 확인 합니다.
title: 추적 가능한 타일 알림
ms.assetid: E9AB7156-A29E-4ED7-B286-DA4A6E683638
label: Chaseable tile notifications
template: detail.hbs
ms.date: 06/13/2017
ms.topic: article
keywords: windows 10, uwp, chaseable 타일, 라이브 타일, chaseable 타일 알림
ms.localizationpriority: medium
ms.openlocfilehash: 951dc891fb34ae4be7551c08ff47eabc19ae9eb6
ms.sourcegitcommit: c5df8832e9df8749d0c3eee9e85f4c2d04f8b27b
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/15/2020
ms.locfileid: "92100281"
---
# <a name="chaseable-tile-notifications"></a>추적 가능한 타일 알림

Chaseable 타일 알림을 사용 하면 사용자가 타일을 클릭 했을 때 앱의 라이브 타일이 표시 하는 타일 알림을 확인할 수 있습니다.  
예를 들어, 뉴스 앱은이 기능을 사용 하 여 사용자가 시작할 때 해당 라이브 타일이 표시 되는 뉴스 스토리를 확인할 수 있습니다. 사용자가 찾을 수 있도록 스토리가 두드러지게 표시 되도록 할 수 있습니다. 

> [!IMPORTANT]
> **기념일 업데이트 필요**: c #, c + + 또는 VB 기반 UWP 앱에서 chaseable 타일 알림을 사용 하려면 SDK 14393를 대상으로 하 고 빌드 14393 이상을 실행 해야 합니다. JavaScript 기반 UWP 앱의 경우 SDK 17134을 대상으로 하 고 빌드 17134 이상을 실행 해야 합니다. 


> **중요 한 api**: [LaunchActivatedEventArgs. TileActivatedInfo 속성](/uwp/api/windows.applicationmodel.activation.launchactivatedeventargs.TileActivatedInfo), [TileActivatedInfo 클래스](/uwp/api/windows.applicationmodel.activation.tileactivatedinfo)


## <a name="how-it-works"></a>작동 방법

Chaseable 타일 알림을 사용 하도록 설정 하려면 알림 메시지 페이로드의 시작 속성과 비슷한 타일 알림 페이로드의 **Arguments** 속성을 사용 하 여 타일 알림의 콘텐츠에 대 한 정보를 포함 합니다.

라이브 타일을 통해 앱을 시작 하면 시스템은 현재/최근 표시 된 타일 알림에서 인수 목록을 반환 합니다.


## <a name="when-to-use-chaseable-tile-notifications"></a>Chaseable 타일 알림을 사용 하는 경우

Chaseable 타일 알림은 라이브 타일에서 알림 큐를 사용 하는 경우 일반적으로 사용 됩니다. 즉, 최대 5 개의 서로 다른 알림을 순환 하 고 있음을 의미 합니다. 라이브 타일의 콘텐츠가 앱의 최신 콘텐츠와 동기화 되지 않을 수 있는 경우에도 유용 합니다. 예를 들어, 뉴스 앱은 30 분 마다 라이브 타일을 새로 고치 며, 앱이 시작 될 때 마지막 폴링 간격에서 타일에 있던 항목을 포함 하지 않을 수 있는 최신 뉴스를 로드 합니다. 이 경우 사용자는 라이브 타일에서 본 스토리를 찾지 못할 수 있습니다. 타일에서 사용자가 확인 하는 항목을 쉽게 검색할 수 있도록 하 여 chaseable 타일 알림이 도움이 될 수 있습니다.

## <a name="what-to-do-with-a-chaseable-tile-notifications"></a>Chaseable 타일 알림과 관련 하 여 수행할 작업

가장 중요 한 사항은 대부분의 시나리오에서 사용자가이를 클릭 했을 때 타일에 있던 **특정 알림으로 직접 이동** 해서는 안 된다는 것입니다. 라이브 타일은 응용 프로그램에 대 한 진입점으로 사용 됩니다. 사용자가 라이브 타일을 클릭 하면 (1) 앱을 정상적으로 시작 하려고 하거나 (2) 라이브 타일에 있던 특정 알림에 대 한 자세한 정보를 확인 하려는 경우 두 가지 시나리오가 있을 수 있습니다. 사용자가 원하는 동작을 명시적으로 말할 수 있는 방법은 없으므로 **사용자가 확인 한 알림이 쉽게 검색 가능 하도록 하는 동시에 앱을 정상적으로 시작**하는 것이 좋습니다.

예를 들어 MSN News 앱의 라이브 타일을 클릭 하면 앱이 정상적으로 시작 됩니다. 홈 페이지 또는 사용자가 이전에 읽은 모든 문서를 표시 합니다. 그러나 홈 페이지에서 앱은 라이브 타일의 스토리를 쉽게 검색할 수 있도록 합니다. 이러한 방식으로 두 시나리오를 모두 지원 합니다. 단순히 앱을 시작/재개 하려는 시나리오와 특정 스토리를 보려는 시나리오입니다.


## <a name="how-to-include-the-arguments-property-in-your-tile-notification-payload"></a>타일 알림 페이로드에 Arguments 속성을 포함 하는 방법

알림 페이로드의 arguments 속성을 사용 하면 앱에서 나중에 알림을 식별 하는 데 사용할 수 있는 데이터를 제공할 수 있습니다. 예를 들어 인수에 스토리 id가 포함 되어 있을 수 있으므로 시작할 때 스토리를 검색 하 고 표시할 수 있습니다. 속성은 문자열을 허용 합니다 .이 문자열은 쿼리 문자열, JSON 등으로 serialize 할 수 있지만 일반적으로는 간단 하 고 XML로 인코딩됩니다. 쿼리 문자열 형식은 일반적으로 권장 됩니다.

**TileVisual** 및 **TileBinding** 요소 모두에 속성을 설정할 수 있으며이 속성은 하위로 설정 됩니다. 모든 타일 크기에서 동일한 인수를 원하는 경우 **TileVisual**에서 인수를 설정 하면 됩니다. 특정 타일 크기에 대 한 특정 인수가 필요한 경우 개별 **TileBinding** 요소에 대 한 인수를 설정할 수 있습니다.

이 예에서는 알림을 나중에 확인할 수 있도록 arguments 속성을 사용 하는 알림 페이로드를 만듭니다. 

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


## <a name="how-to-check-for-the-arguments-property-when-your-app-launches"></a>앱이 시작 될 때 arguments 속성을 확인 하는 방법

대부분의 앱에는 [Onlaunched](/uwp/api/windows.ui.xaml.application#Windows_UI_Xaml_Application_OnLaunched_Windows_ApplicationModel_Activation_LaunchActivatedEventArgs_) 된 메서드에 대 한 재정의가 포함 된 App.xaml.cs 파일이 있습니다. 이름에서 알 수 있듯이 앱이 시작 될 때이 메서드를 호출 합니다. 단일 인수인 [LaunchActivatedEventArgs](/uwp/api/windows.applicationmodel.activation.launchactivatedeventargs) 개체를 사용 합니다.

LaunchActivatedEventArgs 개체에는 chaseable 알림을 사용 하도록 설정 하는 속성이 있습니다. [TileActivatedInfo 속성](/uwp/api/windows.applicationmodel.activation.launchactivatedeventargs.TileActivatedInfo)은 [TileActivatedInfo 개체](/uwp/api/windows.applicationmodel.activation.tileactivatedinfo)에 대 한 액세스를 제공 합니다. 사용자가 앱 목록, 검색 또는 다른 진입점이 아닌 타일에서 앱을 시작 하면 앱이이 속성을 초기화 합니다.

[TileActivatedInfo 개체](/uwp/api/windows.applicationmodel.activation.tileactivatedinfo) 에는 지난 15 분 이내에 타일에 표시 된 알림 목록을 포함 하는 [RecentlyShownNotifications](/uwp/api/windows.applicationmodel.activation.tileactivatedinfo.RecentlyShownNotifications)라는 속성이 포함 되어 있습니다. 목록의 첫 번째 항목은 현재 타일에 있는 알림을 나타내고 후속 항목은 현재 사용자가 이전에 보았던 알림을 나타냅니다. 타일이 지워진 경우이 목록은 비어 있습니다.

각 ShownTileNotification에는 Arguments 속성이 있습니다. Arguments 속성은 타일 알림 페이로드의 인수 문자열을 사용 하 여 초기화 되거나 페이로드에 인수 문자열이 포함 되지 않은 경우 null입니다.

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


### <a name="accessing-onlaunched-from-desktop-applications"></a>데스크톱 응용 프로그램에서 OnLaunched 액세스

데스크톱 [브리지](https://developer.microsoft.com/windows/bridges/desktop)를 사용 하는 데스크톱 응용 프로그램 (예: WPF 등)은 chaseable 타일을 사용할 수 있습니다. 유일한 차이점은 OnLaunched 된 인수에 액세스 하는 것입니다. 먼저 [데스크톱 브리지를 사용](/windows/msix/desktop/source-code-overview)하 여 앱을 패키지 해야 합니다.

> [!IMPORTANT]
> **10 월 2018 업데이트 필요**: API를 사용 하려면 `AppInstance.GetActivatedEventArgs()` SDK 17763를 대상으로 하 고 빌드 17763 이상을 실행 해야 합니다.

데스크톱 응용 프로그램의 경우 시작 인수에 액세스 하려면 다음을 수행 합니다.

```csharp

static void Main()
{
    Application.EnableVisualStyles();
    Application.SetCompatibleTextRenderingDefault(false);

    // API only available on build 17763 or higher
    var args = AppInstance.GetActivatedEventArgs();
    switch (args.Kind)
    {
        case ActivationKind.Launch:

            var launchArgs = args as LaunchActivatedEventArgs;

            // If clicked on from tile
            if (launchArgs.TileActivatedInfo != null)
            {
                // If tile notification(s) were present
                if (launchArgs.TileActivatedInfo.RecentlyShownNotifications.Count > 0)
                {
                    // Get arguments from the notifications that were recently displayed
                    string[] allTileArgs = launchArgs.TileActivatedInfo.RecentlyShownNotifications
                    .Select(i => i.Arguments)
                    .ToArray();
     
                    // TODO: Highlight each story in the app
                }
            }
    
            break;
```


## <a name="raw-xml-example"></a>원시 XML 예제

알림 라이브러리 대신 원시 XML을 사용 하는 경우 XML은 다음과 같습니다.

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



## <a name="related-articles"></a>관련된 문서

- [LaunchActivatedEventArgs TileActivatedInfo 속성](/uwp/api/windows.applicationmodel.activation.launchactivatedeventargs#Windows_ApplicationModel_Activation_LaunchActivatedEventArgs_TileActivatedInfo_)
- [TileActivatedInfo 클래스](/uwp/api/windows.applicationmodel.activation.tileactivatedinfo)