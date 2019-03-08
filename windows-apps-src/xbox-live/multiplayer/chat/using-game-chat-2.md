---
title: 게임 채팅 2를 사용 하 여
description: Xbox Live 게임 채팅 2를 사용 하 여 음성 통신 게임에 추가 하는 방법에 알아봅니다.
ms.date: 03/20/2018
ms.topic: article
keywords: xbox live, xbox, 게임, uwp, windows 10, 하나는 xbox, 2 게임 채팅, 게임 채팅, 음성 통신
ms.localizationpriority: medium
ms.openlocfilehash: 43fbd7cec037df735686aa60bc6cd57217875e33
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57639258"
---
# <a name="using-game-chat-2-c"></a>게임 채팅 2 (c + +)를 사용 하 여

이것이 Game 채팅 2의 c + + API를 사용 하 여 간단한 연습입니다. 게임 게임 채팅 2를 통해 액세스 하려는 개발자에 게 C# 나타납니다 [사용 하 여 게임 채팅 2 WinRT 프로젝션](using-game-chat-2-winrt.md)합니다.

1. [필수 구성 요소](#prerequisites)
2. [초기화](#initialization)
3. [사용자 구성](#configuring-users)
4. [데이터 프레임을 처리합니다.](#processing-data-frames)
5. [상태 변경 내용 처리](#processing-state-changes)
6. [텍스트 채팅](#text-chat)
7. [접근성](#accessibility)
8. [UI](#ui)
9. [음소거](#muting)
10. [잘못 된 평판 자동 음소거](#bad-reputation-auto-mute)
11. [권한 및 개인 정보](#privilege-and-privacy)
12. [정리](#cleanup)
13. [모델 오류](#failure-model)
14. [인기 있는 시나리오를 구성 하는 방법](#how-to-configure-popular-scenarios)

## <a name="prerequisites"></a>필수 구성 요소

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

게임 채팅 2 컴파일 기본 GameChat2.h 헤더를 포함 해야 합니다. 을 올바르게 연결 하려면 프로젝트 (공용 미리 컴파일된 헤더는 작고 "인라인"으로 생성 하도록 컴파일러에 쉽게 이러한 스텁 함수 구현 되므로 권장 됨) 컴파일 단위를 하나 이상에서 GameChat2Impl.h도 포함 해야 합니다.

게임 채팅 2 인터페이스 프로젝트를 C +를 사용 하 여 컴파일 중에서 선택할 필요가 없습니다 + 기존 c + + 및 CX 사용 하 여 사용할 수 있습니다. 또한 구현 치명적이 지 않은 오류 보고를 사용할 수 있습니다 쉽게 예외 없는 프로젝트에서 원하는 경우 있도록 수단으로 예외를 throw 하지 않습니다. 하지만 구현, 심각한 오류를 보고 하는 수단으로 예외를 throw (참조 [실패 모델](#failure) 자세한).

## <a name="initialization"></a>초기화

먼저, singleton의 초기화 기간에 적용 되는 매개 변수를 사용 하 여 게임 채팅 2 singleton 인스턴스를 초기화 하 여 라이브러리와 상호 작용 합니다.

```cpp
chat_manager::singleton_instance().initialize(...);
```

## <a name="configuring-users"></a>사용자 구성

인스턴스가 초기화 되 면 게임 채팅 2 인스턴스에 로컬 사용자를 추가 해야 합니다. 이 예제에서는 사용자를 A는 로컬 사용자를 나타냅니다.

```cpp
chat_user* chatUserA = chat_manager::singleton_instance().add_local_user(<user_a_xuid>);
```

원격 사용자 및 원격 "끝점"을 나타내는 데 사용할 식별자를 추가 해야 사용자입니다. "끝점"이는 원격 장치에서 실행 되는 앱의 인스턴스입니다. 이 예제에서는 사용자 B는 끝점 X, 사용자 C와 D에 끝점 Y. 끝점 X는 "1" 식별자를 임의로 할당 하 고 끝점 Y 식별자 "2"에 임의로 할당 됩니다. 이러한 호출을 사용 하 여 원격 사용자의 게임 채팅 2에 게 알려주는:

```cpp
chat_user* chatUserB = chat_manager::singleton_instance().add_remote_user(<user_b_xuid>, 1);
chat_user* chatUserC = chat_manager::singleton_instance().add_remote_user(<user_c_xuid>, 2);
chat_user* chatUserD = chat_manager::singleton_instance().add_remote_user(<user_d_xuid>, 2);
```

이제 각 원격 사용자는 로컬 사용자와의 통신 관계를 구성 합니다. 이 예제에서는 같은 팀에 사용자 A와 사용자 B는 및 양방향 통신이 허용 되는 가정 합니다. `c_communicationRelationshipSendAndReceiveAll` 상수 양방향 통신을 나타내는 GameChat2.h에서 정의 됩니다. 사용 하 여 사용자 B를 A의 사용자 관계를 설정 합니다.

```cpp
chatUserA->local()->set_communication_relationship(chatUserB, c_communicationRelationshipSendAndReceiveAll);
```

이제 사용자가 C와 D 'spectators'를 사용할 수 없습니다. 하지만 사용자에 게 수신 되도록 허용 되어야 가정 합니다. `c_communicationRelationshipSendAll` 상수를 나타내는이 단방향 통신 GameChat2.h에서 정의 됩니다. 사용 하 여 관계를 설정 합니다.

```cpp
chatUserA->local()->set_communication_relationship(chatUserC, c_communicationRelationshipSendAll);
chatUserA->local()->set_communication_relationship(chatUserD, c_communicationRelationshipSendAll);
```

언제 든 지 원격 사용자가 단일 인스턴스로 추가 되었지만 모든 로컬 사용자-를 사용 하 여 통신 하도록 구성 되어 있는 경우 좋은 현상입니다. 이 사용자의 팀 결정할 또는 말하기 채널을 임의로 변경할 수 있는 시나리오에서 발생할 수 있습니다. 게임 채팅 2 시간에서 특정 지점에서 로컬 사용자에 게 말할 수 없는 경우에 가능한 모든 사용자의 채팅 2 게임을 알리는 데 유용 하므로 인스턴스에 추가 된 사용자에 대 한 정보 (예: 개인 정보 관계 및 신뢰도)만 캐시 됩니다.

마지막으로, 사용자 D 게임에서 로컬 게임 채팅 2 인스턴스에서 제거할 가정 합니다. 다음 호출을 사용 하 여 수행할 수 있습니다.

```cpp
chat_manager::singleton_instance().remove_user(chatUserD);
```

호출 `chat_manager::remove_user()` 사용자 개체를 무효화 될 수 있습니다. 사용 중인 경우 [실시간 오디오 조작](real-time-audio-manipulation.md)를 참조 하십시오 합니다 [채팅 사용자 수명](real-time-audio-manipulation.md#chat-user-lifetimes) 추가 정보에 대 한 설명서입니다. 사용자 개체를 즉시 무효화 그러지 때 `chat_manager::remove_user()` 라고 합니다. 사용자를 제거할 수 있습니다에 미묘한 제한에 자세히 설명 되어 [상태 변경 내용을 처리](#state)합니다.

## <a name="processing-data-frames"></a>데이터 프레임을 처리합니다.

게임 채팅 2에 전송 계층을 자체 없습니다. 이 앱에서 제공 되어야 합니다. 이 플러그 인 응용 프로그램의 일반, 자주 호출 하 여 관리 되는 `chat_manager::start_processing_data_frames()` 및 `chat_manager::finish_processing_data_frames()` 메서드 쌍입니다. 이러한 메서드는 게임 채팅 2 앱에 보내는 데이터를 제공 하는 방법입니다. 전용 네트워킹 스레드에서 자주 폴링 될 수 있도록 신속 하 게 작동 하도록 설계 되었습니다. 이 네트워크 타이밍 지출 하거나 복잡 한 다중 스레드 콜백의 예측 불가능성을 고려 하는 방법에 대 한 걱정 없이 모든 큐에 대기 중인된 데이터를 검색 하는 편리한 장소를 제공 합니다.

때 `chat_manager::start_processing_data_frames()` 라고, 모든 배열에 큐에 대기 중인된 데이터를 보고 `game_chat_data_frame` 구조체 포인터입니다. 앱은 배열을 반복 하 고 대상 "끝점"을 검사 하 고 앱의 네트워킹 계층을 사용 하 여 해당 원격 앱 인스턴스에 데이터를 전달 해야 합니다. 모두를 사용 하 여 한 번 완료 합니다 `game_chat_data_frame` 구조체 배열에 전달 되어야 다시 호출 하 여 리소스를 해제 하도록 게임 채팅 2 `chat_manager:finish_processing_data_frames()`합니다. 예를 들어 다음과 같은 가치를 제공해야 합니다.

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

데이터 프레임을 처리 하는 더 자주, 작을수록 더 낮은 오디오 대기 시간이 최종 사용자에 게는 표시 됩니다. 오디오는 40 ms 데이터 프레임에 결합 됩니다. 제안 된 폴링 기간입니다.

## <a name="processing-state-changes"></a>상태 변경 내용 처리

게임 채팅 2는 앱의 일반, 자주 호출을 통해 받은 텍스트 메시지와 같은 앱에 대 한 업데이트를 제공 합니다 `chat_manager::start_processing_state_changes()` 및 `chat_manager::finish_processing_state_changes()` 메서드 쌍입니다. UI 렌더링 루프에서 그래픽 프레임 마다 호출할 수 있습니다는 신속 하 게 작동 하도록 설계 되었습니다. 이 네트워크 타이밍 지출 하거나 복잡 한 다중 스레드 콜백의 예측 불가능성을 고려 하는 방법에 대 한 걱정 없이 모든 큐에 대기 중인된 변경 내용을 검색 하는 데 편리한 위치를 제공 합니다.

때 `chat_manager::start_processing_state_changes()` 가 배열을에 모든 큐에 대기 중인된 업데이트는 보고 호출 `game_chat_state_change` 구조체 포인터입니다. 앱의 보다 구체적인 형식에 대 한 기본 구조를 검사, 자세한를 입력 하 고 적절 하 게 업데이트를 처리 합니다. 해당 기본 구조를 캐스팅 배열에 대해 반복 해야 합니다. 모든 완료 되 면 `game_chat_state_change` 개체를 현재 사용할 수 있는, 해당 배열에 전달 되어야 다시 호출 하 여 리소스를 해제 하도록 게임 채팅 2 `chat_manager::finish_processing_state_changes()`합니다. 예를 들어 다음과 같은 가치를 제공해야 합니다.

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

때문에 `chat_manager::remove_user()` 사용자 개체에 연결 된 메모리를 즉시 무효화 상태 변경 내용을 사용자 개체에 대 한 포인터를 포함할 수 있습니다 및 `chat_manager::remove_user()` 상태 변경을 처리 하는 동안 호출 되지 않아야 합니다.

## <a name="text-chat"></a>텍스트 채팅

사용 하 여 텍스트 채팅을 보내려면 `chat_user::chat_user_local::send_chat_text()`합니다. 예를 들어 다음과 같은 가치를 제공해야 합니다.

```cpp
chatUserA->local()->send_chat_text(L"Hello");
```

게임 채팅 2는이 메시지를 포함 하는 데이터 프레임을 생성 합니다. 데이터 프레임에 대 한 대상 끝점에 연결 된 로컬 사용자의 텍스트를 수신 하도록 구성 된 사용자가 됩니다. 메시지를 통해 노출 될 데이터를 원격 끝점에서 처리 되 면을 `game_chat_text_chat_received_state_change`입니다. 음성 채팅와 마찬가지로 권한 및 개인 정보 제한은 텍스트 채팅에 대 한 적용 됩니다. 텍스트 채팅 허용 하도록 구성 된 한 쌍의 사용자 권한 및 개인 정보 제한을 통신을 허용 하지 않습니다 하지만 경우에 문자 메시지 삭제 됩니다.

텍스트 채팅 입력 및 표시를 지 원하는 내게 필요한 옵션에 대 한 필요 (참조 [내게 필요한 옵션](#access) 자세한).

## <a name="accessibility"></a>접근성

입력 텍스트 채팅 및 표시를 지 원하는 필요 합니다. 텍스트 입력 이므로 필요한 플랫폼, 지금까지 사용 하 여 광범위 한 실제 키보드를 보유 하지 않은 게임 장르 에서도 사용자 텍스트 음성 변환 보조 기술을 사용 하 여 시스템 구성할 수 있습니다. 마찬가지로, 텍스트 표시 되므로 필요한 사용자 음성-텍스트를 사용 하도록 시스템을 구성할 수 있습니다. 이러한 기본 설정은 호출 하 여 로컬 사용자에서 검색할 수는 `chat_user::chat_user_local::text_to_speech_conversion_preference_enabled()` 및 `chat_user::chat_user_local::speech_to_text_conversion_preference_enabled()` 메서드 각각 및 조건에 따라 텍스트 메커니즘을 사용 하도록 설정 하려고 할 수 있습니다.

### <a name="text-to-speech"></a>텍스트 음성 변환

사용자에 게 사용 하도록 설정 하는 텍스트 음성 변환 `chat_user::chat_user_local::text_to_speech_conversion_preference_enabled()` 'true'를 반환 합니다. 이 상태가 감지 되 면 앱의 텍스트 입력 메서드를 제공 해야 합니다. 실제 또는 가상 키보드에서 제공 되는 텍스트 입력을 만든 후에 문자열을 전달 합니다 `chat_user::chat_user_local::synthesize_text_to_speech()` 메서드. 게임 채팅 2 감지 하 고 문자열 및 사용자의 액세스 가능한 음성 기본 설정을 기반으로 하는 오디오 데이터를 합성 합니다. 예를 들어 다음과 같은 가치를 제공해야 합니다.

```cpp
chat_userA->local()->synthesize_text_to_speech(L"Hello");
```

이 작업의 일환으로 합성 오디오가 로컬 사용자에서 오디오를 수신 하도록 구성 된 모든 사용자에 게 전송 됩니다. 경우 `chat_user::chat_user_local::synthesize_text_to_speech()` 작업도 발생 하지 않도록 설정 된 게임 채팅 2는 텍스트 음성 변환 되지 않은 사용자에서 호출 됩니다.

### <a name="speech-to-text"></a>음성-텍스트

사용자가 음성-텍스트를 사용 하는 경우 `chat_user::chat_user_local::speech_to_text_conversion_preference_enabled()` true를 반환 합니다. 이 상태가 감지 되 면 기록된 채팅 메시지와 연결 된 UI를 제공 하는 앱 준비 되어야 합니다. 게임 채팅 2 자동으로 각 원격 사용자의 오디오를 기록 하 고를 통해 노출 된 `game_chat_transcribed_chat_received_state_change`합니다.

> `Windows::Xbox::UI::Accessibility` Xbox One 클래스를 하도록 설계 된 음성-텍스트 보조 기술에 중점을 둔 텍스트 게임 내 채팅의 간단한 렌더링을 제공 합니다.

### <a name="speech-to-text-performance-considerations"></a>음성-텍스트 성능 고려 사항

음성-텍스트를 사용 하는 경우 각 원격 장치에서 게임 채팅 2 인스턴스 음성 서비스 끝점을 사용 하 여 웹 소켓 연결을 시작 합니다. 각 원격 게임 채팅 2 클라이언트;이 websocket 통해 음성 서비스 끝점에 오디오를 업로드합니다 경우에 따라 음성 서비스 끝점을 원격 장치에 기록 메시지를 반환합니다. 원격 장치를 기록 메시지를 전송 합니다 (즉, 문자 메시지) 로컬 장치에 있는 기록된 메시지에 지정 됩니다 게임 채팅 2로 앱을 렌더링 하 합니다.

따라서 음성-텍스트의 기본 성능 저하는 네트워크 사용량. 대부분의 네트워크 트래픽이 인코딩된 오디오 업로드입니다. Websocket이 이미 인코딩된 게임 채팅 2로 "normal" 음성 채팅 경로;에 오디오를 업로드합니다 앱에 제어를 통해 비트 전송률 `chat_manager::set_audio_encoding_type_and_bitrate`합니다.

## <a name="ui"></a>UI

어디서 나 플레이어에에서 표시 되어 있는지, 특히는 스코어 같은 게이머 태그의 목록을 사용자에 대 한 피드백으로 말하기 음소거 아이콘도 표시 하는 것이 좋습니다. 호출 하 여 이렇게 `chat_user::chat_indicator()` 검색할는 `game_chat_user_chat_indicator` 는 플레이어에 대 한 채팅의 즉각적인, 현재 상태를 나타내는입니다. 다음 예제에 대 한 표시기 값을 검색 하는 방법을 보여 줍니다는 `chat_user` 'iconToShow' 변수에 값을 할당할 특정 아이콘 상수 값을 확인 하려면 ' chatUserA' 변수가 가리키는 개체:

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

보고 된 값 `xim_player::chat_indicator()` 예를 들어 플레이어 시작 및 중지 통신으로 자주 변경 해야 합니다. 폴링 해당 UI 프레임 마다 결과적으로 앱을 지원 하도록 설계 되었습니다.

## <a name="muting"></a>음소거

`chat_user::chat_user_local::set_microphone_muted()` 로컬 사용자의 마이크 음소거 상태를 전환 하려면 메서드를 사용할 수 있습니다. 마이크 음소거 되 면 해당 마이크에서 오디오 없는 캡처됩니다. 사용자가 공유 장치, Kinect, 예: 음소거 상태를 모든 사용자에 게 적용 됩니다.

`chat_user::chat_user_local::microphone_muted()` 메서드를 사용 하 여 로컬 사용자의 마이크 음소거 상태를 검색할 수 있습니다. 이 메서드 호출을 통해 소프트웨어에서 로컬 사용자의 마이크에 음소거 되어 있는지 여부를 반영 `chat_user::chat_user_local::set_microphone_muted()`;이 메서드는 하드웨어 음소거 제어, 예를 들어 사용자의 헤드셋에서 단추를 통해 반영 되지 않습니다. 게임 채팅 2를 통해 사용자의 오디오 장치 하드웨어 음소거 상태를 검색 하는 방법은 없습니다.

`chat_user::chat_user_local::set_remote_user_muted()` 메서드를 특정 로컬 사용자와 관련 하 여 원격 사용자의 음소거 상태 전환에 사용할 수 있습니다. 원격 사용자 음소거 되어 로컬 사용자 오디오 듣거나 원격 사용자의 모든 텍스트 메시지를 수신 하지 않습니다.

## <a name="bad-reputation-auto-mute"></a>잘못 된 평판 자동 음소거

일반적으로 원격 사용자는 시작 unmuted 합니다. 게임 채팅 2가 (1) 원격 사용자가 로컬 사용자와 친구 및 (2) 원격 사용자가 잘못 된 평판 플래그 면 음소거 상태의 사용자가 시작 됩니다. 사용자가이 작업으로 인해 음소거 되 면 `chat_user::chat_indicator()` 돌아갑니다 `game_chat_user_chat_indicator::reputation_restricted`합니다. 첫 번째 호출에서이 상태를 재정의할 수는 `chat_user::chat_user_local::set_remote_user_muted()` 대상 사용자로 원격 사용자를 포함 하는 합니다.

## <a name="privilege-and-privacy"></a>권한 및 개인 정보

게임에서 구성 하 여 통신 관계를 기반으로 게임 채팅 2 권한 및 개인 정보 제한을 적용 합니다. 게임 채팅 2 권한 및 개인 정보 보호 제한에서 조회를 수행 먼저 사용자를 추가 합니다. 사용자의 `chat_user::chat_indicator()` 는 항상 반환 `game_chat_user_chat_indicator::silent` 해당 작업이 완료 될 때까지 합니다. 사용자와의 통신을 권한 또는 개인 정보 제한을 사용자의 영향을 받는 경우 `chat_user::chat_indicator()` 돌아갑니다 `game_chat_user_chat_indicator::platform_restricted`합니다. 텍스트 및 음성 채팅;에 플랫폼 통신 제한 사항이 적용 됩니다. 인스턴스는 텍스트 채팅 플랫폼 제한에 의해 차단 됩니다 아닌 음성 채팅, 또는 그 반대로 안 됩니다.

`chat_user::chat_user_local::get_effective_communication_relationship()` 불완전 한 권한 및 개인 정보 작업으로 인해 사용자가 통신할 수 없는 경우와 구분 하는 데 사용할 수 있습니다. 형태로 게임 채팅 2에 의해 적용 통신 관계가 반환 `game_chat_communication_relationship_flags` 이유 관계 동일 하지 않을 구성 관계의 형태로 및 `game_chat_communication_relationship_adjuster`합니다. 예를 들어 조회 작업은 계속 진행에서 합니다 `game_chat_communication_relationship_adjuster` 됩니다 `game_chat_communication_relationship_adjuster::intializing`합니다. 이 메서드는 개발 및 디버깅 시나리오에서 사용할 수 해야 UI에 영향을 줄을 사용 해야 (참조 [UI](#UI)).

## <a name="cleanup"></a>정리

앱에서 게임 채팅 2를 통해 통신을 더 이상 필요한 경우 호출 해야 `chat_manager::cleanup()`합니다. 이 게임 채팅 2 통신을 관리 하려면 할당 된 리소스를 회수할 수 있습니다.

## <a name="failure-model"></a>모델 오류

게임 채팅 2 구현 치명적이 지 않은 오류 보고를 사용할 수 있습니다 쉽게 예외 없는 프로젝트에서 원하는 경우 있도록 수단으로 예외를 throw 하지 않습니다. 하지만 게임 채팅 2는 치명적 오류에 게 알리기 위해 예외를 throw는. 이러한 오류는 게임 채팅 인스턴스를 인스턴스를 초기화 하거나 게임 채팅 2 인스턴스에서 제거 된 후 사용자 개체에 액세스 하기 전에 사용자를 추가 하는 등의 API 오용의 결과입니다. 이러한 오류는 개발 초기에 파악할 수를 예상 되 고 게임 채팅 2와 상호 작용 하는 데 사용 되는 패턴을 수정 하 여 수정할 수 있습니다. 이러한 오류가 발생 하면 예외가 발생 하기 전에 오류를 일으킨 원인을 관한 힌트 디버거에 출력 됩니다.

## <a name="how-to-configure-popular-scenarios"></a>인기 있는 시나리오를 구성 하는 방법

### <a name="push-to-talk"></a>푸시-강연

강연에 푸시를 사용 하 여 구현 되어야 합니다 `chat_user::chat_user_local::set_microphone_muted()`합니다. 호출 `set_microphone_muted(false)` 음성 수 있도록 및 `set_microphone_muted(true)` 를 제한 합니다. 이 메서드는 게임 채팅 2에서 가장 낮은 대기 시간 응답을 제공 합니다.

### <a name="teams"></a>팀

블루 팀에서 사용자 A와 사용자 B는 있고 사용자 C와 D 사용자 팀 Red에 있다고 가정 합니다. 각 사용자가 앱의 고유 인스턴스입니다.

사용자 A의 장치:

```cpp
chatUserA->local()->set_communication_relationship(chatUserB, c_communicationRelationshipSendAndReceiveAll);
chatUserA->local()->set_communication_relationship(chatUserC, game_chat_communication_relationship_flags::none);
chatUserA->local()->set_communication_relationship(chatUserD, game_chat_communication_relationship_flags::none);
```

사용자 B의 장치:

```cpp
chatUserB->local()->set_communication_relationship(chatUserA, c_communicationRelationshipSendAndReceiveAll);
chatUserB->local()->set_communication_relationship(chatUserC, game_chat_communication_relationship_flags::none);
chatUserB->local()->set_communication_relationship(chatUserD, game_chat_communication_relationship_flags::none);
```

사용자 C 장치의 경우:

```cpp
chatUserC->local()->set_communication_relationship(chatUserA, game_chat_communication_relationship_flags::none);
chatUserC->local()->set_communication_relationship(chatUserB, game_chat_communication_relationship_flags::none);
chatUserC->local()->set_communication_relationship(chatUserD, c_communicationRelationshipSendAndReceiveAll);
```

사용자 D 장치의 경우:

```cpp
chatUserD->local()->set_communication_relationship(chatUserA, game_chat_communication_relationship_flags::none);
chatUserD->local()->set_communication_relationship(chatUserB, game_chat_communication_relationship_flags::none);
chatUserD->local()->set_communication_relationship(chatUserC, c_communicationRelationshipSendAndReceiveAll);
```

### <a name="broadcast"></a>방송

사용자는 주문을 제공 리더가 고 사용자 B, C 및 D만 수신할 수 있는지를 가정 합니다. 각 플레이어는 고유한 장치 켜져 있습니다.

사용자 A의 장치:

```cpp
chatUserA->local()->set_communication_relationship(chatUserB, c_communicationRelationshipSendAll);
chatUserA->local()->set_communication_relationship(chatUserC, c_communicationRelationshipSendAll);
chatUserA->local()->set_communication_relationship(chatUserD, c_communicationRelationshipSendAll);
```

사용자 B의 장치:

```cpp
chatUserB->local()->set_communication_relationship(chatUserA, c_communicationRelationshipReceiveAll);
chatUserB->local()->set_communication_relationship(chatUserC, game_chat_communication_relationship_flags::none);
chatUserB->local()->set_communication_relationship(chatUserD, game_chat_communication_relationship_flags::none);
```

사용자 C 장치의 경우:

```cpp
chatUserC->local()->set_communication_relationship(chatUserA, c_communicationRelationshipReceiveAll);
chatUserC->local()->set_communication_relationship(chatUserB, game_chat_communication_relationship_flags::none);
chatUserC->local()->set_communication_relationship(chatUserD, game_chat_communication_relationship_flags::none);
```

사용자 D 장치의 경우:

```cpp
chatUserD->local()->set_communication_relationship(chatUserA, c_communicationRelationshipReceiveAll);
chatUserD->local()->set_communication_relationship(chatUserB, game_chat_communication_relationship_flags::none);
chatUserD->local()->set_communication_relationship(chatUserC, game_chat_communication_relationship_flags::none);
```
