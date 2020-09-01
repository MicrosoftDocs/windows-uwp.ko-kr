---
Description: 이 문서에서는 Ibasic오디오 효과 인터페이스를 구현 하는 Windows 런타임 구성 요소를 만들어 오디오 스트림에 대 한 사용자 지정 효과를 만들 수 있도록 하는 방법을 설명 합니다.
title: 사용자 지정 오디오 효과
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.assetid: 360faf3f-7e73-4db4-8324-3391f801d827
ms.localizationpriority: medium
ms.openlocfilehash: 52d2c9e9288446029d65fd7741d797939953d23d
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89175707"
---
# <a name="custom-audio-effects"></a>사용자 지정 오디오 효과

이 문서에서는 [**Ibasic오디오 효과**](/uwp/api/Windows.Media.Effects.IBasicAudioEffect) 인터페이스를 구현 하는 Windows 런타임 구성 요소를 만들어 오디오 스트림에 대 한 사용자 지정 효과를 만드는 방법을 설명 합니다. 사용자 지정 효과는 장치 카메라에 대 한 액세스를 제공 하는 [MediaCapture](/uwp/api/Windows.Media.Capture.MediaCapture)을 비롯 한 다양 한 Windows 런타임 api와 함께 사용할 수 있습니다. [**Mediacomposition**](/uwp/api/Windows.Media.Editing.MediaComposition)을 사용 하 여 미디어 클립에서 복잡 한 컴퍼지션을 만들 수 있는 [**medigraph**](/uwp/api/Windows.Media.Audio.AudioGraph) 와 다양 한 오디오 입력, 출력 및 서브 믹스 노드의 그래프를 신속 하 게 조합할 수 있습니다.

## <a name="add-a-custom-effect-to-your-app"></a>앱에 사용자 지정 효과 추가


사용자 지정 오디오 효과는 [**Ibasic오디오 효과**](/uwp/api/Windows.Media.Effects.IBasicAudioEffect) 인터페이스를 구현 하는 클래스에서 정의 됩니다. 이 클래스는 앱의 프로젝트에 직접 포함할 수 없습니다. 대신 Windows 런타임 구성 요소를 사용 하 여 오디오 효과 클래스를 호스팅해야 합니다.

**오디오 효과에 대 한 Windows 런타임 구성 요소 추가**

1.  Microsoft Visual Studio에서 솔루션을 연 상태에서 **파일** 메뉴로 이동 하 여 **추가- &gt; 새 프로젝트**를 선택 합니다.
2.  **Windows 런타임 구성 요소 (유니버설 Windows)** 프로젝트 형식을 선택 합니다.
3.  이 예에서는 프로젝트 이름을 *AudioEffectComponent*로 합니다. 이 이름은 나중에 코드에서 참조 됩니다.
4.  **확인**을 클릭합니다.
5.  프로젝트 템플릿은 Class1.cs 이라는 클래스를 만듭니다. **솔루션 탐색기**에서 Class1.cs의 아이콘을 마우스 오른쪽 단추로 클릭 하 고 **이름 바꾸기**를 선택 합니다.
6.  파일 이름을 *ExampleAudioEffect.cs*로 바꿉니다. Visual Studio에는 새 이름에 대 한 모든 참조를 업데이트할 것인지 묻는 메시지가 표시 됩니다. **예**를 클릭합니다.
7.  **ExampleAudioEffect.cs** 를 열고 클래스 정의를 업데이트 하 여 [**Ibasic오디오 효과**](/uwp/api/Windows.Media.Effects.IBasicAudioEffect) 인터페이스를 구현 합니다.


[!code-cs[ImplementIBasicAudioEffect](./code/AudioGraph/AudioEffectComponent/ExampleAudioEffect.cs#SnippetImplementIBasicAudioEffect)]

이 문서의 예제에 사용 된 모든 형식에 액세스 하려면 다음 네임 스페이스를 효과 클래스 파일에 포함 해야 합니다.

[!code-cs[EffectUsing](./code/AudioGraph/AudioEffectComponent/ExampleAudioEffect.cs#SnippetEffectUsing)]

## <a name="implement-the-ibasicaudioeffect-interface"></a>Ibasic오디오 효과 인터페이스 구현

오디오 효과는 [**Ibasic오디오 효과**](/uwp/api/Windows.Media.Effects.IBasicAudioEffect) 인터페이스의 모든 메서드 및 속성을 구현 해야 합니다. 이 섹션에서는 기본 에코 효과를 만들기 위해이 인터페이스를 간단 하 게 구현 하는 과정을 안내 합니다.

### <a name="supportedencodingproperties-property"></a>SupportedEncodingProperties 속성

시스템은 [**SupportedEncodingProperties**](/uwp/api/windows.media.effects.ibasicaudioeffect.supportedencodingproperties) 속성을 확인 하 여 효과에서 지원 되는 인코딩 속성을 확인 합니다. 사용자가 지정한 속성을 사용 하 여 오디오를 인코딩할 수 없는 경우 시스템은 효과에 대해 [**Close**](/uwp/api/windows.media.effects.ibasicaudioeffect.close) 를 호출 하 고 오디오 파이프라인에서 효과를 제거 합니다. 이 예제에서는  [**AudioEncodingProperties**](/uwp/api/Windows.Media.MediaProperties.AudioEncodingProperties) 개체가 만들어지고 반환 된 목록에 추가 되어 44.1 khz와 48 khz, 32 비트 float, mono 인코딩을 지원 합니다.

[!code-cs[SupportedEncodingProperties](./code/AudioGraph/AudioEffectComponent/ExampleAudioEffect.cs#SnippetSupportedEncodingProperties)]

### <a name="setencodingproperties-method"></a>SetEncodingProperties 메서드

시스템은 효과가 작동 하는 오디오 스트림에 대 한 인코딩 속성을 알려 주는 효과에 대해 [**SetEncodingProperties**](/uwp/api/windows.media.effects.ibasicvideoeffect.setencodingproperties) 를 호출 합니다. Echo 효과를 구현 하기 위해이 예제에서는 버퍼를 사용 하 여 두 번째 오디오 데이터를 저장 합니다. 이 메서드는 오디오가 인코딩된 샘플 주기를 기반으로 하 여 한 번에 하나의 오디오에서 버퍼 크기를 샘플 수로 초기화할 수 있는 기회를 제공 합니다. 또한 지연 효과는 정수 카운터를 사용 하 여 지연 버퍼의 현재 위치를 추적 합니다. **SetEncodingProperties** 는 오디오가 오디오 파이프라인에 추가 될 때마다 호출 되므로이 값을 0으로 초기화 하는 것이 좋습니다. 이 메서드에 전달 된 **AudioEncodingProperties** 개체를 캡처하여 효과의 다른 곳에서 사용할 수도 있습니다.

[!code-cs[DeclareEchoBuffer](./code/AudioGraph/AudioEffectComponent/ExampleAudioEffect.cs#SnippetDeclareEchoBuffer)]
[!code-cs[SetEncodingProperties](./code/AudioGraph/AudioEffectComponent/ExampleAudioEffect.cs#SnippetSetEncodingProperties)]


### <a name="setproperties-method"></a>SetProperties 메서드

[**SetProperties**](/uwp/api/windows.media.imediaextension.setproperties) 메서드를 사용 하면 효과를 사용 하는 앱에서 효과 매개 변수를 조정할 수 있습니다. 속성은 속성 이름 및 값의 [**IPropertySet**](/uwp/api/Windows.Foundation.Collections.IPropertySet) 맵으로 전달 됩니다.

[!code-cs[SetProperties](./code/AudioGraph/AudioEffectComponent/ExampleAudioEffect.cs#SnippetSetProperties)]

이 간단한 예제에서는 **mix** 속성의 값에 따라 현재 오디오 샘플을 지연 버퍼의 값과 혼합 합니다. 속성이 선언 되 고 TryGetValue가 호출 앱에서 설정한 값을 가져오는 데 사용 됩니다. 값이 설정 되지 않은 경우 기본값. 5가 사용 됩니다. 이 속성은 읽기 전용입니다. **SetProperties**를 사용 하 여 속성 값을 설정 해야 합니다.

[!code-cs[MixProperty](./code/AudioGraph/AudioEffectComponent/ExampleAudioEffect.cs#SnippetMixProperty)]

### <a name="processframe-method"></a>ProcessFrame 메서드

[**Processframe**](/uwp/api/windows.media.effects.ibasicaudioeffect.processframe) 메서드는 효과가 스트림의 오디오 데이터를 수정 하는 위치입니다. 메서드는 프레임당 한 번 호출 되며 [**ProcessAudioFrameContext**](/uwp/api/Windows.Media.Effects.ProcessAudioFrameContext) 개체에 전달 됩니다. 이 개체에는 처리할 들어오는 프레임을 포함 하는 입력 오디오 [**프레임**](/uwp/api/Windows.Media.AudioFrame) 개체와 오디오 파이프라인의 나머지 부분에 전달 되는 오디오 데이터를 쓰는 출력 오디오 **프레임** 개체가 포함 되어 있습니다. 오디오 프레임은 오디오 데이터의 짧은 조각을 나타내는 오디오 샘플의 버퍼입니다.

오디오 **프레임** 의 데이터 버퍼에 액세스 하려면 COM interop 필요 하므로 효과 클래스 파일에 **t e m** 네임 스페이스를 포함 하 고 오디오 버퍼에 액세스 하기 위한 인터페이스를 가져오기 위해 다음 코드를 네임 스페이스 내에 추가 해야 합니다.

[!code-cs[ComImport](./code/AudioGraph/AudioEffectComponent/ExampleAudioEffect.cs#SnippetComImport)]

> [!NOTE]
> 이 기술은 관리 되지 않는 네이티브 이미지 버퍼에 액세스 하므로 안전 하지 않은 코드를 허용 하도록 프로젝트를 구성 해야 합니다.
> 1.  솔루션 탐색기에서 AudioEffectComponent 프로젝트를 마우스 오른쪽 단추로 클릭 하 고 **속성**을 선택 합니다.
> 2.  **빌드** 탭을 선택합니다.
> 3.  **안전 하지 않은 코드 허용** 확인란을 선택 합니다.

 

이제 **Processframe** 메서드 구현을 효과에 추가할 수 있습니다. 첫째,이 메서드는 입력 및 출력 오디오 프레임에서 [**AudioBuffer**](/uwp/api/Windows.Media.AudioBuffer) 개체를 가져옵니다. 출력 프레임은 쓰기용으로 열리고 입력은 읽기를 위해 열립니다. 그런 다음 [**CreateReference**](/uwp/api/windows.graphics.imaging.bitmapbuffer.createreference)를 호출 하 여 각 버퍼에 대해 [**IMemoryBufferReference**](/uwp/api/Windows.Foundation.IMemoryBufferReference) 를 가져옵니다. 그런 다음 **IMemoryBufferReference** 개체를 위에서 정의한 COM interop 인터페이스로 캐스팅 하 고 **IMemoryByteAccess**다음 **getbuffer**를 호출 하 여 실제 데이터 버퍼를 가져옵니다.

이제 데이터 버퍼를 얻 었으 면 입력 버퍼에서 읽고 출력 버퍼에 쓸 수 있습니다. Inputbuffer의 각 샘플에 대해 값을 가져오고 1 **믹스** 를 곱하여 효과의 마른 신호 값을 설정 합니다. 다음으로 샘플은 echo 버퍼의 현재 위치에서 검색 되 고 **조합** 으로 곱하여 효과의 고 지 값을 설정 합니다. 출력 샘플은 원음 및 고 값의 합계로 설정 됩니다. 마지막으로 각 입력 샘플은 echo 버퍼에 저장 되 고 현재 샘플 인덱스가 증가 합니다.

[!code-cs[ProcessFrame](./code/AudioGraph/AudioEffectComponent/ExampleAudioEffect.cs#SnippetProcessFrame)]



### <a name="close-method"></a>Close 메서드

시스템은 효과가 종료 될 때 클래스에서 [**close**](/uwp/api/windows.media.effects.ibasicaudioeffect.close) [**close**](/uwp/api/windows.media.effects.ibasicaudioeffect.close) 메서드를 호출 합니다. 사용자가 만든 리소스를 삭제 하려면이 메서드를 사용 해야 합니다. 메서드에 대 한 인수는 효과를 정상적으로 닫 았는 지, 오류가 발생 한 경우 또는 효과가 필요한 인코딩 형식을 지원 하지 않는지를 알 수 있도록 하는 [**MediaEffectClosedReason**](/uwp/api/Windows.Media.Effects.MediaEffectClosedReason) 입니다.

[!code-cs[Close](./code/AudioGraph/AudioEffectComponent/ExampleAudioEffect.cs#SnippetClose)]

### <a name="discardqueuedframes-method"></a>DiscardQueuedFrames 메서드

[**DiscardQueuedFrames**](/uwp/api/windows.media.effects.ibasicvideoeffect.discardqueuedframes) 메서드는 효과를 다시 설정 해야 할 때 호출 됩니다. 이에 대 한 일반적인 시나리오는 효과가 현재 프레임을 처리 하는 데 사용 하기 위해 이전에 처리 된 프레임을 저장 하는 경우입니다. 이 메서드를 호출 하면 저장 한 이전 프레임 집합을 삭제 해야 합니다. 이 메서드는 누적 오디오 프레임 뿐만 아니라 이전 프레임과 관련 된 모든 상태를 다시 설정 하는 데 사용할 수 있습니다.

[!code-cs[DiscardQueuedFrames](./code/AudioGraph/AudioEffectComponent/ExampleAudioEffect.cs#SnippetDiscardQueuedFrames)]

### <a name="timeindependent-property"></a>TimeIndependent 속성

TimeIndependent [**timeindependent**](/uwp/api/windows.media.effects.ibasicvideoeffect.timeindependent) 속성을 사용 하면 효과에 균일 한 시간이 필요 하지 않은 경우 시스템에서 알 수 있습니다. True로 설정 되 면 시스템은 성능 향상을 위해 최적화를 사용할 수 있습니다.

[!code-cs[TimeIndependent](./code/AudioGraph/AudioEffectComponent/ExampleAudioEffect.cs#SnippetTimeIndependent)]

### <a name="useinputframeforoutput-property"></a>Useinputunlink Foroutput 속성

[**Useinputframeforoutput**](/uwp/api/windows.media.effects.ibasicaudioeffect.useinputframeforoutput) 속성을 **true** 로 설정 하 여 결과를 [**Outputframe**](/uwp/api/windows.media.effects.processaudioframecontext.outputframe)에 쓰지 않고 [**Processframe**](/uwp/api/windows.media.effects.ibasicaudioeffect.processframe) 에 전달 된 [**ProcessAudioFrameContext**](/uwp/api/Windows.Media.Effects.ProcessAudioFrameContext) 의 [**inputframe**](/uwp/api/windows.media.effects.processaudioframecontext.inputframe) 오디오 버퍼에 쓰도록 합니다. 

[!code-cs[UseInputFrameForOutput](./code/AudioGraph/AudioEffectComponent/ExampleAudioEffect.cs#SnippetUseInputFrameForOutput)]


## <a name="adding-your-custom-effect-to-your-app"></a>앱에 사용자 지정 효과 추가


앱에서 오디오 효과를 사용 하려면 앱에 효과 프로젝트에 대 한 참조를 추가 해야 합니다.

1.  솔루션 탐색기의 앱 프로젝트에서 **참조** 를 마우스 오른쪽 단추로 클릭 하 고 **참조 추가**를 선택 합니다.
2.  **프로젝트** 탭을 확장 하 고 **솔루션**을 선택한 다음 효과 프로젝트 이름에 대 한 확인란을 선택 합니다. 이 예에서는 이름이 *AudioEffectComponent*입니다.
3.  **확인**을 클릭합니다.

오디오 효과 클래스를 다른 네임 스페이스로 선언 하는 경우에는 해당 네임 스페이스를 코드 파일에 포함 해야 합니다.

[!code-cs[UsingAudioEffectComponent](./code/AudioGraph/cs/MainPage.xaml.cs#SnippetUsingAudioEffectComponent)]

### <a name="add-your-custom-effect-to-an-audiograph-node"></a>오디오 그래프 노드에 사용자 지정 효과 추가
오디오 그래프를 사용 하는 방법에 대 한 일반적인 내용은 [audio garphs](audio-graphs.md)를 참조 하세요. 다음 코드 조각에서는이 문서에 표시 된 예제 echoe 효과를 오디오 그래프 노드에 추가 하는 방법을 보여 줍니다. 먼저 [**PropertySet**](/uwp/api/Windows.Foundation.Collections.PropertySet) 가 만들어지고 효과에 정의 된 **Mix** 속성의 값이 설정 됩니다. 그런 다음, [**AudioEffectDefinition**](/uwp/api/Windows.Media.Effects.AudioEffectDefinition) 생성자를 호출 하 여 사용자 지정 효과 형식 및 속성 집합의 전체 클래스 이름을 전달 합니다. 마지막으로 효과 정의가 기존 [**Fileinputnode**](/uwp/api/windows.media.audio.createaudiofileinputnoderesult.fileinputnode)의 [**EffectDefinitions**](/uwp/api/windows.media.audio.audiofileinputnode.effectdefinitions) 속성에 추가 되어 사용자 지정 효과에 의해 내보내지는 오디오를 처리 합니다. 

[!code-cs[AddCustomEffect](./code/AudioGraph/cs/MainPage.xaml.cs#SnippetAddCustomEffect)]

노드에 추가 된 후에는 [**DisableEffectsByDefinition**](/uwp/api/windows.media.audio.audiofileinputnode.disableeffectsbydefinition) 를 호출 하 고 **AudioEffectDefinition** 개체를 전달 하 여 사용자 지정 효과를 사용 하지 않도록 설정할 수 있습니다. 앱에서 오디오 그래프를 사용 하는 방법에 대 한 자세한 내용은 오디오 [그래프](audio-graphs.md)를 참조 하세요.

### <a name="add-your-custom-effect-to-a-clip-in-a-mediacomposition"></a>MediaComposition의 클립에 사용자 지정 효과 추가

다음 코드 조각에서는 비디오 클립에 사용자 지정 오디오 효과를 추가 하 고 미디어 컴포지션에 배경 오디오 트랙을 추가 하는 방법을 보여 줍니다. 비디오 클립에서 미디어 컴퍼지션을 만들고 배경 오디오 트랙을 추가 하는 방법에 대 한 일반적인 지침은 [미디어 컴포지션 및 편집](media-compositions-and-editing.md)을 참조 하세요.

[!code-cs[AddCustomAudioEffect](./code/MediaEditing/cs/MainPage.xaml.cs#SnippetAddCustomAudioEffect)]



## <a name="related-topics"></a>관련 항목
* [간단한 카메라 미리 보기 액세스](simple-camera-preview-access.md)
* [미디어 컴퍼지션 및 편집](media-compositions-and-editing.md)
* [Win2D 설명서](https://microsoft.github.io/Win2D/html/Introduction.htm)
* [미디어 재생](media-playback.md)

 