---
description: 이 문서에서는 적응 타일 템플릿을 사용 하 여 기본 타일 및 보조 타일에 로컬 타일 알림을 보내는 방법을 설명 합니다.
title: 로컬 타일 알림 보내기
ms.assetid: D34B0514-AEC6-4C41-B318-F0985B51AF8A
template: detail.hbs
ms.date: 05/19/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 6b93f9731fb9bf843ce9bb03bd8c6526546c4249
ms.sourcegitcommit: a3bbd3dd13be5d2f8a2793717adf4276840ee17d
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/30/2020
ms.locfileid: "93034466"
---
# <a name="send-a-local-tile-notification"></a>로컬 타일 알림 보내기
 

Windows 10의 기본 앱 타일은 앱 매니페스트에서 정의 되 고, 보조 타일은 앱 코드에 의해 프로그래밍 방식으로 만들어지고 정의 됩니다. 이 문서에서는 적응 타일 템플릿을 사용 하 여 기본 타일 및 보조 타일에 로컬 타일 알림을 보내는 방법을 설명 합니다. (로컬 알림은 웹 서버에서 푸시되는 것과는 반대로 응용 프로그램 코드에서 전송 되는 알림입니다.)

![기본 타일 및 알림이 포함 된 타일](images/sending-local-tile-01.png)

> [!NOTE] 
>[적응 타일](create-adaptive-tiles.md) 및 [타일 콘텐츠 스키마](../tiles-and-notifications/tile-schema.md)를 만드는 방법에 대해 알아봅니다.

 

## <a name="install-the-nuget-package"></a>NuGet 패키지 설치


[알림 라이브러리 NuGet 패키지](https://www.nuget.org/packages/Microsoft.Toolkit.Uwp.Notifications/)를 설치 하는 것이 좋습니다 .이 패키지는 원시 XML 대신 개체를 사용 하 여 타일 페이로드를 생성 하 여 작업을 단순화 합니다.

이 문서의 인라인 코드 예제는 알림 라이브러리를 사용 하는 c #에 대 한 것입니다. 사용자 고유의 XML을 만들려고 하는 경우 문서 끝에 대 한 알림 라이브러리 없이 코드 예제를 찾을 수 있습니다.

## <a name="add-namespace-declarations"></a>네임스페이스 선언 추가


타일 Api에 액세스 하려면 [**Windows. notification**](/uwp/api/Windows.UI.Notifications) 네임 스페이스를 포함 합니다. 또한 타일 도우미 Api를 활용할 수 있도록 **Microsoft Toolkit. notification** 네임 스페이스를 포함 하는 것이 좋습니다 (이러한 api에 액세스 하려면 [알림 라이브러리](https://www.nuget.org/packages/Microsoft.Toolkit.Uwp.Notifications/) NuGet 패키지를 설치 해야 함).

```csharp
using Windows.UI.Notifications;
using Microsoft.Toolkit.Uwp.Notifications; // Notifications library
```

## <a name="create-the-notification-content"></a>알림 콘텐츠 만들기


Windows 10에서 타일 페이로드는 사용자 알림에 대 한 사용자 지정 시각적 개체 레이아웃을 만들 수 있도록 하는 적응 타일 템플릿을 사용 하 여 정의 됩니다. 적응 타일을 사용 하 여 수행할 수 있는 작업에 대 한 자세한 내용은 [적응 타일 만들기](create-adaptive-tiles.md)를 참조 하세요.

이 코드 예제에서는 중간 및 광범위 타일에 대 한 적응 타일 콘텐츠를 만듭니다.

```csharp
// In a real app, these would be initialized with actual data
string from = "Jennifer Parker";
string subject = "Photos from our trip";
string body = "Check out these awesome photos I took while in New Zealand!";
 
 
// Construct the tile content
TileContent content = new TileContent()
{
    Visual = new TileVisual()
    {
        TileMedium = new TileBinding()
        {
            Content = new TileBindingContentAdaptive()
            {
                Children =
                {
                    new AdaptiveText()
                    {
                        Text = from
                    },

                    new AdaptiveText()
                    {
                        Text = subject,
                        HintStyle = AdaptiveTextStyle.CaptionSubtle
                    },

                    new AdaptiveText()
                    {
                        Text = body,
                        HintStyle = AdaptiveTextStyle.CaptionSubtle
                    }
                }
            }
        },

        TileWide = new TileBinding()
        {
            Content = new TileBindingContentAdaptive()
            {
                Children =
                {
                    new AdaptiveText()
                    {
                        Text = from,
                        HintStyle = AdaptiveTextStyle.Subtitle
                    },

                    new AdaptiveText()
                    {
                        Text = subject,
                        HintStyle = AdaptiveTextStyle.CaptionSubtle
                    },

                    new AdaptiveText()
                    {
                        Text = body,
                        HintStyle = AdaptiveTextStyle.CaptionSubtle
                    }
                }
            }
        }
    }
};
```

중간 타일에 표시 되는 경우 알림 콘텐츠는 다음과 같습니다.

![미디어 타일의 알림 콘텐츠](images/sending-local-tile-02.png)

## <a name="create-the-notification"></a>알림 만들기


알림 콘텐츠가 있으면 새 [**TileNotification**](/uwp/api/Windows.UI.Notifications.TileNotification)를 만들어야 합니다. **TileNotification** 생성자는 Windows 런타임 [**XmlDocument**](/uwp/api/windows.data.xml.dom.xmldocument) 개체를 사용 합니다 .이 개체는 [알림 라이브러리](https://www.nuget.org/packages/Microsoft.Toolkit.Uwp.Notifications/)를 사용 하는 경우 **TileContent GetXml** 메서드에서 가져올 수 있습니다.

이 코드 예제에서는 새 타일에 대 한 알림을 만듭니다.

```csharp
// Create the tile notification
var notification = new TileNotification(content.GetXml());
```

## <a name="set-an-expiration-time-for-the-notification-optional"></a>알림에 대 한 만료 시간 설정 (선택 사항)


기본적으로 로컬 타일 및 배지 알림은 만료 되지 않지만 푸시, 정기 및 예약 된 알림은 3 일 후에 만료 됩니다. 타일 콘텐츠는 필요한 것 보다 더 오래 지속 되어서는 안 되므로 앱에 적합 한 만료 시간을 설정 하는 것이 가장 좋습니다. 특히 로컬 타일 및 배지 알림에 사용 됩니다.

이 코드 예제에서는 10 분 후에 만료 되어 타일에서 제거 되는 알림을 만듭니다.

```csharp
tileNotification.ExpirationTime = DateTimeOffset.UtcNow.AddMinutes(10);
```

## <a name="send-the-notification"></a>알림 보내기


타일 알림은 로컬에서 간단 하 게 보낼 수 있지만 기본 또는 보조 타일에 알림을 보내는 것은 약간 다릅니다.

**기본 타일**

기본 타일에 알림을 보내려면 [**TileUpdateManager**](/uwp/api/Windows.UI.Notifications.TileUpdateManager) 를 사용 하 여 기본 타일에 대 한 타일 업데이트 프로그램을 만들고 "업데이트"를 호출 하 여 알림을 보냅니다. 표시 되는지 여부에 관계 없이 앱의 기본 타일이 항상 존재 하므로 고정 되지 않은 경우에도 알림을 보낼 수 있습니다. 사용자가 나중에 기본 타일을 고정 하면 전송 된 알림이 표시 됩니다.

이 코드 예제에서는 기본 타일에 알림을 보냅니다.


```csharp
// Send the notification to the primary tile
TileUpdateManager.CreateTileUpdaterForApplication().Update(notification);
```

**보조 타일**

보조 타일에 알림을 보내려면 먼저 보조 타일이 있는지 확인 합니다. 존재 하지 않는 보조 타일에 대 한 타일 업데이트 프로그램을 만들려고 하면 (예: 사용자가 보조 타일을 고정 해제 한 경우) 예외가 throw 됩니다. [**Secondarytile. Exists**](/uwp/api/Windows.UI.StartScreen.SecondaryTile#Windows_UI_StartScreen_SecondaryTile_Exists_System_String_)(tileId)를 사용 하 여 보조 타일이 고정 되어 있는지 검색 한 다음 보조 타일에 대 한 타일 업데이트 프로그램을 만들고 알림을 보낼 수 있습니다.

이 코드 예제는 보조 타일에 알림을 보냅니다.

```csharp
// If the secondary tile is pinned
if (SecondaryTile.Exists("MySecondaryTile"))
{
    // Get its updater
    var updater = TileUpdateManager.CreateTileUpdaterForSecondaryTile("MySecondaryTile");
 
    // And send the notification
    updater.Update(notification);
}
```

![기본 타일 및 알림이 포함 된 타일](images/sending-local-tile-01.png)

## <a name="clear-notifications-on-the-tile-optional"></a>타일에 대 한 알림 지우기 (선택 사항)


대부분의 경우 사용자가 해당 콘텐츠와 상호 작용 하면 알림을 지워야 합니다. 예를 들어 사용자가 앱을 시작할 때 타일에서 모든 알림을 지울 수 있습니다. 알림이 시간 범위에 속하는 경우 알림을 명시적으로 지우는 대신 알림에 만료 시간을 설정 하는 것이 좋습니다.

이 코드 예제는 기본 타일에 대 한 타일 알림을 지웁니다. 보조 타일에 대 한 타일 업데이트 프로그램을 만들어 보조 타일에 대해 동일한 작업을 수행할 수 있습니다.

```csharp
TileUpdateManager.CreateTileUpdaterForApplication().Clear();
```

알림 큐가 활성화 되 고 큐에서 알림이 있는 타일의 경우 Clear 메서드를 호출 하면 큐가 비워집니다. 그러나 앱 서버를 통해 알림을 지울 수는 없습니다. 로컬 앱 코드만 알림을 지울 수 있습니다.

정기적 또는 푸시 알림은 새 알림만 추가 하거나 기존 알림만 바꿀 수 있습니다. Clear 메서드에 대 한 로컬 호출은 알림이 푸시, 주기적 또는 로컬을 통해 제공 되는지 여부에 관계 없이 타일을 지웁니다. 아직 표시 되지 않은 예약 된 알림은이 메서드에서 지워지지 않습니다.

![선택이 취소 된 후 알림 및 타일이 있는 타일](images/sending-local-tile-03.png)

## <a name="next-steps"></a>다음 단계


**알림 큐 사용**

첫 번째 타일 업데이트를 완료 했으므로 [알림 큐](/previous-versions/windows/apps/hh868234(v=win.10))를 사용 하도록 설정 하 여 타일의 기능을 확장할 수 있습니다.

**기타 알림 배달 방법**

이 문서에서는 타일 업데이트를 알림으로 보내는 방법을 보여 줍니다. 예약 됨, 정기 및 푸시를 포함 하 여 다른 알림 배달 방법을 살펴보려면 [알림](choosing-a-notification-delivery-method.md)배달을 참조 하세요.

**XmlEncode 배달 방법**

[알림 라이브러리](https://www.nuget.org/packages/Microsoft.Toolkit.Uwp.Notifications/)를 사용 하 고 있지 않은 경우에는이 알림 배달 방법이 또 다른 대안입니다.


```csharp
public string XmlEncode(string text)
{
    StringBuilder builder = new StringBuilder();
    using (var writer = XmlWriter.Create(builder))
    {
        writer.WriteString(text);
    }

    return builder.ToString();
}
```

## <a name="code-examples-without-notifications-library"></a>알림 라이브러리가 없는 코드 예제


[알림 라이브러리](https://www.nuget.org/packages/Microsoft.Toolkit.Uwp.Notifications/) NuGet 패키지 대신 원시 XML로 작업 하려는 경우이 문서에 제공 된 세 가지 예제에 대 한 대체 코드 예제를 사용 합니다. 나머지 코드 예제는 [알림 라이브러리나](https://www.nuget.org/packages/Microsoft.Toolkit.Uwp.Notifications/) 원시 XML과 함께 사용할 수 있습니다.

네임스페이스 선언 추가

```csharp
using Windows.UI.Notifications;
using Windows.Data.Xml.Dom;
```

알림 콘텐츠 만들기

```csharp
// In a real app, these would be initialized with actual data
string from = "Jennifer Parker";
string subject = "Photos from our trip";
string body = "Check out these awesome photos I took while in New Zealand!";
 
 
// TODO - all values need to be XML escaped
 
 
// Construct the tile content as a string
string content = $@"
<tile>
    <visual>
 
        <binding template='TileMedium'>
            <text>{from}</text>
            <text hint-style='captionSubtle'>{subject}</text>
            <text hint-style='captionSubtle'>{body}</text>
        </binding>
 
        <binding template='TileWide'>
            <text hint-style='subtitle'>{from}</text>
            <text hint-style='captionSubtle'>{subject}</text>
            <text hint-style='captionSubtle'>{body}</text>
        </binding>
 
    </visual>
</tile>";
```

알림 만들기

```csharp
// Load the string into an XmlDocument
XmlDocument doc = new XmlDocument();
doc.LoadXml(content);
 
// Then create the tile notification
var notification = new TileNotification(doc);
```

## <a name="related-topics"></a>관련 항목

* [적응형 타일 만들기](create-adaptive-tiles.md)
* [타일 콘텐츠 스키마](../tiles-and-notifications/tile-schema.md)
* [알림 라이브러리](https://www.nuget.org/packages/Microsoft.Toolkit.Uwp.Notifications/)
* [GitHub의 전체 코드 샘플](https://github.com/WindowsNotifications/quickstart-sending-local-tile-win10)
* [**Windows. Notification 네임 스페이스**](/uwp/api/Windows.UI.Notifications)
* [알림 큐를 사용 하는 방법 (XAML)](/previous-versions/windows/apps/hh868234(v=win.10))
* [알림 배달](choosing-a-notification-delivery-method.md)
 

 
