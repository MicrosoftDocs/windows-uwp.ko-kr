---
author: mijacobs
Description: Learn how to use tiles, badges, toasts, and notifications to provide entry points into your app and keep users up-to-date.
title: UWP 앱에 대한 배지 알림
ms.assetid: 48ee4328-7999-40c2-9354-7ea7d488c538
label: Tiles, badges, and notifications
template: detail.hbs
ms.author: mijacobs
ms.date: 05/19/2017
ms.topic: article
keywords: Windows 10 uwp
ms.localizationpriority: medium
ms.openlocfilehash: 667efeb67c956f8d4378b0e7e4011f7e06977519
ms.sourcegitcommit: 3257416aebb5a7b1515e107866806f8bd57845a8
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/17/2018
ms.locfileid: "7163668"
---
# <a name="badge-notifications-for-uwp-apps"></a>UWP 앱에 대한 배지 알림

 

<div style="float:left; font-size:80%; text-align:left; margin: 0px 15px 15px 0px;">
<img src="images/badge-example.png" alt="A tile with a numeric badge displaying the number 63 to indicate 63 unread mails." style="padding-bottom:0.0em; margin-bottom: 2px" /><br/>읽지 않은 메일 63개가 있다는 의미의 숫자 63이<br/> 표시된 숫자 배지를 포함하는 타일</div>

알림 배지는 앱에 대한 요약 또는 상태 정보를 전달합니다. 숫자(1-99) 또는 시스템 제공 문자 모양 중 하나일 수 있습니다. 배지를 통해 전달하기 적합한 정보로는 온라인 게임의 네트워크 연결 상태, 메시지 앱의 사용자 상태, 메일 앱의 읽지 않은 메일 수, 소셜 미디어 앱의 새 게시 수 등이 있습니다. 

알림 배지는 앱의 작업 표시줄 아이콘 및 시작 타일의 오른쪽 아래 모서리에 앱 실행 여부와 관계없이 나타납니다. 배지는 모든 타일 크기에 표시할 수 있습니다.  

> [!NOTE]
> 직접 만든 배지 이미지를 제공할 수는 없으며, 시스템 제공 배지 이미지만 사용할 수 있습니다.


## <a name="numeric-badges"></a>숫자 배지

<table>
    <tr>
        <th>값</th>
        <th>배지</th>
        <th>XML</th>
    </tr>
    <tr>
        <td>1-99 사이의 숫자. 값 0은 문자 값 "없음"과 동일하며 배지를 지웁니다.</td>
        <td><img src="images/badges/badge-numeric.png" alt="A numeric badge less than 100." /></td>
        <td>`<badge value="1"/>`</td>
    </tr>
    <tr>
        <td>99보다 큰 모든 숫자</td>
        <td><img src="images/badges/badge-numeric-greater.png" alt="A numeric badge greater than 99." /></td></td>
        <td>`<badge value="100"/>`</td>
    </tr>    
</table>

## <a name="glyph-badges"></a>문자 배지
배지는 숫자 대신 확장할 수 없는 상태 문자 모양 집합 중 하나를 표시할 수 있습니다. 

<table>
<tr>
    <th>상태</th>
    <th>문자 모양</th>
    <th>XML</th>
</tr>
<tr>
    <td>none</td>
    <td>표시되는 배지 없음</td>
    <td>`<badge value="none"/>`</td>
</tr>
<tr>
    <td>activity</td>
    <td><img src="images/badges/badge-activity.png" alt="Glyph" /></td>
    <td>`<badge value="activity"/>`</td>
</tr>
<tr>
    <td>alarm</td>
    <td><img src="images/badges/badge-alarm.png" alt="Glyph" /></td>
    <td>`<badge value="alarm"/>`</td>
</tr>
<tr>
    <td>alert</td>
    <td><img src="images/badges/badge-alert.png" alt="Glyph" /></td>
    <td>`<badge value="alert"/>`</td>
</tr>
<tr>
    <td>attention</td>
    <td><img src="images/badges/badge-attention.png" alt="Glyph" /></td>
    <td>`<badge value="attention"/>`</td>
</tr>
<tr>
    <td>available</td>
    <td><img src="images/badges/badge-available.png" alt="Glyph" /></td>
    <td>`<badge value="available"/>`</td>
</tr>
<tr>
    <td>away</td>
    <td><img src="images/badges/badge-away.png" alt="Glyph" /></td>
    <td>`<badge value="away"/>`</td>
</tr>
<tr>
    <td>busy</td>
    <td><img src="images/badges/badge-busy.png" alt="Glyph" /></td>
    <td>`<badge value="busy"/>`</td>
</tr>
<tr>
    <td>error</td>
    <td><img src="images/badges/badge-error.png" alt="Glyph" /></td>
    <td>`<badge value="error"/>`</td>
</tr>
<tr>
    <td>newMessage</td>
    <td><img src="images/badges/badge-newMessage.png" alt="Glyph" /></td>
    <td>`<badge value="newMessage"/>`</td>
</tr>
<tr>
    <td>paused</td>
    <td><img src="images/badges/badge-paused.png" alt="Glyph" /></td>
    <td>`<badge value="paused"/>`</td>
</tr>
<tr>
    <td>playing</td>
    <td><img src="images/badges/badge-playing.png" alt="Glyph" /></td>
    <td>`<badge value="playing"/>`</td>
</tr>
<tr>
    <td>unavailable</td>
    <td><img src="images/badges/badge-unavailable.png" alt="Glyph" /></td>
    <td>`<badge value="unavailable"/>`</td>
</tr>
</table>

## <a name="create-a-badge"></a>배지 만들기

이 예제에서는 배지 업데이트를 만드는 방법을 보여 줍니다.

### <a name="create-a-numeric-badge"></a>숫자 배지 만들기

````csharp
private void setBadgeNumber(int num)
{

    // Get the blank badge XML payload for a badge number
    XmlDocument badgeXml = 
        BadgeUpdateManager.GetTemplateContent(BadgeTemplateType.BadgeNumber);

    // Set the value of the badge in the XML to our number
    XmlElement badgeElement = badgeXml.SelectSingleNode("/badge") as XmlElement;
    badgeElement.SetAttribute("value", num.ToString());

    // Create the badge notification
    BadgeNotification badge = new BadgeNotification(badgeXml);

    // Create the badge updater for the application
    BadgeUpdater badgeUpdater = 
        BadgeUpdateManager.CreateBadgeUpdaterForApplication();

    // And update the badge
    badgeUpdater.Update(badge);

}
````

### <a name="create-a-glyph-badge"></a>문자 모양 배지 만들기
````csharp
private void updateBadgeGlyph()
{
    string badgeGlyphValue = "alert";

    // Get the blank badge XML payload for a badge glyph
    XmlDocument badgeXml = 
        BadgeUpdateManager.GetTemplateContent(BadgeTemplateType.BadgeGlyph);

    // Set the value of the badge in the XML to our glyph value
    Windows.Data.Xml.Dom.XmlElement badgeElement = 
        badgeXml.SelectSingleNode("/badge") as Windows.Data.Xml.Dom.XmlElement;
    badgeElement.SetAttribute("value", badgeGlyphValue);

    // Create the badge notification
    BadgeNotification badge = new BadgeNotification(badgeXml);

    // Create the badge updater for the application
    BadgeUpdater badgeUpdater = 
        BadgeUpdateManager.CreateBadgeUpdaterForApplication();

    // And update the badge
    badgeUpdater.Update(badge);

}
````

### <a name="clear-a-badge"></a>배지 지우기

````csharp
private void clearBadge()
{
    BadgeUpdateManager.CreateBadgeUpdaterForApplication().Clear();
}
````

## <a name="get-the-sample-code"></a>샘플 코드 다운로드

* [알림 샘플](https://github.com/Microsoft/Windows-universal-samples/blob/master/Samples/Notifications)<br/> 라이브 타일을 만들고, 배지 업데이트를 보내고, 알림 메시지를 표시하는 방법을 보여 줍니다. 

## <a name="related-articles"></a>관련 문서

* [적응형 및 대화형 알림 메시지](adaptive-interactive-toasts.md)
* [타일 만들기](creating-tiles.md)
* [적응형 타일 만들기](create-adaptive-tiles.md)
