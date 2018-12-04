---
title: 소리 추가
description: XAudio2 Api를 사용 하 여 재생 게임 음악 및 사운드 효과를 간단한 사운드 엔진을 개발 합니다.
ms.assetid: aa05efe2-2baa-8b9f-7418-23f5b6cd2266
ms.date: 10/24/2017
ms.topic: article
keywords: windows 10, uwp, 게임, 사운드
ms.localizationpriority: medium
ms.openlocfilehash: 94044e3d10df15cb1cb256d86ced798395e6af6f
ms.sourcegitcommit: b4c502d69a13340f6e3c887aa3c26ef2aeee9cee
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 12/03/2018
ms.locfileid: "8485585"
---
# <a name="add-sound"></a>소리 추가

이 항목에서는 [XAudio2](https://msdn.microsoft.com/library/windows/desktop/ee415813) Api를 사용 하 여 간단한 사운드 엔진을 만듭니다. __XAudio2__을 처음 접하는 경우 [오디오 개념](#audio-concepts)에서 간단한 소개를 포함 되어 있습니다.

>[!Note]
>이 샘플의 최신 게임 코드를 다운로드하지 않은 경우 [Direct3D 게임 샘플](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/Simple3DGameDX)로 이동합니다. 이 샘플은 UWP 기능 샘플의 큰 컬렉션의 일부입니다. 샘플을 다운로드하는 방법에 대한 지침은 [GitHub에서 UWP 샘플 가져오기](https://docs.microsoft.com/windows/uwp/get-started/get-uwp-app-samples)를 참조하세요.

## <a name="objective"></a>목표

[XAudio2](https://msdn.microsoft.com/library/windows/desktop/ee415813)를 사용 하 여 샘플 게임에 소리를 추가 합니다.

## <a name="define-the-audio-engine"></a>오디오 엔진 정의

게임 샘플에서 오디오 개체와 동작은 다음 세 개의 파일에 정의됩니다.

* __[Audio.h](#audioh)/.cpp__: 사운드 재생을 위한 __XAudio2__ 리소스를 포함 하는 __오디오__ 개체를 정의 합니다. 또한 게임이 일시 중지되거나 비활성화된 경우 오디오 재생을 일시 중단하고 다시 시작하는 메서드를 정의합니다.
* __ [MediaReader.h](#mediareaderh)/.cpp__: 로컬 저장소에서 오디오.wav 파일을 읽는 메서드를 정의 합니다.
* __ [SoundEffect.h](#soundeffecth)/.cpp__: 게임 내 사운드 재생을 위한 개체를 정의 합니다.

## <a name="overview"></a>개요

오디오 재생을 게임에 대 한 설정에 세 가지 주요 부분이 있습니다.

1. [만들기 및 오디오 리소스 초기화](#create-and-initialize-the-audio-resources)
2. [오디오 파일 로드](#load-audio-file)
3. [소리 개체에 연결](#associate-sound-to-object)

모든 정의 된 [simple3dgame:: initialize](#simple3dgameinitialize-method) 메서드에서 합니다. 이제 먼저이 메서드를 검사 하 고 다음의 각 섹션에서 자세히 살펴볼 합니다.

를 설정한 후 재생 소리 효과 트리거하는 방법에 알아봅니다 했습니다. 자세한 내용은 [소리를 재생](#play-the-sound)하려면 이동 합니다.

### <a name="simple3dgameinitialize-method"></a>Simple3dgame:: initialize 메서드

__Simple3dgame:: initialize__, 여기서 __m\_controller__ 및 __m\_renderer__ 도 초기화에서는 오디오 엔진을 설정 하 고 소리를 재생 하려면 준비 합니다.

 * [Audio](#audioh) 클래스의 인스턴스는 __m\_audioController__를 만듭니다.
 * [Audio::CreateDeviceIndependentResources](#audiocreatedeviceindependentresources-method) 메서드를 사용 하 여 필요한 오디오 리소스를 만듭니다. 다음은, 두 개의 __XAudio2__ 개체 &mdash; 각각에 대 한 마스터 음성을 음악 엔진 개체 및 사운드 엔진 개체를 생성 합니다. 게임에 대 한 배경 음악을 재생 하는 음악 엔진 개체를 사용할 수 있습니다. 게임에 소리 효과 재생 소리 엔진을 사용할 수 있습니다. 자세한 내용은 참조 [만들기 및 오디오 리소스 초기화](#create-and-initialize-the-audio-resources)합니다.
 * [MediaReader](#mediareaderh) 클래스의 인스턴스는 __mediaReader__를 만듭니다. [MediaReader](#mediareaderh), [SoundEffect](#soundeffecth) 클래스에 대 한 도우미 클래스는 파일 위치에서 동기적으로 작은 오디오 파일을 읽고 고 바이트 배열과 사운드 데이터를 반환 합니다.
 * [Mediareader:: Loadmedia](#mediareaderloadmedia-method) 를 사용 하 여 해당 위치에서 사운드 파일을 로드 하 고 로드.wav 사운드 데이터를 저장할 __targetHitSound__ 변수를 만듭니다. 자세한 내용은 [오디오 파일 로드를](#load-audio)참조 하세요. 

사운드 효과 게임 개체와 연결 됩니다. 따라서 해당 게임 개체를 사용 하 여 충돌이 발생 하면 재생 되도록 소리 효과 트리거합니다. 이 게임 샘플 (어떻게 사용 하 여 대상으로 촬영을) 탄약 및 대상에 대 한 사운드 효과 했습니다. 
    
* __GameObject__ 클래스에서 소리 효과 개체에 연결 하는 데 사용 되는 __HitSound__ 속성이 있습니다.
* [SoundEffect](#soundeffecth) 클래스의 새 인스턴스를 만들고 초기화 합니다. 초기화 하는 동안 소리 효과 대 한 원본 음성이 만들어집니다. 
* 이 클래스는 [오디오](#audioh) 클래스에서 제공 하는 마스터링 음성을 사용 하 여 소리를 재생 합니다. 사운드 데이터는 [MediaReader](#mediareaderh) 클래스를 사용 하 여 파일 위치에서 읽습니다. 자세한 내용은 [연결 소리 개체를](#associate-sound-to-object)참조 하세요.

>[!Note]
>소리를 재생 하려면 실제 트리거 동작 및 이러한 게임 개체의 충돌에 의해 결정 됩니다. 따라서 실제로 이러한 소리를 재생에 대 한 호출 [Simple3DGame::UpdateDynamics](#simple3dgameupdatedynamics-method) 메서드에서 정의 됩니다. 자세한 내용은 [소리를 재생](#play-the-sound)하려면 이동 합니다.

```cpp
void Simple3DGame::Initialize(
    _In_ MoveLookController^ controller,
    _In_ GameRenderer^ renderer
    )
{
    // ...
    // Create a new Audio class object.
    m_audioController = ref new Audio;

    // Create the audio resources needed.
    // Two XAudio2 objects are created - one for music engine,
    // the other for sound engine. A mastering voice is also
    // created for each of the objects.
    m_audioController->CreateDeviceIndependentResources();

    m_ammo = std::vector<Sphere^>(GameConstants::MaxAmmo);

    // ..
    // Create a media reader which is used to read audio files from its file location.
    MediaReader^ mediaReader = ref new MediaReader;
    auto targetHitSound = mediaReader->LoadMedia("Assets\\hit.wav");

    // Instantiate the targets for use in the game.
    // Each target has a different initial position, size, and orientation.
    // But share a common set of material properties.
    for (int a = 1; a < GameConstants::MaxTargets; a++)
    {
        // ...
        // Create a new sound effect object and associate it
        // with the game object's (target) HitSound property.
        target->HitSound(ref new SoundEffect());

        // Initialize the sound effect object with
        // the sound effect engine, format of the audio wave, and audio data
        // During initialization, source voice of this sound effect is also created.
        target->HitSound()->Initialize(
            m_audioController->SoundEffectEngine(),
            mediaReader->GetOutputWaveFormatEx(),
            targetHitSound
            );
        // ...
    }

    // Instantiate a set of spheres to be used as ammunition for the game
    // and set the material properties of the spheres.
    auto ammoHitSound = mediaReader->LoadMedia("Assets\\bounce.wav");

    for (int a = 0; a < GameConstants::MaxAmmo; a++)
    {
        m_ammo[a] = ref new Sphere;
        m_ammo[a]->Radius(GameConstants::AmmoRadius);
        m_ammo[a]->HitSound(ref new SoundEffect());
        m_ammo[a]->HitSound()->Initialize(
            m_audioController->SoundEffectEngine(),
            mediaReader->GetOutputWaveFormatEx(),
            ammoHitSound
            );
        m_ammo[a]->Active(false);
        m_renderObjects.push_back(m_ammo[a]);
    }
    // ...
}
```

## <a name="create-and-initialize-the-audio-resources"></a>만들기 및 오디오 리소스 초기화

* [XAudio2Create](https://msdn.microsoft.com/library/windows/desktop/ee419212), XAudio2 API를 사용 하 여 음악 및 사운드 효과 엔진을 정의 하는 두 개의 새로운 XAudio2 개체를 만듭니다. 이 메서드는 오디오 처리 스레드, 음성 그래프 모든 오디오 엔진 상태를 관리 하는 개체의 [IXAudio2](https://msdn.microsoft.com/library/windows/desktop/ee415908) 인터페이스에 대 한 포인터를 반환 합니다.
* 엔진이 후 인스턴스화된, [ixaudio2:: Createmasteringvoice](https://msdn.microsoft.com/library/windows/desktop/hh405048) 를 사용 하 여 사운드 엔진 개체 각각에 대 한 마스터 음성을 만듭니다.

자세한 내용을 보려면 [하는 방법: XAudio2 초기화](https://msdn.microsoft.com/library/windows/desktop/ee415779.aspx)합니다.

### <a name="audiocreatedeviceindependentresources-method"></a>Audio::CreateDeviceIndependentResources 메서드

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

## <a name="load-audio-file"></a>오디오 파일 로드

게임 샘플에서 오디오 형식 파일을 읽는 코드는 [MediaReader.h](#mediareaderh)/cpp__에 정의 되어 있습니다.  인코드된.wav 오디오 파일을 읽고, [mediareader:: Loadmedia](#mediareaderloadmedia-method), 입력된 매개 변수로.wav 파일 이름을 전달 하 여 호출 합니다.

### <a name="mediareaderloadmedia-method"></a>Mediareader:: Loadmedia 메서드

이 메서드는 [Media Foundation](https://msdn.microsoft.com/library/windows/desktop/ms694197) API를 사용하여 .wav 오디오 파일을 PCM(Pulse Code Modulation) 버퍼로 읽습니다.

#### <a name="set-up-the-source-reader"></a>원본 뷰어 설정

1. [MFCreateSourceReaderFromURL](https://msdn.microsoft.com/library/windows/desktop/dd388110) 를 사용 하 여 미디어 원본 뷰어 ([IMFSourceReader](https://msdn.microsoft.com/library/windows/desktop/dd374655))를 만듭니다.
2. [MFCreateMediaType](https://msdn.microsoft.com/library/windows/desktop/ms693861) 를 사용 하 여 미디어 유형 ([IMFMediaType](https://msdn.microsoft.com/library/windows/desktop/ms704850)) 개체 (_mediaType_)를 만듭니다. 미디어 형식에 대 한 설명을 나타냅니다. 
3. 디코드된 출력이 _mediaType_의 __XAudio2__ 사용할 수 있는 오디오 유형인 PCM 오디오가 되도록 지정 합니다.
4. [:: Setcurrentmediatype](https://msdn.microsoft.com/library/windows/desktop/dd374667.aspx)호출 하 여 원본 뷰어 디코딩된 출력 미디어 유형을 설정 합니다.

원본 뷰어를 사용 하는 이유에 대 한 자세한 내용은 [소스 판독기](https://msdn.microsoft.com/library/windows/desktop/dd940436.aspx)으로 이동 합니다.

#### <a name="describe-the-data-format-of-the-audio-stream"></a>오디오 스트림의 데이터 형식을 설명합니다

1. [Imfsourcereader:: Getcurrentmediatype](https://msdn.microsoft.com/library/windows/desktop/dd374660) 를 사용 하 여 스트림에 대 한 현재 미디어 형식을 가져옵니다.
2. [IMFMediaType::MFCreateWaveFormatExFromMFMediaType](https://msdn.microsoft.com/library/windows/desktop/ms702177) 를 사용 하 여 이전 작업의 결과 사용 하 여 입력으로 [WAVEFORMATEX](https://msdn.microsoft.com/library/windows/hardware/ff538799) 버퍼로 현재 오디오 미디어 유형으로 변환 합니다. 이 구조에는 오디오를 로드 한 후에 사용 되는 진행할 오디오 스트림의 데이터 형식을 지정 합니다. 

__WAVEFORMATEX__ 형식은 PCM 버퍼를 설명 하기 위해 사용할 수 있습니다. [WAVEFORMATEXTENSIBLE](https://msdn.microsoft.com/library/windows/hardware/ff538802) 구조와 비교할 때만 사용할 수 오디오 진행할 형식 중 일부를 설명 하 합니다. __WAVEFORMATEX__ 와 __WAVEFORMATEXTENSIBLE__간의 차이점에 대 한 자세한 내용은 [확장 가능한 진행할 형식 설명자](https://docs.microsoft.com/windows-hardware/drivers/audio/extensible-wave-format-descriptors)를 참조 하세요.

#### <a name="read-the-audio-stream"></a>오디오 스트림 읽기

1.  [Imfsourcereader:: Getpresentationattribute](https://msdn.microsoft.com/library/windows/desktop/dd374662) 및 변환 바이트로 기간을 호출 하 여 오디오 스트림의 초 기간을 가져옵니다.
2.  [:: Readsample](https://msdn.microsoft.com/library/windows/desktop/dd374665)를 호출 하 여에서 오디오 파일을 스트림으로 읽습니다. __ReadSample__ 미디어 소스에서 다음 샘플을 읽습니다.
3.  [IMFSample::ConvertToContiguousBuffer](https://msdn.microsoft.com/library/windows/desktop/ms698917.aspx) 를 사용 하 여 (_샘플_) 오디오 샘플 버퍼의 내용을 배열 (_mediaBuffer_)에 복사 합니다.

```cpp
Platform::Array<byte>^ MediaReader::LoadMedia(_In_ Platform::String^ filename)
{
    DX::ThrowIfFailed(
        MFStartup(MF_VERSION)
        );

    // Creates a media source reader.
    ComPtr<IMFSourceReader> reader;
    DX::ThrowIfFailed(
        MFCreateSourceReaderFromURL(
            Platform::String::Concat(m_installedLocationPath, filename)->Data(),
            nullptr,
            &reader
            )
        );

    // Set the decoded output format as PCM.
    // XAudio2 on Windows can process PCM and ADPCM-encoded buffers.
    // When using MediaFoundation, this sample always decodes into PCM.
    Microsoft::WRL::ComPtr<IMFMediaType> mediaType;
    DX::ThrowIfFailed(
        MFCreateMediaType(&mediaType)
        );

    // Define the major category of the media as audio. For more info about major media types,
    // go to: https://msdn.microsoft.com/library/windows/desktop/aa367377.aspx
    DX::ThrowIfFailed(
        mediaType->SetGUID(MF_MT_MAJOR_TYPE, MFMediaType_Audio)
        );

    // Define the sub-type of the media as uncompressed PCM audio. For more info about audio sub-types,
    // go to: https://msdn.microsoft.com/library/windows/desktop/aa372553.aspx
    DX::ThrowIfFailed(
        mediaType->SetGUID(MF_MT_SUBTYPE, MFAudioFormat_PCM)
        );
    
    // Sets the media type for a stream. This media type defines that format that the Source Reader 
    // produces as output. It can differ from the native format provided by the media source.
    // For more info, go to https://msdn.microsoft.com/library/windows/desktop/dd374667.aspx
    DX::ThrowIfFailed(
        reader->SetCurrentMediaType(static_cast<uint32>(MF_SOURCE_READER_FIRST_AUDIO_STREAM), 0, mediaType.Get())
        );

    // Get the current media type for the stream.
    // For more info, go to:
    // https://msdn.microsoft.com/library/windows/desktop/dd374660.aspx
        Microsoft::WRL::ComPtr<IMFMediaType> outputMediaType;
    DX::ThrowIfFailed(
        reader->GetCurrentMediaType(static_cast<uint32>(MF_SOURCE_READER_FIRST_AUDIO_STREAM), &outputMediaType)
        );

    // Converts the current media type into the WaveFormatEx buffer structure.
    UINT32 size = 0;
    WAVEFORMATEX* waveFormat;
    DX::ThrowIfFailed(
        MFCreateWaveFormatExFromMFMediaType(outputMediaType.Get(), &waveFormat, &size)
        );

    // Copies the waveFormat's block of memory to the starting address of the m_waveFormat variable in MediaReader.
    // Then free the waveFormat memory block.
    // For more info, go to https://msdn.microsoft.com/library/windows/desktop/aa366535.aspx and
    // https://msdn.microsoft.com/library/windows/desktop/ms680722.aspx
    CopyMemory(&m_waveFormat, waveFormat, sizeof(m_waveFormat));
    CoTaskMemFree(waveFormat);

    PROPVARIANT propVariant;
    DX::ThrowIfFailed(
        reader->GetPresentationAttribute(static_cast<uint32>(MF_SOURCE_READER_MEDIASOURCE), MF_PD_DURATION, &propVariant)
        );

    // 'duration' is in 100ns units; convert to seconds, and round up
    // to the nearest whole byte.
    LONGLONG duration = propVariant.uhVal.QuadPart;
    unsigned int maxStreamLengthInBytes =
        static_cast<unsigned int>(
            ((duration * static_cast<ULONGLONG>(m_waveFormat.nAvgBytesPerSec)) + 10000000) /
            10000000
            );

    Platform::Array<byte>^ fileData = ref new Platform::Array<byte>(maxStreamLengthInBytes);

    ComPtr<IMFSample> sample;
    ComPtr<IMFMediaBuffer> mediaBuffer;
    DWORD flags = 0;

    int positionInData = 0;
    bool done = false;
    while (!done)
    {
        //...
        // Read audio data.
    }

    return fileData;
}
```
## <a name="associate-sound-to-object"></a>소리 개체에 연결

소리 개체에 연결 [simple3dgame:: initialize](#simple3dgameinitialize-method) 메서드에서 게임 초기화 될 때 수행이 됩니다.

요점:
* __GameObject__ 클래스에서 소리 효과 개체에 연결 하는 데 사용 되는 __HitSound__ 속성이 있습니다.
* [SoundEffect](#soundeffecth) 클래스 개체의 새 인스턴스를 만들고 게임 개체와 연결 합니다. 이 클래스는 __XAudio2__ Api를 사용 하 여 소리를 재생 합니다.  [Audio](#audioh) 클래스에서 제공 하는 마스터링 음성을 사용 합니다. [MediaReader](#mediareaderh) 클래스를 사용 하 여 파일 위치에서 사운드 데이터를 읽을 수 있습니다.

[Soundeffect:: Initialize](#soundeffectinitialize-method) __SoundEffect__ 인스턴스 다음 입력된 매개 변수를 사용 하 여 초기화 하는 데 사용 됩니다: 사운드 엔진 개체 (IXAudio2 [Audio::CreateDeviceIndependentResources](#audiocreatedeviceindependentresources-method) 메서드에서 만든 개체)에 대 한 포인터 형식에 대 한 포인터는.wav의 __mediareader:: Getoutputwaveformatex__및 사운드 데이터를 사용 하 여 파일 [mediareader:: Loadmedia](#mediareaderloadmedia-method) 메서드를 사용 하 여 로드 합니다. 초기화 하는 동안 소리 효과 대 한 원본 음성 생성 됩니다.

### <a name="soundeffectinitialize-method"></a>Soundeffect:: Initialize 메서드

```cpp
void SoundEffect::Initialize(
    _In_ IXAudio2 *masteringEngine,
    _In_ WAVEFORMATEX *sourceFormat,
    _In_ Platform::Array<byte>^ soundData)
{
    m_soundData = soundData;

    if (masteringEngine == nullptr)
    {
        // Audio is not available so just return.
        m_audioAvailable = false;
        return;
    }

    // Create a source voice for this sound effect.
    DX::ThrowIfFailed(
        masteringEngine->CreateSourceVoice(
            &m_sourceVoice,
            sourceFormat
            )
        );
    m_audioAvailable = true;
}
```

## <a name="play-the-sound"></a>소리를 재생 합니다.

이 여기서 업데이트 되는 개체의 움직임 않고 개체 간 충돌은 결정 소리 효과 재생 하는 트리거 [Simple3DGame::UpdateDynamics](#simple3dgameupdatedynamics-method) 메서드에서 정의 됩니다.

개체의 상호 작용 게임에 따라 크게 달라 하므로 하지 하겠습니다 여기 게임 개체의 역학에 설명 합니다. 구현 이해 관심이 [Simple3DGame::UpdateDynamics](#simple3dgameupdatedynamics-method) 방법으로 이동 합니다.

원칙적으로 충돌이 발생 하는 경우 표시할 [SoundEffect::PlaySound]((soundeffectplaysound-method) 호출 하 여 재생 소리 효과. 이 메서드는 현재 재생 되 고 메모리 버퍼 원하는 사운드 데이터가 대기 하는 모든 소리 효과 중지 합니다. 원본 음성을 사용 하 여 볼륨을 설정, 사운드 데이터를 전송 하 고 재생을 시작 합니다.

### <a name="soundeffectplaysound-method"></a>Soundeffect:: Playsound 메서드

* 원본 음성 개체 **m\_sourceVoice** 를 사용 하 여 사운드 데이터 버퍼 **m\_soundData** 의 재생을 시작 하려면
* [XAUDIO2\_BUFFER](https://msdn.microsoft.com/library/windows/desktop/ee419228)에 있는 것 사운드 데이터 버퍼에 대 한 참조를 제공 하 고 다음 제출 [ixaudio2sourcevoice:: Submitsourcebuffer](https://msdn.microsoft.com/library/windows/desktop/ee418473)호출 하 여 만듭니다. 
* 사운드 데이터가 대기하고 있으면 **SoundEffect::PlaySound**에서 [IXAudio2SourceVoice::Start](https://msdn.microsoft.com/library/windows/desktop/ee418471)를 호출하여 재생을 시작합니다.

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

### <a name="simple3dgameupdatedynamics-method"></a>Simple3DGame::UpdateDynamics 메서드

상호 작용 하 고 게임 개체 간 충돌 __Simple3DGame::UpdateDynamics__ 메서드를 처리합니다. 개체 충돌 하거나 교차, 관련된 소리 효과를 재생을 트리거합니다.

```cpp
void Simple3DGame::UpdateDynamics()
{
    // ...
    // Check for collisions between ammo.
#pragma region inter-ammo collision detection
    if (m_ammoCount > 1)
    {
       // ...
       // Check collision between instances One and Two.
       // ...
       if (distanceSquared < (GameConstants::AmmoSize * GameConstants::AmmoSize))
       {
           // The two ammo are intersecting.
           // ...
           // Start playing the sounds for the impact between the two balls.
              m_ammo[one]->PlaySound(impact, m_player->Position());
              m_ammo[two]->PlaySound(impact, m_player->Position());

       }
    }
#pragma endregion

#pragma region Ammo-Object intersections
    // Check for intersections between the ammo and the other objects in the scene.
    // ...
    // Ball is in contact with Object.
    // ...

    // Make sure that the ball is actually headed towards the object. At grazing angles there
    // could appear to be an impact when the ball is actually already hit and moving away.
    if (impact > 0.0f)
    {
        // ...
        // Play the sound associated with the Ammo hitting something.
           m_ammo[one]->PlaySound(impact, m_player->Position());

        if (m_objects[i]->Target() && !m_objects[i]->Hit())
        {
            // The object is a target and isn't currently hit, so mark it as hit and
            // play the sound associated with the impact.
             m_objects[i]->Hit(true);
             m_objects[i]->HitTime(timeTotal);
             m_totalHits++;

             m_objects[i]->PlaySound(impact, m_player->Position());
        }
        // ...
    }
#pragma endregion

#pragma region Apply Gravity and world intersection
    // Apply gravity and check for collision against enclosing volume.
    // ...
    if (position.z < limit)
    {
        // The ammo instance hit the a wall in the min Z direction.
        // Align the ammo instance to the wall, invert the Z component of the velocity and
        // play the impact sound.
           position.z = limit;
           m_ammo[i]->PlaySound(-velocity.z, m_player->Position());
           velocity.z = -velocity.z * GameConstants::Physics::GroundRestitution;
    }
    // ...
#pragma endregion
}
```
## <a name="next-steps"></a>다음 단계

우리는 UWP 프레임 워크, 그래픽, 컨트롤, 사용자 인터페이스 및 Windows 10 게임의 오디오 처리 됩니다. [게임 샘플 확장](tutorial-resources.md),이 자습서의 다음 부분 게임을 개발 하는 경우 사용할 수 있는 다른 옵션을 설명 합니다.

## <a name="audio-concepts"></a>오디오 개념

Windows 10 게임 개발을 위한 XAudio2 버전 2.9를 사용 합니다. 이 버전은 Windows 10 함께 제공 됩니다. 자세한 내용은 [XAudio2 버전](https://msdn.microsoft.com/library/windows/desktop/ee415802.aspx)으로 이동 합니다.

__AudioX2__ 신호 처리 및 믹싱 기초를 제공 하는 하위 수준 API입니다. 자세한 내용은 [XAudio2 주요 개념](https://msdn.microsoft.com/library/windows/desktop/ee415764.aspx)을 참조 하세요.

### <a name="xaudio2-voices"></a>XAudio2 음성

세 가지 유형의 XAudio2 음성 개체가 있습니다: 원본, 서브 믹스 및 마스터링 음성 합니다. 음성은 XAudio2 개체 사용 조작 하 고 오디오 데이터를 재생 하는의 작업을 처리 합니다. 
* 원본 음성은 클라이언트에서 제공한 오디오 데이터에서 작동합니다. 
* 원본 및 서브믹스 음성은 해당 출력을 하나 이상의 서브믹스 또는 마스터링 음성으로 보냅니다. 
* 서브믹스 및 마스터링 음성은 해당 음성을 공급하는 모든 음성으로부터 오디오를 믹싱하고 그 결과에서 작동합니다. 
* 마스터링 음성은 원본 음성 및 서브 믹스 음성에서 데이터를 받을 하 고 해당 데이터를 오디오 하드웨어에 보냅니다.

자세한 내용은 [XAudio2 음성](https://msdn.microsoft.com/library/windows/desktop/ee415824.aspx)으로 이동 합니다.

### <a name="audio-graph"></a>오디오 그래프

오디오 그래프에는 [XAudio2 음성](#xaudio2-voice-objects)의 모음입니다. 오디오 원본 음성의 오디오 그래프의 한쪽에서 시작 하 고, 필요에 따라 하나 이상의 서브 믹스 음성 통과, 마스터링 음성에서 끝납니다. 오디오 그래프는 현재 재생, 서브 믹스 음성 0 개 이상의 각 소리의 원본 음성 및 마스터 음성 1 개가 포함 됩니다. 가장 간단한 오디오 그래프 및 XAudio2에서 노이즈를 확인 하는 데 필요한 최소 마스터 음성에 직접 출력 단일 원본 음성입니다. 자세한 내용은 [오디오 그래프](https://msdn.microsoft.com/library/windows/desktop/ee415739.aspx)으로 이동 합니다.

### <a name="additional-reading"></a>추가 정보

* [방법: XAudio2 초기화](https://msdn.microsoft.com/library/windows/desktop/ee415779.aspx)
* [방법: XAudio2에 오디오 데이터 파일 로드](https://msdn.microsoft.com/library/windows/desktop/ee415781(v=vs.85).aspx)
* [방법: XAudio2를 사용한 소리 재생](https://msdn.microsoft.com/library/windows/desktop/ee415787.aspx)

## <a name="key-audio-h-files"></a>주요 오디오.h 파일

### <a name="audioh"></a>Audio.h

```cpp
// Audio:
// This class uses XAudio2 to provide sound output.  It creates two
// engines - one for music and the other for sound effects - each as
// a separate mastering voice.
// The SuspendAudio and ResumeAudio methods can be used to stop
// and start all audio playback.
public:
    Audio();

    void Initialize();
    void CreateDeviceIndependentResources();
    IXAudio2* MusicEngine();
    IXAudio2* SoundEffectEngine();
    void SuspendAudio();
    void ResumeAudio();

protected:
    // ...
};
```
### <a name="mediareaderh"></a>MediaReader.h

```cpp
// MediaReader:
// This is a helper class for the SoundEffect class.  It reads small audio files
// synchronously from the package installed folder and returns sound data as a
// byte array.

ref class MediaReader
{
internal:
    MediaReader();

    Platform::Array<byte>^          LoadMedia(_In_ Platform::String^ filename);
    WAVEFORMATEX*                   GetOutputWaveFormatEx();

protected private:
    Windows::Storage::StorageFolder^ m_installedLocation;
    Platform::String^               m_installedLocationPath;
    WAVEFORMATEX                    m_waveFormat;
};
```
### <a name="soundeffecth"></a>SoundEffect.h

```cpp
// SoundEffect:
// This class plays a sound using XAudio2.  It uses a mastering voice provided
// from the Audio class.  The sound data can be read from disk using the MediaReader
// class.

ref class SoundEffect
{
internal:
    SoundEffect();

    void Initialize(
        _In_ IXAudio2*              masteringEngine,
        _In_ WAVEFORMATEX*          sourceFormat,
        _In_ Platform::Array<byte>^ soundData
        );

    void PlaySound(_In_ float volume);

protected private:
    //...

};
```


