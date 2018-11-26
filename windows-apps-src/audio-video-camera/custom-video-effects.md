---
Description: This article describes how to create a Windows Runtime component that implements the IBasicVideoEffect interface to allow you to create custom effects for video streams.
MS-HAID: dev\_audio\_vid\_camera.custom\_video\_effects
MSHAttr: PreferredLib:/library/windows/apps
Search.Product: eADQiWindows 10XVcnh
title: 사용자 지정 비디오 효과
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.assetid: 40a6bd32-a756-400f-ba34-2c5f507262c0
ms.localizationpriority: medium
ms.openlocfilehash: a9e796eee76025e7697c08669e6942e0d69206f7
ms.sourcegitcommit: 681c70f964210ab49ac5d06357ae96505bb78741
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/26/2018
ms.locfileid: "7695890"
---
# <a name="custom-video-effects"></a>사용자 지정 비디오 효과




이 문서에서는 비디오 스트림에 대한 사용자 지정 효과를 만들 수 있도록 하는 [**IBasicVideoEffect**](https://msdn.microsoft.com/library/windows/apps/dn764788) 인터페이스를 구현하는 Windows 런타임 구성 요소를 만드는 방법을 설명합니다. 사용자 지정 효과는 장치의 카메라에 액세스할 수 있도록 하는 [MediaCapture](https://msdn.microsoft.com/library/windows/apps/br241124)와 미디어 클립에서 복잡한 컴퍼지션을 만들 수 있도록 하는 [**MediaComposition**](https://msdn.microsoft.com/library/windows/apps/dn652646)을 비롯한 여러 다른 Windows 런타임 API에서 사용할 수 있습니다.

## <a name="add-a-custom-effect-to-your-app"></a>앱에 사용자 지정 효과 추가


사용자 지정 비디오 효과는 [**IBasicVideoEffect**](https://msdn.microsoft.com/library/windows/apps/dn764788) 인터페이스를 구현하는 클래스에 정의됩니다. 이 클래스를 앱의 프로젝트에 직접 포함할 수는 없습니다. 대신 Windows 런타임 구성 요소를 사용하여 비디오 효과 클래스를 호스트해야 합니다.

**비디오 효과에 대한 Windows 런타임 구성 요소 추가**

1.  Microsoft Visual Studio에서 솔루션을 열고 **파일** 메뉴로 이동한 후 **추가-&gt;새 프로젝트**를 선택합니다.
2.  **Windows 런타임 구성 요소(유니버설 Windows)** 프로젝트 유형을 선택합니다.
3.  이 예제에서는 프로젝트 이름을 *VideoEffectComponent*로 지정합니다. 이 이름은 나중에 코드에서 참조됩니다.
4.  **확인**을 클릭합니다.
5.  프로젝트 템플릿은 Class1.cs라는 클래스를 만듭니다. **솔루션 탐색기**에서 Class1.cs 아이콘을 마우스 오른쪽 단추로 클릭하고 **이름 바꾸기**를 선택합니다.
6.  파일 이름을 *ExampleVideoEffect.cs*로 바꿉니다. Visual Studio에서 새 이름에 대한 모든 참조를 업데이트할지 묻는 메시지가 표시됩니다. **예**를 클릭합니다.
7.  **ExampleVideoEffect.cs**를 열고 클래스 정의를 업데이트하여 [**IBasicVideoEffect**](https://msdn.microsoft.com/library/windows/apps/dn764788) 인터페이스를 구현합니다.

[!code-cs[ImplementIBasicVideoEffect](./code/VideoEffect_Win10/cs/VideoEffectComponent/ExampleVideoEffect.cs#SnippetImplementIBasicVideoEffect)]


이 문서의 예제에서 사용되는 모든 유형에 액세스하기 위해 효과 클래스 파일에 다음 네임스페이스를 포함해야 합니다.

[!code-cs[EffectUsing](./code/VideoEffect_Win10/cs/VideoEffectComponent/ExampleVideoEffect.cs#SnippetEffectUsing)]


## <a name="implement-the-ibasicvideoeffect-interface-using-software-processing"></a>소프트웨어 처리를 사용하여 IBasicVideoEffect 인터페이스를 구현합니다.


비디오 효과는 [**IBasicVideoEffect**](https://msdn.microsoft.com/library/windows/apps/dn764788) 인터페이스의 모든 메서드 및 속성을 구현해야 합니다. 이 섹션에서는 소프트웨어 처리를 사용하는 이 인터페이스의 간단한 구현 과정을 안내합니다.

### <a name="close-method"></a>Close 메서드

시스템은 효과를 종료해야 하는 경우 클래스에 대해 [**Close**](https://msdn.microsoft.com/library/windows/apps/dn764789) 메서드를 호출합니다. 이 메서드를 사용하여 생성된 모든 리소스를 삭제해야 합니다. 이 메서드에 대한 인수는 효과가 정상적으로 닫혔는지, 오류가 발생했는지 또는 효과가 필수 인코딩 형식을 지원하지 않는지를 알 수 있도록 하는 [**MediaEffectClosedReason**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Effects.MediaEffectClosedReason)입니다.

[!code-cs[Close](./code/VideoEffect_Win10/cs/VideoEffectComponent/ExampleVideoEffect.cs#SnippetClose)]


### <a name="discardqueuedframes-method"></a>DiscardQueuedFrames 메서드

[**DiscardQueuedFrames**](https://msdn.microsoft.com/library/windows/apps/dn764790) 메서드는 효과를 다시 설정해야 할 때 호출됩니다. 이에 대한 일반적인 시나리오는 효과가 현재 프레임 처리에 사용하기 위해 이전에 처리한 프레임을 저장하는 경우입니다. 이 메서드가 호출되면 저장했던 이전 프레임 집합을 삭제해야 합니다. 이 메서드는 누적된 비디오 프레임이 아니라 이전 프레임과 관련된 상태를 다시 설정하는 데 사용할 수 있습니다.


[!code-cs[DiscardQueuedFrames](./code/VideoEffect_Win10/cs/VideoEffectComponent/ExampleVideoEffect.cs#SnippetDiscardQueuedFrames)]



### <a name="isreadonly-property"></a>IsReadOnly 속성

[**IsReadOnly**](https://msdn.microsoft.com/library/windows/apps/dn764792) 속성을 사용하면 효과가 효과의 출력에 쓰는지를 시스템에서 알 수 있게 됩니다. 앱에서 비디오 프레임을 수정하지 않는 경우(예: 비디오 프레임 분석만 수행하는 효과) 이 속성을 true로 설정해야 합니다. 그러면 프레임 입력이 프레임 출력에 효율적으로 복사됩니다.

> [!TIP]
> [**IsReadOnly**](https://msdn.microsoft.com/library/windows/apps/dn764792) 속성이 true로 설정되면 시스템은 [**ProcessFrame**](https://msdn.microsoft.com/library/windows/apps/dn764794)이 호출되기 전에 입력 프레임을 출력 프레임으로 복사합니다. **IsReadOnly** 속성을 true로 설정해도 **ProcessFrame**에서 효과의 출력 프레임에 쓸 수 없게 제한되지는 않습니다.


[!code-cs[IsReadOnly](./code/VideoEffect_Win10/cs/VideoEffectComponent/ExampleVideoEffect.cs#SnippetIsReadOnly)]

### <a name="setencodingproperties-method"></a>SetEncodingProperties 메서드

시스템에서 효과에 대해 [**SetEncodingProperties**](https://msdn.microsoft.com/library/windows/apps/dn919884)를 호출하므로 효과가 작동하는 비디오 스트림에 대한 인코딩 속성을 알 수 있습니다. 또한 이 메서드는 하드웨어 렌더링에 사용되는 Direct3D 장치에 대한 참조를 제공합니다. 이 장치의 사용법은 이 문서 뒷부분에 나오는 하드웨어 처리 예제에 나와 있습니다.

[!code-cs[SetEncodingProperties](./code/VideoEffect_Win10/cs/VideoEffectComponent/ExampleVideoEffect.cs#SnippetSetEncodingProperties)]


### <a name="supportedencodingproperties-property"></a>SupportedEncodingProperties 속성

시스템은 [**SupportedEncodingProperties**](https://msdn.microsoft.com/library/windows/apps/dn764799) 속성을 검사하여 효과에서 지원되는 인코딩 속성을 확인합니다. 효과의 소비자가 사용자가 지정한 속성을 사용하여 비디오를 인코딩할 수 없는 경우 이 속성은 효과에 대해 [**Close**](https://msdn.microsoft.com/library/windows/apps/dn764789)를 호출하고 비디오 파이프라인에서 해당 효과를 제거합니다.


[!code-cs[SupportedEncodingProperties](./code/VideoEffect_Win10/cs/VideoEffectComponent/ExampleVideoEffect.cs#SnippetSupportedEncodingProperties)]


> [!NOTE] 
> **SupportedEncodingProperties**에서 [**VideoEncodingProperties**](https://msdn.microsoft.com/library/windows/apps/hh701217) 개체의 빈 목록을 반환하는 경우 시스템에서는 기본적으로 ARGB32 인코딩이 사용됩니다.

 

### <a name="supportedmemorytypes-property"></a>SupportedMemoryTypes 속성

시스템은 [**SupportedMemoryTypes**](https://msdn.microsoft.com/library/windows/apps/dn764801) 속성을 검사하여 효과가 소프트웨어 메모리에 있는 비디오 프레임에 액세스하는지 또는 하드웨어(GPU) 메모리에 있는 비디오 프레임에 액세스하는지를 확인합니다. [**MediaMemoryTypes.Cpu**](https://msdn.microsoft.com/library/windows/apps/dn764822)를 반환하면 효과에는 [**SoftwareBitmap**](https://msdn.microsoft.com/library/windows/apps/dn887358) 개체에 이미지 데이터를 포함하는 입력 및 출력 프레임이 전달됩니다. **MediaMemoryTypes.Gpu**를 반환하면 효과에는 [**IDirect3DSurface**](https://msdn.microsoft.com/library/windows/apps/dn965505) 개체에 이미지 데이터를 포함하는 입력 및 출력 프레임이 전달됩니다.

[!code-cs[SupportedMemoryTypes](./code/VideoEffect_Win10/cs/VideoEffectComponent/ExampleVideoEffect.cs#SnippetSupportedMemoryTypes)]


> [!NOTE]
> [**MediaMemoryTypes.GpuAndCpu**](https://msdn.microsoft.com/library/windows/apps/dn764822)를 지정하는 경우 시스템은 GPU 또는 시스템 메모리 중에서 파이프라인에 더 효율적인 메모리를 사용합니다. 이 값을 사용할 때는 [**ProcessFrame**](https://msdn.microsoft.com/library/windows/apps/dn764794) 메서드를 검사하여 메서드에 전달된 [**SoftwareBitmap**](https://msdn.microsoft.com/library/windows/apps/dn887358) 또는 [**IDirect3DSurface**](https://msdn.microsoft.com/library/windows/apps/dn965505) 중에서 어디에 데이터가 들어 있는지 확인한 후 그에 따라 프레임을 처리합니다.

 

### <a name="timeindependent-property"></a>TimeIndependent 속성

[**TimeIndependent**](https://msdn.microsoft.com/library/windows/apps/dn764803) 속성을 사용하면 시스템은 효과에 균일한 타이밍이 필요한지 여부를 알 수 있습니다. 이 속성을 true로 설정하면 시스템에서는 효과 성능을 개선하는 최적화가 사용될 수 있습니다.

[!code-cs[TimeIndependent](./code/VideoEffect_Win10/cs/VideoEffectComponent/ExampleVideoEffect.cs#SnippetTimeIndependent)]

### <a name="setproperties-method"></a>SetProperties 메서드

[**SetProperties**](https://msdn.microsoft.com/library/windows/apps/br240986) 메서드를 사용하면 효과를 사용하는 앱에 따라 효과 매개 변수가 조정될 수 있습니다. 속성은 속성 이름 및 값의 [**IPropertySet**](https://msdn.microsoft.com/library/windows/apps/br226054) 맵으로 전달됩니다.


[!code-cs[SetProperties](./code/VideoEffect_Win10/cs/VideoEffectComponent/ExampleVideoEffect.cs#SnippetSetProperties)]


이 간단한 예제에서는 지정된 값에 따라 각 비디오 프레임의 픽셀이 흐리게 표시됩니다. 속성이 선언되고 TryGetValue를 사용하여 호출 앱에 의해 설정된 값을 가져옵니다. 값이 설정되지 않은 경우 기본값은 .5가 사용됩니다.

[!code-cs[FadeValue](./code/VideoEffect_Win10/cs/VideoEffectComponent/ExampleVideoEffect.cs#SnippetFadeValue)]


### <a name="processframe-method"></a>ProcessFrame 메서드

[**ProcessFrame**](https://msdn.microsoft.com/library/windows/apps/dn764794) 메서드는 효과가 비디오의 이미지 데이터를 수정하는 위치입니다. 이 메서드는 프레임당 한번씩 호출되고 [**ProcessVideoFrameContext**](https://msdn.microsoft.com/library/windows/apps/dn764826) 개체가 전달됩니다. 이 개체에는 처리될 수신 프레임이 포함된 입력 [**VideoFrame**](https://msdn.microsoft.com/library/windows/apps/dn930917) 개체와 나머지 비디오 파이프라인에 전달되는 이미지 데이터를 쓰는 출력 **VideoFrame** 개체가 포함됩니다. 이러한 각 **VideoFrame** 개체에는 [**SoftwareBitmap**](https://msdn.microsoft.com/library/windows/apps/dn930926) 속성 및 [**Direct3DSurface**](https://msdn.microsoft.com/library/windows/apps/dn930920) 속성이 있지만 이러한 속성 중에서 사용할 수 있는 속성은 [**SupportedMemoryTypes**](https://msdn.microsoft.com/library/windows/apps/dn764801) 속성에서 반환한 값에 따라 결정됩니다.

다음 예제에서는 소프트웨어 처리를 사용하는 **ProcessFrame** 메서드의 간단한 구현을 보여 줍니다. [**SoftwareBitmap**](https://msdn.microsoft.com/library/windows/apps/dn887358) 개체 사용에 대한 자세한 내용은 [이미징](imaging.md)을 참조하세요. 하드웨어 처리를 사용하는 **ProcessFrame** 구현 예제는 이 문서 뒷부분에 나와 있습니다.

**SoftwareBitmap**의 데이터 버퍼에 액세스하려면 COM interop이 필요하므로 **System.Runtime.InteropServices** 네임스페이스를 효과 클래스 파일에 포함해야 합니다.

[!code-cs[COMUsing](./code/VideoEffect_Win10/cs/VideoEffectComponent/ExampleVideoEffect.cs#SnippetCOMUsing)]


효과에서 이미지 버퍼에 액세스하기 위한 인터페이스를 가져오려면 네임스페이스 내에 다음 코드를 추가합니다.

[!code-cs[COMImport](./code/VideoEffect_Win10/cs/VideoEffectComponent/ExampleVideoEffect.cs#SnippetCOMImport)]


> [!NOTE]
> 이 기술은 관리되지 않는 네이티브 이미지 버퍼에 액세스하므로 안전하지 않은 코드를 허용하도록 프로젝트를 구성해야 합니다.
> 1.  솔루션 탐색기에서 VideoEffectComponent 프로젝트를 마우스 오른쪽 단추로 클릭하고 **속성**을 선택합니다.
> 2.  **빌드** 탭을 선택합니다.
> 3.  **안전하지 않은 코드 허용** 확인란을 선택합니다.

 

이제 **ProcessFrame** 메서드 구현을 추가할 수 있습니다. 먼저 이 메서드는 입력 및 출력 소프트웨어 비트맵에서 [**BitmapBuffer**](https://msdn.microsoft.com/library/windows/apps/dn887325) 개체를 가져옵니다. 쓰기 작업을 위해 출력 프레임이 열려 있고 읽기 작업을 위해 입력 프레임이 열려 있는지 확인합니다. 다음으로 [**CreateReference**](https://msdn.microsoft.com/library/windows/apps/dn949046)를 호출하여 각 버퍼에 대해 [**IMemoryBufferReference**](https://msdn.microsoft.com/library/windows/apps/dn921671)를 획득합니다. 그런 후 **IMemoryBufferReference** 개체를 위에 정의된 COM interop 인터페이스인 **IMemoryByteAccess**로 캐스팅한 후 **GetBuffer**를 호출하여 실제 데이터 버퍼를 획득합니다.

이제 데이터 버퍼를 획득했으므로 입력 버퍼에서 읽고 출력 버퍼에 쓸 수 있습니다. 버퍼의 너비, 진행 속도 및 초기 오프셋에 대한 정보를 제공하는 [**GetPlaneDescription**](https://msdn.microsoft.com/library/windows/apps/dn887330)을 호출하여 버퍼의 레이아웃을 획득합니다. 픽셀당 비트 수는 이전에 [**SetEncodingProperties**](https://msdn.microsoft.com/library/windows/apps/dn919884) 메서드를 사용하여 설정한 인코딩 속성에 의해 결정됩니다. 버퍼 형식 정보는 각 픽셀에 대한 버퍼의 인덱스를 찾는 데 사용됩니다. 원본 버퍼의 픽셀 값이 대상 버퍼로 복사되고, 색 값을 이 효과에 대해 정의된FadeValue 속성과 곱하여 지정된 크기만큼 흐리게 나타납니다.

[!code-cs[ProcessFrameSoftwareBitmap](./code/VideoEffect_Win10/cs/VideoEffectComponent/ExampleVideoEffect.cs#SnippetProcessFrameSoftwareBitmap)]


## <a name="implement-the-ibasicvideoeffect-interface-using-hardware-processing"></a>하드웨어 처리를 사용하여 IBasicVideoEffect 인터페이스를 구현합니다.


하드웨어(GPU)를 사용하여 사용자 지정 비디오 효과를 만드는 작업은 위에 설명된 소프트웨어 처리를 사용하는 방법과 거의 동일합니다. 이 섹션에서는 하드웨어 처리를 사용하는 효과의 몇 가지 차이점을 보여 줍니다. 이 예제에서는 Win2D Windows 런타임 API를 사용합니다. Win2D에 대한 자세한 내용은 [Win2D 설명서](http://go.microsoft.com/fwlink/?LinkId=519078)를 참조하세요.

이 문서 맨 처음에 나오는 **앱에 사용자 지정 효과 추가** 섹션에 설명된 것처럼 다음 단계에 따라 생성된 프로젝트에 Win2D NuGet 패키지를 추가합니다.

**효과 프로젝트에 Win2D NuGet 패키지를 추가하려면**

1.  **솔루션 탐색기**에서 **VideoEffectComponent** 프로젝트를 마우스 오른쪽 단추로 클릭하고 **NuGet 패키지 관리**를 선택합니다.
2.  창 위쪽에서 **찾아보기** 탭을 선택합니다.
3.  검색 상자에 **Win2D**를 입력합니다.
4.  **Win2D.uwp**를 선택한 다음 오른쪽 창에서 **설치**를 선택합니다.
5.  **변경 내용 검토** 대화 상자에 설치할 패키지가 표시됩니다. **확인**을 클릭합니다.
6.  패키지 라이선스에 동의합니다.

기본 프로젝트 설정에 포함된 네임스페이스 외에 Win2D에서 제공하는 다음 네임스페이스를 포함해야 합니다.

[!code-cs[UsingWin2D](./code/VideoEffect_Win10/cs/VideoEffectComponent/ExampleVideoEffectWin2D.cs#SnippetUsingWin2D)]


이 효과는 이미지 데이터에 작동하기 위해 GPU 메모리를 사용하므로 [**SupportedMemoryTypes**](https://msdn.microsoft.com/library/windows/apps/dn764801) 속성에서 [**MediaMemoryTypes.Gpu**](https://msdn.microsoft.com/library/windows/apps/dn764822)를 반환해야 합니다.

[!code-cs[SupportedMemoryTypesWin2D](./code/VideoEffect_Win10/cs/VideoEffectComponent/ExampleVideoEffectWin2D.cs#SnippetSupportedMemoryTypesWin2D)]


[**SupportedEncodingProperties**](https://msdn.microsoft.com/library/windows/apps/dn764799) 속성을 사용하여 효과가 지원할 인코딩 속성을 설정합니다. Win2D를 사용할 때는 ARGB32 인코딩을 사용해야 합니다.

[!code-cs[SupportedEncodingPropertiesWin2D](./code/VideoEffect_Win10/cs/VideoEffectComponent/ExampleVideoEffectWin2D.cs#SnippetSupportedEncodingPropertiesWin2D)]


[**SetEncodingProperties**](https://msdn.microsoft.com/library/windows/apps/dn919884) 메서드를 사용하여 해당 메서드에 전달된 [**IDirect3DDevice**](https://msdn.microsoft.com/library/windows/apps/dn895092)에서 새 Win2D **CanvasDevice** 개체를 만듭니다.

[!code-cs[SetEncodingPropertiesWin2D](./code/VideoEffect_Win10/cs/VideoEffectComponent/ExampleVideoEffectWin2D.cs#SnippetSetEncodingPropertiesWin2D)]


[**SetProperties**](https://msdn.microsoft.com/library/windows/apps/br240986) 구현은 이전 소프트웨어 처리 예제와 동일합니다. 이 예제에서는 **BlurAmount** 속성을 사용하여 Win2D 흐림 효과를 구성합니다.

[!code-cs[SetPropertiesWin2D](./code/VideoEffect_Win10/cs/VideoEffectComponent/ExampleVideoEffectWin2D.cs#SnippetSetPropertiesWin2D)]

[!code-cs[BlurAmountWin2D](./code/VideoEffect_Win10/cs/VideoEffectComponent/ExampleVideoEffectWin2D.cs#SnippetBlurAmountWin2D)]


마지막 단계는 이미지 데이터를 실제로 처리하는 [**ProcessFrame**](https://msdn.microsoft.com/library/windows/apps/dn764794) 메서드를 구현하는 것입니다.

Win2D API를 사용하면 입력 프레임의 [**Direct3DSurface**](https://msdn.microsoft.com/library/windows/apps/dn930920) 속성에서 **CanvasBitmap**이 만들어집니다. **CanvasRenderTarget**은 출력 프레임의 **Direct3DSurface**에서 만들어지고 **CanvasDrawingSession**은 이 렌더링 대상에서 만들어집니다. [**SetProperties**](https://msdn.microsoft.com/library/windows/apps/br240986)를 통해 효과가 노출하는 **BlurAmount** 속성을 사용하여 새 Win2D **GaussianBlurEffect**가 초기화됩니다. 마지막으로 **CanvasDrawingSession.DrawImage** 메서드가 호출되면서 흐림 효과를 사용하여 입력 비트맵을 렌더링 대상에 그립니다.

[!code-cs[ProcessFrameWin2D](./code/VideoEffect_Win10/cs/VideoEffectComponent/ExampleVideoEffectWin2D.cs#SnippetProcessFrameWin2D)]


## <a name="adding-your-custom-effect-to-your-app"></a>앱에 사용자 지정 효과 추가


앱에서 비디오 효과를 사용하려면 앱에 효과 프로젝트에 대한 참조를 추가해야 합니다.

1.  솔루션 탐색기에서 앱 프로젝트 아래에 있는 **참조**를 마우스 오른쪽 단추로 클릭하고 **참조 추가**를 선택합니다.
2.  **프로젝트** 탭을 확장하고 **솔루션**을 선택한 다음 효과 프로젝트 이름에 대한 확인란을 선택합니다. 이 예제에서 이름은 *VideoEffectComponent*입니다.
3.  **확인**을 클릭합니다.

### <a name="add-your-custom-effect-to-a-camera-video-stream"></a>카메라의 비디오 스트림에 사용자 지정 효과 추가

[간단한 카메라 미리 보기 액세스](simple-camera-preview-access.md) 문서의 단계에 따라 카메라에서 간단한 미리 보기 스트림을 설정할 수 있습니다. 이러한 단계에 따르면 카메라의 비디오 스트림에 액세스하는 데 사용되는 초기화된 [**MediaCapture**](https://msdn.microsoft.com/library/windows/apps/br241124) 개체가 제공됩니다.

카메라 스트림에 사용자 지정 비디오 효과를 추가하려면 먼저 새 [**VideoEffectDefinition**](https://msdn.microsoft.com/library/windows/apps/dn608055) 개체를 만들고 효과에 대한 네임스페이스 및 클래스 이름에 전달합니다. 다음으로 **MediaCapture** 개체의 [**AddVideoEffect**](https://msdn.microsoft.com/library/windows/apps/dn878035) 메서드를 호출하여 지정된 스트림에 효과를 추가합니다. 이 예제에서는 [**MediaStreamType.VideoPreview**](https://msdn.microsoft.com/library/windows/apps/br226640) 값을 사용하여 해당 효과가 미리 보기 스트림에 추가되어야 함을 지정합니다. 앱에서 비디오 캡처를 지원하는 경우 **MediaStreamType.VideoRecord**를 사용하여 캡처 스트림에 효과를 추가할 수도 있습니다. **AddVideoEffect**는 사용자 지정 효과 나타내는 [**IMediaExtension**](https://msdn.microsoft.com/library/windows/apps/br240985) 개체를 반환합니다. SetProperties 메서드를 사용하여 효과에 대한 구성을 설정할 수 있습니다.

효과가 추가되면 미리 보기 스트림을 시작하기 위해 [**StartPreviewAsync**](https://msdn.microsoft.com/library/windows/apps/br226613)가 호출됩니다.

[!code-cs[AddVideoEffectAsync](./code/VideoEffect_Win10/cs/VideoEffect_Win10/MainPage.xaml.cs#SnippetAddVideoEffectAsync)]



### <a name="add-your-custom-effect-to-a-clip-in-a-mediacomposition"></a>MediaComposition의 클립에 사용자 지정 효과 추가

비디오 클립에서 미디어 컴퍼지션을 만드는 방법에 대한 일반적인 지침은 [미디어 컴퍼지션 및 편집](media-compositions-and-editing.md)을 참조하세요. 다음 코드 조각은 사용자 지정 비디오 효과를 사용하는 간단한 미디어 컴퍼지션을 만드는 방법을 보여 줍니다. [**FileOpenPicker**](https://msdn.microsoft.com/library/windows/apps/br207847)로 사용자가 선택한 비디오 파일을 전달하여 [**CreateFromFileAsync**](https://msdn.microsoft.com/library/windows/apps/dn652607)를 호출함으로써 [**MediaClip**](https://msdn.microsoft.com/library/windows/apps/dn652596) 개체가 만들어지고 새 [**MediaComposition**](https://msdn.microsoft.com/library/windows/apps/dn652646)에 클립이 추가됩니다. 다음으로 새 [**VideoEffectDefinition**](https://msdn.microsoft.com/library/windows/apps/dn608055) 개체가 만들어지고 효과에 대한 네임스페이스와 클래스 이름이 생성자에 전달됩니다. 마지막으로 효과 정의가 **MediaClip** 개체의 [**VideoEffectDefinitions**](https://msdn.microsoft.com/library/windows/apps/dn652643) 컬렉션에 추가됩니다.


[!code-cs[AddEffectToComposition](./code/VideoEffect_Win10/cs/VideoEffect_Win10/MainPage.xaml.cs#SnippetAddEffectToComposition)]


## <a name="related-topics"></a>관련 항목
* [간단한 카메라 미리 보기 액세스](simple-camera-preview-access.md)
* [미디어 컴퍼지션 및 편집](media-compositions-and-editing.md)
* [Win2D 설명서](http://go.microsoft.com/fwlink/p/?LinkId=519078)
* [미디어 재생](media-playback.md)