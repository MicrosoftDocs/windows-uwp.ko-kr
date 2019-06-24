---
title: 소리 추가
description: 재생 게임 음악 및 소리 효과를 XAudio2 Api를 사용 하 여 간단한 사운드 엔진을 개발 합니다.
ms.assetid: aa05efe2-2baa-8b9f-7418-23f5b6cd2266
ms.date: 10/24/2017
ms.topic: article
keywords: windows 10, uwp, 게임, 사운드
ms.localizationpriority: medium
ms.openlocfilehash: 06c06e1ffe52cae37a000f748076d78ebf6afff4
ms.sourcegitcommit: 6f32604876ed480e8238c86101366a8d106c7d4e
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 06/21/2019
ms.locfileid: "67318860"
---
# <a name="add-sound"></a>소리 추가

이 항목에서는 간단한 사운드 엔진을 만듭니다 [XAudio2](https://docs.microsoft.com/windows/desktop/xaudio2/xaudio2-introduction) Api. 처음 사용 하는 경우 __XAudio2__, 아래 짧은 소개 포함 했습니다 [오디오 개념](#audio-concepts)합니다.

>[!Note]
>이 샘플의 최신 게임 코드를 다운로드하지 않은 경우 [Direct3D 게임 샘플](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/Simple3DGameDX)로 이동합니다. 이 샘플은 UWP 기능 샘플의 큰 컬렉션의 일부입니다. 샘플을 다운로드하는 방법에 대한 지침은 [GitHub에서 UWP 샘플 가져오기](https://docs.microsoft.com/windows/uwp/get-started/get-uwp-app-samples)를 참조하세요.

## <a name="objective"></a>목표

사용 하 여 샘플 게임에 사운드를 추가 [XAudio2](/windows/desktop/xaudio2/xaudio2-introduction)합니다.

## <a name="define-the-audio-engine"></a>오디오 엔진 정의

게임 샘플에서 오디오 개체와 동작은 다음 세 개의 파일에 정의됩니다.

* __[Audio.h](#audioh)/.cpp__: 정의 __오디오__ 포함 된 개체를 __XAudio2__ 소리 재생에 대 한 리소스입니다. 또한 게임이 일시 중지되거나 비활성화된 경우 오디오 재생을 일시 중단하고 다시 시작하는 메서드를 정의합니다.
* __[MediaReader.h](#mediareaderh)/.cpp__: 로컬 저장소에서 오디오.wav 파일을 읽기 위한 메서드를 정의 합니다.
* __[SoundEffect.h](#soundeffecth)/.cpp__: 게임에서 소리 재생을 위한 개체를 정의합니다.

## <a name="overview"></a>개요

게임에 오디오 재생에 대 한 설정의 세 가지 주요 부분이 있습니다.

1. [만들기 및 오디오 리소스를 초기화 합니다.](#create-and-initialize-the-audio-resources)
2. [오디오 파일 로드](#load-audio-file)
3. [개체는 소리를 연결 합니다.](#associate-sound-to-object)

모두에 정의 된 [Simple3DGame::Initialize](#simple3dgameinitialize-method) 메서드. 그럼 먼저이 메서드를 검사 하 고 다음의 각 섹션에서 자세한 세부 정보에 자세히 알아보기.

를 설정한 후 재생할 소리 효과 트리거하는 방법을 설명 합니다. 자세한 내용은 이동 [소리를 재생](#play-the-sound)합니다.

### <a name="simple3dgameinitialize-method"></a>Simple3DGame::Initialize 메서드

__Simple3DGame::Initialize__여기서 __m\_컨트롤러__ 하 고 __m\_렌더러__ 는 또한 초기화 오디오 엔진을 설정 하 고 가져오기 소리를 재생할 준비가 되었습니다.

 * 만들 __m\_audioController__의 인스턴스인 합니다 [오디오](#audioh) 클래스입니다.
 * 사용 하 여 필요한 오디오 리소스를 만들려고 합니다 [Audio::CreateDeviceIndependentResources](#audiocreatedeviceindependentresources-method) 메서드. 여기, 두 __XAudio2__ 개체 &mdash; 음악 엔진 개체 및 사운드 엔진 개체 및 각각에 대해 마스터 음성 생성 되었습니다. 음악 엔진 개체 게임에 대 한 배경 음악을 재생 하는 데 사용할 수 있습니다. 사운드 엔진 게임에서 음향 효과 재생 하는 데 사용할 수 있습니다. 자세한 내용은 참조 하세요. [만들기 및 오디오 리소스 초기화](#create-and-initialize-the-audio-resources)합니다.
 * 만들 __mediaReader__의 인스턴스인 [MediaReader](#mediareaderh) 클래스입니다. [MediaReader](#mediareaderh)에 대 한 도우미 클래스인 합니다 [SoundEffect](#soundeffecth) 클래스 읽기 작은 오디오 파일 위치에서 파일을 동기적으로 및 사운드 데이터를 바이트 배열로 반환 합니다.
 * 사용 하 여 [MediaReader::LoadMedia](#mediareaderloadmedia-method) 사운드 파일을 해당 위치에서 로드 한를 __targetHitSound__ 로드할된.wav 사운드 데이터를 보유할 변수로 대체 합니다. 자세한 내용은 참조 하세요. [오디오 파일 로드](#load-audio-file)합니다. 

음향 효과 게임 개체와 연결 됩니다. 따라서 해당 게임 개체를 사용 하 여 충돌이 발생 하면 재생할 소리 효과를 트리거합니다. 이 게임 샘플에서는 음향 효과 (어떤를 사용 하 여 대상을 사용 하 여 문제를 해결) 탄약 및 대상 했습니다. 
    
* 에 __GameObject__ 클래스, 즉를 __HitSound__ 소리 효과 개체에 연결 하는 데 사용 되는 속성입니다.
* 새 인스턴스를 만들고 합니다 [SoundEffect](#soundeffecth) 클래스 및 초기화 합니다. 초기화 하는 동안 소리 효과 대 한 원본 음성을 생성 됩니다. 
* 이 클래스에서 제공 하는 마스터 음성을 사용 하 여 소리를 재생 합니다 [오디오](#audioh) 클래스입니다. 사운드 데이터를 사용 하 여 파일 위치에서 읽기를 [MediaReader](#mediareaderh) 클래스입니다. 자세한 내용은 참조 하세요. [개체는 소리 연결](#associate-sound-to-object)합니다.

>[!Note]
>소리를 재생 하려면 실제 트리거 이동 및 이러한 게임 개체의 충돌에 의해 결정 됩니다. 따라서 실제로 이러한 소리를 재생에 대 한 호출에 정의 된 [Simple3DGame::UpdateDynamics](#simple3dgameupdatedynamics-method) 메서드. 자세한 내용은 이동 [소리를 재생](#play-the-sound)합니다.

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

## <a name="create-and-initialize-the-audio-resources"></a>만들기 및 오디오 리소스를 초기화 합니다.

* 사용 하 여 [XAudio2Create](https://docs.microsoft.com/windows/desktop/api/xaudio2/nf-xaudio2-xaudio2create), XAudio2 API 음악 및 소리 효과 엔진을 정의 하는 두 새 XAudio2 개체를 만듭니다. 이 메서드는 개체에 대 한 포인터를 반환 [IXAudio2](https://docs.microsoft.com/windows/desktop/api/xaudio2/nn-xaudio2-ixaudio2) 모든 오디오 엔진을 관리 하는 인터페이스 상태, 오디오, 스레드, 음성 그래프 등을 처리 합니다.
* 엔진에서 인스턴스화된 후 사용 하 여 [IXAudio2::CreateMasteringVoice](https://docs.microsoft.com/windows/desktop/api/xaudio2/nf-xaudio2-ixaudio2-createmasteringvoice) 각 사운드 엔진 개체에 대 한 마스터 음성을 만들려고 합니다.

자세한 내용은 이동 [방법: XAudio2 초기화](https://docs.microsoft.com/windows/desktop/xaudio2/how-to--initialize-xaudio2)합니다.

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

게임 샘플에 오디오 형식 파일을 읽기 위한 코드 정의 [MediaReader.h](#mediareaderh)/cpp__ 합니다.  호출로 인코딩된.wav 오디오 파일을 읽으려면 [MediaReader::LoadMedia](#mediareaderloadmedia-method)의 입력된 매개 변수로.wav 파일 이름을 전달 합니다.

### <a name="mediareaderloadmedia-method"></a>MediaReader::LoadMedia 메서드

이 메서드는 [Media Foundation](https://docs.microsoft.com/windows/desktop/medfound/microsoft-media-foundation-sdk) API를 사용하여 .wav 오디오 파일을 PCM(Pulse Code Modulation) 버퍼로 읽습니다.

#### <a name="set-up-the-source-reader"></a>원본 판독기 설정

1. 사용 하 여 [MFCreateSourceReaderFromURL](https://docs.microsoft.com/windows/desktop/api/mfreadwrite/nf-mfreadwrite-mfcreatesourcereaderfromurl) 미디어 원본 판독기를 만들려면 ([IMFSourceReader](https://docs.microsoft.com/windows/desktop/api/mfreadwrite/nn-mfreadwrite-imfsourcereader)).
2. 사용 하 여 [MFCreateMediaType](https://docs.microsoft.com/windows/desktop/api/mfapi/nf-mfapi-mfcreatemediatype) 미디어 형식을 만들려면 ([IMFMediaType](https://docs.microsoft.com/windows/desktop/api/mfobjects/nn-mfobjects-imfmediatype)) 개체 (_mediaType_). 미디어 형식에 대 한 설명을 나타냅니다. 
3. 지정 합니다 _mediaType_의 디코딩된 출력은 오디오 PCM 오디오를 입력 하는 __XAudio2__ 사용할 수 있습니다.
4. 호출 하 여 원본 판독기에 대 한 디코딩된 출력 미디어 유형 집합 [IMFSourceReader::SetCurrentMediaType](https://docs.microsoft.com/windows/desktop/api/mfreadwrite/nf-mfreadwrite-imfsourcereader-setcurrentmediatype)합니다.

에 대 한 자세한 내용은 원본 판독기를 사용 하는 이유,로 이동 [원본 판독기](https://docs.microsoft.com/windows/desktop/medfound/source-reader)합니다.

#### <a name="describe-the-data-format-of-the-audio-stream"></a>데이터 형식의 오디오 스트림 설명

1. 사용 하 여 [IMFSourceReader::GetCurrentMediaType](https://docs.microsoft.com/windows/desktop/api/mfreadwrite/nf-mfreadwrite-imfsourcereader-getcurrentmediatype) 스트림에 대 한 현재 미디어 형식을 가져오려고 합니다.
2. 사용 하 여 [IMFMediaType::MFCreateWaveFormatExFromMFMediaType](https://docs.microsoft.com/windows/desktop/api/mfapi/nf-mfapi-mfcreatewaveformatexfrommfmediatype) 변환할 현재 오디오 미디어 유형입니다는 [WAVEFORMATEX](https://docs.microsoft.com/windows/desktop/api/mmreg/ns-mmreg-twaveformatex) 버퍼를 입력으로 이전 작업의 결과 사용 합니다. 이 구조는 웨이브 오디오 스트림을 오디오 로드 된 후에 사용 되는 데이터 형식을 지정 합니다. 

합니다 __WAVEFORMATEX__ 형식은 PCM 버퍼를 설명 하기 위해 사용할 수 있습니다. 비교 했을 때를 [WAVEFORMATEXTENSIBLE](https://docs.microsoft.com/windows-hardware/drivers/ddi/content/ksmedia/ns-ksmedia-waveformatextensible) 구조를 사용할 수 있습니다만 오디오 wave 형식의 하위 집합을 설명 합니다. 간의 차이점에 대 한 자세한 내용은 __WAVEFORMATEX__ 및 __WAVEFORMATEXTENSIBLE__를 참조 하십시오 [확장할 수 있는 웨이브 형식 설명자](https://docs.microsoft.com/windows-hardware/drivers/audio/extensible-wave-format-descriptors)합니다.

#### <a name="read-the-audio-stream"></a>오디오 스트림에서 읽기

1.  가져오기는 기간 (초)를 호출 하 여 오디오 스트림의 [IMFSourceReader::GetPresentationAttribute](https://docs.microsoft.com/windows/desktop/api/mfreadwrite/nf-mfreadwrite-imfsourcereader-getpresentationattribute) 바이트를 지속 시간을 변환 합니다.
2.  오디오 파일을 호출 하 여 스트림으로 읽는 [IMFSourceReader::ReadSample](https://docs.microsoft.com/windows/desktop/api/mfreadwrite/nf-mfreadwrite-imfsourcereader-readsample)합니다. __ReadSample__ 미디어 소스에서 다음 샘플을 읽습니다.
3.  사용 하 여 [IMFSample::ConvertToContiguousBuffer](https://docs.microsoft.com/windows/desktop/api/mfobjects/nf-mfobjects-imfsample-converttocontiguousbuffer) 오디오 샘플 버퍼의 내용을 복사할 (_샘플_)을 배열로 (_mediaBuffer_).

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
## <a name="associate-sound-to-object"></a>개체는 소리를 연결 합니다.

게임 초기화 때 발생 하는 개체에 소리를 연결 합니다 [Simple3DGame::Initialize](#simple3dgameinitialize-method) 메서드.

요약:
* 에 __GameObject__ 클래스, 즉를 __HitSound__ 소리 효과 개체에 연결 하는 데 사용 되는 속성입니다.
* 새 인스턴스를 만들고 합니다 [SoundEffect](#soundeffecth) 개체 클래스 및 게임 개체와 연결 합니다. 이 클래스를 사용 하는 사운드 재생 __XAudio2__ Api.  제공한 마스터 음성을 사용 합니다 [오디오](#audioh) 클래스입니다. 사운드 데이터를 사용 하 여 파일 위치에서 읽을 수 있습니다 합니다 [MediaReader](#mediareaderh) 클래스입니다.

[SoundEffect::Initialize](#soundeffectinitialize-method) 초기화 하는 데 사용 되는 __SoundEffect__ 다음 입력된 매개 변수를 사용 하 여 인스턴스: 사운드 엔진 개체에 대 한 포인터 (IXAudio2 개체에서 만든는 [오디오: CreateDeviceIndependentResources](#audiocreatedeviceindependentresources-method) 메서드), 형식에 대 한 포인터 사용 하 여.wav 파일 __MediaReader::GetOutputWaveFormatEx__를 사용 하 여 사운드 데이터 로드 및 [MediaReader::LoadMedia ](#mediareaderloadmedia-method) 메서드. 초기화 하는 동안 소리 효과 대 한 원본 음성도 생성 됩니다.

### <a name="soundeffectinitialize-method"></a>SoundEffect::Initialize 메서드

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

## <a name="play-the-sound"></a>소리 재생

에 음향 효과 재생 하려면 트리거가 정의 [Simple3DGame::UpdateDynamics](#simple3dgameupdatedynamics-method) 메서드는 개체의 이동 업데이트 되 고 개체 간의 충돌 이기 때문에 결정 됩니다.

게임을 따라 크게 달라 집니다 개체 간의 상호 작용 하므로 여기 게임 개체의 동적 특성을 설명 하지 않습니다. 로 이동 하는 구현 방식을 이해 하려면 싶다면 [Simple3DGame::UpdateDynamics](#simple3dgameupdatedynamics-method) 메서드.

원칙적으로 충돌이 발생 하는 경우 해당 트리거를 호출 하 여 재생할 소리 효과 **SoundEffect::PlaySound**합니다. 이 메서드는 현재 재생 중인 소리 원하는 데이터를 사용 하 여 메모리 내 버퍼를 큐에 모든 음향 효과 중지 합니다. 원본 음성 사용 하 여 볼륨 설정, 사운드 데이터 전송 및 재생을 시작 합니다.

### <a name="soundeffectplaysound-method"></a>SoundEffect::PlaySound 메서드

* 원본 음성 개체를 사용 하 여 **m\_sourceVoice** 사운드 데이터 버퍼의 재생을 시작 하 **m\_soundData**
* 만듭니다는 [XAUDIO2\_버퍼](https://docs.microsoft.com/windows/desktop/api/xaudio2/ns-xaudio2-xaudio2_buffer)하는 사운드 데이터 버퍼에 대 한 참조를 제공 하 고 다음에 대 한 호출을 사용 하 여 전송 [IXAudio2SourceVoice::SubmitSourceBuffer](https://docs.microsoft.com/windows/desktop/api/xaudio2/nf-xaudio2-ixaudio2sourcevoice-submitsourcebuffer)합니다. 
* 사운드 데이터로 대기 **SoundEffect::PlaySound** 시작 호출 하 여 재생 [IXAudio2SourceVoice::Start](https://docs.microsoft.com/windows/desktop/api/xaudio2/nf-xaudio2-ixaudio2sourcevoice-start)합니다.

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

합니다 __Simple3DGame::UpdateDynamics__ 메서드는 상호 작용 및 게임 개체 간의 충돌 담당 합니다. 개체 충돌, 교차 하는 경우 재생 하려면 연결 된 소리 효과를 트리거합니다.

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

UWP 프레임 워크, 그래픽, 컨트롤, 사용자 인터페이스 및 Windows 10 게임의 오디오 설명 했습니다. 이 자습서의 다음 부분 [게임 샘플 확장](tutorial-resources.md), 게임을 개발할 때 사용할 수 있는 다른 옵션을 설명 합니다.

## <a name="audio-concepts"></a>오디오 개념

Windows 10 게임 개발을 위한 XAudio2 버전 2.9를 사용 합니다. 이 버전은 Windows 10과 함께 제공 됩니다. 자세한 내용은 이동 [XAudio2 버전](https://docs.microsoft.com/windows/desktop/xaudio2/xaudio2-versions)합니다.

__AudioX2__ 는 신호 처리 및 foundation 혼합 제공 하는 하위 수준 API. 자세한 내용은 참조 하세요. [XAudio2 주요 개념](https://docs.microsoft.com/windows/desktop/xaudio2/xaudio2-key-concepts)합니다.

### <a name="xaudio2-voices"></a>XAudio2 음성

XAudio2 음성 개체의 세 가지 종류가 있습니다: 원본를 및 음성을 마스터 합니다. 음성은 XAudio2 개체 처리, 조작 및 오디오 데이터를 재생 하는 데 사용 됩니다. 
* 원본 음성은 클라이언트에서 제공한 오디오 데이터에서 작동합니다. 
* 원본 및 서브믹스 음성은 해당 출력을 하나 이상의 서브믹스 또는 마스터링 음성으로 보냅니다. 
* 서브믹스 및 마스터링 음성은 해당 음성을 공급하는 모든 음성으로부터 오디오를 믹싱하고 그 결과에서 작동합니다. 
* 마스터 음성 원본 음성 및 믹스 음성 데이터를 수신 하 고 오디오 하드웨어에 해당 데이터를 보냅니다.

자세한 내용은 이동 [XAudio2 음성](/windows/desktop/xaudio2/xaudio2-voices)합니다.

### <a name="audio-graph"></a>오디오 그래프

오디오 그래프의 컬렉션인 [XAudio2 음성](/windows/desktop/xaudio2/xaudio2-voices)합니다. 오디오 소스 음성의 오디오 그래프의 한 쪽에서 시작 하 고, 필요에 따라 하나 이상의 믹스 음성 통과, 마스터 음성에서 끝납니다. 각 현재 재생 중인 소리를 0 개 이상의 믹스 음성에 대 한 원본 음성 및 하나의 마스터 음성 오디오 그래프를 포함 됩니다. 가장 간단한 오디오 그래프 및 XAudio2에 노이즈를 확인 하는 데 필요한 최소 마스터 음성에 직접 출력을 단일 소스 음성입니다. 자세한 내용은 이동 [오디오 그래프](https://docs.microsoft.com/windows/desktop/xaudio2/audio-graphs)합니다.

### <a name="additional-reading"></a>추가 참조 항목

* [방법: XAudio2 초기화](https://docs.microsoft.com/windows/desktop/xaudio2/how-to--initialize-xaudio2)
* [방법: XAudio2의 오디오 데이터 파일 로드](https://docs.microsoft.com/windows/desktop/xaudio2/how-to--load-audio-data-files-in-xaudio2)
* [방법: XAudio2 사용 하 여 소리 재생](https://docs.microsoft.com/windows/desktop/xaudio2/how-to--play-a-sound-with-xaudio2)

## <a name="key-audio-h-files"></a>키 오디오.h 파일

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


