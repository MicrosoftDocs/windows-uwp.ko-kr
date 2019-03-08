---
title: 실시간 오디오 조작
description: 조작 하 고 게임 채팅 2에 의해 캡처된 채팅 오디오를 처리 하는 방법에 알아봅니다.
ms.date: 05/10/2018
ms.topic: article
keywords: xbox live, xbox, 게임, uwp, windows 10, 하나는 xbox, 게임 채팅 2, 게임 채팅, 음성 통신, 버퍼 조작, 오디오 조작
ms.localizationpriority: medium
ms.openlocfilehash: 7746080ea8a9698993a679b425f41442e4a6d943
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57643898"
---
# <a name="real-time-audio-manipulation"></a>실시간 오디오 조작

게임 채팅 2 개발자에 게 검사 하 고 선수 채팅 오디오 데이터를 조작할 채팅 오디오 파이프라인으로 주입 하는 옵션을 제공 합니다. 이 흥미로운 게임에서 플레이어의 음성 오디오 효과 적용 하는 것에 대 한 유용할 수 있습니다. 게임 채팅 2의 오디오 조작 파이프라인은 오디오 데이터를 폴링할 수 있는 오디오 스트림에서 개체를 통해 상호 작용 합니다. 콜백을 사용 하는 대신이 모델 검사 하거나 조작 하는 개발자에 가장 편리 하 게 모든 처리 스레드에 오디오 허용 합니다.

실시간 오디오 조작을 사용 하 여 간단한 연습 다음 항목을 포함 하는 아래 추천은:

1. [오디오 조작 파이프라인을 초기화합니다.](#initializing-the-audio-manipulation-pipeline)
2. [오디오 스트림 상태 변경 처리](#processing-audio-stream-state-changes)
3. [채팅 오디오 인코딩 미리 조작](#manipulating-pre-encode-chat-audio-data)
4. [채팅 오디오 디코딩 후 조작](#manipulating-post-decode-chat-audio-data)
5. [채팅 사용자 수명](#chat-user-lifetimes)

## <a name="initializing-the-audio-manipulation-pipeline"></a>오디오 조작 파이프라인을 초기화합니다.

기본적으로 게임 채팅 2가 실시간 오디오 조작을 사용 하지 않습니다. 실시간 오디오 조작에 사용 하려면 앱을 지정 해야 합니다 길 원하는 오디오 조작의 형태는에서 사용 하도록 설정 `chat_manager::initialize()` audioManipulationMode 매개 변수를 설정 합니다.

현재, 다음과 같은 오디오 조작 지원 됩니다.

* `game_chat_audio_manipulation_mode_flags::none` -오디오 조작을 사용 하지 않도록 설정 합니다. 이는 기본 구성입니다. 이 모드에서는 채팅 오디오 중단 없이 전송 됩니다.
* `game_chat_audio_manipulation_mode_flags::pre_encode_stream_manipulation` 사용 하도록 설정에는 미리 오디오 조작을 인코딩합니다. 이 모드에서는 인코딩되기 전에 오디오 조작 파이프라인을 통해 로컬 사용자가 생성 하는 모든 채팅 오디오를 공급 됩니다. @만 채팅 오디오 데이터를 검사 하 고 조작 하는 앱 인 경우에 인코딩된 고 전송할 수 있도록 게임 채팅 2로 다시 변경 되지 않은 오디오 버퍼를 제출 하려면 앱의 책임 이기도 합니다.
* `game_chat_audio_manipulation_mode_flags::post_decode_stream_manipulation` 사용 하도록 설정 후 오디오 조작을 디코딩합니다. 이 모드에서는 수신자가 있지만 렌더링 되기 전에 디코딩하면 후 오디오 조작 파이프라인을 통해 원격 사용자 로부터 받은 모든 채팅 오디오를 공급 됩니다. @만 채팅 오디오 데이터를 검사 하 고 조작 하는 앱 인 경우에 혼합 렌더링 될 수 있도록 게임 채팅 2로 다시 변경 되지 않은 오디오 버퍼를 제출 하는 앱의 책임 이기도 합니다.

## <a name="processing-audio-stream-state-changes"></a>오디오 스트림 상태 변경 처리

통해 오디오 스트림 상태에 대 한 업데이트를 제공 하는 게임 채팅 2 `game_chat_stream_state_change` 구조입니다. 이러한 업데이트 정보에 대 한 스트림이 업데이트 된 및 업데이트 된 방법을 저장 합니다. 이러한 업데이트에 대 한 호출을 통해 폴링할 수 있습니다 합니다 `chat_manager::start_processing_stream_state_changes()` 및 `chat_manager::finish_processing_stream_state_changes()` 메서드 쌍입니다. 이 메서드 쌍을 제공 최신, 큐에 대기 중인 오디오 스트림의 모든 상태 업데이트의 배열로 `game_chat_stream_state_change` 구조체 포인터입니다. 앱은 배열을 반복 하 고 각 업데이트를 적절 하 게 처리 해야 합니다. 한 번만 사용할 수 있는 모든 `game_chat_stream_state_change` 업데이트 처리 되었는지, 해당 배열 게임 채팅 2 ~에 다시 전달 되어야 `chat_manager::finish_processing_stream_state_changes()`합니다. 예를 들어 다음과 같은 가치를 제공해야 합니다.

```cpp
uint32_t streamStateChangeCount;
game_chat_stream_state_change_array streamStateChanges;
chat_manager::singleton_instance().start_processing_stream_state_changes(&streamStateChangeCount, &streamStateChanges);

for (uint32_t streamStateChangeIndex = 0; streamStateChangeIndex < streamStateChangeCount; ++streamStateChangeIndex)
{
    switch (streamStateChanges[streamStateChangeIndex]->state_change_type)
    {
        case game_chat_stream_state_change_type::pre_encode_audio_stream_created:
        {
            HandlePreEncodeAudioStreamCreated(streamStateChanges[streamStateChangeIndex].pre_encode_audio_stream);
            break;
        }

        case Xs::game_chat_2::game_chat_stream_state_change_type::pre_encode_audio_stream_closed:
        {
            HandlePreEncodeAudioStreamClosed(streamStateChanges[streamStateChangeIndex].pre_encode_audio_stream);
            break;
        }

        ...
    }
}
chat_manager::singleton_instance().finish_processing_stream_state_changes(streamStateChanges);
```

## <a name="manipulating-pre-encode-chat-audio-data"></a>채팅 오디오 데이터를 미리 인코딩 조작

채팅을 통해 로컬 사용자에 대 한 오디오 데이터를 미리 인코딩에 대 한 액세스를 제공 하는 게임 채팅 2는 `pre_encode_audio_stream` 클래스입니다.

### <a name="stream-lifetime"></a>Stream 수명
새 `pre_encode_audio_stream` 인스턴스를 사용 하도록 앱에 대 한 준비를 통해 제공 될 것을 `game_chat_stream_state_change` 의 구조 `state_change_type` 필드로 `game_chat_stream_state_change_type::pre_encode_audio_stream_created`합니다. 오디오 스트림 수 있게 됩니다 게임 채팅 2에 스트림 상태 변경이이 반환 되 면 미리 인코딩 오디오 조작 합니다.

기존 `pre_encode_audio_stream` 됩니다 오디오 조작 하는 데 사용할 수 없습니다, 앱을 통해 알림을 받게 됩니다는 `game_chat_stream_state_change` 의 구조 `state_change_type` 로 설정 된 필드 `game_chat_stream_state_change_type::pre_encode_audio_stream_closed`합니다. 이 오디오 스트림과 사용 하 여 연결 된 리소스를 정리 하려면 앱의 기회입니다. 이 스트림 상태 변경 게임 채팅 2에 반환 되 면 오디오 스트림의 수 없게 됩니다에 대 한 오디오 조작을 미리 인코딩.

닫힌 경우 `pre_encode_audio_stream` 에 해당 리소스를 모두 반환 스트림이 파괴 하 고 앱을 통해 알림을 받게 됩니다는 `game_chat_stream_state_change` 의 구조 `state_change_type` 로 설정 된 필드 `game_chat_stream_state_change_type::pre_encode_audio_stream_destroyed`합니다. 모든 참조 또는이 스트림에 대 한 포인터를 정리 해야 합니다. 이 스트림 상태 변경 게임 채팅 2에 반환 되 면 오디오 스트림 메모리가 무효화 됩니다.

### <a name="stream-users"></a>Stream 사용자
스트림과 연결 된 사용자 목록을 사용 하 여 검사할 수 있습니다 `pre_encode_audio_stream::get_users()`합니다.

### <a name="audio-formats"></a>오디오 형식
오디오 형식의 게임 채팅 2에서 앱을 검색 하는 버퍼를 사용 하 여 검사할 수 있습니다 `pre_encode_audio_stream::get_pre_processed_format()`합니다. 전처리 된 오디오 형식이 mono은 항상입니다. 32 비트 부동 소수점 수, 16 비트 정수 및 32 비트 정수 표현 하는 데이터를 처리 하도록 앱을 예상 해야 합니다.

앱을 인코딩 및 전송에 사용 하 여 전송 되는 조작 버퍼 오디오 포맷의 게임 채팅 2 알려야 `pre_encode_audio_stream::set_processed_format()`합니다. 처리 된 형식에 대 한 오디오를 미리 인코딩 스트림을 이러한 전제 조건을 충족 해야 합니다.

* 형식 mono 이어야 합니다.
* 형식에는 32 비트 float PCM, PCM 32 비트 정수 또는 16 비트 정수 PCM 형식 이어야 합니다.
* 형식으로 샘플링 주기 플랫폼을 기반으로 하는 사전 조건을 따라야 합니다. Xbox One 연대 8 kHz, 12 kHz, 16 kHz 및 24 kHz 샘플 속도 지원합니다. Xbox One 및 PC에 대 한 UWP 8 kHz, 12 kHz, kHz 16, 24 kHz, 32 kHz, 44.1 kHz 및 48 kHz 샘플 속도 지원합니다.

### <a name="retrieving-and-submitting-audio"></a>검색 하 고 오디오를 전송 합니다.
앱을 쿼리할 수 미리 사용 하 여 처리 하는 데 사용할 수 있는 버퍼의 수에 대 한 오디오 스트림 인코딩 `pre_encode_audio_stream::get_available_buffer_count()`합니다. 앱이 최소 버퍼 수 없을 때까지 오디오 처리를 지연 하려는 경우이 정보를 사용할 수 있습니다. 각 10 버퍼 큐에 대기 되며 미리 오디오 스트림 인코딩 및 오디오 지연은 오디오 파이프라인에서 대기 시간이 제공 하므로 앱을 드레이닝는 것이 좋습니다. 해당 4 개 이상의 버퍼 큐는 전에 오디오 스트림을 미리 인코딩.

앱에서 오디오 버퍼를 검색할 수 있습니다를 사용 하 여 오디오 스트림의 미리 인코딩 `pre_encode_audio_stream::get_next_buffer()`합니다. 새 오디오 버퍼 모든 40ms 한 번에 사용할 수 있습니다. 이 메서드에서 반환 된 버퍼를 해제 해야 합니다 `pre_encode_audio_stream::return_buffer()` 작업이 완료 되 면 사용 합니다. 최대 10 개 큐에 대기 중이거나 unreturned 버퍼에 대 한 특정된 시점에 존재할 수는 미리 오디오 스트림 인코딩. 이 제한에 도달 오디오 플레이어의 소스에서 캡처한 새 버퍼에 처리 중인 버퍼의 일부는 반환 될 때까지 삭제 됩니다.

앱 인코딩 및 전송에 사용 하 여 게임 채팅 2를 다시 검사 한 후 조작 버퍼를 제출할 수 `pre_encode_audio_stream::submit_buffer()`입니다. 게임 채팅 2 버퍼에 전송 되므로 전체 및 확장-전체의 오디오 조작을 지원 `pre_encode_audio_stream::submit_buffer()` 반드시 동일한 버퍼에서 검색 되도록 `pre_encode_audio_stream::get_next_buffer()`합니다. 이 스트림과 사용 하 여 연결 된 사용자에 따라 이러한 제출 된 버퍼에 대 한 개인 정보/권한 적용 됩니다. 모든 40ms 오디오 스트림에서 다음 40ms 인코딩된 되며 전송 합니다. 오디오 발생을 방지 하려면 일정 한 속도로이 스트림에 지속적으로 경 해야 하는 오디오에 대 한 버퍼를 제출 해야 합니다.

### <a name="stream-contexts"></a>Stream 컨텍스트
앱 사용자 지정 포인터 크기의 컨텍스트 값을 관리할 수 있습니다를 사용 하 여 오디오 스트림을 미리 인코딩 `pre_encode_audio_stream::set_custom_stream_context()` 고 `pre_encode_audio_stream::custom_stream_context()`입니다. 이러한 사용자 지정 스트림 컨텍스트 게임 채팅 2의 오디오 스트림 및 보조 데이터 간의 매핑을 만드는 데 도움이 됩니다: 메타 데이터, 게임 상태 등을 스트림 합니다.

### <a name="example"></a>예
다음은 사용 하는 방법에 대 한 간소화 된 종단 간 샘플 미리 오디오 처리 프레임의 오디오 스트림 인코딩:

```cpp
uint32_t streamStateChangeCount;
game_chat_stream_state_change_array streamStateChanges;
chat_manager::singleton_instance().start_processing_stream_state_changes(&streamStateChangeCount, &streamStateChanges);

for (uint32_t streamStateChangeIndex = 0; streamStateChangeIndex < streamStateChangeCount; ++streamStateChangeIndex)
{
    switch (streamStateChanges[streamStateChangeIndex]->state_change_type)
    {
        case game_chat_stream_state_change_type::pre_encode_audio_stream_created:
        {
            pre_encode_audio_stream* stream = streamStateChanges[streamStateChangeIndex]->pre_encode_audio_stream;
            stream->set_processed_audio_format(...);
            stream->set_custom_stream_context(...);
            HandlePreEncodeAudioStreamCreated(stream);
            break;
        }

        case game_chat_2::game_chat_stream_state_change_type::pre_encode_audio_stream_closed:
        {
            HandlePreEncodeAudioStreamClosed(streamStateChanges[streamStateChangeIndex].pre_encode_audio_stream);
            break;
        }

        case game_chat_2::game_chat_stream_state_change_type::pre_encode_audio_stream_destroyed:
        {
            HandlePreEncodeAudioStreamDestroyed(streamStateChanges[streamStateChangeIndex].pre_encode_audio_stream);
            break;
        }

        ...
    }
}
chat_manager::singleton_instance().finish_processing_stream_state_changes(streamStateChanges);

uint32_t preEncodeAudioStreamCount;
pre_encode_audio_stream_array preEncodeAudioStreams;
chat_manager::singleton_instance().get_pre_encode_audio_streams(&preEncodeAudioStreamCount, &preEncodeAudioStreams);
for (uint32_t preEncodeAudioStreamIndex = 0; preEncodeAudioStreamIndex < preEncodeAudioStreamCount; ++preEncodeAudioStreamIndex)
{
    pre_encode_audio_stream* stream = preEncodeAudioStreams[preEncodeAudioStreamIndex];
    StreamContext* context = reinterpret_cast<StreamContext*>(stream->custom_stream_context());

    game_chat_audio_format audio_format = stream->get_pre_processed_format();

    uint32_t preProcessedBufferByteCount;
    void* preProcessedBuffer;
    stream->get_next_buffer(&preProcessedBufferByteCount, &preProcessedBuffer);

    while (preProcessedBuffer != nullptr)
    {
        void* processedBuffer = nullptr;
        switch (audio_format.bits_per_sample)
        {
            case 16:
            {
                assert (audio_format.sample_type == game_chat_sample_type::integer);
                processedBuffer = ManipulateChatBuffer<int16_t>(preProcessedBufferByteCount, preProcessedBuffer, context);
                break;
            }

            case 32:
            {
                switch (audio_format.sample_type)
                {
                    case game_chat_sample_type::integer:
                    {
                        processedBuffer = ManipulateChatBuffer<int32_t>(preProcessedBufferByteCount, preProcessedBuffer, context);
                        break;
                    }

                    case game_chat_sample_type::ieee_float:
                    {
                        processedBuffer = ManipulateChatBuffer<float>(preProcessedBufferByteCount, preProcessedBuffer, context);
                        break;
                    }

                    default:
                    {
                        assert(false);
                        break;
                    }
                }
                break;
            }

            default:
            {
                assert(false);
                break;
            }
        }
        // processedBuffer can be the same as preProcessedBuffer (in-place manipulation) or it can be a buffer of
        // memory not managed by Game Chat 2 (out-of-place manipulation).
        stream->submit_buffer(processedBuffer);
        // Only return buffers retrieved from Game Chat 2. Do not return foreign memory to return_buffer.
        stream->return_buffer(preProcessedBuffer);
        stream->get_next_buffer(&preProcessedBufferByteCount, &preProcessedBuffer);
    }
}

Sleep(audioProcessingPeriodInMilliseconds);
```

## <a name="manipulating-post-decode-chat-audio-data"></a>채팅 오디오 데이터를 디코딩 후 조작

게임 채팅 2는 채팅을 통해 오디오 데이터를 디코딩 후에 대 한 액세스를 제공 합니다 `post_decode_audio_source_stream` 및 `post_decode_audio_sink_stream` 클래스를 사용자가 조작할 수 있습니다. 원격 사용자의 오디오 고유 하 게 채팅 오디오의 각 로컬 수신기에 대해 되도록 합니다.

### <a name="sources-and-sinks"></a>원본 및 싱크
Pre-encode 파이프라인 달리 모델 처리 후 오디오 데이터를 디코딩 한 개의 분할 되어 두 클래스: `post_decode_audio_source_stream` 고 `post_decode_audio_sink_stream`입니다. 원격 사용자의 디코딩된 오디오를 검색할 수 있습니다 `post_decode_audio_source_stream` 개체를 조작 하 고 보낼 `post_decode_audio_sink_stream` 렌더링에 대 한 개체입니다. 이렇게 하면 게임 채팅 2의 간의 통합 후 오디오 처리 파이프라인과 유용한 오디오 미들웨어를 디코딩 한 수 있습니다.

### <a name="stream-lifetime"></a>Stream 수명
새 `post_decode_audio_source_stream` 또는 `post_decode_audio_sink_stream` 인스턴스를 사용 하도록 앱에 대 한 준비를 통해 제공 될 것을 `game_chat_stream_state_change` 의 구조 `state_change_type` 필드 설정 `game_chat_stream_state_change_type::post_decode_audio_source_stream_created` 또는 `game_chat_stream_state_change_type::post_decode_audio_sink_stream_created`, 각각. 오디오 스트림의 스트림 상태 변경이이 게임 채팅 2에 반환 되 면 수 있게 됩니다 후 오디오 조작을 디코딩합니다.

기존 `post_decode_audio_source_stream` 또는 `post_decode_audio_sink_stream` 가 오디오 조작 하는 데 사용할 수 없습니다, 앱을 통해 알림을 받게 됩니다는 `game_chat_stream_state_change` 의 구조 `state_change_type` 로 설정 된 필드 `game_chat_stream_state_change_type::post_decode_audio_source_stream_closed` 또는 `game_chat_stream_state_change_type::post_decode_audio_sink_stream`각각. 이 오디오 스트림과 사용 하 여 연결 된 리소스를 정리 하려면 앱의 기회입니다. 이 스트림 상태 변경 게임 채팅 2에 반환 되 면 오디오 스트림의 수 없게 됩니다에 대 한 후 오디오 조작을 디코딩합니다. 원본 스트림을 조작에 대 한 버퍼가 더 이상은 대기를 의미 합니다. 싱크 스트림 버퍼를 더 이상 렌더링할를 제출 하는 의미입니다.

닫힌 경우 `post_decode_audio_source_stream` 또는 `post_decode_audio_sink_stream` 에 해당 리소스를 모두 반환 스트림이 파괴 하 고 앱을 통해 알림을 받게 됩니다는 `game_chat_stream_state_change` 의 구조 `state_change_type` 로 설정 된 필드 `game_chat_stream_state_change_type::post_decode_audio_source_stream_destroyed` 또는 `game_chat_stream_state_change_type::post_decode_audio_sink_stream_destroyed`, 각각. 모든 참조 또는이 스트림에 대 한 포인터를 정리 해야 합니다. 이 스트림 상태 변경 게임 채팅 2에 반환 되 면 오디오 스트림 메모리가 무효화 됩니다.

### <a name="stream-users"></a>Stream 사용자
원격 사용자 post-decode 원본 스트림에 연결 된 목록을 사용 하 여 검사할 수 있습니다 `post_decode_audio_source_stream::get_users()`합니다. Post-decode 싱크 스트림과 연결 된 로컬 사용자 목록을 사용 하 여 검사할 수 있습니다 `post_decode_audio_sink_stream::get_users()`합니다.

### <a name="audio-formats"></a>오디오 형식
오디오 형식의 게임 채팅 2에서 앱을 검색 하는 버퍼를 사용 하 여 검사할 수 있습니다 `post_decode_audio_source_stream::get_pre_processed_format()`합니다. 전처리 된 오디오 형식에는 항상 mono, 16 비트 정수 PCM 됩니다.

앱을 사용 하 여 렌더링에 대 한 전송 되는 조작 버퍼 오디오 포맷의 게임 채팅 2 알려야 `post_decode_audio_sink_stream::set_processed_format()`합니다. 처리 된 형식에 대 한 후 오디오를 디코딩 싱크 스트림을 이러한 전제 조건을 충족 해야 합니다.

* 형식 보다 작거나 64 채널 있어야 합니다.
* 16 비트 정수 PCM (최적의), 20 비트 정수 (24 비트 컨테이너)에서 PCM, PCM 24 비트 정수로, 32 비트 정수 PCM 또는 32 비트 float PCM (16 비트 정수 PCM 기본 형식) 형식 이어야 합니다. 
* 형식으로 샘플링 주기는 초당 1000 개 및 200000 샘플 사이 여야 합니다.

### <a name="retrieving-and-submitting-audio"></a>검색 하 고 오디오를 전송 합니다.
앱을 쿼리할 수 후 오디오 소스 스트림을 사용 하 여 처리 하는 데 사용할 수 있는 버퍼의 수에 대 한 디코딩 `post_decode_audio_source_stream::get_available_buffer_count()`합니다. 앱이 최소 버퍼 수 없을 때까지 오디오 처리를 지연 하려는 경우이 정보를 사용할 수 있습니다. 각 10 버퍼 큐에 대기 되며 후 오디오 원본 스트림을 디코드 하 고 오디오 지연 되므로 앱 드레이닝는 것이 좋습니다. 오디오 파이프라인에서 대기 시간을 소개 합니다 해당 4 개 이상의 버퍼 큐는 전에 후 오디오 스트림을 디코딩합니다.

앱에서 오디오 버퍼를 검색할 수는 후 오디오 소스 스트림을 사용 하 여 디코딩 `post_decode_audio_source_stream::get_next_buffer()`합니다. 새 오디오 버퍼 모든 40ms 한 번에 사용할 수 있습니다. 이 메서드에서 반환 된 버퍼를 해제 해야 합니다 `post_decode_audio_source_stream::return_buffer()` 작업이 완료 되 면 사용 합니다. 최대 10 개 큐에 대기 중이거나 unreturned 버퍼에 대 한 특정된 시점에 존재할 수는 후 오디오 원본 스트림을 디코드 합니다. 이 제한에 도달 원격 플레이어에서 디코딩된 새 버퍼에 처리 중인 버퍼의 일부는 반환 될 때까지 삭제 됩니다.

앱을 통해 게임 채팅 2를 다시 검사 한 후 조작 버퍼 후 오디오 싱크 스트림을 사용 하 여 렌더링할을 디코딩 제출할 수 `post_decode_audio_sink_stream::submit_mixed_buffer()`입니다. 게임 채팅 2 버퍼에 전송 되므로 전체 및 확장-전체의 오디오 조작을 지원 `post_decode_audio_sink_stream::submit_mixed_buffer()` 반드시 동일한 버퍼에서 검색 되도록 `post_decode_audio_source_stream::get_next_buffer()`합니다. 모든 40ms 오디오 스트림에서 다음 40ms 렌더링 됩니다. 오디오 발생을 방지 하려면 일정 한 속도로이 스트림에 지속적으로 경 해야 하는 오디오에 대 한 버퍼를 제출 해야 합니다.

### <a name="privacy-and-mixing"></a>개인 정보 보호 및 혼합
앱의 책임에서 검색 하는 버퍼를 혼합 하는 것 post-decode 파이프라인의 소스-싱크 모델로 인해 `post_decode_audio_source_stream` 개체를 제출 하는 혼합 된 버퍼가 `post_decode_audio_sink_stream` 렌더링에 대 한 개체입니다. 즉, 앱의 책임 적절 한 개인 정보 보호 및 권한을 적용을 사용 하 여 혼합을 수행 하는 것입니다. 게임 채팅 2 제공 `post_decode_audio_sink_stream::can_receive_audio_from_source_stream()` 간단 하 고 효율적이 정보를 쿼리할 수 있도록 합니다.

### <a name="chat-indicators"></a>채팅 표시기

오디오를 디코딩 후 조작 각 사용자에 대 한 채팅 표시기 상태는 적용 되지 것입니다. 예를 들어 원격 사용자 음소거 되어 오디오 앱에 제공 됩니다 있지만 원격 사용자에 대 한 채팅 표시기도 음소거 표시 됩니다. 원격 사용자 통신, 오디오는 제공 되지만 앱은 해당 사용자의 오디오를 포함 하는 오디오 믹스를 제공 하는 여부에 관계 없이 통신 채팅 표시기 표시 됩니다. UI와 채팅 표시기에 대 한 자세한 내용은 참조 하세요. [게임 채팅 2를 사용 하 여](using-game-chat-2.md#ui)입니다. 추가 앱 별 제한을 오디오 믹스에 있는 사용자 확인을 사용 하는 경우 것 게임 채팅 2에서 제공 하는 채팅 지표를 읽어 이러한 동일한 제한 때는 앱의 책임입니다.

### <a name="stream-contexts"></a>Stream 컨텍스트
앱 사용자 지정 포인터 크기의 컨텍스트 값을 관리할 수 있습니다 사용 하 여 오디오 스트림을 디코딩 후에 `set_custom_stream_context()` 및 `custom_stream_context()` 메서드. 이러한 사용자 지정 스트림 컨텍스트 게임 채팅 2의 오디오 스트림 및 보조 데이터 간의 매핑을 만드는 데 도움이 됩니다: 메타 데이터, 게임 상태 등을 스트림 합니다.

### <a name="example"></a>예
다음은 사용 하는 방법에 대 한 간소화 된 종단 간 샘플 후 오디오 스트림을 오디오 처리 프레임을 디코딩할:

```cpp
uint32_t streamStateChangeCount;
game_chat_stream_state_change_array streamStateChanges;
chat_manager::singleton_instance().start_processing_stream_state_changes(&streamStateChangeCount, &streamStateChanges);

for (uint32_t streamStateChangeIndex = 0; streamStateChangeIndex < streamStateChangeCount; ++streamStateChangeIndex)
{
    switch (streamStateChanges[streamStateChangeIndex]->state_change_type)
    {
        case game_chat_stream_state_change_type::post_decode_audio_source_stream_created:
        {
            post_decode_audio_source_stream* stream = streamStateChanges[streamStateChangeIndex]->post_decode_audio_source_stream;
            stream->set_custom_stream_context(...);
            HandlePostDecodeAudioSourceStreamCreated(stream);
            break;
        }

        case game_chat_stream_state_change_type::post_decode_audio_source_stream_closed:
        {
            HandlePostDecodeAudioSourceStreamClosed(stream);
            break;
        }

        case game_chat_stream_state_change_type::post_decode_audio_source_stream_destroyed:
        {
            HandlePostDecodeAudioSourceStreamDestroyed(stream);
            break;
        }

        case game_chat_stream_state_change_type::post_decode_audio_sink_stream_created:
        {
            post_decode_audio_sink_stream* stream = streamStateChanges[streamStateChangeIndex]->post_decode_audio_sink_stream;
            stream->set_custom_stream_context(...);
            stream->set_processed_format(...);
            HandlePostDecodeAudioSinkStreamCreated(stream);
            break;
        }

        case game_chat_stream_state_change_type::post_decode_audio_sink_stream_closed:
        {
            HandlePostDecodeAudioSinkStreamClosed(stream);
            break;
        }

        case game_chat_stream_state_change_type::post_decode_audio_sink_stream_destroyed:
        {
            HandlePostDecodeAudioSinkStreamDestroyed(stream);
            break;
        }

        ...
    }
}

chat_manager::singleton_instance().finish_processing_stream_state_changes(streamStateChanges);

uint32_t sourceStreamCount;
post_decode_audio_source_stream_array sourceStreams;
chatManager::singleton_instance().get_post_decode_audio_source_streams(&sourceStreamCount, &sourceStreams);

uint32_t sinkStreamCount;
post_decode_audio_sink_stream_array sinkStreams;
chatManager::singleton_instance().get_post_decode_audio_sink_streams(&sinkStreamCount, &sinkStreams);

//
// MixBuffer is a custom type defined as:
// struct MixBuffer
// {
//     uint32_t bufferByteCount;
//     void* buffer;
// };
//
std::vector<std::pair<post_decode_audio_source_stream*, MixBuffer>> cachedSourceBuffers;

for (uint32_t sourceStreamIndex = 0; sourceStreamIndex < sourceStreamCount; ++sourceStreamIndex)
{
    post_decode_audio_source_stream* sourceStream = sourceStreams[sourceStreamIndex];

    MixBuffer mixBuffer;
    sourceStream->get_next_buffer(&mixBuffer.bufferByteCount, &mixBuffer.buffer);
    if (buffer != nullptr)
    {
        // Stash the buffer to return after we are done with mixing. If this program was using audio middleware, now
        // would be an appropriate time to plumb the buffer through the middleware
        cachedSourceBuffer.push_back(std::pair<post_decode_audio_source_stream*, MixBuffer>{sourceStream, mixBuffer});
    }
}

// Loop over each sink stream, perform mixing, and submit
for (uint32_t sinkStreamIndex = 0; sinkStreamIndex < sinkStreamCount; ++sinkStreamIndex)
{
    post_decode_audio_sink_stream* sinkStream = sinkStreams[sinkStreamIndex];

    if (sinkStream->is_open())
    {
        std::vector<std::pair<MixBuffer, float>> buffersToMixForThisStream;

        for (const std::pair<post_decode_audio_source_stream, MixBuffer>& sourceBufferPair : cachedSourceBuffers)
        {
            float volume;
            if (sinkStream->can_receive_audio_from_source_stream(sourceBufferPair.first, &volume))
            {
                buffersToMixForThisStream.push_back(std::pair<MixBuffer, float>{sourceBufferPair.second, volume});
            }
        }

        if (buffersToMixForThisStream.size() > 0)
        {
            uint32_t mixedBufferByteCount;
            uint8_t* mixedBuffer;
            MixPostDecodeBuffers(buffersToMixForThisStream, &mixedBufferByteCount, &mixedBuffer);
            sinkStream->submit_mixed_buffer(mixedBufferByteCount, mixedBuffer);
        }
    }
}

// Return buffers after mix and submission
for (const std::pair<post_decode_audio_source_stream*, MixBuffer>& cachedSourceBuffer : cachedSourceBuffers)
{
    post_decode_audio_source_stream* sourceStream = cachedSourceBuffer.first;
    void* bufferToReturn = cachedSourceBuffer.second.buffer;
    sourceStream->return_buffer(bufferToReturn);
}

Sleep(audioProcessingPeriodInMilliseconds);
```

## <a name="chat-user-lifetimes"></a>채팅 사용자 수명

실시간 오디오 조작을 사용 하도록 설정 하면 채팅 사용자의 수명을 영향을 줍니다. 경우 `chat_manager::remove_user(chatUserX)` 호출 되는 chat_user chatUserX 참조 하는 모든 오디오 스트림을 제거 될 때까지 chatUserX가 가리키는 개체가 유효한 유지 됩니다. 다음과 같은 경우를 고려할 수 있습니다.

```cpp
// At somepoint a chat user, chatUserX, leaves the game session.
chat_manager::singleton_instance().remove_user(chatUserX);

// chatUserX is still valid, but to avoid further synchronization, prevent non-audio-stream use of chatUserX
chatUserX = nullptr;

// On the audio processing thread...
uint32_t streamStateChangeCount;
game_chat_stream_state_change_array streamStateChanges;
chat_manager::singleton_instance().start_processing_stream_state_changes(&streamStateChangeCount, &streamStateChanges);
for (uint32_t streamStateChangeIndex = 0; streamStateChangeIndex < streamStateChangeCount; ++streamStateChangeIndex)
{
    switch (streamStateChanges[streamStateChangeIndex]->state_change_type)
    {
        ...

        // All of the streams associated with chatUserX will close.
        case Xs::game_chat_2::game_chat_stream_state_change_type::pre_encode_audio_stream_closed:
        {
            CleanupPreEncodeAudioStreamResources(streamStateChanges[streamStateChangeIndex].pre_encode_audio_stream);
            break;
        }

        ...
    }
}
chat_manager::singleton_instance().finish_processing_stream_state_changes(streamStateChanges);

// The next time the app processes stream state changes...
uint32_t streamStateChangeCount;
game_chat_stream_state_change_array streamStateChanges;
chat_manager::singleton_instance().start_processing_stream_state_changes(&streamStateChangeCount, &streamStateChanges);
for (uint32_t streamStateChangeIndex = 0; streamStateChangeIndex < streamStateChangeCount; ++streamStateChangeIndex)
{
    switch (streamStateChanges[streamStateChangeIndex]->state_change_type)
    {
        ...

        case Xs::game_chat_2::game_chat_stream_state_change_type::pre_encode_audio_stream_destroyed:
        {
            uint32_t chatUserCount;
            Xs::game_chat_2::chat_user_array chatUsers;
            streamStateChanges[streamStateChangeIndex].pre_encode_audio_stream->get_users(&chatUserCount, &chatUsers);
            assert(chatUserCount != 0);
            for (uint32_t chatUserIndex = 0; chatUserIndex < chatUserCount; ++chatUserIndex)
            {
                // chat_user objects such as chatUserX will still be valid while the destroyed state change is being processed.
                Log(chatUsers[chatUserIndex]->xbox_user_id());
            }
            break;
        }

        ...
    }
}
chat_manager::singleton_instance().finish_processing_stream_state_changes(streamStateChanges);
// Once the all destroyed state changes have been processed for all streams associated with chatUserX, it's memory will be invalidated.
// Do not call methods on chatUserX, e.g. chatUserX->xbox_user_id()
```
