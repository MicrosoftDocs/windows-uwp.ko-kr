---
author: PatrickFarley
title: 원격 세션을 통해 디바이스 연결
description: 원격 세션에서 여러 장치를 연결하여 공유되는 환경을 만듭니다.
ms.assetid: 1c8dba9f-c933-4e85-829e-13ad784dd3e2
ms.author: pafarley
ms.date: 06/28/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp, 연결 장치, 원격 시스템, 로마, 로마 프로젝트
ms.localizationpriority: medium
ms.openlocfilehash: 8e5226b23a454bf48add22d590a3ff247c629e4f
ms.sourcegitcommit: 7aa1933e6970f878faf50d59e1f799b90afd7cc7
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 09/05/2018
ms.locfileid: "3373665"
---
# <a name="connect-devices-through-remote-sessions"></a>원격 세션을 통해 디바이스 연결

원격 세션 기능을 사용하면 앱은 명시적 앱 메시지 또는 Windows Holographic 디바이스 간의 홀로그램 공유를 위한 **[SpatialEntityStore](https://docs.microsoft.com/uwp/api/windows.perception.spatial.spatialentitystore)** 등 시스템 관리 데이터의 조정된 교환을 위해 세션을 통해 다른 디바이스에 연결할 수 있습니다.

원격 세션은 모든 Windows 장치에서 만들 수 있으며, 모든 Windows 장치는 다른 사용자가 로그인한 디바이스를 포함하여 연결을 요청할 수 있습니다(세션은 초대만 볼 수 있음). 이 가이드는 원격 세션을 사용하는 모든 주요 시나리오에 대한 기본 샘플 코드를 제공합니다. 이 코드는 기존 앱 프로젝트에 통합되고 필요에 따라 수정할 수 있습니다. 종단 간 구현은 [퀴즈 게임 샘플 앱](https://github.com/microsoft/Windows-appsample-remote-system-sessions)을 참조하세요.

## <a name="preliminary-setup"></a>사전 설정

### <a name="add-the-remotesystem-capability"></a>remoteSystem 접근 권한 값 추가

앱이 원격 디바이스에서 다른 앱을 실행하려면 앱 패키지 매니페스트에 `remoteSystem` 접근 권한 값을 추가해야 합니다. 패키지 매니페스트 디자이너의 **접근 권한 값** 탭에서 **원격 시스템**을 선택하여 접근 권한 값을 추가하거나 프로젝트의 _Package.appxmanifest_ 파일에 다음 줄을 수동으로 추가할 수 있습니다.

``` xml
<Capabilities>
   <uap3:Capability Name="remoteSystem"/>
</Capabilities>
```

### <a name="enable-cross-user-discovering-on-the-device"></a>디바이스에서 사용자 간 검색 활성화
원격 세션은 여러 다른 사용자를 연결하기 위한 것이므로 관련된 디바이스에는 사용자 간 공유가 활성화되어 있어야 합니다. 이는 **RemoteSystem** 클래스의 정적 메서드로 쿼리할 수 있는 시스템 설정입니다.

```csharp
if (!RemoteSystem.IsAuthorizationKindEnabled(RemoteSystemAuthorizationKind.Anonymous)) {
    // The system is not authorized to connect to cross-user devices. 
    // Inform the user that they can discover more devices if they
    // update the setting to "Everyone nearby".
}
```

이 설정을 변경하려면 사용자가 **설정** 앱을 열어야 합니다. **시스템** > **공유 환경** > **장치 간 공유** 메뉴에 사용자가 시스템에서 공유할 수 있는 장치를 지정하는 드롭다운 상자가 있습니다.

![공유 환경 설정 페이지](images/shared-experiences-settings.png)

### <a name="include-the-necessary-namespaces"></a>필요한 네임스페이스 포함
이 가이드의 모든 코드 조각을 사용하려면 클래스 파일에 다음과 같은 `using` 문이 필요합니다.

```csharp
using System.Runtime.Serialization.Json;
using Windows.Foundation.Collections;
using Windows.System.RemoteSystems;
```

## <a name="create-a-remote-session"></a>원격 세션 만들기

원격 세션 인스턴스를 만들려면 **[RemoteSystemSessionController](https://docs.microsoft.com/uwp/api/windows.system.remotesystems.remotesystemsessioncontroller)** 개체로 시작해야 합니다. 다음 프레임워크를 사용하여 새 세션을 만들고 다른 디바이스의 연결 요청을 처리합니다.

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

### <a name="make-a-remote-session-invite-only"></a>초대 전용 원격 세션 만들기

원격 세션을 공개적으로 검색할 수 없게 하려면 초대 전용으로 만들 수 있습니다. 초대를 받는 디바이스만 연결 요청을 보낼 수 있습니다. 

절차는 위와 거의 동일하지만 **[RemoteSystemSessionController](https://docs.microsoft.com/uwp/api/windows.system.remotesystems.remotesystemsessioncontroller)** 인스턴스를 생성할 때, 구성된 **[RemoteSystemSessionOptions](https://docs.microsoft.com/uwp/api/windows.system.remotesystems.RemoteSystemSessionOptions)** 개체를 전달하게 됩니다.

```csharp
// define the session options with the invite-only designation
RemoteSystemSessionOptions sessionOptions = new RemoteSystemSessionOptions();
sessionOptions.IsInviteOnly = true;

// create the session controller
RemoteSystemSessionController manager = new RemoteSystemSessionController("Bob's Minecraft game", sessionOptions);

//...
```

초대를 보내려면 수신하는 원격 시스템에 대한 참조가 있어야 합니다(정상 원격 시스템 검색을 통해 획득). 이 참조를 세션 개체의 **[SendInvitationAsync](https://docs.microsoft.com/uwp/api/windows.system.remotesystems.remotesystemsession.sendinvitationasync)** 메서드에 전달하면 됩니다. 세션의 모든 참가자는 원격 세션에 대한 참조를 가지므로(다음 섹션 참조) 모든 참가자가 초대를 보낼 수 있습니다.

```csharp
// "currentSession" is a reference to a RemoteSystemSession.
// "guestSystem" is a previously discovered RemoteSystem instance
currentSession.SendInvitationAsync(guestSystem); 
```

## <a name="discover-and-join-a-remote-session"></a>원격 세션 검색 및 참가

원격 세션을 검색하는 프로세스는 **[RemoteSystemSessionWatcher](https://docs.microsoft.com/uwp/api/windows.system.remotesystems.remotesystemsessionwatcher)** 클래스에 의해 처리되며 개별 원격 시스템을 검색하는 것과 유사합니다.

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

**[RemoteSystemSessionInfo](https://docs.microsoft.com/uwp/api/windows.system.remotesystems.remotesystemsessioninfo)** 인스턴스를 얻으면 해당 세션을 제어하는 디바이스에 연결 요청을 하는 데 사용할 수 있습니다. 수락된 연결 요청은 연결된 세션에 대한 참조를 포함하는 **[RemoteSystemSessionJoinResult](https://docs.microsoft.com/uwp/api/windows.system.remotesystems.remotesystemsessionjoinresult)** 개체를 비동기식으로 반환합니다.

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

디바이스를 동시에 여러 세션에 연결할 수 있습니다. 이러한 이유로 각 세션과의 실제 상호 작용에서 연결 기능을 분리하는 것이 좋을 수 있습니다. **[RemoteSystemSession](https://docs.microsoft.com/uwp/api/windows.system.remotesystems.remotesystemsession)** 인스턴스에 대한 참조가 앱에 유지되는 한 해당 세션을 통해 통신을 시도할 수 있습니다.

## <a name="share-messages-and-data-through-a-remote-session"></a>원격 세션을 통해 메시지 및 데이터 공유

### <a name="receive-messages"></a>메시지 수신

단일 세션 전체 통신 채널을 나타내는 **[RemoteSystemSessionMessageChannel](https://docs.microsoft.com/uwp/api/windows.system.remotesystems.remotesystemsessionmessagechannel)** 인스턴스를 사용하여 세션의 다른 참가 디바이스와 메시지 및 데이터를 교환할 수 있습니다. 초기화가 완료되면 들어오는 메시지 수신을 시작합니다.

>[!NOTE]
>메시지는 전송 및 수신 시 바이트 배열에서 직렬화 및 역직렬화되어야 합니다. 이 기능은 다음 예제에 포함되어 있지만 더 나은 코드 모듈성을 위해 별도로 구현할 수 있습니다. 이러한 예제는 [샘플 앱](https://github.com/microsoft/Windows-appsample-remote-system-sessions)을 참조하세요.

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

### <a name="send-messages"></a>메시지 전송

채널이 설정되면 모든 세션 참가자에게 메시지를 보내는 것이 간단해집니다.

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

특정 참가자에게만 메시지를 보내려면 먼저 세션에 참여하는 원격 시스템에 대한 참조를 얻기 위해 검색 프로세스를 시작해야 합니다. 이는 세션 외부의 원격 시스템을 검색하는 프로세스와 유사합니다. **[RemoteSystemSessionParticipantWatcher](https://docs.microsoft.com/uwp/api/windows.system.remotesystems.remotesystemsessionparticipantwatcher)** 인스턴스를 사용하여 세션의 참가 디바이스를 찾습니다.

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

세션 참가자에 대한 참조 목록을 얻으면 메시지 집합으로 메시지를 보낼 수 있습니다.

단일 참가자에게 메시지를 보내려면(사용자가 화면상 가장 적합하게 선택) 다음과 같은 방법으로 참조를 전달합니다.

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

여러 참가자에게 메시지를 보내려면(사용자가 화면상 가장 적합하게 선택) 목록 개체에 추가하고 다음과 같은 방법으로 목록을 전달합니다.

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
* [연결된 앱 및 장치(프로젝트 로마)](connected-apps-and-devices.md)
* [원격 시스템 API 참조](https://msdn.microsoft.com/library/windows/apps/Windows.System.RemoteSystems)
