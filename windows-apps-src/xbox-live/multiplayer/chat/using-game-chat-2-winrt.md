---
title: 게임 채팅 2 2(winrt 프로젝션 사용
author: KevinAsgari
description: 2(winrt 프로젝션 있는 Xbox Live 게임 채팅 2를 사용 하 여 음성 통신을 게임에 추가 하는 방법을 알아봅니다.
ms.author: kevinasg
ms.date: 4/11/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: xbox live, xbox, 게임, uwp, windows 10, 하나는 xbox, 게임 채팅 2, 게임 채팅, 음성 통신
ms.localizationpriority: medium
ms.openlocfilehash: 5ca62427a5e8f51143ef9e40faf24272ffbe97d7
ms.sourcegitcommit: 1c6325aa572868b789fcdd2efc9203f67a83872a
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/17/2018
ms.locfileid: "4755460"
---
# <a name="using-game-chat-2-winrt-projections"></a>게임 채팅 2 (2(winrt 프로젝션)를 사용 하 여

게임 채팅 2의 C# API 사용에 대 한 간략 한 연습입니다. C + +를 통해 게임 채팅 2에 액세스 하려는 게임 개발자 [Game Chat 2를 사용 하 여](using-game-chat-2.md)보일 것입니다.

1. [필수 구성 요소](#prereq)
2. [초기화](#init)
3. [사용자 구성](#config)
4. [데이터 프레임 처리](#data)
5. [상태 변경 내용 처리](#state)
6. [텍스트 채팅](#text)
7. [접근성](#access)
8. [UI](#UI)
9. [음소거](#mute)
10. [나쁜 평판 자동 음소거](#automute)
11. [권한 및 개인 정보](#priv)
12. [정리](#cleanup)
13. [인기 있는 시나리오를 구성 하는 방법](#how-to-configure-popular-scenarios)

## <a name="prerequisites-a-nameprereq"></a>필수 구성 요소<a name="prereq">

게임 채팅 2를 사용 하기 위해 [Microsoft.Xbox.Services.GameChat2 nuget 패키지](https://www.nuget.org/packages/Microsoft.Xbox.Game.Chat.2.WinRT.UWP/)를 포함 해야 합니다.

게임 채팅 2를 사용 하 여 코딩을 시작 하기 전에 "마이크" 장치 기능을 선언 하는 앱의 AppXManifest 구성 했지만 해야 합니다. AppXManifest 기능 플랫폼 설명서;의 해당 각 섹션에서 자세히 설명 되어 있습니다. 다음 조각은 패키지/기능 노드 아래에 있어야 하는 "마이크" 장치 접근 권한 값 노드를 보여 줍니다. 그렇지 않으면 채팅 차단 됩니다.

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

## <a name="initialization-a-nameinit"></a>초기화<a name="init">

인스턴스화하여 라이브러리와 상호 작용을 시작 하기는 `GameChat2ChatManager` 동시 채팅 사용자의 최대 수를 사용 하 여 개체 인스턴스를 추가 될 예정입니다. 이 값을 변경 하는 `GameChat2ChatManager` 개체를 삭제 하 고 원하는 값으로 다시 작성 해야 합니다. 하나만 사용할 수 있습니다 `GameChat2ChatManager` 한 번에 인스턴스화됩니다.

```cs
GameChat2ChatManager myGameChat2ChatManager = new GameChat2ChatManager(<maxChatUserCount>);
```

`GameChat2ChatManager` 개체에 추가, 선택적 속성을 언제 든 지에서 구성할 수 있습니다. 다음 코드 예제는 변수 주어 집니다 값에서 속성을 설정 하는 사용 하기 전에 가정 합니다 `myGameChat2ChatManager` 개체.

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

## <a name="configuring-users-a-nameconfig"></a>사용자 구성<a name="config">

인스턴스를 초기화 되 면 로컬 사용자 g c 2 인스턴스를 추가 해야 합니다. 이 예제에서는 A 사용자를 로컬 사용자를 나타냅니다.

```cs
string userAXuid;

...

IGameChat2ChatUser userA = myGameChat2ChatManager.AddLocalUser(userAXuid);
```

그런 다음 원격 사용자 및 원격 "끝점"를 나타내는 데 사용할 identifers 추가 해야 각 원격 사용자가 켜져 있는 합니다. "끝점"를 구현 하는 앱을 소유한 개체에 표시는 `IGameChat2Endpoint` 인터페이스입니다. 다음 예제에서는의 샘플 구현 `MyEndpoint` 클래스를 구현 하는 `IGameChat2Endpoint`.

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

다음 코드 예제에서는 사용자 B, C 및 D 추가 되는 원격 사용자가 있습니다. 사용자 B 한 원격 디바이스에 있고 C 및 D 사용자가 다른 원격 장치에 합니다. 이 예제에서는 변수를 설정 하는 것을 가정 하 고 `myGameChat2ChatManager` 의 인스턴스가 `GameChat2ChatManager` "MyEndpoint"는 구현 하는 클래스 및 `IGameChat2Endpoint`합니다.

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

이제 각 원격 사용자와 로컬 사용자 간의 통신 관계를 구성 합니다. 이 예제에서는 사용자 A와 B 사용자가 같은 팀에 있으며 양방향 통신을 허용 되는 가정 합니다. `GameChat2CommunicationRelationship.SendAndReceiveAll` 양방향 통신을 나타내는 정의 됩니다. 사용 하 여 사용자 B 사용자 A의 관계를 설정 합니다.

```cs
GameChat2ChatUserLocal localUserA = userA as GameChat2ChatUserLocal;
localUserA.SetCommunicationRelationship(remoteUserB, GameChat2CommunicationRelationship.SendAndReceiveAll);
```

이제 사용자가 C 및 D 'spectators'를 사용할 수 없습니다. 하지만 사용자 A 수신 하도록 할지 가정 합니다. `GameChat2CommunicationRelationship.ReceiveAll` 정의이 단방향 통신 합니다. 와 관계를 설정 합니다.

```cs
localUserA.SetCommunicationRelationship(remoteUserC, GameChat2CommunicationRelationship.ReceiveAll);
localUserA.SetCommunicationRelationship(remoteUserD, GameChat2CommunicationRelationship.ReceiveAll);
```

언제 든 지 있는지에 추가 된 원격 사용자는 `GameChat2ChatManager` 인스턴스 하지만 요구 사항을 충족 하는 모든 로컬 사용자-와 통신 하도록 구성 된 하지 않은! 이 수 예상 되는 시나리오 있는 팀을 결정 하는 사용자나 임의로 말하는 채널을 변경할 수 있습니다. 게임 채팅 2 시간에 특정 지점에서 모든 로컬 사용자에 게 말할 수 없는 경우에 모든 사용자의 게임 채팅 2를 알리기 위해 유용 하므로 인스턴스에 추가 된 사용자에 대 한 정보 (예: 개인 정보 및 관계 신뢰도)만 캐시 됩니다.

마지막으로 한다고 가정는 사용자가 D 게임에서 게임 채팅 2 인스턴스를 로컬에서 제거 해야 합니다. 다음을 호출 하는 수행 될 수 있습니다.

```cs
remoteUserD.Remove();
```

사용자 개체 즉시 무효화 되 면 `Remove()` 라고 합니다. 제거 되 면 사용자의 마지막 상태는 캐시 됩니다. 사용자 개체를 무효화 한 후에 호출 모든 정보 메서드 제거 되기 전에 사용자의 상태를 반영 됩니다. 다른 메서드 호출 되 면 오류가 throw 됩니다.

## <a name="processing-data-frames-a-namedata"></a>데이터 프레임 처리<a name="data">

게임 채팅 2는 고유한 전송 계층을; 없습니다. 이 앱에서 제공 되어야 합니다. 이 플러그 인 앱의 일반, 자주 호출을 통해 관리 되는 `GameChat2ChatManager.GetDataFrames()`. 이 방법은 게임 채팅 2 앱에 보내는 데이터를 제공 하는 방법입니다. 전용 네트워킹 스레드에서 자주 폴링할 수 있도록 신속 하 게 작동 하도록 설계 되었습니다. 이 네트워크 타이밍 또는 다중 스레드 콜백 복잡성 예측 불가능성 상관 없이 모든 대기 중인된 데이터를 검색할 수 있는 편리한 위치를 제공 합니다.

때 `GameChat2ChatManager.GetDataFrames()` 가 호출 모든 대기 중인된 데이터를 보고 하는 목록에서 `IGameChat2DataFrame` 개체. 검사 목록에서 앱 반복 해야 합니다 `TargetEndpointIdentifiers`, 및 앱의 네트워킹 계층을 사용 하 여 적절 한 원격 앱 인스턴스를 데이터를 제공 합니다. 이 예제에서는 `HandleOutgoingDataFrame` 에서 데이터를 전송 하는 함수는 `Buffer` 에 지정 된 각 "끝점"에서 각 사용자에 게는 `TargetEndpointIdentifiers` 에 따라는 `TransportRequirement`.

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

데이터 프레임 처리 되는 더 자주, 낮을수록 오디오 대기 시간 최종 사용자에 게 표시 됩니다. 오디오는 40 ms 데이터 프레임;으로 결합 됩니다. 제안 된 폴링 간격입니다.

## <a name="processing-state-changes-a-namestate"></a>상태 변경 내용 처리<a name="state">

앱의 일반, 자주 호출을 통해 받은 텍스트 메시지 등의 앱에 대 한 업데이트를 제공 하는 게임 채팅 2는 `GameChat2ChatManager.GetStateChanges()` 메서드. UI 렌더링 루프의 그래픽 프레임 마다 호출 될 수 있도록 신속 하 게 작동 하도록 설계 되었습니다. 이 네트워크 타이밍 또는 다중 스레드 콜백 복잡성 예측 불가능성 상관 없이 대기 중인된 모든 변경 내용을 검색 하는 편리한 위치를 제공 합니다.

때 `GameChat2ChatManager.GetStateChanges()` 는 호출 대기 중인된 모든 업데이트 목록에 보고 됩니다 `IGameChat2StateChange` 개체입니다. 앱은 다음과 같습니다.
1. 목록에서 반복
2. 더욱 구체적인 해당 형식에 대 한 기본 구조를 검사 합니다.
3. 기본 구조를 캐스팅 해당 요소에 더 자세한 유형
4. 해당 업데이트를 적절 하 게 처리 합니다. 

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

## <a name="text-chat-a-nametext"></a>텍스트 채팅<a name="text">

사용 하 여 텍스트 채팅을 보내려면 `GameChat2ChatUserLocal.SendChatText()`. 예를 들면 다음과 같습니다.

```cs
localUserA.SendChatText("Hello");
```

게임 채팅 2에서이 메시지를 포함 하는 데이터 프레임을 생성 데이터 프레임에 대 한 대상 끝점 사용자가 로컬에서 텍스트를 수신 하도록 구성 된 사용자와 연결 됩니다. 원격 끝점에서 데이터를 처리할 때 메시지를 통해 노출 되는 `GameChat2TextChatReceivedStateChange`. 음성 채팅와 마찬가지로 텍스트 채팅에 대 한 권한 및 개인 정보 보호 제한은 유지 됩니다. 한 쌍의 사용자가 텍스트 채팅을 허용 하도록 구성 된 경우 권한 또는 개인 정보 보호 제한 통신을 허용 하지 않습니다, 텍스트 메시지 삭제 됩니다.

접근성에 대 한 필수는 입력 텍스트 채팅 및 디스플레이 지원 ( [접근성](#access) 에 대 한 자세한 내용은 참조).

## <a name="accessibility-a-nameaccess"></a>접근성<a name="access">

입력 텍스트 채팅 및 디스플레이 지원 하는 것이 필요 합니다. 텍스트 입력이 플랫폼 또는 지금까지 본 적이 광범위 한 실제 키보드를 사용 하는 게임 장르에도 사용자가 텍스트 음성 변환 보조 기술을 사용 하 여 시스템 구성할 수 있기 때문에 필요 합니다. 마찬가지로, 텍스트 표시는 사용자가 음성-텍스트를 사용 하 여 시스템을 구성할 수 있으므로 필요 합니다. 이러한 기본 설정을 확인 하 여 로컬 사용자에 게 감지할 수는 `GameChat2ChatUserLocal.TextToSpeechConversionPreferenceEnabled` 및 `GameChat2ChatUserLocal.SpeechToTextConversionPreferenceEnabled` 속성 각각 및 조건부로 텍스트 메커니즘을 사용 하고자 할 수 있습니다.

### <a name="text-to-speech"></a>텍스트 음성 변환

사용자가 사용 하도록 설정 하는 텍스트 음성 변환 하는 경우 `GameChat2ChatUserLocal.TextToSpeechConversionPreferenceEnabled` 'true' 됩니다. 이 상태 감지 되 면 앱 텍스트 입력의 메서드를 제공 해야 합니다. 실제 또는 가상 키보드에서 제공 하는 텍스트 입력을 구성한 후에 문자열을 전달 합니다 `GameChat2ChatUserLocal.SynthesizeTextToSpeech()` 메서드. 게임 채팅 2은 검색 하 고 문자열 및 사용자의 음성 액세스할 수 있는 기본 설정에 따라 오디오 데이터를 합성 합니다. 예를 들면 다음과 같습니다.

```cs
localUserA.SynthesizeTextToSpeech("Hello");
```

이 작업의 일부로 합성 오디오가 로컬 사용자의 오디오를 수신 하도록 구성 된 모든 사용자에 게 전송 됩니다. 하는 경우 `GameChat2ChatUserLocal.SynthesizeTextToSpeech()` 사용자가 아무 작업도 텍스트 음성 변환 사용된 Game Chat 2가 없는 스레드에서 호출 됩니다.

### <a name="speech-to-text"></a>음성-텍스트

음성-텍스트를 사용 하도록 설정 되어 있는 경우 `GameChat2ChatUserLocal.SpeechToTextConversionPreferenceEnabled` 발생할 수 있습니다. 이 상태 감지 되 면 앱 변환 된 채팅 메시지와 관련 된 UI를 제공 하기 위해 준비 해야 합니다. GC는 자동으로 각 원격 사용자의 오디오 번역기 변환 하 고 통해 노출는 `GameChat2TranscribedChatReceivedStateChange`.

### <a name="speech-to-text-performance-considerations"></a>음성-텍스트 성능 고려 사항

음성-텍스트를 사용 하면 각 원격 장치에서 게임 채팅 2 인스턴스 음성 서비스 끝점을 사용 하 여 웹 소켓 연결을 시작 합니다. 이 websocket; 통해 음성 서비스 끝점을 오디오를 업로드 하는 각 원격 Game Chat 2 클라이언트 경우에 따라 음성 서비스 끝점 원격 장치에 기록과 메시지를 반환합니다. 원격 디바이스는 기록과 메시지를 전송 합니다 (즉, 문자 메시지) 로컬 장치에 있는 transcribed 메시지는 제공한 Game Chat 2 앱에 렌더링 합니다.

따라서 음성-텍스트의 기본 성능 저하는 네트워크 사용 합니다. 대부분의 네트워크 트래픽은 인코딩된 오디오 업로드 됩니다. Websocket이 이미 인코딩된 게임 채팅 2 "일반" 음성 채팅 경로; 오디오를 업로드합니다 앱을 통해 비트 전송률 제어에 `GameChat2ChatManager.AudioEncodingTypeAndBitrate`.

## <a name="ui-a-nameui"></a>UI<a name="UI">

아무 곳 이나 플레이어에에서 표시 되어 있는지, 특히 게이머 태그는 스코어 보드 같은 목록을 사용자에 대 한 피드백으로 음소거/말하기 아이콘도 표시 하는 것이 좋습니다. `IGameChat2ChatUser.ChatIndicator` 속성 해당 플레이어에 대 한 채팅 순간, 현재 상태를 나타냅니다. 다음 예제는 표시기에 대 한 값을 검색 하는 `IGameChat2ChatUser` 변수를 'iconToShow' 변수에 할당 하는 특정 아이콘 상수 값을 확인 하려면 ' 사용자 ' a 가리키는 개체:

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

값 `IGameChat2ChatUser.ChatIndicator` 예를 들어에 플레이어 시작 및 중지 말하기, 자주 변경 해야 합니다. 폴링 것 UI 프레임 마다 결과적으로 앱을 지원 하도록 설계 되었습니다.

## <a name="muting-a-namemute"></a>음소거<a name="mute">

`GameChat2ChatUserLocal.MicrophoneMuted` 로컬 사용자의 마이크의 음소거 상태를 전환 하 속성을 사용할 수 있습니다. 마이크가 음소거 되어, 해당 마이크에서 오디오 없음 캡처됩니다. Kinect 등의 공유 장치에서 사용자가 음소거 상태 모든 사용자에 게 적용 됩니다. 이 메서드는 하드웨어 음소거 컨트롤 (예: 사용자의 헤드셋에서 단추)을 반영 하지 않습니다. 게임 채팅 2를 통해 사용자의 오디오 장치의 하드웨어 음소거 상태를 검색할 수 있는 방법은 없습니다.

`GameChat2ChatUserLocal.SetRemoteUserMuted()` 특정 로컬 사용자와 관련 하 여 원격 사용자의 음소거 상태를 전환 하 메서드를 사용할 수 있습니다. 원격 사용자 음이 소거 로컬 사용자가 모든 오디오를 들 되지 않거나 원격 사용자 로부터 텍스트 메시지를 수신 합니다.

## <a name="bad-reputation-auto-mute-a-nameautomute"></a>나쁜 평판 자동 음소거<a name="automute">

일반적으로 원격 사용자가 시작 unmuted 합니다. 게임 채팅 2 (1) 원격 사용자가 로컬 사용자를 사용 하 여 친구 및 (2) 원격 사용자가 잘못 된 평판 플래그 때 음소거 상태에서 사용자가 시작 됩니다. 이 작업으로 인해 사용자는 음소거 때 `IGameChat2ChatUser.ChatIndicator` 돌아올 `GameChat2UserChatIndicator.ReputationRestricted`. 이 상태를 처음 호출 하면 보다 우선 합니다 `GameChat2ChatUserLocal.SetRemoteUserMuted()` 대상 사용자로 원격 사용자 포함 합니다.

## <a name="privilege-and-privacy-a-namepriv"></a>권한 및 개인 정보<a name="priv">

게임에서 구성 된 통신 관계를 기반으로 게임 채팅 2 권한 및 개인 정보 보호 제한을 적용 합니다. 게임 채팅 2 권한 및 개인 정보 보호 제한 조회 수행 하는 사용자가 처음 추가 합니다. 사용자의 `IGameChat2ChatUser.ChatIndicator` 는 항상 반환 `GameChat2UserChatIndicator.Silent` 해당 작업이 완료 될 때까지 합니다. 사용자와의 통신을 권한 또는 개인 정보 restriciton, 사용자의에 영향을 받는 경우 `IGameChat2ChatUser.ChatIndicator` 돌아올 `GameChat2UserChatIndicator.PlatformRestricted`. 플랫폼 통신에 제한이 모두 음성 및 텍스트 채팅; 여기서 텍스트 채팅은 플랫폼 제한에 의해 차단 하지 않으면이 음성 채팅, 또는 그 반대로 인스턴스 안 됩니다.

`GameChat2ChatUserLocal.GetEffectiveCommunicationRelationship()` 불완전 한 권한 및 개인 정보 보호 작업으로 인해 통신 하지 못하는 사용자를 구별 하는 데 사용할 수 있습니다. 통신 관계의 형태로 Game Chat 2에 의해 적용 반환 `GameChat2CommunicationRelationship` 을 이유 관계 되지 않을 구성 된 관계의 형태로 같지 `GameChat2CommunicationRelationshipAdjuster`. 예를 들어 조회 작업이 진행 되는 `GameChat2CommunicationRelationshipAdjuster` 는 `GameChat2CommunicationRelationshipAdjuster.Initializing`. 이 메서드는 개발 및 디버깅 시나리오; 사용할 것으로 예상 UI에 영향을 사용 해야 ( [UI](#UI)참조).

## <a name="cleanup-a-namecleanup"></a>정리<a name="cleanup">

호출 해야 앱에서 더 이상 Game Chat 2를 통해 통신을 필요로 경우 `GameChat2ChatManager.Dispose()`. 이 통해 통신을 관리 하려면 할당 된 리소스를 확보 하기 위해 Game Chat 2.

## <a name="how-to-configure-popular-scenarios"></a>인기 있는 시나리오를 구성 하는 방법

### <a name="push-to-talk"></a>푸시-통화

푸시-작용 구현 되어야 합니다 `GameChat2ChatUserLocal.MicrophoneMuted`. 설정 `MicrophoneMuted` 을 false로 음성 및 `MicrophoneMuted` true로 제한 합니다. 이 속성 변경 Game Chat 2에서 가장 낮은 대기 시간 응답을 제공 합니다.

### <a name="teams"></a>Teams

사용자 A와 B 사용자가 파란색 팀에 사용자 C 및 D 사용자가 팀 빨간색에 한다고 가정 합니다. 각 사용자가 앱의 고유한 인스턴스.

A의 사용자 장치의:

```cs
localUserA.SetCommunicationRelationship(remoteUserB, GameChat2CommunicationRelationship.SendAndReceiveAll);
localUserA.SetCommunicationRelationship(remoteUserC, GameChat2CommunicationRelationship.None);
localUserA.SetCommunicationRelationship(remoteUserD, GameChat2CommunicationRelationship.None);
```

사용자 B 장치의:

```cs
localUserB.SetCommunicationRelationship(remoteUserA, GameChat2CommunicationRelationship.SendAndReceiveAll);
localUserB.SetCommunicationRelationship(remoteUserC, GameChat2CommunicationRelationship.None);
localUserB.SetCommunicationRelationship(remoteUserD, GameChat2CommunicationRelationship.None);
```

사용자가 C 장치의:

```cs
localUserC.SetCommunicationRelationship(remoteUserA, GameChat2CommunicationRelationship.None);
localUserC.SetCommunicationRelationship(remoteUserB, GameChat2CommunicationRelationship.None);
localUserC.SetCommunicationRelationship(remoteUserD, GameChat2CommunicationRelationship.SendAndReceiveAll);
```

사용자가 D 장치의:

```cs
localUserD.SetCommunicationRelationship(remoteUserA, GameChat2CommunicationRelationship.None);
localUserD.SetCommunicationRelationship(remoteUserB, GameChat2CommunicationRelationship.None);
localUserD.SetCommunicationRelationship(remoteUserC, GameChat2CommunicationRelationship.SendAndReceiveAll);
```

### <a name="broadcast"></a>브로드캐스트

사용자 A가 주문 제공 리더 사용자 B, C 및 D 수만 수신 대기 한다고 가정 합니다. 각 플레이어는 고유의 장치 켜져 있습니다.

A의 사용자 장치의:

```cs
localUserA.SetCommunicationRelationship(remoteUserB, GameChat2CommunicationRelationship.SendAll);
localUserA.SetCommunicationRelationship(remoteUserC, GameChat2CommunicationRelationship.SendAll);
localUserA.SetCommunicationRelationship(remoteUserD, GameChat2CommunicationRelationship.SendAll);
```

사용자 B 장치의:

```cs
localUserB.SetCommunicationRelationship(remoteUserA, GameChat2CommunicationRelationship.ReceiveAll);
localUserB.SetCommunicationRelationship(remoteUserC, GameChat2CommunicationRelationship.None);
localUserB.SetCommunicationRelationship(remoteUserD, GameChat2CommunicationRelationship.None);
```

사용자가 C 장치의:

```cs
localUserC.SetCommunicationRelationship(remoteUserA, GameChat2CommunicationRelationship.ReceiveAll);
localUserC.SetCommunicationRelationship(remoteUserB, GameChat2CommunicationRelationship.None);
localUserC.SetCommunicationRelationship(remoteUserD, GameChat2CommunicationRelationship.None);
```

사용자가 D 장치의:

```cs
localUserD.SetCommunicationRelationship(remoteUserA, GameChat2CommunicationRelationship.ReceiveAll);
localUserD.SetCommunicationRelationship(remoteUserB, GameChat2CommunicationRelationship.None);
localUserD.SetCommunicationRelationship(remoteUserC, GameChat2CommunicationRelationship.None);
```
