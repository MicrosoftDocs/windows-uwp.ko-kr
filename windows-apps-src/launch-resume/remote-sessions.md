---
title: 원격 세션을 통해 디바이스 연결
description: 원격 세션에서 여러 디바이스를 연결하여 공유 환경을 만듭니다.
ms.assetid: 1c8dba9f-c933-4e85-829e-13ad784dd3e2
ms.date: 06/28/2017
ms.topic: article
keywords: windows 10, uwp, 연결 된 장치, 원격 시스템, 로마, 프로젝트 로마
ms.localizationpriority: medium
ms.openlocfilehash: f88f44d26c3a6f4971422074e855ffca53935c7f
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89164827"
---
# <a name="connect-devices-through-remote-sessions"></a>원격 세션을 통해 디바이스 연결

원격 세션 기능을 사용 하면 앱이 명시적인 앱 메시징 또는 Windows Holographic 장치 간 **[SpatialEntityStore](/uwp/api/windows.perception.spatial.spatialentitystore)** for holographic 공유와 같은 시스템 관리 데이터의 조정 된 교환에 대해 세션을 통해 다른 장치에 연결할 수 있습니다.

모든 Windows 장치에서 원격 세션을 만들 수 있으며, 다른 사용자가 로그인 한 장치를 비롯 한 모든 Windows 장치에서 참가를 요청할 수 있습니다 (세션은 초대 전용 표시 가능). 이 가이드에서는 원격 세션을 사용 하는 모든 주요 시나리오에 대 한 기본 샘플 코드를 제공 합니다. 이 코드는 기존 앱 프로젝트에 통합 하 고 필요에 따라 수정할 수 있습니다. 종단 간 구현에 대해서는 [퀴즈 게임 샘플 앱](https://github.com/microsoft/Windows-appsample-remote-system-sessions)을 참조 하세요.

## <a name="preliminary-setup"></a>예비 설정

### <a name="add-the-remotesystem-capability"></a>RemoteSystem 기능 추가

앱이 원격 장치에서 앱을 시작 하도록 하려면 `remoteSystem` 앱 패키지 매니페스트에 기능을 추가 해야 합니다. 패키지 매니페스트 디자이너를 사용 하 여 **기능** 탭에서 **원격 시스템** 을 선택 하 여 추가 하거나 프로젝트의 _appxmanifest.xml_ 파일에 다음 줄을 수동으로 추가할 수 있습니다.

``` xml
<Capabilities>
   <uap3:Capability Name="remoteSystem"/>
</Capabilities>
```

### <a name="enable-cross-user-discovering-on-the-device"></a>장치에서 교차 사용자 검색 사용
원격 세션은 서로 다른 여러 사용자를 연결 하는 데 사용 되므로 관련 된 장치에서 사용자 간 공유를 사용 하도록 설정 해야 합니다. 다음은 **RemoteSystem** 클래스에서 정적 메서드를 사용 하 여 쿼리할 수 있는 시스템 설정입니다.

```csharp
if (!RemoteSystem.IsAuthorizationKindEnabled(RemoteSystemAuthorizationKind.Anonymous)) {
    // The system is not authorized to connect to cross-user devices. 
    // Inform the user that they can discover more devices if they
    // update the setting to "Everyone nearby".
}
```

이 설정을 변경 하려면 사용자가 **설정** 앱을 열어야 합니다. 장치에서 **시스템**  >  **공유 환경**  >  **공유** 메뉴에는 사용자가 시스템을 공유할 수 있는 장치를 지정할 수 있는 드롭다운 상자가 있습니다.

![공유 환경 설정 페이지](images/shared-experiences-settings.png)

### <a name="include-the-necessary-namespaces"></a>필요한 네임 스페이스 포함
이 가이드의 모든 코드 조각을 사용 하려면 `using` 클래스 파일에 다음 문이 필요 합니다.

```csharp
using System.Runtime.Serialization.Json;
using Windows.Foundation.Collections;
using Windows.System.RemoteSystems;
```

## <a name="create-a-remote-session"></a>원격 세션 만들기

원격 세션 인스턴스를 만들려면 **[RemoteSystemSessionController](/uwp/api/windows.system.remotesystems.remotesystemsessioncontroller)** 개체로 시작 해야 합니다. 다음 프레임 워크를 사용 하 여 새 세션을 만들고 다른 장치의 조인 요청을 처리할 수 있습니다.

```csharp
public async void CreateSession() {
    
    // create a session controller
    RemoteSystemSessionController manager = new RemoteSystemSessionController("Bob’s Minecraft game");
    
    // register the following code to handle the JoinRequested event
    manager.JoinRequested += async (sender, args) => {
        // Get the deferral
        var deferral = args.GetDeferral();
        
        // display the participant (args.JoinRequest.Participant) on UI, giving the 
        // user an opportunity to respond
        // ...
        
        // If the user chooses "accept", accept this remote system as a participant
        args.JoinRequest.Accept();
    };
    
    // create and start the session
    RemoteSystemSessionCreationResult createResult = await manager.CreateSessionAsync();
    
    // handle the creation result
    if (createResult.Status == RemoteSystemSessionCreationStatus.Success) {
        // creation was successful, get a reference to the session
        RemoteSystemSession currentSession = createResult.Session;
        
        // optionally subscribe to the disconnection event
        currentSession.Disconnected += async (sender, args) => {
            // update the UI, using args.Reason
            //...
        };
    
        // Use session (see later section)
        //...
    
    } else if (createResult.Status == RemoteSystemSessionCreationStatus.SessionLimitsExceeded) {
        // creation failed. Optionally update UI to indicate that there are too many sessions in progress
    } else {
        // creation failed for an unknown reason. Optionally update UI
    }
}
```

### <a name="make-a-remote-session-invite-only"></a>원격 세션을 초대 전용으로 설정

원격 세션을 공개적으로 검색 가능 하 게 유지 하려는 경우 초대 전용으로 설정할 수 있습니다. 초대를 수신 하는 장치만 참가 요청을 보낼 수 있습니다. 

프로시저는 대부분 위와 동일 하지만 **[RemoteSystemSessionController](/uwp/api/windows.system.remotesystems.remotesystemsessioncontroller)** 인스턴스를 생성할 때 구성 된 **[RemoteSystemSessionOptions](/uwp/api/windows.system.remotesystems.RemoteSystemSessionOptions)** 개체를 전달 합니다.

```csharp
// define the session options with the invite-only designation
RemoteSystemSessionOptions sessionOptions = new RemoteSystemSessionOptions();
sessionOptions.IsInviteOnly = true;

// create the session controller
RemoteSystemSessionController manager = new RemoteSystemSessionController("Bob's Minecraft game", sessionOptions);

//...
```

초대를 보내려면 정상적인 원격 시스템 검색을 통해 얻은 수신 원격 시스템에 대 한 참조가 있어야 합니다. 이 참조를 session 개체의 **[SendInvitationAsync](/uwp/api/windows.system.remotesystems.remotesystemsession.sendinvitationasync)** 메서드에 전달 하기만 하면 됩니다. 세션의 모든 참가자에 게는 원격 세션에 대 한 참조가 있습니다 (다음 섹션 참조). 따라서 모든 참가자가 초대를 보낼 수 있습니다.

```csharp
// "currentSession" is a reference to a RemoteSystemSession.
// "guestSystem" is a previously discovered RemoteSystem instance
currentSession.SendInvitationAsync(guestSystem); 
```

## <a name="discover-and-join-a-remote-session"></a>원격 세션 검색 및 조인

원격 세션을 검색 하는 프로세스는 **[RemoteSystemSessionWatcher](/uwp/api/windows.system.remotesystems.remotesystemsessionwatcher)** 클래스에 의해 처리 되며 개별 원격 시스템을 검색 하는 것과 비슷합니다.

```csharp
public void DiscoverSessions() {
    
    // create a watcher for remote system sessions
    RemoteSystemSessionWatcher sessionWatcher = RemoteSystemSession.CreateWatcher();
    
    // register a handler for the "added" event
    sessionWatcher.Added += async (sender, args) => {
        
        // get a reference to the info about the discovered session
        RemoteSystemSessionInfo sessionInfo = args.SessionInfo;
        
        // Optionally update the UI with the sessionInfo.DisplayName and 
        // sessionInfo.ControllerDisplayName strings. 
        // Save a reference to this RemoteSystemSessionInfo to use when the
        // user selects this session from the UI
        //...
    };
    
    // Begin watching
    sessionWatcher.Start();
}
```

**[RemoteSystemSessionInfo](/uwp/api/windows.system.remotesystems.remotesystemsessioninfo)** 인스턴스를 가져오는 경우 해당 세션을 제어 하는 장치에 대 한 조인 요청을 실행 하는 데 사용할 수 있습니다. 수락 된 조인 요청은 조인 된 세션에 대 한 참조를 포함 하는 **[RemoteSystemSessionJoinResult](/uwp/api/windows.system.remotesystems.remotesystemsessionjoinresult)** 개체를 비동기적으로 반환 합니다.

```csharp
public async void JoinSession(RemoteSystemSessionInfo sessionInfo) {

    // issue a join request and wait for result.
    RemoteSystemSessionJoinResult joinResult = await sessionInfo.JoinAsync();
    if (joinResult.Status == RemoteSystemSessionJoinStatus.Success) {
        // Join request was approved

        // RemoteSystemSession instance "currentSession" was declared at class level.
        // Assign the value obtained from the join result.
        currentSession = joinResult.Session;
        
        // note connection and register to handle disconnection event
        bool isConnected = true;
        currentSession.Disconnected += async (sender, args) => {
            isConnected = false;

            // update the UI with args.Reason value
        };
        
        if (isConnected) {
            // optionally use the session here (see next section)
            //...
        }
    } else {
        // Join was unsuccessful.
        // Update the UI, using joinResult.Status value to show cause of failure.
    }
}
```

여러 세션에 동시에 장치를 조인할 수 있습니다. 이러한 이유로 조인 기능을 각 세션과 실제 상호 작용에서 분리 하는 것이 좋을 수 있습니다. **[RemoteSystemSession](/uwp/api/windows.system.remotesystems.remotesystemsession)** 인스턴스에 대 한 참조가 앱에서 유지 관리 되는 동안 해당 세션을 통해 통신을 시도할 수 있습니다.

## <a name="share-messages-and-data-through-a-remote-session"></a>원격 세션을 통해 메시지 및 데이터 공유

### <a name="receive-messages"></a>메시지 받기

단일 세션 전체 통신 채널을 나타내는 **[RemoteSystemSessionMessageChannel](/uwp/api/windows.system.remotesystems.remotesystemsessionmessagechannel)** 인스턴스를 사용 하 여 세션의 다른 참가자 장치와 메시지 및 데이터를 교환할 수 있습니다. 초기화 되는 즉시 들어오는 메시지의 수신 대기를 시작 합니다.

>[!NOTE]
>메시지는 보내고 받을 때 바이트 배열에서 serialize 및 deserialize 되어야 합니다. 이 기능은 다음 예제에 포함 되어 있지만 더 나은 코드 모듈화를 위해 별도로 구현할 수 있습니다. 이에 대 한 예제는 [샘플 앱](https://github.com/microsoft/Windows-appsample-remote-system-sessions)을 참조 하세요.

```csharp
public async void StartReceivingMessages() {
    
    // Initialize. The channel name must be known by all participant devices 
    // that will communicate over it.
    RemoteSystemSessionMessageChannel messageChannel = new RemoteSystemSessionMessageChannel(currentSession, 
        "Everyone in Bob's Minecraft game", 
        RemoteSystemSessionMessageChannelReliability.Reliable);
    
    // write the handler for incoming messages on this channel
    messageChannel.ValueSetReceived += async (sender, args) => {
        
        // Update UI: a message was received from the participant args.Sender
        
        // Deserialize the message 
        // (this app must know what key to use and what object type the value is expected to be)
        ValueSet receivedMessage = args.Message;
        object rawData = receivedMessage["appKey"]);
        object value = new ExpectedType(); // this must be whatever type is expected

        using (var stream = new MemoryStream((byte[])rawData)) {
            value = new DataContractJsonSerializer(value.GetType()).ReadObject(stream);
        }
        
        // do something with the "value" object
        //...
    };
}
```

### <a name="send-messages"></a>메시지 보내기

채널이 설정 되 면 모든 세션 참가자에 게 메시지를 보내는 것은 간단 합니다.

```csharp
public async void SendMessageToAllParticipantsAsync(RemoteSystemSessionMessageChannel messageChannel, object value){

    // define a ValueSet message to send
    ValueSet message = new ValueSet();
    
    // serialize the "value" object to send
    using (var stream = new MemoryStream()){
        new DataContractJsonSerializer(value.GetType()).WriteObject(stream, value);
        byte[] rawData = stream.ToArray();
            message["appKey"] = rawData;
    }
    
    // Send message to all participants. Ordering is not guaranteed.
    await messageChannel.BroadcastValueSetAsync(message);
}
```

특정 참가자로만 메시지를 보내려면 먼저 검색 프로세스를 시작 하 여 세션에 참여 하는 원격 시스템에 대 한 참조를 가져와야 합니다. 이는 세션 외부에서 원격 시스템을 검색 하는 프로세스와 유사 합니다. **[RemoteSystemSessionParticipantWatcher](/uwp/api/windows.system.remotesystems.remotesystemsessionparticipantwatcher)** 인스턴스를 사용 하 여 세션의 참가자 장치를 찾을 수 있습니다.

```csharp
public void WatchForParticipants() {
    // "currentSession" is a reference to a RemoteSystemSession.
    RemoteSystemSessionParticipantWatcher watcher = currentSession.CreateParticipantWatcher();

    watcher.Added += (sender, participant) => {
        // save a reference to "participant"
        // optionally update UI
    };   

    watcher.Removed += (sender, participant) => {
        // remove reference to "participant"
        // optionally update UI
    };

    watcher.EnumerationCompleted += (sender, args) => {
        // Apps can delay data model render up until this point if they wish.
    };

    // Begin watching for session participants
    watcher.Start();
}
```

세션 참가자에 대 한 참조 목록을 가져오는 경우 모든 집합에 메시지를 보낼 수 있습니다.

단일 참가자에 게 메시지를 보내려면 (가장 적합 한 사용자가 화면에서 선택) 참조를 다음과 같은 메서드에 전달 하면 됩니다.

```csharp
public async void SendMessageToParticipantAsync(RemoteSystemSessionMessageChannel messageChannel, RemoteSystemSessionParticipant participant, object value) {
    
    // define a ValueSet message to send
    ValueSet message = new ValueSet();
    
    // serialize the "value" object to send
    using (var stream = new MemoryStream()){
        new DataContractJsonSerializer(value.GetType()).WriteObject(stream, value);
        byte[] rawData = stream.ToArray();
            message["appKey"] = rawData;
    }

    // Send message to the participant
    await messageChannel.SendValueSetAsync(message,participant);
}
```

여러 참가자에 게 메시지를 보내려면 (가장 적합 한 사용자가 화면에서 선택) 목록 개체에 추가 하 고 다음과 같은 메서드로 목록을 전달 합니다.

```csharp
public async void SendMessageToListAsync(RemoteSystemSessionMessageChannel messageChannel, IReadOnlyList<RemoteSystemSessionParticipant> myTeam, object value){

    // define a ValueSet message to send
    ValueSet message = new ValueSet();
    
    // serialize the "value" object to send
    using (var stream = new MemoryStream()){
        new DataContractJsonSerializer(value.GetType()).WriteObject(stream, value);
        byte[] rawData = stream.ToArray();
            message["appKey"] = rawData;
    }

    // Send message to specific participants. Ordering is not guaranteed.
    await messageChannel.SendValueSetToParticipantsAsync(message, myTeam);   
}
```

## <a name="related-topics"></a>관련 항목
* [연결된 앱 및 디바이스(프로젝트 로마)](connected-apps-and-devices.md)
* [원격 시스템 API 참조](/uwp/api/Windows.System.RemoteSystems)