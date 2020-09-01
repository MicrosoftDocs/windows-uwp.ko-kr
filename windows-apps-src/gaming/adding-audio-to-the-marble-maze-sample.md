---
title: Marble Maze 샘플에 오디오 추가
description: 이 문서에서는 오디오를 사용할 때 고려해 야 할 주요 방법에 대해 설명 하 고, 대리석이 이러한 사례를 적용 하는 방법을 보여 줍니다.
ms.assetid: 77c23d0a-af6d-17b5-d69e-51d9885b0d44
ms.date: 10/18/2017
ms.topic: article
keywords: windows 10, uwp, 오디오, 게임, 샘플
ms.localizationpriority: medium
ms.openlocfilehash: df1be02cd962f086e52609550e9cae65e5280b71
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89172137"
---
# <a name="adding-audio-to-the-marble-maze-sample"></a>Marble Maze 샘플에 오디오 추가

이 문서에서는 오디오를 사용할 때 고려해 야 할 주요 방법에 대해 설명 하 고, 대리석이 이러한 사례를 적용 하는 방법을 보여 줍니다. 대리석 미로는 [Microsoft 미디어 파운데이션](/windows/desktop/medfound/microsoft-media-foundation-sdk) 을 사용 하 여 파일에서 오디오 리소스를 로드 하 고 오디오를 혼합 및 재생 하 고 오디오에 효과를 적용 [XAudio2](/windows/desktop/xaudio2/xaudio2-apis-portal) .

대리석은 배경에서 음악을 재생 하 고 게임 이벤트 (예: 대리석이 벽에 도달 하는 경우)를 나타내기 위해 게임 플레이 사운드를 사용 합니다. 이 구현의 중요 한 부분은 대리석에서 반향 하는 경우 대리석 화면을 사용 하 여 대리석의 소리를 시뮬레이션 하는 것입니다. 반향 효과를 구현 하면 에코에서 더 빠르고 loudly 작은 방에 연결 됩니다. 에코는 더 조용한 공간에서 더 느리게 전달 됩니다.

> [!NOTE]
> 이 문서에 해당 하는 샘플 코드는 [DirectX 대리석 미로 game 샘플](https://github.com/microsoft/Windows-appsample-marble-maze)에서 찾을 수 있습니다.

게임에서 오디오로 작업할 때이 문서에서 설명 하는 몇 가지 주요 사항은 다음과 같습니다.

- 미디어 파운데이션를 사용 하 여 오디오 자산과 XAudio2 오디오 재생을 디코딩하는 것이 좋습니다. 그러나 UWP (유니버설 Windows 플랫폼) 앱에서 작동 하는 기존 오디오 자산 로드 메커니즘이 있는 경우이를 사용할 수 있습니다.

- 오디오 그래프에는 각 활성 사운드, 0 개 이상의 서브 믹스 음성 및 1 개의 마스터링 음성이 포함 된 원본 음성이 하나씩 들어 있습니다. 원본 음성이 서브 믹스 음성 및/또는 마스터링 음성에 피드 될 수 있습니다. 서브 믹스 음성을 다른 서브 믹스 음성 또는 마스터링 음성으로 피드 합니다.

- 배경 음악 파일이 클 경우 적은 양의 메모리를 사용 하도록 음악을 작은 버퍼로 스트리밍하는 것이 좋습니다.

- 이 작업을 수행 하는 것이 적합 한 경우 앱이 포커스를 잃거나 표시 되지 않거나 일시 중단 되 면 오디오 재생을 일시 중지 합니다. 앱이 포커스를 다시 얻으면 표시 되거나 다시 시작 될 때 재생을 다시 시작 합니다.

- 각 소리의 역할을 반영 하도록 오디오 범주를 설정 합니다. 예를 들어 일반적으로 음향 효과를 위해 game background audio 및 **AudioCategory \_ GameEffects** 에 **AudioCategory \_ GameMedia** 를 사용 합니다.

- 모든 오디오 리소스와 인터페이스를 해제 하 고 다시 만들어 헤드폰을 비롯 한 장치 변경을 처리 합니다.

- 디스크 공간과 스트리밍 비용을 최소화할 때 오디오 파일을 압축할지 여부를 고려 합니다. 그렇지 않으면 오디오를 압축 되지 않은 상태로 유지 하 여 더 빠르게 로드할 수 있습니다.

## <a name="introducing-xaudio2-and-microsoft-media-foundation"></a>XAudio2 및 Microsoft 미디어 파운데이션 소개

XAudio2는 게임 오디오를 특별히 지 원하는 Windows 용 하위 수준 오디오 라이브러리입니다. 게임을 위한 DSP (디지털 신호 처리) 및 오디오 그래프 엔진을 제공 합니다. XAudio2는 SIMD 부동 소수점 아키텍처 및 HD 오디오와 같은 컴퓨팅 추세를 지원 하 여 선행, DirectSound 및 XAudio를 확장 합니다. 또한 오늘날 게임의 더 복잡 한 소리 처리 요구를 지원 합니다.

문서 [XAudio2 주요 개념](/windows/desktop/xaudio2/xaudio2-key-concepts) 에서는 XAudio2 사용에 대 한 주요 개념을 설명 합니다. 간단히 말해서 개념은 다음과 같습니다.

- [IXAudio2](/windows/desktop/api/xaudio2/nn-xaudio2-ixaudio2) 인터페이스는 XAudio2 엔진의 핵심입니다. 대리석 미로는이 인터페이스를 사용 하 여 음성을 만들고 출력 장치가 변경 되거나 실패 하는 경우 알림을 받습니다.

- **음성** 은 오디오 데이터를 처리 하 고 조정 하 고 재생 합니다.

- **원본 음성** 은 오디오 채널 (mono, 5.1 등)의 컬렉션으로, 오디오 데이터의 한 스트림을 나타냅니다. XAudio2에서 원본 음성은 오디오 처리가 시작 되는 위치입니다. 일반적으로 소리 데이터는 파일 또는 네트워크와 같은 외부 원본에서 로드 되 고 원본 음성으로 전송 됩니다. 대리석 미로는 [미디어 파운데이션](/windows/desktop/medfound/microsoft-media-foundation-sdk) 을 사용 하 여 파일에서 사운드 데이터를 로드 합니다. 미디어 파운데이션는이 문서의 뒷부분에서 도입 되었습니다.

- **서브 믹스 음성** 은 오디오 데이터를 처리 합니다. 이 처리에는 오디오 스트림을 변경 하거나 여러 스트림을 하나로 결합 하는 작업이 포함 될 수 있습니다. 대리석 미로는 submixes을 사용 하 여 반향 효과를 만듭니다.

- **마스터링 음성** 은 원본 및 서브 믹스 음성의 데이터를 결합 하 고 해당 데이터를 오디오 하드웨어로 보냅니다.

- **오디오 그래프** 에는 각 활성 사운드, 0 개 이상의 서브 믹스 음성 및 마스터링 음성 하나에 대 한 원본 음성이 하나씩 들어 있습니다.

- **콜백은** 일부 이벤트가 음성 또는 엔진 개체에서 발생 했음을 클라이언트 코드에 알립니다. 콜백을 사용 하 여 XAudio2가 버퍼로 완료 되 면 메모리를 다시 사용 하 고 오디오 장치가 변경 될 때 (예: 헤드폰 연결 또는 연결 해제)에 대응할 수 있습니다. 이 문서의 뒷부분에 있는 [헤드폰 및 장치 변경 처리](#handling-headphones-and-device-changes) 에서는 대리석에서이 메커니즘을 사용 하 여 장치 변경을 처리 하는 방법을 설명 합니다.

대리석 미로는 오디오를 처리 하는 두 개의 오디오 엔진 (즉, 두 개의 [IXAudio2](/windows/desktop/api/xaudio2/nn-xaudio2-ixaudio2) 개체)을 사용 합니다. 한 엔진은 배경 음악을 처리 하 고 다른 엔진은 게임 플레이 소리를 처리 합니다.

또한 대리석 메 이즈는 각 엔진에 대해 하나의 마스터링 음성을 만들어야 합니다. 마스터링 엔진은 오디오 스트림을 하나의 스트림으로 결합 하 고 해당 스트림을 오디오 하드웨어로 보냅니다. 배경 음악 스트림 인 원본 음성은 마스터링 음성 및 두 개의 서브 믹스 음성으로 데이터를 출력 합니다. 서브 믹스 음성은 반향 효과를 수행 합니다.

미디어 파운데이션은 많은 오디오 및 비디오 형식을 지 원하는 멀티미디어 라이브러리입니다. XAudio2 및 미디어 파운데이션 상호 보완적입니다. 대리석 미로는 미디어 파운데이션을 사용 하 여 파일에서 오디오 자산을 로드 하 고 오디오를 재생 하는 데 XAudio2를 사용 합니다. 오디오 자산을 로드 하는 데 미디어 파운데이션를 사용할 필요가 없습니다. UWP (유니버설 Windows 플랫폼) 앱에서 작동 하는 기존 오디오 자산 로드 메커니즘이 있는 경우이를 사용 합니다. [오디오, 비디오 및 카메라](../audio-video-camera/index.md) 는 UWP 앱에서 오디오를 구현 하는 여러 가지 방법에 대해 설명 합니다.

XAudio2에 대 한 자세한 내용은 [프로그래밍 가이드](/windows/desktop/xaudio2/programming-guide)를 참조 하세요. 미디어 파운데이션에 대 한 자세한 내용은 [Microsoft 미디어 파운데이션](/windows/desktop/medfound/microsoft-media-foundation-sdk)을 참조 하세요.

## <a name="initializing-audio-resources"></a>오디오 리소스 초기화

대리석 Mazes은 배경 음악에는 Windows Media 오디오 (.wma) 파일을 사용 하 고 게임 사운드에는 WAV 파일 (.wav)을 사용 합니다. 이러한 형식은 미디어 파운데이션 지원 됩니다. .Wav 파일 형식은 기본적으로 XAudio2에서 지원 되지만, 게임은 파일 형식을 수동으로 구문 분석 하 여 적절 한 XAudio2 데이터 구조를 채워야 합니다. 대리석 미로는 미디어 파운데이션을 사용 하 여 .wav 파일을 보다 쉽게 사용할 수 있습니다. 미디어 파운데이션에서 지 원하는 미디어 형식의 전체 목록은 [미디어 파운데이션에서 지원 되는 미디어 형식](/windows/desktop/medfound/supported-media-formats-in-media-foundation)을 참조 하세요. 대리석의 미로는 별도의 디자인 타임 및 런타임 오디오 형식을 사용 하지 않으며 XAudio2 ADPCM 압축 지원을 사용 하지 않습니다. XAudio2의 ADPCM 압축에 대 한 자세한 내용은 [Adpcm 개요](/windows/desktop/xaudio2/adpcm-overview)를 참조 하세요.

**MarbleMazeMain:: LoadDeferredResources**에서 호출 되는 **Audio:: createresources.vb** 메서드는 파일에서 오디오 스트림을 로드 하 고, XAudio2 엔진 개체를 초기화 하 고, 소스, 서브 믹스 및 마스터링 음성을 만듭니다.

### <a name="creating-the-xaudio2-engines"></a>XAudio2 엔진 만들기

대리석은 사용 하는 각 오디오 엔진을 나타내는 하나의 [IXAudio2](/windows/desktop/api/xaudio2/nn-xaudio2-ixaudio2) 개체를 만듭니다. 오디오 엔진을 만들려면 [XAudio2Create](/windows/desktop/api/xaudio2/nf-xaudio2-xaudio2create) 메서드를 호출 합니다. 다음 예제에서는 대리석이 배경 음악을 처리 하는 오디오 엔진을 만드는 방법을 보여 줍니다.

```cpp
// In Audio.h
class Audio
{
private:
    IXAudio2*                   m_musicEngine;
// ...
}

// In Audio.cpp
void Audio::CreateResources()
{
    try
    {
        // ...
        DX::ThrowIfFailed(
            XAudio2Create(&m_musicEngine)
            );
        // ...
    }
    // ...
}
```

대리석 미로는 비슷한 단계를 수행 하 여 게임 사운드를 재생 하는 오디오 엔진을 만듭니다.

UWP 앱에서 [IXAudio2](/windows/desktop/api/xaudio2/nn-xaudio2-ixaudio2) 인터페이스를 사용 하는 방법은 두 가지 방법으로 데스크톱 앱과 다릅니다. 먼저 [XAudio2Create](/windows/desktop/api/xaudio2/nf-xaudio2-xaudio2create)를 호출 하기 전에 [CoInitializeEx](/windows/desktop/api/combaseapi/nf-combaseapi-coinitializeex) 를 호출할 필요가 없습니다. 또한 **IXAudio2** 는 더 이상 장치 열거를 지원 하지 않습니다. 오디오 장치를 열거 하는 방법에 대 한 자세한 내용은 [장치 열거](/previous-versions/windows/apps/hh464977(v=win.10))를 참조 하세요.

### <a name="creating-the-mastering-voices"></a>마스터링 음성 만들기

다음 예제에서는 **Audio:: createresources.vb** 메서드가 [IXAudio2:: CreateMasteringVoice](/windows/desktop/api/xaudio2/nf-xaudio2-ixaudio2-createmasteringvoice) 메서드를 사용 하 여 배경 음악에 대 한 마스터링 음성을 만드는 방법을 보여 줍니다. 이 예제에서 **m \_ MusicMasteringVoice** 는 [IXAudio2MasteringVoice](/windows/desktop/api/xaudio2/nn-xaudio2-ixaudio2masteringvoice) 개체입니다. 두 개의 입력 채널을 지정 합니다. 그러면 반향 효과에 대 한 논리가 간단해 집니다. 

48000을 입력 샘플 속도로 지정 합니다. 이 샘플링 주기는 오디오 품질과 필요한 CPU 처리의 양에 균형을 유지 하기 때문에 선택 했습니다. 더 큰 샘플 속도는 상당한 품질의 이점을 제공 하지 않고 더 많은 CPU 처리가 필요 합니다. 

마지막으로, 사용자가 게임을 재생 하는 동안 다른 응용 프로그램에서 음악을 수신할 수 있도록 오디오 스트림 범주로 **AudioCategory_GameMedia** 를 지정 합니다. 음악 앱을 재생 하는 경우 Windows는 **AudioCategory \_ GameMedia** 옵션에 의해 생성 된 모든 음성을 mutes 합니다. **AudioCategory \_ GameEffects** 옵션을 사용 하 여 생성 되었기 때문에 사용자는 게임 소리를 계속 듣게 됩니다. 오디오 범주에 대 한 자세한 내용은 [오디오 \_ 스트림 \_ 범주](/windows/desktop/api/audiosessiontypes/ne-audiosessiontypes-_audio_stream_category)를 참조 하세요.

```cpp
// This sample plays the equivalent of background music, which we tag on the  
// mastering voice as AudioCategory_GameMedia. In ordinary usage, if we were  
// playing the music track with no effects, we could route it entirely through 
// Media Foundation. Here, we are using XAudio2 to apply a reverb effect to the 
// music, so we use Media Foundation to decode the data then we feed it through 
// the XAudio2 pipeline as a separate Mastering Voice, so that we can tag it 
// as Game Media. We default the mastering voice to 2 channels to simplify  
// the reverb logic.
DX::ThrowIfFailed(
    m_musicEngine->CreateMasteringVoice(
        &m_musicMasteringVoice,
        2,
        48000,
        0,
        nullptr,
        nullptr,
        AudioCategory_GameMedia
        )
);
```

**Audio:: createresources.vb** 메서드는 *streamcategory* 매개 변수에 대 한 **AudioCategory \_ GameEffects** 를 지정 하는 경우를 제외 하 고 게임 사운드에 대해 마스터링 음성을 만드는 유사한 단계를 수행 합니다 (기본값).

### <a name="creating-the-reverb-effect"></a>반향 효과 만들기

각 음성에 대해 XAudio2를 사용 하 여 오디오를 처리 하는 효과의 시퀀스를 만들 수 있습니다. 이러한 시퀀스를 효과 체인 이라고 합니다. 음성에 효과를 하나 이상 적용 하려면 효과 체인을 사용 합니다. 효과 체인은 파괴적인 일 수 있습니다. 즉, 체인의 각 효과는 오디오 버퍼를 덮어쓸 수 있습니다. XAudio2는 출력 버퍼가 침묵를 사용 하 여 초기화 되는 것을 보장 하지 않기 때문에이 속성은 중요 합니다. 효과 개체는 XAudio2에서 플랫폼 간 오디오 처리 개체 (XAPO)로 표시 됩니다. XAPO에 대 한 자세한 내용은 [Xapo 개요](/windows/desktop/xaudio2/xapo-overview)를 참조 하세요.

효과 체인을 만들 때 다음 단계를 수행 합니다.

1. 효과 개체를 만듭니다.

2. 효과 데이터를 사용 하 여 [XAUDIO2 \_ 효과 \_ 설명자](/windows/desktop/api/xaudio2/ns-xaudio2-xaudio2_effect_descriptor) 구조를 채웁니다.

3. [XAUDIO2 \_ 효과 \_ 체인](/windows/desktop/api/xaudio2/ns-xaudio2-xaudio2_effect_chain) 구조를 데이터로 채웁니다.

4. 음성에 효과 체인을 적용 합니다.

5. 효과 매개 변수 구조를 채우고 효과에 적용 합니다.

6. 적절 한 경우에는 효과를 사용 하지 않거나 사용 하도록 설정 합니다.

**Audio** 클래스는 **createreverb** 메서드를 정의 하 여 반향을 구현 하는 효과 체인을 만듭니다. 이 메서드는 [XAudio2CreateReverb](/windows/desktop/api/xaudio2fx/nf-xaudio2fx-xaudio2createreverb) 메서드를 호출 하 여 반향 효과에 대 한 서브 믹스 음성의 역할을 하는 **ComPtr &lt; &gt; IUnknown** 개체 **soundEffectXAPO**를 만듭니다.

```cpp
Microsoft::WRL::ComPtr<IUnknown> soundEffectXAPO;

DX::ThrowIfFailed(
    XAudio2CreateReverb(&soundEffectXAPO)
    );
```

[XAUDIO2 \_ 효과 \_ 설명자](/windows/desktop/api/xaudio2/ns-xaudio2-xaudio2_effect_descriptor) 구조에는 효과 체인에서 사용 하기 위한 xapo (예: 출력 채널의 대상 수)에 대 한 정보가 포함 되어 있습니다. **Audio:: CreateReverb** 메서드는 비활성 상태로 설정 된 **XAUDIO2 \_ 효과 \_ 설명자** 개체 **soundEffectdescriptor**를 만들고, 두 개의 출력 채널을 사용 하며, 반향 효과에 대 한 **soundEffectXAPO** 를 참조 합니다. **soundEffectdescriptor** 게임 소리 수정을 시작 하기 전에 매개 변수를 설정 해야 하기 때문에 사용 안 함 상태로 시작 됩니다. 대리석 미로는 두 개의 출력 채널을 사용 하 여 반향 효과에 대 한 논리를 단순화 합니다.

```cpp
soundEffectdescriptor.InitialState = false;
soundEffectdescriptor.OutputChannels = 2;
soundEffectdescriptor.pEffect = soundEffectXAPO.Get();
```

효과 체인에 여러 효과가 있는 경우 각 효과에는 개체가 필요 합니다. [XAUDIO2 \_ 효과 \_ 체인](/windows/desktop/api/xaudio2/ns-xaudio2-xaudio2_effect_chain) 구조는 효과에 참여 하는 [XAUDIO2 \_ 효과 \_ 설명자](/windows/desktop/api/xaudio2/ns-xaudio2-xaudio2_effect_descriptor) 개체의 배열을 보유 합니다. 다음 예제에서는 **Audio:: CreateReverb** 메서드가 반향을 구현 하기 위한 하나의 효과를 지정 하는 방법을 보여 줍니다.

```cpp
XAUDIO2_EFFECT_CHAIN soundEffectChain;

// ...

soundEffectChain.EffectCount = 1;
soundEffectChain.pEffectDescriptors = &soundEffectdescriptor;
```

**Audio:: CreateReverb** 메서드는 [IXAudio2:: CreateSubmixVoice](/windows/desktop/api/xaudio2/nf-xaudio2-ixaudio2-createsubmixvoice) 메서드를 호출 하 여 효과에 대 한 서브 믹스 음성을 만듭니다. 효과 체인을 음성에 연결 하는 *pEffectChain* 매개 변수에 대 한 [XAUDIO2 \_ 효과 \_ 체인](/windows/desktop/api/xaudio2/ns-xaudio2-xaudio2_effect_chain) 개체인 **soundEffectChain**를 지정 합니다. 또한 대리석 메 이즈는 두 개의 출력 채널과 48 kilohertz의 샘플링 주기를 지정 합니다.

```cpp
DX::ThrowIfFailed(
    engine->CreateSubmixVoice(newSubmix, 2, 48000, 0, 0, nullptr, &soundEffectChain)
    );
```

> [!TIP]
> 기존 효과 체인을 기존 서브 믹스 음성에 연결 하거나 현재 효과 체인을 교체 하려는 경우 [IXAudio2Voice:: SetEffectChain](/windows/desktop/api/xaudio2/nf-xaudio2-ixaudio2voice-seteffectchain) 메서드를 사용 합니다.

**Audio:: CreateReverb** 메서드는 [IXAudio2Voice:: SetEffectParameters](/windows/desktop/api/xaudio2/nf-xaudio2-ixaudio2voice-seteffectparameters) 를 호출 하 여 효과와 연결 된 추가 매개 변수를 설정 합니다. 이 메서드는 효과와 관련 된 매개 변수 구조를 사용 합니다. 모든 반향 효과가 동일한 매개 변수를 공유 하기 때문에, 반향에 대 한 효과 매개 변수를 포함 하는 [XAUDIO2FX \_ 반향 \_ 매개 변수](/windows/desktop/api/xaudio2fx/ns-xaudio2fx-xaudio2fx_reverb_parameters) **m_reverbParametersSmall**개체는 **Audio:: Initialize** 메서드에서 초기화 됩니다. 다음 예제에서는 **Audio:: Initialize** 메서드가 거의 모든 필드 반향에 대해 반향 매개 변수를 초기화 하는 방법을 보여 줍니다.

```cpp
m_reverbParametersSmall.ReflectionsDelay = XAUDIO2FX_REVERB_DEFAULT_REFLECTIONS_DELAY;
m_reverbParametersSmall.ReverbDelay = XAUDIO2FX_REVERB_DEFAULT_REVERB_DELAY;
m_reverbParametersSmall.RearDelay = XAUDIO2FX_REVERB_DEFAULT_REAR_DELAY;
m_reverbParametersSmall.PositionLeft = XAUDIO2FX_REVERB_DEFAULT_POSITION;
m_reverbParametersSmall.PositionRight = XAUDIO2FX_REVERB_DEFAULT_POSITION;
m_reverbParametersSmall.PositionMatrixLeft = XAUDIO2FX_REVERB_DEFAULT_POSITION_MATRIX;
m_reverbParametersSmall.PositionMatrixRight = XAUDIO2FX_REVERB_DEFAULT_POSITION_MATRIX;
m_reverbParametersSmall.EarlyDiffusion = 4;
m_reverbParametersSmall.LateDiffusion = 15;
m_reverbParametersSmall.LowEQGain = XAUDIO2FX_REVERB_DEFAULT_LOW_EQ_GAIN;
m_reverbParametersSmall.LowEQCutoff = XAUDIO2FX_REVERB_DEFAULT_LOW_EQ_CUTOFF;
m_reverbParametersSmall.HighEQGain = XAUDIO2FX_REVERB_DEFAULT_HIGH_EQ_GAIN;
m_reverbParametersSmall.HighEQCutoff = XAUDIO2FX_REVERB_DEFAULT_HIGH_EQ_CUTOFF;
m_reverbParametersSmall.RoomFilterFreq = XAUDIO2FX_REVERB_DEFAULT_ROOM_FILTER_FREQ;
m_reverbParametersSmall.RoomFilterMain = XAUDIO2FX_REVERB_DEFAULT_ROOM_FILTER_MAIN;
m_reverbParametersSmall.RoomFilterHF = XAUDIO2FX_REVERB_DEFAULT_ROOM_FILTER_HF;
m_reverbParametersSmall.ReflectionsGain = XAUDIO2FX_REVERB_DEFAULT_REFLECTIONS_GAIN;
m_reverbParametersSmall.ReverbGain = XAUDIO2FX_REVERB_DEFAULT_REVERB_GAIN;
m_reverbParametersSmall.DecayTime = XAUDIO2FX_REVERB_DEFAULT_DECAY_TIME;
m_reverbParametersSmall.Density = XAUDIO2FX_REVERB_DEFAULT_DENSITY;
m_reverbParametersSmall.RoomSize = XAUDIO2FX_REVERB_DEFAULT_ROOM_SIZE;
m_reverbParametersSmall.WetDryMix = XAUDIO2FX_REVERB_DEFAULT_WET_DRY_MIX;
m_reverbParametersSmall.DisableLateField = TRUE;
```

이 예제에서는 대부분의 반향 매개 변수에 대 한 기본값을 사용 하지만 **DisableLateField** 를 TRUE로 설정 하 여 근거리 **반향를 지정** 하 고, 값을 4로 설정 하 여 플랫 주변 표면을 시뮬레이션 하 고, **LateDiffusion** 를 15로 설정 하 여 멀리 떨어져 있는 서피스를 시뮬레이션 합니다. 평면 가까이 표면에서 에코를 더 신속 하 고 loudly 수 있습니다. 멀리 떨어져 있는 표면을 확산 하 고 더 천천히 이동 하 게 됩니다. 반향 값으로 실험 하 여 게임에서 원하는 효과를 얻거나 **ReverbConvertI3DL2ToNative** 메서드를 사용 하 여 업계 표준 I3DL2 (대화형 3D 오디오 렌더링 지침 수준 2.0) 매개 변수를 사용할 수 있습니다.

다음 예제에서는 **Audio:: CreateReverb** 가 반향 매개 변수를 설정 하는 방법을 보여 줍니다. **Newsubmix** [IXAudio2SubmixVoice](/windows/desktop/api/xaudio2/nn-xaudio2-ixaudio2submixvoice)* * 개체입니다. **매개 변수** 는 [XAUDIO2FX \_ 반향 \_ 매개 변수](/windows/desktop/api/xaudio2fx/ns-xaudio2fx-xaudio2fx_reverb_parameters)* 개체입니다.

```cpp
DX::ThrowIfFailed(
    (*newSubmix)->SetEffectParameters(0, parameters, sizeof(m_reverbParametersSmall))
    );
```

**Audio:: CreateReverb** 메서드는 **enableeffect** 플래그가 설정 된 경우 [IXAudio2Voice:: enableeffect](/windows/desktop/api/xaudio2/nf-xaudio2-ixaudio2voice-enableeffect) 를 사용 하 여 효과를 사용 하도록 설정 하 여 완료 됩니다. 또한 [IXAudio2Voice:: SetOutputMatrix](/windows/desktop/api/xaudio2/nf-xaudio2-ixaudio2voice-setoutputmatrix)를 사용 하 여 [IXAudio2Voice:: setvolume](/windows/desktop/api/xaudio2/nf-xaudio2-ixaudio2voice-setvolume) 및 output 행렬을 사용 하 여 볼륨을 설정 합니다. 이 부분에서는 볼륨을 전체 (1.0)로 설정 하 고 왼쪽 및 오른쪽 입력 및 왼쪽 및 오른쪽 출력 스피커 모두에 대해 볼륨 매트릭스를 침묵로 지정 합니다. 이 작업은 나중에 다른 코드가 두 reverbs (벽 근처의 전환을 시뮬레이션 하는 것을 시뮬레이션 하는 것을 시뮬레이션) 간에 교차 페이드를 수행 하기 때문에이 작업을 수행 합니다. 반향 경로가 나중에 unmuted 게임은 {1.0 f, 0.0 f, 0.0 f, 1.0 f}의 매트릭스를 설정 하 여 왼쪽 반향 출력을 마스터링 음성의 왼쪽 입력으로 라우팅하고 오른쪽 반향를 마스터링 음성의 오른쪽 입력으로 라우팅합니다.

```cpp
if (enableEffect)
{
    DX::ThrowIfFailed(
        (*newSubmix)->EnableEffect(0)
        );    
}

DX::ThrowIfFailed(
    (*newSubmix)->SetVolume (1.0f)
    );

float outputMatrix[4] = {0, 0, 0, 0};
DX::ThrowIfFailed(
    (*newSubmix)->SetOutputMatrix(masteringVoice, 2, 2, outputMatrix)
    );
```

대리석 미로는 **오디오:: CreateReverb** 메서드를 4 번 호출 합니다. 즉, 배경 음악의 경우 두 번, 플레이 사운드의 경우 두 번입니다. 다음은 대리석이 배경 음악에 대해 **Createreverb** 메서드를 호출 하는 방법을 보여 줍니다.

```cpp
CreateReverb(
    m_musicEngine, 
    m_musicMasteringVoice, 
    &m_reverbParametersSmall, 
    &m_musicReverbVoiceSmallRoom, 
    true
    );
CreateReverb(
    m_musicEngine, 
    m_musicMasteringVoice, 
    &m_reverbParametersLarge, 
    &m_musicReverbVoiceLargeRoom, 
    true
    );
```

XAudio2와 함께 사용할 수 있는 결과 소스 목록은 [XAudio2 Audio 효과](/windows/desktop/xaudio2/xaudio2-audio-effects)를 참조 하세요.

### <a name="loading-audio-data-from-file"></a>파일에서 오디오 데이터를 로드 하는 중

대리석 미로는 미디어 파운데이션을 사용 하 여 파일에서 오디오 리소스를 로드 하는 **MediaStreamer** 클래스를 정의 합니다. 대리석 미로는 하나의 **MediaStreamer** 개체를 사용 하 여 각 오디오 파일을 로드 합니다.

대리석- **MediaStreamer:: Initialize** 메서드를 호출 하 여 각 오디오 스트림을 초기화 합니다. 다음은 **audio:: createresources.vb** 메서드가 **MediaStreamer:: Initialize** 를 호출 하 여 배경 음악에 대 한 오디오 스트림을 초기화 하는 방법입니다.

```cpp
// Media Foundation is a convenient way to get both file I/O and format decode for 
// audio assets. You can replace the streamer in this sample with your own file I/O 
// and decode routines.
m_musicStreamer.Initialize(L"Media\\Audio\\background.wma");
```

**MediaStreamer:: initialize** 메서드는 [MFStartup](/windows/desktop/api/mfapi/nf-mfapi-mfstartup) 메서드를 호출 하 여 미디어 파운데이션를 초기화 하는 방식으로 시작 됩니다. **MF_VERSION** 는 **mfapi**에 정의 된 매크로 이며 사용할 미디어 파운데이션 버전으로 지정 합니다.

```cpp
DX::ThrowIfFailed(
    MFStartup(MF_VERSION)
    );
```

**MediaStreamer:: Initialize** 는 [MFCreateSourceReaderFromURL](/windows/desktop/api/mfreadwrite/nf-mfreadwrite-mfcreatesourcereaderfromurl) 를 호출 하 여 [IMFSourceReader](/windows/desktop/api/mfreadwrite/nn-mfreadwrite-imfsourcereader) 개체를 만듭니다. **IMFSourceReader** 개체 **m_reader**는 **url**로 지정 된 파일에서 미디어 데이터를 읽습니다.

```cpp
DX::ThrowIfFailed(
    MFCreateSourceReaderFromURL(url, nullptr, &m_reader)
    );
```

그런 다음 **MediaStreamer:: Initialize** 메서드는 오디오 스트림의 형식을 설명 하기 위해 [MFCreateMediaType](/windows/desktop/api/mfapi/nf-mfapi-mfcreatemediatype) 를 사용 하 여 [imfmediatype](/windows/desktop/api/mfobjects/nn-mfobjects-imfmediatype) 개체를 만듭니다. 오디오 형식에는 주 형식 및 하위 형식 이라는 두 가지 형식이 있습니다. 주 유형은 미디어의 전체 형식 (예: 비디오, 오디오, 스크립트 등)을 정의 합니다. 하위 유형은 PCM, ADPCM 또는 WMA와 같은 형식을 정의 합니다.

**MediaStreamer:: Initialize** 메서드는 [Imfattributes:: setguid](/windows/desktop/api/mfobjects/nf-mfobjects-imfattributes-setguid) 메서드를 사용 하 여 주 유형 ([MF_MT_MAJOR_TYPE](/windows/desktop/medfound/mf-mt-major-type-attribute))을 오디오 (**MFMediaType \_ audio**)로, 부 유형 ([MF_MT_SUBTYPE](/windows/desktop/medfound/mf-mt-subtype-attribute))을 압축 되지 않은 pcm 오디오 (**MFAudioFormat \_ pcm**)로 지정 합니다. **MF_MT_MAJOR_TYPE** 및 **MF_MT_SUBTYPE** 는 [특성 미디어 파운데이션](/windows/desktop/medfound/media-foundation-attributes)됩니다. **MFMediaType_Audio** 및 **MFAudioFormat_PCM** 는 형식 및 하위 형식 guid입니다. 자세한 내용은 [오디오 미디어 유형](/windows/desktop/medfound/audio-media-types) 을 참조 하세요. [IMFSourceReader:: SetCurrentMediaType](/windows/desktop/api/mfreadwrite/nf-mfreadwrite-imfsourcereader-setcurrentmediatype) 메서드는 미디어 형식을 스트림 판독기와 연결 합니다.

```cpp
// Set the decoded output format as PCM. 
// XAudio2 on Windows can process PCM and ADPCM-encoded buffers. 
// When this sample uses Media Foundation, it always decodes into PCM.

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
    m_reader->SetCurrentMediaType(MF_SOURCE_READER_FIRST_AUDIO_STREAM, 0, mediaType.Get())
    );
```

그런 다음 **MediaStreamer:: Initialize** 메서드는 [IMFSourceReader:: getcurrentmediatype](/windows/desktop/api/mfreadwrite/nf-mfreadwrite-imfsourcereader-getcurrentmediatype) 을 사용 하 여 미디어 파운데이션에서 전체 출력 미디어 형식을 가져오고 [MFCreateWaveFormatExFromMFMediaType](/windows/desktop/api/mfapi/nf-mfapi-mfcreatewaveformatexfrommfmediatype) 메서드를 호출 하 여 미디어 파운데이션 오디오 미디어 형식을 [WAVEFORMATEX](/windows/desktop/api/mmreg/ns-mmreg-twaveformatex) 구조체로 변환 합니다. **WAVEFORMATEX** 구조체는 파형-오디오 데이터의 형식을 정의 합니다. 대리석 메 이즈는이 구조체를 사용 하 여 소스 음성을 만들고 하위 패스의 필터를 대리석의 롤링 사운드에 적용 합니다.

```cpp
// Get the complete WAVEFORMAT from the Media Type.
DX::ThrowIfFailed(
    m_reader->GetCurrentMediaType(MF_SOURCE_READER_FIRST_AUDIO_STREAM, &outputMediaType)
    );

uint32 formatSize = 0;
WAVEFORMATEX* waveFormat;
DX::ThrowIfFailed(
    MFCreateWaveFormatExFromMFMediaType(outputMediaType.Get(), &waveFormat, &formatSize)
    );
CopyMemory(&m_waveFormat, waveFormat, sizeof(m_waveFormat));
CoTaskMemFree(waveFormat);
```

> [!IMPORTANT]
> [MFCreateWaveFormatExFromMFMediaType](/windows/desktop/api/mfapi/nf-mfapi-mfcreatewaveformatexfrommfmediatype) 메서드는 **CoTaskMemAlloc** 를 사용 하 여 [WAVEFORMATEX](/windows/desktop/api/mmreg/ns-mmreg-twaveformatex) 개체를 할당 합니다. 따라서이 개체 사용을 마쳤을 때 **CoTaskMemFree** 를 호출 해야 합니다.

 

**MediaStreamer:: Initialize** 메서드는 스트림의 길이 **m \_ maxStreamLengthInBytes**(바이트)을 계산 하 여 완료 합니다. 이렇게 하기 위해 [IMFSourceReader:: GetPresentationAttribute](/windows/desktop/api/mfreadwrite/nf-mfreadwrite-imfsourcereader-getpresentationattribute) 메서드를 호출 하 여 오디오 스트림의 지속 시간을 100 나노초 단위로 가져오고, 기간을 섹션으로 변환한 다음, 초당 평균 데이터 전송 율 (바이트)을 곱합니다. 대리석은 나중에이 값을 사용 하 여 각 게임 플레이 소리를 포함 하는 버퍼를 할당 합니다.

```cpp
// Get the total length of the stream, in bytes.
PROPVARIANT var;
DX::ThrowIfFailed(
    m_reader->
        GetPresentationAttribute(MF_SOURCE_READER_MEDIASOURCE, MF_PD_DURATION, &var)
    );

// duration is in 100ns units; convert to seconds, and round up
// to the nearest whole byte.
ULONGLONG duration = var.uhVal.QuadPart;
m_maxStreamLengthInBytes =
    static_cast<unsigned int>(
        ((duration * static_cast<ULONGLONG>(m_waveFormat.nAvgBytesPerSec)) + 10000000)
        / 10000000
        );
```

### <a name="creating-the-source-voices"></a>원본 음성 만들기

대리석은 XAudio2 원본 음성을 만들어 원본 음성에서 각 게임 소리와 음악을 재생 합니다. **Audio** 클래스는 배경 음악에 대해 [IXAudio2SourceVoice](/windows/desktop/api/xaudio2/nn-xaudio2-ixaudio2sourcevoice) 개체를 정의 하 고 게임 소리를 보관할 **SoundEffectData** 개체의 배열을 정의 합니다. **SoundEffectData** 구조체는 효과에 대 한 **IXAudio2SourceVoice** 개체를 보유 하 고 오디오 버퍼와 같은 다른 효과 관련 데이터도 정의 합니다. **SoundEvent** 열거를 정의 합니다 **.** 대리석 메 이즈는이 열거형을 사용 하 여 각 게임 플레이 소리를 식별 합니다. 또한 **Audio** 클래스는이 열거형을 사용 하 여 **SoundEffectData** 개체의 배열을 인덱싱합니다.

```cpp
enum SoundEvent
{
    RollingEvent        = 0,
    FallingEvent        = 1,
    CollisionEvent      = 2,
    CheckpointEvent     = 3,
    MenuChangeEvent     = 4,
    MenuSelectedEvent   = 5,
    LastSoundEvent,
};
```

다음 표에서는 이러한 각 값의 관계, 연결 된 소리 데이터가 포함 된 파일 및 각 소리의 의미에 대 한 간략 한 설명을 보여 줍니다. 오디오 파일은 ** \\ 미디어 \\ 오디오** 폴더에 있습니다.

| SoundEvent 값  | 파일 이름      | Description                                              |
|-------------------|----------------|----------------------------------------------------------|
| RollingEvent      | MarbleRoll | 구슬이 구를 때 재생됩니다.                              |
| FallingEvent      | MarbleFall | 대리석이 미로를 벗어날 때 재생 됩니다.               |
| CollisionEvent    | MarbleHit  | 대리석이 미로와 충돌 하는 경우 재생 됩니다.           |
| CheckpointEvent   | 검사점 .wav | 대리석이 검사점을 통과할 때 재생 됩니다.         |
| MenuChangeEvent   | MenuChange .wav | 사용자가 현재 메뉴 항목을 변경할 때 재생 됩니다. |
| MenuSelectedEvent | MenuSelect | 사용자가 메뉴 항목을 선택할 때 재생 됩니다.           |

 

다음 예제에서는 **Audio:: createresources.vb** 메서드가 배경 음악에 대 한 소스 음성을 만드는 방법을 보여 줍니다. [XAUDIO2 \_ 송신 \_ 설명자](/windows/desktop/api/xaudio2/ns-xaudio2-xaudio2_send_descriptor) 구조는 다른 음성에서 대상 대상 음성을 정의 하 고 필터를 사용할지 여부를 지정 합니다. 대리석 미로는 **오디오:: SetSoundEffectFilter** 메서드를 호출 하 여 필터를 사용 하 여 구슬이 롤업되는 경우의 소리를 변경 합니다. [XAUDIO2 \_ 음성 \_ 송신](/windows/desktop/api/xaudio2/ns-xaudio2-xaudio2_voice_sends) 구조는 단일 출력 음성에서 데이터를 수신 하는 음성 집합을 정의 합니다. 대리석 미로는 원본 음성에서 마스터링 음성으로 데이터를 전송 하 고 (마른 경우, 재생 중인 소리의 경우), 플레이 사운드의 고가 또는 reverberant를 구현 하는 두 개의 서브 믹스 음성으로 데이터를 전송 합니다.

[IXAudio2:: CreateSourceVoice](/windows/desktop/api/xaudio2/nf-xaudio2-ixaudio2-createsourcevoice) 메서드는 소스 음성을 만들고 구성 합니다. 음성으로 전송 되는 오디오 버퍼의 형식을 정의 하는 [WAVEFORMATEX](/windows/desktop/api/mmreg/ns-mmreg-twaveformatex) 구조를 사용 합니다. 앞에서 설명한 것 처럼 대리석 메 이즈는 PCM 형식을 사용 합니다.

```cpp
XAUDIO2_SEND_DESCRIPTOR descriptors[3];
descriptors[0].pOutputVoice = m_musicMasteringVoice;
descriptors[0].Flags = 0;
descriptors[1].pOutputVoice = m_musicReverbVoiceSmallRoom;
descriptors[1].Flags = 0;
descriptors[2].pOutputVoice = m_musicReverbVoiceLargeRoom;
descriptors[2].Flags = 0;
XAUDIO2_VOICE_SENDS sends = {0};
sends.SendCount = 3;
sends.pSends = descriptors;
WAVEFORMATEX& waveFormat = m_musicStreamer.GetOutputWaveFormatEx();

DX::ThrowIfFailed(
    m_musicEngine->CreateSourceVoice(&m_musicSourceVoice, &waveFormat, 0, 1.0f, &m_voiceContext, &sends, nullptr)
    );

DX::ThrowIfFailed(
    m_musicMasteringVoice->SetVolume(0.4f)
    );
```

## <a name="playing-background-music"></a>배경 음악 재생


원본 음성이 중지 됨 상태로 만들어집니다. 대리석-게임 루프의 배경 음악을 시작 합니다. **MarbleMazeMain:: Update** 에 대 한 첫 번째 호출은 **Audio:: start** 를 호출 하 여 배경 음악을 시작 합니다.

```cpp
if (!m_audio.m_isAudioStarted)
{
    m_audio.Start();
}
```

**Audio:: start** 메서드는 [IXAudio2SourceVoice:: start](/windows/desktop/api/xaudio2/nf-xaudio2-ixaudio2sourcevoice-start) 메서드를 호출 하 여 배경 음악의 소스 음성 처리를 시작 합니다.

```cpp
void Audio::Start()
{     
    if (m_engineExperiencedCriticalError)
    {
        return;
    }

    HRESULT hr = m_musicSourceVoice->Start(0);

    if SUCCEEDED(hr) {
        m_isAudioStarted = true;
    }
    else
    {
        m_engineExperiencedCriticalError = true;
    }
}
```

원본 음성은 오디오 데이터를 오디오 그래프의 다음 단계로 전달 합니다. 대리석의 경우 다음 단계에는 오디오에 두 개의 반향 효과를 적용 하는 두 개의 서브 믹스 음성이 포함 됩니다. 한 서브 믹스 음성은 늦은 늦은 필드 반향를 적용 합니다. 두 번째는 far 늦은 필드 반향를 적용 합니다.

각 서브 믹스 음성이 최종 조합에 기여 하는 양은 방의 크기와 모양에 따라 결정 됩니다. 근거리 반향는 공이 벽 근처에 있거나 작은 방에 있을 때 더 많은 기능을 제공 하 고, 고가는 넓은 공간에 있는 경우에는 늦은 필드 반향에 더 많은 도움이 됩니다. 이 기법은 대리석을 미로로 이동 하는 경우 보다 현실적인 에코 효과를 냅니다. 대리석이 이러한 효과를 구현 하는 방법에 대 한 자세한 내용은 대리석 미로 소스 코드의 **Audio:: SetRoomSize** 및 **물리학:: CalculateCurrentRoomSize** 를 참조 하세요.

> [!NOTE]
> 대부분의 방 크기가 비교적 동일한 게임에서 더 기본적인 반향 모델을 사용할 수 있습니다. 예를 들어 모든 방에 대해 하나의 반향 설정을 사용 하거나 각 방에 대해 미리 정의 된 반향 설정을 만들 수 있습니다.

**Audio:: createresources.vb** 메서드는 미디어 파운데이션을 사용 하 여 배경 음악을 로드 합니다. 그러나이 시점에서 원본 음성은 작업할 오디오 데이터를 포함 하지 않습니다. 또한 배경 음악이 반복 되므로 음악이 계속 재생 되도록 원본 음성이 정기적으로 데이터로 업데이트 되어야 합니다.

원본 음성을 데이터로 채우기를 유지 하기 위해 게임 루프는 모든 프레임에서 오디오 버퍼를 업데이트 합니다. **MarbleMazeMain:: render** 메서드는 **Audio:: render** 를 호출 하 여 배경 음악 오디오 버퍼를 처리 합니다. **Audio** 클래스는 세 개의 오디오 버퍼 인 m 오디오 버퍼의 배열을 정의 합니다. ** \_ ** 각 버퍼에는 64 KB (65536 바이트)의 데이터가 포함 됩니다. 루프는 미디어 파운데이션 개체에서 데이터를 읽고 소스 음성에 지연 된 버퍼가 3 개 있을 때까지 해당 데이터를 원본 음성에 씁니다.

> [!CAUTION]
> 대리석은 음악 데이터를 저장 하는 데 64 KB 버퍼를 사용 하지만 더 크거나 작은 버퍼를 사용 해야 할 수도 있습니다. 이 크기는 게임의 요구 사항에 따라 달라 집니다.

```cpp
// This sample processes audio buffers during the render cycle of the application.
// As long as the sample maintains a high-enough frame rate, this approach should
// not glitch audio. In game code, it is best for audio buffers to be processed
// on a separate thread that is not synced to the main render loop of the game.
void Audio::Render()
{
    if (m_engineExperiencedCriticalError)
    {
        m_engineExperiencedCriticalError = false;
        ReleaseResources();
        Initialize();
        CreateResources();
        Start();
        if (m_engineExperiencedCriticalError)
        {
            return;
        }
    }

    try
    {
        bool streamComplete;
        XAUDIO2_VOICE_STATE state;
        uint32 bufferLength;
        XAUDIO2_BUFFER buf = {0};

        // Use MediaStreamer to stream the buffers.
        m_musicSourceVoice->GetState(&state);
        while (state.BuffersQueued <= MAX_BUFFER_COUNT - 1)
        {
            streamComplete = m_musicStreamer.GetNextBuffer(
                m_audioBuffers[m_currentBuffer],
                STREAMING_BUFFER_SIZE,
                &bufferLength
                );

            if (bufferLength > 0)
            {
                buf.AudioBytes = bufferLength;
                buf.pAudioData = m_audioBuffers[m_currentBuffer];
                buf.Flags = (streamComplete) ? XAUDIO2_END_OF_STREAM : 0;
                buf.pContext = 0;
                DX::ThrowIfFailed(
                    m_musicSourceVoice->SubmitSourceBuffer(&buf)
                    );

                m_currentBuffer++;
                m_currentBuffer %= MAX_BUFFER_COUNT;
            }

            if (streamComplete)
            {
                // Loop the stream.
                m_musicStreamer.Restart();
                break;
            }

            m_musicSourceVoice->GetState(&state);
        }
    }
    catch (...)
    {
        m_engineExperiencedCriticalError = true;
    }
}
```

루프는 미디어 파운데이션 개체가 스트림의 끝에 도달 하는 경우에도 처리 합니다. 이 경우 [IMFSourceReader:: SetCurrentPosition](/windows/desktop/api/mfreadwrite/nf-mfreadwrite-imfsourcereader-setcurrentposition) 메서드를 호출 하 여 오디오 소스의 위치를 다시 설정 합니다.

```cpp
void MediaStreamer::Restart()
{
    if (m_reader == nullptr)
    {
        return;
    }

    PROPVARIANT var = {0};
    var.vt = VT_I8;

    DX::ThrowIfFailed(
        m_reader->SetCurrentPosition(GUID_NULL, var)
        );
}
```

단일 버퍼 (또는 메모리로 완전히 로드 되는 전체 사운드)에 대해 오디오 루프를 구현 하려면 소리를 초기화할 때 [XAUDIO2_BUFFER](/windows/desktop/api/xaudio2/ns-xaudio2-xaudio2_buffer):: LoopCount 필드를 **XAUDIO2 \_ LOOP \_ 무한** 으로 설정 하면 됩니다. 대리석 미로는이 기술을 사용 하 여 대리석의 롤링 소리를 재생 합니다.

```cpp
if (sound == RollingEvent)
{
    m_soundEffects[sound].m_audioBuffer.LoopCount = XAUDIO2_LOOP_INFINITE;
}
```

그러나 배경 음악의 경우, 대리석은 사용 되는 메모리 양을 더 효과적으로 제어할 수 있도록 버퍼를 직접 관리 합니다. 음악 파일이 크면 음악 데이터를 더 작은 버퍼로 스트리밍할 수 있습니다. 이렇게 하면 오디오 데이터를 처리 하 고 스트림 하는 게임의 빈도로 메모리 크기의 균형을 맞출 수 있습니다.

> [!TIP]
> 게임의 프레임 속도가 낮거나 다른 경우 오디오 엔진에 사용할 수 있는 버퍼링 된 오디오 데이터가 부족 하기 때문에 주 스레드에서 오디오를 처리 하면 오디오에서 예기치 않은 일시 중지 또는 pop가 발생할 수 있습니다. 이 문제에 대 한 영향을 주는 게임의 경우 렌더링을 수행 하지 않는 별도의 스레드에서 오디오를 처리 하는 것이 좋습니다. 이 방법은 게임에서 유휴 프로세서를 사용할 수 있기 때문에 여러 프로세서가 있는 컴퓨터에서 특히 유용 합니다.

## <a name="reacting-to-game-events"></a>게임 이벤트에 대응

**Audio** 클래스는 **PlaySoundEffect**, **IsSoundEffectStarted**, **StopSoundEffect**, **SetSoundEffectVolume**, **SetSoundEffectPitch**및 **SetSoundEffectFilter** 와 같은 메서드를 제공 하 여 게임에서 소리를 재생 하 고 중지 하는 시기를 제어 하 고 볼륨 및 피치와 같은 소리 속성을 제어 합니다. 예를 들어, 대리석이 미로를 벗어나면 **MarbleMazeMain:: Update** 는 **오디오::P laySoundEffect** 메서드를 호출 하 여 **FallingEvent** 소리를 재생 합니다.

```cpp
m_audio.PlaySoundEffect(FallingEvent);
```

**Audio::P laySoundEffect** 메서드는 [IXAudio2SourceVoice:: Start](/windows/desktop/api/xaudio2/nf-xaudio2-ixaudio2sourcevoice-start) 메서드를 호출 하 여 소리의 재생을 시작 합니다. **IXAudio2SourceVoice:: Start** 메서드가 이미 호출 된 경우에는 다시 시작 되지 않습니다. **오디오::P laySoundEffect** 는 특정 소리에 대 한 사용자 지정 논리를 수행 합니다.

```cpp
void Audio::PlaySoundEffect(SoundEvent sound)
{
    XAUDIO2_BUFFER buf = {0};
    XAUDIO2_VOICE_STATE state = {0};

    if (m_engineExperiencedCriticalError)
    {
        // If there's an error, then we'll recreate the engine on the next
        // render pass.
        return;
    }

    SoundEffectData* soundEffect = &m_soundEffects[sound];
    HRESULT hr = soundEffect->m_soundEffectSourceVoice->Start();

    if FAILED(hr)
    {
        m_engineExperiencedCriticalError = true;
        return;
    }

    // For one-off voices, submit a new buffer if there's none queued up,
    // and allow up to two collisions to be queued up. 
    if (sound != RollingEvent)
    {
        XAUDIO2_VOICE_STATE state = {0};

        soundEffect->m_soundEffectSourceVoice->
            GetState(&state, XAUDIO2_VOICE_NOSAMPLESPLAYED);

        if (state.BuffersQueued == 0)
        {
            soundEffect->m_soundEffectSourceVoice->
                SubmitSourceBuffer(&soundEffect->m_audioBuffer);
        }
        else if (state.BuffersQueued < 2 && sound == CollisionEvent)
        {
            soundEffect->m_soundEffectSourceVoice->
                SubmitSourceBuffer(&soundEffect->m_audioBuffer);
        }

        // For the menu clicks, we want to stop the voice and replay the click
        // right away.
        // Note that stopping and then flushing could cause a glitch due to the
        // waveform not being at a zero-crossing, but due to the nature of the 
        // sound (fast and 'clicky'), we don't mind.
        if (state.BuffersQueued > 0 && sound == MenuChangeEvent)
        {
            soundEffect->m_soundEffectSourceVoice->Stop();
            soundEffect->m_soundEffectSourceVoice->FlushSourceBuffers();

            soundEffect->m_soundEffectSourceVoice->
                SubmitSourceBuffer(&soundEffect->m_audioBuffer);

            soundEffect->m_soundEffectSourceVoice->Start();
        }
    }

    m_soundEffects[sound].m_soundEffectStarted = true;
}
```

롤링 이외의 소리의 경우 **오디오::P laySoundEffect** 메서드는 [IXAudio2SourceVoice:: getstate](/windows/desktop/api/xaudio2/nf-xaudio2-ixaudio2sourcevoice-getstate) 를 호출 하 여 원본 음성이 재생 하는 버퍼 수를 확인 합니다. 활성 상태인 버퍼가 없는 경우 [IXAudio2SourceVoice:: SubmitSourceBuffer](/windows/desktop/api/xaudio2/nf-xaudio2-ixaudio2sourcevoice-submitsourcebuffer) 를 호출 하 여 사운드의 오디오 데이터를 음성의 입력 큐에 추가 합니다. **Audio::P laySoundEffect** 메서드를 사용 하 여 충돌 소리를 차례로 두 번 재생할 수 있습니다. 예를 들어, 대리석이 미로의 모퉁이와 충돌 하는 경우이 오류가 발생 합니다.

이미 설명한 대로 Audio 클래스는 롤링 이벤트의 소리를 초기화할 때 **XAUDIO2 \_ LOOP \_ 무한** 플래그를 사용 합니다. 이 이벤트에 대 한 **Audio::P laySoundEffect** 가 호출 되 면 처음으로 루프가 반복 재생 됩니다. 롤링 소리의 재생 논리를 단순화 하기 위해 대리석 무늬 메 이즈는 소리를 중지 하는 대신 mutes 합니다. 대리석이 변경 되 면 대리석 무늬 메 이즈는 소리의 피치와 볼륨을 변경 하 여 더 현실적인 효과를 제공 합니다. 다음은 **MarbleMazeMain:: Update** 메서드가 속도 변경에 따라 대리석의 피치와 볼륨을 업데이트 하는 방법과, 대리석이 중지 될 때 볼륨을 0으로 설정 하 여 소리를 mutes 하는 방법을 보여 줍니다.

```cpp
// Play the roll sound only if the marble is actually rolling.
if (ci.isRollingOnFloor && volume > 0)
{
    if (!m_audio.IsSoundEffectStarted(RollingEvent))
    {
        m_audio.PlaySoundEffect(RollingEvent);
    }

    // Update the volume and pitch by the velocity.
    m_audio.SetSoundEffectVolume(RollingEvent, volume);
    m_audio.SetSoundEffectPitch(RollingEvent, pitch);

    // The rolling sound has at most 8000Hz sounds, so we linearly
    // ramp up the low-pass filter the faster we go.
    // We also reduce the Q-value of the filter, starting with a
    // relatively broad cutoff and get progressively tighter.
    m_audio.SetSoundEffectFilter(
        RollingEvent,
        600.0f + 8000.0f * volume,
        XAUDIO2_MAX_FILTER_ONEOVERQ - volume*volume
        );
}
else
{
    m_audio.SetSoundEffectVolume(RollingEvent, 0);
}
```

## <a name="reacting-to-suspend-and-resume-events"></a>일시 중단 및 다시 시작 이벤트에 대응

[대리석 미로 응용 프로그램 구조](marble-maze-application-structure.md) 대리석에서 일시 중단 및 재개를 지 원하는 방법을 설명 합니다. 게임이 일시 중단 되 면 게임에서 오디오를 일시 중지 합니다. 게임이 다시 시작 되 면 게임이 중단 된 위치에서 오디오를 다시 시작 합니다. 이 작업을 위해 필요하지 않을 경우 리소스를 사용하지 않는 모범 사례를 따릅니다.

**Audio:: SuspendAudio** 메서드는 게임이 일시 중단 될 때 호출 됩니다. 이 메서드는 [IXAudio2:: StopEngine](/windows/desktop/api/xaudio2/nf-xaudio2-ixaudio2-stopengine) 메서드를 호출 하 여 모든 오디오를 중지 합니다. **IXAudio2:: StopEngine** 은 모든 오디오 출력을 즉시 중지 하지만 오디오 그래프와 해당 효과 매개 변수를 유지 합니다 (예: 대리석을 바운스 하는 경우 적용 되는 반향 효과).

```cpp
// Uses the IXAudio2::StopEngine method to stop all audio immediately.  
// It leaves the audio graph untouched, which preserves all effect parameters   
// and effect histories (like reverb effects) voice states, pending buffers,  
// cursor positions and so on. 
// When the engines are restarted, the resulting audio will sound as if it had  
// never been stopped except for the period of silence. 
void Audio::SuspendAudio()
{
    if (m_engineExperiencedCriticalError)
    {
        return;
    }

    if (m_isAudioStarted)
    {
        m_musicEngine->StopEngine();
        m_soundEffectEngine->StopEngine();
    }

    m_isAudioStarted = false;
}
```

**Audio:: ResumeAudio** 메서드는 게임을 다시 시작할 때 호출 됩니다. 이 메서드는 [IXAudio2:: StartEngine](/windows/desktop/api/xaudio2/nf-xaudio2-ixaudio2-startengine) 메서드를 사용 하 여 오디오를 다시 시작 합니다. [IXAudio2:: StopEngine](/windows/desktop/api/xaudio2/nf-xaudio2-ixaudio2-stopengine) 에 대 한 호출이 오디오 그래프와 해당 효과 매개 변수를 유지 하기 때문에 오디오 출력은 중단 된 위치에서 다시 시작 됩니다.

```cpp
// Restarts the audio streams. A call to this method must match a previous call
// to SuspendAudio. This method causes audio to continue where it left off.
// If there is a problem with the restart, the m_engineExperiencedCriticalError
// flag is set. The next call to Render will recreate all the resources and
// reset the audio pipeline.
void Audio::ResumeAudio()
{
    if (m_engineExperiencedCriticalError)
    {
        return;
    }

    HRESULT hr = m_musicEngine->StartEngine();
    HRESULT hr2 = m_soundEffectEngine->StartEngine();

    if (FAILED(hr) || FAILED(hr2))
    {
        m_engineExperiencedCriticalError = true;
    }
}
```

## <a name="handling-headphones-and-device-changes"></a>헤드폰 및 장치 변경 내용 처리

대리석 미로는 엔진 콜백을 사용 하 여 오디오 장치가 변경 되는 경우와 같은 XAudio2 엔진 오류를 처리 합니다. 게임 사용자가 헤드폰을 연결 하거나 연결을 끊을 때 장치 변경이 발생할 수 있습니다. 장치 변경을 처리 하는 엔진 콜백을 구현 하는 것이 좋습니다. 그렇지 않으면 게임을 다시 시작할 때까지 사용자가 헤드폰을 연결 하거나 제거할 때 소리 재생이 중지 됩니다.

**AudioEngineCallbacks** 클래스 **를 정의 합니다.** 이 클래스는 [IXAudio2EngineCallback](/windows/desktop/api/xaudio2/nn-xaudio2-ixaudio2enginecallback) 인터페이스를 구현 합니다.

```cpp
class AudioEngineCallbacks: public IXAudio2EngineCallback
{
private:
    Audio* m_audio;

public :
    AudioEngineCallbacks(){};
    void Initialize(Audio* audio);

    // Called by XAudio2 just before an audio processing pass begins.
    void _stdcall OnProcessingPassStart(){};

    // Called just after an audio processing pass ends.
    void  _stdcall OnProcessingPassEnd(){};

    // Called when a critical system error causes XAudio2
    // to be closed and restarted. The error code is given in Error.
    void  _stdcall OnCriticalError(HRESULT Error);
};
```

[IXAudio2EngineCallback](/windows/desktop/api/xaudio2/nn-xaudio2-ixaudio2enginecallback) 인터페이스를 사용 하면 오디오 처리 이벤트가 발생 하는 경우와 엔진에 심각한 오류가 발생할 때 코드를 알릴 수 있습니다. 콜백을 위해 등록 하기 위해 대리석은 음악 엔진에 대 한 [IXAudio2](/windows/desktop/api/xaudio2/nn-xaudio2-ixaudio2) 개체를 만든 후 **Audio:: createresources.vb**의 [IXAudio2:: registerforcallbacks](/windows/desktop/api/xaudio2/nf-xaudio2-ixaudio2-registerforcallbacks) 메서드를 호출 합니다.

```cpp
m_musicEngineCallback.Initialize(this);
m_musicEngine->RegisterForCallbacks(&m_musicEngineCallback);
```

대리석-오디오 처리를 시작 하거나 종료 하는 경우에는 알림이 필요 하지 않습니다. 따라서 [IXAudio2EngineCallback:: OnProcessingPassStart](/windows/desktop/api/xaudio2/nf-xaudio2-ixaudio2enginecallback-onprocessingpassstart) 및 [IXAudio2EngineCallback:: OnProcessingPassEnd](/windows/desktop/api/xaudio2/nf-xaudio2-ixaudio2enginecallback-onprocessingpassend) 메서드를 구현 하 여 아무 작업도 수행 하지 않습니다. [IXAudio2EngineCallback:: OnCriticalError](/windows/desktop/api/xaudio2/nf-xaudio2-ixaudio2enginecallback-oncriticalerror) 메서드의 경우 대리석 미로는 **m \_ engineExperiencedCriticalError** 플래그를 설정 하는 **SetEngineExperiencedCriticalError** 메서드를 호출 합니다.

```cpp
// Audio.cpp

// Called when a critical system error causes XAudio2 
// to be closed and restarted. The error code is given in Error. 
void  _stdcall AudioEngineCallbacks::OnCriticalError(HRESULT Error)
{
    m_audio->SetEngineExperiencedCriticalError();
}
```

```cpp
// Audio.h (Audio class)

// This flag can be used to tell when the audio system 
// is experiencing critial errors.
// XAudio2 gives a critical error when the user unplugs
// the headphones and a new speaker configuration is generated.
void SetEngineExperiencedCriticalError()
{
    m_engineExperiencedCriticalError = true;
}
```

심각한 오류가 발생 하면 오디오 처리가 중지 되 고 XAudio2에 대 한 모든 추가 호출이 실패 합니다. 이러한 상황에서 복구 하려면 XAudio2 인스턴스를 해제 하 고 새 인스턴스를 만들어야 합니다. 게임 루프에서 각 프레임으로 호출 되는 **Audio:: Render** 메서드는 먼저 **m \_ engineExperiencedCriticalError** 플래그를 확인 합니다. 이 플래그를 설정 하면 플래그를 지우고, 현재 XAudio2 인스턴스를 해제 하 고, 리소스를 초기화 한 다음, 배경 음악을 시작 합니다.

```cpp
if (m_engineExperiencedCriticalError)
{
    m_engineExperiencedCriticalError = false;
    ReleaseResources();
    Initialize();
    CreateResources();
    Start();
    if (m_engineExperiencedCriticalError)
    {
        return;
    }
}
```

또한 대리석 미로는 **m \_ engineExperiencedCriticalError** 플래그를 사용 하 여 오디오 장치를 사용할 수 없는 경우 XAudio2에 대 한 호출을 방지 합니다. 예를 들어이 플래그가 설정 된 경우 **MarbleMazeMain:: Update** 메서드는 롤링 또는 충돌 이벤트에 대 한 오디오를 처리 하지 않습니다. 필요한 경우 앱은 모든 프레임에서 오디오 엔진을 복구 하려고 시도 합니다. 그러나 컴퓨터에 오디오 장치가 없거나 헤드폰이 분리 되어 있고 사용 가능한 다른 오디오 장치가 없는 경우에는 항상 **m \_ engineExperiencedCriticalError** 플래그가 설정 될 수 있습니다.

> [!CAUTION]
> 엔진 콜백의 본문에서 차단 작업을 수행 하지 않도록 하는 것이 규칙입니다. 이렇게 하면 성능 문제가 발생할 수 있습니다. Marble Maze는 **OnCriticalError** 콜백에 플래그를 설정하고 나중에 일반적인 오디오 처리 단계에서 오류를 처리합니다. XAudio2 콜백에 대 한 자세한 내용은 [XAudio2 콜백](/windows/desktop/xaudio2/xaudio2-callbacks)을 참조 하세요.

## <a name="conclusion"></a>결론

그러면 대리석으로 어 메 이즈 게임 샘플을 래핑합니다. 비교적 간단한 게임 이지만, UWP DirectX 게임으로 이동 하는 중요 한 부분을 많이 포함 하 고 있습니다 .이 예제는 사용자가 게임을 만들 때 따르는 좋은 예입니다.

지금까지 완료 했으므로 소스 코드를 tinkering 하 고 어떻게 되는지 확인 하세요. 또는 다른 UWP DirectX game 샘플을 [사용 하 여 간단한 uwp 게임 만들기](tutorial--create-your-first-uwp-directx-game.md)를 확인 하세요.

DirectX를 사용 하 여 더 진행할 준비가 되셨습니까? 그런 다음 [DirectX 프로그래밍](directx-programming.md)에서 가이드를 확인 하세요.

일반적으로 UWP의 게임 개발에 관심이 있는 경우 [게임 프로그래밍](index.md)에서 설명서를 참조 하세요.

## <a name="related-topics"></a>관련 항목

* [Marble Maze 샘플에 입력 및 대화형 작업 추가](adding-input-and-interactivity-to-the-marble-maze-sample.md)
* [C + + 및 c + +의 UWP 게임, 대리석](developing-marble-maze-a-windows-store-game-in-cpp-and-directx.md)