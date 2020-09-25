---
title: UWP에서 VAPID를 사용하는 대체 푸시 채널
description: Windows 앱에서 VAPID 프로토콜을 사용 하 여 대체 푸시 채널을 사용 하기 위한 지침
ms.date: 09/24/2020
ms.topic: article
keywords: windows 10, uwp, WinRT API, WNS
localizationpriority: medium
ms.openlocfilehash: 79ea88cb457e9a0d7ba33ef51a184e6f52ab19c4
ms.sourcegitcommit: eda7bbe9caa9d61126e11f0f1a98b12183df794d
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 09/24/2020
ms.locfileid: "91219276"
---
# <a name="alternate-push-channels-using-vapid-in-windows"></a>Windows에서 VAPID를 사용 하는 대체 푸시 채널 
Windows 앱은 VAPID 인증을 사용 하 여 푸시 알림을 보낼 수 있습니다.  

> [!NOTE]
> 이러한 Api는 다른 웹 사이트를 호스팅하고 대신 채널을 만드는 웹 브라우저용입니다.  웹 앱에 webpush 알림을 추가 하려는 경우 서비스 작업자를 만들고 알림을 보내는 데 W3C 및 WhatWG 표준을 따르는 것이 좋습니다.

## <a name="introduction"></a>소개
웹 푸시 표준의 도입으로 웹 사이트는 사용자가 웹 사이트에 있지 않은 경우에도 앱을 더 유사 하 게 작동 하 고 알림을 보낼 수 있습니다.

VAPID 인증 프로토콜은 웹 사이트에서 공급 업체의 독립적인 방식으로 푸시 서버를 인증할 수 있도록 하기 위해 만들어졌습니다. VAPID 프로토콜을 사용 하는 모든 공급 업체에서 웹 사이트는 실행 중인 브라우저를 알지 못해도 푸시 알림을 보낼 수 있습니다. 이는 각 플랫폼에 대해 다른 푸시 프로토콜을 구현 하는 것에 비해 크게 향상 된 기능입니다. 

Windows 앱은 VAPID를 사용 하 여 이러한 장점과 함께 푸시 알림을 보낼 수 있습니다. 이러한 프로토콜을 통해 새 앱에 대 한 개발 시간을 절약 하 고 기존 앱에 대 한 플랫폼 간 지원을 간소화할 수 있습니다. 또한 enterprise apps 또는 테스트용으로 로드 apps는 Microsoft Store에 등록 하지 않고 알림을 보낼 수 있습니다. 이렇게 하면 모든 플랫폼에서 사용자를 사용 하는 새로운 방법이 열립니다.  

## <a name="alternate-channels"></a>대체 채널 
UWP에서 이러한 VAPID 채널을 대체 채널 이라고 하며 웹 푸시 채널과 유사한 기능을 제공 합니다. 앱 백그라운드 작업을 실행 하 고, 메시지 암호화를 사용 하도록 설정 하 고, 단일 앱에서 여러 채널을 허용 하는 작업을 트리거할 수 있습니다. 서로 다른 채널 형식 간의 차이점에 대 한 자세한 내용은 [오른쪽 채널 선택](channel-types.md)을 참조 하세요.

앱이 기본 채널을 사용할 수 없는 경우 또는 웹 사이트와 앱 간에 코드를 공유 하려는 경우에는 대체 채널을 사용 하 여 푸시 알림에 액세스할 수 있습니다. 채널을 설정 하는 것은 웹 푸시 표준을 사용 하거나 이전에 Windows 푸시 알림과 함께 작업 하는 모든 사용자에 게 친숙 합니다.

## <a name="code-example"></a>코드 예제

Windows 앱에 대 한 대체 채널을 설정 하는 기본 프로세스는 기본 채널 또는 보조 채널을 설정 하는 것과 비슷합니다. 먼저 [WNS 서버](windows-push-notification-services--wns--overview.md)에 채널을 등록 합니다. 그런 다음를 등록 하 여 백그라운드 작업으로 실행 합니다. 알림을 보내고 백그라운드 태스크가 트리거된 후 이벤트를 처리 합니다.  

### <a name="get-a-channel"></a>채널 가져오기 
대체 채널을 만들려면 앱에서 서버에 대 한 공개 키와 자신이 만드는 채널의 이름 등 두 가지 정보를 제공 해야 합니다. 서버 키에 대 한 세부 정보는 웹 푸시 사양에서 사용할 수 있지만 서버에서 표준 웹 푸시 라이브러리를 사용 하 여 키를 생성 하는 것이 좋습니다.  

앱에서 여러 대체 채널을 만들 수 있기 때문에 채널 ID는 특히 중요 합니다. 각 채널은 해당 채널을 따라 전송 되는 알림 페이로드에 포함 될 고유 ID로 식별 되어야 합니다.  

```csharp
private async void AppCreateVAPIDChannelAsync(string appChannelId, IBuffer applicationServerKey) 
{ 
    // From the spec: applicationServer Key (p256dh):  
    //               An Elliptic curve Diffie–Hellman public key on the P-256 curve 
    //               (that is, the NIST secp256r1 elliptic curve).   
    //               The resulting key is an uncompressed point in ANSI X9.62 format             
    // ChannelId is an app provided value for it to identify the channel later.  
    // case of this app it is from the set { "Football", "News", "Baseball" } 
    PushNotificationChannel webChannel = await PushNotificationChannelManager.GetDefault().CreateRawPushNotificationChannelWithAlternateKeyForApplicationAsync(applicationServerKey, appChannelId); 
 
    //Save the channel  
    AppUpdateChannelMapping(appChannelId, webChannel); 
             
    //Tell our web service that we have a new channel to push notifications to 
    AppPassChannelToSite(webChannel.Uri); 
} 
```
앱은 채널을 서버에 다시 보내고 로컬로 저장 합니다. 채널 ID를 로컬로 저장 하면 앱에서 채널과 채널을 갱신 하 여 만료 되지 않도록 할 수 있습니다.

다른 모든 형식의 푸시 알림 채널과 마찬가지로 웹 채널은 만료 될 수 있습니다. 앱을 알지 못해도 채널이 만료 되지 않도록 하려면 앱이 시작 될 때마다 새 채널을 만듭니다.    

### <a name="register-for-a-background-task"></a>백그라운드 작업 등록 

앱이 대체 채널을 만든 후에는 포그라운드 또는 백그라운드에서 알림을 받도록 등록 해야 합니다. 아래 예제에서는 단일 프로세스 모델을 사용 하 여 백그라운드에서 알림을 받도록 등록 합니다.  

```csharp
var builder = new BackgroundTaskBuilder(); 
builder.Name = "Push Trigger"; 
builder.SetTrigger(new PushNotificationTrigger()); 
BackgroundTaskRegistration task = builder.Register(); 
```
### <a name="receive-the-notifications"></a>알림 받기 

앱에서 알림을 받도록 등록 한 후에는 들어오는 알림을 처리할 수 있어야 합니다. 단일 앱에서 여러 채널을 등록할 수 있으므로 알림을 처리 하기 전에 채널 ID를 확인 해야 합니다.  

```csharp
protected override void OnBackgroundActivated(BackgroundActivatedEventArgs args) 
{ 
    base.OnBackgroundActivated(args); 
    var raw = args.TaskInstance.TriggerDetails as RawNotification; 
    switch (raw.ChannelId) 
    { 
        case "Football": 
            AppPostFootballScore(raw.Content); 
            break; 
        case "News": 
            AppProcessNewsItem(raw.Content); 
            break; 
        case "Baseball": 
            AppProcessBaseball(raw.Content); 
            break; 
 
        default: 
            //We don't have the channelID registered, should only happen in the case of a 
            //caching issue on the application server 
            break; 
    }                           
} 
```

기본 채널에서 알림을 보내는 경우 채널 ID는 설정 되지 않습니다.  

## <a name="note-on-encryption"></a>암호화에 대 한 참고 사항 

앱에 더 유용 하 게 사용할 수 있는 모든 암호화 체계를 사용할 수 있습니다. 경우에 따라 서버와 Windows 장치 간의 TLS 표준에 의존 하는 것으로 충분 합니다. 다른 경우에는 웹 푸시 암호화 스키마 또는 다른 디자인 체계를 사용 하는 것이 더 적합할 수 있습니다.  

다른 형태의 암호화를 사용 하려는 경우 키가 raw를 사용 하는 것입니다. Headers 속성입니다. 여기에는 push server에 대 한 POST 요청에 포함 된 모든 암호화 헤더가 포함 됩니다. 여기에서 앱은 키를 사용 하 여 메시지를 해독할 수 있습니다.  

## <a name="related-topics"></a>관련 항목
- [알림 채널 유형](channel-types.md)
- [Windows Push Notification Services(WNS)](windows-push-notification-services--wns--overview.md)
- [PushNotificationChannel 클래스](/uwp/api/windows.networking.pushnotifications.pushnotificationchannel)
- [PushNotificationChannelManager 클래스](/uwp/api/windows.networking.pushnotifications.pushnotificationchannelmanager)
