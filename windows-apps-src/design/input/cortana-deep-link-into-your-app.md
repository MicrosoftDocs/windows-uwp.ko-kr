---
title: Cortana의 백그라운드 앱에서 전경 앱으로의 딥 링크-Cortana UWP 디자인 및 개발
description: 앱을 특정 상태나 컨텍스트에서 포그라운드로 시작 하는 **Cortana** 의 백그라운드 앱에서 심층 링크를 제공 합니다.
ms.assetid: 6fe5fcc5-9ee4-4c04-92f4-7b1bf7ef5651
ms.date: 01/28/2021
ms.topic: article
keywords: 나
ms.openlocfilehash: 5d0f841ed653adac6770089a9ec9c1be0335e346
ms.sourcegitcommit: d7efd35c1749f695aebbc0db99d8b62b70fb72da
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 01/29/2021
ms.locfileid: "99057883"
---
# <a name="deep-link-from-a-background-app-in-cortana-to-a-foreground-app"></a>Cortana의 백그라운드 앱에서 포그라운드 앱으로의 딥 링크

>[!WARNING]
> 이 기능은 Windows 10 5 월 2020 업데이트 (버전 2004, 코드명 "20H1")에서 더 이상 지원 되지 않습니다.

앱을 특정 상태나 컨텍스트에서 포그라운드로 시작 하는 **Cortana** 의 백그라운드 앱에서 심층 링크를 제공 합니다.

> [!NOTE]
> **Cortana** 와 백그라운드 앱 서비스는 모두 포그라운드 앱이 시작 될 때 종료 됩니다.

여기에 표시 된 것 처럼 전체 링크는 **Cortana** 완료 화면에 기본적으로 표시 됩니다 ("AdventureWorks로 이동"). 하지만 여러 다른 화면에 딥 링크를 표시할 수 있습니다.

:::image type="content" source="images/cortana/cortana-completion-screen-upcomingtrip-small.png" alt-text="향후 여행에 대 한 Cortana 백그라운드 앱 완성의 스크린샷":::

> [!NOTE]
> **중요 API**
>
> - [**VoiceCommands**](/uwp/api/Windows.ApplicationModel.VoiceCommands)
> - [**VCD 요소 및 특성 v 1.2**](/uwp/schemas/voicecommands/voice-command-elements-and-attributes-1-2)

## <a name="overview"></a>개요

사용자는 다음을 통해 **Cortana** 를 통해 앱에 액세스할 수 있습니다.

- 전경 앱으로 활성화 ( [Cortana를 통해 음성 명령을 사용 하 여 전경 앱 활성화](cortana-launch-a-foreground-app-with-voice-commands.md))를 참조 하세요.
- 백그라운드 앱 서비스로 특정 기능 노출 ( [음성 명령을 사용 하 여 Cortana에서 백그라운드 앱 활성화](cortana-launch-a-background-app-with-voice-commands.md)참조)
- 특정 페이지, 콘텐츠 및 상태나 컨텍스트에 대 한 딥 링크

여기에서 딥 링크에 대해 설명 합니다.

딥 링크는 Cortana와 app service가 시작 메뉴를 통해 앱을 시작 하도록 요구 하는 대신, 전체 기능을 갖춘 앱에 대 한 게이트웨이로 작동 하거나 Cortana를 통해 불가능 한 앱 내에서 다양 한 세부 정보 및 기능에 대 한 액세스를 제공 하는 경우에 유용 합니다. 심층 링크는 유용성을 높이고 앱을 홍보 하는 또 다른 방법입니다.

심층 링크를 제공 하는 방법에는 세 가지가 있습니다.

- &lt; &gt; 다양 한 **Cortana** 화면에서 "앱으로 이동" 링크
- 다양 한 **Cortana** 화면에서 콘텐츠 타일에 포함 된 링크입니다.
- 백그라운드 앱 서비스에서 프로그래밍 방식으로 포그라운드 앱 시작

## <a name="go-to-ltappgt-deep-link"></a>"앱으로 &lt; 이동 &gt; " 딥 링크

**Cortana** 는 &lt; &gt; 대부분의 화면에서 콘텐츠 카드 아래에 "앱으로 이동" 딥 링크를 표시 합니다.

:::image type="content" source="images/cortana/cortana-completion-screen.png" alt-text="백그라운드 앱 완료 화면에서 Cortana의 ' 앱으로 이동 ' 딥 링크에 대 한 스크린샷":::

앱 서비스와 유사한 컨텍스트에서 앱을 여는이 링크에 대 한 시작 인수를 제공할 수 있습니다. 시작 인수를 제공 하지 않으면 앱이 주 화면으로 시작 됩니다.

**AdventureWorks** 샘플 AdventureWorksVoiceCommandService.cs의이 예제에서는 지정 된 destination ( `destination` ) 문자열을 Send messagefordestination 메서드에 전달 합니다 .이 메서드는 일치 하는 모든 여행을 검색 하 고 앱에 대 한 딥 링크를 제공 합니다.

먼저 Cortana에서 말한 [**VoiceCommandUserMessage**](/uwp/api/Windows.ApplicationModel.VoiceCommands.VoiceCommandUserMessage) ()를 만들고 ```userMessage``` **cortana** 캔버스에  표시 합니다. 그런 다음 캔버스에 결과 카드의 컬렉션을 표시 하기 위해 [**VoiceCommandContentTile**](/uwp/api/Windows.ApplicationModel.VoiceCommands.VoiceCommandContentTile) list 개체가 생성 됩니다.

그런 다음이 두 개체는 [**VoiceCommandResponse**](/uwp/api/Windows.ApplicationModel.VoiceCommands.VoiceCommandResponse) 개체의 [CreateResponse](/uwp/api/Windows.ApplicationModel.VoiceCommands.VoiceCommandResponse) 메서드에 전달 됩니다 ( `response` ). 그런 다음 응답 개체의 [**AppLaunchArgument**](/uwp/api/Windows.ApplicationModel.VoiceCommands.VoiceCommandResponse) 속성 값을 `destination` 이 함수에 대 한 passsed 값으로 설정 합니다. 사용자가 Cortana 캔버스에서 콘텐츠 타일을 탭 하면 매개 변수 값이 응답 개체를 통해 앱에 전달 됩니다.

마지막으로 [**VoiceCommandServiceConnection**](/uwp/api/Windows.ApplicationModel.VoiceCommands.VoiceCommandServiceConnection)의 [**ReportSuccessAsync**](/uwp/api/Windows.ApplicationModel.VoiceCommands.VoiceCommandServiceConnection) 메서드를 호출 합니다.

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

## <a name="content-tile-deep-link"></a>콘텐츠 타일 딥 링크

다양 한 **Cortana** 화면에서 콘텐츠 카드에 딥 링크를 추가할 수 있습니다.

:::image type="content" source="images/cortana/cortana-backgroundapp-progress-result.png" alt-text="종단 간 cortana 캔버스의 스크린샷 스크린샷을 사용 하 여 향후 전달":::*adventureworks "예정 된 여행" 여행 화면*

"앱으로 이동 &lt; &gt; " 링크와 마찬가지로 시작 인수를 제공 하 여 앱 서비스와 비슷한 컨텍스트를 사용 하 여 앱을 열 수 있습니다. 시작 인수를 제공 하지 않으면 콘텐츠 타일이 앱에 연결 되지 않습니다.

**AdventureWorks** 샘플 AdventureWorksVoiceCommandService.cs의이 예제에서는 지정 된 대상을 Send Messagefordestination 메서드에 전달 합니다 .이 메서드는 일치 하는 모든 여행을 검색 하 고 앱에 대 한 딥 링크를 포함 하는 콘텐츠 카드를 제공 합니다.

먼저 Cortana에서 말한 [**VoiceCommandUserMessage**](/uwp/api/Windows.ApplicationModel.VoiceCommands.VoiceCommandUserMessage) ()를 만들고 ```userMessage``` **cortana** 캔버스에  표시 합니다. 그런 다음 캔버스에 결과 카드의 컬렉션을 표시 하기 위해 [**VoiceCommandContentTile**](/uwp/api/Windows.ApplicationModel.VoiceCommands.VoiceCommandContentTile) list 개체가 생성 됩니다. 

그런 다음이 두 개체는 [**VoiceCommandResponse**](/uwp/api/Windows.ApplicationModel.VoiceCommands.VoiceCommandResponse) 개체의 [CreateResponse](/uwp/api/Windows.ApplicationModel.VoiceCommands.VoiceCommandResponse) 메서드에 전달 됩니다 ( ```response``` ). 그런 다음 [**AppLaunchArgument**](/uwp/api/Windows.ApplicationModel.VoiceCommands.VoiceCommandResponse) 속성 값을 음성 명령의 대상 값으로 설정 합니다.

마지막으로 [**VoiceCommandServiceConnection**](/uwp/api/Windows.ApplicationModel.VoiceCommands.VoiceCommandServiceConnection)의 [**ReportSuccessAsync**](/uwp/api/Windows.ApplicationModel.VoiceCommands.VoiceCommandServiceConnection) 메서드를 호출 합니다.
여기서는 다른 [**AppLaunchArgument**](/uwp/api/Windows.ApplicationModel.VoiceCommands.VoiceCommandResponse) 매개 변수 값을 가진 두 개의 콘텐츠 타일을 [**VoiceCommandServiceConnection**](/uwp/api/Windows.ApplicationModel.VoiceCommands.VoiceCommandServiceConnection) 개체의 [**ReportSuccessAsync**](/uwp/api/Windows.ApplicationModel.VoiceCommands.VoiceCommandServiceConnection) 호출에 사용 되는 [**VoiceCommandContentTile**](/uwp/api/Windows.ApplicationModel.VoiceCommands.VoiceCommandContentTile) 목록에 추가 합니다.

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

## <a name="programmatic-deep-link"></a>프로그래밍 심층 링크

시작 인수를 사용 하 여 프로그래밍 방식으로 앱을 시작 하 여 앱 서비스와 비슷한 컨텍스트를 사용 하 여 앱을 열 수도 있습니다. 시작 인수를 제공 하지 않으면 앱이 주 화면으로 시작 됩니다.

여기서는 [**RequestAppLaunchAsync**](/uwp/api/Windows.ApplicationModel.VoiceCommands.VoiceCommandServiceConnection) 개체의 [**VoiceCommandServiceConnection**](/uwp/api/Windows.ApplicationModel.VoiceCommands.VoiceCommandServiceConnection) 호출에 사용 되는 [**VoiceCommandResponse**](/uwp/api/Windows.ApplicationModel.VoiceCommands.VoiceCommandResponse) 개체에 값이 "Las Vegas" 인 [**AppLaunchArgument**](/uwp/api/Windows.ApplicationModel.VoiceCommands.VoiceCommandResponse) 매개 변수를 추가 합니다.

```CSharp
var userMessage = new VoiceCommandUserMessage();
userMessage.DisplayMessage = "Here are your trips.";
userMessage.SpokenMessage = 
  "You have one trip to Vegas coming up.";

response = VoiceCommandResponse.CreateResponse(userMessage);
response.AppLaunchArgument = “Las Vegas”;
await  VoiceCommandServiceConnection.RequestAppLaunchAsync(response);
```

## <a name="app-manifest"></a>앱 매니페스트

앱에 대 한 심층 링크를 사용 하도록 설정 하려면 `windows.personalAssistantLaunch` 앱 프로젝트의 appxmanifest.xml 파일에서 확장을 선언 해야 합니다.

여기서는 `windows.personalAssistantLaunch` **놀이 Works** 앱에 대 한 확장을 선언 합니다.

```XML
<Extensions>
  <uap:Extension Category="windows.appService" 
    EntryPoint="AdventureWorks.VoiceCommands.AdventureWorksVoiceCommandService">
    <uap:AppService Name="AdventureWorksVoiceCommandService"/>
  </uap:Extension>
  <uap:Extension Category="windows.personalAssistantLaunch"/> 
</Extensions>
```

## <a name="protocol-contract"></a>프로토콜 계약

응용 프로그램은 [**프로토콜**](/uwp/api/Windows.ApplicationModel.Activation.ActivationKind) 계약을 사용 하 여 URI (Uniform resource Identifier) 활성화를 통해 포그라운드로 시작 됩니다. 앱은 앱의 [**Onactivated**](/uwp/api/Windows.UI.Xaml.Application) 된 이벤트를 재정의 하 고 **ActivationKind** **프로토콜** 을 확인 해야 합니다. 자세한 내용은 [URI 활성화 처리](/windows/uwp/launch-resume/handle-uri-activation)를 참조 하세요.

여기서는 [**ProtocolActivatedEventArgs**](/uwp/api/Windows.ApplicationModel.Activation.ProtocolActivatedEventArgs) 에서 제공 하는 URI를 디코딩하여 시작 인수에 액세스 합니다. 이 예제에서 [**Uri**](/uwp/api/Windows.ApplicationModel.Activation.ProtocolActivatedEventArgs) 는 "personalassistantlaunch:?로 설정 됩니다. LaunchContext = Las Vegas "입니다.

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

## <a name="related-articles"></a>관련된 문서

- [Windows 앱에서 Cortana 상호 작용](cortana-interactions.md)
- [Cortana 디자인 지침](cortana-design-guidelines.md)
- [VCD 요소 및 특성 v 1.2](/uwp/schemas/voicecommands/voice-command-elements-and-attributes-1-2)
- [Cortana 음성 명령 샘플](https://go.microsoft.com/fwlink/p/?LinkID=619899)
