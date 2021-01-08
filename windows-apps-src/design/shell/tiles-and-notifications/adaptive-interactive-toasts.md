---
description: 적응형 및 대화형 알림 메시지를 사용하면 더 많은 콘텐츠, 선택적 인라인 이미지 및 선택적 사용자 조작이 포함된 유연한 팝업 알림을 만들 수 있습니다.
title: 알림 콘텐츠
ms.assetid: 1FCE66AF-34B4-436A-9FC9-D0CF4BDA5A01
label: Toast content
template: detail.hbs
ms.date: 09/24/2020
ms.topic: article
keywords: windows 10, uwp, 알림 메시지, 대화형 알림을, 적응 알림을, 알림 콘텐츠, 알림 페이로드
ms.localizationpriority: medium
ms.openlocfilehash: 92a8f53b2951d7fb3ed5bb5c0afbdf1e7e2424cc
ms.sourcegitcommit: fc7fb82121a00e552eaebafba42e5f8e1623c58a
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 01/07/2021
ms.locfileid: "97978588"
---
# <a name="toast-content"></a>알림 콘텐츠

적응 및 대화형 알림 메시지를 통해 텍스트, 이미지 및 단추/입력을 사용 하 여 유연한 알림을 만들 수 있습니다.

> **중요 API**: [UWP 커뮤니티 도구 키트 알림 NuGet 패키지](https://www.nuget.org/packages/Microsoft.Toolkit.Uwp.Notifications/)

> [!NOTE]
> Windows 8.1 및 Windows Phone 8.1에서 레거시 템플릿을 보려면 [레거시 알림 템플릿 카탈로그](/previous-versions/windows/apps/hh761494(v=win.10))를 참조 하세요.


## <a name="getting-started"></a>시작

**알림 라이브러리를 설치 합니다.** XML 대신 c #을 사용 하 여 알림을 생성 하려는 경우에 [는 이름이 "](https://www.nuget.org/packages/Microsoft.Toolkit.Uwp.Notifications/) notification uwp" 인 NuGet 패키지를 설치 합니다. 이 문서에 제공 된 c # 샘플에서는 NuGet 패키지의 버전 1.0.0을 사용 합니다.

**알림 시각화 도우미를 설치 합니다.** 이 무료 Windows 앱을 사용 하면 Visual Studio의 XAML 편집기/디자인 뷰와 유사 하 게 편집할 때 toast의 즉각적인 시각적 미리 보기를 제공 하 여 대화형 알림 메시지를 디자인할 수 있습니다. 자세한 내용은 [알림 시각화 도우미](notifications-visualizer.md) 를 참조 하거나, [스토어에서 알림 시각화 도우미를 다운로드](https://www.microsoft.com/store/apps/notifications-visualizer/9nblggh5xsl1)하세요.


## <a name="sending-a-toast-notification"></a>알림 메시지 보내기

알림을 보내는 방법을 알아보려면 [로컬 알림 보내기](send-local-toast.md)를 참조 하세요. 이 설명서에서는 알림 콘텐츠를 만드는 방법에 대해서만 다룹니다.


## <a name="toast-notification-structure"></a>알림 메시지 구조

알림 메시지는 태그/그룹 (알림을 식별 하는 데 사용 됨) 및 *알림 콘텐츠와* 같은 일부 데이터 속성의 조합입니다.

알림 콘텐츠의 핵심 구성 요소는 ...
* **시작**: 사용자가 알림을 클릭할 때 앱에 다시 전달 되는 인수를 정의 하 여 알림이 표시 되는 올바른 콘텐츠에 대 한 심층 링크를 제공 합니다. 자세히 알아보려면 [로컬 알림 보내기](send-local-toast.md)를 참조 하세요.
* **시각적 개체**: 텍스트 및 이미지를 포함 하는 제네릭 바인딩을 포함 하는 알림 표시 부분입니다.
* **작업**: 입력 및 작업을 포함 하 여 알림 메시지의 대화형 부분입니다.
* **audio**: 알림이 사용자에 게 표시 될 때 재생 되는 오디오를 제어 합니다.

알림 콘텐츠는 원시 XML로 정의 되지만, [NuGet 라이브러리](https://www.nuget.org/packages/Microsoft.Toolkit.Uwp.Notifications/) 를 사용 하 여 알림 콘텐츠를 생성 하는 c # (또는 c + +) 개체 모델을 가져올 수 있습니다. 이 문서에서는 알림 콘텐츠 내에 있는 모든 것을 설명 합니다.

#### <a name="builder-syntax"></a>[작성기 구문](#tab/builder-syntax)

```csharp
new ToastContentBuilder()
    .AddToastActivationInfo("app-defined-string", ToastActivationType.Foreground)
    .AddText("Some text")
    .AddButton("Archive", ToastActivationType.Background, "archive")
    .AddAudio(new Uri("ms-appx:///Sound.mp3"));
```

#### <a name="xml"></a>[XML](#tab/xml)

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

---

다음은 알림 메시지의 시각적 표현입니다.

![알림 메시지 구조](images/adaptivetoasts-structure.jpg)


## <a name="visual"></a>시각적 개체

각 알림이 텍스트, 이미지 등을 포함할 수 있는 일반 알림 바인딩을 제공 해야 하는 시각적 개체를 지정 해야 합니다. 이러한 요소는 데스크톱, 휴대폰, 태블릿 및 Xbox를 포함 한 다양 한 Windows 장치에서 렌더링 됩니다.

시각적 섹션과 해당 자식 요소에서 지원 되는 모든 특성 [은 스키마 설명서를 참조](toast-schema.md#toastvisual)하세요.

알림 알림의 앱 id는 앱 아이콘을 통해 전달 됩니다. 그러나 앱 로고 재정의를 사용 하는 경우 텍스트 줄 아래에 앱 이름을 표시 합니다.

| 일반적인 알림 메시지의 앱 id | AppLogoOverride를 사용 하는 앱 id |
| -- | -- |
| <img src="images/adaptivetoasts-withoutapplogooverride.jpg" alt="notification without appLogoOverride" width="364"/> | <img alt="notification with appLogoOverride" src="images/adaptivetoasts-withapplogooverride.jpg" width="364"/> |


## <a name="text-elements"></a>텍스트 요소

각 알림 메시지에는 하나 이상의 텍스트 요소가 있어야 하 고, [**AdaptiveText**](toast-schema.md#adaptivetext)형식의 추가 텍스트 요소가 두 개 이상 포함 될 수 있습니다.

<img alt="Toast with title and description" src="images/toast-title-and-description.jpg" width="364"/>

Windows 10 기념일 업데이트 이후에는 텍스트의 **Hintmaxlines** 속성을 사용 하 여 표시할 텍스트의 줄 수를 제어할 수 있습니다. 기본 (및 최대)은 제목에 대해 최대 2 줄의 텍스트, 두 개의 추가 설명 요소에 대 한 최대 4 줄 (결합)입니다 (두 번째 및 세 번째 **AdaptiveText**).

#### <a name="builder-syntax"></a>[작성기 구문](#tab/builder-syntax)

```csharp
new ToastContentBuilder()
    .AddText("Adaptive Tiles Meeting", hintMaxLines: 1)
    .AddText("Conf Room 2001 / Building 135")
    .AddText("10:00 AM - 10:30 AM");
```

#### <a name="xml"></a>[XML](#tab/xml)

```xml
<binding template="ToastGeneric">
    <text hint-maxLines="1">Adaptive Tiles Meeting</text>
    <text>Conf Room 2001 / Building 135</text>
    <text>10:00 AM - 10:30 AM</text>
</binding>
```

---


## <a name="app-logo-override"></a>앱 로고 재정의

기본적으로 알림 메시지는 앱 로고를 표시 합니다. 그러나이 로고는 사용자 고유의 [**Togenericgenericogo**](toast-schema.md#toastgenericapplogo) 이미지를 사용 하 여 재정의할 수 있습니다. 예를 들어이 알림이 사람의 알림이 면 해당 사람의 그림으로 앱 로고를 재정의 하는 것이 좋습니다.

<img alt="Toast with app logo override" src="images/toast-applogooverride.jpg" width="364"/>

**Hintcrop** 속성을 사용 하 여 이미지의 자르기를 변경할 수 있습니다. 예를 들어 **원은** 원에 잘린 이미지를 생성 합니다. 그렇지 않으면 이미지는 사각형입니다. 이미지 크기는 100% 크기 조정 시 48x48 픽셀입니다. 일반적으로 각 배율 인수에 대해 100%, 125%, 150%, 200% 및 400%의 각 아이콘 자산에 버전을 제공 하는 것이 좋습니다. 

#### <a name="builder-syntax"></a>[작성기 구문](#tab/builder-syntax)

```csharp
new ToastContentBuilder()
    ...
    
    .AddAppLogoOverride(new Uri("https://picsum.photos/48?image=883"), NotificationAppLogoCrop.Circle);
```

#### <a name="xml"></a>[XML](#tab/xml)

```xml
<binding template="ToastGeneric">
    ...
    <image placement="appLogoOverride" hint-crop="circle" src="https://picsum.photos/48?image=883"/>
</binding>
```

---



## <a name="hero-image"></a>주인공 이미지

**기념일 업데이트의 새로운** 기능: 알림을는 알림 배너 내에서 명확 하 게 표시 되는 주요 [**ToastGenericHeroImage**](toast-schema.md#toastgenericheroimage) 작업 센터 내에 있는 주인공 이미지를 표시할 수 있습니다. 이미지 크기는 364x180 픽셀 이며, 크기는 100%입니다.

<img alt="Toast with hero image" src="images/toast-heroimage.jpg" width="364"/>

#### <a name="builder-syntax"></a>[작성기 구문](#tab/builder-syntax)

```csharp
new ToastContentBuilder()
    ...
    
    .AddHeroImage(new Uri("https://picsum.photos/364/180?image=1043"));
```

#### <a name="xml"></a>[XML](#tab/xml)

```xml
<binding template="ToastGeneric">
    ...
    <image placement="hero" src="https://picsum.photos/364/180?image=1043"/>
</binding>
```

---


## <a name="inline-image"></a>인라인 이미지

알림을 확장할 때 표시 되는 전자/반자 인라인 이미지를 제공할 수 있습니다.

<img alt="Toast with additional image" src="images/toast-additionalimage.jpg" width="364"/>

#### <a name="builder-syntax"></a>[작성기 구문](#tab/builder-syntax)

```csharp
new ToastContentBuilder()
    ...
    
    .AddInlineImage(new Uri("https://picsum.photos/360/202?image=1043"));
```

#### <a name="xml"></a>[XML](#tab/xml)

```xml
<binding template="ToastGeneric">
    ...
    <image src="https://picsum.photos/360/202?image=1043" />
</binding>
```

---


## <a name="image-size-restrictions"></a>이미지 크기 제한

알림 메시지에서 사용 하는 이미지는 다음에서 원본으로 지정할 수 있습니다.

 - http://
 - ms-appx:///
 - ms-appdata:///

Http 및 https 원격 웹 이미지의 경우 각 개별 이미지의 파일 크기에 제한이 있습니다. 이에 대 한 연결 업데이트 (16299)에서는 일반적인 연결의 경우 3mb, 요금제 연결의 경우 1mb로 증가 했습니다. 이전에는 이미지가 항상 200 KB로 제한 되었습니다.

| 일반 연결 | 요금제 연결 | 동일 하 게 작성자 업데이트 전 |
| - | - | - |
| 3 MB | 1MB | 200KB |

이미지가 파일 크기를 초과 하거나 다운로드에 실패 하거나 시간이 초과 되 면 이미지가 삭제 되 고 나머지 알림이 표시 됩니다.


## <a name="attribution-text"></a>특성 텍스트

**기념일 업데이트의 새로운** 내용: 콘텐츠의 원본을 참조 해야 하는 경우 특성 텍스트를 사용할 수 있습니다. 이 텍스트는 항상 사용자의 앱 id 또는 알림의 타임 스탬프와 함께 알림 아래쪽에 표시 됩니다.

특성 텍스트를 지원 하지 않는 이전 버전의 Windows에서는 텍스트가 다른 텍스트 요소로 표시 됩니다 (최대 3 개의 텍스트 요소가 아직 없는 경우).

<img alt="Toast with attribution text" src="images/toast-attributiontext.jpg" width="364"/>

#### <a name="builder-syntax"></a>[작성기 구문](#tab/builder-syntax)

```csharp
new ToastContentBuilder()
    ...
    
    .AddAttributionText("Via SMS");
```

#### <a name="xml"></a>[XML](#tab/xml)

```xml
<binding template="ToastGeneric">
    ...
    <text placement="attribution">Via SMS</text>
</binding>
```

---


## <a name="custom-timestamp"></a>사용자 지정 타임 스탬프

**크리에이터 업데이트의 새로운** 기능: 이제 메시지/정보/내용이 생성 될 때 정확 하 게 나타내는 타임 스탬프를 사용 하 여 시스템 제공 타임 스탬프를 재정의할 수 있습니다. 이 타임 스탬프는 작업 센터 내에서 볼 수 있습니다.

<img alt="Toast with custom timestamp" src="images/toast-customtimestamp.jpg" width="396"/>

사용자 지정 타임 스탬프를 사용 하는 방법에 대 한 자세한 내용은 [알림을의 사용자 지정 타임 스탬프](custom-timestamps-on-toasts.md)를 참조 하세요.

#### <a name="builder-syntax"></a>[작성기 구문](#tab/builder-syntax)

```csharp
new ToastContentBuilder()
    ...
    
    .AddCustomTimeStamp(new DateTime(2017, 04, 15, 19, 45, 00, DateTimeKind.Utc));
```

#### <a name="xml"></a>[XML](#tab/xml)

```xml
<toast displayTimestamp="2017-04-15T19:45:00Z">
  ...
</toast>
```

---


## <a name="progress-bar"></a>진행률 표시줄

**작성자 업데이트의 새로운** 기능: 알림 메시지에 진행률 표시줄을 제공 하 여 다운로드와 같은 작업의 진행 상황을 사용자에 게 알릴 수 있습니다.

<img alt="Toast with progress bar" src="images/toast-progressbar.png" width="364"/>

진행률 표시줄을 사용 하는 방법에 대해 자세히 알아보려면 [알림 진행률 표시줄](toast-progress-bar.md)을 참조 하세요.


## <a name="headers"></a>헤더

**크리에이터 업데이트의 새로운** 기능: 알림 센터 내의 헤더 아래에서 알림을 그룹화 할 수 있습니다. 예를 들어 머리글 아래에 있는 그룹 채팅의 메시지 또는 머리글 아래에 있는 공통 테마의 그룹 알림을 그룹화 할 수 있습니다.

<img alt="Toasts with header" src="images/toast-headers-action-center.png" width="396"/>

헤더를 사용 하는 방법에 대 한 자세한 내용은 [알림 헤더](toast-headers.md)를 참조 하세요.


## <a name="adaptive-content"></a>적응 콘텐츠

**기념일 업데이트의 새로운** 내용: 위에서 지정한 내용 외에도 알림이 확장 될 때 표시 되는 추가 적응 콘텐츠를 표시할 수 있습니다.

이 추가 콘텐츠는 적응 [타일 설명서](create-adaptive-tiles.md)를 읽어 자세히 알아볼 수 있는 적응을 사용 하 여 지정 됩니다.

적응 콘텐츠는 [**AdaptiveGroup**](./toast-schema.md#adaptivegroup)내에 포함 되어야 합니다. 그렇지 않으면 적응을 사용 하 여 렌더링 되지 않습니다.


### <a name="columns-and-text-elements"></a>열 및 텍스트 요소

열 및 일부 고급 적응 텍스트 요소가 사용 되는 예는 다음과 같습니다. 텍스트 요소는 **AdaptiveGroup** 내에 있기 때문에 다양 한 적응 스타일 지정 속성을 지원 합니다.

<img alt="Toast with additional text" src="images/toast-additionaltext.jpg" width="364"/>

#### <a name="builder-syntax"></a>[작성기 구문](#tab/builder-syntax)

```csharp
new ToastContentBuilder()
    ...
    
    .AddVisualChild(new AdaptiveGroup()
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
    });
```

#### <a name="xml"></a>[XML](#tab/xml)

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

---


## <a name="buttons"></a>단추

단추는 사용자가 현재 워크플로를 중단 하지 않고 알림 메시지에 대 한 빠른 작업을 수행할 수 있도록 하는 알림 메시지를 대화형으로 만듭니다. 예를 들어 사용자는 알림 내에서 직접 메시지에 회신 하거나 메일 앱을 열지 않고도 전자 메일을 삭제할 수 있습니다. 사용자 알림의 확장 된 부분에 단추가 표시 됩니다.

종단 간 단추 구현에 대 한 자세한 내용은 [로컬 알림 보내기](send-local-toast.md)를 참조 하세요.

단추는 다음과 같은 다른 작업을 수행할 수 있습니다.

-   특정 페이지/컨텍스트를 탐색 하는 데 사용할 수 있는 인수를 사용 하 여 포그라운드에서 앱을 활성화 합니다.
-   빠른 회신 또는 유사한 시나리오에 대 한 앱의 백그라운드 작업 활성화
-   프로토콜 시작을 통해 다른 앱을 활성화 합니다.
-   Snoozing 또는 알림 해제와 같은 시스템 작업을 수행 합니다.

> [!NOTE]
> 단추를 5 개 까지만 포함할 수 있습니다 (나중에 설명 하는 상황에 맞는 메뉴 항목 포함).

<img alt="notification with actions, example 1" src="images/adaptivetoasts-xmlsample02.jpg" width="364"/>

#### <a name="builder-syntax"></a>[작성기 구문](#tab/builder-syntax)

```csharp
new ToastContentBuilder()
    ...
    
    .AddButton("See more details", ToastActivationType.Foreground, "action=viewdetails&contentId=351")
    .AddButton("Remind me later", ToastActivationType.Background, "action=remindlater&contentId=351");
```

#### <a name="xml"></a>[XML](#tab/xml)

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

---


### <a name="buttons-with-icons"></a>아이콘이 있는 단추

단추에 아이콘을 추가할 수 있습니다. 이러한 아이콘은 크기 조정 100%의 흰색 투명 16x16 픽셀 이미지 이며, 이미지 자체에는 패딩이 포함 되지 않아야 합니다. 알림 메시지에 아이콘을 제공 하도록 선택 하는 경우 단추의 스타일을 아이콘 단추로 변형 하므로 알림의 모든 단추에 대 한 아이콘을 제공 해야 합니다.

> [!NOTE]
> 내게 필요한 옵션에 대 한 자세한 내용은 아이콘의 대비 흰색 버전을 포함 해야 합니다. 즉, 흰색 배경의 검정색 아이콘을 사용 하 여 사용자 고대비가 흰색 모드를 켜면 아이콘이 표시 됩니다. [알림 내게 필요한 옵션 페이지](tile-toast-language-scale-contrast.md)에서 자세히 알아보세요.

<img src="images\adaptivetoasts-buttonswithicons.png" width="364" alt="Toast that has buttons with icons"/>

#### <a name="builder-syntax"></a>[작성기 구문](#tab/builder-syntax)

```csharp
new ToastContentBuilder()
    ...
    
    .AddButton(
        "Dismiss",
        ToastActivationType.Foreground,
        "dismiss", new Uri("Assets/NotificationButtonIcons/Dismiss.png", UriKind.Relative));
```

#### <a name="xml"></a>[XML](#tab/xml)

```xml
<action
    content="Dismiss"
    imageUri="Assets/NotificationButtonIcons/Dismiss.png"
    arguments="dismiss"
    activationType="background"/>
```

---


### <a name="buttons-with-pending-update-activation"></a>업데이트 활성화가 보류 중인 단추

**새 기능 작성자 업데이트**: 백그라운드 활성화 단추를 사용 하는 경우 **PendingUpdate** 의 after 활성화 동작을 사용 하 여 알림 메시지에 다중 단계 상호 작용을 만들 수 있습니다. 사용자가 단추를 클릭 하면 백그라운드 작업이 활성화 되 고 알림이 "보류 중인 업데이트" 상태로 전환 됩니다. 그러면 백그라운드 작업에서 알림이 새 알림으로 바뀔 때까지 화면에 남아 있습니다.

이를 구현 하는 방법을 알아보려면 [알림 보류 중 업데이트](toast-pending-update.md)를 참조 하세요.

![업데이트 보류 중인 알림](images/toast-pendingupdate.gif)


### <a name="context-menu-actions"></a>상황에 맞는 메뉴 작업

**기념일 업데이트의 새로운** 기능: 사용자가 알림 센터 내에서 알림을 마우스 오른쪽 단추로 클릭할 때 나타나는 기존 상황에 맞는 메뉴에 상황에 맞는 메뉴 작업을 추가할 수 있습니다. 이 메뉴는 동작 센터에서 마우스 오른쪽 단추를 클릭 한 경우에만 표시 됩니다. 알림 팝업 배너를 마우스 오른쪽 단추로 클릭 하면 표시 되지 않습니다.

> [!NOTE]
> 이전 장치에서는 이러한 추가 상황에 맞는 메뉴 작업이 알림에서 일반 단추로 표시 됩니다.

추가 하는 상황에 맞는 메뉴 작업 (예: "위치 변경")은 두 개의 기본 시스템 항목 위에 표시 됩니다.

<img alt="Toast with context menu" src="images/toast-contextmenu.png" width="444"/>

#### <a name="builder-syntax"></a>[작성기 구문](#tab/builder-syntax)

작성기 구문은 상황에 맞는 메뉴 작업을 지원 하지 않으므로 이니셜라이저 구문을 사용 하는 것이 좋습니다.

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

#### <a name="xml"></a>[XML](#tab/xml)

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

---

> [!NOTE]
> 추가 상황에 맞는 메뉴 항목은 알림에서 5 개 단추의 총 제한에 영향을 주지 않습니다.

추가 상황에 맞는 메뉴 항목의 활성화는 알림 단추와 동일 하 게 처리 됩니다.


## <a name="inputs"></a>입력

입력은 알림 메시지의 알림 영역에 있는 작업 영역 내에서 지정 됩니다. 즉, 알림이 확장 된 경우에만 표시 됩니다.


### <a name="quick-reply-text-box"></a>빠른 회신 텍스트 상자

빠른 회신 텍스트 상자를 사용 하도록 설정 하려면 (예: 메시징 앱에서) 텍스트 입력과 단추를 추가 하 고 텍스트 입력 필드의 ID를 참조 하 여 입력 필드 옆에 단추가 표시 되도록 합니다. 단추의 아이콘은 안쪽 여백이 없고, 흰색 픽셀이 투명으로 설정 되 고, 100% 배율로 설정 된 32x32 픽셀 이미지 여야 합니다.

<img alt="notification with text input and actions" src="images/adaptivetoasts-xmlsample05.jpg" width="364"/>

#### <a name="builder-syntax"></a>[작성기 구문](#tab/builder-syntax)

```csharp
new ToastContentBuilder()
    ...
    
    .AddInputTextBox("tbReply", "Type a reply")

    .AddButton(
        textBoxId: "tbReply", // To place button next to text box, reference text box's id
        content: "Reply",
        activationType: ToastActivationType.Background,
        arguments: "action=reply&convId=9318",
        imageUri: new Uri("Assets/Reply.png", UriKind.Relative));
```

#### <a name="xml"></a>[XML](#tab/xml)

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

---



### <a name="inputs-with-buttons-bar"></a>단추 모음이 포함 된 입력

입력 아래에 일반 단추가 표시 된 하나 이상의 입력을 사용할 수도 있습니다.

<img alt="notification with text and input actions" src="images/adaptivetoasts-xmlsample04.jpg" width="364"/>

#### <a name="builder-syntax"></a>[작성기 구문](#tab/builder-syntax)

```csharp
new ToastContentBuilder()
    ...
    
    .AddInputTextBox("tbReply", "Type a reply")

    .AddButton("Reply", ToastActivationType.Background, "action=reply&threadId=9218")
    .AddButton("Video call", ToastActivationType.Foreground, "action=videocall&threadId=9218");
```

#### <a name="xml"></a>[XML](#tab/xml)

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

---


### <a name="selection-input"></a>선택 입력

텍스트 상자 외에도 선택 메뉴를 사용할 수 있습니다.

<img alt="notification with selection input and actions" src="images/adaptivetoasts-xmlsample06.jpg" width="364"/>

#### <a name="builder-syntax"></a>[작성기 구문](#tab/builder-syntax)

```csharp
new ToastContentBuilder()
    ...
    
    .AddToastInput(new ToastSelectionBox("time")
    {
        DefaultSelectionBoxItemId = "lunch",
        Items =
        {
            new ToastSelectionBoxItem("breakfast", "Breakfast"),
            new ToastSelectionBoxItem("lunch", "Lunch"),
            new ToastSelectionBoxItem("dinner", "Dinner")
        }
    })

    .AddButton(...)
    .AddButton(...);
```

#### <a name="xml"></a>[XML](#tab/xml)

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

---



### <a name="snoozedismiss"></a>다시 알림/해제

선택 메뉴와 두 개의 단추를 사용 하 여 시스템 다시 알림 및 해제 작업을 활용 하는 미리 알림 알림을 만들 수 있습니다. 알림이 미리 알림 처럼 동작 하도록 시나리오를 미리 알림으로 설정 해야 합니다.

<img alt="reminder notification" src="images/adaptivetoasts-xmlsample07.jpg" width="364"/>

알림 단추에서 **Selectionboxid** 속성을 사용 하 여 선택 메뉴 입력에 다시 알림 단추를 연결 합니다.

#### <a name="builder-syntax"></a>[작성기 구문](#tab/builder-syntax)

```csharp
new ToastContentBuilder()
    .SetToastScenario(ToastScenario.Reminder)
    
    ...
    
    .AddToastInput(new ToastSelectionBox("snoozeTime")
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
    })

    .AddButton(new ToastButtonSnooze() { SelectionBoxId = "snoozeTime" })
    .AddButton(new ToastButtonDismiss());
```

#### <a name="xml"></a>[XML](#tab/xml)

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

---

시스템을 다시 알림 및 해제 작업을 사용 하려면:

-   **To Button다시 알림** 또는 **to buttonno** 를 지정 합니다.
-   선택적으로 사용자 지정 콘텐츠 문자열을 지정 합니다.
    -   문자열을 제공 하지 않는 경우 "다시 알림" 및 "해제"에 대해 지역화 된 문자열을 자동으로 사용 합니다.
-   필요에 따라 **Selectionboxid** 를 지정 합니다.
    -   사용자가 다시 알림 간격을 선택 하지 않고 시스템에 정의 된 시간 간격 (OS 전체에 걸쳐 일치)에 대해 알림을 한 번만 다시 알리도록 하려는 경우에는 어떠한 입력도 구성 하지 않습니다 &lt; &gt; .
    -   다시 알림 간격 선택 항목을 제공 하려면 다음을 수행 합니다.
        -   다시 알림 작업에서 **Selectionboxid** 를 지정 합니다.
        -   입력의 id를 다시 알림 동작의 **Selectionboxid** 와 일치 시킵니다.
        -   알림 간격 (분)을 나타내는 nonNegativeInteger로 **Toastselectionboxitem** 의 값을 지정 합니다.



## <a name="audio"></a>오디오

사용자 지정 오디오는 항상 모바일에서 지원 되며 데스크톱 버전 1511 (빌드 10586) 이상에서 지원 됩니다. 사용자 지정 오디오는 다음 경로를 통해 참조할 수 있습니다.

-   ms-appx:///
-   ms-appdata:///

또는 항상 두 플랫폼에서 모두 지원 되는 [winsoundevents 목록](/uwp/schemas/tiles/toastschema/element-audio#attributes-and-elements)에서 선택할 수 있습니다.

#### <a name="builder-syntax"></a>[작성기 구문](#tab/builder-syntax)

```csharp
new ToastContentBuilder()
    ...
    
    .AddAudio(new Uri("ms-appx:///Assets/NewMessage.mp3"));
```

#### <a name="xml"></a>[XML](#tab/xml)

```xml
<toast launch="app-defined-string">

    ...

    <audio src="ms-appx:///Assets/NewMessage.mp3"/>

</toast>
```

---


알림 메시지의 오디오에 대 한 정보는 [오디오 스키마 페이지](/uwp/schemas/tiles/toastschema/element-audio) 를 참조 하세요. 사용자 지정 오디오를 사용 하 여 알림 메시지를 보내는 방법을 알아보려면 [알림을의 사용자 지정 오디오](custom-audio-on-toasts.md)를 참조 하세요.


## <a name="alarms-reminders-and-incoming-calls"></a>경보, 미리 알림 및 들어오는 호출

경보, 미리 알림 및 들어오는 통화 알림을 만들려면 시나리오 값이 할당 된 일반 알림 메시지를 사용 하면 됩니다. 시나리오는 일관적인 통합 사용자 환경을 만들기 위한 몇 가지 동작을 adusts 합니다.

> [!IMPORTANT]
> 미리 알림 또는 경보를 사용 하는 경우 알림 메시지에 하나 이상의 단추를 제공 해야 합니다. 그렇지 않으면 알림이 일반적인 알림 메시지로 처리 됩니다.

* **미리 알림**: 사용자가 작업을 해제 하거나 작업을 수행할 때까지 알림이 화면에 계속 유지 됩니다. Windows Mobile에서 알림 메시지에는 미리 확장 됨도 표시 됩니다. 미리 알림 소리가 재생 됩니다.
* **경보**: 미리 알림 동작 외에도 알람은 기본 알람 소리를 사용 하 여 오디오를 추가로 반복 합니다.
* **IncomingCall**: 들어오는 통화 알림은 Windows Mobile 장치에 전체 화면으로 표시 됩니다. 그렇지 않은 경우에는 벨 소리를 사용 하 고 단추의 단추가 다르게 지정 되는 경우를 제외 하 고는 경보와 동일한 동작이 적용 됩니다.

#### <a name="builder-syntax"></a>[작성기 구문](#tab/builder-syntax)

```csharp
new ToastContentBuilder()
    .SetToastScenario(ToastScenario.Reminder)
    ...
```

#### <a name="xml"></a>[XML](#tab/xml)

```xml
<toast scenario="reminder" launch="app-defined-string">

    ...

</toast>
```

---


## <a name="localization-and-accessibility"></a>지역화 및 내게 필요한 옵션

타일과 알림을는 표시 언어, 표시 배율 비율, 고대비 및 기타 런타임 컨텍스트에 맞게 조정 된 문자열과 이미지를 로드할 수 있습니다. 자세한 내용은 [언어, 규모 및 고대비에 대 한 타일 및 알림 알림 지원](tile-toast-language-scale-contrast.md)을 참조 하세요.


## <a name="handling-activation"></a>활성화 처리
알림 활성화를 처리 하는 방법을 알아보려면 (알림 메시지를 클릭 하는 사용자) [로컬 알림 보내기](send-local-toast.md)를 참조 하세요.
 
## <a name="related-topics"></a>관련 항목

* [로컬 알림 보내기 및 활성화 처리](send-local-toast.md)
* [GitHub의 알림 라이브러리 (UWP 커뮤니티 도구 키트의 일부)](https://github.com/windows-toolkit/WindowsCommunityToolkit/tree/master/Microsoft.Toolkit.Uwp.Notifications)
* [언어, 규모 및 고대비에 대 한 타일 및 알림 알림 지원](tile-toast-language-scale-contrast.md)
