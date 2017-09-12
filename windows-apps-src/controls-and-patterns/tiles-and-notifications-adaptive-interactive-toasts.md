---
author: mijacobs
Description: "적응형 및 대화형 알림 메시지를 사용하면 더 많은 콘텐츠, 선택적 인라인 이미지 및 선택적 사용자 조작이 포함된 유연한 팝업 알림을 만들 수 있습니다."
title: "적응형 및 대화형 알림 메시지"
ms.assetid: 1FCE66AF-34B4-436A-9FC9-D0CF4BDA5A01
label: Adaptive and interactive toast notifications
template: detail.hbs
ms.author: mijacobs
ms.date: 05/19/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: Windows 10 uwp
ms.openlocfilehash: c8e77773b9118c3177dc958ddc7b51d32a452fa5
ms.sourcegitcommit: 9d1ca16a7edcbbcae03fad50a4a10183a319c63a
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 06/09/2017
---
# <a name="adaptive-and-interactive-toast-notifications"></a>적응형 및 대화형 알림 메시지

<link rel="stylesheet" href="https://az835927.vo.msecnd.net/sites/uwp/Resources/css/custom.css">

적응형 및 대화형 알림 메시지를 사용하여 텍스트, 이미지 및 단추/입력을 포함한 유연한 알림을 만들 수 있습니다.

> **중요 API**: [UWP 커뮤니티 도구 키트 알림 NuGet 패키지](https://www.nuget.org/packages/Microsoft.Toolkit.Uwp.Notifications/)

> [!NOTE]
> Windows 8.1 및 Windows Phone 8.1의 레거시 템플릿을 보려면 [레거시 알림 템플릿 카탈로그](https://msdn.microsoft.com/library/windows/apps/hh761494)를 참조하세요.


## <a name="getting-started"></a>시작

**알림 라이브러리 설치.** XML 대신 C#을 사용하여 알림을 생성하려면 [Microsoft.Toolkit.Uwp.Notifications](https://www.nuget.org/packages/Microsoft.Toolkit.Uwp.Notifications/)("notifications uwp" 검색)라는 NuGet 패키지를 설치합니다. 이 문서에서 제공하는 C# 샘플은 NuGet 패키지 버전 1.0.0을 사용합니다.

**알림 시각화 도우미 설치.** 이 무료 UWP 앱은 Visual Studio의 XAML 편집기/디자인 뷰와 비슷하게, 알림을 편집할 때 시각적 미리 보기를 곧바로 제공하여 대화형 알림 메시지를 디자인하는 데 도움이 됩니다. [이 블로그 게시물](http://blogs.msdn.com/b/tiles_and_toasts/archive/2015/09/22/introducing-notifications-visualizer-for-windows-10.aspx)에서 자세한 정보를 얻을 수 있고 [여기](https://www.microsoft.com/store/apps/notifications-visualizer/9nblggh5xsl1)서 알림 시각화 도우미를 다운로드할 수 있습니다.


## <a name="sending-a-toast-notification"></a>알림 메시지 보내기

알림을 보내는 방법은 [Send local toast(로컬 알림 보내기)](tiles-and-notifications-send-local-toast.md)를 참조하세요. 이 설명서에는 알림 콘텐츠 생성만 다룹니다.


## <a name="toast-notification-structure"></a>알림 메시지 구조

알림 메시지는 알림 식별에 사용할 수 있는 Tag/Group과 같은 몇 가지 데이터 속성과 *알림 콘텐츠*의 조합입니다.

알림 콘텐츠의 주요 구성 요소는 다음과 같습니다.
* **실행**: 사용자가 알림을 클릭할 때 앱에 다시 전달될 인수를 정의하여 알림이 표시 중인 올바른 콘텐츠로 딥 링크할 수 있게 합니다. 자세한 내용은 [Send local toast(로컬 알림 보내기)](tiles-and-notifications-send-local-toast.md)를 참조하세요.
* **시각적 개체**: 알림의 시각적 부분으로 텍스트, 이미지 및 앱 로고가 들어 있는 일반 바인딩을 포함합니다.
* **작업**: 알림의 대화형 부분으로 입력과 작업을 포함합니다.
* **오디오**: 알림이 사용자에게 표시될 때 재생되는 오디오를 제어합니다.

알림 콘텐츠를 원시 XML에 정의되지만 [NuGet 라이브러리](https://www.nuget.org/packages/Microsoft.Toolkit.Uwp.Notifications/)를 사용하여 알림 콘텐츠 생성을 위한 C# 또는 C++ 개체 모델을 구할 수 있습니다. 알림 콘텐츠 내의 모든 것이 이 문서에 설명되어 있습니다.

```csharp
ToastContent content = new ToastContent()
{
    Launch = "app-defined-string",
 
    Visual = new ToastVisual()
    {
        BindingGeneric = new ToastBindingGeneric() { ... }
    },
 
    Actions = new ToastActionsCustom() { ... },
 
    Audio = new ToastAudio() { ... }
};
```

```xml
<toast launch="app-defined-string">

  <visual>
    <binding template="ToastGeneric">
      ...
    </binding>
  </visual>

  <actions>
    ...
  </actions>

  <audio src="ms-winsoundevent:Notification.Reminder"/>

</toast>
```

알림 콘텐츠의 시각적 표현은 다음과 같습니다.

![알림 메시지 구조](images/adaptivetoasts-structure.jpg)


## <a name="visual"></a>시각적 개체

각 알림은 텍스트, 이미지, 로고 등을 포함할 수 있는 일반 알림 바인딩을 제공해야 하는 시각적 개체를 지정해야 합니다. 이러한 요소는 데스크톱, 전화, 태블릿 및 Xbox를 포함한 다양한 Windows 장치에서 렌더링됩니다.

시각적 섹션 및 하위 요소에서 지원되는 모든 특성에 대해서는 [스키나 설명서](tiles-and-notifications-toast-schema.md#toastvisual)를 참조하세요.

알림 메시지의 앱 ID는 앱 아이콘을 통해 전달됩니다. 그러나 앱 로고 재정의를 사용하는 경우 텍스트 줄 아래에 앱 이름이 표시됩니다.

| 일반적인 알림                                                                              | appLogoOverride를 사용하는 알림                                                          |
| ----------------------------------------------------------------------------------------- | ----------------------------------------------------------------------------------- |
| ![appLogoOverride를 사용하지 않는 알림](images/adaptivetoasts-withoutapplogooverride.jpg) | ![appLogoOverride를 사용하는 알림](images/adaptivetoasts-withapplogooverride.jpg) |


## <a name="text-elements"></a>텍스트 요소

각 알림에 텍스트 요소가 1개 이상 있어야 하며 [AdaptiveText](tiles-and-notifications-toast-schema.md#adaptivetext) 유형의 추가 텍스트 요소를 2개 포함할 수 있습니다.

![제목 및 설명이 있는 알림](images/toast-title-and-description.jpg)

1주년 업데이트 이후로 텍스트에 **HintMaxLines** 속성을 사용하여 표시되는 텍스트 줄 수를 제어할 수 있습니다. 기본적으로 제목은 텍스트 2줄까지 표시하며 설명 줄은 각각 텍스트 4줄까지 표시합니다.

```csharp
new ToastBindingGeneric()
{
    Children =
    {
        new AdaptiveText()
        {
            Text = "Adaptive Tiles Meeting",
            HintMaxLines = 1
        },

        new AdaptiveText()
        {
            Text = "Conf Room 2001 / Building 135"
        },

        new AdaptiveText()
        {
            Text = "10:00 AM - 10:30 AM"
        }
    }
}
```

```xml
<binding template="ToastGeneric">
    <text hint-maxLines="1">Adaptive Tiles Meeting</text>
    <text>Conf Room 2001 / Building 135</text>
    <text>10:00 AM - 10:30 AM</text>
</binding>
```


## <a name="app-logo-override"></a>앱 로고 재정의

기본적으로 알림에서 앱의 로고를 표시합니다. 그러나 자신의 [ToastGenericAppLogo](tiles-and-notifications-toast-schema.md#toastgenericapplogo) 이미지로 이 로고를 재정의할 수 있습니다. 예를 들어, 어떤 사용자가 보내는 알림일 경우 해당 사용자의 사진으로 앱 로고를 재정의하는 것이 좋습니다.

![앱 로고 재정의를 사용하는 알림](images/toast-applogooverride.jpg)

**HintCrop** 속성을 사용하여 이미지의 자르기를 변경할 수 있습니다. 예를 들어, *circle*은 원으로 잘린 이미지를 생성합니다. 그렇지 않으면 이미지가 사각형입니다. 이미지 치수는 100% 배율에서 64x64픽셀입니다.

```csharp
new ToastBindingGeneric()
{
    ...

    AppLogoOverride = new ToastGenericAppLogo()
    {
        Source = "https://unsplash.it/64?image=883",
        HintCrop = ToastGenericAppLogoCrop.Circle
    }
}
```

```xml
<binding template="ToastGeneric">
    ...
    <image placement="appLogoOverride" hint-crop="circle" src="https://unsplash.it/64?image=883"/>
</binding>
```


## <a name="hero-image"></a>영웅 이미지

**1주년 업데이트의 새로운 기능**: 알림은 알림 센터 내에 있는 동안 알림 배너 내에 눈에 띄게 표시되는 추천 [ToastGenericHeroImage](tiles-and-notifications-toast-schema.md#toastgenericheroimage)인 영웅 이미지를 표시할 수 있습니다. 이미지 치수는 100& 배율에서 360x180픽셀입니다.

![영웅 이미지가 있는 알림](images/toast-heroimage.jpg)

```csharp
new ToastBindingGeneric()
{
    ...

    HeroImage = new ToastHeroImage()
    {
        Source = "https://unsplash.it/360/180?image=1043"
    }
}
```

```xml
<binding template="ToastGeneric">
    ...
    <image placement="hero" src="https://unsplash.it/360/180?image=1043"/>
</binding>
```


## <a name="inline-image"></a>인라인 이미지

알림을 확장할 때 나타나는 전체 너비 인라인 이미지를 제공할 수 있습니다.

![추가 이미지가 있는 알림](images/toast-additionalimage.jpg)

```csharp
new ToastBindingGeneric()
{
    Children =
    {
        ...

        new AdaptiveImage()
        {
            Source = "https://unsplash.it/360/180?image=1043"
        }
    }
}
```

```xml
<binding template="ToastGeneric">
    ...
    <image src="https://unsplash.it/360/180?image=1043" />
</binding>
```


## <a name="attribution-text"></a>특성 텍스트

**1주년 업데이트의 새로운 기능**: 콘텐츠의 소스를 참조해야 하는 경우 특성 텍스트를 사용할 수 있습니다. 이 텍스트는 앱의 ID나 알림의 타임스탬프와 함께 알림의 아래쪽에 항상 표시됩니다.

특성 텍스트를 지원하지 않는 더 오래된 버전의 Windows에서는 텍스트가 다른 텍스트 요소로 표시됩니다. 단, 최대 3개의 텍스트 요소가 아직 없어야 합니다.

![특성 텍스트가 있는 알림](images/toast-attributiontext.jpg)

```csharp
new ToastBindingGeneric()
{
    ...

    Attribution = new ToastGenericAttributionText()
    {
        Text = "Via SMS"
    }
}
```

```xml
<binding template="ToastGeneric">
    ...
    <text placement="attribution">Via SMS</text>
</binding>
```


## <a name="custom-timestamp"></a>사용자 지정 타임스탬프

**크리에이터 업데이트의 새로운 기능**: 이제 시스템에서 제공하는 타임스탬프를 메시지/정보/콘텐츠 생성 시기를 정확하게 나타내는 타임스탬프로 재정의할 수 있습니다. 이 타임스탬프는 알림 센터 내에 표시됩니다.

![사용자 지정 타임스탬프가 있는 알림](images/toast-customtimestamp.jpg)

사용자 지정 타임스탬프 사용에 대한 자세한 내용은 [블로그 게시물](https://blogs.msdn.microsoft.com/tiles_and_toasts/2017/01/09/custom-timestamp-on-toast-notifications-windows-10-creators-update/)을 참조하세요.

```csharp
ToastContent toastContent = new ToastContent()
{
    DisplayTimestamp = new DateTime(2017, 04, 15, 19, 45, 00, DateTimeKind.Utc),
    ...
};
```

```xml
<toast displayTimestamp="2017-04-15T19:45:00Z">
  ...
</toast>
```


## <a name="adaptive-content"></a>적응형 콘텐츠

**1주년 업데이트의 새로운 기능**: 위에 지정된 콘텐츠 외에도 알림 확장 시 표시되는 추가 적응형 콘텐츠를 표시할 수도 있습니다.

Adaptive를 사용하여 이 추가 콘텐츠를 지정합니다. 이에 대한 자세한 내용은 [Adaptive Tiles documentation(적응형 타일 설명서)](tiles-and-notifications-create-adaptive-tiles.md)을 참조하세요.

적응형 콘텐츠는 AdaptiveGroup 내에 포함되어야 합니다. 그렇지 않으면 적응형으로 렌더링되지 않습니다.


### <a name="columns-and-text-elements"></a>열 및 텍스트 요소

다음은 열과 일부 고급 적응형 텍스트의 사용 예입니다. 텍스트 요소는 AdaptiveGroup 내에 있으므로 다양한 적응형 스타일 지정 속성을 모두 지원합니다.

![텍스트가 있는 알림](images/toast-additionaltext.jpg)

```csharp
new ToastBindingGeneric()
{
    Children =
    {
        ...

        new AdaptiveGroup()
        {
            Children =
            {
                new AdaptiveSubgroup()
                {
                    Children =
                    {
                        new AdaptiveText()
                        {
                            Text = "52 attendees",
                            HintStyle = AdaptiveTextStyle.Base
                        },
                        new AdaptiveText()
                        {
                            Text = "23 minute drive",
                            HintStyle = AdaptiveTextStyle.CaptionSubtle
                        }
                    }
                },
                new AdaptiveSubgroup()
                {
                    Children =
                    {
                        new AdaptiveText()
                        {
                            Text = "1 Microsoft Way",
                            HintStyle = AdaptiveTextStyle.CaptionSubtle,
                            HintAlign = AdaptiveTextAlign.Right
                        },
                        new AdaptiveText()
                        {
                            Text = "Bellevue, WA 98008",
                            HintStyle = AdaptiveTextStyle.CaptionSubtle,
                            HintAlign = AdaptiveTextAlign.Right
                        }
                    }
                }
            }
        }
    }
}
```

```xml
<binding template="ToastGeneric">
    ...
    <group>
        <subgroup>
            <text hint-style="base">52 attendees</text>
            <text hint-style="captionSubtle">23 minute drive</text>
        </subgroup>
        <subgroup>
            <text hint-style="captionSubtle" hint-align="right">1 Microsoft Way</text>
            <text hint-style="captionSubtle" hint-align="right">Bellevue, WA 98008</text>
        </subgroup>
    </group>
</binding>
```


## <a name="inputs-and-buttons"></a>입력 및 단추

입력과 단추는 알림의 알림 영역의 작업 영역 내에 지정되므로 알림 확장 시에만 표시됩니다.


### <a name="quick-reply-text-box"></a>빠른 회신 텍스트 상자

메시지 시나리오 경우와 같이 빠른 회신 텍스트 상자를 활성화하려면 입력 주위에 단추가 표시되도록 텍스트 입력 및 단추를 추가하고 텍스트 입력의 ID를 참조합니다.

![텍스트 입력 및 작업이 있는 알림](images/adaptivetoasts-xmlsample05.jpg)

```csharp
ToastContent content = new ToastContent()
{
    ...
 
    Actions = new ToastActionsCustom()
    {
        Inputs =
        {
            new ToastTextBox("tbReply")
            {
                PlaceholderContent = "Type a reply"
            }
        },

        Buttons =
        {
            new ToastButton("Reply", "action=reply&convId=9318")
            {
                ActivationType = ToastActivationType.Background,

                // To place the button next to the text box,
                // reference the text box's Id and provide an image
                TextBoxId = "tbReply",
                ImageUri = "Assets/Reply.png"
            }
        }
    }
};
```

```xml
<toast launch="app-defined-string">

    ...

    <actions>

        <input id="textBox" type="text" placeholderContent="Type a reply"/>

        <action
            content="Send",
            arguments="action=reply&amp;convId=9318"
            activationType="background"
            hint-inputId="textBox"
            imageUri="Assets/Reply.png"/>

    </actions>

</toast>
```


### <a name="inputs-with-buttons-bar"></a>단추 모음이 있는 입력

입력 아래에 일반 단추가 표시된 입력이 1개 이상 있을 수 있습니다.

![텍스트 및 입력 작업이 있는 알림](images/adaptivetoasts-xmlsample04.jpg)

```csharp
ToastContent content = new ToastContent()
{
    ...
 
    Actions = new ToastActionsCustom()
    {
        Inputs =
        {
            new ToastTextBox("tbReply")
            {
                PlaceholderContent = "Type a reply"
            }
        },

        Buttons =
        {
            new ToastButton("Reply", "action=reply&threadId=9218")
            {
                ActivationType = ToastActivationType.Background
            },

            new ToastButton("Video call", "action=videocall&threadId=9218")
            {
                ActivationType = ToastActivationType.Foreground
            }
        }
    }
};
```

```xml
<toast launch="app-defined-string">

    ...

    <actions>

        <input id="textBox" type="text" placeholderContent="Type a reply"/>

        <action
            content="Reply",
            arguments="action=reply&amp;threadId=9218"
            activationType="background"/>

        <action
            content="Video call",
            arguments="action=videocall&amp;threadId=9218"
            activationType="foreground"/>

    </actions>

</toast>
```


### <a name="selection-input"></a>선택 입력

텍스트 상자 외에 선택 메뉴를 사용할 수도 있습니다.

![선택 입력 및 작업이 있는 알림](images/adaptivetoasts-xmlsample06.jpg)

```csharp
ToastContent content = new ToastContent()
{
    ...
 
    Actions = new ToastActionsCustom()
    {
        Inputs =
        {
            new ToastSelectionBox("time")
            {
                DefaultSelectionBoxItemId = "lunch",
                Items =
                {
                    new ToastSelectionBoxItem("breakfast", "Breakfast"),
                    new ToastSelectionBoxItem("lunch", "Lunch"),
                    new ToastSelectionBoxItem("dinner", "Dinner")
                }
            }
        },

        Buttons = { ... }
};
```

```xml
<toast launch="app-defined-string">

    ...

    <actions>

        <input id="time" type="selection" defaultInput="lunch">
            <selection id="breakfast" content="Breakfast" />
            <selection id="lunch" content="Lunch" />
            <selection id="dinner" content="Dinner" />
        </input>

        ...

    </actions>

</toast>
```


## <a name="buttons"></a>단추

단추는 알림을 대화형으로 만들어 사용자가 현재 워크플로를 방해하지 않고 알림 메시지에 신속하게 대응할 수 있습니다. 예를 들어, 사용자가 알림 내에서 직접 메시지에 회신하거나 전자 메일 앱을 열지 않고도 전자 메일을 삭제할 수 있습니다.

단추는 다음과 같은 여러 작업을 수행할 수 있습니다.

-   특정 페이지/컨텍스트로 이동하는 데 사용될 수 있는 인수를 사용하여 앱을 포그라운드에서 활성화합니다.
-   빠른 회신이나 비슷한 시나리오를 위해 앱의 백그라운드 작업을 활성화합니다.
-   프로토콜 실행을 통해 다른 앱을 활성화합니다.
-   알림 해제 또는 다시 알림과 같은 시스템 작업을 수행합니다.

나중에 다룰 상황에 따른 메뉴 항목을 포함하여 단추는 5개까지만 가능합니다.

![작업이 있는 알림, 예 1](images/adaptivetoasts-xmlsample02.jpg)

```csharp
ToastContent content = new ToastContent()
{
    ...
 
    Actions = new ToastActionsCustom()
    {
        Buttons =
        {
            new ToastButton("See more details", "action=viewdetails&contentId=351")
            {
                ActivationType = ToastActivationType.Foreground
            },

            new ToastButton("Remind me later", "action=remindlater&contentId=351")
            {
                ActivationType = ToastActivationType.Background
            }
        }
    }
};
```

```xml
<toast launch="app-defined-string">

    ...

    <actions>

        <action
            content="See more details",
            arguments="action=viewdetails&amp;contentId=351"
            activationType="foreground"/>

        <action
            content="Remind me later",
            arguments="action=remindlater&amp;contentId=351"
            activationType="background"/>

    </actions>

</toast>
```


### <a name="snoozedismiss-buttons"></a>다시 알림/해제 단추

선택 메뉴와 2개의 단추를 사용하여 시스템 다시 알림 및 해제 작업을 활용하는 미리 알림을 만들 수 있습니다. 미리 알림처럼 작동하도록 알림에 대해 시나리오를 미리 알림으로 설정합니다.

![미리 알림](images/adaptivetoasts-xmlsample07.jpg)

알림 단추에 *SepectionBoxId* 속성을 사용하여 선택 메뉴 입력에 미리 알림 단추를 연결합니다.

```csharp
ToastContent content = new ToastContent()
{
    Scenario = ToastScenario.Reminder,

    ...
 
    Actions = new ToastActionsCustom()
    {
        Inputs =
        {
            new ToastSelectionBox("snoozeTime")
            {
                DefaultSelectionBoxItemId = "15",
                Items =
                {
                    new ToastSelectionBoxItem("5", "5 minutes"),
                    new ToastSelectionBoxItem("15", "15 minutes"),
                    new ToastSelectionBoxItem("60", "1 hour"),
                    new ToastSelectionBoxItem("240", "4 hours"),
                    new ToastSelectionBoxItem("1440", "1 day")
                }
            }
        },

        Buttons =
        {
            new ToastButtonSnooze()
            {
                SelectionBoxId = "snoozeTime"
            },
 
            new ToastButtonDismiss()
        }
    }
};
```

```xml
<toast scenario="reminder" launch="action=viewEvent&amp;eventId=1983">
   
  ...
 
  <actions>
     
    <input id="snoozeTime" type="selection" defaultInput="15">
      <selection id="1" content="1 minute"/>
      <selection id="15" content="15 minutes"/>
      <selection id="60" content="1 hour"/>
      <selection id="240" content="4 hours"/>
      <selection id="1440" content="1 day"/>
    </input>
 
    <action activationType="system" arguments="snooze" hint-inputId="snoozeTime" content="" />
 
    <action activationType="system" arguments="dismiss" content=""/>
     
  </actions>
   
</toast>
```

시스템 다시 알림 및 해제 작업을 사용하려면 다음을 수행합니다.

-   ToastButtonSnooze 또는 ToastButtonDismiss 지정
-   필요에 따라 사용자 지정 콘텐츠 문자열 지정
    -   문자열을 제공하지 않는 경우 "다시 알림" 및 "해제"에 대한 지역화된 문자열이 자동으로 사용됩니다.
-   필요에 따라 *SelectionBoxId*를 지정합니다.
    -   사용자가 다시 알림 간격을 선택하게 하지 않고 OS 전체에서 일관성 있는 시스템 정의 시간 간격에 한 번만 알림을 다시 발생하게 하려면 &lt;input&gt;을 구성하지 마세요.
    -   다시 알림 간격 선택 항목을 제공하려면
        -   다시 알림 작업에 *SelectionBoxId* 지정
        -   다시 알림 작업의 *SelectionBoxId*에 입력의 ID 맞춤
        -   *ToastSelectionBoxItem*의 값을 다시 알림 간격(분)을 나타내는 nonNegativeInteger이 되도록 지정합니다.



## <a name="audio"></a>오디오

사용자 지정 오디오는 항상 모바일로 지원되었으며, 데스크톱 버전 1511(빌드 10586) 이상의 최신 버전에서도 지원됩니다. 사용자 지정 오디오는 다음 경로에서 참조할 수 있습니다.

-   ms-appx:///
-   ms-appdata:///

또는 두 플랫폼 모두에서 지원되는 [ms-winsoundevents 목록](https://msdn.microsoft.com/library/windows/apps/br230842)에서 선택하는 것도 가능합니다.

```csharp
ToastContent content = new ToastContent()
{
    ...

    Audio = new ToastAudio()
    {
        Src = new Uri("ms-appx:///Assets/NewMessage.mp3")
    }
}
```

```xml
<toast launch="app-defined-string">

    ...

    <audio src="ms-appx:///Assets/NewMessage.mp3"/>

</toast>
```

토스트 알림의 오디오에 대한 자세한 내용은 [오디오 스키마 페이지](https://msdn.microsoft.com/library/windows/apps/br230842)을 참조하세요. 사용자 지정 오디오를 사용하여 토스트를 보내는 방법에 대한 자세한 내용은 [블로그 게시물](https://blogs.msdn.microsoft.com/tiles_and_toasts/2016/06/18/quickstart-sending-a-toast-notification-with-custom-audio/)을 참조하세요.


## <a name="alarms-reminders-and-incoming-calls"></a>알람, 미리 알림 및 수신 전화

알람, 미리 알림 및 수신 전화 알림을 만들려면 시나리오 값이 할당된 일반 알림 메시지를 사용하면 됩니다. 시나리오는 몇 가지 동작을 조정하여 일관성 있고 통합된 사용자 환경을 만듭니다.

* **미리 알림**: 알림이 사용자가 알림을 해제하거나 작업을 수행할 때까지 화면에 표시됩니다. 또한 Windows Mobile에서는 알림이 미리 확장되어 표시됩니다. 미리 알림 소리가 재생됩니다.
* **알람**: 미리 알림 동작 외에도 기본 알람 소리로 알림이 추가로 오디오를 반복합니다.
* **IncomingCall**: Windows Mobile 장치의 전체 화면에 수신 전화 알림이 표시됩니다. 그렇지 않으면 벨소리 오디오를 사용한다는 점을 제외하고는 알람으로 동일한 동작을 수행합니다.

```csharp
ToastContent content = new ToastContent()
{
    Scenario = ToastScenario.Reminder,

    ...
}
```

```xml
<toast scenario="reminder" launch="app-defined-string">

    ...

</toast>
```


## <a name="handling-activation"></a>활성화 처리
알림 활성화를 처리하는 방법(사용자가 알림 또는 알림의 단추 클릭)은 [Send local toast(로컬 알림 보내기)](tiles-and-notifications-send-local-toast.md)를 참조하세요.


 
## <a name="related-topics"></a>관련 항목

* [로컬 알림 보내기 및 활성화 처리](tiles-and-notifications-send-local-toast.md)
* [GitHub의 알림 라이브러리](https://github.com/Microsoft/UWPCommunityToolkit/tree/dev/Notifications)