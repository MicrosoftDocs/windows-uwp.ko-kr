---
title: 게임 채팅 2 마이그레이션
author: KevinAsgari
description: 게임 채팅 2를 사용 하 여 게임 채팅 기존 코드를 마이그레이션하는 방법을 알아봅니다.
ms.author: kevinasg
ms.date: 5/2/2018
ms.topic: article
keywords: xbox live, xbox, 게임, uwp, windows 10, 하나는 xbox, 게임 채팅 2, 게임 채팅, 음성 통신
ms.localizationpriority: medium
ms.openlocfilehash: 467e4ca69550e6cebdd0b20711dd0e7a6abd305d
ms.sourcegitcommit: 70ab58b88d248de2332096b20dbd6a4643d137a4
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/02/2018
ms.locfileid: "5924410"
---
# <a name="migration-from-game-chat-to-game-chat-2"></a>게임 채팅 2 게임 채팅에서 마이그레이션

이 문서에는 게임 채팅 및 게임 채팅 2와 게임 채팅에서 게임 채팅 2로 마이그레이션하는 방법 간의 유사성 자세히 설명 합니다. 따라서 게임 채팅 2로 마이그레이션할 하고자 하는 기존 게임 채팅 구현 된 제목입니다. 게임 채팅 구현, 아직 없는 경우 제안 된 시작점 [Game Chat 2를 사용 하 여](using-game-chat-2.md)됩니다. 이 문서에는 다음 항목을 포함합니다.

1. [앞](#preface)
2. [사전 요구 사항](#prerequisites)
3. [초기화](#initialization)
4. [사용자 구성](#configuring-users)
5. [데이터 처리](#processing-data)
6. [이벤트 처리](#processing-events)
7. [텍스트 채팅](#text-chat)
8. [접근성](#accessibility)
9. [UI](#UI)
10. [음소거](#muting)
11. [나쁜 평판 자동 음소거](#bad-reputation-auto-mute)
12. [권한 및 개인 정보 보호](#privilege-and-privacy)
13. [정리](#cleanup)
14. [오류 모델 및 디버깅](#failure-model-and-debugging)
15. [인기 있는 시나리오를 구성 하는 방법](#how-to-configure-popular-scenarios)
16. [미리 인코딩 및 사후 오디오 조작 디코드](#pre-encode-and-post-decode-audio-manipulation)

## <a name="preface"></a>앞

원래 게임 채팅 API는 Xbox Live 게임에서 음성 채팅 시나리오의 구현에 도움이 되도록 채팅 사용자 및 음성 채널의 개념을 노출 하는 WinRT API입니다. 게임 채팅 API는 기본 오디오 장치의 하위 수준 관리를 요구 하는 동안 채팅 사용자 및 음성 채널의 개념을 노출 하는 WinRT API는 그 자체가 채팅 API를 기반으로 합니다. 게임 채팅 2 원래 게임 채팅 및 채팅 Api에 대 한 후속-동시 방송 스타일 같은 고급 채팅 시나리오에 대 한 더 많은 유연성을 제공 하는 동안 팀 통신 같은 기본 채팅 시나리오에 대 한 간단한 API 되도록 설계 되었습니다. 통신 및 실시간 오디오 조작 합니다. 게임 채팅 및 게임 채팅 2 모두 동일한 틈새 fill: Xbox Live 통합 사용, 게임에서 음성 채팅 제목 원격 인스턴스 게임 채팅 또는 게임에서 데이터 패킷을 전송 하는 데 전송 계층을 제공 하는 동안 각 Api에는 편리한 방법을 제공 채팅 2입니다.

게임 채팅 2 API에는 원래 게임 채팅 및 채팅 Api를 통해 많은 이점이 있습니다. 몇 가지 주요 기능은 다음과 같습니다.
* 유연한 사용자 기반 "채널" 모델에 제한 되지 않는 API입니다.
* 패킷 데이터 원본과 데이터 수신기로 필터링으로 인해 대역폭 향상 되었습니다.
* 앱을 구성할 수 있는 선호도 사용 하 여 2 수명이 긴 스레드를 내부 제한 합니다.
* C + + 및 WinRT 형식에서 사용할 수 있는 API입니다.
* 간소화 된 소비 모델 "작업" 패턴을 사용 하 여 맞춥니다.
* 사용자 지정 할당을 통해 게임 채팅 2 할당 리디렉션할 수 있으며 메모리 후크 합니다.
* 동일한 UWP + 단독 리소스 응용 프로그램 (시대) 헤더 간 같은 개발 환경을 위해서는 보다 편리 합니다.

이 문서에는 게임 채팅 및 게임 채팅 2와 게임 채팅에서 게임 채팅 2 c + + API로 마이그레이션하는 방법 간의 유사성 자세히 설명 합니다. 게임 채팅 2로 게임 채팅 개념을 매핑하는 방법을 알아보려면이 문서 읽고 WinR 특정 패턴에 대 한 [사용 하 여 게임 채팅 2 WinRT 프로젝션](using-game-chat-2-winrt.md) 그러면 추천 게임 채팅 2 WinRT API에 게임 채팅에서 마이그레이션 관심 있는 경우 화 이 문서의 원래 게임 채팅에 대 한 샘플 코드를 사용 하 여 C + + CX 합니다.

## <a name="prerequisites"></a>사전 요구 사항

게임 채팅 2를 사용 하 여 코딩을 시작 하기 전에 "마이크" 장치 기능을 선언 하는 앱의 AppXManifest 구성 했지만 해야 합니다. AppXManifest 기능 플랫폼 설명서;의 각 섹션에서 자세히 설명 되어 있습니다. 다음 코드 조각은 패키지/기능 노드 아래에 있어야 하는 "마이크" 장치 접근 권한 값은 노드를 보여 줍니다. 그렇지 않으면 채팅 차단 됩니다.

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

게임 채팅 2 컴파일 기본 GameChat2.h 헤더를 포함 해야 합니다. 제대로 연결 하려면 프로젝트 (일반적인 미리 컴파일된 헤더는 이러한 스텁 함수 구현은 컴파일러 "inline"으로 생성 하는 작은 쉽고 때문에 권장 됨) 하나 이상의 컴파일 단위에서 GameChat2Impl.h도 포함 해야 합니다.

게임 채팅 2 인터페이스 C + 컴파일 중에서 선택 하는 프로젝트를 컴파일할 필요가 + CX 기존 c + +; 비교 함께 사용할 수 있습니다. 또한 구현 치명적이 지 않은 오류 수 사용 하기 쉽게 예외 즈 프리 프로젝트에서 원하는 경우 하므로 보고 하는 수단으로 예외를 throw 하지 않습니다. 하지만 구현 체, 치명적인 오류 보고 (자세한 내용은 [오류 모델](#failure) 참조) 하는 수단으로 예외를 throw 합니다.

## <a name="initialization"></a>초기화

### <a name="game-chat"></a>게임 채팅

통해 수행 원래 게임 채팅 상호 작용은 `ChatManager` 클래스. 다음 예제에서는 생성 하는 `ChatManager` 기본 매개 변수를 사용 하 여 인스턴스:

```cpp
auto chatManager = ref new ChatManager();
```

### <a name="game-chat-2"></a>게임 채팅 2

게임 채팅 2와 모든 상호 작용 게임 채팅 2의를 통해 수행 됩니다 `chat_manager` 의도 하지 않은 단일 합니다. 라이브러리과 의미 있는 상호 작용이 발생 하기 전에 singleton은 초기화 되어야 합니다. Singleton은 초기화 시; 최대 동시 로컬 및 원격 채팅 사용자 지정 해야 사용자의 예상된 수에 비례하여 메모리를 미리 할당 하는 게임 채팅 2 때문입니다. 다음 예제에서는 로컬 및 원격 채팅 동시 사용자의 최대 4 될 때 의도 하지 않은 단일 인스턴스를 초기화 하는 방법을 보여 줍니다.

```cpp
chat_manager::singleton_instance().initialize(4);
```

기본 렌더링 볼륨 원격 채팅 사용자 및 게임 채팅 2 자동 음성-텍스트 변환 수행 해야 하는지 여부와 같은 초기화 하는 동안 지정할 수 있는 선택적 매개 변수가 여러 개 있습니다. 이러한 매개 변수에 대 한 자세한 내용은 설명서를 참조 `chat_manager::initialize()`.

## <a name="configuring-users"></a>사용자 구성

### <a name="game-chat"></a>게임 채팅

통해 수행 원래 게임 채팅 API에 로컬 사용자 추가 `ChatManager::AddLocalUserToChatChannelAsync()`. 여러 채팅 채널에 사용자 추가 다른 채널을 지정 하는 각 여러 번 호출에 필요 합니다. 다음 예제에서는 0 채널 xuid "myXuid"를 사용 하 여 사용자를 추가 하는 방법을 보여 줍니다.

```cpp
auto asyncOperation = chatManager->AddLocalUserToChatChannelAsync(0, L"myXuid");
```

원격 사용자 인스턴스를 직접 추가 되지 않습니다. 제목 원격 장치를 나타내는 통해 새 디바이스의 게임 채팅 알립니다 개체를 만드는 원격 장치 제목의 네트워크에 표시 되 면 `ChatManager::HandleNewRemoteConsole()`. 제목 게임 채팅을 구현 하 여 원격 장치를 나타내는 개체를 비교 하는 방법을 제공 해야 합니다 `CompareUniqueConsoleIdentifiersHandler`. 다음 예제에서는 게임 채팅에이 대리자를 제공 하는 방법을 보여 줍니다 가정 `Platform::String` 개체는 원격 장치와 문자열로 표현 하는 새 장치를 나타내는 데 `L"1"` 제목의 네트워크에 연결 합니다.

```cpp
auto token = chatManager->OnCompareUniqueConsoleIdentifiers +=
    ref new CompareUniqueConsoleIdentifiersHandler(
        [this](Platform::Object^ obj1, Platform::Object^ obj2)
    {
        return (static_cast<Platform::String^>(obj1)->Equals(static_cast<Platform::String^>(obj2)));
    });

...

Platform::String^ newDeviceIdentifier = ref new Platform::String(L"1");
chatManager->HandleNewRemoteConsole(newDeviceIdentifier);
```

이 장치의 게임 채팅에 게 되어, 되 면 모든 로컬 사용자에 대 한 정보가 포함 된 패킷을 생성 되며 제목에 이러한 패킷을 전송 원격 디바이스에서 게임 채팅 인스턴스를 제공 합니다. 마찬가지로, 원격 디바이스에서 게임 채팅 인스턴스 제목은 게임 채팅의 로컬 인스턴스에 전송 하는 해당 원격 장치에서 사용자에 대 한 정보가 포함 된 패킷을 생성 됩니다. 새 원격 사용자는 사용자를 통해 사용할 수 있는 로컬 인스턴스를 목록에 추가 로컬 인스턴스에 원격 새 사용자에 대 한 정보가 포함 된 패킷을 받으면 `ChatManager::GetUsers()`.

게임 채팅 인스턴스에서 사용자 제거는 유사한 호출을 통해 수행 `ChatManager::RemoveLocalUserFromChatChannelAsync()` 및 `ChatManager::RemoveRemoteConsoleAsync()`

### <a name="game-chat-2"></a>게임 채팅 2

동기적으로 통해 이루어집니다 Game Chat 2에 로컬 사용자 추가 `chat_manager::add_local_user()`. 사용자는 예제에서는 Xbox 사용자 Id를 사용 하 여 로컬 사용자를 나타내는 됩니다 `L"myLocalXboxUserId"`:

```cpp
chat_user* chatUserA = chat_manager::singleton_instance().add_local_user(L"myLocalXboxUserId");
```

사용자가 없는 했다는 특정 채널-추가 Game Chat 2를 사용 하 여 "통신 관계"의 개념을 채널 하는 대신 사용자에 게 문의 하 고 서로 통신할 수 있는지를 관리 합니다. 이 섹션의 통신 관계를 구성 하는 방법은 나중 주소가 지정 됩니다.

로컬 비슷합니다, 원격 사용자가 추가 된 사용자가 동기적으로 로컬 게임 채팅 2 인스턴스. 원격 사용자가 동시에 원격 디바이스를 나타내는 데 사용할 식별자와 연결 되어야 합니다. 게임 채팅 2 "끝점" 원격 장치에서 실행 되는 앱의 인스턴스를 나타냅니다. 이 예제에서는 사용자 B Xbox 사용자 Id를 사용 하 여 사용자 됩니다 `L"remoteXboxUserId"` 정수 표현 하는 끝점에서 `1`.

```cpp
chat_user* chatUserB = chat_manager::singleton_instance().add_remote_user(L"remoteXboxUserId", 1);
```

사용자가 인스턴스에 추가 되 면 각 원격 사용자와 각 로컬 사용자 간의 "통신 관계"을 구성 해야 합니다. 이 예제에서는 사용자 A와 B 사용자가 같은 팀에 및 양방향 통신을 허용 되는 가정 합니다. `c_communicationRelationshiSendAndReceiveAll` 상수를 양방향 통신을 나타내는 GameChat2.h에서 정의 됩니다. 관계를 사용 하 여 구성할 수 있습니다.

```cpp
chatUserA->local()->set_communication_relationship(chatUserB, c_communicationRelationshipSendAndReceiveAll);
```

마지막으로, 한다고 가정는 사용자 B가 떠났음을 게임에서 로컬 게임 채팅 인스턴스를 제거 해야 합니다. 다음을 호출을 사용 하 여 동기적으로 수행 됩니다.

```cpp
chat_manager::singleton_instance().remove_user(chatUserD);
```

더 자세한 예시 또는 자세한 정보에 대 한 개별 API 메서드에 대 한 참조를 [사용 하 여 게임 채팅 2-사용자가 구성](using-game-chat-2.md#configuring-users) 를 참조 하세요.

## <a name="processing-data"></a>데이터 처리

### <a name="game-chat"></a>게임 채팅

게임 채팅에는 고유한 전송 계층입니다. 이 앱에서 제공 되어야 합니다. 구독 하 여 나가는 패킷을 처리 되는 `OnOutgoingChatPacketReady` 이벤트 및 패킷 대상과 전송 요구 사항을 결정 하는 인수를 검사 합니다. 이벤트를 구독 하 고 인수를 전달 하는 방법을 보여 주는 다음 예제는 `HandleOutgoingPacket()` 제목에 의해 구현 하는 방법:

```cpp
auto token = chatManager->OnOutgoingChatPacketReady +=
    ref new Windows::Foundation::EventHandler<Microsoft::Xbox::GameChat::ChatPacketEventArgs^>(
        [this](Platform::Object^, Microsoft::Xbox::GameChat::ChatPacketEventArgs^ args)
    {
        this->HandleOutgoingPacket(args);
    });
```

들어오는 패킷을 통해 게임 채팅에 제출 된 `ChatManager::ProcessingIncomingChatMessage()`. 원시 패킷 버퍼와 원격 디바이스 식별자를 제공 해야 합니다. 다음 예제에서는 로컬에 저장 된 패킷을 전송 하는 방법을 보여 줍니다 `packetBuffer` 원격 디바이스 식별자는 로컬 변수에 저장 하 고 `remoteIdentifier`.

```cpp
Platform::String^ remoteIdentifier = /* The identifier associated with the device that generated this packet */
Windows::Storage::Streams::IBuffer^ packetBuffer = /* The incoming chat packet */

chatManager->ProcessIncomingChatMessage(packetBuffer, remoteIdentifier);
```

### <a name="game-chat-2"></a>게임 채팅 2

마찬가지로, Game Chat 2 없는 자체 전송 계층입니다. 이 앱에서 제공 되어야 합니다. 앱의 일반, 자주 호출을 통해 나가는 패킷을 처리 되는 `chat_manager::start_processing_data_frames()` 및 `chat_manager::finish_processing_data_frames()` 메서드 쌍. 이러한 메서드는 게임 채팅 2 앱에 보내는 데이터를 제공 하는 방법. 이러한 전용 네트워킹 스레드에서 자주 폴링할 수 있도록 신속 하 게 작동 하도록 설계 되었습니다. 이 네트워크 타이밍 또는 다중 스레드 콜백 복잡성 예측 불가능성 상관 없이 모든 대기 중인된 데이터를 검색할 수 있는 편리한 위치를 제공 합니다.

때 `chat_manager::start_processing_data_frames()` 라고, 모든 배열에서 대기 중인된 데이터를 보고 하는 `game_chat_data_frame` 포인터를 구성 합니다. 앱 배열에 대해 반복 하 고 대상 "끝점"를 검사 하 고 앱의 네트워킹 계층을 사용 하 여 적절 한 원격 앱 인스턴스를 데이터를 제공 해야 합니다. 모든 완료 한 번의 `game_chat_data_frame` 구조를 다시 호출 하 여 리소스를 해제할 Game Chat 2에 배열 전달 해야 할 `chat_manager:finish_processing_data_frames()`. 예:

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

데이터 프레임 처리 되는 더 자주, 낮을수록 오디오 대기 시간 최종 사용자에 게 표시 됩니다. 오디오는 40 ms 데이터 프레임;에 결합 됩니다. 제안 된 폴링 간격입니다.

들어오는 데이터를 통해 게임 채팅 2로 제출 `chat_manager::processing_incoming_data()`. 데이터 버퍼와 원격 끝점 식별자를 제공 해야 합니다. 다음 예제에서는 로컬 변수에 저장 된 데이터 패킷을 전송 하는 방법을 보여 줍니다 `dataFrame` 로컬 변수에 저장 된 원격 끝점 식별자 `remoteEndpointIdentifier`:

```cpp
uin64_t remoteEndpointIdentier = /* The identifier associated with the endpoint that generated this packet */
uint32_t dataFrameSize = /* The size of the incoming data frame, in bytes */
uint8_t* dataFrame = /* A pointer to the buffer containing the incoming data */

chatManager::singleton_instance().process_incoming_data(remoteEndpointIdentifier, dataFrameSize, dataFrame);
```

## <a name="processing-events"></a>이벤트 처리

### <a name="game-chat"></a>게임 채팅

게임 채팅 텍스트 메시지 수신을 또는 사용자의 접근성 기본 설정의 변경 등의-관심가 발생할 때 앱을 알리기 위해 이벤트 모델을 사용 합니다. 앱에 가입 하 고 관심 각 이벤트에 대 한 처리기를 구현 해야 합니다. 에 가입 하는 방법을 보여 주는이 예제는 `OnTextMessageReceived` 이벤트에 대 한 인수를 전달 합니다 `OnTextChatReceived()` 또는 `OnTranscribedChatReceived()` 앱에 의해 구현 하는 방법:

```cpp
auto token = chatManager->OnTextMessageReceived +=
    ref new Windows::Foundation::EventHandler<Microsoft::Xbox::GameChat::TextMessageReceivedEventArgs^>(
        [this](Platform::Object^, Microsoft::Xbox::GameChat::TextMessageReceivedEventArgs^ args)
    {
        if (args->ChatTextMessageType != ChatTextMessageType::TranscribedSpeechMessage)
        {
            this->OnTextChatReceived(args);
        }
        else
        {
            this->OnTranscribedChatReceived(args);
        }
    });
```

### <a name="game-chat-2"></a>게임 채팅 2

게임 채팅 2는 앱의 일반, 자주 호출을 통해 받은 문자 메시지 등의 앱에 대 한 업데이트를 제공 합니다 `chat_manager::start_processing_state_changes()` 및 `chat_manager::finish_processing_state_changes()` 메서드 쌍. 이러한 UI 렌더링 루프에서 매 프레임 마다 그래픽 호출할 수 있습니다 되도록 신속 하 게 작동 하도록 설계 되었습니다. 이 네트워크 타이밍 또는 다중 스레드 콜백 복잡성 예측 불가능성 상관 없이 대기 중인된 모든 변경 내용을 검색 하는 편리한 위치를 제공 합니다.

때 `chat_manager::start_processing_state_changes()` 는 호출 모든 대기 중인된 업데이트의 배열에 보고 됩니다 `game_chat_state_change` 포인터를 구성 합니다. 앱의 더욱 구체적인 형식에 대 한 기본 구조를 검사, 기본 구조를 입력 하 고 적절 하 게 해당 업데이트를 처리 합니다. 자세한 내용은 해당를 캐스팅 배열 반복 해야 합니다. 모든 완료 한 번 `game_chat_state_change` 현재 사용할 수 있는 개체를 호출 하 여 리소스를 해제할 게임 채팅 2로 다시 해당 배열 전달 해야 할 `chat_manager::finish_processing_state_changes()`. 예:

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

때문에 `chat_manager::remove_user()` 사용자 개체와 관련 된 메모리를 무효화 하는 즉시 상태 변경 사용자 개체에 대 한 포인터를 포함할 수 있습니다 `chat_manager::remove_user()` 상태 변경을 처리 하는 동안 호출 되지 않아야 합니다.

## <a name="text-chat"></a>텍스트 채팅

### <a name="game-chat"></a>게임 채팅

게임 채팅을 사용 하 여 텍스트 채팅을 보내려면 `GameChatUser::GenerateTextMessage()` 사용할 수 있습니다. 다음 예제에서는 로컬 채팅 사용자가 개체로 채팅 텍스트 메시지를 보내는 방법의 `chatUser` 변수:

```cpp
chatUser->GenerateTextMessage(L"Hello", false);
```

두 번째 부울 매개 변수는 텍스트-음성 변환을 제어합니다. 자세한 내용은 참조 [접근성] (#accessibilityGame 채팅에서는이 메시지를 포함 하는 채팅 패킷 다음 오류가 발생 합니다. 게임 채팅의 원격 인스턴스를 통해 텍스트 메시지의 알려주지 합니다 `OnTextMessageReceived` 이벤트입니다.

### <a name="game-chat-2"></a>게임 채팅 2

게임 채팅 2를 사용 하 여 텍스트 채팅을 보내려면 사용 하 여 `chat_user::chat_user_local::send_chat_text()`. 로컬 채팅 사용자가 나타내는 채팅 텍스트 메시지를 전송 하는 방법을 보여 주는 다음 예제는 `chatUser` 변수:

```cpp
chatUser->local()->send_chat_text(L"Hello");
```

게임 채팅 2에서이 메시지를 포함 하는 데이터 프레임을 생성 데이터 프레임에 대 한 대상 끝점 로컬 사용자의 텍스트를 수신 하도록 구성 된 사용자와 연결 됩니다. 원격 끝점에서 데이터를 처리할 때 메시지를 통해 노출 되는 `game_chat_text_chat_received_state_change`. 음성 채팅와 마찬가지로 텍스트 채팅에 대 한 권한 및 개인 정보 보호 제한은 유지 됩니다. 한 쌍의 사용자가 텍스트 채팅을 허용 하도록 구성 된 권한 또는 개인 정보 보호 제한 통신을 허용 되지만 하는 경우 텍스트 메시지 자동으로 삭제 됩니다.

입력 텍스트 채팅 및 디스플레이 지 원하는 접근성 필요 ( [접근성](#accessibility) 에 대 한 자세한 내용은 참조).

## <a name="accessibility"></a>접근성

입력 텍스트 채팅 및 디스플레이 지 원하는 게임 채팅 및 게임 채팅 2 모두 필요 합니다. 텍스트 입력이 플랫폼을 사용 하 여 광범위 한 실제 키보드 지금까지 적는 게임 장르에도 사용자가 텍스트 음성 변환 보조 기술을 사용 하 여 시스템 구성할 수 있기 때문에 필요 합니다. 마찬가지로, 텍스트 표시는 사용자가 음성-텍스트를 사용 하 여 시스템을 구성할 수 있으므로 필요 합니다. 게임 채팅 및 게임 채팅 2 감지 하 고 사용자의 접근성 기본; 존중 메서드를 제공 조건에 따라 이러한 설정에 따라 텍스트 메커니즘을 사용 하고자 할 수도 있습니다.

### <a name="text-to-speech---game-chat"></a>텍스트 음성 변환-게임 채팅

사용자가 사용 하도록 설정 하는 텍스트 음성 변환 하는 경우 `GameChatUser::HasRequestedSynthesizedAudio()` 가 true를 반환 합니다. 이 상태 감지 되 면 `GameChatUser::GenerateTextMessage()` 로컬 사용자와 관련 된 오디오 스트림을에 삽입 되는 텍스트-음성 오디오를 또한 생성 됩니다. 다음 예제에서는 로컬 사용자가 개체로 채팅 텍스트 메시지를 보내는 방법의 `chatUser` 변수:

```cpp
chatUser->GenerateTextMessage(L"Hello", true);
```

두 번째 부울 매개 변수는 앱이 음성-텍스트 기본 설정 재정의 데-'true' 게임 채팅 생성 해야 함을 나타냅니다 텍스트-음성 오디오 사용자의 텍스트 음성 변환 기본 설정을 사용 하도록 설정한 'false'을 표시 하는 경우 게임 채팅은 메시지에서 텍스트-음성 오디오를 생성 하지 해야 합니다.

### <a name="text-to-speech---game-chat-2"></a>텍스트 음성 변환-게임 채팅 2

사용자가 사용 하도록 설정 하는 텍스트 음성 변환 하는 경우 `chat_user::chat_user_local::text_to_speech_conversion_preference_enabled()` 'true' 반환 됩니다. 이 상태 감지 되 면 앱 텍스트 입력의 메서드를 제공 해야 합니다. 실제 또는 가상 키보드에서 제공 하는 텍스트 입력을 구성한 후에 문자열을 전달 합니다 `chat_user::chat_user_local::synthesize_text_to_speech()` 메서드. 게임 채팅 2를 감지 하 고 문자열 및 사용자의 접근성 있는 음성 기본 설정에 따라 오디오 데이터를 합성 하겠습니다. 예:

```cpp
chat_userA->local()->synthesize_text_to_speech(L"Hello");
```

이 작업의 일환으로 합성 오디오가 로컬 사용자의 오디오를 수신 하도록 구성 된 모든 사용자에 게 전송 됩니다. 하는 경우 `chat_user::chat_user_local::synthesize_text_to_speech()` 사용자가 아무 작업도 텍스트 음성 변환 활성화 된 게임 채팅 2가 없는 스레드에서 호출 됩니다.

### <a name="speech-to-text---game-chat"></a>음성-텍스트-게임 채팅

사용자가 음성-텍스트를 사용 하는 경우 `GameChatUser::HasRequestSynthesizedAudio()` 'true' 반환 됩니다. 이 상태 감지 되 면 게임 채팅 자동으로 각 원격 사용자의 오디오의 오디오 번역기 변환 하 고 통해 노출 합니다 `OnTextMessageReceived` 이벤트입니다. 때의 `OnTextMessageReceived` 기록과 메시지 수신 인해 이벤트가 발생 합니다 `TextMessageReceivedEventArgs` 메시지 유형으로 나타납니다 `ChatTextMessageType::TranscribedSpeechMessage`합니다.

### <a name="speech-to-text---game-chat-2"></a>음성-텍스트-게임 채팅 2

사용자가 음성-텍스트를 사용 하는 경우 `chat_user::chat_user_local::speech_to_text_conversion_preference_enabled()` 가 true를 반환 합니다. 이 상태 감지 되 면 변환 된 채팅 메시지와 관련 된 UI를 제공 하기 위해 앱을 준비 되어야 합니다. 게임 채팅 2 자동으로 각 원격 사용자의 오디오 번역기 변환 되며 통해 노출 되는 `game_chat_transcribed_chat_received_state_change`.

> `Windows::Xbox::UI::Accessibility` Xbox One 클래스를 하도록 설계 된 음성-텍스트 보조 기술에 초점을 사용 하 여 게임에서 텍스트 채팅의 간단한 렌더링을 제공 합니다.

음성-텍스트 성능 고려 사항에 대 한 자세한 내용은 [사용 하 여 게임 채팅 2-음성-텍스트 성능 고려 사항을](using-game-chat-2.md#speech-to-text-performance-considerations) 참조 하세요.

## <a name="ui"></a>UI

아무 곳 이나 플레이어에에서 표시 되어 있는지, 특히 게이머 점수 판, 같은 태그 목록을 사용자에 대 한 피드백으로 아이콘 음소거/말하기를 표시할 수도 것이 좋습니다. 게임 채팅 2 표시 하려면 적절 한 UI 요소를 결정 하는 간단한 수단을 제공 하는 결합 된 표시기를 소개 합니다.

### <a name="game-chat"></a>게임 채팅

A `GameChatUser` 적절 한 UI 요소-결정할 때 일반적으로 검사 하는 세 가지 속성이 `GameChatUser::TalkingMode`, `GameChatUser::IsMuted`, 및 `GameChatUser::RestrictionMode`. 다음 예제에서는 결정을 할당 하는 특정 아이콘 상수 값입니다 가능한 추론은 `iconToShow` 에서 변수는 `GameChatUser` 변수 'chatUser' 가리키는 개체.

```cpp
if (chatUser->RestrictionMode != None)
{
    if (!chatUser->IsMuted)
    {
        if (chatUser->TalkingMode != NotTalking)
        {
            iconToShow = Icon_ActiveSpeaker;
        }
        else
        {
            iconToShow = Icon_InactiveSpeaker;
        }
    }
    else
    {
        iconToShow = Icon_MutedSpeaker;
    }
}
else
{
    iconToShow = Icon_RestrictedSpeaker;
}
```

### <a name="game-chat-2"></a>게임 채팅 2

게임 채팅 2에는 결합 된 `game_chat_user_chat_indicator` 는 플레이어에 대 한 채팅의 현재, 일시적인 상태를 나타내는 데 사용 합니다. 이 값을 호출 하 여 검색 `chat_user::chat_indicator()`. 다음 예제에 대 한 표시기 값을 검색 하는 `chat_user` 'chatUser'를 'iconToShow' 변수에 할당 하는 특정 아이콘 상수 값을 결정 하는 변수 가리키는 개체:

```cpp
switch (chatUser->chat_indicator())
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

## <a name="muting"></a>음소거

## <a name="game-chat"></a>게임 채팅

음소거 또는 unmuting 게임 채팅에서 사용자에 대 한 호출을 통해 수행 됩니다 `ChatManager::MuteUserFromAllChannels()` 또는 `ChatManager::UnMuteUIserFromAllChannels()`각각 합니다. 사용자의 음소거 상태를 검사 하 여 검색할 수 있습니다 `GameChatUser::IsMuted` 또는 `GameChatUser::IsLocalUserMuted`.

## <a name="game-chat-2"></a>게임 채팅 2

`chat_user::chat_user_local::set_microphone_muted()` 로컬 사용자의 마이크의 음소거 상태를 전환 하 메서드를 사용할 수 있습니다. 마이크가 음소거 되어, 해당 마이크에서 오디오 없음 캡처됩니다. Kinect 등의 공유 장치에서 사용자가 음소거 상태 모든 사용자에 게 적용 됩니다.

`chat_user::chat_user_local::microphone_muted()` 메서드는 로컬 사용자의 마이크의 음소거 상태를 검색할 사용할 수 있습니다. 이 메서드는 로컬 사용자의 마이크에 대 한 호출을 통해 소프트웨어에서 음소거 되었습니다가 있는지 여부를 반영 `chat_user::chat_user_local::set_microphone_muted()`. 이 메서드는 하드웨어 음소거 제어, 예를 들어, 사용자의 헤드셋에서 단추를 통해 반영 하지 않습니다. 게임 채팅 2를 통해 사용자의 오디오 장치의 하드웨어 음소거 상태를 검색할 수 있는 방법은 없습니다.

`chat_user::chat_user_local::set_remote_user_muted()` 특정 로컬 사용자와 관련 하 여 원격 사용자의 음소거 상태를 전환 하 메서드를 사용할 수 있습니다. 원격 사용자 음이 소거 로컬 사용자가 모든 오디오를 들 되지 않거나 원격 사용자 로부터 텍스트 메시지를 수신 합니다.

## <a name="bad-reputation-auto-mute"></a>나쁜 평판 자동 음소거

일반적으로 원격 사용자가 시작 unmuted 합니다. 게임 채팅 및 게임 채팅 2에는 "나쁜 평판 자동 음소거" 기능이 있습니다. 이 사용자가 음소거 상태로 시작 (1) 원격 사용자가 로컬 사용자를 사용 하 여 친구 및 (2) 원격 사용자가 잘못 된 평판 플래그 때 의미 합니다. 게임 채팅 2는이 기능으로 인해 사용자 음소거 하는 경우 피드백을 제공 합니다. 자세한 정보를 [사용 하 여 게임 채팅 2-잘못 된 평판 자동 음소거를](using-game-chat-2.md#bad-reputation-auto-mute) 참조 하십시오.

## <a name="privilege-and-privacy"></a>권한 및 개인 정보 보호

게임 채팅 및 게임 채팅 2 제한을 Xbox Live 권한 및 개인 정보 보호 앱에서 관리 하는 모든 채널 또는 통신 관계를 기반으로 합니다. 또한 게임 채팅 2 정확 하 게 어떻게 제한은 영향을 주지 오디오 (예: 오디오는 오디오 제한 인지 uni 또는 양방향)의 방향을 결정 하는 진단 정보를 제공 합니다.

### <a name="game-chat"></a>게임 채팅

게임 채팅 권한 및 개인 정보 제외를 노출 합니다 `RestrictionMode` 속성입니다. 검사 하 여 검색할 수 있습니다 `GameChatUser::RestrictionMode`.

### <a name="game-chat-2"></a>게임 채팅 2

게임 채팅 2 권한 및 개인 정보 보호 제한 조회 수행 하는 사용자가 처음 추가 합니다. 사용자의 `chat_user::chat_indicator()` 는 항상 반환 `game_chat_user_chat_indicator::silent` 해당 작업이 완료 될 때까지 합니다. 사용자와의 통신 제한에 의해는 권한 또는 개인 정보, 사용자의 영향을 받는 경우 `chat_user::chat_indicator()` 돌아올 `game_chat_user_chat_indicator::platform_restricted`. 음성 및 텍스트를 둘 다 채팅; 플랫폼 통신 제한이 적용 여기서 텍스트 채팅 플랫폼 제한에 의해 차단 하지 않으면이 음성 채팅, 또는 그 반대로 인스턴스 안 됩니다.

`chat_user::chat_user_local::get_effective_communication_relationship()` 불완전 한 권한 및 개인 정보 보호 작업으로 인해 통신 하지 못하는 사용자를 구별 하는 데 사용할 수 있습니다. 통신 관계의 형태로 게임 채팅 2에 의해 적용 반환 `game_chat_communication_relationship_flags` 이유 관계 같지 않을 구성 된 관계의 형태로 및 `game_chat_communication_relationship_adjuster`. 예를 들어 조회 작업이 진행 되는 `game_chat_communication_relationship_adjuster` 는 `game_chat_communication_relationship_adjuster::intializing`. 이 메서드는 개발 및 디버깅 시나리오; 사용할 것으로 예상 UI에 영향을 사용 해야 ( [UI](#UI)참조).

## <a name="cleanup"></a>정리

원래 게임 채팅의 `ChatManager` WinRT 런타임 클래스에 대 한 참조 인터페이스를 계산 합니다. 마지막 참조 개수를 0으로 떨어질 때 메모리 리소스는 회수 하는 이와 같이 정리 하 고 있습니다. 게임 채팅 2의 c + + API와 상호 작용에 의도 하지 않은 단일 인스턴스를 통해 수행 됩니다. 호출 해야 앱에서 더 이상 게임 채팅 2를 통해 통신을 필요로 경우 `chat_manager::cleanup()`. 이렇게 하면 게임 채팅 2 즉시 통신 관리에 할당 된 모든 리소스를 해제 합니다. 게임 채팅 2의 WinRT API 및 리소스 관리에 대 한 세부 정보를 [사용 하 여 게임 채팅 2 WinRT 프로젝션](using-game-chat-2-winrt.md#cleanup)를 참조 하세요.

## <a name="failure-model-and-debugging"></a>오류 모델 및 디버깅

원래 게임 채팅는 WinRT API입니다. 따라서 예외를 통해 오류 보고 했습니다. 치명적이 지 않은 오류 또는 경고는 선택적 디버그 콜백을 통해 보고 됩니다. 게임 채팅 2의 c + + API는 치명적이 지 않은 오류 수 사용 하기 쉽게 예외 즈 프리 프로젝트에서 원하는 경우 하므로 보고 하는 수단으로 예외를 throw 하지 않습니다. 그러나 게임 채팅 2, 심각한 오류를 알리는 예외를 throw 하 고 있습니다. 이러한 오류는 API 오용, 인스턴스를 초기화 하거나 게임 채팅 인스턴스에서 제거한 후 사용자 개체에 액세스 하기 전에 사용자 게임 채팅 인스턴스를 추가 하는 등의 결과입니다. 이러한 오류 개발 초기에 발견 될 것으로 예상 되 고 Game Chat 2와 상호 작용 하는 데 패턴을 수정 하 여 해결할 수 있습니다. 이러한 오류가 발생 하면 예외가 발생 하기 전에 오류 원인을 하는지에 대 한 힌트 디버거에 인쇄 됩니다. 게임 채팅 2의 WinRT API는 예외를 통해 오류를 보고 WinRT 패턴을 따릅니다.

## <a name="how-to-configure-popular-scenarios"></a>인기 있는 시나리오를 구성 하는 방법

참조 [를 사용 하 여 게임 채팅 2-인기 있는 시나리오를 구성 하는 방법](using-game-chat-2.md#how-to-configure-popular-scenarios) 푸시-통화, 팀 및 브로드캐스트 스타일 통신 시나리오와 같은 인기 있는 시나리오를 구성 하는 방법에 대 한 예입니다.

## <a name="pre-encode-and-post-decode-audio-manipulation"></a>미리 인코딩 및 사후 오디오 조작 디코드

원래 게임 채팅을 통해 원시 마이크 오디오에 대 한 액세스를 허용 하 여 오디오 조작에 허용 되는 `OnPreEncodeAudioBuffer` 및 `OnPostDecodeAudioBuffers` 이벤트입니다. 게임 채팅 2 폴링 모델을 통해이 기능을 노출합니다. 자세한 내용은 [실시간 오디오 조작](real-time-audio-manipulation.md)를 참조 하세요.
