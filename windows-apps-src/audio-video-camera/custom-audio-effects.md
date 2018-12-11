---
Description: This article describes how to create a Windows Runtime component that implements the IBasicAudioEffect interface to allow you to create custom effects for audio streams.
title: 사용자 지정 오디오 효과
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.assetid: 360faf3f-7e73-4db4-8324-3391f801d827
ms.localizationpriority: medium
ms.openlocfilehash: 5278a845e56f1df76cc663bbb0c517b589a23524
ms.sourcegitcommit: 8921a9cc0dd3e5665345ae8eca7ab7aeb83ccc6f
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 12/10/2018
ms.locfileid: "8875045"
---
# <a name="custom-audio-effects"></a>사용자 지정 오디오 효과

이 문서에서는 오디오 스트림에 대한 사용자 지정 효과를 만들 수 있는 [**IBasicAudioEffect**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Effects.IBasicAudioEffect) 인터페이스를 구현하는 Windows 런타임 구성 요소를 만드는 방법을 설명합니다. 사용자 지정 효과는 디바이스의 카메라에 액세스할 수 있도록 하는 [MediaCapture](https://msdn.microsoft.com/library/windows/apps/br241124), 미디어 클립에서 복잡한 컴퍼지션을 만들 수 있도록 하는 [**MediaComposition**](https://msdn.microsoft.com/library/windows/apps/dn652646), 다양한 오디오 입출력 및 서브믹스 노드 그래프를 빠르게 조합할 수 있도록 하는 [**AudioGraph**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Audio.AudioGraph)를 비롯한 여러 다른 Windows 런타임 API에서 사용할 수 있습니다.

## <a name="add-a-custom-effect-to-your-app"></a>앱에 사용자 지정 효과 추가


사용자 지정 오디오 효과는 [**IBasicAudioEffect**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Effects.IBasicAudioEffect) 인터페이스를 구현하는 클래스에 정의됩니다. 이 클래스를 앱의 프로젝트에 직접 포함할 수는 없습니다. 대신 Windows 런타임 구성 요소를 사용하여 오디오 효과 클래스를 호스트해야 합니다.

**오디오 효과에 대한 Windows 런타임 구성 요소 추가**

1.  Microsoft Visual Studio에서 솔루션을 열고 **파일** 메뉴로 이동한 후 **추가-&gt;새 프로젝트**를 선택합니다.
2.  **Windows 런타임 구성 요소(유니버설 Windows)** 프로젝트 유형을 선택합니다.
3.  이 예제에서는 프로젝트 이름을 *AudioEffectComponent*로 지정합니다. 이 이름은 나중에 코드에서 참조됩니다.
4.  **확인**을 클릭합니다.
5.  프로젝트 템플릿은 Class1.cs라는 클래스를 만듭니다. **솔루션 탐색기**에서 Class1.cs 아이콘을 마우스 오른쪽 단추로 클릭하고 **이름 바꾸기**를 선택합니다.
6.  파일 이름을 *ExampleAudioEffect.cs*로 바꿉니다. Visual Studio에서 새 이름에 대한 모든 참조를 업데이트할지 묻는 메시지가 표시됩니다. **예**를 클릭합니다.
7.  **ExampleAudioEffect.cs**를 열고 클래스 정의를 업데이트하여 [**IBasicAudioEffect**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Effects.IBasicAudioEffect) 인터페이스를 구현합니다.


[!code-cs[ImplementIBasicAudioEffect](./code/AudioGraph/AudioEffectComponent/ExampleAudioEffect.cs#SnippetImplementIBasicAudioEffect)]

이 문서의 예제에서 사용되는 모든 유형에 액세스하기 위해 효과 클래스 파일에 다음 네임스페이스를 포함해야 합니다.

[!code-cs[EffectUsing](./code/AudioGraph/AudioEffectComponent/ExampleAudioEffect.cs#SnippetEffectUsing)]

## <a name="implement-the-ibasicaudioeffect-interface"></a>IBasicAudioEffect 인터페이스 구현

오디오 효과는 [**IBasicAudioEffect**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Effects.IBasicAudioEffect) 인터페이스의 모든 메서드 및 속성을 구현해야 합니다. 이 섹션에서는 기본 에코 효과를 만드는 이 인터페이스의 간단한 구현 과정을 안내합니다.

### <a name="supportedencodingproperties-property"></a>SupportedEncodingProperties 속성

시스템은 [**SupportedEncodingProperties**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Effects.IBasicAudioEffect.SupportedEncodingProperties) 속성을 검사하여 효과에서 지원되는 인코딩 속성을 확인합니다. 효과의 소비자가 사용자가 지정한 속성을 사용하여 오디오를 인코딩할 수 없는 경우 시스템은 효과에 대해 [**Close**](https://msdn.microsoft.com/library/windows/apps/dn764782)를 호출하고 오디오 파이프라인에서 해당 효과를 제거합니다. 이 예제에서는 [**AudioEncodingProperties**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.MediaProperties.AudioEncodingProperties) 개체를 만들고 반환된 목록에 추가하여 44.1kHz 및 48kHz, 32비트 부동 소수점, 모노 인코딩을 지원합니다.

[!code-cs[SupportedEncodingProperties](./code/AudioGraph/AudioEffectComponent/ExampleAudioEffect.cs#SnippetSupportedEncodingProperties)]

### <a name="setencodingproperties-method"></a>SetEncodingProperties 메서드

시스템에서 효과에 대해 [**SetEncodingProperties**](https://msdn.microsoft.com/library/windows/apps/dn919884)를 호출하므로 효과가 작동하는 오디오 스트림에 대한 인코딩 속성을 알 수 있습니다. 에코 효과를 구현하기 위해 이 예제에서는 1초 오디오 데이터를 저장하는 버퍼를 사용합니다. 이 방법을 통해 오디오를 인코딩하는 샘플 속도를 기반으로 버퍼 크기를 오디오 초당 샘플 수로 초기화할 수 있게 됩니다. 또한 지연 효과에서는 정수 카운터를 사용하여 지연 버퍼에서 현재 위치를 추적합니다. 오디오 파이프라인에 효과를 추가할 때마다 **SetEncodingProperties**가 호출되므로 해당 값을 0으로 초기화할 좋은 기회입니다. 다른 효과에서 사용할 수 있도록 이 메서드를 통과하는 **AudioEncodingProperties** 개체를 캡처할 수도 있습니다.

[!code-cs[DeclareEchoBuffer](./code/AudioGraph/AudioEffectComponent/ExampleAudioEffect.cs#SnippetDeclareEchoBuffer)]
[!code-cs[SetEncodingProperties](./code/AudioGraph/AudioEffectComponent/ExampleAudioEffect.cs#SnippetSetEncodingProperties)]


### <a name="setproperties-method"></a>SetProperties 메서드

[**SetProperties**](https://msdn.microsoft.com/library/windows/apps/br240986) 메서드를 사용하면 효과를 사용하는 앱에 따라 효과 매개 변수가 조정될 수 있습니다. 속성은 속성 이름 및 값의 [**IPropertySet**](https://msdn.microsoft.com/library/windows/apps/br226054) 맵으로 전달됩니다.

[!code-cs[SetProperties](./code/AudioGraph/AudioEffectComponent/ExampleAudioEffect.cs#SnippetSetProperties)]

이 간단한 예제에서는 **Mix** 속성 값에 따라 현재 오디오 샘플을 지연 버퍼의 값과 혼합합니다. 속성이 선언되고 TryGetValue를 사용하여 호출 앱에 의해 설정된 값을 가져옵니다. 값이 설정되지 않은 경우 기본값은 .5가 사용됩니다. 이 속성은 읽기 전용입니다. 속성 값은 **SetProperties**를 사용하여 설정해야 합니다.

[!code-cs[MixProperty](./code/AudioGraph/AudioEffectComponent/ExampleAudioEffect.cs#SnippetMixProperty)]

### <a name="processframe-method"></a>ProcessFrame 메서드

[**ProcessFrame**](https://msdn.microsoft.com/library/windows/apps/dn764784) 메서드는 효과가 스트림의 오디오 데이터를 수정하는 위치입니다. 이 메서드는 프레임당 한번씩 호출되고 [**ProcessAudioFrameContext**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Effects.ProcessAudioFrameContext) 개체가 전달됩니다. 이 개체에는 처리될 수신 프레임이 포함된 입력 [**AudioFrame**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.AudioFrame) 개체와 나머지 오디오 파이프라인에 전달되는 오디오 데이터를 쓰는 출력 **AudioFrame** 개체가 포함됩니다. 오디오 프레임은 오디오 데이터의 짧은 조각을 나타내는 오디오 샘플 버퍼입니다.

**AudioFrame**의 데이터 버퍼에 액세스하려면 COM interop이 필요하므로 **System.Runtime.InteropServices** 네임스페이스를 효과 클래스 파일에 포함한 다음 효과에서 오디오 버퍼 액세스에 필요한 인터페이스를 가져오도록 네임스페이스 내부에 다음 코드를 추가합니다.

[!code-cs[ComImport](./code/AudioGraph/AudioEffectComponent/ExampleAudioEffect.cs#SnippetComImport)]

> [!NOTE]
> 이 기술은 관리되지 않는 네이티브 이미지 버퍼에 액세스하므로 안전하지 않은 코드를 허용하도록 프로젝트를 구성해야 합니다.
> 1.  솔루션 탐색기에서 AudioEffectComponent 프로젝트를 마우스 오른쪽 단추로 클릭하고 **속성**을 선택합니다.
> 2.  **빌드** 탭을 선택합니다.
> 3.  **안전하지 않은 코드 허용** 확인란을 선택합니다.

 

이제 효과에 **ProcessFrame** 메서드 구현을 추가할 수 있습니다. 먼저 이 메서드는 입력 및 출력 오디오 프레임에서 [**AudioBuffer**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.AudioBuffer) 개체를 가져옵니다. 쓰기 작업을 위해 출력 프레임이 열려 있고 읽기 작업을 위해 입력 프레임이 열려 있는지 확인합니다. 다음으로 [**CreateReference**](https://msdn.microsoft.com/library/windows/apps/dn949046)를 호출하여 각 버퍼에 대해 [**IMemoryBufferReference**](https://msdn.microsoft.com/library/windows/apps/dn921671)를 획득합니다. 그런 후 **IMemoryBufferReference** 개체를 위에 정의된 COM interop 인터페이스인 **IMemoryByteAccess**로 캐스팅한 후 **GetBuffer**를 호출하여 실제 데이터 버퍼를 획득합니다.

이제 데이터 버퍼를 획득했으므로 입력 버퍼에서 읽고 출력 버퍼에 쓸 수 있습니다. 입력 버퍼에 있는 각 샘플에 대해 값을 가져와 1 - **Mix**로 곱하여 효과가 없는 신호 값을 설정합니다. 다음으로 샘플은 에코 버퍼의 현재 위치에서 가져와 **Mix**로 곱하여 효과가 적용된 값을 설정합니다. 출력 샘플은 효과 적용 전과 후의 값의 합계로 설정됩니다. 마지막으로 각 입력 샘플은 에코 버퍼에 저장되며 현재 샘플 인덱스는 증가합니다.

[!code-cs[ProcessFrame](./code/AudioGraph/AudioEffectComponent/ExampleAudioEffect.cs#SnippetProcessFrame)]



### <a name="close-method"></a>Close 메서드

시스템은 효과를 종료해야 하는 경우 클래스에 대해 [**Close**](https://msdn.microsoft.com/library/windows/apps/dn764782)[**Close**](https://msdn.microsoft.com/library/windows/apps/dn764782) 메서드를 호출합니다. 이 메서드를 사용하여 생성된 모든 리소스를 삭제해야 합니다. 이 메서드에 대한 인수는 효과가 정상적으로 닫혔는지, 오류가 발생했는지 또는 효과가 필수 인코딩 형식을 지원하지 않는지를 알 수 있도록 하는 [**MediaEffectClosedReason**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Effects.MediaEffectClosedReason)입니다.

[!code-cs[Close](./code/AudioGraph/AudioEffectComponent/ExampleAudioEffect.cs#SnippetClose)]

### <a name="discardqueuedframes-method"></a>DiscardQueuedFrames 메서드

[**DiscardQueuedFrames**](https://msdn.microsoft.com/library/windows/apps/dn764790) 메서드는 효과를 다시 설정해야 할 때 호출됩니다. 이에 대한 일반적인 시나리오는 효과가 현재 프레임 처리에 사용하기 위해 이전에 처리한 프레임을 저장하는 경우입니다. 이 메서드가 호출되면 저장했던 이전 프레임 집합을 삭제해야 합니다. 이 메서드는 누적된 오디오 프레임이 아니라 이전 프레임과 관련된 상태를 다시 설정하는 데 사용할 수 있습니다.

[!code-cs[DiscardQueuedFrames](./code/AudioGraph/AudioEffectComponent/ExampleAudioEffect.cs#SnippetDiscardQueuedFrames)]

### <a name="timeindependent-property"></a>TimeIndependent 속성

TimeIndependent[**TimeIndependent**](https://msdn.microsoft.com/library/windows/apps/dn764803) 속성을 사용하면 시스템은 효과에 균일한 타이밍이 필요한지 여부를 알 수 있습니다. 이 속성을 true로 설정하면 시스템에서는 효과 성능을 개선하는 최적화가 사용될 수 있습니다.

[!code-cs[TimeIndependent](./code/AudioGraph/AudioEffectComponent/ExampleAudioEffect.cs#SnippetTimeIndependent)]

### <a name="useinputframeforoutput-property"></a>UseInputFrameForOutput 속성

[**UseInputFrameForOutput**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Effects.IBasicAudioEffect.UseInputFrameForOutput) 속성을 **true**로 설정하여 효과가 출력을 [**OutputFrame**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Effects.ProcessAudioFrameContext.OutputFrame)에 기록하는 대신 [**ProcessFrame**](https://msdn.microsoft.com/library/windows/apps/dn764784)로 전달된 [**ProcessAudioFrameContext**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Effects.ProcessAudioFrameContext)의 [**InputFrame**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Effects.ProcessAudioFrameContext.InputFrame) 오디오 버퍼에 기록한다고 시스템에 알립니다. 

[!code-cs[UseInputFrameForOutput](./code/AudioGraph/AudioEffectComponent/ExampleAudioEffect.cs#SnippetUseInputFrameForOutput)]


## <a name="adding-your-custom-effect-to-your-app"></a>앱에 사용자 지정 효과 추가


앱에서 오디오 효과를 사용하려면 앱에 효과 프로젝트에 대한 참조를 추가해야 합니다.

1.  솔루션 탐색기에서 앱 프로젝트 아래에 있는 **참조**를 마우스 오른쪽 단추로 클릭하고 **참조 추가**를 선택합니다.
2.  **프로젝트** 탭을 확장하고 **솔루션**을 선택한 다음 효과 프로젝트 이름에 대한 확인란을 선택합니다. 이 예제에서 이름은 *AudioEffectComponent*입니다.
3.  **확인**을 클릭합니다.

오디오 효과 클래스에서 다른 네임스페이스로 선언되는 경우 해당 네임스페이스를 코드 파일에 포함해야 합니다.

[!code-cs[UsingAudioEffectComponent](./code/AudioGraph/cs/MainPage.xaml.cs#SnippetUsingAudioEffectComponent)]

### <a name="add-your-custom-effect-to-an-audiograph-node"></a>AudioGraph 노드에 사용자 지정 효과 추가
오디오 그래프를 사용하는 방법에 대한 일반적인 내용은 [오디오 그래프](audio-graphs.md)를 참조하세요. 다음 코드 조각은 이 문서에 나오는 에코 효과 예제를 오디오 그래프 노드에 추가하는 방법을 보여 줍니다. 먼저 효과에서 정의한 대로 [**PropertySet**](https://msdn.microsoft.com/library/windows/apps/Windows.Foundation.Collections.PropertySet)이 만들어지고 **Mix** 속성에 대한 값이 설정됩니다. 다음으로 [**AudioEffectDefinition**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Effects.AudioEffectDefinition) 생성자가 호출되어 사용자 지정 효과 유형 및 속성 집합의 전체 클래스 이름에 전달합니다. 마지막으로 효과 정의가 기존 [**FileInputNode**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Audio.CreateAudioFileInputNodeResult.FileInputNode)의 [**EffectDefinitions**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Audio.AudioFileInputNode.EffectDefinitions) 속성에 추가되어 이로 인해 방출된 오디오가 사용자 지정 효과로 처리됩니다. 

[!code-cs[AddCustomEffect](./code/AudioGraph/cs/MainPage.xaml.cs#SnippetAddCustomEffect)]

노드에 추가하고 나면 [**DisableEffectsByDefinition**](https://msdn.microsoft.com/library/windows/apps/dn958480)을 호출하고 **AudioEffectDefinition** 개체에 전달하여 사용자 지정 효과를 사용하지 않도록 설정할 수 있습니다. 앱에서 오디오 그래프를 사용하는 방법에 대한 자세한 내용은 [오디오 그래프](audio-graphs.md)를 참조하세요.

### <a name="add-your-custom-effect-to-a-clip-in-a-mediacomposition"></a>MediaComposition의 클립에 사용자 지정 효과 추가

다음 코드 조각은 비디오 클립에 대한 사용자 지정 오디오 효과와 미디어 컴퍼지션에 백그라운드 오디오 트랙을 추가하는 방법을 보여 줍니다. 비디오 클립에서 미디어 컴퍼지션을 만들고 백그라운드 오디오 트랙을 추가하는 방법에 대한 일반적인 지침은 [미디어 컴퍼지션 및 편집](media-compositions-and-editing.md)을 참조하세요.

[!code-cs[AddCustomAudioEffect](./code/MediaEditing/cs/MainPage.xaml.cs#SnippetAddCustomAudioEffect)]



## <a name="related-topics"></a>관련 항목
* [간단한 카메라 미리 보기 액세스](simple-camera-preview-access.md)
* [미디어 컴퍼지션 및 편집](media-compositions-and-editing.md)
* [Win2D 설명서](http://go.microsoft.com/fwlink/p/?LinkId=519078)
* [미디어 재생](media-playback.md)

 



