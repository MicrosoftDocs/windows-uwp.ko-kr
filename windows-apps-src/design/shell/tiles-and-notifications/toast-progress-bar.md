---
author: andrewleader
Description: Learn how to use a progress bar within your toast notification.
title: 알림 진행률 표시줄 및 데이터 바인딩
label: Toast progress bar and data binding
template: detail.hbs
ms.author: mijacobs
ms.date: 12/7/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: Windows 10, uwp, 알림, 진행률 표시줄, 알림 진행률 표시줄, 알림, 알림 데이터 바인딩
ms.localizationpriority: medium
ms.openlocfilehash: b99c2479bef3c10ecc82707e475f49fd2b9014ec
ms.sourcegitcommit: 106aec1e59ba41aae2ac00f909b81bf7121a6ef1
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/15/2018
ms.locfileid: "4616879"
---
# <a name="toast-progress-bar-and-data-binding"></a>알림 진행률 표시줄 및 데이터 바인딩

알림 메시지에서 진행률 표시줄을 사용하면 다운로드, 비디오 렌더링, 운동 목표 등과 같은 장기 실행 작업의 상태를 사용자에게 전달할 수 있습니다.

> [!IMPORTANT]
> **Creators Update 및 알림 라이브러리 1.4.0 필요**: 알림에서 진행률 표시줄을 사용하려면 SDK 15063을 대상으로 지정하고 빌드 15063 이상을 실행해야 합니다. 버전 1.4.0 이상의 [UWP 커뮤니티 도구 키트 알림 NuGet 라이브러리](https://www.nuget.org/packages/Microsoft.Toolkit.Uwp.Notifications/)를 사용해야만 알림의 콘텐츠에 대한 진행률 표시줄을 생성할 수 있습니다.

알림 내부의 진행률 표시줄은 '미확정'(특정 값 없음, 움직이는 점이 작업이 일어나고 있음을 나타냄) 또는 '확정'(표시줄의 특정 비율이 60%와 같이 채워짐)일 수 있습니다.

> **중요 API**: [NotificationData 클래스](https://docs.microsoft.com/uwp/api/windows.ui.notifications.notificationdata), [ToastNotifier.Update 메서드](https://docs.microsoft.com/uwp/api/Windows.UI.Notifications.ToastNotifier.Update), [ToastNotification 클래스](https://docs.microsoft.com/uwp/api/Windows.UI.Notifications.ToastNotification)

> [!NOTE]
> 데스크톱은 알림 메시지에서 진행률 표시줄만을 지원합니다. 다른 디바이스에서는 알림의 진행률 표시줄이 삭제됩니다.

아래 그림은 레이블이 지정된 모든 해당 속성을 사용한 확정 진행률 표시줄을 보여줍니다.

<img alt="Toast with progress bar properties labeled" src="images/toast-progressbar-annotated.png" width="626"/>

| 속성 | 형식 | 필수 | 설명 |
|---|---|---|---|
| **제목** | 문자열 또는 [BindableString](toast-schema.md#bindablestring) | false | 선택적 제목 문자열을 가져오거나 설정합니다. 데이터 바인딩을 지원합니다. |
| **값** | 이중 또는 [AdaptiveProgressBarValue](toast-schema.md#adaptiveprogressbarvalue) 또는 [BindableProgressBarValue](toast-schema.md#bindableprogressbarvalue) | false | 진행률 표시줄의 값을 가져오거나 설정합니다. 데이터 바인딩을 지원합니다. 기본값은 0입니다. 0.0과 1.0, `AdaptiveProgressBarValue.Indeterminate` 또는 `new BindableProgressBarValue("myProgressValue")` 사이에서 이중일 수 있습니다. |
| **ValueStringOverride** | 문자열 또는 [BindableString](toast-schema.md#bindablestring) | false | 기본 백분율 문자열 대신 표시할 선택적 문자열을 가져오거나 설정합니다. 이것을 제공하지 않는 경우 '70%'와 같은 내용이 표시됩니다. |
| **Status** | 문자열 또는 [BindableString](toast-schema.md#bindablestring) | true | 왼쪽의 진행률 표시줄 아래에 표시되는 상태 문자열(필수)을 가져오거나 설정합니다. 이 문자열은 '다운로드 중...' 또는 '설치 중...'과 같은 작업의 상태를 반영해야 합니다. |


위에서 확인한 알림을 생성하는 방법은 다음과 같습니다.

```csharp
ToastContent content = new ToastContent()
{
    Visual = new ToastVisual()
    {
        BindingGeneric = new ToastBindingGeneric()
        {
            Children =
            {
                new AdaptiveText()
                {
                    Text = "Downloading your weekly playlist..."
                },
 
                new AdaptiveProgressBar()
                {
                    Title = "Weekly playlist",
                    Value = 0.6,
                    ValueStringOverride = "15/26 songs",
                    Status = "Downloading..."
                }
            }
        }
    }
};
```

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

그러나 실제로 '라이브' 상태가 되도록 진행률 표시줄의 값을 동적으로 업데이트해야 합니다. 이 작업은 알림을 업데이트하기 위해 데이터 바인딩을 사용하여 수행할 수 있습니다.


## <a name="using-data-binding-to-update-a-toast"></a>데이터 바인딩을 사용하여 알림 업데이트

데이터 바인딩을 사용하는 단계는 다음과 같습니다.

1. 데이터가 바인딩된 필드를 사용하는 알린 콘텐츠 생성
2. **Tag**(그리고 선택적으로 **Group**)을 **ToastNotification**에 할당
3. **ToastNotification**에서 초기 **Data** 값 정의
4. 알림 보내기
5. **Tag** 및 **Group**을 사용하여 새로운 값으로 **Data** 값 업데이트

다음 코드 조각은 1~4단계를 보여줍니다. 다음 조각은 알림 **Data** 값을 업데이트하는 방법을 보여줍니다.

```csharp
using Windows.UI.Notifications;
using Microsoft.Toolkit.Uwp.Notifications;
 
public void SendUpdatableToastWithProgress()
{
    // Define a tag (and optionally a group) to uniquely identify the notification, in order update the notification data later;
    string tag = "weekly-playlist";
    string group = "downloads";
 
    // Construct the toast content with data bound fields
    var content = new ToastContent()
    {
        Visual = new ToastVisual()
        {
            BindingGeneric = new ToastBindingGeneric()
            {
                Children =
                {
                    new AdaptiveText()
                    {
                        Text = "Downloading your weekly playlist..."
                    },
    
                    new AdaptiveProgressBar()
                    {
                        Title = "Weekly playlist",
                        Value = new BindableProgressBarValue("progressValue"),
                        ValueStringOverride = new BindableString("progressValueString"),
                        Status = new BindableString("progressStatus")
                    }
                }
            }
        }
    };
 
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

그런 다음 **Data** 값을 변경할 때에는 [**Update**](https://docs.microsoft.com/uwp/api/Windows.UI.Notifications.ToastNotifier.Update) 메서드를 사용하여 전체 알림 페이로드를 다시 생성하지 말고 새로운 데이터를 제공합니다.

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

전체 알림을 교체하지 않고 **Update** 메서드를 사용하면 알림 메시지가 알림 센터의 동일한 위치에 그대로 유지되고 위아래로 움직이지 않습니다. 진행률 표시줄이 채워지는 동안 알림이 몇 초마다 알림 센터 상단으로 이동하면 사용자에게 매우 혼란스러울 수 있습니다!

**Update** 메서드는 열거형, [**NotificationUpdateResult**](https://docs.microsoft.com/uwp/api/windows.ui.notifications.notificationupdateresult)를 반환하며, 이것을 통해 업데이트 성공 여부나 알림 유무를 알 수 있습니다(즉, 사용자가 알림을 무시하고 업데이트를 중지할 수 있음) 진행 작업이 완료될 때까지(예: 다운로드가 완료 될 때 등) 다른 알림을 표시하지 않는 것이 좋습니다.


## <a name="elements-that-support-data-binding"></a>데이터 바인딩을 지원하는 요소
데이터 바인딩을 지원하는 알림 메시지의 다음 요소

- **AdaptiveProgress**의 모든 속성
- 최상위 **AdaptiveText** 요소의 **Text** 속성


## <a name="update-or-replace-a-notification"></a>알림 업데이트 또는 바꾸기

Windows 10부터 동일한 **Tag** 및 **Group**을 사용하여 새로운 알림을 보내 항상 알림을 **바꿀** 수 있습니다. 알림 **바꾸기**와 알림의 데이터 **업데이트**의 차이는?

| | 바꾸기 | 업데이트 |
| -- | -- | --
| **알림 센터의 위치** | 알림을 알림 센터 상단으로 이동 | 알림 센터 내에서 알림을 제자리에 둡니다. |
| **콘텐츠 수정** | 전체 알림 콘텐츠/레이아웃을 완전히 변경할 수 있습니다. | 데이터 바인딩을 지원하는 속성만 변경할 수 있습니다(진행률 표시줄 및 최상위 텍스트). |
| **팝업으로 다시 표시** | **SuppressPopup**을 `false`로 설정하는 경우 알림 팝업으로 다시 표시할 수 있습니다(또는 true로 설정하여 알림 센터에 무음으로 보냄). | 팝업으로 다시 표시하지 않습니다. 즉, 알림의 데이터는 알림 센터 내에서 무음으로 업데이트됩니다. |
| **사용자 해제됨** | 사용자가 이전 알림을 해제했는지에 상관 없이 교체 알림이 항상 발송됩니다. | 사용자가 알림을 해제하면 알림 업데이트에 실패합니다. |

일반적으로 **업데이트가 유용한 경우...**

- 단기간에 자주 변경되고 사용자의 주의를 바로 끌 필요가 없는 정보
- 50%에서 65%로의 변경과 같이 미미한 변경 내용

흔히 업데이트 시퀀스가 완료된 후(예: 파일을 다운로드 완료 등) 마지막 단계에서 바꾸는 것이 좋으며, 그 이유는 다음과 같습니다.

- 최종 알림에는 진행률 표시줄 제거, 새 단추 추가 등과 같은 큰 레이아웃 변화가 있을 수 있습니다.
- 사용자는 다운로드를 지켜보는 일에는 신경 쓰지 않지만 작업이 완료되면 팝업 알림을 받기를 원하기 때문에 보류 중인 진행 알림을 무시할 수 있습니다.


## <a name="related-topics"></a>관련 항목

- [GitHub의 전체 코드 샘플](https://github.com/WindowsNotifications/quickstart-toast-progress-bar)
- [알림 콘텐츠 설명서](adaptive-interactive-toasts.md)