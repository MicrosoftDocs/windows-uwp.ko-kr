---
title: 게임 채팅 2 WinRT 프로젝션을 사용 하 여
description: WinRT 프로젝션과 함께 Xbox Live 게임 채팅 2를 사용 하 여 음성 통신 게임에 추가 하는 방법에 알아봅니다.
ms.date: 04/11/2018
ms.topic: article
keywords: xbox live, xbox, 게임, uwp, windows 10, 하나는 xbox, 2 게임 채팅, 게임 채팅, 음성 통신
ms.localizationpriority: medium
ms.openlocfilehash: 1cb0578151d4262d61f5fbc078bebab721fb3bfe
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57639168"
---
# <a name="using-game-chat-2-winrt-projections"></a>게임 채팅 2 (WinRT 프로젝션)를 사용 하 여

게임 채팅 2의 사용에 대 한 간략 한 연습 C# API. C + +를 통해 게임 채팅 2에 액세스 하려는 게임 개발자에 게 표시 됩니다 [게임 채팅 2를 사용 하 여](using-game-chat-2.md)입니다.

## <a name="prerequisites-a-nameprereq"></a>필수 구성 요소 <a name="prereq">

게임 채팅 2를 사용 하려면 포함 해야 합니다 [Microsoft.Xbox.Services.GameChat2 nuget 패키지](https://www.nuget.org/packages/Microsoft.Xbox.Game.Chat.2.WinRT.UWP/)합니다.

게임 채팅 2를 사용 하 여 코딩을 시작 하기 전에 "마이크" 장치 기능을 선언 하려면 앱의 AppXManifest 구성 했어야 합니다. AppXManifest 기능 플랫폼 문서의 각 섹션에 자세히 설명 되어 있습니다. 채팅을 차단 합니다. 그렇지 않으면 다음 코드 조각은 패키지/기능 노드 아래에 존재 해야 하는 "마이크" 장치 기능 노드를 보여 줍니다.

```xml
 <?xml version="1.0" encoding="utf-8"?>
 <Package ...>
   <Identity ... />
   ...
   <Capabilities>
     <DeviceCapability Name="microphone" />
   </Capabilities>
 </Package>
```

## <a name="initialization-a-nameinit"></a>초기화 <a name="init">

인스턴스화하여 라이브러리와 상호 작용을 시작 하는 `GameChat2ChatManager` 채팅 동시 사용자 최대 수를 사용 하 여 개체 인스턴스에 추가 될 예정입니다. 이 값을 변경 하려면를 `GameChat2ChatManager` 개체 삭제 하 고 원하는 값을 사용 하 여 다시 만들어야 합니다. 텍스트를 포함할 수 있습니다 `GameChat2ChatManager` 한 번에 인스턴스화됩니다.

```cs
GameChat2ChatManager myGameChat2ChatManager = new GameChat2ChatManager(<maxChatUserCount>);
```

`GameChat2ChatManager` 개체 추가 (선택 사항)는 속성이에서 언제 든 지 구성할 수 있습니다. 다음 코드 예제는 변수 주어 집니다 값에서 속성을 설정 하는 사용 하기 전에 가정은 `myGameChat2ChatManager` 개체입니다.

```cs
GameChat2SpeechToTextConversionMode mySpeechToTextConversionMode;
GameChat2SharedDeviceCommunicationRelationshipResolutionMode mySharedDeviceResolutionMode;
GameChat2CommunicationRelationship myDefaultLocalToRemoteCommunicationRelationship;
float myDefaultAudioRenderVolume;
GameChat2AudioEncodingTypeAndBitrate myAudioEncodingTypeAndBitRate;

...

myGameChat2ChatManager.SpeechToTextConversionMode = mySpeechToTextConversionMode;
myGameChat2ChatManager.SharedDeviceResolutionMode = mySharedDeviceResolutionMode;
myGameChat2ChatManager.DefaultLocalToRemoteCommunicationRelationship = myDefaultLocalToRemoteCommunicationRelationship;
myGameChat2ChatManager.DefaultAudioRenderVolume = myDefaultAudioRenderVolume;
myGameChat2ChatManager.AudioEncodingTypeAndBitRate = myAudioEncodingTypeAndBitRate;
```

## <a name="configuring-users-a-nameconfig"></a>사용자 구성 <a name="config">

인스턴스가 초기화 되 면 GC2 인스턴스에 로컬 사용자를 추가 해야 합니다. 이 예제에서는 사용자를 A는 로컬 사용자를 나타냅니다.

```cs
string userAXuid;

...

IGameChat2ChatUser userA = myGameChat2ChatManager.AddLocalUser(userAXuid);
```

그런 다음 원격 사용자 및 원격 "끝점"을 나타내는 데 사용할 식별자를 추가 해야에 있는 각 원격 사용자입니다. "끝점"을 구현 하는 앱 소유 하는 개체에 표시 된 `IGameChat2Endpoint` 인터페이스입니다. 다음 예제에서는의 샘플 구현을 보여 줍니다 `MyEndpoint` 클래스를 구현 하는 `IGameChat2Endpoint`합니다.

```cs
class MyEndpoint : IGameChat2Endpoint
{
    private uint endpointIdentifier;
    public MyEndpoint(uint identifier)
    {
        endpointIdentifier = identifier;
    }

    // Implementing IsSameEndpointAs, the only method on the IGameChat2Endpoint interface
    public bool IsSameEndpointAs(IGameChat2Endpoint gameChat2Endpoint)
    {
        return endpointIdentifier == ((MyEndpoint)gameChat2Endpoint).endpointIdentifier;
    }
}
```

다음 코드 예제에서는 사용자 B, C 및 D에는 추가 되는 원격 사용자입니다. 사용자 B가 하나의 원격 장치에 및 다른 원격 장치에 사용자 C와 D. 이 예에서는 변수를 설정할 수는 가정 하 고 `myGameChat2ChatManager` 의 인스턴스이고 `GameChat2ChatManager` "MyEndpoint" 구현 하는 클래스는 및 `IGameChat2Endpoint`합니다.

```cs
string userBXuid;
string userCXuid;
string userDXuid;
MyEndpoint myRemoteEndpointOne;
MyEndpoint myRemoteEndpointTwo;

...

IGameChat2ChatUser remoteUserB = myGameChat2ChatManager.AddRemoteUser(userBXuid, myRemoteEndpointOne);
IGameChat2ChatUser remoteUserC = myGameChat2ChatManager.AddRemoteUser(userCXuid, myRemoteEndpointTwo);
IGameChat2ChatUser remoteUserD = myGameChat2ChatManager.AddRemoteUser(userDXuid, myRemoteEndpointTwo);
```

이제 각 원격 사용자는 로컬 사용자와의 통신 관계를 구성 합니다. 이 예제에서는 같은 팀에 사용자 A와 사용자 B는 및 양방향 통신이 허용 되는 가정 합니다. `GameChat2CommunicationRelationship.SendAndReceiveAll` 양방향 통신을 나타내는 정의 됩니다. 사용 하 여 사용자 B를 A의 사용자 관계를 설정 합니다.

```cs
GameChat2ChatUserLocal localUserA = userA as GameChat2ChatUserLocal;
localUserA.SetCommunicationRelationship(remoteUserB, GameChat2CommunicationRelationship.SendAndReceiveAll);
```

이제 사용자가 C와 D 'spectators'를 사용할 수 없습니다. 하지만 사용자에 게 수신 되도록 허용 되어야 가정 합니다. `GameChat2CommunicationRelationship.ReceiveAll` 이 정의이 단방향 통신 합니다. 사용 하 여 관계를 설정 합니다.

```cs
localUserA.SetCommunicationRelationship(remoteUserC, GameChat2CommunicationRelationship.ReceiveAll);
localUserA.SetCommunicationRelationship(remoteUserD, GameChat2CommunicationRelationship.ReceiveAll);
```

언제 든 지 많은 경우에 추가 된 원격 사용자는 `GameChat2ChatManager` 인스턴스를 만들지만 해도 모든 로컬 사용자-를 사용 하 여 통신 하도록 구성 되어! 이 사용자의 팀 결정할 또는 말하기 채널을 임의로 변경할 수 있는 시나리오에서 발생할 수 있습니다. 게임 채팅 2 시간에서 특정 지점에서 로컬 사용자에 게 말할 수 없는 경우에 가능한 모든 사용자의 채팅 2 게임을 알리는 데 유용 하므로 인스턴스에 추가 된 사용자에 대 한 정보 (예: 개인 정보 관계 및 신뢰도)만 캐시 됩니다.

마지막으로, 사용자 D 게임에서 로컬 게임 채팅 2 인스턴스에서 제거할 가정 합니다. 다음 호출을 사용 하 여 수행할 수 있습니다.

```cs
remoteUserD.Remove();
```

사용자 개체를 즉시 무효화 될 때 `Remove()` 라고 합니다. 사용자의 마지막 상태가 제거 될 때 캐시 됩니다. 모든 사용자 개체가 무효화 되 면 호출 되는 정보 제공 용 이므로 메서드는 제거 되기 전에 사용자의 상태를 반영 합니다. 다른 메서드는 호출 될 때 오류를 throw 합니다.

## <a name="processing-data-frames-a-namedata"></a>데이터 프레임을 처리합니다. <a name="data">

게임 채팅 2에 전송 계층을 자체 없습니다. 이 앱에서 제공 되어야 합니다. 이 플러그 인 응용 프로그램의 일반, 자주 호출 하 여 관리 되는 `GameChat2ChatManager.GetDataFrames()`합니다. 이 메서드는 게임 채팅 2 앱에 보내는 데이터를 제공 하는 방법. 전용 네트워킹 스레드에서 자주 폴링 될 수 있도록 신속 하 게 작동 하도록 설계 되었습니다. 이 네트워크 타이밍 지출 하거나 복잡 한 다중 스레드 콜백의 예측 불가능성을 고려 하는 방법에 대 한 걱정 없이 모든 큐에 대기 중인된 데이터를 검색 하는 편리한 장소를 제공 합니다.

때 `GameChat2ChatManager.GetDataFrames()` 가 호출 모두 목록의 큐에 대기 중인된 데이터를 보고 `IGameChat2DataFrame` 개체입니다. 검사할 앱 목록에 대해 반복 해야 합니다 `TargetEndpointIdentifiers`, 앱의 네트워킹 계층을 사용 하 여 해당 원격 앱 인스턴스에 데이터를 제공 합니다. 이 예에서 `HandleOutgoingDataFrame` 에서 데이터를 전송 하는 함수를 `Buffer` 에 지정 된 각 "끝점"에서 각 사용자에 게는 `TargetEndpointIdentifiers` 에 따라는 `TransportRequirement`합니다.

```cs
IReadOnlyList<IGameChat2DataFrame> frames = myGameChat2ChatManager.GetDataFrames();
foreach (IGameChat2DataFrame dataFrame in frames)
{
    HandleOutgoingDataFrame(
        dataFrame.Buffer,
        dataFrame.TargetEndpointIdentifiers,
        dataFrame.TransportRequirement
        );
}
```

데이터 프레임을 처리 하는 더 자주, 작을수록 더 낮은 오디오 대기 시간이 최종 사용자에 게는 표시 됩니다. 오디오는 40 ms 데이터 프레임에 결합 됩니다. 제안 된 폴링 기간입니다.

## <a name="processing-state-changes-a-namestate"></a>상태 변경 내용 처리 <a name="state">

앱의 일반, 자주 호출을 통해 받은 텍스트 메시지와 같은 앱에 업데이트를 제공 하는 게임 채팅 2는 `GameChat2ChatManager.GetStateChanges()` 메서드. UI 렌더링 루프에서 그래픽 프레임 마다 호출 될 수 있도록 신속 하 게 작동 하도록 설계 되었습니다. 이 네트워크 타이밍 지출 하거나 복잡 한 다중 스레드 콜백의 예측 불가능성을 고려 하는 방법에 대 한 걱정 없이 모든 큐에 대기 중인된 변경 내용을 검색 하는 데 편리한 위치를 제공 합니다.

때 `GameChat2ChatManager.GetStateChanges()` 는 호출 목록의 큐에 대기 중인된 업데이트를 모두 보고 됩니다 `IGameChat2StateChange` 개체입니다. 앱을 수행 해야합니다.
1. 목록 반복
2. 보다 구체적인 형식에 대 한 기본 구조를 검사 합니다.
3. 기본 구조를 캐스팅할 형식을 해당 자세한
4. 업데이트를 적절 하 게 처리 합니다. 

```cs
IReadOnlyList<IGameChat2StateChange> stateChanges = myGameChat2ChatManager.GetStateChanges();
foreach (IGameChat2StateChange stateChange in stateChanges)
{
    switch (stateChange.Type)
    {
        case GameChat2StateChangeType.CommunicationRelationshipAdjusterChanged:
        {
            MyHandleCommunicationRelationshipAdjusterChanged(stateChange as GameChat2CommunicationRelationshipAdjusterChangedStateChange);
            break;
        }
        case GameChat2StateChangeType.TextChatReceived:
        {
            MyHandleTextChatReceived(stateChange as GameChat2TextChatReceivedStateChange);
            break;
        }

        ...
    }
}
```

## <a name="text-chat-a-nametext"></a>텍스트 채팅 <a name="text">

사용 하 여 텍스트 채팅을 보내려면 `GameChat2ChatUserLocal.SendChatText()`합니다. 예를 들어 다음과 같은 가치를 제공해야 합니다.

```cs
localUserA.SendChatText("Hello");
```

게임 채팅 2는이 메시지를 포함 하는 데이터 프레임을 생성 합니다. 데이터 프레임에 대 한 대상 끝점에 연결 된 로컬 사용자의 텍스트를 수신 하도록 구성 된 사용자가 됩니다. 메시지를 통해 노출 될 데이터를 원격 끝점에서 처리 되 면을 `GameChat2TextChatReceivedStateChange`입니다. 음성 채팅와 마찬가지로 권한 및 개인 정보 제한은 텍스트 채팅에 대 한 적용 됩니다. 텍스트 채팅 허용 하도록 구성 된 한 쌍의 사용자 권한 및 개인 정보 제한을 통신을 허용 하지 않습니다 하지만 경우에 문자 메시지 삭제 됩니다.

텍스트 채팅 입력 및 표시를 지 원하는 내게 필요한 옵션에 대 한 필요 (참조 [내게 필요한 옵션](#access) 자세한).

## <a name="accessibility-a-nameaccess"></a>내게 필요한 옵션 <a name="access">

입력 텍스트 채팅 및 표시를 지 원하는 필요 합니다. 텍스트 입력 이므로 필요한 플랫폼, 지금까지 사용 하 여 광범위 한 실제 키보드를 보유 하지 않은 게임 장르 에서도 사용자 텍스트 음성 변환 보조 기술을 사용 하 여 시스템 구성할 수 있습니다. 마찬가지로, 텍스트 표시 되므로 필요한 사용자 음성-텍스트를 사용 하도록 시스템을 구성할 수 있습니다. 이러한 기본 설정을 확인 하 여 로컬 사용자에서 검색할 수는 `GameChat2ChatUserLocal.TextToSpeechConversionPreferenceEnabled` 및 `GameChat2ChatUserLocal.SpeechToTextConversionPreferenceEnabled` 속성 각각을 조건부로 텍스트 메커니즘을 사용 하도록 설정 하려고 할 수 있습니다.

### <a name="text-to-speech"></a>텍스트 음성 변환

사용자에 게 사용 하도록 설정 하는 텍스트 음성 변환 `GameChat2ChatUserLocal.TextToSpeechConversionPreferenceEnabled` 'true'가 됩니다. 이 상태가 감지 되 면 앱의 텍스트 입력 메서드를 제공 해야 합니다. 실제 또는 가상 키보드에서 제공 되는 텍스트 입력을 만든 후에 문자열을 전달 합니다 `GameChat2ChatUserLocal.SynthesizeTextToSpeech()` 메서드. 게임 채팅 2 감지 하 고 문자열 및 사용자의 액세스 가능한 음성 기본 설정을 기반으로 하는 오디오 데이터를 합성 합니다. 예를 들어 다음과 같은 가치를 제공해야 합니다.

```cs
localUserA.SynthesizeTextToSpeech("Hello");
```

이 작업의 일환으로 합성 오디오가 로컬 사용자에서 오디오를 수신 하도록 구성 된 모든 사용자에 게 전송 됩니다. 경우 `GameChat2ChatUserLocal.SynthesizeTextToSpeech()` 작업도 발생 하지 않도록 설정 된 게임 채팅 2는 텍스트 음성 변환 되지 않은 사용자에서 호출 됩니다.

### <a name="speech-to-text"></a>음성-텍스트

사용자가 음성-텍스트를 사용 하는 경우 `GameChat2ChatUserLocal.SpeechToTextConversionPreferenceEnabled` true가 됩니다. 이 상태가 감지 되 면 기록된 채팅 메시지와 연결 된 UI를 제공 하는 앱 준비 되어야 합니다. GC는 자동으로 각 원격 사용자의 오디오를 기록 하 고를 통해 노출 된 `GameChat2TranscribedChatReceivedStateChange`합니다.

### <a name="speech-to-text-performance-considerations"></a>음성-텍스트 성능 고려 사항

음성-텍스트를 사용 하는 경우 각 원격 장치에서 게임 채팅 2 인스턴스 음성 서비스 끝점을 사용 하 여 웹 소켓 연결을 시작 합니다. 각 원격 게임 채팅 2 클라이언트;이 websocket 통해 음성 서비스 끝점에 오디오를 업로드합니다 경우에 따라 음성 서비스 끝점을 원격 장치에 기록 메시지를 반환합니다. 원격 장치를 기록 메시지를 전송 합니다 (즉, 문자 메시지) 로컬 장치에 있는 기록된 메시지에 지정 됩니다 게임 채팅 2로 앱을 렌더링 하 합니다.

따라서 음성-텍스트의 기본 성능 저하는 네트워크 사용량. 대부분의 네트워크 트래픽이 인코딩된 오디오 업로드입니다. Websocket이 이미 인코딩된 게임 채팅 2로 "normal" 음성 채팅 경로;에 오디오를 업로드합니다 앱에 제어를 통해 비트 전송률 `GameChat2ChatManager.AudioEncodingTypeAndBitrate`합니다.

## <a name="ui-a-nameui"></a>UI <a name="UI">

어디서 나 플레이어에에서 표시 되어 있는지, 특히는 스코어 같은 게이머 태그의 목록을 사용자에 대 한 피드백으로 말하기 음소거 아이콘도 표시 하는 것이 좋습니다. `IGameChat2ChatUser.ChatIndicator` 속성은 플레이어에 대 한 채팅의 즉각적인, 현재 상태를 나타냅니다. 다음 예제에 대 한 표시기 값을 검색 하는 방법을 보여 줍니다는 `IGameChat2ChatUser` 'iconToShow' 변수에 값을 할당할 특정 아이콘 상수 값을 확인 하려면 ' userA' 변수가 가리키는 개체:

```cs
switch (userA.ChatIndicator)
{
    case GameChat2UserChatIndicator.Silent:
    {
        iconToShow = Icon_InactiveSpeaker;
        break;
    }
    case GameChat2UserChatIndicator.Talking:
    {
        iconToShow = Icon_ActiveSpeaker;
        break;
    }
    case GameChat2UserChatIndicator.LocalMicrophoneMuted:
    {
        iconToShow = Icon_MutedSpeaker;
        break;
    }
    
    ...
}
```

변수의 `IGameChat2ChatUser.ChatIndicator` 예를 들어 플레이어 시작 및 중지 통신으로 자주 변경 해야 합니다. 폴링 해당 UI 프레임 마다 결과적으로 앱을 지원 하도록 설계 되었습니다.

## <a name="muting-a-namemute"></a>음소거 <a name="mute">

`GameChat2ChatUserLocal.MicrophoneMuted` 속성의 로컬 사용자의 마이크 음소거 상태 전환에 사용할 수 있습니다. 마이크 음소거 되 면 해당 마이크에서 오디오 없는 캡처됩니다. 사용자가 공유 장치, Kinect, 예: 음소거 상태를 모든 사용자에 게 적용 됩니다. 이 메서드는 하드웨어 음소거 컨트롤 (예: 사용자의 헤드셋에 단추)를 반영 하지 않습니다. 게임 채팅 2를 통해 사용자의 오디오 장치 하드웨어 음소거 상태를 검색 하는 방법은 없습니다.

`GameChat2ChatUserLocal.SetRemoteUserMuted()` 메서드를 특정 로컬 사용자와 관련 하 여 원격 사용자의 음소거 상태 전환에 사용할 수 있습니다. 원격 사용자 음소거 되어 로컬 사용자 오디오 듣거나 원격 사용자의 모든 텍스트 메시지를 수신 하지 않습니다.

## <a name="bad-reputation-auto-mute-a-nameautomute"></a>잘못 된 평판 자동 음소거 <a name="automute">

일반적으로 원격 사용자는 시작 unmuted 합니다. 게임 채팅 2가 (1) 원격 사용자가 로컬 사용자와 친구 및 (2) 원격 사용자가 잘못 된 평판 플래그 면 음소거 상태의 사용자가 시작 됩니다. 사용자가이 작업으로 인해 음소거 되 면 `IGameChat2ChatUser.ChatIndicator` 돌아갑니다 `GameChat2UserChatIndicator.ReputationRestricted`합니다. 첫 번째 호출에서이 상태를 재정의할 수는 `GameChat2ChatUserLocal.SetRemoteUserMuted()` 대상 사용자로 원격 사용자를 포함 하는 합니다.

## <a name="privilege-and-privacy-a-namepriv"></a>권한 및 개인 정보 <a name="priv">

게임에서 구성 하 여 통신 관계를 기반으로 게임 채팅 2 권한 및 개인 정보 제한을 적용 합니다. 게임 채팅 2 권한 및 개인 정보 보호 제한에서 조회를 수행 먼저 사용자를 추가 합니다. 사용자의 `IGameChat2ChatUser.ChatIndicator` 는 항상 반환 `GameChat2UserChatIndicator.Silent` 해당 작업이 완료 될 때까지 합니다. 사용자와의 통신을 권한 또는 개인 정보 보호 제한, 사용자의 영향을 받는 경우 `IGameChat2ChatUser.ChatIndicator` 돌아갑니다 `GameChat2UserChatIndicator.PlatformRestricted`합니다. 텍스트 및 음성 채팅;에 플랫폼 통신 제한 사항이 적용 됩니다. 인스턴스는 텍스트 채팅 플랫폼 제한에 의해 차단 됩니다 아닌 음성 채팅, 또는 그 반대로 안 됩니다.

`GameChat2ChatUserLocal.GetEffectiveCommunicationRelationship()` 불완전 한 권한 및 개인 정보 작업으로 인해 사용자가 통신할 수 없는 경우와 구분 하는 데 사용할 수 있습니다. 형태로 게임 채팅 2에 의해 적용 통신 관계가 반환 `GameChat2CommunicationRelationship` 이유 관계 동일 하지 않을 구성 관계의 형태로 및 `GameChat2CommunicationRelationshipAdjuster`합니다. 예를 들어 조회 작업은 계속 진행에서 합니다 `GameChat2CommunicationRelationshipAdjuster` 됩니다 `GameChat2CommunicationRelationshipAdjuster.Initializing`합니다. 이 메서드는 개발 및 디버깅 시나리오에서 사용할 수 해야 UI에 영향을 줄을 사용 해야 (참조 [UI](#UI)).

## <a name="cleanup-a-namecleanup"></a>정리 <a name="cleanup">

앱에서 게임 채팅 2를 통해 통신을 더 이상 필요한 경우 호출 해야 `GameChat2ChatManager.Dispose()`합니다. 이 게임 채팅 2 통신을 관리 하려면 할당 된 리소스를 회수할 수 있습니다.

## <a name="how-to-configure-popular-scenarios"></a>인기 있는 시나리오를 구성 하는 방법

### <a name="push-to-talk"></a>푸시-강연

강연에 푸시를 사용 하 여 구현 되어야 합니다 `GameChat2ChatUserLocal.MicrophoneMuted`합니다. 설정 `MicrophoneMuted` 을 false로 음성 및 `MicrophoneMuted` true로 제한 합니다. 이 속성의 변경 게임 채팅 2에서 가장 낮은 대기 시간 응답을 제공 합니다.

### <a name="teams"></a>팀

블루 팀에서 사용자 A와 사용자 B는 있고 사용자 C와 D 사용자 팀 Red에 있다고 가정 합니다. 각 사용자가 앱의 고유 인스턴스입니다.

사용자 A의 장치:

```cs
localUserA.SetCommunicationRelationship(remoteUserB, GameChat2CommunicationRelationship.SendAndReceiveAll);
localUserA.SetCommunicationRelationship(remoteUserC, GameChat2CommunicationRelationship.None);
localUserA.SetCommunicationRelationship(remoteUserD, GameChat2CommunicationRelationship.None);
```

사용자 B의 장치:

```cs
localUserB.SetCommunicationRelationship(remoteUserA, GameChat2CommunicationRelationship.SendAndReceiveAll);
localUserB.SetCommunicationRelationship(remoteUserC, GameChat2CommunicationRelationship.None);
localUserB.SetCommunicationRelationship(remoteUserD, GameChat2CommunicationRelationship.None);
```

사용자 C 장치의 경우:

```cs
localUserC.SetCommunicationRelationship(remoteUserA, GameChat2CommunicationRelationship.None);
localUserC.SetCommunicationRelationship(remoteUserB, GameChat2CommunicationRelationship.None);
localUserC.SetCommunicationRelationship(remoteUserD, GameChat2CommunicationRelationship.SendAndReceiveAll);
```

사용자 D 장치의 경우:

```cs
localUserD.SetCommunicationRelationship(remoteUserA, GameChat2CommunicationRelationship.None);
localUserD.SetCommunicationRelationship(remoteUserB, GameChat2CommunicationRelationship.None);
localUserD.SetCommunicationRelationship(remoteUserC, GameChat2CommunicationRelationship.SendAndReceiveAll);
```

### <a name="broadcast"></a>방송

사용자는 주문을 제공 리더가 고 사용자 B, C 및 D만 수신할 수 있는지를 가정 합니다. 각 플레이어는 고유한 장치 켜져 있습니다.

사용자 A의 장치:

```cs
localUserA.SetCommunicationRelationship(remoteUserB, GameChat2CommunicationRelationship.SendAll);
localUserA.SetCommunicationRelationship(remoteUserC, GameChat2CommunicationRelationship.SendAll);
localUserA.SetCommunicationRelationship(remoteUserD, GameChat2CommunicationRelationship.SendAll);
```

사용자 B의 장치:

```cs
localUserB.SetCommunicationRelationship(remoteUserA, GameChat2CommunicationRelationship.ReceiveAll);
localUserB.SetCommunicationRelationship(remoteUserC, GameChat2CommunicationRelationship.None);
localUserB.SetCommunicationRelationship(remoteUserD, GameChat2CommunicationRelationship.None);
```

사용자 C 장치의 경우:

```cs
localUserC.SetCommunicationRelationship(remoteUserA, GameChat2CommunicationRelationship.ReceiveAll);
localUserC.SetCommunicationRelationship(remoteUserB, GameChat2CommunicationRelationship.None);
localUserC.SetCommunicationRelationship(remoteUserD, GameChat2CommunicationRelationship.None);
```

사용자 D 장치의 경우:

```cs
localUserD.SetCommunicationRelationship(remoteUserA, GameChat2CommunicationRelationship.ReceiveAll);
localUserD.SetCommunicationRelationship(remoteUserB, GameChat2CommunicationRelationship.None);
localUserD.SetCommunicationRelationship(remoteUserC, GameChat2CommunicationRelationship.None);
```
