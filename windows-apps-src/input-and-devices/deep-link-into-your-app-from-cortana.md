---
Description: 백그라운드 앱 서비스의 딥 링크를 Cortana에 제공하여 특정 상태 또는 컨텍스트에서 앱을 포그라운드로 시작합니다.
title: Cortana에서 백그라운드 앱으로의 딥 링크
ms.assetid: BE811A87-8821-476A-90E4-2E20D37E4043
label: Deep link to a background app
template: detail.hbs
---

# Cortana에서 백그라운드 앱으로의 딥 링크


\[ Windows 10의 UWP 앱에 맞게 업데이트되었습니다. Windows 8.x 문서는 [보관](http://go.microsoft.com/fwlink/p/?linkid=619132)을 참조하세요. \]


**중요 API**

-   [**Windows.ApplicationModel.VoiceCommands**](https://msdn.microsoft.com/library/windows/apps/dn706594)
-   [**VCD(음성 명령 정의) 요소 및 특성 v1.2**](https://msdn.microsoft.com/library/windows/apps/dn706593)

**Cortana**의 백그라운드 앱 서비스에서 딥 링크를 제공하여 특정 상태 또는 컨텍스트에서 앱을 포그라운드로 시작합니다.

> **참고**  
**Cortana**와 백그라운드 앱 서비스는 둘 다 포그라운드 앱이 시작되면 종료됩니다.

여기에 표시("AdventureWorks로 이동")된 대로 딥 링크는 기본적으로 **Cortana** 완료 화면에 표시되지만 다양한 다른 화면에도 딥 링크를 표시할 수 있습니다. 

![Cortana 백그라운드 앱 완료 화면](images/cortana-completion-screen-upcomingtrip-small.png)

**필수 조건: **

이 항목은 [Cortana에서 백그라운드 앱 조작](interact-with-a-background-app-in-cortana.md)을 기반으로 합니다. **Adventure Works**라는 여행 계획 및 관리 앱으로 계속 다양한 **Cortana** 기능을 보여 줍니다.

UWP(유니버설 Windows 플랫폼) 앱을 처음 개발하는 경우 다음 항목을 검토하여 여기서 설명하는 기술에 대해 알아보세요.

-   [첫 번째 앱 만들기](https://msdn.microsoft.com/library/windows/apps/bg124288)
-   이벤트에 대한 자세한 내용은 [이벤트 및 라우트된 이벤트 개요](https://msdn.microsoft.com/library/windows/apps/mt185584)를 참조하세요.

**사용자 환경 지침: **

**Cortana**와 앱을 통합하는 방법에 대한 자세한 내용은 [Cortana 디자인 지침](https://msdn.microsoft.com/library/windows/apps/dn974233)을 참조하고, 유용하고 매력적인 음성 사용 앱 디자인에 도움이 되는 팁은 [음성 디자인 지침](https://msdn.microsoft.com/library/windows/apps/dn596121)을 참조하세요.

## <span id="Overview"> </span> <span id="overview"> </span> <span id="OVERVIEW"> </span>개요


사용자는 다음과 같은 방법으로 **Cortana**를 통해 앱에 액세스할 수 있습니다.

-   포그라운드 앱으로 활성화([Cortana를 통해 음성 명령으로 포그라운드 앱 활성화](launch-a-foreground-app-with-voice-commands-in-cortana.md) 참조)
-   백그라운드 앱 서비스로 특정 기능 노출([Cortana를 통해 음성 명령으로 백그라운드 앱 활성화](launch-a-background-app-with-voice-commands-in-cortana.md) 참조)
-   특정 페이지, 콘텐츠 및 상태 또는 컨텍스트에 대한 딥 링크 설정

여기에서는 딥 링크 설정을 설명합니다.

딥 링크 설정은 Cortana 및 앱 서비스가 전체 기능 앱에 대한 게이트웨이로 작동하거나(사용자가 시작 메뉴를 통해 앱을 시작할 필요가 없음), Cortana를 통해서는 불가능한 앱 내의 더 풍부한 세부 정보 및 기능에 대한 액세스를 제공하는 경우에 유용합니다. 딥 링크 설정은 앱에 대한 유용성과 홍보를 향상시키는 또 다른 방법입니다.

딥 링크를 제공하는 세 가지 방법은 다음과 같습니다.

-   다양한 **Cortana** 화면의 "&lt;앱&gt;으로 이동" 링크
-   다양한 **Cortana** 화면의 콘텐츠 타일에 포함된 링크
-   프로그래밍 방식으로 백그라운드 앱 서비스에서 포그라운드 앱 시작

## <span id="Go_to__app__deep_link"> </span> <span id="go_to__app__deep_link"> </span> <span id="GO_TO__APP__DEEP_LINK"> </span>"&lt;앱&gt;으로 이동" 딥 링크


**Cortana**는 대부분의 화면에서 콘텐츠 카드 아래 "&lt;앱&gt;으로 이동" 딥 링크를 표시합니다.

![Cortana 백그라운드 앱 완료 화면](images/cortana-completion-screen.png)

이 링크에 시작 인수를 제공하여 앱 서비스와 유사한 컨텍스트로 앱을 열 수 있습니다. 시작 인수를 제공하지 않으면 앱이 주 화면으로 시작됩니다.

**AdventureWorks** 샘플의 AdventureWorksVoiceCommandService.cs에 대한 이 예제에서는 모든 일치하는 경로를 검색하고 앱에 딥 링크를 제공하는 SendCompletionMessageForDestination 메서드에 지정된 대상을 전달합니다.

먼저 **Cortana**를 통해 말하고 **Cortana** 캔버스에 표시되는 [**VoiceCommandUserMessage**](https://msdn.microsoft.com/en-us/library/windows/apps/windows.applicationmodel.voicecommands.voicecommandusermessage.aspx)(```userMessage```)를 만듭니다. [
            **VoiceCommandContentTile**](https://msdn.microsoft.com/en-us/library/windows/apps/windows.applicationmodel.voicecommands.voicecommandcontenttile.aspx) 목록 개체가 만들어져 캔버스에 결과 카드의 컬렉션을 표시합니다. 

이러한 두 개체는 [**VoiceCommandResponse**](https://msdn.microsoft.com/library/windows/apps/dn974182) 개체(```response```)의 [CreateResponse](https://msdn.microsoft.com/en-us/library/windows/apps/windows.applicationmodel.voicecommands.voicecommandresponse.createresponse.aspx) 메서드로 전달됩니다. 그런 다음 [**AppLaunchArgument**](https://msdn.microsoft.com/library/windows/apps/dn974183) 속성 값을 음성 명령의 대상 값으로 설정합니다.

마지막으로 [**VoiceCommandServiceConnection**](https://msdn.microsoft.com/library/windows/apps/dn974204)의 [**ReportSuccessAsync**](https://msdn.microsoft.com/library/windows/apps/dn706580) 메서드를 호출합니다.

```csharp
/// <summary>
/// Show details for a single trip, if the trip can be found. 
/// This demonstrates a simple response flow in Cortana.
/// </summary>
/// <param name="destination">The destination specified in the voice command.</param>
private async Task SendCompletionMessageForDestination(string destination)
{
...
    IEnumerable<Model.Trip> trips = store.Trips.Where(p => p.Destination == destination);

    var userMessage = new VoiceCommandUserMessage();
    var destinationsContentTiles = new List<VoiceCommandContentTile>();
...
    var response = VoiceCommandResponse.CreateResponse(userMessage, destinationsContentTiles);

    if (trips.Count() > 0)
    {
        response.AppLaunchArgument = destination;
    }

    await voiceServiceConnection.ReportSuccessAsync(response);
}
```


## <span id="Content_tile_deep_link"> </span> <span id="content_tile_deep_link"> </span> <span id="CONTENT_TILE_DEEP_LINK"> </span>콘텐츠 타일 딥 링크


다양한 **Cortana** 화면에서 콘텐츠 카드에 딥 링크를 추가할 수 있습니다.

![Cortana 백그라운드 앱 넘기기 화면 ](images/cortana-backgroundapp-progress-result.png)

"&lt;앱&gt;으로 이동" 링크처럼 시작 인수를 제공하여 앱 서비스와 유사한 컨텍스트로 앱을 열 수 있습니다. 시작 인수를 제공하지 않으면 콘텐츠 타일이 앱에 연결되지 않습니다.

**AdventureWorks** 샘플의 AdventureWorksVoiceCommandService.cs에 대한 이 예제에서는 모든 일치하는 경로를 검색하고 앱에 딥 링크가 있는 콘텐츠 카드를 제공하는 SendCompletionMessageForDestination 메서드로 지정된 대상을 전달합니다.

먼저 **Cortana**를 통해 말하고 **Cortana** 캔버스에 표시되는 [**VoiceCommandUserMessage**](https://msdn.microsoft.com/en-us/library/windows/apps/windows.applicationmodel.voicecommands.voicecommandusermessage.aspx)(```userMessage```)를 만듭니다. [
            **VoiceCommandContentTile**](https://msdn.microsoft.com/en-us/library/windows/apps/windows.applicationmodel.voicecommands.voicecommandcontenttile.aspx) 목록 개체가 만들어져 캔버스에 결과 카드의 컬렉션을 표시합니다. 

이러한 두 개체는 [**VoiceCommandResponse**](https://msdn.microsoft.com/library/windows/apps/dn974182) 개체(```response```)의 [CreateResponse](https://msdn.microsoft.com/en-us/library/windows/apps/windows.applicationmodel.voicecommands.voicecommandresponse.createresponse.aspx) 메서드로 전달됩니다. 그런 다음 [**AppLaunchArgument**](https://msdn.microsoft.com/library/windows/apps/dn974183) 속성 값을 음성 명령의 대상 값으로 설정합니다.

마지막으로 [**VoiceCommandServiceConnection**](https://msdn.microsoft.com/library/windows/apps/dn974204)의 [**ReportSuccessAsync**](https://msdn.microsoft.com/library/windows/apps/dn706580) 메서드를 호출합니다.
여기서는 [**VoiceCommandServiceConnection**](https://msdn.microsoft.com/library/windows/apps/dn974204) 개체의 [**ReportSuccessAsync**](https://msdn.microsoft.com/library/windows/apps/dn706580) 호출에 사용되는 [**VoiceCommandContentTile**](https://msdn.microsoft.com/library/windows/apps/dn974168) 목록에 서로 다른 [**AppLaunchArgument**](https://msdn.microsoft.com/library/windows/apps/dn974183) 매개 변수 값을 가진 두 개의 콘텐츠 타일을 추가합니다.

```csharp
/// <summary>
/// Show details for a single trip, if the trip can be found. 
/// This demonstrates a simple response flow in Cortana.
/// </summary>
/// <param name="destination">The destination specified in the voice command.</param>
private async Task SendCompletionMessageForDestination(string destination)
{
    // If this operation is expected to take longer than 0.5 seconds, the task must
    // supply a progress response to Cortana before starting the operation, and
    // updates must be provided at least every 5 seconds.
    string loadingTripToDestination = string.Format(
               cortanaResourceMap.GetValue("LoadingTripToDestination", cortanaContext).ValueAsString,
               destination);
    await ShowProgressScreen(loadingTripToDestination);
    Model.TripStore store = new Model.TripStore();
    await store.LoadTrips();

    // Query for the specified trip. 
    // The destination should be in the phrase list. However, there might be  
    // multiple trips to the destination. We pick the first.
    IEnumerable<Model.Trip> trips = store.Trips.Where(p => p.Destination == destination);

    var userMessage = new VoiceCommandUserMessage();
    var destinationsContentTiles = new List<VoiceCommandContentTile>();
    if (trips.Count() == 0)
    {
        string foundNoTripToDestination = string.Format(
               cortanaResourceMap.GetValue("FoundNoTripToDestination", cortanaContext).ValueAsString,
               destination);
        userMessage.DisplayMessage = foundNoTripToDestination;
        userMessage.SpokenMessage = foundNoTripToDestination;
    }
    else
    {
        // Set plural or singular title.
        string message = "";
        if (trips.Count() > 1)
        {
            message = cortanaResourceMap.GetValue("PluralUpcomingTrips", cortanaContext).ValueAsString;
        }
        else
        {
            message = cortanaResourceMap.GetValue("SingularUpcomingTrip", cortanaContext).ValueAsString;
        }
        userMessage.DisplayMessage = message;
        userMessage.SpokenMessage = message;

        // Define a tile for each destination.
        foreach (Model.Trip trip in trips)
        {
            int i = 1;
            
            var destinationTile = new VoiceCommandContentTile();

            destinationTile.ContentTileType = VoiceCommandContentTileType.TitleWith68x68IconAndText;
            destinationTile.Image = await StorageFile.GetFileFromApplicationUriAsync(new Uri("ms-appx:///AdventureWorks.VoiceCommands/Images/GreyTile.png"));

            destinationTile.AppLaunchArgument = trip.Destination;
            destinationTile.Title = trip.Destination;
            if (trip.StartDate != null)
            {
                destinationTile.TextLine1 = trip.StartDate.Value.ToString(dateFormatInfo.LongDatePattern);
            }
            else
            {
                destinationTile.TextLine1 = trip.Destination + " " + i;
            }

            destinationsContentTiles.Add(destinationTile);
            i++;
        }
    }

    var response = VoiceCommandResponse.CreateResponse(userMessage, destinationsContentTiles);

    if (trips.Count() > 0)
    {
        response.AppLaunchArgument = destination;
    }

    await voiceServiceConnection.ReportSuccessAsync(response);
}
```
## <span id="Programmatic_deep_link"> </span> <span id="programmatic_deep_link"> </span> <span id="PROGRAMMATIC_DEEP_LINK"> </span>프로그래밍 방식 딥 링크


또한 시작 인수를 사용해서 앱을 프로그래밍 방식으로 시작하여 앱 서비스와 유사한 컨텍스트로 앱을 열 수도 있습니다. 시작 인수를 제공하지 않으면 앱이 주 화면으로 시작됩니다.

여기서는 [**VoiceCommandServiceConnection**](https://msdn.microsoft.com/library/windows/apps/dn974204) 개체의 [**RequestAppLaunchAsync**](https://msdn.microsoft.com/library/windows/apps/dn706581) 호출에 사용되는 [**VoiceCommandResponse**](https://msdn.microsoft.com/library/windows/apps/dn974182) 개체에 값을 "Las Vegas"로 하여 [**AppLaunchArgument**](https://msdn.microsoft.com/library/windows/apps/dn974183) 매개 변수를 추가합니다.

```CSharp
var userMessage = new VoiceCommandUserMessage();
userMessage.DisplayMessage = "Here are your trips.";
userMessage.SpokenMessage = 
  "You have one trip to Vegas coming up.";

response = VoiceCommandResponse.CreateResponse(userMessage);
response.AppLaunchArgument = “Las Vegas”;
await  VoiceCommandServiceConnection.RequestAppLaunchAsync(response);
```

## <span id="App_manifest"> </span> <span id="app_manifest"> </span> <span id="APP_MANIFEST"> </span>앱 매니페스트


앱에 대해 딥 링크 설정을 사용하려면 앱 프로젝트의 Package.appxmanifest 파일에서 `windows.personalAssistantLaunch` 확장을 선언해야 합니다.

여기서는 **Adventure Works** 앱에 대한 `windows.personalAssistantLaunch` 확장을 선언합니다.

```XML
<Extensions>
  <uap:Extension Category="windows.appService" 
    EntryPoint="AdventureWorks.VoiceCommands.AdventureWorksVoiceCommandService">
    <uap:AppService Name="AdventureWorksVoiceCommandService"/>
  </uap:Extension>
  <uap:Extension Category="windows.personalAssistantLaunch"/> 
</Extensions>
```

## <span id="Protocol_contract"> </span> <span id="protocol_contract"> </span> <span id="PROTOCOL_CONTRACT"> </span>프로토콜 계약


앱은 [**Protocol**](https://msdn.microsoft.com/library/windows/apps/br224693) 계약을 사용하는 URI(Uniform Resource Identifier) 활성화를 통해 포그라운드로 시작됩니다. 앱은 앱의 [**OnActivated**](https://msdn.microsoft.com/library/windows/apps/br242330) 이벤트를 재정의하고 **Protocol**의 **ActivationKind**를 확인해야 합니다. 자세한 내용은 [URI 활성화 처리](https://msdn.microsoft.com/library/windows/apps/mt228339)를 참조하세요.

여기서는 [**ProtocolActivatedEventArgs**](https://msdn.microsoft.com/library/windows/apps/br224742)에서 제공하는 URI를 디코드하여 시작 인수에 액세스합니다. 이 예제에서는 [**Uri**](https://msdn.microsoft.com/library/windows/apps/br224746)가 "windows.personalassistantlaunch:?LaunchContext=Las Vegas"로 설정되어 있습니다.

```CSharp
if (args.Kind == ActivationKind.Protocol)
  {
    var commandArgs = args as ProtocolActivatedEventArgs;
    Windows.Foundation.WwwFormUrlDecoder decoder = 
      new Windows.Foundation.WwwFormUrlDecoder(commandArgs.Uri.Query);
    var destination = decoder.GetFirstValueByName("LaunchContext");

    navigationCommand = new ViewModel.TripVoiceCommand(
      "protocolLaunch",
      "text",
      "destination",
      destination);

    navigationToPageType = typeof(View.TripDetails);

    rootFrame.Navigate(navigationToPageType, navigationCommand);

    // Ensure the current window is active.
    Window.Current.Activate();
  }
```

## <span id="related_topics"> </span>관련 문서


**개발자**
* [Cortana 조작](cortana-interactions.md)
* [**VCD 요소 및 특성 v1.2**](https://msdn.microsoft.com/library/windows/apps/dn706593)

**디자이너**
* [Cortana 디자인 지침](https://msdn.microsoft.com/library/windows/apps/dn974233)
* [음성 디자인 지침](https://msdn.microsoft.com/library/windows/apps/dn596121)

**샘플**
* [Cortana 음성 명령 샘플](http://go.microsoft.com/fwlink/p/?LinkID=619899)
 

 






<!--HONumber=Mar16_HO4-->


