---
author: Karl-Bridge-Microsoft
Description: "사용자가 음성 명령을 실행하는 동안 Cortana 음성 및 캔버스를 통해 백그라운드 앱을 조작할 수 있는 방법을 알아봅니다."
title: "백그라운드 앱 조작"
ms.assetid: 6C60F03C-A242-435D-96BB-736892CC1CA6
label: Interact with a background app
template: detail.hbs
ms.sourcegitcommit: 7d9f5eff0f6561b18024658fe99d1e11bbe3309f
ms.openlocfilehash: 675553f5c3954597982360900e965b2a756d7f63

---

# Cortana에서 백그라운드 앱 조작

사용자가 음성 명령을 실행하는 동안 **Cortana** 캔버스의 음성 및 텍스트 입력을 통해 백그라운드 앱을 조작할 수 있게 합니다.



**중요 API**

-   [**Windows.ApplicationModel.VoiceCommands**](https://msdn.microsoft.com/library/windows/apps/dn706594)
-   [**VCD(음성 명령 정의) 요소 및 특성 v1.2**](https://msdn.microsoft.com/library/windows/apps/dn706593)


Cortana는 앱을 사용한 전체 턴바이턴 워크플로를 지원합니다. 이 워크플로는 앱에서 정의되며 다음과 같은 기능을 지원할 수 있습니다. 

-   성공적으로 완료
-   넘기기
-   진행률
-   확인
-   명확성
-   오류

**필수 조건:**

이 항목은 [Cortana에서 음성 명령으로 백그라운드 앱 시작](launch-a-background-app-with-voice-commands-in-cortana.md)을 기반으로 합니다. 여기서는 **Adventure Works**라는 여행 계획 및 관리 앱으로 계속 기능을 소개합니다.

UWP(유니버설 Windows 플랫폼) 앱을 처음 개발하는 경우 다음 항목을 검토하여 여기서 설명하는 기술에 대해 알아보세요.

-   [첫 번째 앱 만들기](https://msdn.microsoft.com/library/windows/apps/bg124288)
-   이벤트에 대한 자세한 내용은 [이벤트 및 라우트된 이벤트 개요](https://msdn.microsoft.com/library/windows/apps/mt185584)를 참조하세요.

**사용자 환경 지침:  **

**Cortana**와 앱을 통합하는 방법에 대한 자세한 내용은 [Cortana 디자인 지침](https://msdn.microsoft.com/library/windows/apps/dn974233)을 참조하고, 유용하고 매력적인 음성 사용 앱 디자인에 도움이 되는 팁은 [음성 디자인 지침](https://msdn.microsoft.com/library/windows/apps/dn596121)을 참조하세요.

## <span id="Feedback_strings"></span><span id="feedback_strings"></span><span id="FEEDBACK_STRINGS"></span>피드백 문자열

**Cortana**를 통해 표시하고 말할 피드백 문자열을 작성합니다.

[Cortana 디자인 지침](https://msdn.microsoft.com/library/windows/apps/dn974233)에서는 **Cortana**용 문자열 작성에 대한 권장 사항을 제공합니다.

## <span id="Feedback_strings"></span><span id="feedback_strings"></span><span id="FEEDBACK_STRINGS"></span>피드백 문자열

콘텐츠 카드는 사용자에 대한 추가 컨텍스트를 제공하고 피드백 문자열을 간결하게 유지할 수 있습니다.

**Cortana**는 다음 콘텐츠 카드 템플릿을 지원합니다. 완료 화면에서 한 템플릿만 사용할 수 있습니다.

    -   Title only
    -   Title with up to three lines of text
    -   Title with image
    -   Title with image and up to three lines of text

이미지는 다음과 같을 수 있습니다.

    -   68w x 68h
    -   68w x 92h
    -   280w x 140h

또한 사용자가 카드 또는 앱의 텍스트 링크를 클릭하여 포그라운드에서 앱을 실행하도록 설정할 수 있습니다.

## <span id="Completion_screen"></span><span id="completion_screen"></span><span id="COMPLETION_SCREEN"></span>완료 화면

완료 화면에서는 완료된 음성 명령 작업에 대한 정보를 사용자에게 제공합니다.

앱이 응답하는 데 500밀리초 미만의 시간이 걸리고 사용자로부터의 추가 정보가 불필요한 작업은 **Cortana**와의 추가 상호 작용 없이 완료됩니다. Cortana에서 완료 화면을 표시합니다.

여기서는 **Adventure Works** 앱을 사용하여 예정된 런던 여행을 표시하는 음성 명령 요청에 대한 완료 화면을 표시합니다. 

![Cortana 백그라운드 앱 완료 화면](images/cortana-completion-screen-upcomingtrip-small.png)

음성 명령은 AdventureWorksCommands.xml에서 정의됩니다.
```
<Command Name="whenIsTripToDestination">
  <Example> When is my trip to Las Vegas?</Example>
  <ListenFor RequireAppName="BeforeOrAfterPhrase"> when is [my] trip to {destination}</ListenFor>
  <ListenFor RequireAppName="ExplicitlySpecified"> when is [my] {builtin:AppName} trip to {destination} </ListenFor>
  <Feedback> Looking for trip to {destination}</Feedback>
  <VoiceCommandService Target="AdventureWorksVoiceCommandService"/>
</Command>
```

AdventureWorksVoiceCommandService.cs에는 완료 메시지 메서드가 포함되어 있습니다.

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

## <span id="Hand-off_screen"></span><span id="hand-off_screen"></span><span id="HAND-OFF_SCREEN"></span>넘기기 화면

음성 명령이 인식되면 **Cortana**에서 ReportSuccessAsync를 호출하고 약 500ms 이내에 피드백을 제공해야 합니다. 앱 서비스에서 500밀리초 이내에 음성 명령으로 지정된 작업을 완료할 수 없는 경우 **Cortana**에서 최대 5초 동안 표시되는 넘기기 화면을 제공합니다.

앱 서비스가 ReportSuccessAsync 또는 다른 VoiceCommandServiceConnection 메서드를 호출하지 않는 경우 사용자에게 오류 메시지가 표시되고 앱 서비스 호출이 취소됩니다.

다음은 **Adventure Works** 앱에 대한 넘기기 화면의 예입니다. 이 예제에서 사용자는 예정된 여행을 **Cortana**에 쿼리했습니다. 넘기기 화면에는 앱 서비스 이름, 아이콘 및 **Feedback** 문자열로 사용자 지정된 메시지가 포함되어 있습니다. 

[!NOTE] **Feedback** 문자열은 VCD 파일에서 선언할 수 있습니다. 이 문자열은 Cortana 캔버스에 표시되는 UI 텍스트에 영향을 주지 않으며 **Cortana**가 말하는 텍스트에만 영향을 줍니다.

![Cortana 백그라운드 앱 넘기기 화면](images/cortana-backgroundapp-progress-result.png)


## <span id="Progress_screen"></span><span id="progress_screen"></span><span id="PROGRESS_SCREEN"></span>진행률 화면


앱 서비스가 ReportSuccessAsync를 호출하는 데 500ms 이상 걸리는 경우 **Cortana**에서 사용자에게 진행률 화면을 제공합니다. 앱 아이콘이 표시됩니다. 작업이 원활하게 처리되고 있음을 나타내기 위해 GUI 및 TTS 진행률 문자열을 둘 다 제공해야 합니다.

**Cortana**에서는 최대 5 초 동안 진행률 화면을 표시합니다. 5초 후 **Cortana**에서 사용자에게 오류 메시지를 제공하고 앱 서비스가 닫힙니다. 앱 서비스에서 작업을 완료 하는 데 5 초 이상 걸리는 경우 계속 진행률 화면으로 **Cortana**를 업데이트합니다.

다음은 **Adventure Works** 앱에 대한 진행률 화면의 예입니다. 이 예제에서는 사용자가 라스베이거스 여행을 취소했습니다. 진행률 화면에는 동작, 아이콘 및 취소 중인 여행에 대한 정보가 있는 콘텐츠 타일에 맞게 사용자 지정된 메시지가 포함됩니다.

![Cortana 백그라운드 앱 진행률 화면 ](images/cortana-progress-screen.png)

AdventureWorksVoiceCommandService.cs에는 [**ReportProgressAsync**](https://msdn.microsoft.com/library/windows/apps/dn706579)를 호출하여 **Cortana**에서 진행률 화면을 표시하는 다음과 같은 진행률 메시지 메서드가 포함되어 있습니다.


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

## <span id="Confirmation_screen"></span><span id="confirmation_screen"></span><span id="CONFIRMATION_SCREEN"></span>확인 화면


음성 명령으로 지정된 동작이 되돌릴 수 없는 동작이고 상당한 영향을 미치거나 인식 신뢰도가 높지 않은 경우 앱 서비스에서 확인을 요청할 수 있습니다.

다음은 **Adventure Works** 앱에 대한 확인 화면의 예입니다. 이 예에서는 사용자가 **Cortana**를 통해 라스베이거스에 대한 여행을 취소하도록 앱 서비스에 지시했습니다. 앱 서비스에서 여행을 취소하기 전에 사용자에게 예 또는 아니요 응답을 묻는 확인 화면을 **Cortana**에 제공했습니다.

사용자가 "예" 또는 "아니요" 이외의 말을 할 경우 **Cortana**에서 질문에 대한 응답을 확인할 수 없습니다. 이 경우 **Cortana**가 앱 서비스에서 제공하는 유사한 질문을 사용자에게 표시합니다.

두 번째 프롬프트에서, 사용자가 여전히 "예" 또는 "아니요"를 말하지 않으면 **Cortana**에서 사과 다음에 동일한 질문을 3번째로 사용자에게 표시합니다. 사용자가 여전히 "예" 또는 "아니요"를 말하지 않으면 **Cortana**가 음성 입력에 대한 수신 대기를 중지하고 단추 중 하나를 탭하도록 요청합니다.

확인 화면에는 동작, 아이콘 및 취소 중인 여행에 대한 정보가 있는 콘텐츠 타일에 맞게 사용자 지정된 메시지가 포함됩니다.

![Cortana 백그라운드 앱 확인 화면](images/cortana-confirmation-screen.png)

AdventureWorksVoiceCommandService.cs에는 [**RequestConfirmationAsync**](https://msdn.microsoft.com/library/windows/apps/dn706582)를 호출하여 **Cortana**에서 확인 화면을 표시하는 다음과 같은 여행 취소 메서드가 포함되어 있습니다.

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

## <span id="Disambiguation_screen"></span><span id="disambiguation_screen"></span><span id="DISAMBIGUATION_SCREEN"></span>명확성 화면


음성 명령으로 지정된 동작의 작업 결과가 둘 이상이 될 수 있는 경우 앱 서비스에서 사용자에게 더 자세한 정보를 요청할 수 있습니다.

다음은 **Adventure Works** 앱의 명확성 화면에 대한 예입니다. 이 예에서는 사용자가 **Cortana**를 통해 라스베이거스에 대한 여행을 취소하도록 앱 서비스에 지시했습니다. 그러나 날짜가 다른 라스베이거스 여행이 두 개 있어서 앱 서비스에서 사용자가 의도한 여행 선택 작업을 완료할 수 없습니다.

앱이 취소하기 전에 일치하는 목록에서 취소하려는 서비스를 선택하도록 요청하는 명확성 화면을 **Cortana**에 제공합니다.

이 경우 **Cortana**가 앱 서비스에서 제공하는 유사한 질문을 사용자에게 표시합니다.

두 번째 프롬프트에서 사용자가 여전히 선택 항목을 식별하는 데 사용할 수 있는 대답을 말하지 않으면 **Cortana**에서 사과 다음에 동일한 질문이 포함된 프롬프트를 3번째로 표시합니다. 사용자가 여전히 선택 항목을 식별하는 데 사용할 수 있는 응답을 말하지 않으면 **Cortana**에서 음성 입력에 대한 수신 대기를 중지하고 단추 중 하나를 탭하도록 요청합니다.

명확성 화면에는 동작, 아이콘 및 취소 중인 여행에 대한 정보가 있는 콘텐츠 타일에 맞게 사용자 지정된 메시지가 포함됩니다.

![Cortana 백그라운드 앱 명확성 화면 ](images/cortana-disambiguation-screen.png)

AdventureWorksVoiceCommandService.cs에는 [**RequestDisambiguationAsync**](https://msdn.microsoft.com/library/windows/apps/dn706583)를 호출하여 **Cortana**에서 명확성 화면을 표시하는 다음과 같은 여행 취소 메서드가 포함되어 있습니다.

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

## <span id="Error_screen"></span><span id="error_screen"></span><span id="ERROR_SCREEN"></span>오류 화면


음성 명령으로 지정된 작업을 완료할 수 없으면 앱 서비스에서 오류 화면을 제공할 수 있습니다.

다음은 **Adventure Works** 앱에 대한 오류 화면의 예입니다. 이 예에서는 사용자가 **Cortana**를 통해 라스베이거스에 대한 여행을 취소하도록 앱 서비스에 지시했습니다. 그러나 예정된 라스베이거스 여행이 없습니다.

앱 서비스에서 작업, 아이콘 및 특정 오류 메시지에 대해 사용자 지정된 메시지가 포함된 오류 화면을 **Cortana**에 제공합니다.

[
            **ReportFailureAsync**](https://msdn.microsoft.com/library/windows/apps/dn706578)를 호출하여 **Cortana**에 오류 화면을 표시합니다.

```csharp
var userMessage = new VoiceCommandUserMessage();
    userMessage.DisplayMessage = userMessage.SpokenMessage = 
      "Sorry, you don't have any trips to Las Vegas";
                
    var response = VoiceCommandResponse.CreateResponse(userMessage);

    response.AppLaunchArgument = "showUpcomingTrips";
    await voiceServiceConnection.ReportFailureAsync(response);
```

## <span id="related_topics"></span>관련 문서


**개발자**
* [Cortana 조작](cortana-interactions.md)
* [**VCD 요소 및 특성 v1.2**](https://msdn.microsoft.com/library/windows/apps/dn706593)

**디자이너**
* [Cortana 디자인 지침](https://msdn.microsoft.com/library/windows/apps/dn974233)
* [음성 디자인 지침](https://msdn.microsoft.com/library/windows/apps/dn596121)

**샘플**
* [Cortana 음성 명령 샘플](http://go.microsoft.com/fwlink/p/?LinkID=619899)
 

 







<!--HONumber=Jun16_HO3-->


