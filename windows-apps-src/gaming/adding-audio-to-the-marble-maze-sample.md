---
author: eliotcowley
title: Marble Maze 샘플에 오디오 추가
description: 이 문서에서는 오디오 작업을 할 때 고려할 몇 가지 주요 사항을 설명하고 Marble Maze가 이러한 사례를 적용하는 방법을 보여 줍니다.
ms.assetid: 77c23d0a-af6d-17b5-d69e-51d9885b0d44
ms.author: elcowle
ms.date: 10/18/2017
ms.topic: article
keywords: windows 10, uwp, 오디오, 게임, 샘플
ms.localizationpriority: medium
ms.openlocfilehash: 89612e3fbc4ef2ccb855f7709820f9445d0fd77c
ms.sourcegitcommit: e38b334edb82bf2b1474ba686990f4299b8f59c7
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/15/2018
ms.locfileid: "6847916"
---
# <a name="adding-audio-to-the-marble-maze-sample"></a>Marble Maze 샘플에 오디오 추가

이 문서에서는 오디오 작업을 할 때 고려할 몇 가지 주요 사항을 설명하고 Marble Maze가 이러한 사례를 적용하는 방법을 보여 줍니다. Marble Maze는 [Microsoft 미디어 파운데이션](https://msdn.microsoft.com/library/windows/desktop/ms694197)을 사용하여 파일에서 오디오 리소스를 로드하고 [XAudio2](https://msdn.microsoft.com/library/windows/desktop/hh405049)를 사용하여 오디오를 믹싱 및 재생하고 오디오에 효과를 적용합니다.

Marble Maze는 백그라운드에서 음악을 재생할 뿐 아니라 게임 플레이 소리를 사용하여 구슬이 벽에 부딪치는 경우와 같은 게임 이벤트를 나타냅니다. 구현의 중요한 부분 중 하나는 Marble Maze가 반향 또는 에코 효과를 사용하여 구슬이 튀어 나갈 때 나는 소리를 시뮬레이트하는 것입니다. 반향 효과 구현에서는 에코가 작은 방에서 더 빠르고 크게 들리고, 큰 방에서 더 조용하고 느리게 들립니다.

> [!NOTE]
> 이 문서에 해당하는 샘플 코드는 [DirectX Marble Maze 게임 샘플](http://go.microsoft.com/fwlink/?LinkId=624011)에 있습니다.

게임에서 오디오 작업을 하는 경우에 대해 논의하는 주요 사항은 다음과 같습니다.

- 미디어 파운데이션을 사용하여 오디오 자산을 디코드하고 XAudio2를 사용하여 오디오를 재생하는 것이 좋습니다. 그러나 UWP(유니버설 Windows 플랫폼) 앱에서 작동하는 기존 오디오 자산 로드 메커니즘이 있는 경우 해당 메커니즘을 사용해도 됩니다.

- 오디오 그래프에는 각 활성 소리의 원본 음성 1개, 서브믹스 음성 0개 이상, 마스터 음성 1개가 포함됩니다. 원본 음성은 서브믹스 음성 및/또는 마스터 음성에 반영될 수 있습니다. 서브믹스 음성은 다른 서브믹스 음성 또는 마스터 음성에 반영됩니다.

- 배경 음악 파일이 큰 경우 메모리가 더 적게 사용되도록 음악을 작은 버퍼로 스트리밍하는 것이 좋습니다.

- 이렇게 하려면 앱이 포커스를 잃거나, 표시되지 않거나, 일시 중단될 때 오디오 재생을 일시 중지합니다. 앱이 포커스를 다시 얻거나, 표시되거나, 다시 시작되면 재생을 다시 시작합니다.

- 각 소리의 역할에 맞게 오디오 범주를 설정합니다. 예를 들어 일반적으로 게임 배경 오디오에는 **AudioCategory\_GameMedia**를 사용하고 소리 효과에는 **AudioCategory\_GameEffects**를 사용합니다.

- 모든 오디오 리소스와 인터페이스를 해제하고 다시 만들어 헤드폰을 비롯한 장치 변경을 처리합니다.

- 디스크 공간 및 스트리밍 비용 최소화가 요구 사항인 경우 오디오 파일을 압축할지 여부를 고려합니다. 그렇지 않으면 오디오가 더 빨리 로드되도록 오디오를 압축되지 않은 상태로 둘 수 있습니다.

## <a name="introducing-xaudio2-and-microsoft-media-foundation"></a>XAudio2 및 Microsoft 미디어 파운데이션 소개

XAudio2는 특히 게임 오디오를 지원하는 Windows용 하위 수준 오디오 라이브러리입니다. 게임에 대해 DSP(디지털 신호 처리) 및 오디오 그래프 엔진을 제공합니다. XAudio2는 SIMD 부동 소수점 아키텍처, HD 오디오 등의 컴퓨팅 추세를 지원하여 기존의 DirectSound 및 XAudio를 확장합니다. 또한 최신 게임의 복잡한 소리 처리 요구를 지원합니다.

[XAudio2 주요 개념](https://msdn.microsoft.com/library/windows/desktop/ee415764) 문서에서는 XAudio2 사용을 위한 주요 개념을 설명합니다. 간단히 요약하면 개념은 다음과 같습니다.

- [IXAudio2](https://msdn.microsoft.com/library/windows/desktop/ee415908) 인터페이스는 XAudio2 엔진의 핵심입니다. Marble Maze는 이 인터페이스를 사용하여 음성을 만들고 출력 장치가 변경되거나 실패할 때 알림을 받습니다.

- **음성**은 오디오 데이터를 처리, 조정 및 재생합니다.

- **원본 음성**은 오디오 채널 모음(모노, 5.1 등)이며 하나의 오디오 데이터 스트림을 나타냅니다. XAudio2에서 원본 음성은 오디오 처리가 시작되는 위치입니다. 일반적으로 소리 데이터는 파일, 네트워크 등의 외부 원본에서 로드되고 원본 음성에 전송됩니다. Marble Maze는 [미디어 파운데이션](https://msdn.microsoft.com/library/windows/desktop/ms694197)을 사용하여 파일에서 소리 데이터를 로드합니다. 미디어 파운데이션은 이 문서의 뒷부분에 소개되어 있습니다.

- **서브믹스 음성**은 오디오 데이터를 처리합니다. 이 처리에는 오디오 스트림을 변경하거나 여러 스트림을 하나로 결합하는 작업이 포함될 수 있습니다. Marble Maze는 서브믹스를 사용하여 반향 효과를 만듭니다.

- **마스터 음성**은 원본 및 서브믹스 음성의 데이터를 결합하고 해당 데이터를 오디오 하드웨어에 보냅니다.

- **오디오 그래프**에는 각 활성 소리의 원본 음성 1개, 서브믹스 음성 0개 이상 및 마스터 음성 1개만 포함됩니다.

- **콜백**은 음성 또는 엔진 개체에서 일부 이벤트가 발생했다고 클라이언트 코드에 알립니다. 콜백을 사용하면 XAudio2가 버퍼를 완료할 때 메모리를 다시 사용하고, 오디오 장치가 변경될 때(예: 헤드폰을 연결하거나 분리할 때) 반응할 수 있습니다. 이 문서의 뒷부분에 있는 [헤드폰 및 장치 변경 처리](#handling-headphones-and-device-changes)에서는 Marble Maze가 이 메커니즘을 사용하여 장치 변경을 처리하는 방법을 설명합니다.

Marble Maze는 오디오 엔진 2개(즉, [IXAudio2](https://msdn.microsoft.com/library/windows/desktop/ee415908) 개체 2개)를 사용하여 오디오를 처리합니다. 한 엔진은 배경 음악을 처리하고, 다른 엔진은 게임 플레이 소리를 처리합니다.

또한 Marble Maze는 각 엔진에 대해 하나의 마스터 음성을 만들어야 합니다. 마스터 엔진은 오디오 스트림을 하나의 스트림으로 결합하고 해당 스트림을 오디오 하드웨어에 보냅니다. 원본 음성인 배경 음악 스트림은 마스터 음성 1개와 서브믹스 음성 2개에 데이터를 출력합니다. 서브믹스 음성은 반향 효과를 수행합니다.

미디어 파운데이션은 다양한 오디오 및 비디오 형식을 지원하는 멀티미디어 라이브러리입니다. XAudio2 및 미디어 파운데이션은 서로 보완적입니다. Marble Maze는 미디어 파운데이션을 사용하여 파일에서 오디오 자산을 로드하고 XAudio2를 사용하여 오디오를 재생합니다. 미디어 파운데이션을 사용하여 오디오 자산을 로드하지 않아도 됩니다. UWP(유니버설 Windows 플랫폼) 앱에서 작동하는 기존 오디오 자산 로드 메커니즘이 있는 경우 해당 메커니즘을 사용합니다. [오디오, 동영상 및 카메라](../audio-video-camera/index.md)에서는 UWP 앱의 오디오를 구현하는 여러 가지 방법을 설명합니다.

XAudio2에 대한 자세한 내용은 [프로그래밍 가이드](https://msdn.microsoft.com/library/windows/desktop/ee415737)를 참조하세요. 미디어 파운데이션에 대한 자세한 내용은 [Microsoft 미디어 파운데이션](https://msdn.microsoft.com/library/windows/desktop/ms694197)을 참조하세요.

## <a name="initializing-audio-resources"></a>오디오 리소스 초기화

Marble Maze는 배경 음악에 Windows Media 오디오(.wma) 파일을 사용하고 게임 플레이 소리에 WAV(.wav) 파일을 사용합니다. 두 형식은 미디어 파운데이션에서 지원됩니다. .wav 파일 형식은 XAudio2에서 기본적으로 지원되지만 게임이 파일 형식을 수동으로 구문 분석하여 적절한 XAudio2 데이터 구조를 작성해야 합니다. Marble Maze는 .wav 파일을 더욱 쉽게 사용하기 위해 미디어 파운데이션을 사용합니다. 미디어 파운데이션에서 지원되는 전체 미디어 형식 목록은 [미디어 파운데이션에서 지원되는 미디어 형식](https://msdn.microsoft.com/library/windows/desktop/dd757927)을 참조하세요. Marble Maze는 별도의 디자인 타임 및 런타임 오디오 형식을 사용하지 않고 XAudio2 ADPCM 압축 지원을 사용하지 않습니다. XAudio2의 ADPCM 압축에 대한 자세한 내용은 [ADPCM 개요](https://msdn.microsoft.com/library/windows/desktop/ee415711)를 참조하세요.

**MarbleMazeMain::LoadDeferredResources**에서 호출되는 **Audio::CreateResources** 메서드는 파일에서 오디오 스트림을 로드하고, XAudio2 엔진 개체를 초기화하고, 원본, 서브믹스 및 마스터 음성을 만듭니다.

### <a name="creating-the-xaudio2-engines"></a>XAudio2 엔진 만들기

Marble Maze는 하나의 [IXAudio2](https://msdn.microsoft.com/library/windows/desktop/ee415908) 개체를 만들어 사용되는 각 오디오 엔진을 나타냅니다. 오디오 엔진을 만들려면 [XAudio2Create](https://msdn.microsoft.com/library/windows/desktop/ee419212) 메서드를 호출합니다. 다음 예제에서는 Marble Maze가 배경 음악을 처리하는 오디오 엔진을 만드는 방법을 보여 줍니다.

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

Marble Maze는 유사한 단계를 수행하여 게임 플레이 소리를 재생하는 오디오 엔진을 만듭니다.

UWP 앱에서 [IXAudio2](https://msdn.microsoft.com/library/windows/desktop/ee415908) 인터페이스를 사용하는 방법은 데스크톱 앱과 두 가지 방식에서 서로 다릅니다. 첫째, [XAudio2Create](https://msdn.microsoft.com/library/windows/desktop/ee419212)를 호출하기 전에 [CoInitializeEx](https://msdn.microsoft.com/library/windows/desktop/ms695279)를 호출할 필요가 없습니다. 또한 **IXAudio2**는 더 이상 디바이스 열거를 지원하지 않습니다. 오디오 장치를 열거하는 방법에 대한 자세한 내용은 [디바이스 열거](https://msdn.microsoft.com/library/windows/apps/hh464977)를 참조하세요.

### <a name="creating-the-mastering-voices"></a>마스터 음성 만들기

다음 예제에서는 **Audio::CreateResources** 메서드가 [IXAudio2::CreateMasteringVoice](https://msdn.microsoft.com/library/windows/desktop/hh405048) 메서드를 사용하여 배경 음악에 대한 마스터 음성을 만드는 방법을 보여 줍니다. 이 예제에서 **m\_musicMasteringVoice**는 [IXAudio2MasteringVoice](https://msdn.microsoft.com/library/windows/desktop/ee415912) 개체입니다. Marble Maze는 출력 채널 2개를 지정하여 반향 효과에 대한 논리를 간소화합니다. 

입력 샘플 속도는 48000으로 지정되어 있습니다. 이 샘플 속도가 선택된 이유는 필요한 CPU 처리량과 오디오 품질 간의 균형을 나타내기 때문입니다. 샘플 속도가 크면 눈에 띄는 품질 이점 없이 필요한 CPU 처리가 증가합니다. 

마지막으로 Marble Maze는 사용자가 게임을 플레이할 때 다른 응용 프로그램에서 음악을 들을 수 있도록 **AudioCategory_GameMedia**를 오디오 스트림 범주로 지정하고 있습니다. 음악 앱이 재생 중이면 Windows는 **AudioCategory\_GameMedia** 옵션으로 생성된 음성을 모두 음소거합니다. 게임 플레이 소리는 **AudioCategory\_GameEffects** 옵션으로 생성되기 때문에 사용자가 들을 수 있습니다. 오디오 범주에 대한 자세한 내용은 [AUDIO\_STREAM\_CATEGORY](https://msdn.microsoft.com/library/windows/desktop/hh404178) 를 참조하세요.

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

**Audio::CreateResources** 메서드는 *StreamCategory* 매개 변수에 대해 기본값인 **AudioCategory\_GameEffects**를 지정한다는 점을 제외하고 유사한 단계를 수행하여 게임 플레이 소리에 대한 마스터 음성을 만듭니다.

### <a name="creating-the-reverb-effect"></a>반향 효과 만들기

각 음성에 대해 XAudio2를 사용하여 오디오를 처리하는 효과 시퀀스를 만들 수 있습니다. 이러한 시퀀스를 효과 체인이라고 합니다. 음성에 하나 이상의 효과를 적용하려는 경우 효과 체인을 사용합니다. 효과 체인은 파괴적일 수 있습니다. 즉, 체인의 각 효과가 오디오 버퍼를 덮어쓸 수 있습니다. XAudio2에서 출력 버퍼가 자동으로 초기화되도록 보장하지 않으므로 이 속성은 중요합니다. 효과 개체는 XAudio2에서 XAPO(플랫폼 간 오디오 처리 개체)로 표현됩니다. XAPO에 대한 자세한 내용은 [XAPO 개요](https://msdn.microsoft.com/library/windows/desktop/ee415735)를 참조하세요.

효과 체인을 만드는 경우 다음 단계를 따르세요.

1. 효과 개체를 만듭니다.

2. [XAUDIO2\_EFFECT\_DESCRIPTOR](https://msdn.microsoft.com/library/windows/desktop/ee419236) 구조체를 효과 데이터로 채웁니다.

3. [XAUDIO2\_EFFECT\_CHAIN](https://msdn.microsoft.com/library/windows/desktop/ee419235) 구조체를 데이터로 채웁니다.

4. 음성에 효과 체인을 적용합니다.

5. 효과 매개 변수 구조체를 채우고 효과에 적용합니다.

6. 해당하는 경우 효과를 사용하거나 사용하지 않도록 설정합니다.

**Audio** 클래스는 반향을 구현하는 효과 체인을 만드는 **CreateReverb** 메서드를 정의합니다. 이 메서드는 [XAudio2CreateReverb](https://msdn.microsoft.com/library/windows/desktop/ee419213) 메서드를 호출하여 반향 효과를 위한 서브믹스 음성 역할을 하는 **ComPtr&lt;IUnknown&gt;** 개체인 **soundEffectXAPO**를 만듭니다.

```cpp
Microsoft::WRL::ComPtr<IUnknown> soundEffectXAPO;

DX::ThrowIfFailed(
    XAudio2CreateReverb(&soundEffectXAPO)
    );
```

[XAUDIO2\_EFFECT\_DESCRIPTOR](https://msdn.microsoft.com/library/windows/desktop/ee419236) 구조체에는 효과 체인에 사용할 XAPO 정보(예: 출력 채널의 대상 번호)가 포함됩니다. **Audio::CreateReverb** 메서드는 **XAUDIO2\_EFFECT\_DESCRIPTOR** 개체인 **soundEffectdescriptor**를 만듭니다. 이 개체는 사용 안 함 상태로 설정되고, 출력 채널 2개를 사용하며, 반향 효과를 위해 **soundEffectXAPO**를 참조합니다. 효과가 게임 소리 수정을 시작하려면 게임에서 매개 변수를 설정해야 하므로 **soundEffectdescriptor**가 사용 안 함 상태로 시작됩니다. Marble Maze는 출력 채널 2개를 사용하여 반향 효과에 대한 논리를 간소화합니다.

```cpp
soundEffectdescriptor.InitialState = false;
soundEffectdescriptor.OutputChannels = 2;
soundEffectdescriptor.pEffect = soundEffectXAPO.Get();
```

효과 체인에 여러 효과가 있는 경우 각 효과에 XAUDIO2_EFFECT_DESCRIPTOR 개체가 필요합니다. [XAUDIO2\_EFFECT\_CHAIN](https://msdn.microsoft.com/library/windows/desktop/ee419235) 구조체는 효과에 참여하는 [XAUDIO2\_EFFECT\_DESCRIPTOR](https://msdn.microsoft.com/library/windows/desktop/ee419236) 개체의 배열을 저장합니다. 다음 예제에서는 **Audio::CreateReverb** 메서드가 반향을 구현할 효과 하나를 지정하는 방법을 보여 줍니다.

```cpp
XAUDIO2_EFFECT_CHAIN soundEffectChain;

// ...

soundEffectChain.EffectCount = 1;
soundEffectChain.pEffectDescriptors = &soundEffectdescriptor;
```

**Audio::CreateReverb** 메서드는 [IXAudio2::CreateSubmixVoice](https://msdn.microsoft.com/library/windows/desktop/ee418608) 메서드를 호출하여 효과의 서브믹스 음성을 만듭니다. 효과 체인을 음성에 연결하기 위해 *pEffectChain* 매개 변수에 대한 [XAUDIO2\_EFFECT\_CHAIN](https://msdn.microsoft.com/library/windows/desktop/ee419235) 개체인 **soundEffectChain**을 지정합니다. 또한 Marble Maze는 출력 채널 2개와 샘플 속도 48kHz를 지정합니다.

```cpp
DX::ThrowIfFailed(
    engine->CreateSubmixVoice(newSubmix, 2, 48000, 0, 0, nullptr, &soundEffectChain)
    );
```

> [!TIP]
> 기존 효과 체인을 기존 서브믹스 음성에 연결하거나 현재 효과 체인을 바꾸려는 경우 [IXAudio2Voice::SetEffectChain](https://msdn.microsoft.com/library/windows/desktop/ee418594) 메서드를 사용합니다.

**Audio::CreateReverb** 메서드는 [IXAudio2Voice::SetEffectParameters](https://msdn.microsoft.com/library/windows/desktop/ee418595)를 호출하여 효과와 관련된 추가 매개 변수를 설정합니다. 이 메서드는 효과와 관련된 매개 변수 구조체를 사용합니다. 모든 반향 효과가 동일한 매개 변수를 공유하기 때문에 반향에 대한 효과 매개 변수가 포함된 [XAUDIO2FX\_REVERB\_PARAMETERS](https://msdn.microsoft.com/library/windows/desktop/ee419224) 개체인 **m_reverbParametersSmall**이 **Audio::Initialize** 메서드에서 초기화됩니다. 다음 예제에서는 **Audio::Initialize** 메서드가 근거리 반향에 대한 반향 매개 변수를 초기화하는 방법을 보여 줍니다.

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

이 예제는 대부분의 반향 매개 변수에 기본값을 사용하지만 **DisableLateField**를 TRUE로 설정하여 근거리 반향을 지정하고, **EarlyDiffusion**을 4로 설정하여 가까운 평면을 시뮬레이트하고, **LateDiffusion**을 15로 설정하여 멀리 떨어진 확산 표면을 시뮬레이트합니다. 가까운 평면에서는 에코가 더 빠르고 크게 들리고, 멀리 떨어진 확산 표면에서는 에코가 더 조용하고 느리게 들립니다. 게임에서 원하는 효과를 얻기 위해 반향 값을 실험하거나, **ReverbConvertI3DL2ToNative** 메서드를 통해 산업 표준 I3DL2(Interactive 3D Audio Rendering Guidelines Level 2.0) 매개 변수를 사용할 수 있습니다.

다음 예제에서는 **Audio::CreateReverb**가 반향 매개 변수를 설정하는 방법을 보여 줍니다. **newSubmix**는 [IXAudio2SubmixVoice](https://msdn.microsoft.com/library/windows/desktop/microsoft.directx_sdk.ixaudio2submixvoice.ixaudio2submixvoice)** 개체입니다. **매개 변수**는 [XAUDIO2FX\_REVERB\_PARAMETERS](https://msdn.microsoft.com/library/windows/desktop/ee419224)* 개체입니다.

```cpp
DX::ThrowIfFailed(
    (*newSubmix)->SetEffectParameters(0, parameters, sizeof(m_reverbParametersSmall))
    );
```

**Audio::CreateReverb** 메서드는 **enableEffect** 플래그가 설정된 경우 [IXAudio2Voice::EnableEffect](https://msdn.microsoft.com/library/windows/desktop/microsoft.directx_sdk.ixaudio2voice.ixaudio2voice.enableeffect)를 사용하도록 설정하여 완료합니다. 또한 [IXAudio2Voice::SetVolume](https://msdn.microsoft.com/library/windows/desktop/microsoft.directx_sdk.ixaudio2voice.ixaudio2voice.setvolume)을 사용하여 볼륨을 설정하고 [IXAudio2Voice::SetOutputMatrix](https://msdn.microsoft.com/library/windows/desktop/microsoft.directx_sdk.ixaudio2voice.ixaudio2voice.setoutputmatrix)를 사용하여 출력 매트릭스를 설정합니다. 이 부분에서는 볼륨을 전체(1.0)로 설정한 다음 왼쪽 및 오른쪽 입력과 왼쪽 및 오른쪽 출력 스피커에 대해 볼륨 매트릭스를 무음으로 지정합니다. 나중에 다른 코드가 두 반향 간을 크로스 페이드하여 벽 근처에서 큰 방으로의 전환을 시뮬레이트하거나, 필요한 경우 두 반향을 모두 음소거하기 때문입니다. 나중에 반향 경로의 음소거를 해제하는 경우 게임은 {1.0f, 0.0f, 0.0f, 1.0f} 행렬을 설정하여 왼쪽 반향 출력을 마스터 음성의 왼쪽 입력으로 보내고 오른쪽 반향 출력을 마스터 음성의 오른쪽 입력으로 보냅니다.

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

Marble Maze는 배경 음악에 대해 2번, 게임 플레이 소리에 대해 2번 총 4번 **Audio::CreateReverb** 메서드를 호출합니다. 다음 예제는 Marble Maze가 배경 음악에 대해 **CreateReverb** 메서드를 호출하는 방법을 보여 줍니다.

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

XAudio2에 사용할 수 있는 효과 원본 목록은 [XAudio2 오디오 효과](https://msdn.microsoft.com/library/windows/desktop/ee415756)를 참조하세요.

### <a name="loading-audio-data-from-file"></a>파일에서 오디오 데이터 로드

Marble Maze는 미디어 파운데이션을 사용하여 파일에서 오디오 리소스를 로드하는 **MediaStreamer** 클래스를 정의합니다. Marble Maze는 하나의 **MediaStreamer** 개체를 사용하여 각 오디오 파일을 로드합니다.

Marble Maze는 **MediaStreamer::Initialize** 메서드를 호출하여 각 오디오 스트림을 초기화합니다. **Audio::CreateResources** 메서드가 **MediaStreamer::Initialize**를 호출하여 배경 음악의 오디오 스트림을 초기화하는 방법은 다음과 같습니다.

```cpp
// Media Foundation is a convenient way to get both file I/O and format decode for 
// audio assets. You can replace the streamer in this sample with your own file I/O 
// and decode routines.
m_musicStreamer.Initialize(L"Media\\Audio\\background.wma");
```

**MediaStreamer::Initialize** 메서드는 먼저 [MFStartup](https://msdn.microsoft.com/library/windows/desktop/ms702238) 메서드를 호출하여 미디어 파운데이션을 초기화합니다. **MF_VERSION**은 **mfapi.h**에 정의된 매크로이며 사용할 미디어 파운데이션 버전으로 지정됩니다.

```cpp
DX::ThrowIfFailed(
    MFStartup(MF_VERSION)
    );
```

그런 다음 **MediaStreamer::Initialize**는 [MFCreateSourceReaderFromURL](https://msdn.microsoft.com/library/windows/desktop/dd388110)을 호출하여 [IMFSourceReader](https://msdn.microsoft.com/library/windows/desktop/dd374655) 개체를 만듭니다. **IMFSourceReader** 개체인 **m_reader**는 **url**에 지정된 파일에서 미디어 데이터를 읽습니다.

```cpp
DX::ThrowIfFailed(
    MFCreateSourceReaderFromURL(url, nullptr, &m_reader)
    );
```

그런 다음 **MediaStreamer::Initialize** 메서드는 [MFCreateMediaType](https://msdn.microsoft.com/library/windows/desktop/ms693861)을 사용하여 [IMFMediaType](https://msdn.microsoft.com/library/windows/desktop/ms704850) 개체를 만들어 오디오 스트림의 형식을 설명합니다. 오디오 형식에는 주 형식과 하위 형식의 두 유형이 있습니다. 주 형식은 동영상, 오디오, 스크립트 등 미디어의 전체 형식을 정의합니다. 하위 형식은 PCM, ADPCM, WMA 등의 형식을 정의합니다.

**MediaStreamer::Initialize** 메서드는 [IMFAttributes::SetGUID](https://msdn.microsoft.com/library/windows/desktop/bb970530) 메서드를 사용하여 주 형식([MF_MT_MAJOR_TYPE](https://msdn.microsoft.com/library/windows/desktop/ms702272))을 오디오(**MFMediaType\_Audio**)로 지정하고, 보조 형식([MF_MT_SUBTYPE](https://msdn.microsoft.com/library/windows/desktop/ms700208))을 압축되지 않은 PCM 오디오(**MFAudioFormat\_PCM**)로 지정합니다. **MF_MT_MAJOR_TYPE** 및 **MF_MT_SUBTYPE**은 [미디어 파운데이션 특성](https://msdn.microsoft.com/library/windows/desktop/ms696989)입니다. **MFMediaType_Audio** 및 **MFAudioFormat_PCM**은 형식 및 하위 형식 GUID입니다. 자세한 내용은 [오디오 미디어 형식](https://msdn.microsoft.com/library/windows/desktop/bb530108)을 참조하십시오. [IMFSourceReader::SetCurrentMediaType](https://msdn.microsoft.com/library/windows/desktop/dd374667) 메서드는 미디어 유형을 스트림 판독기에 연결합니다.

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

그런 다음 **MediaStreamer::Initialize** 메서드는 [IMFSourceReader::GetCurrentMediaType](https://msdn.microsoft.com/library/windows/desktop/dd374660)을 사용하여 미디어 파운데이션에서 전체 출력 미디어 형식을 가져오고 [MFCreateWaveFormatExFromMFMediaType](https://msdn.microsoft.com/library/windows/desktop/ms702177) 메서드를 호출하여 미디어 파운데이션 오디오 미디어 형식을 [WAVEFORMATEX](https://msdn.microsoft.com/library/windows/hardware/ff538799) 구조체로 변환합니다. **WAVEFORMATEX** 구조체는 파형 오디오 데이터의 형식을 정의합니다. Marble Maze는 이 구조체를 사용하여 원본 음성을 만들고 구슬이 구르는 소리에 저역 필터를 적용합니다.

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
> [MFCreateWaveFormatExFromMFMediaType](https://msdn.microsoft.com/library/windows/desktop/ms702177) 메서드는 **CoTaskMemAlloc**을 사용하여 [WAVEFORMATEX](https://msdn.microsoft.com/library/windows/hardware/ff538799) 개체를 할당합니다. 따라서 이 개체의 사용이 완료되면 **CoTaskMemFree**를 호출해야 합니다.

 

**MediaStreamer::Initialize** 메서드는 바이트 단위의 스트림 길이인 **m\_maxStreamLengthInBytes**를 계산하여 완료됩니다. 이 작업을 위해 [IMFSourceReader::GetPresentationAttribute](https://msdn.microsoft.com/library/windows/desktop/dd374662) 메서드를 호출하여 오디오 스트림 기간을 100 나노초 단위로 가져오고, 기간을 구역으로 변환한 다음 평균 데이터 전송 속도(초당 바이트 수)를 곱합니다. Marble Maze는 나중에 이 값을 사용하여 각 게임 플레이 소리가 저장된 버퍼를 할당합니다.

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

Marble Maze는 XAudio2 원본 음성을 만들어 원본 음성의 각 음악과 게임 소리를 재생합니다. **Audio** 클래스는 배경 음악에 사용할 [IXAudio2SourceVoice](https://msdn.microsoft.com/library/windows/desktop/ee415914) 개체와 게임 플레이 소리를 저장할 **SoundEffectData** 개체 배열을 정의합니다. **SoundEffectData** 구조체는 효과에 대한 **IXAudio2SourceVoice** 개체를 저장하고 오디오 버퍼 등의 다른 효과 관련 데이터도 정의합니다. **Audio.h**는 **SoundEvent** 열거형을 정의합니다. Marble Maze는 이 열거형을 사용하여 각 게임 플레이 소리를 식별합니다. 또한 **Audio** 클래스는 이 열거형을 사용하여 **SoundEffectData** 개체 배열을 인덱싱합니다.

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

다음 표에서는 이러한 값 간의 관계, 관련 소리 데이터가 포함된 파일 및 각 소리가 나타내는 항목에 대한 간략한 설명을 보여 줍니다. 오디오 파일은 **\\Media\\Audio** 폴더에 있습니다.

| SoundEvent 값  | 파일 이름      | 설명                                              |
|-------------------|----------------|----------------------------------------------------------|
| RollingEvent      | MarbleRoll.wav | 구슬이 구를 때 재생됩니다.                              |
| FallingEvent      | MarbleFall.wav | 구슬이 미로에서 떨어질 때 재생됩니다.               |
| CollisionEvent    | MarbleHit.wav  | 구슬이 미로와 충돌할 때 재생됩니다.           |
| CheckpointEvent   | Checkpoint.wav | 구슬이 검사점을 통과할 때 재생됩니다.         |
| MenuChangeEvent   | MenuChange.wav | 사용자가 현재 메뉴 항목을 변경할 때 재생됩니다. |
| MenuSelectedEvent | MenuSelect.wav | 사용자가 메뉴 항목을 선택할 때 재생됩니다.           |

 

다음 예제에서는 **Audio::CreateResources** 메서드가 배경 음악에 대한 원본 음성을 만드는 방법을 보여 줍니다. [XAUDIO2\_SEND\_DESCRIPTOR](https://msdn.microsoft.com/library/windows/desktop/ee419244) 구조체는 다른 음성에서 대상 음성을 정의하고 필터를 사용할지 여부를 지정합니다. Marble Maze는 **Audio::SetSoundEffectFilter** 메서드를 호출하여 필터를 통해 구슬이 구를 때 나는 소리를 변경합니다. [XAUDIO2\_VOICE\_SENDS](https://msdn.microsoft.com/library/windows/desktop/ee419246) 구조체는 단일 출력 음성에서 데이터를 수신할 음성 집합을 정의합니다. Marble Maze는 원본 음성의 데이터를 마스터 음성(재생 소리의 변경되지 않는 부분)과 재생음의 반향 부분을 구현하는 서브믹스 음성 2개에 보냅니다.

[IXAudio2::CreateSourceVoice](https://msdn.microsoft.com/library/windows/desktop/ee418607) 메서드는 원본 음성을 만들고 구성합니다. 음성에 전송되는 오디오 버퍼의 형식을 정의하는 [WAVEFORMATEX](https://msdn.microsoft.com/library/windows/hardware/ff538799) 구조체를 사용합니다. 앞에서 언급했듯이 Marble Maze는 PCM 형식을 사용합니다.

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


원본 음성은 중지된 상태로 생성됩니다. Marble Maze는 게임 루프에서 배경 음악을 시작합니다. **MarbleMazeMain::Update**를 처음 호출하면 **Audio::Start**가 호출되어 배경 음악을 시작합니다.

```cpp
if (!m_audio.m_isAudioStarted)
{
    m_audio.Start();
}
```

**Audio::Start** 메서드는 [IXAudio2SourceVoice::Start](https://msdn.microsoft.com/library/windows/desktop/ee418471)를 호출하여 배경 음악에 대한 원본 음성 처리를 시작합니다.

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

원본 음성은 오디오 데이터를 오디오 그래프의 다음 단계로 전달합니다. Marble Maze의 경우 반향 효과 2개를 오디오에 적용하는 서브믹스 음성 2개가 다음 단계에 포함됩니다. 한 서브믹스 음성은 가까운 지연 필드 반향을 적용하고, 두 번째 서브믹스 음성은 먼 지연 필드 반향을 적용합니다.

각 서브믹스 음성이 최종 믹스에 기여하는 정도는 방의 크기와 모양에 따라 결정됩니다. 근거리 반향은 구슬이 벽 근처나 작은 방에 있을 때 더 많이 기여하고, 지연 필드 반향은 구슬이 큰 공간에 있을 때 더 많이 기여합니다. 이 기술은 구슬이 미로를 통과할 때 보다 사실적인 에코 효과를 생성합니다. Marble Maze가 이 효과를 구현하는 방법에 대한 자세한 내용은 Marble Maze 소스 코드의 **Audio::SetRoomSize** 및 **Physics::CalculateCurrentRoomSize**를 참조하세요.

> [!NOTE]
> 대부분의 방 크기가 비교적 동일한 게임에서는 더 기본적인 반향 모델을 사용할 수 있습니다. 예를 들어 하나의 반향 설정을 모든 방에 사용하거나 각 방에 대해 사전 정의된 반향 설정을 만들 수 있습니다.

**Audio::CreateResources** 메서드는 미디어 파운데이션을 사용하여 배경 음악을 로드합니다. 그러나 이 시점에는 원본 음성에 작업할 오디오 데이터가 없습니다. 또한 배경 음악은 반복되기 때문에 음악이 계속 재생되도록 정기적으로 원본 음성을 데이터로 업데이트해야 합니다.

원본 음성을 데이터가 채워진 상태로 유지하기 위해 게임 루프는 매 프레임마다 오디오 버퍼를 업데이트합니다.  **MarbleMazeMain::Render** 메서드는 **Audio::Render**를 호출하여 배경 음악 오디오 버퍼를 처리합니다. **Audio** 클래스는 3개의 오디오 버퍼로 구성된 배열인 **m\_audioBuffers**를 정의합니다. 각 버퍼는 64KB(65536바이트)의 데이터를 저장합니다. 루프는 미디어 파운데이션 개체에서 데이터를 읽고, 원본 음성에 대기 중인 버퍼 3개가 있을 때까지 해당 데이터를 원본 음성에 씁니다.

> [!CAUTION]
> Marble Maze는 64KB 버퍼를 사용하여 음악 데이터를 저장하지만 더 크거나 작은 버퍼를 사용해야 할 수도 있습니다. 이 크기는 게임의 요구 사항에 따라 달라집니다.

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

루프는 미디어 파운데이션 개체가 스트림 끝에 도달하는 시기도 처리합니다. 이 경우 [IMFSourceReader::SetCurrentPosition](https://msdn.microsoft.com/library/windows/desktop/dd374668) 메서드를 호출하여 오디오 원본 위치를 다시 설정합니다.

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

단일 버퍼(또는 완전히 메모리에 로드된 전체 소리)에 대해 오디오 루프를 구현하기 위해 소리를 초기화할 때 [XAUDIO2_BUFFER](https://msdn.microsoft.com/library/windows/desktop/microsoft.directx_sdk.xaudio2.xaudio2_buffer)::LoopCount 필드를 **XAUDIO2\_LOOP\_INFINITE**로 설정할 수 있습니다. Marble Maze는 이 기술을 사용하여 구슬이 구르는 소리를 재생합니다.

```cpp
if (sound == RollingEvent)
{
    m_soundEffects[sound].m_audioBuffer.LoopCount = XAUDIO2_LOOP_INFINITE;
}
```

그러나 배경 음악의 경우 Marble Maze는 사용되는 메모리 양을 더 효율적으로 제어할 수 있도록 버퍼를 직접 관리합니다. 음악 파일이 큰 경우 음악 데이터를 더 작은 버퍼로 스트리밍 할 수 있습니다. 이렇게 하면 오디오 데이터를 처리하고 스트리밍하는 게임 기능의 빈도와 메모리 크기의 균형을 이루는 데 도움이 될 수 있습니다.

> [!TIP]
> 게임의 프레임 속도가 부족하거나 변하는 경우, 주 스레드에서 오디오를 처리하면 오디오 엔진에 작업할 버퍼링된 오디오 데이터가 부족하기 때문에 오디오에서 예기치 않은 일시 중지나 팝업이 발생할 수 있습니다. 이 문제가 게임에서 중요한 경우 렌더링을 수행하지 않는 별도의 스레드에서 오디오를 처리하는 것이 좋습니다. 이 방법은 게임이 유휴 프로세서를 사용할 수 있기 때문에 여러 개의 프로세서가 있는 컴퓨터에서 특히 유용합니다.

## <a name="reacting-to-game-events"></a>게임 이벤트에 반응

**Audio** 클래스는 **PlaySoundEffect**, **IsSoundEffectStarted**, **StopSoundEffect**, **SetSoundEffectVolume**, **SetSoundEffectPitch** 및 **SetSoundEffectFilter** 등의 메서드를 제공하여 게임에서 소리가 재생 및 중지되는 시기를 제어하고 볼륨, 피치 등의 소리 속성을 제어할 수 있게 합니다. 예를 들어 구슬이 미로에서 떨어질 경우 **MarbleMazeMain::Update** 메서드는 **Audio::PlaySoundEffect** 메서드를 호출하여 **FallingEvent** 소리를 재생합니다.

```cpp
m_audio.PlaySoundEffect(FallingEvent);
```

**Audio::PlaySoundEffect** 메서드는 [IXAudio2SourceVoice::Start](https://msdn.microsoft.com/library/windows/desktop/ee418471) 메서드를 호출하여 소리 재생을 시작합니다. **IXAudio2SourceVoice::Start** 메서드가 이미 호출된 경우에는 다시 시작되지 않습니다. **Audio::PlaySoundEffect**는 특정 소리에 대해 사용자 지정 논리를 수행합니다.

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

구르는 소리가 아닌 소리의 경우 **Audio::PlaySoundEffect** 메서드는 [IXAudio2SourceVoice::GetState](https://msdn.microsoft.com/library/windows/desktop/hh405047)를 호출하여 원본 음성이 재생 중인 버퍼 수를 확인합니다. 활성화된 버퍼가 없는 경우 [IXAudio2SourceVoice::SubmitSourceBuffer](https://msdn.microsoft.com/library/windows/desktop/ee418473)를 호출하여 소리의 오디오 데이터를 음성의 입력 큐에 추가합니다. **Audio::PlaySoundEffect** 메서드를 사용하여 충돌 소리를 차례로 두 번 재생할 수도 있습니다. 예를 들어 구슬이 미로의 모서리에 충돌하는 경우입니다.

이미 설명했듯이, Audio 클래스는 구르기 이벤트의 소리를 **XAUDIO2\_LOOP\_INFINITE** 플래그를 사용합니다. 이 이벤트에 대해 **Audio::PlaySoundEffect**를 처음 호출하면 소리가 반복 재생을 시작합니다. 구르는 소리에 대한 재생 논리를 간소화하기 위해 Marble Maze는 소리를 중지하는 대신 음소거합니다. 구슬의 속도가 변경될 때 Marble Maze는 소리의 피치와 볼륨을 변경하여 더 사실적인 효과를 줍니다. 다음 예제는 **MarbleMazeMain::Update** 메서드가 구슬 속도가 변경될 때 구슬의 피치와 볼륨을 업데이트하는 방법 및 구슬이 중지될 때 볼륨을 0으로 설정하여 소리를 음소거하는 방법을 보여 줍니다.

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

## <a name="reacting-to-suspend-and-resume-events"></a>일시 중단 및 다시 시작 이벤트에 반응

[Marble Maze 응용 프로그램 구조](marble-maze-application-structure.md)에서는 Marble Maze에서 일시 중단 및 다시 시작을 지원하는 방법을 설명합니다. 게임을 일시 중단하면 오디오가 일시 중지됩니다. 게임을 다시 시작하면 오디오가 중지된 위치부터 다시 시작됩니다. 이 작업을 위해 필요하지 않을 경우 리소스를 사용하지 않는 모범 사례를 따릅니다.

게임을 일시 중단하면 **Audio::SuspendAudio** 메서드가 호출됩니다. 이 메서드는 [IXAudio2::StopEngine](https://msdn.microsoft.com/library/windows/desktop/ee418628) 메서드를 호출하여 모든 오디오를 중지합니다. **IXAudio2::StopEngine**은 모든 오디오 출력을 즉시 중지하지만 오디오 그래프와 효과 매개 변수(예: 구슬이 튀어나갈 때 적용되는 반향 효과)를 유지합니다.

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

게임을 다시 시작하면 **Audio::ResumeAudio** 메서드가 호출됩니다. 이 메서드는 [IXAudio2::StartEngine](https://msdn.microsoft.com/library/windows/desktop/ee418626) 메서드를 사용하여 오디오를 다시 시작합니다. [IXAudio2::StopEngine](https://msdn.microsoft.com/library/windows/desktop/ee418628)을 호출할 경우 오디오 그래프와 효과 매개 변수가 유지되기 때문에 오디오 출력이 중지된 위치부터 다시 시작됩니다.

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

## <a name="handling-headphones-and-device-changes"></a>헤드폰 및 장치 변경 처리

Marble Maze는 엔진 콜백을 사용하여 오디오 장치가 변경되는 경우 등의 XAudio2 엔진 오류를 처리합니다. 게임 사용자가 헤드폰을 연결하거나 분리할 때 장치가 변경될 가능성이 큽니다. 장치 변경을 처리하는 엔진 콜백을 구현하는 것이 좋습니다. 그렇지 않으면 사용자가 헤드폰을 연결하거나 분리할 경우 게임을 다시 시작할 때까지 게임에서 소리 재생이 중지됩니다.

**Audio.h**에서는 **AudioEngineCallbacks** 클래스를 정의합니다. 이 클래스는 [IXAudio2EngineCallback](https://msdn.microsoft.com/library/windows/desktop/ee415910) 인터페이스를 구현합니다.

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

[IXAudio2EngineCallback](https://msdn.microsoft.com/library/windows/desktop/ee415910) 인터페이스를 사용하면 오디오 처리 이벤트가 발생할 때 및 엔진에서 오류가 발생할 때 코드에서 알림을 받을 수 있습니다. 콜백을 등록하기 위해 Marble Maze는 음악 엔진에 대한 [IXAudio2](https://msdn.microsoft.com/library/windows/desktop/ee415908) 개체를 만든 후 **Audio::CreateResources**에서 [IXAudio2::RegisterForCallbacks](https://msdn.microsoft.com/library/windows/desktop/ee418620) 메서드를 호출합니다.

```cpp
m_musicEngineCallback.Initialize(this);
m_musicEngine->RegisterForCallbacks(&m_musicEngineCallback);
```

Marble Maze는 오디오 처리가 시작되거나 끝날 때 알림이 필요하지 않습니다. 따라서 아무 작업도 하지 않는 [IXAudio2EngineCallback::OnProcessingPassStart](https://msdn.microsoft.com/library/windows/desktop/ee418463) 및 [IXAudio2EngineCallback::OnProcessingPassEnd](https://msdn.microsoft.com/library/windows/desktop/ee418462) 메서드를 구현합니다. [IXAudio2EngineCallback::OnCriticalError](https://msdn.microsoft.com/library/windows/desktop/ee418461) 메서드의 경우 Marble Maze는 **m\_engineExperiencedCriticalError** 플래그를 설정하는 **SetEngineExperiencedCriticalError** 메서드를 호출합니다.

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

오류가 발생하면 오디오 처리가 중지되고 이후의 XAudio2 호출이 모두 실패합니다. 이 오류를 복구하려면 XAudio2 인스턴스를 해제하고 새로 만들어야 합니다. 매 프레임마다 게임 루프에서 호출되는 **Audio::Render** 메서드는 먼저 **m\_engineExperiencedCriticalError** 플래그를 확인합니다. 이 플래그가 설정된 경우 플래그를 지우고, 현재 XAudio2 인스턴스를 해제하고, 리소스를 초기화한 다음 배경 음악을 시작합니다.

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

또한 Marble Maze는 사용 가능한 오디오 디바이스가 없을 경우 **m\_engineExperiencedCriticalError** 플래그를 사용하여 XAudio2가 호출되지 않도록 합니다. 예를 들어 **MarbleMazeMain::Update** 메서드는 이 플래그가 설정된 경우 구르기 또는 충돌 이벤트에 대한 오디오를 처리하지 않습니다. 필요한 경우 앱은 매 프레임마다 오디오 엔진을 복구하려고 합니다. 그러나 컴퓨터에 오디오 디바이스가 없거나 헤드폰이 분리되었으며 사용 가능한 다른 오디오 디바이스가 없을 경우 **m\_engineExperiencedCriticalError** 플래그가 항상 설정될 수도 있습니다.

> [!CAUTION]
> 일반적으로 엔진 콜백 본문에서 차단 작업을 수행하지 마세요. 이렇게 하면 성능 문제가 발생할 수 있습니다. Marble Maze는 **OnCriticalError** 콜백에 플래그를 설정하고 나중에 일반적인 오디오 처리 단계에서 오류를 처리합니다. XAudio2 콜백에 대한 자세한 내용은 [XAudio2 콜백](https://msdn.microsoft.com/library/windows/desktop/ee415745)을 참조하세요.

## <a name="conclusion"></a>결론

이것으로 Marble Maze 게임 샘플을 마치겠습니다! 상대적으로 간단한 게임이지만 여기에는 UWP DirectX 게임에 들어가는 중요한 요소들이 다수 포함되어 있기 때문에 자체적으로 게임을 만들 때 참조하기에 좋은 예제입니다.

지금까지 이 예제를 통해 소스 코드를 서투르게나마 고쳐보고 어떤 일이 벌어지는지 확인해봤습니다. 아니면 또 다른 UWP DirectX 게임 샘플인 [DirectX로 간단한 UWP 게임 만들기](tutorial--create-your-first-uwp-directx-game.md)를 확인하셔도 좋습니다.

DirectX를 더 사용해 볼 준비가 되었나요? [DirectX programming](directx-programming.md)에서 가이드를 확인하세요.

일반적으로 UWP에서의 게임 개발에 관심이 있는 경우에는 [게임 프로그래밍](index.md)에서 설명서를 참조하세요.

## <a name="related-topics"></a>관련 항목

* [Marble Maze 샘플에 입력 및 대화형 작업 추가](adding-input-and-interactivity-to-the-marble-maze-sample.md)
* [C++ 및 DirectX로 UWP 게임 Marble Maze 개발](developing-marble-maze-a-windows-store-game-in-cpp-and-directx.md)