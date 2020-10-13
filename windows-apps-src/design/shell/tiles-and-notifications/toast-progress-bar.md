---
description: 알림 메시지 내에서 진행률 표시줄을 사용 하 여 장기 실행 작업의 상태를 사용자에 게 전달 합니다.
title: 알림 진행률 표시줄 및 데이터 바인딩
label: Toast progress bar and data binding
template: detail.hbs
ms.date: 12/07/2017
ms.topic: article
keywords: windows 10, uwp, 알림, 진행률 표시줄, 알림 진행률 표시줄, 알림, 알림 데이터 바인딩
ms.localizationpriority: medium
ms.openlocfilehash: 8df76af7dd26792d8d1af0fa8641d76e007ef8e3
ms.sourcegitcommit: 140bbbab0f863a7a1febee85f736b0412bff1ae7
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/13/2020
ms.locfileid: "91984529"
---
# <a name="toast-progress-bar-and-data-binding"></a>알림 진행률 표시줄 및 데이터 바인딩

알림 메시지 내에서 진행률 표시줄을 사용 하면 다운로드, 비디오 렌더링, 연습 목표 등과 같은 장기 실행 작업의 상태를 사용자에 게 전달할 수 있습니다.

> [!IMPORTANT]
> **작성자 업데이트 및 알림 라이브러리 1.4.0 필요**합니다. 알림을에서 진행률 표시줄을 사용 하려면 SDK 15063을 대상으로 하 고 빌드 15063 이상을 실행 해야 합니다. 1.4.0 이상 버전의 [UWP Community Toolkit Notification NuGet library](https://www.nuget.org/packages/Microsoft.Toolkit.Uwp.Notifications/) 를 사용 하 여 알림 콘텐츠에서 진행률 표시줄을 생성 해야 합니다.

알림 내부의 진행률 표시줄은 "확정 되지 않음" (특정 값이 없거나 작업이 발생 했음을 나타내는 점에 애니메이션 됨) 또는 "활성화 상태의" (막대의 특정 백분율이 60%와 같이 채워짐) 일 수 있습니다.

> **중요 한 api**: [notificationdata 클래스](/uwp/api/windows.ui.notifications.notificationdata), [to notification. Update 메서드](/uwp/api/Windows.UI.Notifications.ToastNotifier.Update), [to notification 클래스](/uwp/api/Windows.UI.Notifications.ToastNotification)

> [!NOTE]
> 바탕 화면 에서만 알림 표시줄의 진행률 표시줄을 지원 합니다. 다른 장치에서 진행률 표시줄이 알림에서 삭제 됩니다.

아래 그림은 해당 속성을 모두 포함 하는 활성화 상태의 진행률 표시줄을 보여 줍니다.

<img alt="Toast with progress bar properties labeled" src="images/toast-progressbar-annotated.png" width="626"/>

| 속성 | 유형 | 필수 | Description |
|---|---|---|---|
| **제목** | string 또는 [Bindablestring](toast-schema.md#bindablestring) | false | 선택적 제목 문자열을 가져오거나 설정 합니다. 데이터 바인딩을 지원 합니다. |
| **값** | double 또는 [AdaptiveProgressBarValue](toast-schema.md#adaptiveprogressbarvalue) 또는 [BindableProgressBarValue](toast-schema.md#bindableprogressbarvalue) | false | 진행률 표시줄의 값을 가져오거나 설정 합니다. 데이터 바인딩을 지원 합니다. 기본값은 0입니다. 0.0에서 1.0, 또는 사이의 double 일 수 있습니다 `AdaptiveProgressBarValue.Indeterminate` `new BindableProgressBarValue("myProgressValue")` . |
| **ValueStringOverride** | string 또는 [Bindablestring](toast-schema.md#bindablestring) | false | 기본 백분율 문자열 대신 표시할 선택적 문자열을 가져오거나 설정 합니다. 제공 되지 않은 경우 "70%"와 같은 항목이 표시 됩니다. |
| **상태** | string 또는 [Bindablestring](toast-schema.md#bindablestring) | true | 왼쪽의 진행률 표시줄 아래에 표시 되는 상태 문자열 (필수)을 가져오거나 설정 합니다. 이 문자열은 "다운로드 중 ..."과 같은 작업의 상태를 반영 해야 합니다. 또는 "설치 중 ..." |


위에서 본 알림을 생성 하는 방법은 다음과 같습니다.

#### <a name="builder-syntax"></a>[작성기 구문](#tab/builder-syntax)

```csharp
new ToastContentBuilder()
    .AddText("Downloading your weekly playlist...")
    .AddVisualChild(new AdaptiveProgressBar()
    {
        Title = "Weekly playlist",
        Value = 0.6,
        ValueStringOverride = "15/26 songs",
        Status = "Downloading..."
    });
```

#### <a name="xml"></a>[XML](#tab/xml)

```xml
<toast>
    <visual>
        <binding template="ToastGeneric">
            <text>Downloading your weekly playlist...</text>
            <progress
                title="Weekly playlist"
                value="0.6"
                valueStringOverride="15/26 songs"
                status="Downloading..."/>
        </binding>
    </visual>
</toast>
```

---

그러나 실제로 "라이브" 상태가 되도록 진행률 표시줄의 값을 동적으로 업데이트 해야 합니다. 데이터 바인딩을 사용 하 여 알림 메시지를 업데이트 하면이 작업을 수행할 수 있습니다.


## <a name="using-data-binding-to-update-a-toast"></a>데이터 바인딩을 사용 하 여 알림 업데이트

데이터 바인딩을 사용 하려면 다음 단계를 수행 해야 합니다.

1. 데이터 바인딩된 필드를 활용 하는 알림 콘텐츠 생성
2. 태그 (및 필요에 따라 **그룹**)를 **to태그 알림에** 할당 합니다. **Tag**
3. **To notification** 에서 초기 **데이터** 값 정의
4. 알림 보내기
5. **태그** 및 **그룹** 을 활용 하 여 **데이터** 값을 새 값으로 업데이트

다음 코드 조각에서는 1-4 단계를 보여 줍니다. 다음 코드 조각에서는 알림 **데이터** 값을 업데이트 하는 방법을 보여 줍니다.

```csharp
using Windows.UI.Notifications;
using Microsoft.Toolkit.Uwp.Notifications;
 
public void SendUpdatableToastWithProgress()
{
    // Define a tag (and optionally a group) to uniquely identify the notification, in order update the notification data later;
    string tag = "weekly-playlist";
    string group = "downloads";
 
    // Construct the toast content with data bound fields
    var content = new ToastContentBuilder()
        .AddText("Downloading your weekly playlist...")
        .AddVisualChild(new AdaptiveProgressBar()
        {
            Title = "Weekly playlist",
            Value = new BindableProgressBarValue("progressValue"),
            ValueStringOverride = new BindableString("progressValueString"),
            Status = new BindableString("progressStatus")
        })
        .GetToastContent();
 
    // Generate the toast notification
    var toast = new ToastNotification(content.GetXml());
 
    // Assign the tag and group
    toast.Tag = tag;
    toast.Group = group;
 
    // Assign initial NotificationData values
    // Values must be of type string
    toast.Data = new NotificationData();
    toast.Data.Values["progressValue"] = "0.6";
    toast.Data.Values["progressValueString"] = "15/26 songs";
    toast.Data.Values["progressStatus"] = "Downloading...";
 
    // Provide sequence number to prevent out-of-order updates, or assign 0 to indicate "always update"
    toast.Data.SequenceNumber = 1;
 
    // Show the toast notification to the user
    ToastNotificationManager.CreateToastNotifier().Show(toast);
}
```

그런 다음 **데이터** 값을 변경 하려는 경우에는 [**Update**](/uwp/api/Windows.UI.Notifications.ToastNotifier.Update) 메서드를 사용 하 여 전체 알림 페이로드를 다시 생성 하지 않고 새 데이터를 제공 합니다.

```csharp
using Windows.UI.Notifications;
 
public void UpdateProgress()
{
    // Construct a NotificationData object;
    string tag = "weekly-playlist";
    string group = "downloads";
 
    // Create NotificationData and make sure the sequence number is incremented
    // since last update, or assign 0 for updating regardless of order
    var data = new NotificationData
    {
        SequenceNumber = 2
    };

    // Assign new values
    // Note that you only need to assign values that changed. In this example
    // we don't assign progressStatus since we don't need to change it
    data.Values["progressValue"] = "0.7";
    data.Values["progressValueString"] = "18/26 songs";

    // Update the existing notification's data by using tag/group
    ToastNotificationManager.CreateToastNotifier().Update(data, tag, group);
}
```

전체 알림을 대체 하는 대신 **Update** 메서드를 사용 하면 알림 메시지가 작업 센터의 동일한 위치에 유지 되 고 위나 아래로 이동 하지 않습니다. 진행률 표시줄이 채워지면 몇 초 마다 알림 센터의 맨 위로 이동 하는 경우 사용자에 게 매우 혼란 스 러 울 것입니다.

업데이트 **메서드는** 열거형 ( [**NotificationUpdateResult**](/uwp/api/windows.ui.notifications.notificationupdateresult))을 반환 합니다 .이 열거형을 사용 하면 업데이트가 성공 했는지 여부 또는 알림을 찾을 수 없는지 여부를 알 수 있습니다. 즉, 사용자가 알림을 해제 했 고 업데이트 보내기를 중지 해야 합니다. 진행 작업이 완료 될 때까지 다른 알림을 팝 하지 않는 것이 좋습니다 (예: 다운로드가 완료 된 경우).


## <a name="elements-that-support-data-binding"></a>데이터 바인딩을 지 원하는 요소
알림 메시지의 다음 요소는 데이터 바인딩을 지원 합니다.

- **AdaptiveProgress** 의 모든 속성
- 최상위 **AdaptiveText** 요소의 **Text** 속성


## <a name="update-or-replace-a-notification"></a>알림 업데이트 또는 바꾸기

Windows 10 이후에는 동일한 **태그** 및 **그룹**을 사용 하 여 새 알림을 전송 하 여 항상 알림을 **바꿀** 수 있습니다. 알림 메시지를 **교체** 하 고 알림 데이터를 **업데이트** 하는 경우의 차이점은 무엇 인가요?

| | 대체 | 업데이트 |
| -- | -- | --
| **작업 센터의 위치** | 알림 메시지를 동작 센터의 맨 위로 이동 합니다. | 알림 메시지를 작업 센터 내에 그대로 둡니다. |
| **콘텐츠 수정** | 알림의 모든 콘텐츠/레이아웃을 완전히 변경할 수 있습니다. | 데이터 바인딩 (진행률 표시줄 및 최상위 텍스트)을 지 원하는 속성만 변경할 수 있습니다. |
| **Reappearing 팝업** | **SuppressPopup** 을로 설정 `false` 하거나 true로 설정 하 여 작업 센터에 자동으로 전송 하는 경우 알림 팝업으로 다시 나타날 수 있습니다. | 는 팝업으로 다시 표시 되지 않습니다. 알림 데이터는 알림 센터 내에서 자동으로 업데이트 됩니다. |
| **사용자 해제 됨** | 사용자가 이전 알림을 해제 했는지 여부에 관계 없이 교체 알림이 항상 전송 됩니다. | 사용자가 알림을 해제 한 경우 알림 업데이트는 실패 합니다. |

일반적으로 **업데이트는 다음에 유용** 합니다.

- 짧은 시간에 자주 변경 되 고 사용자의 주의 앞으로 가져올 필요가 없는 정보
- 50%에서 65%로 변경 하는 것과 같이 알림 콘텐츠에 대 한 미묘한 변경

업데이트 시퀀스 (예: 파일 다운로드)가 완료 된 후에는 종종 최종 단계로 대체 하는 것이 좋습니다.

- 최종 알림에서 진행률 표시줄의 제거, 새 단추 추가 등의 레이아웃 변경 내용이 있을 수 있습니다.
- 사용자가 다운로드를 시청 하는 것을 고려 하지 않고 작업이 완료 될 때 팝업 알림 메시지에 대 한 알림을 받을 수 있으므로 보류 중인 진행 알림을 해제 했을 수 있습니다.


## <a name="related-topics"></a>관련 항목

- [GitHub의 전체 코드 샘플](https://github.com/WindowsNotifications/quickstart-toast-progress-bar)
- [알림 콘텐츠 설명서](adaptive-interactive-toasts.md)