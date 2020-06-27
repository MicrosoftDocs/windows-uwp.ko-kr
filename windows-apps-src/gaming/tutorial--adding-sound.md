---
title: 소리 추가
description: XAudio2 Api를 사용 하 여 게임 음악과 음향 효과를 재생 하는 간단한 사운드 엔진을 개발 합니다.
ms.assetid: aa05efe2-2baa-8b9f-7418-23f5b6cd2266
ms.date: 10/24/2017
ms.topic: article
keywords: windows 10, uwp, 게임, 소리
ms.localizationpriority: medium
ms.openlocfilehash: 0e624c750bfce0633bc91d440fd883341b831836
ms.sourcegitcommit: 20969781aca50738792631f4b68326f9171a3980
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 06/26/2020
ms.locfileid: "85409652"
---
# <a name="add-sound"></a>소리 추가

> [!NOTE]
> 이 항목은 DirectX 자습서 시리즈 [를 사용 하 여 단순 유니버설 Windows 플랫폼 (UWP) 게임 만들기](tutorial--create-your-first-uwp-directx-game.md) 의 일부입니다. 해당 링크의 항목은 계열의 컨텍스트를 설정 합니다.

이 항목에서는 [XAudio2](/windows/desktop/xaudio2/xaudio2-introduction) api를 사용 하 여 간단한 사운드 엔진을 만듭니다. __XAudio2__를 처음 접하는 경우에는 [오디오 개념](#audio-concepts)에 대 한 간단한 소개를 포함 했습니다.

>[!Note]
>이 샘플에 대 한 최신 게임 코드를 다운로드 하지 않은 경우 [Direct3D sample game](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/Simple3DGameDX)로 이동 합니다. 이 샘플은 여러 UWP 기능 샘플 컬렉션의 일부입니다. 샘플을 다운로드 하는 방법에 대 한 지침은 [GitHub에서 UWP 샘플 가져오기](/windows/uwp/get-started/get-uwp-app-samples)를 참조 하세요.

## <a name="objective"></a>Objective

[XAudio2](/windows/desktop/xaudio2/xaudio2-introduction)를 사용 하 여 샘플 게임에 소리를 추가 합니다.

## <a name="define-the-audio-engine"></a>오디오 엔진 정의

샘플 게임에서 오디오 개체 및 동작은 세 개의 파일에 정의 되어 있습니다.

* __[오디오나](#audioh)..cpp__: 사운드 재생에 대 한 __XAudio2__ 리소스를 포함 하는 __오디오__ 개체를 정의 합니다. 또한 게임을 일시 중지 하거나 비활성화 한 경우 오디오 재생을 일시 중단 하 고 다시 시작 하는 방법을 정의 합니다.
* __ [Mediareader .h](#mediareaderh)__: 로컬 저장소에서 오디오 .wav 파일을 읽는 메서드를 정의 합니다.
* __ [SoundEffect](#soundeffecth)__: 게임 내 소리 재생에 대 한 개체를 정의 합니다.

## <a name="overview"></a>개요

게임에 오디오 재생을 설정 하는 데는 세 가지 주요 부분이 있습니다.

1. [오디오 리소스 만들기 및 초기화](#create-and-initialize-the-audio-resources)
2. [오디오 파일 로드](#load-audio-file)
3. [개체에 소리 연결](#associate-sound-to-object)

이러한 메서드는 모두 [Simple3DGame:: Initialize](#simple3dgameinitialize-method) 메서드에 정의 되어 있습니다. 먼저이 메서드를 검토 한 다음 각 섹션에서 좀 더 자세히 살펴보겠습니다.

을 설정한 후에는 소리 효과를 트리거하여 재생 하는 방법을 알아봅니다. 자세한 내용은 [소리 재생](#play-the-sound)을 참조 하세요.

### <a name="simple3dgameinitialize-method"></a>Simple3DGame:: Initialize 메서드

__M \_ 컨트롤러__ 와 __m \_ 렌더러에서__ 도 초기화 되는 __Simple3DGame:: Initialize__에서 오디오 엔진을 설정 하 고 소리를 재생 하도록 준비 합니다.

 * [Audio](#audioh) 클래스의 인스턴스인 __m 및 \_ 컨트롤러__를 만듭니다.
 * [Audio:: CreateDeviceIndependentResources](#audiocreatedeviceindependentresources-method) 메서드를 사용 하 여 필요한 오디오 리소스를 만듭니다. 여기에서 두 개의 __XAudio2__ 개체 &mdash; 는 음악 엔진 개체와 사운드 엔진 개체이 고 각각에 대 한 마스터링 음성이 생성 되었습니다. 음악 엔진 개체를 사용 하 여 게임의 배경 음악을 재생할 수 있습니다. 사운드 엔진은 게임에서 소리 효과를 재생 하는 데 사용할 수 있습니다. 자세한 내용은 [오디오 리소스 만들기 및 초기화](#create-and-initialize-the-audio-resources)를 참조 하세요.
 * [Mediareader 클래스의](#mediareaderh) 인스턴스인 __mediareader__를 만듭니다. [SoundEffect](#soundeffecth) 클래스에 대 한 도우미 클래스인 [mediareader](#mediareaderh)는 파일 위치에서 동기적으로 작은 오디오 파일을 읽고 사운드 데이터를 바이트 배열로 반환 합니다.
 * [Mediareader:: LoadMedia](#mediareaderloadmedia-method) 를 사용 하 여 해당 위치에서 소리 파일을 로드 하 고 로드 된 .wav 사운드 데이터를 저장할 __targetHitSound__ 변수를 만듭니다. 자세한 내용은 [오디오 파일 로드](#load-audio-file)를 참조 하세요. 

음향 효과는 게임 개체와 연결 됩니다. 따라서 해당 게임 개체에서 충돌이 발생 하면 재생 되는 소리 효과를 트리거합니다. 이 샘플 게임에서는 탄약 (에서 대상을 촬영 하는 데 사용 하는 항목) 및 대상에 대 한 음향 효과가 있습니다. 
    
* __GameObject__ 클래스에는 소리 효과를 개체에 연결 하는 데 사용 되는 __HitSound__ 속성이 있습니다.
* [SoundEffect](#soundeffecth) 클래스의 새 인스턴스를 만들고 초기화 합니다. 초기화 하는 동안 음향 효과에 대 한 소스 음성이 생성 됩니다. 
* 이 클래스는 [Audio](#audioh) 클래스에서 제공 된 마스터링 음성을 사용 하 여 소리를 재생 합니다. 미디어 데이터를 [미디어 판독기](#mediareaderh) 클래스를 사용 하 여 파일 위치에서 읽습니다. 자세한 내용은 [개체에 소리 연결](#associate-sound-to-object)을 참조 하세요.

>[!Note]
>소리를 재생 하는 실제 트리거는 이러한 게임 개체의 이동과 충돌에 따라 결정 됩니다. 따라서 실제로 이러한 소리를 재생 하는 호출은 [Simple3DGame:: UpdateDynamics](#simple3dgameupdatedynamics-method) 메서드에 정의 되어 있습니다. 자세한 내용은 [소리 재생](#play-the-sound)을 참조 하세요.

```cppwinrt
void Simple3DGame::Initialize(
    _In_ std::shared_ptr<MoveLookController> const& controller,
    _In_ std::shared_ptr<GameRenderer> const& renderer
    )
{
    // The following member is defined in the header file:
    // Audio m_audioController;

    ...

    // Create the audio resources needed.
    // Two XAudio2 objects are created - one for music engine,
    // the other for sound engine. A mastering voice is also
    // created for each of the objects.
    m_audioController.CreateDeviceIndependentResources();

    m_ammo.resize(GameConstants::MaxAmmo);

    ...

    // Create a media reader which is used to read audio files from its file location.
    MediaReader mediaReader;
    auto targetHitSoundX = mediaReader.LoadMedia(L"Assets\\hit.wav");

    // Instantiate the targets for use in the game.
    // Each target has a different initial position, size, and orientation.
    // But share a common set of material properties.
    for (int a = 1; a < GameConstants::MaxTargets; a++)
    {
        ...
        // Create a new sound effect object and associate it
        // with the game object's (target) HitSound property.
        target->HitSound(std::make_shared<SoundEffect>());

        // Initialize the sound effect object with
        // the sound effect engine, format of the audio wave, and audio data
        // During initialization, source voice of this sound effect is also created.
        target->HitSound()->Initialize(
            m_audioController.SoundEffectEngine(),
            mediaReader.GetOutputWaveFormatEx(),
            targetHitSoundX
            );
        ...
    }

    // Instantiate a set of spheres to be used as ammunition for the game
    // and set the material properties of the spheres.
    auto ammoHitSound = mediaReader.LoadMedia(L"Assets\\bounce.wav");

    for (int a = 0; a < GameConstants::MaxAmmo; a++)
    {
        m_ammo[a] = std::make_shared<Sphere>();
        m_ammo[a]->Radius(GameConstants::AmmoRadius);
        m_ammo[a]->HitSound(std::make_shared<SoundEffect>());
        m_ammo[a]->HitSound()->Initialize(
            m_audioController.SoundEffectEngine(),
            mediaReader.GetOutputWaveFormatEx(),
            ammoHitSound
            );
        m_ammo[a]->Active(false);
        m_renderObjects.push_back(m_ammo[a]);
    }
    ...
}
```

## <a name="create-and-initialize-the-audio-resources"></a>오디오 리소스 만들기 및 초기화

* [XAudio2Create](/windows/desktop/api/xaudio2/nf-xaudio2-xaudio2create), XAudio2 API를 사용 하 여 음악 및 음향 효과 엔진을 정의 하는 두 개의 새 XAudio2 개체를 만듭니다. 이 메서드는 모든 오디오 엔진 상태, 오디오 처리 스레드, 음성 그래프 등을 관리 하는 개체의 [IXAudio2](/windows/desktop/api/xaudio2/nn-xaudio2-ixaudio2) 인터페이스에 대 한 포인터를 반환 합니다.
* 엔진이 인스턴스화된 후에는 [IXAudio2:: CreateMasteringVoice](/windows/desktop/api/xaudio2/nf-xaudio2-ixaudio2-createmasteringvoice) 를 사용 하 여 각 사운드 엔진 개체에 대해 마스터링 음성을 만듭니다.

자세한 내용은 [How to: Initialize XAudio2](/windows/desktop/xaudio2/how-to--initialize-xaudio2)를 참조 하세요.

### <a name="audiocreatedeviceindependentresources-method"></a>Audio:: CreateDeviceIndependentResources 메서드

```cppwinrt
void Audio::CreateDeviceIndependentResources()
{
    UINT32 flags = 0;

    winrt::check_hresult(
        XAudio2Create(m_musicEngine.put(), flags)
        );

    HRESULT hr = m_musicEngine->CreateMasteringVoice(&m_musicMasteringVoice);
    if (FAILED(hr))
    {
        // Unable to create an audio device
        m_audioAvailable = false;
        return;
    }

    winrt::check_hresult(
        XAudio2Create(m_soundEffectEngine.put(), flags)
        );

    winrt::check_hresult(
        m_soundEffectEngine->CreateMasteringVoice(&m_soundEffectMasteringVoice)
        );

    m_audioAvailable = true;
}
```

## <a name="load-audio-file"></a>오디오 파일 로드

샘플 게임에서 오디오 형식 파일을 읽는 코드는 [Mediareader. h](#mediareaderh)/cpp__에 정의 되어 있습니다.  인코딩된 .wav 오디오 파일을 읽으려면 입력 매개 변수로 .wav의 파일 이름을 전달 하 여 [Mediareader:: LoadMedia](#mediareaderloadmedia-method)를 호출 합니다.

### <a name="mediareaderloadmedia-method"></a>MediaReader:: LoadMedia 메서드

이 메서드는 [미디어 파운데이션](/windows/desktop/medfound/microsoft-media-foundation-sdk) api를 사용 하 여 .wav 오디오 파일에서 PCM (펄스 코드 변조) 버퍼로 읽습니다.

#### <a name="set-up-the-source-reader"></a>원본 판독기 설정

1. [MFCreateSourceReaderFromURL](/windows/desktop/api/mfreadwrite/nf-mfreadwrite-mfcreatesourcereaderfromurl) 를 사용 하 여[IMFSourceReader](/windows/desktop/api/mfreadwrite/nn-mfreadwrite-imfsourcereader)(미디어 원본 판독기)를 만듭니다.
2. [MFCreateMediaType](/windows/desktop/api/mfapi/nf-mfapi-mfcreatemediatype) 를 사용 하 여 미디어 유형 ([imfmediatype](/windows/desktop/api/mfobjects/nn-mfobjects-imfmediatype)) 개체 (_mediaType_)를 만듭니다. 미디어 형식에 대 한 설명을 나타냅니다. 
3. _미디어 미디어_의 디코딩된 출력을 __XAudio2__ 에서 사용할 수 있는 오디오 유형인 PCM 오디오로 지정 합니다.
4. [IMFSourceReader:: SetCurrentMediaType](/windows/desktop/api/mfreadwrite/nf-mfreadwrite-imfsourcereader-setcurrentmediatype)를 호출 하 여 원본 판독기에 대해 디코딩된 출력 미디어 형식을 설정 합니다.

원본 판독기를 사용 하는 이유에 대 한 자세한 내용은 [소스 판독기](/windows/desktop/medfound/source-reader)로 이동 하세요.

#### <a name="describe-the-data-format-of-the-audio-stream"></a>오디오 스트림의 데이터 형식 설명

1. [IMFSourceReader:: GetCurrentMediaType](/windows/desktop/api/mfreadwrite/nf-mfreadwrite-imfsourcereader-getcurrentmediatype) 을 사용 하 여 스트림의 현재 미디어 유형을 가져옵니다.
2. 이전 작업의 결과를 입력으로 사용 하 여 현재 오디오 미디어 형식을 [WAVEFORMATEX](/windows/desktop/api/mmreg/ns-mmreg-twaveformatex) 버퍼로 변환 하려면 [Imfmediatype:: MFCreateWaveFormatExFromMFMediaType](/windows/desktop/api/mfapi/nf-mfapi-mfcreatewaveformatexfrommfmediatype) 를 사용 합니다. 이 구조는 오디오가 로드 된 후 사용 되는 웨이브 오디오 스트림의 데이터 형식을 지정 합니다. 

__WAVEFORMATEX__ 형식은 PCM 버퍼를 설명 하는 데 사용할 수 있습니다. [WAVEFORMATEXTENSIBLE](/windows-hardware/drivers/ddi/content/ksmedia/ns-ksmedia-waveformatextensible) 구조체와 비교 하 여 오디오 웨이브 형식의 하위 집합을 설명 하는 데만 사용할 수 있습니다. __WAVEFORMATEX__ 와 __WAVEFORMATEXTENSIBLE__의 차이점에 대 한 자세한 내용은 [확장 가능한 Wave 형식 설명자](/windows-hardware/drivers/audio/extensible-wave-format-descriptors)를 참조 하세요.

#### <a name="read-the-audio-stream"></a>오디오 스트림 읽기

1.  [IMFSourceReader:: GetPresentationAttribute](/windows/desktop/api/mfreadwrite/nf-mfreadwrite-imfsourcereader-getpresentationattribute) 를 호출 하 여 오디오 스트림의 지속 시간 (초)을 가져온 다음 지속 시간을 바이트로 변환 합니다.
2.  [IMFSourceReader:: ReadSample](/windows/desktop/api/mfreadwrite/nf-mfreadwrite-imfsourcereader-readsample)을 호출 하 여의 오디오 파일을 스트림으로 읽습니다. __Readsample__ 은 미디어 원본에서 다음 샘플을 읽습니다.
3.  [Imfsample:: ConvertToContiguousBuffer](/windows/desktop/api/mfobjects/nf-mfobjects-imfsample-converttocontiguousbuffer) 를 사용 하 여 오디오 샘플 버퍼 (_샘플_)의 콘텐츠를 배열 (_mediaBuffer_)에 복사 합니다.

```cppwinrt
std::vector<byte> MediaReader::LoadMedia(_In_ winrt::hstring const& filename)
{
    winrt::check_hresult(
        MFStartup(MF_VERSION)
        );

    // Creates a media source reader.
    winrt::com_ptr<IMFSourceReader> reader;
    winrt::check_hresult(
        MFCreateSourceReaderFromURL(
        (m_installedLocationPath + filename).c_str(),
            nullptr,
            reader.put()
            )
        );

    // Set the decoded output format as PCM.
    // XAudio2 on Windows can process PCM and ADPCM-encoded buffers.
    // When using MediaFoundation, this sample always decodes into PCM.
    winrt::com_ptr<IMFMediaType> mediaType;
    winrt::check_hresult(
        MFCreateMediaType(mediaType.put())
        );

    // Define the major category of the media as audio. For more info about major media types,
    // go to: https://msdn.microsoft.com/library/windows/desktop/aa367377.aspx
    winrt::check_hresult(
        mediaType->SetGUID(MF_MT_MAJOR_TYPE, MFMediaType_Audio)
        );

    // Define the sub-type of the media as uncompressed PCM audio. For more info about audio sub-types,
    // go to: https://msdn.microsoft.com/library/windows/desktop/aa372553.aspx
    winrt::check_hresult(
        mediaType->SetGUID(MF_MT_SUBTYPE, MFAudioFormat_PCM)
        );

    // Sets the media type for a stream. This media type defines that format that the Source Reader 
    // produces as output. It can differ from the native format provided by the media source.
    // For more info, go to https://msdn.microsoft.com/library/windows/desktop/dd374667.aspx
    winrt::check_hresult(
        reader->SetCurrentMediaType(static_cast<uint32_t>(MF_SOURCE_READER_FIRST_AUDIO_STREAM), 0, mediaType.get())
        );

    // Get the current media type for the stream.
    // For more info, go to:
    // https://msdn.microsoft.com/library/windows/desktop/dd374660.aspx
    winrt::com_ptr<IMFMediaType> outputMediaType;
    winrt::check_hresult(
        reader->GetCurrentMediaType(static_cast<uint32_t>(MF_SOURCE_READER_FIRST_AUDIO_STREAM), outputMediaType.put())
        );

    // Converts the current media type into the WaveFormatEx buffer structure.
    UINT32 size = 0;
    WAVEFORMATEX* waveFormat;
    winrt::check_hresult(
        MFCreateWaveFormatExFromMFMediaType(outputMediaType.get(), &waveFormat, &size)
        );

    // Copies the waveFormat's block of memory to the starting address of the m_waveFormat variable in MediaReader.
    // Then free the waveFormat memory block.
    // For more info, go to https://msdn.microsoft.com/library/windows/desktop/aa366535.aspx and
    // https://msdn.microsoft.com/library/windows/desktop/ms680722.aspx
    CopyMemory(&m_waveFormat, waveFormat, sizeof(m_waveFormat));
    CoTaskMemFree(waveFormat);

    PROPVARIANT propVariant;
    winrt::check_hresult(
        reader->GetPresentationAttribute(static_cast<uint32_t>(MF_SOURCE_READER_MEDIASOURCE), MF_PD_DURATION, &propVariant)
        );

    // 'duration' is in 100ns units; convert to seconds, and round up
    // to the nearest whole byte.
    LONGLONG duration = propVariant.uhVal.QuadPart;
    unsigned int maxStreamLengthInBytes =
        static_cast<unsigned int>(
            ((duration * static_cast<ULONGLONG>(m_waveFormat.nAvgBytesPerSec)) + 10000000) /
            10000000
            );

    std::vector<byte> fileData(maxStreamLengthInBytes);

    winrt::com_ptr<IMFSample> sample;
    winrt::com_ptr<IMFMediaBuffer> mediaBuffer;
    DWORD flags = 0;

    int positionInData = 0;
    bool done = false;
    while (!done)
    {
        // Read audio data.
        ...
    }

    return fileData;
}
```

## <a name="associate-sound-to-object"></a>개체에 소리 연결

소리를 개체에 연결 하면 게임이 초기화 될 때 [Simple3DGame:: Initialize](#simple3dgameinitialize-method) 메서드에서 발생 합니다.

개요:
* __GameObject__ 클래스에는 소리 효과를 개체에 연결 하는 데 사용 되는 __HitSound__ 속성이 있습니다.
* [SoundEffect](#soundeffecth) class 개체의 새 인스턴스를 만들고 game 개체와 연결 합니다. 이 클래스는 __XAudio2__ api를 사용 하 여 소리를 재생 합니다.  [Audio](#audioh) 클래스에서 제공 하는 마스터링 음성을 사용 합니다. [미디어 판독기](#mediareaderh) 클래스를 사용 하 여 파일 위치에서 사운드 데이터를 읽을 수 있습니다.

[SoundEffect:: Initialize](#soundeffectinitialize-method) 는 다음과 같은 입력 매개 변수를 사용 하 여 __SoundEffect__ 인스턴스를 초기화 하는 데 사용 됩니다. 소리 엔진 개체에 대 한 포인터 ( [Audio:: CreateDeviceIndependentResources](#audiocreatedeviceindependentresources-method) 메서드에서 만든 IXAudio2 개체), __mediareader__를 사용 하 여 .wav 파일의 형식에 대 한 포인터,: GetOutputWaveFormatEx 및 [mediareader:: loadmedia](#mediareaderloadmedia-method) 메서드를 사용 하 여 로드 된 사운드 데이터 초기화 하는 동안 음향 효과에 대 한 소스 음성도 만들어집니다.

### <a name="soundeffectinitialize-method"></a>SoundEffect:: Initialize 메서드

```cppwinrt
void SoundEffect::Initialize(
    _In_ IXAudio2* masteringEngine,
    _In_ WAVEFORMATEX* sourceFormat,
    _In_ std::vector<byte> const& soundData)
{
    m_soundData = soundData;

    if (masteringEngine == nullptr)
    {
        // Audio is not available so just return.
        m_audioAvailable = false;
        return;
    }

    // Create a source voice for this sound effect.
    winrt::check_hresult(
        masteringEngine->CreateSourceVoice(
            &m_sourceVoice,
            sourceFormat
            )
        );
    m_audioAvailable = true;
}
```

## <a name="play-the-sound"></a>소리 재생

소리 효과를 재생 하는 트리거는 [Simple3DGame:: UpdateDynamics](#simple3dgameupdatedynamics-method) 메서드에 정의 됩니다. 여기에는 개체의 이동이 업데이트 되 고 개체 간의 충돌이 결정 되기 때문입니다.

개체 간의 상호 작용은 게임에 따라 크게 달라 지므로 여기에서는 게임 개체의 dynamics에 대해 설명 하지 않습니다. 구현을 이해 하려면 [Simple3DGame:: UpdateDynamics](#simple3dgameupdatedynamics-method) 메서드를 참조 하세요.

원칙적으로 충돌이 발생 하면 **SoundEffect::P laysound**를 호출 하 여 재생에 대 한 음향 효과를 트리거합니다. 이 메서드는 현재 재생 중인 모든 소리 효과를 중지 하 고 원하는 사운드 데이터로 메모리 내 버퍼를 큐에 대기 시킵니다. 원본 음성을 사용 하 여 볼륨을 설정 하 고, 사운드 데이터를 제출 하 고, 재생을 시작 합니다.

### <a name="soundeffectplaysound-method"></a>SoundEffect::P laySound 메서드

* 원본 음성 개체 **m \_ sourcevoice** 를 사용 하 여 사운드 데이터 버퍼 **m \_ soundData** 의 재생을 시작 합니다.
* 는 사운드 데이터 버퍼에 대 한 참조를 제공 하는 [XAUDIO2 \_ 버퍼](/windows/desktop/api/xaudio2/ns-xaudio2-xaudio2_buffer)를 만든 다음 [IXAudio2SourceVoice:: submitsourcebuffer](/windows/desktop/api/xaudio2/nf-xaudio2-ixaudio2sourcevoice-submitsourcebuffer)를 호출 하 여 해당 버퍼를 전송 합니다. 
* 사운드 데이터를 큐에 **SoundEffect::P Laysound** [IXAudio2SourceVoice:: Start](/windows/desktop/api/xaudio2/nf-xaudio2-ixaudio2sourcevoice-start)를 호출 하 여 재생을 시작 합니다.

```cppwinrt
void SoundEffect::PlaySound(_In_ float volume)
{
    XAUDIO2_BUFFER buffer = { 0 };

    if (!m_audioAvailable)
    {
        // Audio is not available so just return.
        return;
    }

    // Interrupt sound effect if it is currently playing.
    winrt::check_hresult(
        m_sourceVoice->Stop()
        );
    winrt::check_hresult(
        m_sourceVoice->FlushSourceBuffers()
        );

    // Queue the memory buffer for playback and start the voice.
    buffer.AudioBytes = (UINT32)m_soundData.size();
    buffer.pAudioData = m_soundData.data();
    buffer.Flags = XAUDIO2_END_OF_STREAM;

    winrt::check_hresult(
        m_sourceVoice->SetVolume(volume)
        );
    winrt::check_hresult(
        m_sourceVoice->SubmitSourceBuffer(&buffer)
        );
    winrt::check_hresult(
        m_sourceVoice->Start()
        );
}
```

### <a name="simple3dgameupdatedynamics-method"></a>Simple3DGame:: UpdateDynamics 메서드

__Simple3DGame:: UpdateDynamics__ 메서드는 게임 개체 간의 상호 작용 및 충돌을 처리 합니다. 개체가 충돌 하거나 교차 하면 연결 된 음향 효과를 트리거하여 재생 합니다.

```cppwinrt
void Simple3DGame::UpdateDynamics()
{
    ...
    // Check for collisions between ammo.
#pragma region inter-ammo collision detection
if (m_ammoCount > 1)
{
    ...
    // Check collision between instances One and Two.
    ...
    if (distanceSquared < (GameConstants::AmmoSize * GameConstants::AmmoSize))
    {
        // The two ammo are intersecting.
        ...
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
        ...
        // Play the sound associated with the Ammo hitting something.
        m_objects[i]->PlaySound(impact, m_player->Position());

        if (m_objects[i]->Target() && !m_objects[i]->Hit())
        {
            // The object is a target and isn't currently hit, so mark
            // it as hit and play the sound associated with the impact.
            m_objects[i]->Hit(true);
            m_objects[i]->HitTime(timeTotal);
            m_totalHits++;

            m_objects[i]->PlaySound(impact, m_player->Position());
        }
        ...
    }
#pragma endregion

#pragma region Apply Gravity and world intersection
            // Apply gravity and check for collision against enclosing volume.
            ...
                if (position.z < limit)
                {
                    // The ammo instance hit the a wall in the min Z direction.
                    // Align the ammo instance to the wall, invert the Z component of the velocity and
                    // play the impact sound.
                    position.z = limit;
                    m_ammo[i]->PlaySound(-velocity.z, m_player->Position());
                    velocity.z = -velocity.z * GameConstants::Physics::GroundRestitution;
                }
                ...
#pragma endregion
}
```

## <a name="next-steps"></a>다음 단계

Windows 10 게임의 UWP 프레임 워크, 그래픽, 컨트롤, 사용자 인터페이스 및 오디오에 대해 살펴보았습니다. [샘플 게임을 확장](tutorial-resources.md)하는이 자습서의 다음 부분에서는 게임을 개발할 때 사용할 수 있는 다른 옵션을 설명 합니다.

## <a name="audio-concepts"></a>오디오 개념

Windows 10 게임 개발의 경우 XAudio2 버전 2.9을 사용 합니다. 이 버전은 Windows 10과 함께 제공 됩니다. 자세한 내용은 [XAudio2 버전](/windows/desktop/xaudio2/xaudio2-versions)을 참조 하세요.

__AudioX2__ 는 신호 처리 및 혼합 토대를 제공 하는 하위 수준 API입니다. 자세한 내용은 [XAudio2 주요 개념](/windows/desktop/xaudio2/xaudio2-key-concepts)을 참조 하세요.

### <a name="xaudio2-voices"></a>XAudio2 음성

XAudio2 음성 개체에는 원본, 서브 믹스 및 마스터링 음성의 세 가지 유형이 있습니다. 음성은 오디오 데이터를 처리 하 고 조작 하 고 재생 하는 데 사용 되는 XAudio2 개체입니다. 
* 원본 음성은 클라이언트에서 제공 하는 오디오 데이터에 대해 작동 합니다. 
* 원본 및 서브 믹스 음성은 하나 이상의 서브 믹스 또는 마스터링 음성으로 출력을 보냅니다. 
* 서브 믹스 및 마스터링 음성은 오디오를 공급 하는 모든 음성에서 오디오를 혼합 하 고 결과에 대해 작동 합니다. 
* 마스터링 음성은 원본 음성 및 서브 믹스 음성에서 데이터를 수신 하 고 해당 데이터를 오디오 하드웨어로 보냅니다.

자세한 내용은 [XAudio2 음성](/windows/desktop/xaudio2/xaudio2-voices)을 참조 하세요.

### <a name="audio-graph"></a>오디오 그래프

오디오 그래프는 [XAudio2 음성](/windows/desktop/xaudio2/xaudio2-voices)의 컬렉션입니다. 오디오는 소스 음성에서 오디오 그래프의 한쪽에서 시작 하 고, 필요에 따라 하나 이상의 서브 믹스 음성을 통과 하 고, 마스터링 음성에서 끝납니다. 오디오 그래프에는 현재 재생 중인 각 사운드, 0 개 이상의 서브 믹스 음성 및 1 개의 마스터링 음성이 포함 된 원본 음성이 포함 됩니다. 가장 간단한 오디오 그래프와 XAudio2에서 노이즈를 만드는 데 필요한 최소는 마스터링 음성으로 직접 출력 되는 단일 원본 음성입니다. 자세한 내용을 보려면 [오디오 그래프](/windows/desktop/xaudio2/audio-graphs)로 이동 하세요.

### <a name="additional-reading"></a>추가 참조 항목

* [방법: XAudio2 초기화](/windows/desktop/xaudio2/how-to--initialize-xaudio2)
* [방법: XAudio2에서 오디오 데이터 파일 로드](/windows/desktop/xaudio2/how-to--load-audio-data-files-in-xaudio2)
* [방법: XAudio2를 사용 하 여 소리 재생](/windows/desktop/xaudio2/how-to--play-a-sound-with-xaudio2)

## <a name="key-audio-h-files"></a>키 오디오 .h 파일

### <a name="audioh"></a>오디오 .h

```cppwinrt
// Audio:
// This class uses XAudio2 to provide sound output. It creates two
// engines - one for music and the other for sound effects - each as
// a separate mastering voice.
// The SuspendAudio and ResumeAudio methods can be used to stop
// and start all audio playback.

class Audio
{
public:
    Audio();

    void Initialize();
    void CreateDeviceIndependentResources();
    IXAudio2* MusicEngine();
    IXAudio2* SoundEffectEngine();
    void SuspendAudio();
    void ResumeAudio();

private:
    ...
};
```

### <a name="mediareaderh"></a>MediaReader. h

```cppwinrt
// MediaReader:
// This is a helper class for the SoundEffect class. It reads small audio files
// synchronously from the package installed folder and returns sound data as a
// vector of bytes.

class MediaReader
{
public:
    MediaReader();

    std::vector<byte> LoadMedia(_In_ winrt::hstring const& filename);
    WAVEFORMATEX* GetOutputWaveFormatEx();

private:
    winrt::Windows::Storage::StorageFolder  m_installedLocation{ nullptr };
    winrt::hstring                          m_installedLocationPath;
    WAVEFORMATEX                            m_waveFormat;
};
```

### <a name="soundeffecth"></a>SoundEffect

```cppwinrt
// SoundEffect:
// This class plays a sound using XAudio2. It uses a mastering voice provided
// from the Audio class. The sound data can be read from disk using the MediaReader
// class.

class SoundEffect
{
public:
    SoundEffect();

    void Initialize(
        _In_ IXAudio2* masteringEngine,
        _In_ WAVEFORMATEX* sourceFormat,
        _In_ std::vector<byte> const& soundData
        );

    void PlaySound(_In_ float volume);

private:
    ...
};
```
