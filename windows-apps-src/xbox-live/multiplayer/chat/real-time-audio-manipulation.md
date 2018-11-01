---
title: 실시간 오디오 조작
author: KevinAsgari
description: 게임 채팅 2 캡처될 채팅 오디오를 처리 하 고 조작 하는 방법을 알아봅니다.
ms.author: kevinasg
ms.date: 05/10/2018
ms.topic: article
keywords: xbox live, xbox, 게임, uwp, windows 10, 하나는 xbox, 게임 채팅 2, 게임 채팅, 음성 통신, 버퍼 조작, 오디오 조작
ms.localizationpriority: medium
ms.openlocfilehash: cd0a88c5d3ae50cb7dd86585507c951cc061503d
ms.sourcegitcommit: cd00bb829306871e5103db481cf224ea7fb613f0
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/01/2018
ms.locfileid: "5865207"
---
# <a name="real-time-audio-manipulation"></a>실시간 오디오 조작

게임 채팅 2 개발자 검사 하 고 플레이어의 채팅 오디오 데이터를 조작할 채팅 오디오 파이프라인에 스스로 삽입 하는 옵션을 제공 합니다. 이 흥미로운 게임에서 플레이어의 음성 오디오 효과 적용 하는 데 유용할 수 있습니다. 게임 채팅 2의 오디오 조작 파이프라인은 오디오 데이터를 폴링할 수 있는 오디오 스트림 개체를 통해 상호 작용 합니다. 콜백을 사용 하 여, 아니라이 모델 검사 하거나 조작 하기 위해 개발자가 오디오 처리 스레드가 있으며 가장 편리한에 있습니다.

실시간 오디오 조작을 사용 하 여 간단한 연습 다음 항목을 포함 하는 아래 추천는:

1. [오디오 조작 파이프라인 초기화](#initializing-the-audio-manipulation-pipeline)
2. [오디오 스트림 상태 변경 처리](#processing-audio-stream-state-changes)
3. [채팅 오디오를 인코딩할 미리 조작](#manipulating-pre-encode-chat-audio-data)
4. [채팅 오디오 디코드 사후 조작](#manipulating-post-decode-chat-audio-data)
5. [채팅 사용자 수명](#chat-user-lifetimes)

## <a name="initializing-the-audio-manipulation-pipeline"></a>오디오 조작 파이프라인 초기화

기본적으로 게임 채팅 2 실시간 오디오 조작을 사용 하지 않습니다. 실시간 오디오 조작 하도록 앱을 지정 해야 원하는 하는 양식을 오디오 조작의에서 활성화 `chat_manager::initialize()` audioManipulationMode 매개 변수를 설정 하 여 합니다.

현재 다음 오디오 조작 형식 지원 됩니다.

* `game_chat_audio_manipulation_mode_flags::none` -오디오 조작을 사용 하지 않도록 설정 합니다. 기본 구성입니다. 이 모드에서 채팅 오디오를 계속 전송 됩니다.
* `game_chat_audio_manipulation_mode_flags::pre_encode_stream_manipulation` -사용 하면 오디오 조작을 미리 인코딩합니다. 이 모드에서는 인코드 전에 조작 오디오 파이프라인을 통해 로컬 사용자가 생성 된 모든 채팅 오디오를 공급 됩니다. 앱만 채팅 오디오 데이터를 검사 하 고 조작 하지 인 경우에 것은 앱의 책임 인코딩된 고 전송할 수 있도록 게임 채팅 2로 다시 변경 되지 않은 오디오 버퍼를 제출 하려면 여전히 합니다.
* `game_chat_audio_manipulation_mode_flags::post_decode_stream_manipulation` -활성화 후 오디오 조작을 디코딩합니다. 이 모드는 현재 개발 중인를 사용할 수 없습니다.

## <a name="processing-audio-stream-state-changes"></a>오디오 스트림 상태 변경 처리

게임 채팅 2를 통해 오디오 스트림의 상태에 대 한 업데이트를 제공 합니다. `game_chat_stream_state_change` 구조입니다. 이러한 업데이트는 어떤 스트림 업데이트 된와 업데이트 된 어떻게 정보를 저장 합니다. 이러한 업데이트에 대 한 호출을 통해 폴링할 수는 `chat_manager::start_processing_stream_state_changes()` 및 `chat_manager::finish_processing_stream_state_changes()` 메서드 쌍. 이 메서드 쌍을 최신, 대기 중인 오디오 스트림의 모든 상태 업데이트 제공 배열로 `game_chat_stream_state_change` 포인터를 구성 합니다. 앱 배열에 대해 반복 하 고 각 업데이트를 적절 하 게 처리 해야 합니다. 한 번 사용 가능한 모든 `game_chat_stream_state_change` 업데이트 처리 되어, 해당 배열을 통해 Game Chat 2에 다시 전달 되어야 `chat_manager::finish_processing_stream_state_changes()`. 예:

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

## <a name="manipulating-pre-encode-chat-audio-data"></a>채팅 오디오 데이터를 인코딩하고 미리 조작

게임 채팅 2 미리 채팅을 통해 로컬 사용자에 대 한 오디오 데이터를 인코딩하는 데 대 한 액세스를 제공 합니다 `pre_encode_audio_stream` 클래스.

### <a name="stream-lifetime"></a>스트림 수명
새 `pre_encode_audio_stream` 인스턴스가 사용 하도록 앱에 대 한 준비를 통해 배달할는 `game_chat_stream_state_change` 있는 구조 `state_change_type` 필드 설정 `game_chat_stream_state_change_type::pre_encode_audio_stream_created`. 이 스트림 상태 변경을 Game Chat 2를 반환 되 면 오디오 스트림에 대 한 사용할 수 있게 됩니다 미리 오디오 조작 인코드 합니다.

기존 때 `pre_encode_audio_stream` 해질 오디오 조작에 사용 하 여 사용할 수 없는 앱 알려주지 통해는 `game_chat_stream_state_change` 있는 구조 `state_change_type` 필드 설정 `game_chat_stream_state_change_type::pre_encode_audio_stream_closed`. 이 오디오 스트림와 관련 된 리소스를 정리 하려면 앱의 기회입니다. 오디오 스트림에 대 한 없게 일단이 스트림 상태 변경을 Game Chat 2를 반환 되 면 오디오 조작 미리 인코드 합니다.

닫힌 경우 `pre_encode_audio_stream` 모든의 리소스 및 반환 하는지, 스트림 지워집니다. 앱을 통해 알려주지는 `game_chat_stream_state_change` 구조체를의 `state_change_type` 필드 설정 `game_chat_stream_state_change_type::pre_encode_audio_stream_destroyed`. 참조 또는이 스트림에 대 한 포인터를 정리 해야 합니다. 일단이 스트림 상태 변경을 Game Chat 2를 반환 되 면 오디오 스트림 메모리 무효화 됩니다.

### <a name="stream-users"></a>스트림 사용자
스트림과 사용 하 여 연결 된 사용자 목록을 사용 하 여 검사할 수 있습니다 `pre_encode_audio_stream::get_users()`.

### <a name="audio-formats"></a>오디오 형식
게임 채팅 2에서 앱을 검색 하는 버퍼의 오디오 형식을 사용 하 여 검사할 수 있습니다 `pre_encode_audio_stream::get_pre_processed_format()`. 사전 처리 된 오디오 형식 항상 모노 됩니다. 앱 32 비트 부동 소수점, 16 비트 정수 및 32 비트 정수도 표현 되는 데이터를 처리할 수 있어야 합니다.

앱의 인코드 및 사용 하 여 전송에 전송 되는 조작된 버퍼의 오디오 형식 Game Chat 2 알려야 합니다. `pre_encode_audio_stream::set_processed_format()`. 처리 된 형식에 대 한 미리 오디오를 인코딩할 스트림을 이러한 필수 조건을 충족 해야 합니다.

* 모노 형식을 사용 해야 합니다.
* 32 비트 부동 소수점 PCM, 32 비트 정수 PCM, 또는 16 비트 정수 PCM 형식을 형식 이어야 합니다.
* 해당 형식의 샘플 속도 플랫폼을 기반으로 하는 필수 조건을 따라야 합니다. Xbox One 연대 8 kHz, 12 kHz, 16 kHz 및 24 kHz 샘플 속도 지원합니다. Xbox One 및 PC에 대 한 UWP 8 kHz, 12 kHz, kHz 16, 24 z, 32 kHz, 44.1 k h z, 및 샘플 속도 48khz를 지원합니다.

### <a name="retrieving-and-submitting-audio"></a>검색 하 고 오디오를 전송 합니다.
앱이 쿼리할 수 있는 사전 처리를 사용 하 여 사용할 수 있는 버퍼의 수에 대 한 오디오 스트림을 인코딩하 `pre_encode_audio_stream::get_available_buffer_count()`. 앱이 버퍼의 최소 수 없을 때까지 오디오 처리를 지연 하려는 경우이 정보를 사용할 수 있습니다. 10 개만 버퍼는 각 부분에 대기 미리 오디오 스트림 인코딩 및 오디오 지연 하므로 앱 드레이닝는 것이 좋습니다. 대기 시간 오디오 파이프라인을 소개 합니다의 4 개 이상의 버퍼를 큐 전에 오디오 스트림을 미리 인코드 합니다.

앱에서 오디오 버퍼를 검색할 수는 미리 사용 하 여 오디오 스트림 인코딩 `pre_encode_audio_stream::get_next_buffer()`. 새 오디오 버퍼 마다 40ms 한 번 평균 사용할 수 있습니다. 이 메서드에서 반환 된 버퍼를 해제 해야 합니다 `pre_encode_audio_stream::return_buffer()` 작업이 완료 되 면 사용 합니다. 최대 10 대기 또는 unreturned 버퍼에 대 한 지정된 된 시간에 존재할 수는 미리 오디오 스트림에 인코드 합니다. 이 제한에 도달 뛰어난 버퍼 중 일부는 반환 될 때까지 플레이어의 오디오 소스에서 캡처한 새 버퍼 삭제 됩니다.

앱 인코드 및 전송을 사용 하 여 게임 채팅 2로 다시 검사 및 무중단 버퍼를 제출할 수 있습니다 `pre_encode_audio_stream::submit_buffer()`. 게임 채팅 2 버퍼에 전송 되므로 바로 및 위치를-오디오 조작 지원 `pre_encode_audio_stream::submit_buffer()` 반드시 동일한 버퍼에서 검색 되도록 `pre_encode_audio_stream::get_next_buffer()`. 제출 된 이러한 버퍼에 대 한 개인 정보 보호/권한이이 스트림과 연결 된 사용자에 따라 적용 됩니다. 모든 40ms이 스트림에서 오디오의 다음 40ms 인코딩된을 전송 합니다. 오디오 hiccups를 방지 하려면 상수 속도로이 스트림에 지속적으로 들립니다 오디오 버퍼를 제출 해야 합니다.

### <a name="stream-contexts"></a>스트림 컨텍스트
앱 사용자 지정 포인터 크기의 컨텍스트 값을 관리할 수에 사용 하 여 오디오 스트림을 미리 인코드 `pre_encode_audio_stream::set_custom_stream_context()` 및 `pre_encode_audio_stream::custom_stream_context()`. 이러한 사용자 지정 스트림 컨텍스트 간 게임 채팅 2의 오디오 스트림 및 보조 데이터를 만드는 데 도움이 됩니다.: 메타 데이터, 게임 상태를 스트림 합니다.

### <a name="example"></a>예
다음은 사용 하는 방법에 대 한 간소화 된 종단 간 샘플 미리 오디오 처리 한 프레임에서 오디오 스트림을 인코딩하:

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

## <a name="manipulating-post-decode-chat-audio-data"></a>채팅 오디오 데이터를 디코드 사후 조작

게임 채팅 2 사후 채팅을 통해 오디오 데이터를 디코드에 액세스할 수는 `post_decode_audio_source_stream` 및 `post_decode_audio_sink_stream` 사용자가 수 조작할 오디오 원격 사용자를 고유 하 게 채팅 오디오의 각 로컬 수신기에 대 한 클래스입니다.

### <a name="sources-and-sinks"></a>원본 및 싱크가
Pre-encode 파이프라인 달리 모델 처리 후 오디오 데이터를 디코드에 대 한 나누어져 두 클래스: `post_decode_audio_source_stream` 및 `post_decode_audio_sink_stream`. 원격 사용자 로부터 디코딩된 오디오에서 검색할 수 있습니다 `post_decode_audio_source_stream` 개체를 조작 하 고 전송 `post_decode_audio_sink_stream` 개체를 렌더링 합니다. 게임 채팅 2의 간의 통합을 오디오 처리 파이프라인 및 유용한 오디오 미들웨어 사후 디코드 사용할 수 있게 합니다.

### <a name="stream-lifetime"></a>스트림 수명
새 `post_decode_audio_source_stream` 또는 `post_decode_audio_sink_stream` 인스턴스가 사용 하도록 앱에 대 한 준비를 통해 배달할는 `game_chat_stream_state_change` 있는 구조 `state_change_type` 필드 설정 `game_chat_stream_state_change_type::post_decode_audio_source_stream_created` 또는 `game_chat_stream_state_change_type::post_decode_audio_sink_stream_created`각각 합니다. 이 스트림 상태 변경을 Game Chat 2를 반환 되 면 오디오 스트림에 대 한 사용할 수 있게 됩니다 사후 오디오 조작 디코드 합니다.

기존 때 `post_decode_audio_source_stream` 또는 `post_decode_audio_sink_stream` 해질 오디오 조작에 사용 하 여 사용할 수 없는 앱 알려주지 통해는 `game_chat_stream_state_change` 있는 구조 `state_change_type` 필드 설정 `game_chat_stream_state_change_type::post_decode_audio_source_stream_closed` 또는 `game_chat_stream_state_change_type::post_decode_audio_sink_stream`각각 합니다. 이 오디오 스트림와 관련 된 리소스를 정리 하려면 앱의 기회입니다. 오디오 스트림에 대 한 없게 일단이 스트림 상태 변경을 Game Chat 2를 반환 되 면 사후 오디오 조작 디코드 합니다. 원본 스트림에 대 한 자세한 버퍼가 없는 조작에 대 한 대기을 의미 합니다. 싱크가 스트림을 버퍼는 더 이상 렌더링할 전송에 의미 합니다.

닫힌 경우 `post_decode_audio_source_stream` 또는 `post_decode_audio_sink_stream` 모든의 리소스 및 반환 하는지, 스트림 지워집니다. 앱을 통해 알려주지는 `game_chat_stream_state_change` 있는 구조 `state_change_type` 필드 설정 `game_chat_stream_state_change_type::post_decode_audio_source_stream_destroyed` 또는 `game_chat_stream_state_change_type::post_decode_audio_sink_stream_destroyed`각각 합니다. 참조 또는이 스트림에 대 한 포인터를 정리 해야 합니다. 일단이 스트림 상태 변경을 Game Chat 2를 반환 되 면 오디오 스트림 메모리 무효화 됩니다.

### <a name="stream-users"></a>스트림 사용자
Post-decode 소스 스트림과 연결 된 원격 사용자 목록을 사용 하 여 검사할 수 있습니다 `post_decode_audio_source_stream::get_users()`. Post-decode 싱크가 스트림과 연결 된 로컬 사용자 목록을 사용 하 여 검사할 수 있습니다 `post_decode_audio_sink_stream::get_users()`.

### <a name="audio-formats"></a>오디오 형식
게임 채팅 2에서 앱을 검색 하는 버퍼의 오디오 형식을 사용 하 여 검사할 수 있습니다 `post_decode_audio_source_stream::get_pre_processed_format()`. 사전 처리 된 오디오 형식에는 항상 모노, 16 비트 정수 PCM 됩니다.

앱을 사용 하 여 렌더링에 대 한 전송 되는 조작된 버퍼의 오디오 형식 Game Chat 2 알려야 합니다. `post_decode_audio_sink_stream::set_processed_format()`. 처리 된 형식에 대 한 오디오를 사후 디코드 싱크가 스트림을 이러한 필수 조건을 충족 해야 합니다.

* 형식에는 64 개 미만 채널 있어야 합니다.
* 16 비트 정수 (최적의) PCM, 20 비트 정수 (24 비트 컨테이너)에서 PCM, 24 비트 정수 PCM, 32 비트 정수 PCM, 또는 32 비트 부동 소수점 PCM (16 비트 정수 PCM 후 기본 형식) 형식 이어야 합니다. 
* 해당 형식의 샘플 속도 초당 1000 및 200000 샘플 사이 여야 합니다.

### <a name="retrieving-and-submitting-audio"></a>검색 하 고 오디오를 전송 합니다.
앱이 쿼리할 수 사후 처리를 사용 하 여 사용할 수 있는 버퍼의 수에 대 한 원본 오디오 스트림을 디코드 `post_decode_audio_source_stream::get_available_buffer_count()`. 앱이 버퍼의 최소 수 없을 때까지 오디오 처리를 지연 하려는 경우이 정보를 사용할 수 있습니다. 10 개만 버퍼는 각 부분에 대기 사후 소스 오디오 스트림을 디코드 및 오디오 지연 하므로 앱 드레이닝는 것이 좋습니다. 대기 시간 오디오 파이프라인을 소개 합니다의 4 개 이상의 버퍼를 큐 전에 오디오 스트림을 사후 디코드 합니다.

앱에서 오디오 버퍼를 검색할 수는 사후 스트림을 사용 하 여 오디오 원본 디코드 `post_decode_audio_source_stream::get_next_buffer()`. 새 오디오 버퍼 마다 40ms 한 번 평균 사용할 수 있습니다. 이 메서드에서 반환 된 버퍼를 해제 해야 합니다 `post_decode_audio_source_stream::return_buffer()` 작업이 완료 되 면 사용 합니다. 최대 10 대기 또는 unreturned 버퍼에 대 한 지정된 된 시간에 존재할 수는 사후 원본 오디오 스트림을 디코드 합니다. 이 제한에 도달 원격 플레이어에서 새 디코딩된 버퍼 뛰어난 버퍼 중 일부는 반환 될 때까지 삭제 됩니다.

앱을 통해 게임 채팅 2로 다시 검사 및 무중단 버퍼는 사후 싱크가 오디오 스트림을 렌더링에 사용 하 여 디코드 제출할 수 `post_decode_audio_sink_stream::submit_mixed_buffer()`. 게임 채팅 2 버퍼에 전송 되므로 바로 및 위치를-오디오 조작 지원 `post_decode_audio_sink_stream::submit_mixed_buffer()` 반드시 동일한 버퍼에서 검색 되도록 `post_decode_audio_source_stream::get_next_buffer()`. 모든 40ms 다음 40ms이 스트림에서 오디오 렌더링 됩니다. 오디오 hiccups를 방지 하려면 상수 속도로이 스트림에 지속적으로 들립니다 오디오 버퍼를 제출 해야 합니다.

### <a name="privacy-and-mixing"></a>개인 정보 및 믹싱
것은 앱의 책임에서 검색 하는 버퍼를 혼합 post-decode 파이프라인의 소스 싱크가 모델 때문에 `post_decode_audio_source_stream` 개체를 제출 하는 혼합된 버퍼가 `post_decode_audio_sink_stream` 렌더링을 위한 개체입니다. 또한 적절 한 개인 정보 및 권한 적용을 사용 하 여 혼합을 수행 하는 앱의 책임 임을 의미 합니다. 게임 채팅 2는 `post_decode_audio_sink_stream::can_receive_audio_from_source_stream()` 간단 하 고 효율적이 정보를 쿼리할 수 있도록 합니다.

### <a name="chat-indicators"></a>채팅 표시기

오디오를 디코드 사후 조작 채팅 표시기 상태 각 사용자에 대 한 영향을 주지 것입니다. 예를 들어 원격 사용자 음소거 오디오 앱으로 제공 됩니다 하지만 해당 원격 사용자에 대 한 채팅 표시기는 여전히 음소거을 나타냅니다. 원격 사용자 자문해 오디오를 제공 하지만 앱에서 해당 사용자의 오디오를 포함 하는 오디오 믹스를 제공 하는 여부에 관계 없이 말하기 채팅 표시기를 나타냅니다. UI 및 채팅 표시기에 대 한 자세한 내용은 [사용 하 여 게임 채팅 2](using-game-chat-2.md#ui)를 참조 하세요. 추가 앱 관련 제한 사항이 있는 사용자의 오디오 믹스에 확인 하기 위해 사용 하는 경우 것은 게임 채팅 2 제공한 채팅 표시기를 읽고 해당 동일한 제한이 때는 앱의 책임입니다.

### <a name="stream-contexts"></a>스트림 컨텍스트
앱 사용자 지정 포인터 크기의 컨텍스트 값을 관리할 수에 사용 하 여 오디오 스트림을 사후 디코드 합니다 `set_custom_stream_context()` 및 `custom_stream_context()` 메서드. 이러한 사용자 지정 스트림 컨텍스트 간 게임 채팅 2의 오디오 스트림 및 보조 데이터를 만드는 데 도움이 됩니다.: 메타 데이터, 게임 상태를 스트림 합니다.

### <a name="example"></a>예
다음은 사용 하는 방법에 대 한 종단 간 샘플 간소화 된 후 오디오 처리 한 프레임에서 오디오 스트림을 디코드:

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

실시간 오디오 조작에 사용 하도록 설정 하면 채팅 사용자의 수명을 영향을 미칩니다. 하는 경우 `chat_manager::remove_user(chatUserX)` 호출 되는 chat_user chatUserX 참조 하는 모든 오디오 스트림을 소멸 되었다고 될 때까지 chatUserX가 가리키는 개체가 유효한 남아 있습니다. 다음 시나리오를 고려해 야 합니다.

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
