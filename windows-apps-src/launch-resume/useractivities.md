---
title: 디바이스 간에도 사용자 활동 계속 수행
description: 이 항목에서는 사용자가 앱에서 수행 하 던 작업을 여러 장치에서 다시 시작 하는 방법을 설명 합니다.
keywords: 사용자 활동, 사용자 활동, 타임 라인, cortana가 중단 되 면 cortana를 선택 하 고, 프로젝트 로마를 선택 합니다.
ms.date: 04/27/2018
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: ebaca3b831ae30637a88d01319a89d139dde1cf8
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89162627"
---
# <a name="continue-user-activity-even-across-devices"></a>디바이스 간에도 사용자 활동 계속 수행

이 항목에서는 사용자가 자신의 PC에서 그리고 장치에서 수행 하 던 작업을 다시 시작 하는 방법을 설명 합니다.

## <a name="user-activities-and-timeline"></a>사용자 활동 및 타임 라인

매일 매일 여러 장치에 걸쳐 분산 됩니다. Microsoft는 버스에서, 하루 중에 PC를 사용 하 고 나 서 저녁에 휴대폰 이나 태블릿을 사용할 수 있습니다. Windows 10 빌드 1803 이상부터 [사용자 작업](/uwp/api/windows.applicationmodel.useractivities.useractivity) 을 만들면 해당 작업이 Windows 타임 라인에 표시 되 고 Cortana에서 기능을 중단 한 위치를 선택 하 게 됩니다. 타임 라인은 사용자 활동을 활용 하 여 작업 중인 작업의 시간순 보기를 표시 하는 다양 한 작업 보기입니다. 또한 장치 간에 작업 하 던 작업을 포함할 수 있습니다.

![Windows 타임 라인 이미지](images/timeline.png)

마찬가지로, Windows PC에 휴대폰을 연결 하면 이전에 iOS 또는 Android 장치에서 수행한 작업을 계속할 수 있습니다.

사용자가 앱 내에서 작업 하는 것과 관련 된 **UserActivity** 생각 합니다. 예를 들어 RSS 판독기를 사용 하는 경우 **UserActivity** 는 읽고 있는 피드가 될 수 있습니다. 게임을 재생 하는 경우 **UserActivity** 는 게임 중인 수준입니다. 음악 앱을 수신 대기 하는 경우 **UserActivity** 은 수신 하 고 있는 재생 목록 일 수 있습니다. 문서에 대 한 작업을 수행 하는 경우에는 **UserActivity** 에 대 한 작업을 중단 하는 등의 작업을 수행할 수 있습니다.  즉, **UserActivity** 은 사용자가 수행 중인 작업을 다시 시작할 수 있도록 앱 내의 대상을 나타냅니다.

[CreateSession](/uwp/api/windows.applicationmodel.useractivities.useractivity.createsession)를 호출 하 여 **UserActivity** 에 참여 하는 경우 시스템은 해당 **UserActivity**의 시작 및 종료 시간을 나타내는 기록 레코드를 만듭니다. 시간이 지남에 따라 해당 **UserActivity** 를 다시 사용 하는 경우이에 대 한 여러 기록 레코드가 기록 됩니다.

## <a name="add-user-activities-to-your-app"></a>앱에 사용자 활동 추가

[UserActivity](/uwp/api/windows.applicationmodel.useractivities.useractivity) 는 Windows의 사용자 참여 단위입니다. 활동을 설명 하는 앱을 활성화 하는 데 사용 되는 URI, 시각적 개체, 활동을 설명 하는 메타 데이터 등 세 부분으로 구성 됩니다.

1. [ActivationUri](/uwp/api/windows.applicationmodel.useractivities.useractivity.activationuri#Windows_ApplicationModel_UserActivities_UserActivity_ActivationUri) 는 특정 컨텍스트를 사용 하 여 응용 프로그램을 다시 시작 하는 데 사용 됩니다. 일반적으로이 링크는 체계의 프로토콜 처리기 형식 (예: "my app:/page2? action = edit") 또는 AppUriHandler (예:)를 사용 http://constoso.com/page2?action=edit) 합니다.
2. [Visualelements](/uwp/api/windows.applicationmodel.useractivities.useractivity.visualelements) 는 사용자가 제목, 설명 또는 적응형 카드 요소가 포함 된 활동을 시각적으로 식별할 수 있도록 하는 클래스를 노출 합니다.
3. 마지막으로, [콘텐츠](/uwp/api/windows.applicationmodel.useractivities.useractivityvisualelements.content#Windows_ApplicationModel_UserActivities_UserActivityVisualElements_Content) 는 특정 컨텍스트에서 작업을 그룹화 하 고 검색 하는 데 사용할 수 있는 작업에 대 한 메타 데이터를 저장할 수 있습니다. 데이터 형식을 사용 하는 경우가 많습니다 [https://schema.org](https://schema.org) .

앱에 **UserActivity** 를 추가 하려면 다음을 수행 합니다.

1. 앱 내에서 사용자의 컨텍스트가 변경 될 때 (예: 페이지 탐색, 새 게임 수준 등) **UserActivity** 개체를 생성 합니다.
2. **UserActivity** 개체를 필요한 최소 필드 집합 [(작업 id](/uwp/api/windows.applicationmodel.useractivities.useractivity.activityid#Windows_ApplicationModel_UserActivities_UserActivity_ActivityId), [ActivationUri](/uwp/api/windows.applicationmodel.useractivities.useractivity.activationuri)및 [UserActivity 인 displaytext](/uwp/api/windows.applicationmodel.useractivities.useractivityvisualelements.displaytext#Windows_ApplicationModel_UserActivities_UserActivityVisualElements_DisplayText))으로 채웁니다.
3. **UserActivity**에서 다시 활성화할 수 있도록 사용자 지정 스키마 처리기를 앱에 추가 합니다.

**UserActivity** 는 코드를 몇 줄만 사용 하 여 앱에 통합할 수 있습니다. 예를 들어 MainPage.xaml.cs의 MainPage 클래스 내에서이 코드를 가정 합니다 (참고: 가정 `using Windows.ApplicationModel.UserActivities;` ).

```csharp
UserActivitySession _currentActivity;
private async Task GenerateActivityAsync()
{
    // Get the default UserActivityChannel and query it for our UserActivity. If the activity doesn't exist, one is created.
    UserActivityChannel channel = UserActivityChannel.GetDefault();
    UserActivity userActivity = await channel.GetOrCreateUserActivityAsync("MainPage");
 
    // Populate required properties
    userActivity.VisualElements.DisplayText = "Hello Activities";
    userActivity.ActivationUri = new Uri("my-app://page2?action=edit");
     
    //Save
    await userActivity.SaveAsync(); //save the new metadata
 
    // Dispose of any current UserActivitySession, and create a new one.
    _currentActivity?.Dispose();
    _currentActivity = userActivity.CreateSession();
}
```

위의 메서드에서 첫 번째 줄은 `GenerateActivityAsync()` 사용자의 [Useractivitychannel](/uwp/api/windows.applicationmodel.useractivities.useractivitychannel)을 가져옵니다. 이는이 앱의 활동이 게시 될 피드입니다. 다음 줄은 라는 활동의 채널을 쿼리 합니다 `MainPage` .

* 앱은 사용자가 앱의 특정 위치에 있을 때마다 동일한 ID가 생성 되는 방식으로 활동 이름을 지정 해야 합니다. 예를 들어 앱이 페이지 기반 인 경우 페이지에 대 한 식별자를 사용 합니다. 문서를 기반으로 하는 경우 문서 이름 또는 이름의 해시를 사용 합니다.
* 피드에 동일한 ID를 가진 기존 활동이 있는 경우 해당 활동은 `UserActivity.State` [게시 됨](/uwp/api/windows.applicationmodel.useractivities.useractivitystate)으로 설정 된 채널에서 반환 됩니다. 해당 이름의 활동이 없으면 new로 설정 된 새 활동이 반환 됩니다 `UserActivity.State` . **New**
* 활동의 범위는 앱으로 지정 됩니다. 다른 앱의 Id와 충돌 하는 작업 ID를 걱정 하지 않아도 됩니다.

**UserActivity**를 가져오거나 만든 후에는 다른 두 필수 필드인 및를 지정 `UserActivity.VisualElements.DisplayText` 합니다 `UserActivity.ActivationUri` .

그런 다음 [SaveAsync](/uwp/api/windows.applicationmodel.useractivities.useractivity.saveasync)를 호출 하 여 **UserActivity** 메타 데이터를 저장 하 고, 마지막으로 [Useractivitysession](/uwp/api/windows.applicationmodel.useractivities.useractivitysession)을 반환 하는 [CreateSession](/uwp/api/windows.applicationmodel.useractivities.useractivity.createsession)를 저장 합니다. **Useractivitysession** 은 사용자가 실제로 **UserActivity**를 사용 하는 경우를 관리 하는 데 사용할 수 있는 개체입니다. 예를 들어 `Dispose()` 사용자가 페이지를 떠날 때 **Useractivitysession** 에 대해를 호출 해야 합니다. 위의 예제에서는를 `Dispose()` `_currentActivity` 호출 하기 전에도 호출 `CreateSession()` 합니다. 이는 `_currentActivity` 페이지의 멤버 필드를 작성 했 고 새 작업을 시작 하기 전에 기존 작업을 중지 하려고 하기 때문입니다 (참고:는 `?` 멤버 액세스를 수행 하기 전에 null을 테스트 하는 [null 조건 연산자](/dotnet/csharp/language-reference/operators/member-access-operators#null-conditional-operators--and-) 임).

이 경우은 `ActivationUri` 사용자 지정 스키마 이므로 응용 프로그램 매니페스트에도 프로토콜을 등록 해야 합니다. 이 작업은 appmanifest XML 파일에서 수행 하거나 디자이너를 사용 하 여 수행 합니다.

디자이너를 사용 하 여 변경 하려면 프로젝트에서 appmanifest 파일을 두 번 클릭 하 여 디자이너를 시작 하 고 **선언** 탭을 선택한 후 **프로토콜** 정의를 추가 합니다. 지금은 채워야 할 유일한 속성은 **Name**입니다. 위에서 지정한 URI와 일치 해야 합니다 `my-app` .

이제 응용 프로그램에서 프로토콜에 의해 활성화 된 경우 수행할 작업을 알려주는 코드를 작성 해야 합니다. `OnActivated`App.xaml.cs의 메서드를 재정의 하 여 다음과 같이 기본 페이지에 URI를 전달 합니다.

```csharp
protected override void OnActivated(IActivatedEventArgs e)
{
    if (e.Kind == ActivationKind.Protocol)
    {
        var uriArgs = e as ProtocolActivatedEventArgs;
        if (uriArgs != null)
        {
            if (uriArgs.Uri.Host == "page2")
            {
                // Navigate to the 2nd page of the  app
            }
        }
    }
    Window.Current.Activate();
}
```

이 코드는 응용 프로그램이 프로토콜을 통해 활성화 되었는지 여부를 검색 합니다. 그렇다면 앱이 활성화 되는 작업을 다시 시작 하기 위해 수행 해야 하는 작업을 확인 합니다. 앱이 다시 시작 되는 유일한 작업은 간단한 앱 이며, 앱이 시작 될 때 보조 페이지에 표시 됩니다.

## <a name="use-adaptive-cards-to-improve-the-timeline-experience"></a>적응 카드를 사용 하 여 타임 라인 환경 개선

사용자 활동은 Cortana 및 타임 라인에 표시 됩니다. 작업이 타임 라인에 표시 되 면 [적응 카드](https://adaptivecards.io/) 프레임 워크를 사용 하 여 표시 됩니다. 각 작업에 대 한 적응 카드를 제공 하지 않으면 타임 라인에서 자동으로 응용 프로그램 이름 및 아이콘, 제목 필드 및 선택적 설명 필드를 기반으로 간단한 작업 카드를 만듭니다. 아래에는 적응 카드 페이로드와 생성 카드의 예가 나와 있습니다.

![적응 카드](images/adaptivecard.png)]

적응 카드 페이로드 JSON 문자열 예제:

```json
{ 
  "$schema": "http://adaptivecards.io/schemas/adaptive-card.json", 
  "type": "AdaptiveCard", 
  "version": "1.0",
  "backgroundImage": "https://winblogs.azureedge.net/win/2017/11/eb5d872c743f8f54b957ff3f5ef3066b.jpg", 
  "body": [ 
    { 
      "type": "Container", 
      "items": [ 
        { 
          "type": "TextBlock", 
          "text": "Windows Blog", 
          "weight": "bolder", 
          "size": "large", 
          "wrap": true, 
          "maxLines": 3 
        }, 
        { 
          "type": "TextBlock", 
          "text": "Training Haiti’s radiologists: St. Louis doctor takes her teaching global", 
          "size": "default", 
          "wrap": true, 
          "maxLines": 3 
        } 
      ] 
    } 
  ]
}
```

**UserActivity** 에 다음과 같이 적응 카드 페이로드를 JSON 문자열로 추가 합니다.

```csharp
activity.VisualElements.Content = 
Windows.UI.Shell.AdaptiveCardBuilder.CreateAdaptiveCardFromJson(jsonCardText); // where jsonCardText is a JSON string that represents the card
```

## <a name="cross-platform-and-service-to-service-integration"></a>플랫폼 간 및 서비스 간 통합

앱에서 플랫폼 간 (예: Android 및 iOS)을 실행 하거나 클라우드에서 사용자 상태를 유지 관리 하는 경우 [Microsoft Graph](https://developer.microsoft.com/graph)를 통해 useractivities을 게시할 수 있습니다.
응용 프로그램 또는 서비스가 Microsoft 계정을 사용 하 여 인증 되 면 위에서 설명한 것과 동일한 데이터를 사용 하 여 [작업](/graph/api/resources/projectrome-activity) 및 [기록](/graph/api/resources/projectrome-historyitem) 개체를 생성 하는 두 개의 간단한 REST 호출을 수행 합니다.

## <a name="summary"></a>요약

[UserActivity](/uwp/api/windows.applicationmodel.useractivities) API를 사용 하 여 앱이 타임 라인 및 Cortana에 표시 되도록 할 수 있습니다.
* [ **UserActivity** API에 대 한 자세한 정보](/uwp/api/windows.applicationmodel.useractivities)
* [샘플 코드](https://github.com/Microsoft/project-rome)를 확인 하세요.
* [보다 정교한 적응 카드](https://adaptivecards.io/)를 참조 하세요.
* [Microsoft Graph](https://developer.microsoft.com/graph)를 통해 IOS, Android 또는 웹 서비스에서 **UserActivity** 을 게시 합니다.
* [GitHub의 프로젝트 로마](https://github.com/Microsoft/project-rome)에 대해 자세히 알아보세요.

## <a name="key-apis"></a>키 Api

* [UserActivities 네임스페이스](/uwp/api/windows.applicationmodel.useractivities)

## <a name="related-topics"></a>관련 항목

* [사용자 활동 (Project 로마 문서)](/windows/project-rome/user-activities/)
* [적응 카드](/adaptive-cards/)
* [적응 카드 시각화, 샘플](https://adaptivecards.io/)
* [URI 활성화 처리](./handle-uri-activation.md)
* [Microsoft Graph, 활동 피드 및 적응 카드를 사용 하 여 모든 플랫폼에서 고객 참여](https://channel9.msdn.com/Events/Connect/2017/B111)
* [Microsoft Graph](https://developer.microsoft.com/graph)