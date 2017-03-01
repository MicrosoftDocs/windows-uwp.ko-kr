---
author: mtoepke
title: "소리 추가"
description: "이 단계에서는 슈팅 게임 샘플에서 XAudio2 API를 사용하여 사운드 재생을 위한 개체를 만드는 방법을 검토합니다."
ms.assetid: aa05efe2-2baa-8b9f-7418-23f5b6cd2266
ms.author: mtoepke
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: "windows 10, uwp, 게임, 사운드"
translationtype: Human Translation
ms.sourcegitcommit: c6b64cff1bbebc8ba69bc6e03d34b69f85e798fc
ms.openlocfilehash: 11553a22274a36094a3e839e8fda648f78cfaaf8
ms.lasthandoff: 02/07/2017

---

# <a name="add-sound"></a>소리 추가


\[ Windows 10의 UWP 앱에 맞게 업데이트되었습니다. Windows 8.x 문서는 [보관](http://go.microsoft.com/fwlink/p/?linkid=619132)을 참조하세요. \]

이 단계에서는 슈팅 게임 샘플에서 [XAudio2](https://msdn.microsoft.com/library/windows/desktop/ee415813) API를 사용하여 사운드 재생을 위한 개체를 만드는 방법을 검토합니다.

## <a name="objective"></a>목표


-   [XAudio2](https://msdn.microsoft.com/library/windows/desktop/ee415813)를 사용하여 사운드 출력을 추가합니다.

게임 샘플에서 오디오 개체와 동작은 다음 세 개의 파일에 정의됩니다.

-   **Audio.h/.cpp**. 이 코드 파일은 사운드 재생을 위한 XAudio2 리소스를 포함하는 **Audio** 개체를 정의합니다. 또한 게임이 일시 중지되거나 비활성화된 경우 오디오 재생을 일시 중단하고 다시 시작하는 메서드를 정의합니다.
-   **MediaReader.h/.cpp**. 이 코드는 로컬 저장소에서 오디오 .wav 파일을 읽는 메서드를 정의합니다.
-   **SoundEffect.h/.cpp**. 이 코드는 게임 내 사운드 재생을 위한 개체를 정의합니다.

## <a name="defining-the-audio-engine"></a>오디오 엔진 정의


게임 샘플이 시작되면 게임에 대한 오디오 리소스를 할당하는 **Audio** 개체를 만듭니다. 이 개체를 선언하는 코드는 다음과 같이 표시됩니다.

```cpp
public:
    Audio();

    void Initialize();
    void CreateDeviceIndependentResources();
    IXAudio2* MusicEngine();
    IXAudio2* SoundEffectEngine();
    void SuspendAudio();
    void ResumeAudio();

protected:
    bool                                m_audioAvailable;
    Microsoft::WRL::ComPtr<IXAudio2>    m_musicEngine;
    Microsoft::WRL::ComPtr<IXAudio2>    m_soundEffectEngine;
    IXAudio2MasteringVoice*             m_musicMasteringVoice;
    IXAudio2MasteringVoice*             m_soundEffectMasteringVoice;
};
```

**Audio::MusicEngine** 및 **Audio::SoundEffectEngine** 메서드는 각 오디오 유형에 대해 마스터링 음성을 정의하는 [**IXAudio2**](https://msdn.microsoft.com/library/windows/desktop/ee415908) 개체에 대한 참조를 반환합니다. 마스터링 음성은 재생에 사용되는 오디오 디바이스입니다. 사운드 데이터 버퍼는 마스터링 음성으로 직접 전송할 수 없지만 다른 유형의 음성으로 전송된 데이터를 마스터링 음성으로 전송하여 들을 수는 있습니다.

## <a name="initializing-the-audio-resources"></a>오디오 리소스 초기화


샘플에서는 [**XAudio2Create**](https://msdn.microsoft.com/library/windows/desktop/ee419212)를 호출하여 음악 및 사운드 효과 엔진에 대한 [**IXAudio2**](https://msdn.microsoft.com/library/windows/desktop/ee415908) 개체를 초기화합니다. 엔진이 인스턴스화되면 다음과 같이 [**IXAudio2::CreateMasteringVoice**](https://msdn.microsoft.com/library/windows/desktop/hh405048)를 호출하여 각각에 대한 마스터링 음성을 만듭니다.

```cpp

void Audio::CreateDeviceIndependentResources()
{
    UINT32 flags = 0;

    DX::ThrowIfFailed(
        XAudio2Create(&m_musicEngine, flags)
        );

    HRESULT hr = m_musicEngine->CreateMasteringVoice(&m_musicMasteringVoice);
    if (FAILED(hr))
    {
        // Unable to create an audio device
        m_audioAvailable = false;
        return;
    }

    DX::ThrowIfFailed(
        XAudio2Create(&m_soundEffectEngine, flags)
        );

    DX::ThrowIfFailed(
        m_soundEffectEngine->CreateMasteringVoice(&m_soundEffectMasteringVoice)
        );

    m_audioAvailable = true;
}
```

음악 또는 사운드 효과 오디오 파일이 로드되면 이 메서드는 마스터링 음성에서 [**IXAudio2::CreateSourceVoice**](https://msdn.microsoft.com/library/windows/desktop/ee418607)를 호출하고 재생을 위해 원본 음성의 인스턴스를 만듭니다. 게임 샘플의 오디오 파일 로드 방법에 대한 검토를 완료하는 즉시 이 작업에 대한 코드를 살펴보겠습니다.

## <a name="reading-an-audio-file"></a>오디오 파일 읽기


게임 샘플에서 오디오 형식 파일을 읽는 코드는 **MediaReader.cpp**에 정의되어 있습니다. 인코드된 .wav 오디오 파일로 읽는 특정 메서드인 **MediaReader::LoadMedia**는 다음과 같이 표시됩니다.

```cpp
Platform::Array<byte>^  MediaReader::LoadMedia(_In_ Platform::String^ filename)
{
    DX::ThrowIfFailed(
        MFStartup(MF_VERSION)
        );

    ComPtr<IMFSourceReader> reader;
    DX::ThrowIfFailed(
        MFCreateSourceReaderFromURL(
            Platform::String::Concat(m_installedLocationPath, filename)->Data(),
            nullptr,
            &reader
            )
        );

    // Set the decoded output format as PCM
    // XAudio2 on Windows can process PCM and ADPCM-encoded buffers.
    // When using MediaFoundation, this sample always decodes into PCM.
    Microsoft::WRL::ComPtr<IMFMediaType> mediaType;
    DX::ThrowIfFailed(
        MFCreateMediaType(&mediaType)
        );

    DX::ThrowIfFailed(
        mediaType->SetGUID(MF_MT_MAJOR_TYPE, MFMediaType_Audio)
        );

    DX::ThrowIfFailed(
        mediaType->SetGUID(MF_MT_SUBTYPE, MFAudioFormat_PCM)
        );

    DX::ThrowIfFailed(
        reader->SetCurrentMediaType(MF_SOURCE_READER_FIRST_AUDIO_STREAM, 0, mediaType.Get())
        );

    // Get the complete WAVEFORMAT from the Media Type.
    Microsoft::WRL::ComPtr<IMFMediaType> outputMediaType;
    DX::ThrowIfFailed(
        reader->GetCurrentMediaType(MF_SOURCE_READER_FIRST_AUDIO_STREAM, &outputMediaType)
        );

    UINT32 size = 0;
    WAVEFORMATEX* waveFormat;
    DX::ThrowIfFailed(
        MFCreateWaveFormatExFromMFMediaType(outputMediaType.Get(), &waveFormat, &size)
        );

    CopyMemory(&m_waveFormat, waveFormat, sizeof(m_waveFormat));
    CoTaskMemFree(waveFormat);

    // Get the total length of the stream in bytes.
    PROPVARIANT propVariant;
    DX::ThrowIfFailed(
        reader->GetPresentationAttribute(MF_SOURCE_READER_MEDIASOURCE, MF_PD_DURATION, &propVariant)
        );
    LONGLONG duration = propVariant.uhVal.QuadPart;
    unsigned int maxStreamLengthInBytes;

    double durationInSeconds = (duration / static_cast<double>(10000 * 1000));
    maxStreamLengthInBytes = static_cast<unsigned int>(durationInSeconds * m_waveFormat.nAvgBytesPerSec);

    // Make the length a multiple of 4 bytes.
    maxStreamLengthInBytes = (maxStreamLengthInBytes + 3) / 4 * 4;

    Platform::Array<byte>^ fileData = ref new Platform::Array<byte>(maxStreamLengthInBytes);

    ComPtr<IMFSample> sample;
    ComPtr<IMFMediaBuffer> mediaBuffer;
    DWORD flags = 0;

    int positionInData = 0;
    bool done = false;
    while (!done)
    {
        DX::ThrowIfFailed(
            reader->ReadSample(MF_SOURCE_READER_FIRST_AUDIO_STREAM, 0, nullptr, &flags, nullptr, &sample)
            );

        if (sample != nullptr)
        {
            DX::ThrowIfFailed(
                sample->ConvertToContiguousBuffer(&mediaBuffer)
                );

            BYTE *audioData = nullptr;
            DWORD sampleBufferLength = 0;
            DX::ThrowIfFailed(
                mediaBuffer->Lock(&audioData, nullptr, &sampleBufferLength)
                );

            for (DWORD i = 0; i < sampleBufferLength; i++)
            {
                fileData[positionInData++] = audioData[i];
            }
        }
        if (flags & MF_SOURCE_READERF_ENDOFSTREAM)
        {
            done = true;
        }
    }

    // Fix up the array size on match the actual length.
    Platform::Array<byte>^ realfileData = ref new Platform::Array<byte>((positionInData + 3) / 4 * 4);
    memcpy(realfileData->Data, fileData->Data, positionInData);
    return realfileData;
}
```

이 메서드는 [Media Foundation](https://msdn.microsoft.com/library/windows/desktop/ms694197) API를 사용하여 .wav 오디오 파일을 PCM(Pulse Code Modulation) 버퍼로 읽습니다.

1.  [**MFCreateSourceReaderFromURL**](https://msdn.microsoft.com/library/windows/desktop/dd388110)을 호출하여 미디어 원본 뷰어([**IMFSourceReader**](https://msdn.microsoft.com/library/windows/desktop/dd374655)) 개체를 만듭니다.
2.  [**MFCreateMediaType**](https://msdn.microsoft.com/library/windows/desktop/ms693861)을 호출하여 오디오 파일의 디코딩을 위한 미디어 유형([**IMFMediaType**](https://msdn.microsoft.com/library/windows/desktop/ms704850))을 만듭니다. 이 메서드는 디코드된 출력이 XAudio2에서 사용할 수 있는 오디오 유형인 PCM 오디오가 되도록 지정합니다.
3.  [**IMFSourceReader::SetCurrentMediaType**](https://msdn.microsoft.com/library/windows/desktop/bb970432)을 호출하여 뷰어의 디코드된 출력 미디어 유형을 설정합니다.
4.  [**WAVEFORMATEX**](https://msdn.microsoft.com/library/windows/hardware/ff538799) 버퍼를 만들고 호출 결과를 [**IMFMediaType**](https://msdn.microsoft.com/library/windows/desktop/ms704850) 개체의 [**IMFMediaType::MFCreateWaveFormatExFromMFMediaType**](https://msdn.microsoft.com/library/windows/desktop/ms702177)에 복사합니다. 이렇게 하여 로드된 후 오디오 파일을 보유하는 버퍼의 형식을 지정합니다.
5.  [**IMFSourceReader::GetPresentationAttribute**](https://msdn.microsoft.com/library/windows/desktop/dd374662)를 호출하여 오디오 스트림의 기간(초)을 가져온 다음 해당 기간을 바이트로 변환합니다.
6.  [**IMFSourceReader::ReadSample**](https://msdn.microsoft.com/library/windows/desktop/dd374665)을 호출하여 오디오 파일을 스트림으로 읽습니다.
7.  메서드에서 반환한 배열에 오디오 샘플 버퍼의 내용을 복사합니다.

**SoundEffect::Initialize**에서 가장 중요한 점은 마스터링 음성에서 원본 음성 개체인 **m\_sourceVoice**를 만드는 것입니다. **MediaReader::LoadMedia**에서 가져온 사운드 데이터 버퍼의 실제 재생을 위해 원본 음성을 사용합니다.

샘플 게임에서는 다음과 같이 **SoundEffect** 개체를 초기화할 때 이 메서드를 호출합니다.

```cpp
void SoundEffect::Initialize(
    _In_ IXAudio2 *masteringEngine,
    _In_ WAVEFORMATEX *sourceFormat,
    _In_ Platform::Array<byte>^ soundData)
{
    m_soundData = soundData;

    if (masteringEngine == nullptr)
    {
        // Audio is not available, so return.
        m_audioAvailable = false;
        return;
    }

    // Create and reuse a single source voice for the single sound effect in this sample.
    DX::ThrowIfFailed(
        masteringEngine->CreateSourceVoice(
            &m_sourceVoice,
            sourceFormat
            )
        );
    m_audioAvailable = true;
}
```

이 메서드는 다음과 같이 호출 결과를 **Audio::SoundEffectEngine**(또는 **Audio::MusicEngine**), **MediaReader::GetOutputWaveFormatEx** 및 **MediaReader::LoadMedia** 호출로 반환된 버퍼에 전달합니다.

```cpp
MediaReader^ mediaReader = ref new MediaReader;
auto targetHitSound = mediaReader->LoadMedia("hit.wav");
```

```cpp
myTarget->HitSound(ref new SoundEffect());
myTarget->HitSound()->Initialize(
                m_audioController->SoundEffectEngine(),
                mediaReader->GetOutputWaveFormatEx(),
                targetHitSound);
```

**SoundEffect::Initialize**는 주 게임 개체를 초기화하는 **Simple3DGame:Initialize** 메서드에서 호출됩니다.

샘플 게임에서 오디오 파일을 메모리에 저장하고 있으므로 게임 재생 중 오디오 파일을 재생하는 방법을 살펴보겠습니다.

## <a name="playing-back-an-audio-file"></a>오디오 파일 재생


```cpp
void SoundEffect::PlaySound(_In_ float volume)
{
    XAUDIO2_BUFFER buffer = {0};
    XAUDIO2_VOICE_STATE state = {0};

    if (!m_audioAvailable)
    {
        // Audio is not available, so just return.
        return;
    }

    // Interrupt sound effect if currently playing.
    DX::ThrowIfFailed(
        m_sourceVoice->Stop()
        );
    DX::ThrowIfFailed(
        m_sourceVoice->FlushSourceBuffers()
        );

    // Queue in-memory buffer for playback and start the voice.
    buffer.AudioBytes = m_soundData->Length;
    buffer.pAudioData = m_soundData->Data;
    buffer.Flags = XAUDIO2_END_OF_STREAM;

    m_sourceVoice->SetVolume(volume);
    DX::ThrowIfFailed(
        m_sourceVoice->SubmitSourceBuffer(&buffer)
        );
    DX::ThrowIfFailed(
        m_sourceVoice->Start()
        );
}
```

사운드를 재생하기 위해 이 메서드는 원본 음성 개체 **m\_sourceVoice**를 사용하여 사운드 데이터 버퍼 **m\_soundData**의 재생을 시작합니다. 이 메서드는 사운드 데이터 버퍼에 대한 참조를 제공하는 [**XAUDIO2\_BUFFER**](https://msdn.microsoft.com/library/windows/desktop/ee419228)를 만든 다음 [**IXAudio2SourceVoice::SubmitSourceBuffer**](https://msdn.microsoft.com/library/windows/desktop/ee418473)를 호출하여 전송합니다. 사운드 데이터가 대기하고 있으면 **SoundEffect::PlaySound**에서 [**IXAudio2SourceVoice::Start**](https://msdn.microsoft.com/library/windows/desktop/ee418471)를 호출하여 재생을 시작합니다.

이제 탄약과 타겟이 충돌할 때마다 **SoundEffect::PlaySound**를 호출하여 노이즈를 재생합니다.

## <a name="next-steps"></a>다음 단계


UWP(유니버설 Windows 플랫폼) DirectX 게임 개발에 대해 간략하게 둘러보았습니다. 이제 직접 만든 Windows 8용 게임에 최고의 환경을 제공하기 위해 수행해야 할 작업에 대해 알게 되었습니다. 게임은 다양한 Windows 8 장치 및 플랫폼에서 재생할 수 있으므로 그래픽, 컨트롤, 사용자 인터페이스 및 오디오와 같은 구성 요소를 최대한 광범위한 구성 집합으로 설계해야 합니다.

이러한 문서에 제공된 게임 샘플을 수정하는 방법에 대한 자세한 내용은 [게임 샘플 확장](tutorial-resources.md)을 참조하세요.

## <a name="complete-sample-code-for-this-section"></a>이 섹션에 대한 전체 샘플 코드


Audio.h

```cpp
//// THIS CODE AND INFORMATION IS PROVIDED "AS IS" WITHOUT WARRANTY OF
//// ANY KIND, EITHER EXPRESSED OR IMPLIED, INCLUDING BUT NOT LIMITED TO
//// THE IMPLIED WARRANTIES OF MERCHANTABILITY AND/OR FITNESS FOR A
//// PARTICULAR PURPOSE.
////
//// Copyright (c) Microsoft Corporation. All rights reserved

#pragma once

ref class Audio
{
public:
    Audio();

    void Initialize();
    void CreateDeviceIndependentResources();
    IXAudio2* MusicEngine();
    IXAudio2* SoundEffectEngine();
    void SuspendAudio();
    void ResumeAudio();

protected:
    bool                                m_audioAvailable;
    Microsoft::WRL::ComPtr<IXAudio2>    m_musicEngine;
    Microsoft::WRL::ComPtr<IXAudio2>    m_soundEffectEngine;
    IXAudio2MasteringVoice*             m_musicMasteringVoice;
    IXAudio2MasteringVoice*             m_soundEffectMasteringVoice;
};
```

Audio.cpp

```cpp
//// THIS CODE AND INFORMATION IS PROVIDED "AS IS" WITHOUT WARRANTY OF
//// ANY KIND, EITHER EXPRESSED OR IMPLIED, INCLUDING BUT NOT LIMITED TO
//// THE IMPLIED WARRANTIES OF MERCHANTABILITY AND/OR FITNESS FOR A
//// PARTICULAR PURPOSE.
////
//// Copyright (c) Microsoft Corporation. All rights reserved

#include "pch.h"
#include "Audio.h"
#include "DirectXSample.h"

using namespace Microsoft::WRL;
using namespace Windows::Foundation;
using namespace Windows::UI::Core;
using namespace Windows::Graphics::Display;

Audio::Audio():
    m_audioAvailable(false)
{
}

void Audio::Initialize()
{
}

void Audio::CreateDeviceIndependentResources()
{
    UINT32 flags = 0;

    DX::ThrowIfFailed(
        XAudio2Create(&m_musicEngine, flags)
        );

#if defined(_DEBUG)
    XAUDIO2_DEBUG_CONFIGURATION debugConfiguration = {0};
    debugConfiguration.BreakMask = XAUDIO2_LOG_ERRORS;
    debugConfiguration.TraceMask = XAUDIO2_LOG_ERRORS;
    m_musicEngine->SetDebugConfiguration(&debugConfiguration);
#endif


    HRESULT hr = m_musicEngine->CreateMasteringVoice(&m_musicMasteringVoice);
    if (FAILED(hr))
    {
        // Unable to create an audio device
        m_audioAvailable = false;
        return;
    }

    DX::ThrowIfFailed(
        XAudio2Create(&m_soundEffectEngine, flags)
        );

#if defined(_DEBUG)
    m_soundEffectEngine->SetDebugConfiguration(&debugConfiguration);
#endif

    DX::ThrowIfFailed(
        m_soundEffectEngine->CreateMasteringVoice(&m_soundEffectMasteringVoice)
        );

    m_audioAvailable = true;
}

IXAudio2* Audio::MusicEngine()
{
    return m_musicEngine.Get();
}

IXAudio2* Audio::SoundEffectEngine()
{
    return m_soundEffectEngine.Get();
}

void Audio::SuspendAudio()
{
    if (m_audioAvailable)
    {
        m_musicEngine->StopEngine();
        m_soundEffectEngine->StopEngine();
    }
}

void Audio::ResumeAudio()
{
    if (m_audioAvailable)
    {
        DX::ThrowIfFailed(m_musicEngine->StartEngine());
        DX::ThrowIfFailed(m_soundEffectEngine->StartEngine());
    }
}
```

SoundEffect.h

```cpp
//// THIS CODE AND INFORMATION IS PROVIDED "AS IS" WITHOUT WARRANTY OF
//// ANY KIND, EITHER EXPRESSED OR IMPLIED, INCLUDING BUT NOT LIMITED TO
//// THE IMPLIED WARRANTIES OF MERCHANTABILITY AND/OR FITNESS FOR A
//// PARTICULAR PURPOSE.
////
//// Copyright (c) Microsoft Corporation. All rights reserved

#pragma once

ref class SoundEffect
{
public:
    SoundEffect();

    void Initialize(
        _In_ IXAudio2*              masteringEngine,
        _In_ WAVEFORMATEX*          sourceFormat,
        _In_ Platform::Array<byte>^ soundData
        );

    void PlaySound(_In_ float volume);

protected:
    bool                    m_audioAvailable;
    IXAudio2SourceVoice*    m_sourceVoice;
    Platform::Array<byte>^  m_soundData;
};
```

SoundEffect.cpp

```cpp
//// THIS CODE AND INFORMATION IS PROVIDED "AS IS" WITHOUT WARRANTY OF
//// ANY KIND, EITHER EXPRESSED OR IMPLIED, INCLUDING BUT NOT LIMITED TO
//// THE IMPLIED WARRANTIES OF MERCHANTABILITY AND/OR FITNESS FOR A
//// PARTICULAR PURPOSE.
////
//// Copyright (c) Microsoft Corporation. All rights reserved

#include "pch.h"
#include "SoundEffect.h"
#include "DirectXSample.h"

SoundEffect::SoundEffect():
    m_audioAvailable(false)
{
}
//----------------------------------------------------------------------
void SoundEffect::Initialize(
    _In_ IXAudio2 *masteringEngine,
    _In_ WAVEFORMATEX *sourceFormat,
    _In_ Platform::Array<byte>^ soundData)
{
    m_soundData = soundData;

    if (masteringEngine == nullptr)
    {
        // Audio is not available, so return.
        m_audioAvailable = false;
        return;
    }

    // Create and reuse a single source voice for the single sound effect in this sample.
    DX::ThrowIfFailed(
        masteringEngine->CreateSourceVoice(
            &m_sourceVoice,
            sourceFormat
            )
        );
    m_audioAvailable = true;
}
//----------------------------------------------------------------------
void SoundEffect::PlaySound(_In_ float volume)
{
    XAUDIO2_BUFFER buffer = {0};
    XAUDIO2_VOICE_STATE state = {0};

    if (!m_audioAvailable)
    {
        // Audio is not available, so just return.
        return;
    }

    // Interrupt sound effect if currently playing.
    DX::ThrowIfFailed(
        m_sourceVoice->Stop()
        );
    DX::ThrowIfFailed(
        m_sourceVoice->FlushSourceBuffers()
        );

    // Queue in-memory buffer for playback and start the voice.
    buffer.AudioBytes = m_soundData->Length;
    buffer.pAudioData = m_soundData->Data;
    buffer.Flags = XAUDIO2_END_OF_STREAM;

    m_sourceVoice->SetVolume(volume);
    DX::ThrowIfFailed(
        m_sourceVoice->SubmitSourceBuffer(&buffer)
        );
    DX::ThrowIfFailed(
        m_sourceVoice->Start()
        );
}
```

 

 





