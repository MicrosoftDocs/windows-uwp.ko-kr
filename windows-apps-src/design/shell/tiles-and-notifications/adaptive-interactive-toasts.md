---
author: andrewleader
Description: Adaptive and interactive toast notifications let you create flexible pop-up notifications with more content, optional inline images, and optional user interaction.
title: 알림 내용
ms.assetid: 1FCE66AF-34B4-436A-9FC9-D0CF4BDA5A01
label: Toast content
template: detail.hbs
ms.author: mijacobs
ms.date: 11/20/2017
ms.topic: article
keywords: Windows 10, uwp, 알림 메시지, 대화형 알림, 적응형 알림, 알림 콘텐츠, 알림 페이로드
ms.localizationpriority: medium
ms.openlocfilehash: 791b1dcede799de4ecf8480d994a5c0f1ddb58af
ms.sourcegitcommit: 3257416aebb5a7b1515e107866806f8bd57845a8
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/17/2018
ms.locfileid: "7163120"
---
# <a name="toast-content"></a>알림 콘텐츠

적응형 및 대화형 알림 메시지를 사용하여 텍스트, 이미지 및 단추/입력을 포함한 유연한 알림을 만들 수 있습니다.

> **중요 API**: [UWP 커뮤니티 도구 키트 알림 NuGet 패키지](https://www.nuget.org/packages/Microsoft.Toolkit.Uwp.Notifications/)

> [!NOTE]
> Windows8.1 및 Windows Phone 8.1의 레거시 템플릿을 보려면, [레거시 알림 템플릿 카탈로그](https://msdn.microsoft.com/library/windows/apps/hh761494)를 참조 하세요.


## <a name="getting-started"></a>시작

**알림 라이브러리 설치.** XML 대신 C#을 사용하여 알림을 생성하려면 [Microsoft.Toolkit.Uwp.Notifications](https://www.nuget.org/packages/Microsoft.Toolkit.Uwp.Notifications/)("notifications uwp" 검색)라는 NuGet 패키지를 설치합니다. 이 문서에서 제공하는 C# 샘플은 NuGet 패키지 버전 1.0.0을 사용합니다.

**알림 시각화 도우미 설치.** 이 무료 UWP 앱은 Visual Studio의 XAML 편집기/디자인 뷰와 비슷하게, 알림을 편집할 때 시각적 미리 보기를 곧바로 제공하여 대화형 알림 메시지를 디자인하는 데 도움이 됩니다. 자세한 내용은 [알림 시각화 도우미](notifications-visualizer.md)를 참조하거나 [Microsoft Store에서 알림 시각화 도우미를 다운로드하세요](https://www.microsoft.com/store/apps/notifications-visualizer/9nblggh5xsl1).


## <a name="sending-a-toast-notification"></a>알림 메시지 보내기

알림을 보내는 방법은 [Send local toast(로컬 알림 보내기)](send-local-toast.md)를 참조하세요. 이 설명서에는 알림 콘텐츠 생성만 다룹니다.


## <a name="toast-notification-structure"></a>알림 메시지 구조

알림 메시지는 알림 식별에 사용할 수 있는 Tag/Group과 같은 몇 가지 데이터 속성과 *알림 콘텐츠*의 조합입니다.

알림 콘텐츠의 주요 구성 요소는 다음과 같습니다.
* **실행**: 사용자가 알림을 클릭할 때 앱에 다시 전달될 인수를 정의하여 알림이 표시 중인 올바른 콘텐츠로 딥 링크할 수 있게 합니다. 자세한 내용은 [Send local toast(로컬 알림 보내기)](send-local-toast.md)를 참조하세요.
* **시각적 개체**: 알림의 시각적 부분으로 텍스트와 이미지가 들어 있는 일반 바인딩을 포함합니다.
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

각 알림은 텍스트, 이미지 등을 포함할 수 있는 일반 알림 바인딩을 제공해야 하는 시각적 개체를 지정해야 합니다. 이러한 요소는 데스크톱, 전화, 태블릿 및 Xbox를 포함한 다양한 Windows 장치에서 렌더링됩니다.

시각적 섹션 및 하위 요소에서 지원되는 모든 특성에 대해서는 [스키나 설명서](toast-schema.md#toastvisual)를 참조하세요.

알림 메시지의 앱 ID는 앱 아이콘을 통해 전달됩니다. 그러나 앱 로고 재정의를 사용하는 경우 텍스트 줄 아래에 앱 이름이 표시됩니다.

| 정상 메시지에 대한 앱 ID | appLogoOverride를 통한 앱 ID |
| -- | -- |
| <img src="images/adaptivetoasts-withoutapplogooverride.jpg" alt="notification without appLogoOverride" width="364"/> | <img alt="notification with appLogoOverride" src="images/adaptivetoasts-withapplogooverride.jpg" width="364"/> |


## <a name="text-elements"></a>텍스트 요소

각 알림에 텍스트 요소가 1개 이상 있어야 하며 [**AdaptiveText**](toast-schema.md#adaptivetext) 유형의 추가 텍스트 요소를 2개 포함할 수 있습니다.

<img alt="Toast with title and description" src="images/toast-title-and-description.jpg" width="364"/>

Windows 10 1주년 업데이트 이후로 텍스트에 **HintMaxLines** 속성을 사용하여 표시되는 텍스트 줄 수를 제어할 수 있습니다. 기본 및 최대값은 제목에서 최대 텍스트 2줄이고 추가 설명 요소 2개(두 번째와 세 번째 **AdaptiveText**)에서 최대 4줄(결합됨)입니다.

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

기본적으로 알림에서 앱의 로고를 표시합니다. 그러나 자체 [**ToastGenericAppLogo**](toast-schema.md#toastgenericapplogo) 이미지로 이 로고를 재정의할 수 있습니다. 예를 들어, 어떤 사용자가 보내는 알림일 경우 해당 사용자의 사진으로 앱 로고를 재정의하는 것이 좋습니다.

<img alt="Toast with app logo override" src="images/toast-applogooverride.jpg" width="364"/>

**HintCrop** 속성을 사용하여 이미지의 자르기를 변경할 수 있습니다. 예를 들어 **Circle**은 원으로 잘린 이미지를 생성합니다. 그렇지 않으면 이미지가 사각형입니다. 이미지 치수는 100% 배율에서 48x48픽셀입니다.

```csharp
new ToastBindingGeneric()
{
    ...

    AppLogoOverride = new ToastGenericAppLogo()
    {
        Source = "https://picsum.photos/48?image=883",
        HintCrop = ToastGenericAppLogoCrop.Circle
    }
}
```

```xml
<binding template="ToastGeneric">
    ...
    <image placement="appLogoOverride" hint-crop="circle" src="https://picsum.photos/48?image=883"/>
</binding>
```


## <a name="hero-image"></a>영웅 이미지

**1주년 업데이트의 새로운 기능**: 알림은 알림 센터 내에 있는 동안 알림 배너 내에 눈에 띄게 표시되는 추천 [**ToastGenericHeroImage**](toast-schema.md#toastgenericheroimage)인 영웅 이미지를 표시할 수 있습니다. 이미지 치수는 100% 배율에서 364x180픽셀입니다.

<img alt="Toast with hero image" src="images/toast-heroimage.jpg" width="364"/>

```csharp
new ToastBindingGeneric()
{
    ...

    HeroImage = new ToastGenericHeroImage()
    {
        Source = "https://picsum.photos/364/180?image=1043"
    }
}
```

```xml
<binding template="ToastGeneric">
    ...
    <image placement="hero" src="https://picsum.photos/364/180?image=1043"/>
</binding>
```


## <a name="inline-image"></a>인라인 이미지

알림을 확장할 때 나타나는 전체 너비 인라인 이미지를 제공할 수 있습니다.

<img alt="Toast with additional image" src="images/toast-additionalimage.jpg" width="364"/>

```csharp
new ToastBindingGeneric()
{
    Children =
    {
        ...

        new AdaptiveImage()
        {
            Source = "https://picsum.photos/360/202?image=1043"
        }
    }
}
```

```xml
<binding template="ToastGeneric">
    ...
    <image src="https://picsum.photos/360/202?image=1043" />
</binding>
```


## <a name="image-size-restrictions"></a>이미지 크기 제한

알림 메시지에서 사용하는 이미지는 다음 위치에서 소싱될 수 있습니다...

 - http://
 - ms-appx:///
 - ms-appdata:///

Http 및 https 원격 웹 이미지의 경우 각 개별 이미지의 파일 크기 제한이 있습니다. Fall Creators Update(16299)에서 일반 연결의 경우 이 제한을 3MB로 높이고 데이터 통신 연결의 경우 1MB로 높였습니다. 그 전에 이미지는 항상 200KB로 제한되었습니다.

| 일반 연결 | 데이터 통신 연결 | Fall Creators Update 이전 |
| - | - | - |
| 3MB | 1MB | 200KB |

이미지가 파일 크기를 초과하거나 다운로드하지 못하는 경우 이 이미지는 삭제되고 나머지 알림이 표시됩니다.


## <a name="attribution-text"></a>특성 텍스트

**1주년 업데이트의 새로운 기능**: 콘텐츠의 소스를 참조해야 하는 경우 특성 텍스트를 사용할 수 있습니다. 이 텍스트는 앱의 ID나 알림의 타임스탬프와 함께 알림의 아래쪽에 항상 표시됩니다.

특성 텍스트를 지원하지 않는 더 오래된 버전의 Windows에서는 텍스트가 다른 텍스트 요소로 표시됩니다. 단, 최대 3개의 텍스트 요소가 아직 없어야 합니다.

<img alt="Toast with attribution text" src="images/toast-attributiontext.jpg" width="364"/>

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

<img alt="Toast with custom timestamp" src="images/toast-customtimestamp.jpg" width="396"/>

사용자 지정 타임스탬프 사용에 대한 자세한 내용은 [알림에 대한 사용자 지정 타임스탬프](custom-timestamps-on-toasts.md)를 참조하세요.

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


## <a name="progress-bar"></a>진행률 표시줄

**크리에이터 업데이트의 새로운 기능**: 알림 메시지에 대한 진행률 표시줄이 나타나므로 사용자는 다운로드 등에 대한 작업 진행률을 사용자에게 계속 알릴 수 있습니다.

<img alt="Toast with progress bar" src="images/toast-progressbar.png" width="364"/>

진행률 표시줄 사용에 대해 자세히 알아보려면 [알림 진행률 표시줄](toast-progress-bar.md)을 참조하세요.


## <a name="headers"></a>헤더

**크리에이터 업데이트의 새로운 기능**: 알림 센터의 헤더에서 알림을 그룹화할 수 있습니다. 예를 들어, 헤더에 따라 그룹 채팅 메시지를 그룹화하거나 헤더에 따라 일반적인 테마의 알림을 그룹화할 수 있습니다.

<img alt="Toasts with header" src="images/toast-headers-action-center.png" width="396"/>

헤더 사용에 대한 자세한 내용은 [알림 헤더](toast-headers.md)를 참조하세요.


## <a name="adaptive-content"></a>적응형 콘텐츠

**1주년 업데이트의 새로운 기능**: 위에 지정된 콘텐츠 외에도 알림 확장 시 표시되는 추가 적응형 콘텐츠를 표시할 수도 있습니다.

Adaptive를 사용하여 이 추가 콘텐츠를 지정합니다. 이에 대한 자세한 내용은 [Adaptive Tiles documentation(적응형 타일 설명서)](create-adaptive-tiles.md)을 참조하세요.

적응형 콘텐츠는 [**AdaptiveGroup**](https://docs.microsoft.com/windows/uwp/design/shell/tiles-and-notifications/toast-schema#adaptivegroup) 내에 포함되어야 합니다. 그렇지 않으면 적응형으로 렌더링되지 않습니다.


### <a name="columns-and-text-elements"></a>열 및 텍스트 요소

다음은 열과 일부 고급 적응형 텍스트의 사용 예입니다. 텍스트 요소는 **AdaptiveGroup** 내에 있으므로 다양한 적응형 스타일 지정 속성을 모두 지원합니다.

<img alt="Toast with additional text" src="images/toast-additionaltext.jpg" width="364"/>

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


## <a name="buttons"></a>단추

단추는 알림을 대화형으로 만들어 사용자가 현재 워크플로를 방해하지 않고 알림 메시지에 신속하게 대응할 수 있습니다. 예를 들어, 사용자가 알림 내에서 직접 메시지에 회신하거나 전자 메일 앱을 열지 않고도 전자 메일을 삭제할 수 있습니다. 단추는 알림의 확장된 부분에 표시됩니다.

완벽한 단추 구현에 대한 자세한 내용은 [Send local toast(로컬 알림 보내기)](send-local-toast.md)를 참조하세요.

단추는 다음과 같은 여러 작업을 수행할 수 있습니다.

-   특정 페이지/컨텍스트로 이동하는 데 사용될 수 있는 인수를 사용하여 앱을 포그라운드에서 활성화합니다.
-   빠른 회신이나 비슷한 시나리오를 위해 앱의 백그라운드 작업을 활성화합니다.
-   프로토콜 실행을 통해 다른 앱을 활성화합니다.
-   알림 해제 또는 다시 알림과 같은 시스템 작업을 수행합니다.

> [!NOTE]
> 나중에 다룰 컨텍스트 메뉴 항목을 포함하여 단추는 5개까지만 가능합니다.

<img alt="notification with actions, example 1" src="images/adaptivetoasts-xmlsample02.jpg" width="364"/>

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
            content="See more details"
            arguments="action=viewdetails&amp;contentId=351"
            activationType="foreground"/>

        <action
            content="Remind me later"
            arguments="action=remindlater&amp;contentId=351"
            activationType="background"/>

    </actions>

</toast>
```


### <a name="buttons-with-icons"></a>아이콘이 있는 단추

단추에 아이콘을 추가할 수 있습니다. 이 아이콘은 100% 배율에 흰색 투명 16 x 16 픽셀 이미지이며 이미지 자체에 안쪽 여백이 없습니다. 알림 메시지에서 아이콘을 제공할 경우 단추 스타일을 아이콘 버튼으로 변형하기 때문에 알림에서 단추 전체에 아이콘을 제공해야 합니다.

> [!NOTE]
> 접근성을 위해서는 대비되는 흰색 버전의 아이콘(흰색 배경에 검정색 아이콘)을 포함해야만 사용자가 고대비 흰색 모드를 켤 때 아이콘이 잘 보입니다. [메시지 접근성 페이지](tile-toast-language-scale-contrast.md)에서 자세히 알아보세요.

<img src="images\adaptivetoasts-buttonswithicons.png" width="364" alt="Toast that has buttons with icons"/>

```csharp
new ToastButton("Dismiss", "dismiss")
{
    ActivationType = ToastActivationType.Background,
    ImageUri = "Assets/ToastButtonIcons/Dismiss.png"
}
```


```xml
<action
    content="Dismiss"
    imageUri="Assets/ToastButtonIcons/Dismiss.png"
    arguments="dismiss"
    activationType="background"/>
```


### <a name="buttons-with-pending-update-activation"></a>보류 중인 업데이트 활성화를 적용한 단추

**Fall Creators Update의 새로운 기능**: 백그라운드 인증 단추에서 **PendingUpdate**의 활성화 동작 후 사용하여 알림 메시지에서 다단계 상호 작용을 만들 수 있습니다. 사용자가 버튼을 클릭하면 백그라운드 작업이 활성화되고 알림은 백그라운드 작업이 이 알림을 새로운 알림으로 바꿀 때까지 화면에 머물러 있는 "보류 중인 업데이트" 상태가 됩니다.

이것을 구현하는 방법을 알아보려면 [보류 중인 업데이트 알림](toast-pending-update.md)을 참조하세요.

![대기 중인 업데이트 알림](images/toast-pendingupdate.gif)


### <a name="context-menu-actions"></a>컨텍스트 메뉴 작업

**1주년 업데이트의 새로운 기능**: 사용자가 알림 센터에서 알림을 마우스 오른쪽 단추로 클릭할 때 나타나는 기존 컨텍스트 메뉴에 추가 컨텍스트 메뉴 작업을 추가할 수 있습니다. 이 메뉴는 알림 센터에서 마우스 오른쪽 단추를 클릭할 때에만 나타납니다. 알림 팝업 배너를 마우스 오른쪽 단추로 클릭할 때에는 표시되지 않습니다.

> [!NOTE]
> 이전 디바이스에서는 이러한 추가 컨텍스트 메뉴 작업이 알림에서 간단히 일반 단추로 표시됩니다.

추가하는 추가 컨텍스트 메뉴 작업(예: '위치 변경')은 기본 시스템 항목 2개 위에 나타납니다.

<img alt="Toast with context menu" src="images/toast-contextmenu.png" width="444"/>

```csharp
ToastContent content = new ToastContent()
{
    ...
 
    Actions = new ToastActionsCustom()
    {
        ContextMenuItems =
        {
            new ToastContextMenuItem("Change location", "action=changeLocation")
        }
    }
};
```

```xml
<toast>

    ...

    <actions>

        <action
            placement="contextMenu"
            content="Change location"
            arguments="action=changeLocation"/>

    </actions>

</toast>
```

> [!NOTE]
> 추가 컨텍스트 메뉴 항목은 알림에서 총 5개 버튼 한도에 기여합니다.

추가 컨텍스트 메뉴 항목의 활성화는 알림 단추와 동일하게 처리됩니다.


## <a name="inputs"></a>입력

입력은 알림의 알림 영역에서도 작업 영역에서 지정되므로 알림이 확장될 때에만 표시됩니다.


### <a name="quick-reply-text-box"></a>빠른 회신 텍스트 상자

메시지 시나리오 경우와 같이 빠른 회신 텍스트 상자를 활성화하려면 입력 주위에 단추가 표시되도록 텍스트 입력 및 단추를 추가하고 텍스트 입력의 ID를 참조합니다.

<img alt="notification with text input and actions" src="images/adaptivetoasts-xmlsample05.jpg" width="364"/>

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

        <input id="textBox" type="text" placeHolderContent="Type a reply"/>

        <action
            content="Send"
            arguments="action=reply&amp;convId=9318"
            activationType="background"
            hint-inputId="textBox"
            imageUri="Assets/Reply.png"/>

    </actions>

</toast>
```


### <a name="inputs-with-buttons-bar"></a>단추 모음이 있는 입력

입력 아래에 일반 단추가 표시된 입력이 1개 이상 있을 수 있습니다.

<img alt="notification with text and input actions" src="images/adaptivetoasts-xmlsample04.jpg" width="364"/>

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

        <input id="textBox" type="text" placeHolderContent="Type a reply"/>

        <action
            content="Reply"
            arguments="action=reply&amp;threadId=9218"
            activationType="background"/>

        <action
            content="Video call"
            arguments="action=videocall&amp;threadId=9218"
            activationType="foreground"/>

    </actions>

</toast>
```


### <a name="selection-input"></a>선택 입력

텍스트 상자 외에 선택 메뉴를 사용할 수도 있습니다.

<img alt="notification with selection input and actions" src="images/adaptivetoasts-xmlsample06.jpg" width="364"/>

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


### <a name="snoozedismiss"></a>다시 알림/해제

선택 메뉴와 2개의 단추를 사용하여 시스템 다시 알림 및 해제 작업을 활용하는 미리 알림을 만들 수 있습니다. 미리 알림처럼 작동하도록 알림에 대해 시나리오를 미리 알림으로 설정합니다.

<img alt="reminder notification" src="images/adaptivetoasts-xmlsample07.jpg" width="364"/>

알림 단추에 **SelectionBoxId** 속성을 사용하여 선택 메뉴 입력에 미리 알림 단추를 연결합니다.

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

시스템 다시 알림 및 해제 작업을 사용하려면:

-   **ToastButtonSnooze** 또는 **ToastButtonDismiss** 지정
-   필요에 따라 사용자 지정 콘텐츠 문자열 지정
    -   문자열을 제공하지 않는 경우 "다시 알림" 및 "해제"에 대한 지역화된 문자열이 자동으로 사용됩니다.
-   필요에 따라 **SelectionBoxId**를 지정합니다.
    -   사용자가 다시 알림 간격을 선택하게 하지 않고 OS 전체에서 일관성 있는 시스템 정의 시간 간격에 한 번만 알림을 다시 발생하게 하려면 &lt;input&gt;을 구성하지 마세요.
    -   다시 알림 간격 선택 항목을 제공하려면
        -   다시 알림 작업에 **SelectionBoxId** 지정
        -   다시 알림 작업의 **SelectionBoxId**에 입력의 ID 맞춤
        -   **ToastSelectionBoxItem**의 값을 다시 알림 간격(분)을 나타내는 nonNegativeInteger이 되도록 지정합니다.



## <a name="audio"></a>오디오

사용자 지정 오디오는 항상 모바일로 지원되었으며, 데스크톱 버전 1511(빌드 10586) 이상의 최신 버전에서도 지원됩니다. 사용자 지정 오디오는 다음 경로에서 참조할 수 있습니다.

-   ms-appx:///
-   ms-appdata:///

또는 두 플랫폼 모두에서 지원되는 [ms-winsoundevents 목록](/uwp/schemas/tiles/toastschema/element-audio#attributes-and-elements)에서 선택하는 것도 가능합니다.

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

토스트 알림의 오디오에 대한 자세한 내용은 [오디오 스키마 페이지](/uwp/schemas/tiles/toastschema/element-audio)을 참조하세요. 사용자 지정 오디오를 사용하여 알림을 보내는 방법에 대한 자세한 내용은 [알림에 대한 사용자 지정 오디오](custom-audio-on-toasts.md)를 참조하세요.


## <a name="alarms-reminders-and-incoming-calls"></a>알람, 미리 알림 및 수신 전화

알람, 미리 알림 및 수신 전화 알림을 만들려면 시나리오 값이 할당된 일반 알림 메시지를 사용하면 됩니다. 시나리오는 몇 가지 동작을 조정하여 일관성 있고 통합된 사용자 환경을 만듭니다.

> [!IMPORTANT]
> 미리 알림 또는 알람을 사용할 때 알림 메시지에 대한 하나 이상의 단추를 제공해야 합니다. 그렇지 않으면 이 알림은 일반 알림으로 처리됩니다.

* **미리 알림**: 알림이 사용자가 알림을 해제하거나 작업을 수행할 때까지 화면에 표시됩니다. 또한 Windows Mobile에서는 알림이 미리 확장되어 표시됩니다. 미리 알림 소리가 재생됩니다.
* **알람**: 미리 알림 동작 외에도 기본 알람 소리로 알림이 추가로 오디오를 반복합니다.
* **IncomingCall**: Windows Mobile 장치의 전체 화면에 수신 전화 알림이 표시됩니다. 그렇지 않으면 벨소리 오디오를 사용하고 단추가 다른 스타일로 지정된다는 점을 제외하고는 알람으로 동일한 동작을 수행합니다.

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


## <a name="localization-and-accessibility"></a>지역화 및 손쉬운 사용

타일 및 알림은 표시 언어, 디스플레이 배율 인수, 고대비 및 기타 런타임 컨텍스트에 맞게 조정된 문자열 및 이미지를 로드할 수 있습니다. 자세한 내용은 [언어, 배율, 고대비에 대한 타일 및 알림 메시지 지원](tile-toast-language-scale-contrast.md)을 참조하세요.


## <a name="handling-activation"></a>활성화 처리
알림 활성화를 처리하는 방법(사용자가 알림 또는 알림의 단추 클릭)은 [Send local toast(로컬 알림 보내기)](send-local-toast.md)를 참조하세요.
 
## <a name="related-topics"></a>관련 항목

* [로컬 알림 보내기 및 활성화 처리](send-local-toast.md)
* [GitHub(UWP 커뮤니티 도구 키트의 일부)에 대한 알림 라이브러리](https://github.com/Microsoft/UWPCommunityToolkit/tree/master/Microsoft.Toolkit.Uwp.Notifications)
* [언어, 배율, 고대비에 대한 타일 및 알림 메시지](tile-toast-language-scale-contrast.md)
