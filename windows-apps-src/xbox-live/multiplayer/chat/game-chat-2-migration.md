---
title: 게임 채팅 2 마이그레이션
description: 게임 채팅 2를 사용 하도록 기존 게임 채팅 코드를 마이그레이션하는 방법에 알아봅니다.
ms.date: 05/02/2018
ms.topic: article
keywords: xbox live, xbox, 게임, uwp, windows 10, 하나는 xbox, 2 게임 채팅, 게임 채팅, 음성 통신
ms.localizationpriority: medium
ms.openlocfilehash: e963210091694a07114f10d5a3dc531a353621df
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57653588"
---
# <a name="migration-from-game-chat-to-game-chat-2"></a>채팅 게임에서 게임 채팅 2로 마이그레이션

이 문서 유사성 게임 채팅, 게임 채팅 2 및 게임 채팅 2로 게임 채팅에서 마이그레이션하는 방법을 자세히 설명 합니다. 따라서 게임 채팅 2로 마이그레이션하려면 기존 게임 채팅 구현이 있는 타이틀에 대 한 것입니다. 제안 된 시작점은 게임 채팅 구현 이미 없다면 [게임 채팅 2를 사용 하 여](using-game-chat-2.md)입니다. 

## <a name="preface"></a>접두사

원래 게임 채팅 API는 Xbox Live 게임 음성 채팅 시나리오 구현을 지원 하기 위해 채팅 사용자 및 음성 채널의 개념을 노출 하는 WinRT API. 게임 채팅 API 빌드되는 자체는 오디오 장치의 하위 수준 관리를 필요로 하는 동안 채팅 사용자 및 음성 채널의 개념을 노출 하는 WinRT API는 채팅 API를 기반으로 합니다. 게임 채팅 2는 원래 게임 내 채팅 및 채팅 Api에 대 한 후속-동시에 브로드캐스트 스타일 등의 고급 채팅 시나리오에 대 한 더 많은 유연성을 제공 하는 동안 팀의 커뮤니케이션 등의 기본 채팅 시나리오에 대 한 간단한 API 되도록 설계 되었습니다. 통신 및 실시간 오디오 조작 합니다. 게임 채팅 및 게임 채팅 2 모두 동일한 틈새를 입력 합니다: Xbox Live 통합 사용의 경우 음성 게임 내 채팅, 제목 게임 채팅 이나 게임의 원격 인스턴스 간에 데이터 패킷 전송을 위한 전송 계층을 제공 하는 동안 각 Api에는 편리한 방법을 제공 2를 채팅할 수 있습니다.

게임 채팅 2 API에는 원래 게임 채팅 및 채팅 Api를 통해 많은 이점이 있습니다. 다음과 같은 사항이 중요 합니다.
* 유연한 사용자 기반 API를 "채널" 모델 제한 되지 않습니다.
* 데이터 원본 및 데이터 수신자 모두 필터링 하 여 패킷 대역폭 개선 합니다.
* 내부 제한을 2 수명이 긴 스레드 선호도 앱을 구성할 수 있습니다.
* C + + 및 WinRT 형태로 사용할 수 있는 API입니다.
* 간소화 된 소비 모델 "작업 수행" 패턴에 맞게 조정 합니다.
* 사용자 지정 할당자를 통해 게임 채팅 2 할당 리디렉션할 메모리 후크입니다.
* 동일한 UWP + 편리한 간 구획 개발 환경을 위해 배타적 리소스 응용 프로그램 (시대) 헤더입니다.

이 문서 유사성 게임 채팅, 게임 채팅 2 및 채팅 게임에서 게임 채팅 2 c + + API로 마이그레이션하는 방법을 자세히 설명 합니다. 채팅 게임에서 게임 채팅 2 WinRT API로 마이그레이션에 관심이 것이 좋습니다 게임 채팅 개념 게임 채팅 2를 매핑하고 표시 하는 방법을 이해 하려면이 문서를 읽어 [를 사용 하 여 게임 채팅 2 WinRT 프로젝션](using-game-chat-2-winrt.md) 에 패턴 WinRT에 관련이 있습니다. 이 문서에서는 원래 게임 채팅에 대 한 샘플 코드를 사용 하 여 C + + /cli CX 합니다.

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

게임 채팅 2 인터페이스 프로젝트를 C +를 사용 하 여 컴파일 중에서 선택할 필요가 없습니다 + 기존 c + + 및 CX 사용 하 여 사용할 수 있습니다. 또한 구현 치명적이 지 않은 오류 보고를 사용할 수 있습니다 쉽게 예외 없는 프로젝트에서 원하는 경우 있도록 수단으로 예외를 throw 하지 않습니다. 하지만 구현, 심각한 오류를 보고 하는 수단으로 예외를 throw (참조 [실패 모델](#failure-model-and-debugging) 자세한).

## <a name="initialization"></a>초기화

### <a name="game-chat"></a>게임 채팅

통해 이루어집니다 원래 게임 채팅 상호 작용을 `ChatManager` 클래스입니다. 다음 예제에서는 생성 하는 방법을 보여 줍니다는 `ChatManager` 기본 매개 변수를 사용 하 여 인스턴스:

```cpp
auto chatManager = ref new ChatManager();
```

### <a name="game-chat-2"></a>게임 채팅 2

2를 통해 게임 채팅의 게임 채팅 2를 사용 하 여 모든 상호 작용 이루어집니다 `chat_manager` singleton입니다. Singleton은 라이브러리를 사용 하 여 의미 있는 작용이 발생 하기 전에 초기화 되어야 합니다. 단일 항목을 로컬 및 원격 채팅 동시 사용자 수가 최대; 초기화 시 지정 해야 사용자의 예상된 수에 비례 하는 메모리를 미리 할당 하는 게임 채팅 2 때문입니다. 다음 예제에서는 로컬 및 원격 채팅 동시 사용자 수가 최대 4 개의 될 경우 singleton 인스턴스를 초기화 합니다. 방법을 보여 줍니다.

```cpp
chat_manager::singleton_instance().initialize(4);
```

기본 렌더링 볼륨 사용자 원격 채팅 및 게임 채팅 2 자동 음성 텍스트 변환 수행 해야 하는지 여부와 같은 고 초기화 하는 동안 지정할 수 있는 여러 선택적 매개 변수 있습니다. 이러한 매개 변수에 대 한 세부 정보를 설명서 참조 `chat_manager::initialize()`합니다.

## <a name="configuring-users"></a>사용자 구성

### <a name="game-chat"></a>게임 채팅

로컬 사용자를 원래 게임 채팅 API 추가 통해 수행 됩니다 `ChatManager::AddLocalUserToChatChannelAsync()`합니다. 여러 채팅 채널에 사용자를 추가 여러 개의 호출이 필요를 각각 다른 채널을 지정 합니다. 다음 예제에서는 채널 0에 xuid "myXuid"를 사용 하 여 사용자를 추가 하는 방법을 보여 줍니다.

```cpp
auto asyncOperation = chatManager->AddLocalUserToChatChannelAsync(0, L"myXuid");
```

원격 사용자가 직접 인스턴스에 추가 되지 않습니다. 원격 장치에 표시 되 면 해당 타이틀의 네트워크 제목 게임 채팅을 통해 새 장치의 알립니다을 원격 장치를 나타내는 개체를 만듭니다 `ChatManager::HandleNewRemoteConsole()`합니다. 제목 게임 채팅을 구현 하 여 원격 장치를 나타내는 개체를 비교 하는 방법을 제공 해야 `CompareUniqueConsoleIdentifiersHandler`합니다. 다음 예제에서는 게임, 채팅에이 대리자를 제공 하는 방법을 보여 줍니다 가정 `Platform::String` 개체는 원격 장치 및 문자열을 나타내는 새 장치를 나타내는 데 사용 됩니다 `L"1"` 타이틀의 네트워크 연결 했습니다.

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

게임 채팅에 알렸습니다이 장치 되 면 모든 로컬 사용자에 대 한 정보를 포함 하는 패킷을 생성 하 고 원격 장치에 게임 채팅 인스턴스로 전송에 대 한 제목에 이러한 패킷은 제공 합니다. 마찬가지로, 원격 장치에 게임 채팅 인스턴스 제목 게임 채팅의 로컬 인스턴스로 전송 됩니다 하는 원격 장치에서 사용자에 대 한 정보를 포함 하는 패킷을 생성 됩니다. 새 원격 사용자를 통해 사용할 수 있는 사용자의 로컬 인스턴스 목록에 추가 됩니다 로컬 인스턴스에 새 원격 사용자에 대 한 정보를 포함 하는 패킷을 받으면 `ChatManager::GetUsers()`합니다.

유사한 호출을 통해 수행 됩니다 게임 채팅 인스턴스에서 사용자를 제거 `ChatManager::RemoveLocalUserFromChatChannelAsync()` 및 `ChatManager::RemoveRemoteConsoleAsync()`

### <a name="game-chat-2"></a>게임 채팅 2

통해 동기적으로 이루어지는 게임 채팅 2에 로컬 사용자를 추가 `chat_manager::add_local_user()`합니다. 사용자 A는 Xbox 사용자 Id를 사용 하 여 로컬 사용자를이 예제에서는 나타냅니다 `L"myLocalXboxUserId"`:

```cpp
chat_user* chatUserA = chat_manager::singleton_instance().add_local_user(L"myLocalXboxUserId");
```

사용자가는 게임 채팅 2를 사용 하 여 "통신 관계"의 개념은 채널 대신 사용자에 게 문의 하 고 서로 통신할 수 있는지를 관리-특정 채널에 추가 합니다. 통신 관계를 구성 하는 메서드는이 단원의 뒷부분 해결 됩니다.

원격 사용자가 로컬에 비슷한 사용자에 추가 됩니다 동기적으로 로컬 게임 채팅 2 개의 인스턴스. 원격 사용자가 동시에 원격 장치를 나타내는 데 사용할 식별자를 사용 하 여 연결 해야 합니다. 게임 채팅 2 "끝점"으로 원격 장치에서 실행 되는 앱의 인스턴스를 가리킵니다. 이 예에서 사용자 B Xbox 사용자 Id를 사용 하 여 사용자 됩니다 `L"remoteXboxUserId"` 정수를 나타내는 끝점에서 `1`합니다.

```cpp
chat_user* chatUserB = chat_manager::singleton_instance().add_remote_user(L"remoteXboxUserId", 1);
```

사용자가 인스턴스에 추가 되 면 "통신 관계" 각 원격 사용자와 각 로컬 사용자를 구성 해야 합니다. 이 예제에서는 같은 팀에 사용자 A와 사용자 B는 및 양방향 통신이 허용 되는 가정 합니다. `c_communicationRelationshiSendAndReceiveAll` 상수 양방향 통신을 나타내는 GameChat2.h에서 정의 됩니다. 관계를 사용 하 여 구성할 수 있습니다.

```cpp
chatUserA->local()->set_communication_relationship(chatUserB, c_communicationRelationshipSendAndReceiveAll);
```

마지막으로, 사용자 B 게임에서 게임 채팅 로컬 인스턴스에서 제거 해야 하는지을 가정 합니다. 다음 호출을 사용 하 여 동기적으로 수행 됩니다.

```cpp
chat_manager::singleton_instance().remove_user(chatUserD);
```

가리킵니다 [를 사용 하 여 게임 채팅 2-사용자 구성](using-game-chat-2.md#configuring-users) 더 자세한 예제 또는 자세한 정보에 대 한 개별 API 메서드에 대 한 참조입니다.

## <a name="processing-data"></a>데이터 처리

### <a name="game-chat"></a>게임 채팅

게임 채팅 자체 전송 계층을 사용 되지 않은 이 앱에서 제공 되어야 합니다. 구독 하 여 나가는 패킷을 처리 하는 `OnOutgoingChatPacketReady` 이벤트 및 패킷을 대상과 전송 요구 사항 확인에 대 한 인수를 검사 합니다. 다음 예제에서는 이벤트를 구독할 인수를 전달 하는 방법을 보여 줍니다는 `HandleOutgoingPacket()` 타이틀에 구현 하는 메서드:

```cpp
auto token = chatManager->OnOutgoingChatPacketReady +=
    ref new Windows::Foundation::EventHandler<Microsoft::Xbox::GameChat::ChatPacketEventArgs^>(
        [this](Platform::Object^, Microsoft::Xbox::GameChat::ChatPacketEventArgs^ args)
    {
        this->HandleOutgoingPacket(args);
    });
```

들어오는 패킷 게임 채팅을 통해 전송 됩니다 `ChatManager::ProcessingIncomingChatMessage()`합니다. 원시 패킷 버퍼와 원격 장치 식별자를 제공 해야 합니다. 다음 예제에서는 로컬에 저장 되는 패킷을 전송 하는 방법을 보여 줍니다 `packetBuffer` 및 로컬 변수에 저장 된 원격 장치 식별자 `remoteIdentifier`합니다.

```cpp
Platform::String^ remoteIdentifier = /* The identifier associated with the device that generated this packet */
Windows::Storage::Streams::IBuffer^ packetBuffer = /* The incoming chat packet */

chatManager->ProcessIncomingChatMessage(packetBuffer, remoteIdentifier);
```

### <a name="game-chat-2"></a>게임 채팅 2

마찬가지로, 게임 채팅 2 없는 전송 계층 자체입니다. 이 앱에서 제공 되어야 합니다. 앱의 일반, 자주 호출 하 여 나가는 패킷을 처리 하는 합니다 `chat_manager::start_processing_data_frames()` 및 `chat_manager::finish_processing_data_frames()` 메서드 쌍입니다. 이러한 메서드는 게임 채팅 2 앱에 보내는 데이터를 제공 하는 방법입니다. 전용 네트워킹 스레드에서 자주 폴링 될 수 있도록 신속 하 게 작동 하도록 설계 되었습니다. 이 네트워크 타이밍 지출 하거나 복잡 한 다중 스레드 콜백의 예측 불가능성을 고려 하는 방법에 대 한 걱정 없이 모든 큐에 대기 중인된 데이터를 검색 하는 편리한 장소를 제공 합니다.

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

들어오는 데이터를 통해 게임 채팅 2로 제출 되 `chat_manager::processing_incoming_data()`합니다. 데이터 버퍼 및 원격 끝점 식별자를 제공 되어야 합니다. 다음 예제에서는 로컬 변수에 저장 되는 데이터 패킷을 전송할 `dataFrame` 와 로컬 변수에 저장 된 원격 끝점 식별자 `remoteEndpointIdentifier`:

```cpp
uin64_t remoteEndpointIdentier = /* The identifier associated with the endpoint that generated this packet */
uint32_t dataFrameSize = /* The size of the incoming data frame, in bytes */
uint8_t* dataFrame = /* A pointer to the buffer containing the incoming data */

chatManager::singleton_instance().process_incoming_data(remoteEndpointIdentifier, dataFrameSize, dataFrame);
```

## <a name="processing-events"></a>이벤트를 처리합니다.

### <a name="game-chat"></a>게임 채팅

게임 채팅 이벤트 모델을 사용 하 여 특정 상황이 발생 합니다. 예: 텍스트 메시지를 받으면 또는 사용자의 내게 필요한 옵션 기본 설정을 변경 하는 경우 앱에 알림을 보내야 합니다. 앱에 가입 하 고 관심 있는 각 이벤트에 대 한 처리기를 구현 해야 합니다. 구독 하는 방법을 보여 주는이 예제는 `OnTextMessageReceived` 이벤트에 대 한 인수를 전달 합니다 `OnTextChatReceived()` 또는 `OnTranscribedChatReceived()` 앱에서 구현 되는 메서드:

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

### <a name="game-chat"></a>게임 채팅

게임 내 채팅을 사용 하 여 텍스트 채팅 보낼 `GameChatUser::GenerateTextMessage()` 사용할 수 있습니다. 다음 예제에서는 나타내는 로컬 채팅 사용자와 채팅 텍스트 메시지를 보내는 방법의 `chatUser` 변수:

```cpp
chatUser->GenerateTextMessage(L"Hello", false);
```

두 번째 부울 매개 변수는 텍스트 음성 변환 변환을 제어합니다. 자세한 내용은 참조 하세요. [내게 필요한 옵션](#accessibility)합니다. 게임 채팅에는 다음이 메시지를 포함 하는 채팅 패킷을 생성 합니다. 게임 채팅의 원격 인스턴스를 통해 문자 메시지의 알림이 `OnTextMessageReceived` 이벤트입니다.

### <a name="game-chat-2"></a>게임 채팅 2

사용 하 여 게임 채팅 2를 사용 하 여 텍스트 채팅을 보내려면 `chat_user::chat_user_local::send_chat_text()`합니다. 다음 예제에서는 나타내는 로컬 채팅 사용자와 채팅 텍스트 메시지를 보내는 방법을 보여 줍니다는 `chatUser` 변수:

```cpp
chatUser->local()->send_chat_text(L"Hello");
```

게임 채팅 2는이 메시지를 포함 하는 데이터 프레임을 생성 합니다. 데이터 프레임에 대 한 대상 끝점에 연결 된 로컬 사용자의 텍스트를 수신 하도록 구성 된 사용자가 됩니다. 메시지를 통해 노출 될 데이터를 원격 끝점에서 처리 되 면을 `game_chat_text_chat_received_state_change`입니다. 음성 채팅와 마찬가지로 권한 및 개인 정보 제한은 텍스트 채팅에 대 한 적용 됩니다. 텍스트 채팅 허용 하도록 구성 된 한 쌍의 사용자 권한 및 개인 정보 제한을 통신을 허용 하지 않습니다 하지만 경우 문자 메시지에 자동으로 삭제 됩니다.

텍스트 채팅 입력 및 표시를 지 원하는 내게 필요한 옵션에 대 한 필요 (참조 [내게 필요한 옵션](#accessibility) 자세한).

## <a name="accessibility"></a>접근성

입력 텍스트 채팅 및 표시를 지 원하는 것이 게임 내 채팅 및 게임 채팅 2 모두 필요 합니다. 텍스트 입력 이므로 필요한 플랫폼, 지금까지 사용 하 여 광범위 한 실제 키보드를 보유 하지 않은 게임 장르 에서도 사용자 텍스트 음성 변환 보조 기술을 사용 하 여 시스템 구성할 수 있습니다. 마찬가지로, 텍스트 표시 되므로 필요한 사용자 음성-텍스트를 사용 하도록 시스템을 구성할 수 있습니다. 게임 채팅 및 게임 채팅 2를 모두 검색 하 고 사용자의 내게 필요한 옵션 기본 설정; 존중 메서드를 제공 조건에 따라 이러한 설정에 따라 텍스트 메커니즘을 사용 하도록 설정 하려고 할 수 있습니다.

### <a name="text-to-speech---game-chat"></a>텍스트 음성 변환-게임 채팅

사용자에 게 사용 하도록 설정 하는 텍스트 음성 변환 `GameChatUser::HasRequestedSynthesizedAudio()` true를 반환 합니다. 이 상태가 감지 되 면 `GameChatUser::GenerateTextMessage()` 또한 로컬 사용자와 연결 된 오디오 스트림에 삽입 되는 오디오 텍스트 음성 변환 시킵니다. 다음 예제에서는 나타내는 로컬 사용자와 채팅 텍스트 메시지를 보내는 방법의 `chatUser` 변수:

```cpp
chatUser->GenerateTextMessage(L"Hello", true);
```

응용 프로그램이 음성-텍스트 기본 설정을 재정의할 수 있도록 두 번째 부울 매개 변수는 'true' 게임 채팅 생성 해야 함을 나타냅니다 오디오 텍스트 음성 변환 나타내는 'f a l' 하는 동안 사용자의 텍스트 음성 변환 기본 설정에서 사용할 수 있는 경우-게임 채팅은 메시지의 텍스트 음성 변환 오디오를 생성 하지 않습니다.

### <a name="text-to-speech---game-chat-2"></a>텍스트 음성 변환-게임 채팅 2

사용자에 게 사용 하도록 설정 하는 텍스트 음성 변환 `chat_user::chat_user_local::text_to_speech_conversion_preference_enabled()` 'true'를 반환 합니다. 이 상태가 감지 되 면 앱의 텍스트 입력 메서드를 제공 해야 합니다. 실제 또는 가상 키보드에서 제공 되는 텍스트 입력을 만든 후에 문자열을 전달 합니다 `chat_user::chat_user_local::synthesize_text_to_speech()` 메서드. 게임 채팅 2 감지 하 고 문자열 및 사용자의 액세스 가능한 음성 기본 설정을 기반으로 하는 오디오 데이터를 합성 합니다. 예를 들어 다음과 같은 가치를 제공해야 합니다.

```cpp
chat_userA->local()->synthesize_text_to_speech(L"Hello");
```

이 작업의 일환으로 합성 오디오가 로컬 사용자에서 오디오를 수신 하도록 구성 된 모든 사용자에 게 전송 됩니다. 경우 `chat_user::chat_user_local::synthesize_text_to_speech()` 작업도 발생 하지 않도록 설정 된 게임 채팅 2는 텍스트 음성 변환 되지 않은 사용자에서 호출 됩니다.

### <a name="speech-to-text---game-chat"></a>Speech-to-text - Game Chat

사용자가 음성-텍스트를 사용 하는 경우 `GameChatUser::HasRequestSynthesizedAudio()` 'true'를 반환 합니다. 이 상태가 감지 되 면 게임 채팅 자동으로 각 원격 사용자의 오디오 오디오를 기록 하 고를 통해 노출 된 `OnTextMessageReceived` 이벤트입니다. 경우는 `OnTextMessageReceived` 기록 메시지를 수신으로 인해 이벤트가 발생 합니다 `TextMessageReceivedEventArgs` 메시지 유형으로 표시 됩니다 `ChatTextMessageType::TranscribedSpeechMessage`합니다.

### <a name="speech-to-text---game-chat-2"></a>Speech-to-text - Game Chat 2

사용자가 음성-텍스트를 사용 하는 경우 `chat_user::chat_user_local::speech_to_text_conversion_preference_enabled()` true를 반환 합니다. 이 상태가 감지 되 면 기록된 채팅 메시지와 연결 된 UI를 제공 하는 앱 준비 되어야 합니다. 게임 채팅 2 자동으로 각 원격 사용자의 오디오를 기록 하 고를 통해 노출 된 `game_chat_transcribed_chat_received_state_change`합니다.

> `Windows::Xbox::UI::Accessibility` Xbox One 클래스를 하도록 설계 된 음성-텍스트 보조 기술에 중점을 둔 텍스트 게임 내 채팅의 간단한 렌더링을 제공 합니다.

가리킵니다 [를 사용 하 여 게임 채팅 2-음성-텍스트 성능 고려 사항](using-game-chat-2.md#speech-to-text-performance-considerations) 음성-텍스트 성능 고려 사항에 대 한 자세한 내용은 합니다.

## <a name="ui"></a>UI

어디서 나 플레이어에에서 표시 되어 있는지, 특히는 스코어 같은 게이머 태그의 목록을 사용자에 대 한 피드백으로 말하기 음소거 아이콘도 표시 하는 것이 좋습니다. 게임 채팅 2 표시 하는 적절 한 UI 요소를 결정 하는 간단한 방법을 제공 하는 결합 된 표시기를 소개 합니다.

### <a name="game-chat"></a>게임 채팅

A `GameChatUser` 적절 한 UI 요소-결정할 때 일반적으로 검사 하는 세 가지 속성이 있습니다 `GameChatUser::TalkingMode`를 `GameChatUser::IsMuted`, 및 `GameChatUser::RestrictionMode`합니다. 다음 예제에서는 가능한 경험적 접근을 할당할 특정 아이콘 상수 작업을 결정 하는 데는 `iconToShow` 에서 변수를 `GameChatUser` 'chatUser' 변수가 가리키는 개체입니다.

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

게임 채팅 2에는 결합 된 `game_chat_user_chat_indicator` 플레이어 채팅의 즉시 현재 상태를 나타내는 데 사용 합니다. 이 값을 검색 하는 `chat_user::chat_indicator()`합니다. 다음 예제에 대 한 표시기 값을 검색 하는 방법을 보여 줍니다는 `chat_user` 'iconToShow' 변수에 값을 할당할 특정 아이콘 상수 값을 확인 하려면 ' chatUser' 변수가 가리키는 개체:

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

음소거 또는 사용자 게임 채팅에 음소거를 호출 하 여 수행 되기 `ChatManager::MuteUserFromAllChannels()` 또는 `ChatManager::UnMuteUIserFromAllChannels()`, 각각. 사용자의 음소거 상태를 검사 하 여 검색할 수 있습니다 `GameChatUser::IsMuted` 또는 `GameChatUser::IsLocalUserMuted`합니다.

## <a name="game-chat-2"></a>게임 채팅 2

`chat_user::chat_user_local::set_microphone_muted()` 로컬 사용자의 마이크 음소거 상태를 전환 하려면 메서드를 사용할 수 있습니다. 마이크 음소거 되 면 해당 마이크에서 오디오 없는 캡처됩니다. 사용자가 공유 장치, Kinect, 예: 음소거 상태를 모든 사용자에 게 적용 됩니다.

`chat_user::chat_user_local::microphone_muted()` 메서드를 사용 하 여 로컬 사용자의 마이크 음소거 상태를 검색할 수 있습니다. 이 메서드 호출을 통해 소프트웨어에서 로컬 사용자의 마이크에 음소거 되어 있는지 여부를 반영 `chat_user::chat_user_local::set_microphone_muted()`;이 메서드는 하드웨어 음소거 제어, 예를 들어 사용자의 헤드셋에서 단추를 통해 반영 되지 않습니다. 게임 채팅 2를 통해 사용자의 오디오 장치 하드웨어 음소거 상태를 검색 하는 방법은 없습니다.

`chat_user::chat_user_local::set_remote_user_muted()` 메서드를 특정 로컬 사용자와 관련 하 여 원격 사용자의 음소거 상태 전환에 사용할 수 있습니다. 원격 사용자 음소거 되어 로컬 사용자 오디오 듣거나 원격 사용자의 모든 텍스트 메시지를 수신 하지 않습니다.

## <a name="bad-reputation-auto-mute"></a>잘못 된 평판 자동 음소거

일반적으로 원격 사용자는 시작 unmuted 합니다. 게임 채팅 및 게임 채팅 2에는 "잘못 된 평판 자동 음소거" 기능이 있습니다. 이 사용자가 시작할 수 음소거 상태에서 (1) 원격 사용자가 로컬 사용자와 친구 및 (2) 원격 사용자가 잘못 된 평판 플래그 때 것을 의미 합니다. 게임 채팅 2 사용자는이 기능에 따라 음소거 되어 때 피드백을 제공 합니다. 가리킵니다 [를 사용 하 여 게임 채팅 2-잘못 된 평판 자동 음소거](using-game-chat-2.md#bad-reputation-auto-mute) 자세한 내용은 합니다.

## <a name="privilege-and-privacy"></a>권한 및 개인 정보

게임 채팅 및 게임 채팅 2 Xbox Live 권한 및 개인 정보 제한을 적용 앱에서 관리 되는 모든 채널 또는 통신 관계를 기반으로 합니다. 또한 게임 채팅 2 정확 하 게 하는 방법을 제한에 영향을 오디오 (예: 오디오 오디오 제한이 인지 단위 또는 양방향)의 방향을 결정 하는 진단 정보를 제공 합니다.

### <a name="game-chat"></a>게임 채팅

게임 채트 권한 및 개인 정보를 통해 노출 된 `RestrictionMode` 속성입니다. 검사 하 여 검색할 수 있습니다 `GameChatUser::RestrictionMode`합니다.

### <a name="game-chat-2"></a>게임 채팅 2

게임 채팅 2 권한 및 개인 정보 보호 제한에서 조회를 수행 먼저 사용자를 추가 합니다. 사용자의 `chat_user::chat_indicator()` 는 항상 반환 `game_chat_user_chat_indicator::silent` 해당 작업이 완료 될 때까지 합니다. 사용자와의 통신을 권한 또는 개인 정보 제한을 사용자의 영향을 받는 경우 `chat_user::chat_indicator()` 돌아갑니다 `game_chat_user_chat_indicator::platform_restricted`합니다. 텍스트 및 음성 채팅;에 플랫폼 통신 제한 사항이 적용 됩니다. 인스턴스는 텍스트 채팅 플랫폼 제한에 의해 차단 됩니다 아닌 음성 채팅, 또는 그 반대로 안 됩니다.

`chat_user::chat_user_local::get_effective_communication_relationship()` 불완전 한 권한 및 개인 정보 작업으로 인해 사용자가 통신할 수 없는 경우와 구분 하는 데 사용할 수 있습니다. 형태로 게임 채팅 2에 의해 적용 통신 관계가 반환 `game_chat_communication_relationship_flags` 이유 관계 동일 하지 않을 구성 관계의 형태로 및 `game_chat_communication_relationship_adjuster`합니다. 예를 들어 조회 작업은 계속 진행에서 합니다 `game_chat_communication_relationship_adjuster` 됩니다 `game_chat_communication_relationship_adjuster::intializing`합니다. 이 메서드는 개발 및 디버깅 시나리오에서 사용할 수 해야 UI에 영향을 줄을 사용 해야 (참조 [UI](#UI)).

## <a name="cleanup"></a>정리

원래 게임 채팅의 `ChatManager` 는 WinRT 런타임 클래스-참조 인터페이스를 계산 합니다. 마지막으로 참조 횟수가 0으로 떨어지는 경우 메모리 리소스가 회수 됩니다 따라서을 정리 합니다. 게임 채팅 2의 c + + API와의 상호 작용; singleton 인스턴스를 통해 이루어집니다. 앱에서 게임 채팅 2를 통해 통신을 더 이상 필요한 경우 호출 해야 `chat_manager::cleanup()`합니다. 이 게임 채팅 2를 통해 통신을 관리 하려면 할당 된 모든 리소스를 즉시 해제 합니다. 게임 채팅 2의 WinRT API 및 리소스 관리에 대 한 내용은 참조 하세요 [를 사용 하 여 게임 채팅 2 WinRT 프로젝션](using-game-chat-2-winrt.md#cleanup)합니다.

## <a name="failure-model-and-debugging"></a>오류 모델 및 디버깅

원래 게임 채팅는 WinRT API; 따라서 예외를 통해 오류를 보고 합니다. 선택적 디버그 콜백을 통해 심각 하지 않은 오류 또는 경고가 보고 됩니다. 게임 채팅 2의 c + + API 치명적이 지 않은 오류 보고를 사용할 수 있습니다 쉽게 예외 없는 프로젝트에서 원하는 경우 있도록 수단으로 예외를 throw 하지 않습니다. 하지만 게임 채팅 2는 치명적 오류에 게 알리기 위해 예외를 throw는. 이러한 오류는 게임 채팅 인스턴스를 인스턴스를 초기화 하거나 게임 채팅 인스턴스에서 제거 된 후 사용자 개체에 액세스 하기 전에 사용자를 추가 하는 등의 API 오용의 결과입니다. 이러한 오류는 개발 초기에 파악할 수를 예상 되 고 게임 채팅 2와 상호 작용 하는 데 사용 되는 패턴을 수정 하 여 수정할 수 있습니다. 이러한 오류가 발생 하면 예외가 발생 하기 전에 오류를 일으킨 원인을 관한 힌트 디버거에 출력 됩니다. 게임 채팅 2의 WinRT API는 예외를 통해 오류를 보고 WinRT 패턴을 따릅니다.

## <a name="how-to-configure-popular-scenarios"></a>인기 있는 시나리오를 구성 하는 방법

가리킵니다 [를 사용 하 여 게임 채팅 2-인기 있는 시나리오를 구성 하는 방법](using-game-chat-2.md#how-to-configure-popular-scenarios) 푸시-강연, 팀 및 방송 스타일 통신 시나리오와 같은 인기 있는 시나리오를 구성 하는 방법에 대 한 예제입니다.

## <a name="pre-encode-and-post-decode-audio-manipulation"></a>인코딩 사전 및 사후 오디오 조작을 디코딩

원래 게임 채팅 원시 마이크를 통해 오디오에 대 한 액세스를 허용 하 여 오디오를 조작 하기 위한 허용 되는 `OnPreEncodeAudioBuffer` 고 `OnPostDecodeAudioBuffers` 이벤트입니다. 게임 채팅 2 폴링 모델을 통해이 기능을 노출합니다. 자세한 내용은 참조 [실시간 오디오 조작](real-time-audio-manipulation.md)합니다.
