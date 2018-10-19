---
author: TylerMSFT
title: 장치 간 사용자 활동 계속 수행
description: 이 항목에서는 여러 장치에서 사용자가 앱에서 수행하고 있었던 작업을 다시 시작하도록 돕는 방법에 대해 설명합니다.
keywords: 사용자 활동, 사용자 활동, 타임라인, cortana 사용자의 마지막 종료 지점부터 시작, cortana 내 마지막 종료 지점부터 시작, 프로젝트 로마
ms.author: twhitney
ms.date: 04/27/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
ms.localizationpriority: medium
ms.openlocfilehash: 53aac2375d60df3cd9493f315b20431961378fe8
ms.sourcegitcommit: 72835733ec429a5deb6a11da4112336746e5e9cf
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/19/2018
ms.locfileid: "5157329"
---
# <a name="continue-user-activity-even-across-devices"></a>장치 간 사용자 활동 계속 수행

이 항목에서는 사용자 PC 및 장치에서 사용자가 앱에서 수행하고 있었던 작업을 다시 시작하도록 돕는 방법에 대해 설명합니다.

## <a name="user-activities-and-timeline"></a>사용자 활동 및 타임라인

우리는 매일 여러 장치를 사용하고 있습니다. 버스를 타고 가는 동안 휴대폰을 사용하고 낮 동안 PC를 사용한 후 저녁에는 휴대폰이나 태블릿을 사용하는 식입니다. Windows 10 빌드 1803 이상부터 [사용자 활동](https://docs.microsoft.com/uwp/api/windows.applicationmodel.useractivities.useractivity)을 만들면 Windows 타임라인 및 Cortana의 마지막 종료 지점부터 시작 기능에 해당 활동이 표시됩니다. 타임라인은 사용자 활동을 이용하여 작업해 온 항목을 시간순으로 보여주는 풍부한 작업 보기입니다. 장치 간에 어떤 작업을 하고 있었는지에 대한 정보도 포함할 수 있습니다.

![Windows 타임라인 이미지](images/timeline.png)

마찬가지로 휴대폰을 Windows PC에 연결하면 iOS나 Android 장치에서 이전에 하고 있었던 작업을 계속 할 수 있습니다.

**UserActivity**를 사용자가 앱에서 하고 있었던 특정한 작업으로 생각합니다. 예를 들어, RSS 판독기를 사용하는 경우 **UserActivity**는 읽고 있는 중인 피드일 수 있습니다. 게임을 플레이하고 있는 경우 **UserActivity**는 플레이 중인 레벨일 수 있습니다. 음악 앱을 듣고 있는 경우 **UserActivity**는 듣고 있는 재생 목록일 수 있습니다. 문서 작업을 하고 있는 경우 **UserActivity**는 작업을 마지막으로 종료한 지점 등일 수 있습니다.  요약하자면 **UserActivity**는 사용자가 수행 중이던 작업을 다시 시작할 수 있는 앱 내의 목적지를 나타냅니다.

[UserActivity.CreateSession](https://docs.microsoft.com/uwp/api/windows.applicationmodel.useractivities.useractivity.createsession)을 호출하여 **UserActivity**를 사용하는 경우 시스템은 해당 **UserActivity**의 시작 및 종료 시간을 표시하는 기록 레코드를 만듭니다. 시간 경과에 따라 해당 **UserActivity**를 다시 사용하면서 해당 활동에 대한 여러 기록 레코드가 기록됩니다.

## <a name="add-user-activities-to-your-app"></a>앱에 사용자 활동 추가

[UserActivity](https://docs.microsoft.com/uwp/api/windows.applicationmodel.useractivities.useractivity)는 Windows에서의 사용자 사용 단위입니다. UserActivity는 활동이 속한 앱을 활성화하는 데 사용되는 URI, 시각적 개체 및 활동을 설명하는 메타데이터의 세 부분으로 구성됩니다.

1. [ActivationUri](https://docs.microsoft.com/uwp/api/windows.applicationmodel.useractivities.useractivity.activationuri#Windows_ApplicationModel_UserActivities_UserActivity_ActivationUri)를 사용하여 특정 컨텍스트로 응용 프로그램을 다시 시작합니다. 일반적으로 이 링크는 스키마에 대한 프로토콜 처리기 양식(예: “my-app://page2?action=edit”) 또는 AppUriHandler 양식(예: http://constoso.com/page2?action=edit)을 사용합니다.
2. [VisualElements](https://docs.microsoft.com/uwp/api/windows.applicationmodel.useractivities.useractivity.visualelements)는 사용자가 제목, 설명 또는 적응형 카드 요소로 활동을 시각적으로 식별할 수 있는 클래스를 노출합니다.
3. 마지막으로 [Content](https://docs.microsoft.com/uwp/api/windows.applicationmodel.useractivities.useractivityvisualelements.content#Windows_ApplicationModel_UserActivities_UserActivityVisualElements_Content)에는 특정 컨텍스트에서 활동을 그룹화하고 검색하는 데 사용할 수 있는 활동 메타데이터를 저장할 수 있습니다. 여기서는 종종 [http://schema.org](http://schema.org) 데이터 양식을 사용합니다.

앱에 **UserActivity**를 추가하려면 다음과 같이 합니다.

1. 앱에서 사용자 컨텍스트(예: 페이지 탐색, 새 게임 레벨 등)가 변경될 때 **UserActivity** 개체를 생성합니다.
2. 다음 필수 필드 세트로 **UserActivity** 개체를 채웁니다. [ActivityId](https://docs.microsoft.com/uwp/api/windows.applicationmodel.useractivities.useractivity.activityid#Windows_ApplicationModel_UserActivities_UserActivity_ActivityId), [ActivationUri](https://docs.microsoft.com/uwp/api/windows.applicationmodel.useractivities.useractivity.activationuri) 및 [UserActivity.VisualElements.DisplayText](https://docs.microsoft.com/uwp/api/windows.applicationmodel.useractivities.useractivityvisualelements.displaytext#Windows_ApplicationModel_UserActivities_UserActivityVisualElements_DisplayText).
3. **UserActivity**가 다시 활성화할 수 있도록 앱에 사용자 지정 스키마 처리기를 추가합니다.

단 몇 줄의 코드만으로 **UserActivity**를 앱에 통합할 수 있습니다. 예를 들어 MainPage 클래스 내의 MainPage.xaml.cs에서 이 코드를 가정해 보세요(참고: `using Windows.ApplicationModel.UserActivities;` 가정).

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

위에 있는 `GenerateActivityAsync()` 메서드의 첫 번째 줄은 사용자의 [UserActivityChannel](https://docs.microsoft.com/uwp/api/windows.applicationmodel.useractivities.useractivitychannel)을 가져옵니다. 이것은 이 앱의 활동이 게시될 피드입니다. 다음 줄은 `MainPage`라는 활동 채널을 쿼리합니다.

* 앱은 사용자가 앱의 특정한 위치에 있을 때마다 동일한 ID가 생성되는 방식으로 활동 이름을 지정해야 합니다. 예를 들어 앱이 페이지 기반인 경우 페이지의 식별자를 사용합니다. 앱이 문서 기반인 경우에는 문서 이름(또는 이름의 해시)을 사용합니다.
* 동일한 ID를 가진 피드에 기존 활동이 있는 경우 해당 활동은 `UserActivity.State`가 [Published](https://docs.microsoft.com/uwp/api/windows.applicationmodel.useractivities.useractivitystate))로 설정된 채널에서 반환됩니다. 해당 이름의 활동이 없는 경우 `UserActivity.State`가 **New**로 설정된 새 활동이 반환됩니다.
* 활동의 범위는 앱으로 지정됩니다. 사용자의 활동 ID가 다른 앱의 ID와 충돌하는 것에 대해 걱정할 필요가 없습니다.

**UserActivity**를 가져오거나 만든 후에 `UserActivity.VisualElements.DisplayText` 및 `UserActivity.ActivationUri`라는 두 개의 필수 필드를 지정합니다.

그런 다음 [SaveAsync](https://docs.microsoft.com/uwp/api/windows.applicationmodel.useractivities.useractivity.saveasync)를 호출하여 **UserActivity**를 저장하고, 마지막으로 [UserActivitySession](https://docs.microsoft.com/uwp/api/windows.applicationmodel.useractivities.useractivitysession)을 반환하는 [CreateSession](https://docs.microsoft.com/uwp/api/windows.applicationmodel.useractivities.useractivity.createsession)을 저장합니다. **UserActivitySession**은 사용자가 실제로 **UserActivity**를 사용할 때 관리할 수 있는 개체입니다. 예를 들어 사용자가 페이지에서 나가면 **UserActivitySession**에서 `Dispose()`를 호출해야 합니다. 위의 예에서는 `CreateSession()`을 호출하기 전에 `_currentActivity`에서도 `Dispose()`를 호출합니다. 이는 `_currentActivity`를 페이지의 멤버 필드로 만들었으며 새 활동을 시작하기 전에 기존 활동을 중지하기를 원하기 때문입니다(참고: `?`는 멤버 액세스를 수행하기 전에 널 테스트를 수행하는 [조건부 널 연산자](https://docs.microsoft.com/en-us/dotnet/csharp/language-reference/operators/null-conditional-operators)임).

이 경우 `ActivationUri`는 사용자 지정 스키마이므로 응용 프로그램 매니페스트에도 프로토콜을 등록해야 합니다. 이는 Package.appmanifest XML 파일 또는 디자이너를 사용하여 수행됩니다.

디자이너를 사용하여 변경하려면 프로젝트에 있는 Package.appmanifest 파일을 두 번 클릭하여 디자이너를 시작하고 **선언** 탭을 선택한 후 **프로토콜** 정의를 추가합니다. **이름** 속성만 입력해야 합니다. 이 속성은 위에서 지정한 URI, `my-app`과 일치해야 합니다.

이제는 프로토콜에 의해 앱이 활성화될 때 수행할 작업을 앱에 알리는 코드를 작성해야 합니다. 다음과 같이 App.xaml.cs의 `OnActivated` 메서드를 재정의하여 URI를 기본 페이지에 전달합니다.

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

이 코드는 앱이 프로토콜을 통해 활성화되었는지 여부를 검색합니다. 앱이 프로토콜을 통해 활성화된 경우 활성화되는 작업을 다시 시작하기 위해 앱이 수행해야 하는 작업을 확인합니다. 간단한 앱이기 때문에 이 앱이 다시 시작하는 유일한 활동은 앱이 표시될 때 보조 페이지에서 시작하게 하는 것입니다.

## <a name="use-adaptive-cards-to-improve-the-timeline-experience"></a>적응형 카드를 사용하여 타임라인 환경 개선

사용자 활동은 Cortana 및 타임라인에 표시됩니다. 활동이 타임라인에 표시되는 경우 [적응형 카드](http://adaptivecards.io/) 프레임워크를 사용하여 활동을 표시합니다. 각 활동에 대한 적응형 카드를 제공하지 않으면 타임라인은 응용 프로그램 이름과 아이콘, 제목 필드 및 선택적 설명 필드를 기반으로 하여 단순한 활동 카드를 자동으로 만듭니다. 아래는 적응형 카드 페이로드 및 생성되는 카드의 예입니다.

![적응형 카드](images/adaptivecard.png)]

적응형 카드 페이로드 JSON 문자열의 예는 다음과 같습니다.

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

다음과 같이 **UserActivity**에 적응형 카드 페이로드를 JSON 문자열로 추가합니다.

```csharp
activity.VisualElements.Content = 
Windows.UI.Shell.AdaptiveCardBuilder.CreateAdaptiveCardFromJson(jsonCardText); // where jsonCardText is a JSON string that represents the card
```

## <a name="cross-platform-and-service-to-service-integration"></a>교차 플랫폼 및 서비스 간 통합

앱이 교차 플랫폼(예: Android 및 iOS)에서 실행되거나 클라우드에서 사용자 상태를 유지 관리하는 경우 [Microsoft Graph](https://developer.microsoft.com/graph/)를 통해 UserActivities를 게시할 수 있습니다.
Microsoft 계정을 사용하여 응용 프로그램 또는 서비스를 인증하면 위에서 설명된 대로 동일한 데이터로 두 개의 간단한 REST 호출만 사용하여 [활동](https://developer.microsoft.com/graph/docs/api-reference/beta/api/projectrome_put_activity) 및 [기록](https://developer.microsoft.com/graph/docs/api-reference/beta/resources/projectrome_historyitem) 개체를 생성합니다.

## <a name="summary"></a>요약

[UserActivity](https://docs.microsoft.com/uwp/api/windows.applicationmodel.useractivities) API를 사용하여 앱을 타임라인 및 Cortana에 표시할 수 있습니다.
* [Windows 개발자 센터](https://docs.microsoft.com/uwp/api/windows.applicationmodel.useractivities)에서 **UserActivity** API에 대해 자세히 알아보세요.
* [샘플 코드](https://github.com/Microsoft/project-rome)를 확인합니다.
* [더 정교한 적응형 카드](http://adaptivecards.io/)를 참조하세요.
* [Microsoft Graph](https://developer.microsoft.com/graph/)를 통해 iOS, Android 또는 웹 서비스에서 **UserActivity**를 게시합니다.
* [GitHub의 프로젝트 로마](https://github.com/Microsoft/project-rome)에 대해 자세히 알아보세요.

## <a name="key-apis"></a>주요 API

* [UserActivities 네임 스페이스](https://docs.microsoft.com/uwp/api/windows.applicationmodel.useractivities)

## <a name="related-topics"></a>관련 항목

* [사용자 활동 (프로젝트 "로마" 문서)](https://docs.microsoft.com/windows/project-rome/user-activities/)
* [적응형 카드](https://docs.microsoft.com/adaptive-cards/)
* [적응형 카드 비주얼라이저, 샘플](http://adaptivecards.io/)
* [URI 활성화 처리](https://docs.microsoft.com/windows/uwp/launch-resume/handle-uri-activation)
* [Microsoft Graph, 활동 피드 및 적응형 카드를 사용하여 모든 플랫폼에서 고객과 소통](https://channel9.msdn.com/Events/Connect/2017/B111)
* [Microsoft Graph](https://developer.microsoft.com/graph/)