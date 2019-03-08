---
title: UWP의 Webpush 및 VAPID를 사용 하 여 대체 푸시 채널
description: 대체 푸시 채널을 사용 하 여 UWP 앱에서 VAPID 프로토콜을 사용 하는 것에 대 한 지침
ms.date: 01/10/2017
ms.topic: article
keywords: windows 10, uwp, WinRT API WNS
localizationpriority: medium
ms.openlocfilehash: ba8630a2e877345adeac7eb443dd3e418d3ed277
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57639528"
---
# <a name="alternate-push-channels-using-webpush-and-vapid-in-uwp"></a>UWP의 Webpush 및 VAPID를 사용 하 여 대체 푸시 채널 
부터는 Fall Creators Update에 UWP 앱 수 사용 하 여 웹 푸시 VAPID 인증을 사용 하 여 푸시 알림을 보내도록 합니다.  

## <a name="introduction"></a>소개
웹 푸시 표준의 도입을 통해 웹 사이트 사용자가 웹 사이트에 없는 경우에 알림을 보낼 앱 처럼 더 작동할 수 있습니다.

웹 사이트에서 공급 업체에서 서버 푸시를 사용 하 여 인증을 허용 하도록 생성 된 VAPID 인증 프로토콜을 알 수 없는 방식으로 합니다. VAPID 프로토콜을 사용 하는 모든 공급 업체를 사용 하 여 웹 사이트 실행 되는 브라우저를 모른 채 푸시 알림을 보낼 수 있습니다. 구현 하는 각 플랫폼에 대해 다른 푸시 프로토콜을 통해 상당히 향상 된 기능입니다. 

UWP 앱 이러한 장점에도 사용 하 여 푸시 알림을 보내도록 webpush 및 VAPID를 사용할 수 있습니다. 이러한 프로토콜 새 앱에 대 한 개발 시간을 절약 하 고 기존 앱에 대 한 플랫폼 간 지원을 간소화할 수 있습니다. 또한 엔터프라이즈 앱 또는 테스트용 로드 앱에 Microsoft Store 등록 하지 않고 알림을 보낼 이제 수 있습니다. 다행히 모든 플랫폼에서 사용자를 사용 하 여 참여 하는 새로운 방법을 엽니다.  

## <a name="alternate-channels"></a>대체 채널 
UWP, 이러한 VAPID 채널 대체 채널 이라고 하 고 웹 푸시 채널에 유사한 기능을 제공 합니다. 메시지 암호화 사용을 실행 하는 앱 백그라운드 작업 트리거를 단일 앱에서 여러 채널을 허용 합니다. 다른 채널 형식 간의 차이점에 대 한 자세한 내용은 참조 하십시오 [올바른 채널 선택](channel-types.md)합니다.

대체 채널을 사용 하는 앱에는 기본 채널을 사용할 수 없는 경우 또는 웹 사이트와 앱 간에 코드를 공유 하려는 경우 푸시 알림에 액세스할 수 있는 좋은 방법입니다. 채널을 설정은 웹 푸시 표준 사용 되거나 Windows 푸시 알림 전에 협력 사람에 게 친숙 하 고 간편 합니다.

## <a name="code-example"></a>코드 예제

UWP 앱에 대 한 대체 채널을 설정 하는 기본 프로세스는 기본 또는 보조 채널을 설정 하는 것과 비슷합니다. 먼저 채널을 등록 합니다 [WNS server](windows-push-notification-services--wns--overview.md)합니다. 그런 다음 백그라운드 작업으로 실행 되도록 등록 합니다. 알림이 전송 된 후 백그라운드 작업이 트리거되는 이벤트를 처리 합니다.  

### <a name="get-a-channel"></a>채널 가져오기 
앱 대체는 채널을 만들려면 두 가지 정보를 제공 해야 합니다: 공개 키가 생성 되 고 채널의 이름과 해당 서버에 대 한 합니다. 서버 키에 대 한 세부 정보 웹 푸시 사양에서 사용할 수 있지만 서버에는 표준 웹 푸시 라이브러리를 사용 하 여 키를 생성 하는 것이 좋습니다.  

채널 ID는 앱 여러 대체 채널을 만들 수 있기 때문에 특히 유용 합니다. 각 채널은 해당 채널을 통해 전송 하는 모든 알림 페이로드를 사용 하 여 포함 될 고유한 ID로 식별 되어야 합니다.  

```csharp
private async void AppCreateVAPIDChannelAsync(string appChannelId, IBuffer applicationServerKey) 
{ 
    // From the spec: applicationServer Key (p256dh):  
    //               An Elliptic curve Diffie–Hellman public key on the P-256 curve 
    //               (that is, the NIST secp256r1 elliptic curve).   
    //               The resulting key is an uncompressed point in ANSI X9.62 format             
    // ChannelId is an app provided value for it to identify the channel later.  
    // case of this app it is from the set { "Football", "News", "Baseball" } 
    PushNotificationChannel webChannel = await PushNotificationChannelManager.Current.CreateRawPushNotificationChannelWithAlternateKeyForApplicationAsync(applicationServerKey, appChannelId); 
 
    //Save the channel  
    AppUpdateChannelMapping(appChannelId, webChannel); 
             
    //Tell our web service that we have a new channel to push notifications to 
    AppPassChannelToSite(webChannel.Uri); 
} 
```
앱 서버 백업 채널 보내고 로컬에 저장 합니다. 채널 ID를 로컬로 저장 채널을 구분 하 고 만료 되지 않도록 하려면 채널을 갱신 하는 앱 수 있습니다.

푸시 알림 채널의 다른 모든 형식과 같은 웹 채널 만료 될 수 있습니다. 채널에 앱 모른 채 만료 되지 않도록 하려면 앱이 시작 될 때마다 새 채널을 만듭니다.    

### <a name="register-for-a-background-task"></a>백그라운드 작업에 대 한 등록 

앱에 대체 된 채널을 만든 후에 포그라운드 또는 백그라운드에서 알림을 수신 하도록 등록 해야 합니다. 아래 예제에서는 백그라운드에서 알림을 수신 하도록 한 프로세스 모델을 사용 하도록 등록 합니다.  

```csharp
var builder = new BackgroundTaskBuilder(); 
builder.Name = "Push Trigger"; 
builder.SetTrigger(new PushNotificationTrigger()); 
BackgroundTaskRegistration task = builder.Register(); 
```
### <a name="receive-the-notifications"></a>알림을 수신 합니다. 

앱에 알림을 수신 하도록 등록 되 면 들어오는 알림을 처리 하는 일을 할 수 해야 합니다. 단일 앱을 여러 채널을 등록할 수 있습니다, 되므로 알림을 처리 하기 전에 채널 ID를 확인 해야 합니다.  

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

Note는 알림을 기본 채널에서 들어오는 경우 다음 채널 ID 설정 되지 않습니다.  

## <a name="note-on-encryption"></a>암호화 note 

모든 암호화 체계 보다 유용한 앱을 사용할 수 있습니다. 일부 경우에서는 서버와 모든 Windows 장치 간의 TLS 표준을 사용 하기에 충분 합니다. 다른 경우에 웹 푸시 암호화 체계 또는 디자인의 다른 체계를 사용 하는 것을 해야 합니다.  

다른 형태의 암호화를 사용 하려는 경우 키는 원시 사용 합니다. 헤더 속성입니다. 모든 푸시 서버로 POST 요청에 포함 된 암호화 헤더 포함 합니다. 여기에서 앱 메시지를 해독 하려면 키를 사용할 수 있습니다.  

## <a name="related-topics"></a>관련 항목
- [알림 채널 형식입니다.](channel-types.md)
- [Windows 푸시 알림 서비스 (WNS)](windows-push-notification-services--wns--overview.md)
- [PushNotificationChannel 클래스](https://docs.microsoft.com/uwp/api/windows.networking.pushnotifications.pushnotificationchannel)
- [PushNotificationChannelManager 클래스](https://docs.microsoft.com/uwp/api/windows.networking.pushnotifications.pushnotificationchannelmanager)


