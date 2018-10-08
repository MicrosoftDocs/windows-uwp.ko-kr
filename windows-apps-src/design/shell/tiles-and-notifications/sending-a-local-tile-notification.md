---
author: andrewleader
Description: This article describes how to send a local tile notification to a primary tile and a secondary tile using adaptive tile templates.
title: 로컬 타일 알림 보내기
ms.assetid: D34B0514-AEC6-4C41-B318-F0985B51AF8A
template: detail.hbs
ms.author: mijacobs
ms.date: 05/19/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: Windows 10 uwp
ms.localizationpriority: medium
ms.openlocfilehash: 7e91d4bd481188f4d29af68af2c4572b26d446ae
ms.sourcegitcommit: fbdc9372dea898a01c7686be54bea47125bab6c0
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/08/2018
ms.locfileid: "4421103"
---
# <a name="send-a-local-tile-notification"></a>로컬 타일 알림 보내기
 

Windows 10의 기본 앱 타일은 앱 매니페스트에서 정의되는 반면, 보조 타일은 프로그래밍 방식으로 앱 코드에서 생성 및 정의됩니다. 이 문서에서는 적응형 타일 템플릿을 사용하여 기본 타일 및 보조 타일에 로컬 타일 알림을 보내는 방법을 설명합니다. 로컬 알림은 웹 서버에서 푸시되거나 끌어온 알림이 아니라 앱 코드에서 전송된 알림입니다.

![기본 타일 및 알림 타일](images/sending-local-tile-01.png)

> [!NOTE] 
>[적응형 타일 만들기](create-adaptive-tiles.md) 및 [타일 템플릿 스키마](../tiles-and-notifications/tile-schema.md)에 대해 자세히 알아봅니다.

 

## <a name="install-the-nuget-package"></a>NuGet 패키지 설치


원시 XML 대신 개체로 타일 페이로드를 생성하여 작업을 간소화하는 [알림 라이브러리 NuGet 패키지](https://www.nuget.org/packages/Microsoft.Toolkit.Uwp.Notifications/)를 설치하는 것이 좋습니다.

이 문서에 있는 인라인 코드 예제는 알림 라이브러리를 사용하는 C#용입니다. 고유한 XML을 만들려는 경우 문서의 끝 부분에서 알림 라이브러리가 없는 코드 예제를 찾을 수 있습니다.

## <a name="add-namespace-declarations"></a>네임스페이스 선언 추가


타일 API에 액세스하려면 [**Windows.UI.Notifications**](https://docs.microsoft.com/uwp/api/Windows.UI.Notifications) 네임스페이스를 포함합니다. 타일 도우미 API를 활용할 수 있도록 **Microsoft.Toolkit.Uwp.Notifications** 네임스페이스를 포함하는 것이 좋습니다(이러한 API에 액세스하려면 [알림 라이브러리](https://www.nuget.org/packages/Microsoft.Toolkit.Uwp.Notifications/) NuGet 패키지를 설치해야 함).

```csharp
using Windows.UI.Notifications;
using Microsoft.Toolkit.Uwp.Notifications; // Notifications library
```

## <a name="create-the-notification-content"></a>알림 콘텐츠 만들기


Windows 10에서 타일 페이로드는 알림에 대한 사용자 지정 시각적 레이아웃을 만들 수 있는 적응형 타일 템플릿을 사용하여 정의됩니다. 적응형 타일로 수행할 수 있는 작업에 대한 자세한 내용은 [적응형 타일 만들기](create-adaptive-tiles.md)를 참조하세요.

이 코드 예제에서는 중간 크기 및 와이드 타일에 대한 적응형 타일 콘텐츠를 만듭니다.

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

알림 콘텐츠는 중간 크기 타일에 표시될 때 다음과 같이 나타납니다.

![중간 크기 타일의 알림 콘텐츠](images/sending-local-tile-02.png)

## <a name="create-the-notification"></a>알림 만들기


알림 콘텐츠가 있으면 새 [**TileNotification**](https://docs.microsoft.com/uwp/api/Windows.UI.Notifications.TileNotification)을 만들어야 합니다. **TileNotification** 생성자는 [알림 라이브러리](https://docs.microsoft.com/uwp/api/windows.data.xml.dom.xmldocument)를 사용하는 경우 **TileContent.GetXml** 메서드에서 얻을 수 있는 Windows 런타임 [**XmlDocument**](https://www.nuget.org/packages/Microsoft.Toolkit.Uwp.Notifications/) 개체를 사용합니다.

이 코드 예제에서는 새 타일에 대한 알림을 만듭니다.

```csharp
// Create the tile notification
var notification = new TileNotification(content.GetXml());
```

## <a name="set-an-expiration-time-for-the-notification-optional"></a>알림의 만료 시간 설정(옵션)


기본적으로 로컬 타일과 배지 알림은 만료되지 않는 반면 푸시 알림, 정기 알림 및 예약된 알림은 3일 후에 만료됩니다. 타일 콘텐츠가 필요 이상 오래 유지되면 안 되므로 특히 로컬 타일과 배지 알림에는 앱에 적합한 만료 시간을 설정하는 것이 좋습니다.

이 코드 예제에서는 10분 후에 만료되고 타일에서 제거되는 알림을 만듭니다.

```csharp
tileNotification.ExpirationTime = DateTimeOffset.UtcNow.AddMinutes(10);
```

## <a name="send-the-notification"></a>알림 보내기


로컬에 타일 알림 전송은 간단하지만 기본 타일이나 보조 타일에 알림 전송은 약간 다릅니다.

**기본 타일**

기본 타일에 알림을 보내려면 [**TileUpdateManager**](https://docs.microsoft.com/uwp/api/Windows.UI.Notifications.TileUpdateManager)를 사용하여 기본 타일에 대한 타일 업데이트 프로그램을 만들고 "업데이트"를 호출하여 알림을 보냅니다. 표시 여부에 관계없이 앱의 기본 타일은 항상 존재하므로 고정되지 않은 경우에도 알림을 보낼 수 있습니다. 사용자가 나중에 기본 타일을 고정하면 전송한 알림이 그때 표시됩니다.

이 코드 예제에서는 기본 타일에 알림을 보냅니다.


```csharp
// Send the notification to the primary tile
TileUpdateManager.CreateTileUpdaterForApplication().Update(notification);
```

**보조 타일**

보조 타일에 알림을 보내려면 먼저 보조 타일이 있는지 확인합니다. 존재하지 않는 보조 타일에 대한 타일 업데이트 프로그램을 만들려고 하면(예: 사용자가 보조 타일 고정을 해제한 경우) 예외가 발생합니다. [**SecondaryTile.Exists**](https://docs.microsoft.com/uwp/api/Windows.UI.StartScreen.SecondaryTile#Windows_UI_StartScreen_SecondaryTile_Exists_System_String_)(tileId)를 사용하여 보조 타일이 고정되었는지 검색한 다음 보조 타일에 대한 타일 업데이트 프로그램을 만들고 알림을 보낼 수 있습니다.

이 코드 예제에서는 보조 타일에 알림을 보냅니다.

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

![기본 타일 및 알림 타일](images/sending-local-tile-01.png)

## <a name="clear-notifications-on-the-tile-optional"></a>타일에서 알림 지우기(옵션)


대부분의 경우 사용자가 해당 콘텐츠를 조작한 후에는 알림을 지워야 합니다. 예를 들어 사용자가 앱을 시작하면 타일에서 모든 알림을 지우는 것이 좋습니다. 알림의 시간이 제한된 경우 알림을 명시적으로 지우는 대신 알림에 만료 시간을 설정하는 것이 좋습니다.

이 코드 예제에서는 기본 타일에 대한 타일 알림을 지웁니다. 보조 타일에 대한 타일 업데이트 프로그램을 만들어 보조 타일에 대해서도 동일한 작업을 수행할 수 있습니다.

```csharp
TileUpdateManager.CreateTileUpdaterForApplication().Clear();
```

알림 큐가 사용되고 큐에 알림이 있는 타일의 경우 Clear 메서드를 호출하면 큐가 비워집니다. 그러나 앱의 서버를 통해 알림을 지울 수는 없습니다, 로컬 앱 코드만 알림을 지울 수 있습니다.

정기 또는 푸시 알림은 새 알림을 추가하거나 기존 알림을 바꿀 수만 있습니다. Clear 메서드를 로컬에서 호출하면 알림 자체가 푸시, 정기 또는 로컬로 제공되었는지 여부에 관계없이 타일이 지워집니다. 아직 표시되지 않은 예약된 알림은 이 메서드로 지워지지 않습니다.

![알림이 있는 타일 및 지워진 후의 타일](images/sending-local-tile-03.png)

## <a name="next-steps"></a>다음 단계


**알림 큐 사용**

지금까지 첫 번째 타일 업데이트를 수행했으며 [알림 큐](https://msdn.microsoft.com/library/windows/apps/xaml/hh868234)를 사용하도록 설정하여 타일 기능을 확장할 수 있습니다.

**기타 알림 전달 방법**

이 문서에서는 타일 업데이트를 알림으로 보내는 방법을 보여 줍니다. 예약, 정기, 푸시 등 다른 알림 전달 방법을 탐색하려면 [알림 전달](choosing-a-notification-delivery-method.md)을 참조하세요.

**XmlEncode 전달 방법**

[알림 라이브러리](https://www.nuget.org/packages/Microsoft.Toolkit.Uwp.Notifications/)를 사용하지 않는 경우 이 알림 전달 방법이 다른 대체 방법입니다.


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

## <a name="code-examples-without-notifications-library"></a>알림 라이브러리를 사용하지 않는 코드 예제


[알림 라이브러리](https://www.nuget.org/packages/Microsoft.Toolkit.Uwp.Notifications/) NuGet 패키지 대신 원시 XML을 사용하려는 경우 이 문서에 제공된 처음 세 가지 예제 대신 이러한 대체 코드 예제를 사용합니다. 나머지 코드 예제는 [알림 라이브러리](https://www.nuget.org/packages/Microsoft.Toolkit.Uwp.Notifications/) 또는 원시 XML에서 사용할 수 있습니다.

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
* [**Windows.UI.Notifications 네임스페이스**](https://docs.microsoft.com/uwp/api/Windows.UI.Notifications)
* [알림 큐 사용 방법(XAML)](https://msdn.microsoft.com/library/windows/apps/xaml/hh868234)
* [알림 전달](choosing-a-notification-delivery-method.md)
 

 




