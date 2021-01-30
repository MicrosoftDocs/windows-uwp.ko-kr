---
title: Cortana에서 백그라운드 앱과 상호 작용-Cortana UWP 디자인 및 개발
description: 음성 명령을 실행 하는 동안 Cortana 캔버스의 음성 및 텍스트 입력을 통해 백그라운드 앱과의 사용자 상호 작용을 사용 하도록 설정 합니다.
ms.assetid: e42917dc-aece-4880-813f-80b897f9126c
ms.date: 01/28/2021
ms.topic: article
keywords: 나
ms.openlocfilehash: 111f945c548d34f614305e31c1c79ce9197f0925
ms.sourcegitcommit: d7efd35c1749f695aebbc0db99d8b62b70fb72da
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 01/29/2021
ms.locfileid: "99057796"
---
# <a name="interact-with-a-background-app-in-cortana"></a>Cortana에서 백그라운드 앱 조작

>[!WARNING]
> 이 기능은 Windows 10 5 월 2020 업데이트 (버전 2004, 코드명 "20H1")에서 더 이상 지원 되지 않습니다.

음성 명령을 실행 하는 동안 **Cortana** 캔버스의 음성 및 텍스트 입력을 통해 백그라운드 앱과의 사용자 상호 작용을 사용 하도록 설정 합니다.

> [!NOTE]
> **중요 API**
>
> - [**VoiceCommands**](/uwp/api/Windows.ApplicationModel.VoiceCommands)
> - [**VCD 요소 및 특성 v 1.2**](/uwp/schemas/voicecommands/voice-command-elements-and-attributes-1-2)

Cortana는 앱과 함께 전체 단계별 워크플로를 지원 합니다. 이 워크플로는 앱에서 정의 되며 다음과 같은 기능을 지원할 수 있습니다. 

- 완료되었습니다.
- 백오프
- 진행률
- 확인
- 명확성
- 오류

## <a name="composing-feedback-strings"></a>사용자 의견 문자열 작성

> [!TIP]
> **전제 조건**
>
> UWP (유니버설 Windows 플랫폼) 앱을 처음 개발 하는 경우 여기에 설명 된 기술에 익숙해질 수 있도록 다음 항목을 참조 하세요.
>
> - [첫 번째 앱 만들기](/windows/uwp/get-started/your-first-app)
> - [이벤트 및 라우트된 이벤트](/windows/uwp/xaml-platform/events-and-routed-events-overview) 를 사용 하 여 이벤트에 대해 알아보기 개요
>
> **사용자 환경 지침**
>
> Cortana 및 [음성 상호 작용](speech-interactions.md) **을 사용 하** 여 앱을 통합 하는 방법에 대 한 정보는 [cortana 디자인 지침](cortana-design-guidelines.md) 을 참조 하세요.

**Cortana** 에서 표시 하 고 음성으로 표시 하는 피드백 문자열을 작성 합니다.

[Cortana 디자인 지침은](cortana-design-guidelines.md) **cortana** 에 대 한 문자열 작성에 대 한 권장 사항을 제공 합니다.

콘텐츠 카드는 사용자에 게 추가 컨텍스트를 제공할 수 있으며 피드백 문자열을 간결 하 게 유지 하는 데 도움이 됩니다.

**Cortana** 는 다음과 같은 콘텐츠 카드 템플릿을 지원 합니다 (완료 화면에서 하나의 템플릿만 사용할 수 있음).

- 제목만
- 최대 3 줄의 텍스트를 포함 하는 제목
- 이미지가 있는 제목
- 이미지가 있는 제목 및 최대 3 줄의 텍스트

이미지는 다음과 같을 수 있습니다.

- 68w x 68h
- 68w x 92h
- 280w x 140h

사용자가 앱에 대 한 카드 또는 텍스트 링크를 클릭 하 여 포그라운드에서 앱을 시작 하도록 할 수도 있습니다.

## <a name="completion-screen"></a>완료 화면

완료 화면에서는 완료 된 음성 명령 태스크에 대 한 정보를 사용자에 게 제공 합니다.

앱이 응답 하는 데 500 밀리초 미만이 소요 되 고 사용자가 추가 정보를 요구 하지 않는 작업은  **Cortana** 와의 추가 상호 작용 없이 완료 됩니다. Cortana는 단순히 완료 화면을 표시 합니다.

여기서는 **놀이 Works** 앱을 사용 하 여 음성 명령 요청에 대 한 완료 화면을 표시 하 여 런던으로 예정 된 여행을 표시 합니다.

:::image type="content" source="images/cortana/cortana-completion-screen-upcomingtrip-small.png" alt-text="향후 여행에 대 한 Cortana 백그라운드 앱 완성의 스크린샷":::

음성 명령은 AdventureWorksCommands.xml에 정의 되어 있습니다.

```xml
<Command Name="whenIsTripToDestination">
  <Example> When is my trip to Las Vegas?</Example>
  <ListenFor RequireAppName="BeforeOrAfterPhrase"> when is [my] trip to {destination}</ListenFor>
  <ListenFor RequireAppName="ExplicitlySpecified"> when is [my] {builtin:AppName} trip to {destination} </ListenFor>
  <Feedback> Looking for trip to {destination}</Feedback>
  <VoiceCommandService Target="AdventureWorksVoiceCommandService"/>
</Command>
```

AdventureWorksVoiceCommandService.cs에는 완료 메시지 메서드가 포함 되어 있습니다.

```csharp
/// <summary>
/// Show details for a single trip, if the trip can be found. 
/// This demonstrates a simple response flow in Cortana.
/// </summary>
/// <param name="destination">The destination, expected to be in the phrase list.</param>
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

## <a name="hand-off-screen"></a>백오프 화면

음성 명령이 인식 되 면 **cortana** 는 약 500 이내에 ReportSuccessAsync을 호출 하 고 피드백을 제공 해야 합니다. app service가 500ms 내에서 음성 명령에 지정 된 작업을 완료할 수 없는 경우 **cortana** 는 앱이 ReportSuccessAsync를 호출할 때까지 또는 최대 5 초 동안 표시 되는 손을 꺼진 화면을 표시 합니다.

App service가 ReportSuccessAsync 또는 다른 VoiceCommandServiceConnection 메서드를 호출 하지 않으면 사용자에 게 오류 메시지가 수신 되 고 app service 호출이 취소 됩니다.

다음은 **놀이 Works** 앱에 대 한 직접 화면의 예입니다. 이 예제에서는 사용자가 향후 여행에 대해 **Cortana** 를 쿼리 했습니다. 이 화면에는 app service 이름, 아이콘 및 **피드백** 문자열과 함께 사용자 지정 된 메시지가 포함 되어 있습니다. 

[!NOTE] VCD 파일에서 **피드백** 문자열을 선언할 수 있습니다. 이 문자열은 Cortana 캔버스에 표시 되는 UI 텍스트에 영향을 주지 않고 **cortana** 에서 말하는 텍스트에만 영향을 줍니다.

:::image type="content" source="images/cortana/cortana-backgroundapp-progress-result.png" alt-text="Cortana 백그라운드 앱 핸드 오프 화면 스크린샷":::

## <a name="progress-screen"></a>진행률 화면

App service가 ReportSuccessAsync을 호출 하는 데 500ms 이상을 사용 하는 경우 **Cortana** 는 사용자에 게 진행률 화면을 제공 합니다. 앱 아이콘이 표시 되 고 GUI와 TTS 진행률 문자열을 모두 제공 하 여 태스크가 적극적으로 처리 되 고 있음을 나타내야 합니다.

**Cortana** 는 최대 5 초 동안 진행률 화면을 표시 합니다. 5 초 후 **Cortana** 는 사용자에 게 오류 메시지를 표시 하 고 app service를 닫습니다. App service가 작업을 완료 하는 데 5 초를 초과 해야 하는 경우 진행률 화면에서 **Cortana** 를 계속 업데이트할 수 있습니다.

다음은 **놀이 Works** 앱에 대 한 진행률 화면의 예입니다. 이 예에서는 사용자가 Las Vegas에 대 한 여행을 취소 했습니다. 진행률 화면에는 작업에 대해 사용자 지정 된 메시지, 아이콘 및 취소 중인 여행에 대 한 정보가 포함 된 콘텐츠 타일이 포함 됩니다.

:::image type="content" source="images/cortana/cortana-progress-screen.png" alt-text="백그라운드 앱 진행률 화면의 Cortana 스크린샷":::

AdventureWorksVoiceCommandService.cs에는 **Cortana** 에서 진행률 화면을 표시 하는 [**ReportProgressAsync**](/uwp/api/Windows.ApplicationModel.VoiceCommands.VoiceCommandServiceConnection) 를 호출 하는 다음과 같은 진행 메시지 메서드가 포함 되어 있습니다.

```    CSharp
/// <summary>
/// Show a progress screen. These should be posted at least every 5 seconds for a 
/// long-running operation.
/// </summary>
/// <param name="message">The message to display, relating to the task being performed.</param>
/// <returns></returns>
private async Task ShowProgressScreen(string message)
{
    var userProgressMessage = new VoiceCommandUserMessage();
    userProgressMessage.DisplayMessage = userProgressMessage.SpokenMessage = message;

    VoiceCommandResponse response = VoiceCommandResponse.CreateResponse(userProgressMessage);
    await voiceServiceConnection.ReportProgressAsync(response);
}
```

## <a name="confirmation-screen"></a>확인 화면

음성 명령으로 지정 된 작업이 되돌릴 수 없는 경우, 심각한 영향을 주거나 인식 신뢰도가 높으면 app service에서 확인을 요청할 수 있습니다.

다음은 **놀이 Works** 앱에 대 한 확인 화면의 예입니다. 이 예제에서 사용자는 **Cortana** 를 통해 Las Vegas에 대 한 여행을 취소 하도록 app service에 지시 했습니다. App service에서 사용자에 게 전화를 취소 하기 전에 예 또는 아니오로 대답을 묻는 확인 화면을 제공 하는 **Cortana** 를 제공 했습니다.

사용자가 "예" 또는 "아니요" 이외의 항목을 표시 하는 경우 **Cortana** 는 질문에 대 한 답변을 확인할 수 없습니다. 이 경우 **Cortana** 는 사용자에 게 app service에서 제공 하는 비슷한 질문을 표시 합니다.

두 번째 프롬프트에서 사용자에 게 "예" 또는 "아니요"가 표시 되지 않는 경우 **Cortana** 는 사과 접두사가 있는 동일한 질문을 사용 하 여 사용자에 게 세 번째를 표시 합니다. 사용자에 게 "예" 또는 "아니요"가 표시 되지 않으면 **Cortana** 에서 음성 입력 수신을 중지 하 고 대신 단추 중 하나를 탭 하도록 요청 합니다.

확인 화면에는 작업에 대해 사용자 지정 된 메시지, 아이콘 및 취소 중인 여행에 대 한 정보가 포함 된 콘텐츠 타일이 포함 됩니다.

:::image type="content" source="images/cortana/cortana-confirmation-screen.png" alt-text="백그라운드 앱 확인 화면이 있는 Cortana 스크린샷":::

AdventureWorksVoiceCommandService.cs에는 **Cortana** 에서 확인 화면을 표시 하는 [**RequestConfirmationAsync**](/uwp/api/Windows.ApplicationModel.VoiceCommands.VoiceCommandServiceConnection) 를 호출 하는 다음과 같은 여행 취소 메서드가 포함 되어 있습니다.

```    CSharp
/// <summary>
/// Handle the Trip Cancellation task. This task demonstrates how to prompt a user
/// for confirmation of an operation, show users a progress screen while performing
/// a long-running task, and show a completion screen.
/// </summary>
/// <param name="destination">The name of a destination.</param>
/// <returns></returns>
private async Task SendCompletionMessageForCancellation(string destination)
{
    // Begin loading data to search for the target store. 
    // Consider inserting a progress screen here, in order to prevent Cortana from timing out. 
    string progressScreenString = string.Format(
        cortanaResourceMap.GetValue("ProgressLookingForTripToDest", cortanaContext).ValueAsString,
        destination);
    await ShowProgressScreen(progressScreenString);

    Model.TripStore store = new Model.TripStore();
    await store.LoadTrips();

    IEnumerable<Model.Trip> trips = store.Trips.Where(p => p.Destination == destination);
    Model.Trip trip = null;
    if (trips.Count() > 1)
    {
        // If there is more than one trip, provide a disambiguation screen.
        // However, if a significant number of items are returned, you might want to 
        // just display a link to your app and provide a deeper search experience.
        string disambiguationDestinationString = string.Format(
            cortanaResourceMap.GetValue("DisambiguationWhichTripToDest", cortanaContext).ValueAsString,
            destination);
        string disambiguationRepeatString = cortanaResourceMap.GetValue("DisambiguationRepeat", cortanaContext).ValueAsString;
        trip = await DisambiguateTrips(trips, disambiguationDestinationString, disambiguationRepeatString);
    }
    else
    {
        trip = trips.FirstOrDefault();
    }

    var userPrompt = new VoiceCommandUserMessage();
    
    VoiceCommandResponse response;
    if (trip == null)
    {
        var userMessage = new VoiceCommandUserMessage();
        string noSuchTripToDestination = string.Format(
            cortanaResourceMap.GetValue("NoSuchTripToDestination", cortanaContext).ValueAsString,
            destination);
        userMessage.DisplayMessage = userMessage.SpokenMessage = noSuchTripToDestination;

        response = VoiceCommandResponse.CreateResponse(userMessage);
        await voiceServiceConnection.ReportSuccessAsync(response);
    }
    else
    {
        // Prompt the user for confirmation that this is the correct trip to cancel.
        string cancelTripToDestination = string.Format(
            cortanaResourceMap.GetValue("CancelTripToDestination", cortanaContext).ValueAsString,
            destination);
        userPrompt.DisplayMessage = userPrompt.SpokenMessage = cancelTripToDestination;
        var userReprompt = new VoiceCommandUserMessage();
        string confirmCancelTripToDestination = string.Format(
            cortanaResourceMap.GetValue("ConfirmCancelTripToDestination", cortanaContext).ValueAsString,
            destination);
        userReprompt.DisplayMessage = userReprompt.SpokenMessage = confirmCancelTripToDestination;
        
        response = VoiceCommandResponse.CreateResponseForPrompt(userPrompt, userReprompt);

        var voiceCommandConfirmation = await voiceServiceConnection.RequestConfirmationAsync(response);

        // If RequestConfirmationAsync returns null, Cortana has likely been dismissed.
        if (voiceCommandConfirmation != null)
        {
            if (voiceCommandConfirmation.Confirmed == true)
            {
                string cancellingTripToDestination = string.Format(
               cortanaResourceMap.GetValue("CancellingTripToDestination", cortanaContext).ValueAsString,
               destination);
                await ShowProgressScreen(cancellingTripToDestination);

                // Perform the operation to remove the trip from app data. 
                // As the background task runs within the app package of the installed app,
                // we can access local files belonging to the app without issue.
                await store.DeleteTrip(trip);

                // Provide a completion message to the user.
                var userMessage = new VoiceCommandUserMessage();
                string cancelledTripToDestination = string.Format(
                    cortanaResourceMap.GetValue("CancelledTripToDestination", cortanaContext).ValueAsString,
                    destination);
                userMessage.DisplayMessage = userMessage.SpokenMessage = cancelledTripToDestination;
                response = VoiceCommandResponse.CreateResponse(userMessage);
                await voiceServiceConnection.ReportSuccessAsync(response);
            }
            else
            {
                // Confirm no action for the user.
                var userMessage = new VoiceCommandUserMessage();
                string keepingTripToDestination = string.Format(
                    cortanaResourceMap.GetValue("KeepingTripToDestination", cortanaContext).ValueAsString,
                    destination);
                userMessage.DisplayMessage = userMessage.SpokenMessage = keepingTripToDestination;

                response = VoiceCommandResponse.CreateResponse(userMessage);
                await voiceServiceConnection.ReportSuccessAsync(response);
            }
        }
    }
}
```

## <a name="disambiguation-screen"></a>명확성 화면

음성 명령으로 지정 된 작업에 가능한 결과가 둘 이상 있는 경우 app service에서 사용자 로부터 추가 정보를 요청할 수 있습니다.

다음은 **놀이 Works** 앱의 명확성 화면 예제입니다. 이 예제에서 사용자는 **Cortana** 를 통해 Las Vegas에 대 한 여행을 취소 하도록 app service에 지시 했습니다. 그러나 사용자는 서로 다른 날짜에 Las Vegas를 두 번 이동 하 고, 사용자가 의도 한 여행을 선택 하지 않고도 app service가 작업을 완료할 수 없습니다.

App service는 사용자에 게 일치 항목을 취소 하기 전에 일치 하는 트립 목록에서 선택 하 라는 메시지를 표시 하는 명확성 화면을 **Cortana** 에 제공 합니다.

이 경우 **Cortana** 는 사용자에 게 app service에서 제공 하는 비슷한 질문을 표시 합니다.

두 번째 프롬프트에서 사용자가 선택 항목을 식별 하는 데 사용할 수 있는 어떤 것도 표시 되지 않는 경우 **Cortana** 는 사과 접두사가 있는 동일한 질문을 사용 하 여 사용자에 게 세 번째를 표시 합니다. 사용자가 선택 항목을 식별 하는 데 사용할 수 있는 어떤 것도 없는 경우 **Cortana** 는 음성 입력 수신을 중지 하 고 대신 단추 중 하나를 탭 하도록 요청 합니다.

명확성 화면에는 작업에 대해 사용자 지정 된 메시지, 아이콘 및 취소 중인 여행에 대 한 정보가 포함 된 콘텐츠 타일이 포함 됩니다.

:::image type="content" source="images/cortana/cortana-disambiguation-screen.png" alt-text="백그라운드 앱 명확성 화면을 사용 하는 Cortana 스크린샷":::

AdventureWorksVoiceCommandService.cs에는 **Cortana** 에서 명확성 화면을 표시 하는 [**RequestDisambiguationAsync**](/uwp/api/Windows.ApplicationModel.VoiceCommands.VoiceCommandServiceConnection) 를 호출 하는 다음과 같은 여행 취소 메서드가 포함 되어 있습니다.

```csharp
/// <summary>
/// Provide the user with a way to identify which trip to cancel. 
/// </summary>
/// <param name="trips">The set of trips</param>
/// <param name="disambiguationMessage">The initial disambiguation message</param>
/// <param name="secondDisambiguationMessage">Repeat prompt retry message</param>
private async Task<Model.Trip> DisambiguateTrips(IEnumerable<Model.Trip> trips, string disambiguationMessage, string secondDisambiguationMessage)
{
    // Create the first prompt message.
    var userPrompt = new VoiceCommandUserMessage();
    userPrompt.DisplayMessage =
        userPrompt.SpokenMessage = disambiguationMessage;

    // Create a re-prompt message if the user responds with an out-of-grammar response.
    var userReprompt = new VoiceCommandUserMessage();
    userReprompt.DisplayMessage =
        userReprompt.SpokenMessage = secondDisambiguationMessage;

    // Create card for each item. 
    var destinationContentTiles = new List<VoiceCommandContentTile>();
    int i = 1;
    foreach (Model.Trip trip in trips)
    {
        var destinationTile = new VoiceCommandContentTile();

        destinationTile.ContentTileType = VoiceCommandContentTileType.TitleWith68x68IconAndText;
        destinationTile.Image = await StorageFile.GetFileFromApplicationUriAsync(new Uri("ms-appx:///AdventureWorks.VoiceCommands/Images/GreyTile.png"));
        
        // The AppContext can be any arbitrary object.
        destinationTile.AppContext = trip;
        string dateFormat = "";
        if (trip.StartDate != null)
        {
            dateFormat = trip.StartDate.Value.ToString(dateFormatInfo.LongDatePattern);
        }
        else
        {
            // The app allows a trip to have no date.
            // However, the choices must be unique so they can be distinguished.
            // Here, we add a number to identify them.
            dateFormat = string.Format("{0}", i);
        } 

        destinationTile.Title = trip.Destination + " " + dateFormat;
        destinationTile.TextLine1 = trip.Description;

        destinationContentTiles.Add(destinationTile);
        i++;
    }

    // Cortana handles re-prompting if no valid response.
    var response = VoiceCommandResponse.CreateResponseForPrompt(userPrompt, userReprompt, destinationContentTiles);

    // If cortana is dismissed in this operation, null is returned.
    var voiceCommandDisambiguationResult = await
        voiceServiceConnection.RequestDisambiguationAsync(response);
    if (voiceCommandDisambiguationResult != null)
    {
        return (Model.Trip)voiceCommandDisambiguationResult.SelectedItem.AppContext;
    }

    return null;
}
```

## <a name="error-screen"></a>오류 화면

음성 명령으로 지정 된 작업을 완료할 수 없는 경우 app service에서 오류 화면을 제공할 수 있습니다.

다음은 **놀이 Works** 앱에 대 한 오류 화면의 예입니다. 이 예제에서 사용자는 **Cortana** 를 통해 Las Vegas에 대 한 여행을 취소 하도록 app service에 지시 했습니다. 그러나 사용자에 게는 Las Vegas 예약 된 작업이 없습니다.

App service는 액션, 아이콘 및 특정 오류 메시지에 대해 사용자 지정 된 메시지를 포함 하는 오류 화면을 **Cortana** 에 제공 합니다.

[**ReportFailureAsync**](/uwp/api/Windows.ApplicationModel.VoiceCommands.VoiceCommandServiceConnection) 를 호출 하 여 **Cortana** 에서 오류 화면을 표시 합니다.

```csharp
var userMessage = new VoiceCommandUserMessage();
    userMessage.DisplayMessage = userMessage.SpokenMessage = 
      "Sorry, you don't have any trips to Las Vegas";
                
    var response = VoiceCommandResponse.CreateResponse(userMessage);

    response.AppLaunchArgument = "showUpcomingTrips";
    await voiceServiceConnection.ReportFailureAsync(response);
```

## <a name="related-articles"></a>관련된 문서

- [Windows 앱에서 Cortana 상호 작용](cortana-interactions.md)
- [VCD 요소 및 특성 v 1.2](/uwp/schemas/voicecommands/voice-command-elements-and-attributes-1-2)
- [Cortana 디자인 지침](cortana-design-guidelines.md)
- [Cortana 음성 명령 샘플](https://go.microsoft.com/fwlink/p/?LinkID=619899)
