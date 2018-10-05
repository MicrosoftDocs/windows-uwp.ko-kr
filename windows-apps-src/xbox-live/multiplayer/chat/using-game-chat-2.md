---
title: 게임 채팅 2를 사용 하 여
author: KevinAsgari
description: Xbox Live 게임 채팅 2를 사용 하 여 음성 통신을 게임에 추가 하는 방법을 알아봅니다.
ms.author: kevinasg
ms.date: 3/20/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: xbox live, xbox, 게임, uwp, windows 10, 하나는 xbox, 게임 채팅 2, 게임 채팅, 음성 통신
ms.localizationpriority: medium
ms.openlocfilehash: 15e0abfa001910621e954f4859bb4a94159c31ec
ms.sourcegitcommit: 5c9a47b135c5f587214675e39c1ac058c0380f4c
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/04/2018
ms.locfileid: "4358055"
---
# <a name="using-game-chat-2-c"></a>게임 채팅 2 (c + +)를 사용 하 여

게임 채팅 2의 c + + API 사용에 대 한 간략 한 연습입니다. C#을 통해 게임 채팅 2에 액세스 하려는 게임 개발자가 [사용 하 여 게임 채팅 2 2(winrt 프로젝션](using-game-chat-2-winrt.md)보일 것입니다.

1. [필수 구성 요소](#prerequisites)
2. [초기화](#initialization)
3. [사용자 구성](#configuring-users)
4. [데이터 프레임 처리](#processing-data-frames)
5. [상태 변경 처리](#processing-state-changes)
6. [텍스트 채팅](#text-chat)
7. [접근성](#accessibility)
8. [UI](#ui)
9. [음소거](#muting)
10. [나쁜 평판 자동 음소거](#bad-reputation-auto-mute)
11. [권한 및 개인 정보 보호](#privilege-and-privacy)
12. [정리](#cleanup)
13. [오류 모델](#failure-model)
14. [인기 있는 시나리오를 구성 하는 방법](#how-to-configure-popular-scenarios)

## <a name="prerequisites"></a>필수 구성 요소

게임 채팅 2를 사용 하 여 코딩을 시작 하기 전에 "마이크" 장치 접근 권한 값을 선언 하는 앱의 AppXManifest 구성 해야 합니다. AppXManifest 기능 플랫폼 설명서;의 각 섹션에서 자세히 설명 합니다. 다음 코드 조각은 패키지/기능 노드 아래에 있어야 하는 "마이크" 장치 접근 권한 값은 노드를 보여 줍니다. 그렇지 않으면 채팅 차단 됩니다.

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

Game Chat 2 컴파일 기본 GameChat2.h 헤더를 포함 해야 합니다. 제대로 연결 하기 위해 프로젝트 (일반적인 미리 컴파일된 헤더 컴파일러에서 "inline"으로 생성에 대 한 작은 쉽고 이러한 스텁 함수 구현 때문에 권장 됨) 하나 이상 컴파일 단위에서 GameChat2Impl.h도 포함 해야 합니다.

게임 채팅 2 인터페이스 C + 컴파일 중에서 선택 하는 프로젝트를 컴파일할 필요가 + CX 기존 c + +; 비교 와 함께 사용할 수 있습니다. 또한 구현 사소한 오류 수 사용 하기 쉽게 예외 무료 프로젝트에서 원하는 경우 하므로 보고 하는 수단으로 예외를 throw 하지 않습니다. 그러나 구현, 치명적인 오류 보고 (세부 정보에 대 한 [오류 모델](#failure) 참조) 하는 수단으로 예외를 throw 합니다.

## <a name="initialization"></a>초기화

Singleton은 초기화의 수명에 적용 되는 매개 변수를 사용 하 여 게임 채팅 2 singleton 인스턴스를 초기화 하 여 라이브러리와 상호 작용을 시작 합니다.

```cpp
chat_manager::singleton_instance().initialize(...);
```

## <a name="configuring-users"></a>사용자 구성

인스턴스를 초기화 되 면 게임 채팅 2 인스턴스를 로컬 사용자를 추가 해야 합니다. 이 예제에서는 A 사용자를 로컬 사용자를 나타냅니다.

```cpp
chat_user* chatUserA = chat_manager::singleton_instance().add_local_user(<user_a_xuid>);
```

원격 사용자와 식별자는 원격 "끝점"을 나타내는 데 사용할 추가 해야 하는 사용자가 있습니다. "끝점"는 원격 장치에서 실행 되는 앱의 인스턴스입니다. 이 예제에서는 사용자 B 끝점 x는, 사용자가 C 및 D 끝점, 끝점 X 축은 식별자 "1" 임의로 할당 되며 끝점 Y "2" 식별자를 임의로 할당 됩니다. 이러한 호출을 사용 하 여 원격 사용자의 게임 채팅 2 알려줍니다.

```cpp
chat_user* chatUserB = chat_manager::singleton_instance().add_remote_user(<user_b_xuid>, 1);
chat_user* chatUserC = chat_manager::singleton_instance().add_remote_user(<user_c_xuid>, 2);
chat_user* chatUserD = chat_manager::singleton_instance().add_remote_user(<user_d_xuid>, 2);
```

이제 각 원격 사용자와 로컬 사용자 간의 통신 관계를 구성 합니다. 이 예제에서는 사용자 A와 B 사용자가 같은 팀에 및 양방향 통신을 허용 되는 가정 합니다. `c_communicationRelationshipSendAndReceiveAll` 상수를 양방향 통신을 나타내는 GameChat2.h에서 정의 됩니다. 사용자 B를 사용자 A의 관계를 설정 합니다.

```cpp
chatUserA->local()->set_communication_relationship(chatUserB, c_communicationRelationshipSendAndReceiveAll);
```

이제 사용자가 C 및 D 'spectators'를 사용할 수 없습니다. 하지만 사용자 A 수신 하도록 할지 가정 합니다. `c_communicationRelationshipSendAll` 상수를 나타내는 단방향이 통신 GameChat2.h에서 정의 됩니다. 와 관계를 설정 합니다.

```cpp
chatUserA->local()->set_communication_relationship(chatUserC, c_communicationRelationshipSendAll);
chatUserA->local()->set_communication_relationship(chatUserD, c_communicationRelationshipSendAll);
```

언제 든 지 원격 사용자가 단일 인스턴스에 추가 된 하지만 모든 로컬 사용자-와 통신 하도록 구성 하지 않은 경우 있으니까요! 사용자가 팀을 결정 하는 나 임의로 말하는 채널을 변경할 수는 시나리오에서 예상할 수도 있습니다. 게임 채팅 2 시간에 특정 지점에서 모든 로컬 사용자가 말할 수 없는 경우에 Game Chat 2 가능한 모든 사용자에 게 알리는 것이 유용 하므로 인스턴스를에 추가 된 사용자에 대 한 정보 (예: 개인 정보 보호 관계 및 신뢰도)만 캐시가 합니다.

마지막으로, 사용자가 D는 게임 및 게임 채팅 2 인스턴스를 로컬에서 제거 해야 가정 합니다. 다음을 호출 하는 수행 될 수 있습니다.

```cpp
chat_manager::singleton_instance().remove_user(chatUserD);
```

호출 `chat_manager::remove_user()` 사용자 개체를 무효화 될 수 있습니다. [실시간 오디오 조작](real-time-audio-manipulation.md)를 사용 하는 경우 자세한 내용은 [채팅 사용자 수명](real-time-audio-manipulation.md#chat-user-lifetimes) 설명서를 참조 하십시오. 사용자 개체를 즉시 무효화 그렇지 않으면 때 `chat_manager::remove_user()` 라고 합니다. 미묘한 제한 사용자를 제거할 수 있습니다는 [상태 변경 처리](#state)에 자세히 설명 합니다.

## <a name="processing-data-frames"></a>데이터 프레임 처리

게임 채팅 2는 고유한 전송 계층을; 없습니다. 이 앱에서 제공 되어야 합니다. 이 플러그 인 앱의 일반, 자주 호출을 통해 관리 되는 `chat_manager::start_processing_data_frames()` 및 `chat_manager::finish_processing_data_frames()` 메서드 쌍. 이러한 메서드는 게임 채팅 2 앱에 보내는 데이터를 제공 하는 방법. 이러한 전용 네트워킹 스레드에서 자주 폴링할 수 있도록 신속 하 게 작동 하도록 설계 되었습니다. 이 네트워크 타이밍 또는 다중 스레드 콜백 복잡성 예측 불가능성 상관 없이 대기 중인된 모든 데이터를 검색할 수 있는 편리한 위치를 제공 합니다.

때 `chat_manager::start_processing_data_frames()` 라고, 모든 배열에서 대기 중인된 데이터를 보고 하는 `game_chat_data_frame` 포인터를 구성 합니다. 앱 배열에 대해 반복 하 고 대상 "끝점"를 검사 하 고 앱의 네트워킹 계층을 사용 하 여 데이터를 적절 한 원격 앱 인스턴스를 제공 해야 합니다. 한 번 모두 완료 합니다 `game_chat_data_frame` 구조를 다시 호출 하 여 리소스를 해제 하 여 게임 채팅 2에 배열 전달 해야 `chat_manager:finish_processing_data_frames()`. 예를 들면 다음과 같습니다.

```cpp
uint32_t dataFrameCount;
game_chat_data_frame_array dataFrames;
chat_manager::singleton_instance().start_processing_data_frames(&dataFrameCount, &dataFrames);
for (uint32_t dataFrameIndex = 0; dataFrameIndex < dataFrameCount; ++dataFrameIndex)
{
    game_chat_data_frame const* dataFrame = dataFrames[dataFrameIndex];
    HandleOutgoingDataFrame(
        dataFrame->packet_byte_count,
        dataFrame->packet_buffer,
        dataFrame->target_endpoint_identifier_count,
        dataFrame->target_endpoint_identifiers,
        dataFrame->transport_requirement
        );
}
chat_manager::singleton_instance().finish_processing_data_frames(dataFrames);
```

데이터 프레임 처리 되는 더 자주, 낮을수록 오디오 대기 시간 최종 사용자에 게 표시 됩니다. 오디오는 40 ms 데이터 프레임;으로 결합 됩니다. 제안 된 폴링 기간입니다.

## <a name="processing-state-changes"></a>상태 변경 처리

앱의 일반, 자주 호출을 통해 받은 텍스트 메시지 등의 앱에 대 한 업데이트를 제공 하는 게임 채팅 2는 `chat_manager::start_processing_state_changes()` 및 `chat_manager::finish_processing_state_changes()` 메서드 쌍. UI 렌더링 루프에서 매 프레임 마다 그래픽 호출할 수 있습니다 되도록 신속 하 게 작동 하도록 설계 되었습니다. 이 네트워크 타이밍 또는 다중 스레드 콜백 복잡성 예측 불가능성 상관 없이 대기 중인된 모든 변경 내용을 검색 하는 편리한 위치를 제공 합니다.

때 `chat_manager::start_processing_state_changes()` 는 호출 대기 중인된 모든 업데이트의 배열에 보고 `game_chat_state_change` 포인터를 구성 합니다. 앱 배열 반복 해야, 더욱 구체적인 해당 형식에 대 한 기본 구조를 검사, 기본 구조를 입력 하 고 적절 하 게 해당 업데이트를 처리 합니다. 자세한 내용은 해당 요소에 캐스팅 합니다. 모든에서 한 번 완료 `game_chat_state_change` 현재 사용할 수 있는 개체를 다시 호출 하 여 리소스를 해제 하 여 게임 채팅 2에 해당 배열 전달 해야 `chat_manager::finish_processing_state_changes()`. 예를 들면 다음과 같습니다.

```cpp
uint32_t stateChangeCount;
game_chat_state_change_array gameChatStateChanges;
chat_manager::singleton_instance().start_processing_state_changes(&stateChangeCount, &gameChatStateChanges);

for (uint32_t stateChangeIndex = 0; stateChangeIndex < stateChangeCount; ++stateChangeIndex)
{
    switch (gameChatStateChanges[stateChangeIndex]->state_change_type)
    {
        case game_chat_state_change_type::text_chat_received:
        {
            HandleTextChatReceived(static_cast<const game_chat_text_chat_received_state_change*>(gameChatStateChanges[stateChangeIndex]));
            break;
        }

        case Xs::game_chat_2::game_chat_state_change_type::transcribed_chat_received:
        {
            HandleTranscribedChatReceived(static_cast<const Xs::game_chat_2::game_chat_transcribed_chat_received_state_change*>(gameChatStateChanges[stateChangeIndex]));
            break;
        }

        ...
    }
}
chat_manager::singleton_instance().finish_processing_state_changes(gameChatStateChanges);
```

때문에 `chat_manager::remove_user()` 는 사용자 개체와 관련 된 메모리를 무효화 하는 즉시 상태 변경 사용자 개체에 대 한 포인터를 포함할 수 있습니다 `chat_manager::remove_user()` 상태 변경을 처리 하는 동안 호출할 수 없습니다.

## <a name="text-chat"></a>텍스트 채팅

사용 하 여 텍스트 채팅을 보내려면 `chat_user::chat_user_local::send_chat_text()`. 예를 들면 다음과 같습니다.

```cpp
chatUserA->local()->send_chat_text(L"Hello");
```

게임 채팅 2에서이 메시지를 포함 하는 데이터 프레임을 생성 데이터 프레임에 대 한 대상 끝점 사용자가 로컬에서 텍스트를 수신 하도록 구성 된 사용자와 연결 됩니다. 원격 끝점으로 데이터를 처리할 때 메시지를 통해 노출 되는 `game_chat_text_chat_received_state_change`. 음성 채팅와 마찬가지로 텍스트 채팅에 대 한 권한 및 개인 정보 보호 제한은 유지 됩니다. 한 쌍의 사용자가 텍스트 채팅을 허용 하도록 구성 된 경우, 권한 또는 개인 정보 보호 제한 통신을 허용 하지 않습니다 문자 메시지 삭제 됩니다.

입력 텍스트 채팅 및 디스플레이 지 원하는 접근성 필요 ( [접근성](#access) 에 대 한 자세한 내용은 참조).

## <a name="accessibility"></a>접근성

입력 텍스트 채팅 및 디스플레이 지원 하는 것이 필요 합니다. 텍스트 입력을 플랫폼 또는 지금까지 적이 광범위 한 실제 키보드를 사용 하는 게임 장르에도 사용자가 텍스트 음성 변환 보조 기술을 사용 하 여 시스템 구성할 수 있기 때문에 필요 합니다. 마찬가지로, 텍스트 표시는 사용자가 음성-텍스트를 사용 하 여 시스템을 구성할 수 있으므로 필요 합니다. 이러한 기본 설정 호출 하 여 로컬 사용자에 게 감지할 수는 `chat_user::chat_user_local::text_to_speech_conversion_preference_enabled()` 및 `chat_user::chat_user_local::speech_to_text_conversion_preference_enabled()` 메서드 각각 및 조건부로 텍스트 메커니즘을 사용 하고자 할 수 있습니다.

### <a name="text-to-speech"></a>텍스트 음성 변환

사용자가 사용 하도록 설정 하는 텍스트 음성 변환 하는 경우 `chat_user::chat_user_local::text_to_speech_conversion_preference_enabled()` 'true' 반환 됩니다. 이 상태 감지 되 면 앱의 텍스트 입력 메서드를 제공 해야 합니다. 실제 또는 가상 키보드에서 제공 하는 텍스트 입력을 구성한 후에 문자열을 전달 합니다 `chat_user::chat_user_local::synthesize_text_to_speech()` 메서드. 게임 채팅 2를 감지 하 고 문자열 및 사용자의 음성 액세스할 수 있는 기본 설정에 따라 오디오 데이터를 합성 하겠습니다. 예를 들면 다음과 같습니다.

```cpp
chat_userA->local()->synthesize_text_to_speech(L"Hello");
```

이 작업의 일부로 합성 오디오가 로컬 사용자의 오디오를 수신 하도록 구성 된 모든 사용자에 게 전송 됩니다. 하는 경우 `chat_user::chat_user_local::synthesize_text_to_speech()` 사용자가 아무 작업도 활성화 된 텍스트 음성 변환 Game Chat 2가 없는 스레드에서 호출 됩니다.

### <a name="speech-to-text"></a>음성-텍스트

사용자가 음성-텍스트를 사용 하는 경우 `chat_user::chat_user_local::speech_to_text_conversion_preference_enabled()` 가 true를 반환 합니다. 이 상태 감지 되 면 변환 된 채팅 메시지와 관련 된 UI를 제공 하기 위해 앱을 준비 합니다. 게임 채팅 2는 자동으로 각 원격 사용자의 오디오 번역기 변환 하 고 통해 노출는 `game_chat_transcribed_chat_received_state_change`.

> `Windows::Xbox::UI::Accessibility` Xbox One 클래스는 하도록 설계 된 음성-텍스트 보조 기술에 초점을 사용 하 여 게임에서 텍스트 채팅의 간단한 렌더링을 제공 합니다.

### <a name="speech-to-text-performance-considerations"></a>음성-텍스트 성능 고려 사항

음성-텍스트를 사용 하면 각 원격 장치에서 게임 채팅 2 인스턴스 음성 서비스 끝점을 사용 하 여 웹 소켓 연결을 시작 합니다. 이 websocket;를 통해 음성 서비스 끝점을 오디오를 업로드 하는 각 원격 Game Chat 2 클라이언트 경우에 따라 음성 서비스 끝점 원격 장치에 기록과 메시지를 반환합니다. 원격 디바이스 다음 메시지를 보냅니다 기록과 (즉, 텍스트 메시지) 로컬 장치에 있는 transcribed 메시지에는 제공 Game Chat 2 하 여 앱에 렌더링 합니다.

따라서 음성-텍스트의 주요 성능 비용 네트워크 사용 됩니다. 대부분의 네트워크 트래픽은 인코딩된 오디오 업로드입니다. Websocket이 이미 인코딩된 게임 채팅 2 "일반" 음성 채팅 경로;에서 오디오를 업로드 합니다. 앱이를 통해 비트 전송률을 통해 제어 `chat_manager::set_audio_encoding_type_and_bitrate`.

## <a name="ui"></a>UI

아무 곳 이나 플레이어에에서 표시 되어 있는지, 특히 게이머 점수 판, 같은 태그 목록을 사용자에 대 한 피드백으로 아이콘 음소거/말하기를 표시할 수도 것이 좋습니다. 이렇게 호출 하 여 `chat_user::chat_indicator()` 검색 하는 `game_chat_user_chat_indicator` 해당 플레이어에 대 한 채팅 순간, 현재 상태를 나타내는 합니다. 다음 예제에서는 표시기에 대 한 값을 검색 하는 `chat_user` 변수 'chatUserA' 확인 'iconToShow' 변수에 할당 하는 특정 아이콘 상수 값을 가리키는 개체:

```cpp
switch (chatUserA->chat_indicator())
{
   case game_chat_user_chat_indicator::silent:
   {
       iconToShow = Icon_InactiveSpeaker;
       break;
   }

   case game_chat_user_chat_indicator::talking:
   {
       iconToShow = Icon_ActiveSpeaker;
       break;
   }

   case game_chat_user_chat_indicator::local_microphone_muted:
   {
       iconToShow = Icon_MutedSpeaker;
       break;
   }
   ...
}
```

값에서 보고 `xim_player::chat_indicator()` 예를 들어에 플레이어 시작 및 중지 말하기으로 자주 변경 해야 합니다. 폴링 것 UI 프레임 마다 결과적으로 앱을 지원 하도록 설계 되었습니다.

## <a name="muting"></a>음소거

`chat_user::chat_user_local::set_microphone_muted()` 메서드는 로컬 사용자의 마이크의 음소거 상태를 전환 하 여 사용할 수 있습니다. 마이크가 음소거 되어 해당 마이크에서 오디오 없음 캡처됩니다. 사용자가 Kinect 등의 공유 장치에 있는 경우 음소거 상태 모든 사용자에 게 적용 됩니다.

`chat_user::chat_user_local::microphone_muted()` 메서드는 로컬 사용자의 마이크의 음소거 상태를 검색 데 사용할 수 있습니다. 이 메서드는 로컬 사용자의 마이크에 대 한 호출을 통해 소프트웨어에서 음소거 되었습니다가 있는지 여부를 반영 `chat_user::chat_user_local::set_microphone_muted()`. 이 메서드는 하드웨어 음소거 제어, 예를 들어, 사용자의 헤드셋에서 단추를 통해 반영 하지 않습니다. 게임 채팅 2를 통해 사용자의 오디오 장치의 하드웨어 음소거 상태를 검색할 수 있는 방법은 없습니다.

`chat_user::chat_user_local::set_remote_user_muted()` 메서드는 특정 로컬 사용자와 관련 하 여 원격 사용자의 음소거 상태를 전환 하 여 사용할 수 있습니다. 원격 사용자 음이 소거 로컬 사용자 모든 오디오를 들 되지 않거나 원격 사용자 로부터 텍스트 메시지를 수신 합니다.

## <a name="bad-reputation-auto-mute"></a>나쁜 평판 자동 음소거

일반적으로 원격 사용자가 시작 unmuted 합니다. 게임 채팅 2 (1) 원격 사용자가 로컬 사용자를 사용 하 여 친구 및 (2) 원격 사용자가 나쁜 평판 플래그 때 음소거 상태에서 사용자가 시작 됩니다. 이 작업으로 인해 사용자는 음소거 하는 경우 `chat_user::chat_indicator()` 는 반환 `game_chat_user_chat_indicator::reputation_restricted`. 이 상태를 처음 호출 하면 덮어쓸 수 있습니다 `chat_user::chat_user_local::set_remote_user_muted()` 대상 사용자로 원격 사용자를 포함 합니다.

## <a name="privilege-and-privacy"></a>권한 및 개인 정보 보호

게임에서 구성 된 통신 관계를 기반으로 게임 채팅 2 권한 및 개인 정보 보호 제한을 적용 합니다. 게임 채팅 2 권한 및 개인 정보 보호 제한 조회 수행 하는 사용자가 처음 추가 합니다. 사용자의 `chat_user::chat_indicator()` 는 항상 반환 `game_chat_user_chat_indicator::silent` 해당 작업이 완료 될 때까지 합니다. 사용자와의 통신 제한에 의해는 권한 또는 개인 정보, 사용자의 영향을 받는 경우 `chat_user::chat_indicator()` 는 반환 `game_chat_user_chat_indicator::platform_restricted`. 음성 및 텍스트를 모두 채팅; 플랫폼 통신 제한이 적용 여기서 텍스트 채팅 플랫폼 제한에 의해 차단 하지 않으면이 음성 채팅, 또는 그 반대로 인스턴스 안 됩니다.

`chat_user::chat_user_local::get_effective_communication_relationship()` 불완전 한 권한 및 개인 정보 보호 작업으로 인해 사용자가 통신할 수 없는 경우 구별 하는 데 사용할 수 있습니다. 통신 관계의 형태로 Game Chat 2에 의해 적용 반환 `game_chat_communication_relationship_flags` 이유 관계 같지 않을 구성 된 관계의 형태로 및 `game_chat_communication_relationship_adjuster`. 예를 들어, 조회 작업 진행 되는 `game_chat_communication_relationship_adjuster` 는 `game_chat_communication_relationship_adjuster::intializing`. 이 메서드는 개발 및 디버깅 시나리오; 사용할 것으로 예상 UI에 영향을 사용 해야 ( [UI](#UI)참조).

## <a name="cleanup"></a>정리

호출 해야 앱에서 더 이상 Game Chat 2를 통해 통신을 필요로 경우 `chat_manager::cleanup()`. 이 통해 통신을 관리 하려면 할당 된 리소스를 확보 하기 위해 Game Chat 2.

## <a name="failure-model"></a>오류 모델

게임 채팅 2 구현 사소한 오류 수 사용 하기 쉽게 예외 무료 프로젝트에서 원하는 경우 하므로 보고 하는 수단으로 예외를 throw 하지 않습니다. 그러나 게임 채팅 2, 심각한 오류를 알리는 예외를 throw 하 고 있습니다. 이러한 오류는 인스턴스를 초기화 하거나 Game Chat 2 인스턴스에서 제거 된 후 사용자 개체에 액세스 하기 전에 사용자 게임 채팅 인스턴스를 추가 하는 등의 API 오용의 결과입니다. 이러한 오류 개발 초기에 발견 될 것으로 예상 되 고 Game Chat 2와 상호 작용 하는 데 사용 패턴을 수정 하 여 해결할 수 있습니다. 이러한 오류가 발생 하는 경우 예외가 발생 하기 전에 오류 원인을 하는지에 대 한 힌트 디버거에 인쇄 됩니다.

## <a name="how-to-configure-popular-scenarios"></a>인기 있는 시나리오를 구성 하는 방법

### <a name="push-to-talk"></a>푸시-통화

푸시-, 구현 되어야 합니다 `chat_user::chat_user_local::set_microphone_muted()`. 호출 `set_microphone_muted(false)` 음성 수 있도록 하 고 `set_microphone_muted(true)` 으로만 제한 합니다. 이 메서드는 게임 채팅 2의 가장 낮은 대기 시간 응답을 제공 합니다.

### <a name="teams"></a>Teams

사용자 A와 B 사용자가 파란색 팀에 사용자 C 및 D 사용자가 팀 빨간색에 한다고 가정 합니다. 각 사용자는 앱의 고유한 인스턴스.

사용자 A의 장치:

```cpp
chatUserA->local()->set_communication_relationship(chatUserB, c_communicationRelationshipSendAndReceiveAll);
chatUserA->local()->set_communication_relationship(chatUserC, game_chat_communication_relationship_flags::none);
chatUserA->local()->set_communication_relationship(chatUserD, game_chat_communication_relationship_flags::none);
```

사용자 B 장치에서:

```cpp
chatUserB->local()->set_communication_relationship(chatUserA, c_communicationRelationshipSendAndReceiveAll);
chatUserB->local()->set_communication_relationship(chatUserC, game_chat_communication_relationship_flags::none);
chatUserB->local()->set_communication_relationship(chatUserD, game_chat_communication_relationship_flags::none);
```

사용자가 C 장치의:

```cpp
chatUserC->local()->set_communication_relationship(chatUserA, game_chat_communication_relationship_flags::none);
chatUserC->local()->set_communication_relationship(chatUserB, game_chat_communication_relationship_flags::none);
chatUserC->local()->set_communication_relationship(chatUserD, c_communicationRelationshipSendAndReceiveAll);
```

사용자가 D 장치의:

```cpp
chatUserD->local()->set_communication_relationship(chatUserA, game_chat_communication_relationship_flags::none);
chatUserD->local()->set_communication_relationship(chatUserB, game_chat_communication_relationship_flags::none);
chatUserD->local()->set_communication_relationship(chatUserC, c_communicationRelationshipSendAndReceiveAll);
```

### <a name="broadcast"></a>브로드캐스트

사용자 A가 주문 제공 리더 사용자 B, C 및 D 수만 수신 대기 한다고 가정 합니다. 각 플레이어는 고유의 장치에 있습니다.

사용자 A의 장치:

```cpp
chatUserA->local()->set_communication_relationship(chatUserB, c_communicationRelationshipSendAll);
chatUserA->local()->set_communication_relationship(chatUserC, c_communicationRelationshipSendAll);
chatUserA->local()->set_communication_relationship(chatUserD, c_communicationRelationshipSendAll);
```

사용자 B 장치에서:

```cpp
chatUserB->local()->set_communication_relationship(chatUserA, c_communicationRelationshipReceiveAll);
chatUserB->local()->set_communication_relationship(chatUserC, game_chat_communication_relationship_flags::none);
chatUserB->local()->set_communication_relationship(chatUserD, game_chat_communication_relationship_flags::none);
```

사용자가 C 장치의:

```cpp
chatUserC->local()->set_communication_relationship(chatUserA, c_communicationRelationshipReceiveAll);
chatUserC->local()->set_communication_relationship(chatUserB, game_chat_communication_relationship_flags::none);
chatUserC->local()->set_communication_relationship(chatUserD, game_chat_communication_relationship_flags::none);
```

사용자가 D 장치의:

```cpp
chatUserD->local()->set_communication_relationship(chatUserA, c_communicationRelationshipReceiveAll);
chatUserD->local()->set_communication_relationship(chatUserB, game_chat_communication_relationship_flags::none);
chatUserD->local()->set_communication_relationship(chatUserC, game_chat_communication_relationship_flags::none);
```
