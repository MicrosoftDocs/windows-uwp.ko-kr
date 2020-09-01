---
Description: 이 문서에서는 IBasicVideoEffect 인터페이스를 구현 하는 Windows 런타임 구성 요소를 만들어 비디오 스트림에 대 한 사용자 지정 효과를 만들 수 있도록 하는 방법을 설명 합니다.
MS-HAID: dev\_audio\_vid\_camera.custom\_video\_effects
MSHAttr: PreferredLib:/library/windows/apps
Search.Product: eADQiWindows 10XVcnh
title: 사용자 지정 비디오 효과
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.assetid: 40a6bd32-a756-400f-ba34-2c5f507262c0
ms.localizationpriority: medium
ms.openlocfilehash: 89bb542e0da8249253e40085163defe3ac24e805
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89170607"
---
# <a name="custom-video-effects"></a>사용자 지정 비디오 효과




이 문서에서는 [**Ibasicvideoeffect**](/uwp/api/Windows.Media.Effects.IBasicVideoEffect) 인터페이스를 구현 하는 Windows 런타임 구성 요소를 만들어 비디오 스트림에 대 한 사용자 지정 효과를 만드는 방법을 설명 합니다. 사용자 지정 효과는 장치 카메라에 대 한 액세스를 제공 하는 [MediaCapture](/uwp/api/Windows.Media.Capture.MediaCapture)을 비롯 한 다양 한 Windows 런타임 api와 함께 사용할 수 있으며, 미디어 클립에서 복잡 한 컴퍼지션을 만들 수 있는 [**Mediacomposition**](/uwp/api/Windows.Media.Editing.MediaComposition)을 제공 합니다.

## <a name="add-a-custom-effect-to-your-app"></a>앱에 사용자 지정 효과 추가


사용자 지정 비디오 효과는 [**Ibasicvideoeffect**](/uwp/api/Windows.Media.Effects.IBasicVideoEffect) 인터페이스를 구현 하는 클래스에서 정의 됩니다. 이 클래스는 앱의 프로젝트에 직접 포함할 수 없습니다. 대신 Windows 런타임 구성 요소를 사용 하 여 비디오 효과 클래스를 호스팅해야 합니다.

**비디오 효과에 대 한 Windows 런타임 구성 요소 추가**

1.  Microsoft Visual Studio에서 솔루션을 연 상태에서 **파일** 메뉴로 이동 하 여 **추가- &gt; 새 프로젝트**를 선택 합니다.
2.  **Windows 런타임 구성 요소 (유니버설 Windows)** 프로젝트 형식을 선택 합니다.
3.  이 예에서는 프로젝트 이름을 *VideoEffectComponent*로 합니다. 이 이름은 나중에 코드에서 참조 됩니다.
4.  **확인**을 클릭합니다.
5.  프로젝트 템플릿은 Class1.cs 이라는 클래스를 만듭니다. **솔루션 탐색기**에서 Class1.cs의 아이콘을 마우스 오른쪽 단추로 클릭 하 고 **이름 바꾸기**를 선택 합니다.
6.  파일의 이름을 *ExampleVideoEffect.cs*로 바꿉니다. Visual Studio에는 새 이름에 대 한 모든 참조를 업데이트할 것인지 묻는 메시지가 표시 됩니다. **예**를 클릭합니다.
7.  **ExampleVideoEffect.cs** 를 열고 클래스 정의를 업데이트 하 여 [**Ibasicvideoeffect**](/uwp/api/Windows.Media.Effects.IBasicVideoEffect) 인터페이스를 구현 합니다.

[!code-cs[ImplementIBasicVideoEffect](./code/VideoEffect_Win10/cs/VideoEffectComponent/ExampleVideoEffect.cs#SnippetImplementIBasicVideoEffect)]


이 문서의 예제에 사용 된 모든 형식에 액세스 하려면 다음 네임 스페이스를 효과 클래스 파일에 포함 해야 합니다.

[!code-cs[EffectUsing](./code/VideoEffect_Win10/cs/VideoEffectComponent/ExampleVideoEffect.cs#SnippetEffectUsing)]


## <a name="implement-the-ibasicvideoeffect-interface-using-software-processing"></a>소프트웨어 처리를 사용 하 여 IBasicVideoEffect 인터페이스 구현


비디오 효과는 [**Ibasicvideoeffect**](/uwp/api/Windows.Media.Effects.IBasicVideoEffect) 인터페이스의 모든 메서드 및 속성을 구현 해야 합니다. 이 섹션에서는 소프트웨어 처리를 사용 하는이 인터페이스의 간단한 구현 과정을 안내 합니다.

### <a name="close-method"></a>Close 메서드

시스템은 효과가 종료 될 때 클래스에서 [**Close**](/uwp/api/windows.media.effects.ibasicvideoeffect.close) 메서드를 호출 합니다. 사용자가 만든 리소스를 삭제 하려면이 메서드를 사용 해야 합니다. 메서드에 대 한 인수는 효과를 정상적으로 닫 았는 지, 오류가 발생 한 경우 또는 효과가 필요한 인코딩 형식을 지원 하지 않는지를 알 수 있도록 하는 [**MediaEffectClosedReason**](/uwp/api/Windows.Media.Effects.MediaEffectClosedReason) 입니다.

[!code-cs[Close](./code/VideoEffect_Win10/cs/VideoEffectComponent/ExampleVideoEffect.cs#SnippetClose)]


### <a name="discardqueuedframes-method"></a>DiscardQueuedFrames 메서드

[**DiscardQueuedFrames**](/uwp/api/windows.media.effects.ibasicvideoeffect.discardqueuedframes) 메서드는 효과를 다시 설정 해야 할 때 호출 됩니다. 이에 대 한 일반적인 시나리오는 효과가 현재 프레임을 처리 하는 데 사용 하기 위해 이전에 처리 된 프레임을 저장 하는 경우입니다. 이 메서드를 호출 하면 저장 한 이전 프레임 집합을 삭제 해야 합니다. 이 메서드는 누적 비디오 프레임 뿐만 아니라 이전 프레임과 관련 된 모든 상태를 다시 설정 하는 데 사용할 수 있습니다.


[!code-cs[DiscardQueuedFrames](./code/VideoEffect_Win10/cs/VideoEffectComponent/ExampleVideoEffect.cs#SnippetDiscardQueuedFrames)]



### <a name="isreadonly-property"></a>IsReadOnly 속성

[**IsReadOnly**](/uwp/api/windows.media.effects.ibasicvideoeffect.isreadonly) 속성을 사용 하면 효과가 효과의 출력에 기록 되는지 여부를 시스템에서 알 수 있습니다. 앱이 비디오 프레임의 분석만 수행 하는 효과와 같이 비디오 프레임을 수정 하지 않는 경우에는이 속성을 true로 설정 해야 합니다. 이렇게 하면 시스템에서 프레임 입력을 프레임 출력에 효율적으로 복사할 수 있습니다.

> [!TIP]
> [**IsReadOnly**](/uwp/api/windows.media.effects.ibasicvideoeffect.isreadonly) 속성을 true로 설정 하면 [**processframe**](/uwp/api/windows.media.effects.ibasicvideoeffect.processframe) 이 호출 되기 전에 시스템에서 입력 프레임을 출력 프레임에 복사 합니다. **IsReadOnly** 속성을 true로 설정 하면 **processframe**의 효과 출력 프레임에 쓰지 않아도 됩니다.


[!code-cs[IsReadOnly](./code/VideoEffect_Win10/cs/VideoEffectComponent/ExampleVideoEffect.cs#SnippetIsReadOnly)]

### <a name="setencodingproperties-method"></a>SetEncodingProperties 메서드

시스템은 효과가 적용 되는 비디오 스트림에 대 한 인코딩 속성을 알려 주는 효과에 대해 [**SetEncodingProperties**](/uwp/api/windows.media.effects.ibasicvideoeffect.setencodingproperties) 를 호출 합니다. 또한이 메서드는 하드웨어 렌더링에 사용 되는 Direct3D 장치에 대 한 참조를 제공 합니다. 이 장치를 사용 하는 방법은이 문서의 뒷부분에 나오는 하드웨어 처리 예제에 나와 있습니다.

[!code-cs[SetEncodingProperties](./code/VideoEffect_Win10/cs/VideoEffectComponent/ExampleVideoEffect.cs#SnippetSetEncodingProperties)]


### <a name="supportedencodingproperties-property"></a>SupportedEncodingProperties 속성

시스템은 [**SupportedEncodingProperties**](/uwp/api/windows.media.effects.ibasicvideoeffect.supportedencodingproperties) 속성을 확인 하 여 효과에서 지원 되는 인코딩 속성을 확인 합니다. 사용자가 지정한 속성을 사용 하 여 비디오를 인코딩할 수 없는 경우에는 효과에 대해 [**Close**](/uwp/api/windows.media.effects.ibasicvideoeffect.close) 를 호출 하 여 비디오 파이프라인에서 효과를 제거 합니다.


[!code-cs[SupportedEncodingProperties](./code/VideoEffect_Win10/cs/VideoEffectComponent/ExampleVideoEffect.cs#SnippetSupportedEncodingProperties)]


> [!NOTE] 
> **SupportedEncodingProperties**에서 빈 [**VideoEncodingProperties**](/uwp/api/Windows.Media.MediaProperties.VideoEncodingProperties) 개체 목록을 반환 하면 시스템은 기본적으로 ARGB32 encoding으로 표시 됩니다.

 

### <a name="supportedmemorytypes-property"></a>SupportedMemoryTypes 속성

시스템은 [**Supportedmemorytypes**](/uwp/api/windows.media.effects.ibasicvideoeffect.supportedmemorytypes) 속성을 확인 하 여 효과가 소프트웨어 메모리 또는 하드웨어 (GPU) 메모리의 비디오 프레임에 액세스 하는지 여부를 확인 합니다. [**MediaMemoryTypes**](/uwp/api/Windows.Media.Effects.MediaMemoryTypes)를 반환 하는 경우에는 영향을 받은 입력 및 출력 [**프레임에 데이터**](/uwp/api/Windows.Graphics.Imaging.SoftwareBitmap) 를 포함 하 고 있습니다. **MediaMemoryTypes**를 반환 하는 경우 [**IDirect3DSurface**](/uwp/api/Windows.Graphics.DirectX.Direct3D11.IDirect3DSurface) 개체에 이미지 데이터가 포함 된 입력 및 출력 프레임에 결과가 전달 됩니다.

[!code-cs[SupportedMemoryTypes](./code/VideoEffect_Win10/cs/VideoEffectComponent/ExampleVideoEffect.cs#SnippetSupportedMemoryTypes)]


> [!NOTE]
> [**MediaMemoryTypes. GpuAndCpu**](/uwp/api/Windows.Media.Effects.MediaMemoryTypes)를 지정 하면 시스템은 GPU 또는 시스템 메모리 중에서 파이프라인에 대해 더 효율적으로 사용 됩니다. 이 값을 사용 하는 경우 [**Processframe**](/uwp/api/windows.media.effects.ibasicvideoeffect.processframe) 메서드를 검사 하 여 메서드에 전달 된 IDirect3DSurface [**비트맵**](/uwp/api/Windows.Graphics.Imaging.SoftwareBitmap) 또는 [**IDirect3DSurface**](/uwp/api/Windows.Graphics.DirectX.Direct3D11.IDirect3DSurface) 에 데이터가 포함 되어 있는지 확인 한 다음 그에 따라 프레임을 처리 해야 합니다.

 

### <a name="timeindependent-property"></a>TimeIndependent 속성

[**Timeindependent**](/uwp/api/windows.media.effects.ibasicvideoeffect.timeindependent) 속성을 사용 하면 효과에 균일 한 시간이 필요 하지 않은 경우 시스템에서 알 수 있습니다. True로 설정 되 면 시스템은 성능 향상을 위해 최적화를 사용할 수 있습니다.

[!code-cs[TimeIndependent](./code/VideoEffect_Win10/cs/VideoEffectComponent/ExampleVideoEffect.cs#SnippetTimeIndependent)]

### <a name="setproperties-method"></a>SetProperties 메서드

[**SetProperties**](/uwp/api/windows.media.imediaextension.setproperties) 메서드를 사용 하면 효과를 사용 하는 앱에서 효과 매개 변수를 조정할 수 있습니다. 속성은 속성 이름 및 값의 [**IPropertySet**](/uwp/api/Windows.Foundation.Collections.IPropertySet) 맵으로 전달 됩니다.


[!code-cs[SetProperties](./code/VideoEffect_Win10/cs/VideoEffectComponent/ExampleVideoEffect.cs#SnippetSetProperties)]


이 간단한 예제는 지정 된 값에 따라 각 비디오 프레임의 픽셀을 어둡게 만듭니다. 속성이 선언 되 고 TryGetValue가 호출 앱에서 설정한 값을 가져오는 데 사용 됩니다. 값이 설정 되지 않은 경우 기본값. 5가 사용 됩니다.

[!code-cs[FadeValue](./code/VideoEffect_Win10/cs/VideoEffectComponent/ExampleVideoEffect.cs#SnippetFadeValue)]


### <a name="processframe-method"></a>ProcessFrame 메서드

[**Processframe**](/uwp/api/windows.media.effects.ibasicvideoeffect.processframe) 메서드는 비디오의 이미지 데이터를 효과에서 수정 하는 위치입니다. 메서드는 프레임당 한 번 호출 되며 [**ProcessVideoFrameContext**](/uwp/api/Windows.Media.Effects.ProcessVideoFrameContext) 개체에 전달 됩니다. 이 개체에는 처리할 들어오는 프레임 및 비디오 파이프라인의 나머지 부분에 전달할 이미지 데이터를 작성 하는 출력 **videoframe** 개체를 포함 하는 입력 [**videoframe**](/uwp/api/Windows.Media.VideoFrame) 개체가 포함 되어 있습니다. 이러한 각 **Videoframe** 개체는 [**Direct3DSurface**](/uwp/api/windows.media.videoframe.direct3dsurface) 속성을 포함 하지만 [**이 속성은**](/uwp/api/windows.media.videoframe.softwarebitmap) [**supportedmemorytypes**](/uwp/api/windows.media.effects.ibasicvideoeffect.supportedmemorytypes) 속성에서 반환 된 값에 따라 결정 됩니다.

이 예제에서는 소프트웨어 처리를 사용 하는 **Processframe** 메서드의 간단한 구현을 보여 줍니다. [**\Bitmap**](/uwp/api/Windows.Graphics.Imaging.SoftwareBitmap) 개체 작업에 대 한 자세한 내용은 [Imaging](imaging.md)을 참조 하세요. 하드웨어 처리를 사용 하는 **Processframe** 구현 예제는이 문서의 뒷부분에 나와 있습니다.

T e m **비트맵** 의 데이터 버퍼에 액세스 하려면 COM interop 필요 하므로 효과 클래스 파일에 **System.Runtime.InteropServices** 네임 스페이스를 포함 해야 합니다.

[!code-cs[COMUsing](./code/VideoEffect_Win10/cs/VideoEffectComponent/ExampleVideoEffect.cs#SnippetCOMUsing)]


이미지 버퍼에 액세스 하기 위한 인터페이스를 가져오기 위한 효과에 대 한 네임 스페이스 내에 다음 코드를 추가 합니다.

[!code-cs[COMImport](./code/VideoEffect_Win10/cs/VideoEffectComponent/ExampleVideoEffect.cs#SnippetCOMImport)]


> [!NOTE]
> 이 기술은 관리 되지 않는 네이티브 이미지 버퍼에 액세스 하므로 안전 하지 않은 코드를 허용 하도록 프로젝트를 구성 해야 합니다.
> 1.  솔루션 탐색기에서 VideoEffectComponent 프로젝트를 마우스 오른쪽 단추로 클릭 하 고 **속성**을 선택 합니다.
> 2.  **빌드** 탭을 선택합니다.
> 3.  **안전 하지 않은 코드 허용** 확인란을 선택 합니다.

 

이제 **Processframe** 메서드 구현을 추가할 수 있습니다. 첫째,이 메서드는 입력 및 출력 소프트웨어 비트맵에서 [**BitmapBuffer**](/uwp/api/Windows.Graphics.Imaging.BitmapBuffer) 개체를 가져옵니다. 출력 프레임은 쓰기용으로 열리고 입력은 읽기를 위해 열립니다. 그런 다음 [**CreateReference**](/uwp/api/windows.graphics.imaging.bitmapbuffer.createreference)를 호출 하 여 각 버퍼에 대해 [**IMemoryBufferReference**](/uwp/api/Windows.Foundation.IMemoryBufferReference) 를 가져옵니다. 그런 다음 **IMemoryBufferReference** 개체를 위에서 정의한 COM interop 인터페이스로 캐스팅 하 고 **IMemoryByteAccess**다음 **getbuffer**를 호출 하 여 실제 데이터 버퍼를 가져옵니다.

이제 데이터 버퍼를 얻 었으 면 입력 버퍼에서 읽고 출력 버퍼에 쓸 수 있습니다. 버퍼의 레이아웃은 버퍼의 너비, stride 및 초기 오프셋에 대 한 정보를 제공 하는 [**Getplanedescription**](/uwp/api/windows.graphics.imaging.bitmapbuffer.getplanedescription)을 호출 하 여 가져옵니다. 픽셀당 비트 수는 [**SetEncodingProperties**](/uwp/api/windows.media.effects.ibasicvideoeffect.setencodingproperties) 메서드를 사용 하 여 이전에 설정 된 인코딩 속성에 따라 결정 됩니다. 버퍼 형식 정보는 각 픽셀에 대 한 버퍼의 인덱스를 찾는 데 사용 됩니다. 원본 버퍼의 픽셀 값이 대상 버퍼에 복사 되 고, 색 값이이 효과에 대해 정의 된 FadeValue 속성을 곱하여 지정 된 양만큼 크기가 지정 됩니다.

[!code-cs[ProcessFrameSoftwareBitmap](./code/VideoEffect_Win10/cs/VideoEffectComponent/ExampleVideoEffect.cs#SnippetProcessFrameSoftwareBitmap)]


## <a name="implement-the-ibasicvideoeffect-interface-using-hardware-processing"></a>하드웨어 처리를 사용 하 여 IBasicVideoEffect 인터페이스 구현


하드웨어 (GPU) 처리를 사용 하 여 사용자 지정 비디오 효과를 만드는 것은 위에서 설명한 대로 소프트웨어 처리를 사용 하는 것과 거의 동일 합니다. 이 섹션에서는 하드웨어 처리를 사용 하는 효과의 몇 가지 차이점을 보여 줍니다. 이 예제에서는 Win2D Windows 런타임 API를 사용 합니다. Win2D에 대한 자세한 내용은 [Win2D 설명서](https://microsoft.github.io/Win2D/html/Introduction.htm)를 참조하세요.

다음 단계를 사용 하 여이 문서의 시작 부분에 있는 **앱에 사용자 지정 효과 추가** 섹션에 설명 된 대로 만든 프로젝트에 Win2D NuGet 패키지를 추가 합니다.

**Win2D NuGet 패키지를 효과 프로젝트에 추가 하려면**

1.  **솔루션 탐색기**에서 **VideoEffectComponent** 프로젝트를 마우스 오른쪽 단추로 클릭 하 고 **NuGet 패키지 관리**를 선택 합니다.
2.  창 위쪽에서 **찾아보기** 탭을 선택 합니다.
3.  검색 상자에 **Win2D**를 입력 합니다.
4.  **Win2D**를 선택한 다음 오른쪽 창에서 **설치** 를 선택 합니다.
5.  **변경 내용 검토** 대화 상자에 설치할 패키지를 표시 합니다. **확인**을 클릭합니다.
6.  패키지 라이선스에 동의 합니다.

기본 프로젝트 설정에 포함 된 네임 스페이스 외에도 Win2D에서 제공 하는 다음 네임 스페이스를 포함 해야 합니다.

[!code-cs[UsingWin2D](./code/VideoEffect_Win10/cs/VideoEffectComponent/ExampleVideoEffectWin2D.cs#SnippetUsingWin2D)]


이 효과는 이미지 데이터에서 작동 하는 데 GPU 메모리를 사용 하므로 [**Supportedmemorytypes**](/uwp/api/windows.media.effects.ibasicvideoeffect.supportedmemorytypes) 속성에서 [**MediaMemoryTypes**](/uwp/api/Windows.Media.Effects.MediaMemoryTypes) 를 반환 해야 합니다.

[!code-cs[SupportedMemoryTypesWin2D](./code/VideoEffect_Win10/cs/VideoEffectComponent/ExampleVideoEffectWin2D.cs#SnippetSupportedMemoryTypesWin2D)]


[**SupportedEncodingProperties**](/uwp/api/windows.media.effects.ibasicvideoeffect.supportedencodingproperties) 속성을 사용 하 여 효과에서 지원할 인코딩 속성을 설정 합니다. Win2D를 사용 하 여 작업 하는 경우 ARGB32 encoding을 사용 해야 합니다.

[!code-cs[SupportedEncodingPropertiesWin2D](./code/VideoEffect_Win10/cs/VideoEffectComponent/ExampleVideoEffectWin2D.cs#SnippetSupportedEncodingPropertiesWin2D)]


[**SetEncodingProperties**](/uwp/api/windows.graphics.imaging.softwarebitmap.convert) 메서드를 사용 하 여 메서드로 전달 된 [**IDirect3DDevice**](/uwp/api/Windows.Graphics.DirectX.Direct3D11.IDirect3DDevice) 에서 새 Win2D **CanvasDevice** 개체를 만듭니다.

[!code-cs[SetEncodingPropertiesWin2D](./code/VideoEffect_Win10/cs/VideoEffectComponent/ExampleVideoEffectWin2D.cs#SnippetSetEncodingPropertiesWin2D)]


[**SetProperties**](/uwp/api/windows.media.imediaextension.setproperties) 구현은 이전 소프트웨어 처리 예제와 동일 합니다. 이 예제에서는 **BlurAmount** 속성을 사용 하 여 Win2D 흐림 효과를 구성 합니다.

[!code-cs[SetPropertiesWin2D](./code/VideoEffect_Win10/cs/VideoEffectComponent/ExampleVideoEffectWin2D.cs#SnippetSetPropertiesWin2D)]

[!code-cs[BlurAmountWin2D](./code/VideoEffect_Win10/cs/VideoEffectComponent/ExampleVideoEffectWin2D.cs#SnippetBlurAmountWin2D)]


마지막 단계는 이미지 데이터를 실제로 처리 하는 [**Processframe**](/uwp/api/windows.media.effects.ibasicvideoeffect.processframe) 메서드를 구현 하는 것입니다.

Win2D Api를 사용 하 여 입력 프레임의 [**Direct3DSurface**](/uwp/api/windows.media.videoframe.direct3dsurface) 속성에서 **CanvasBitmap** 를 만듭니다. **CanvasRenderTarget** 는 출력 프레임의 **Direct3DSurface** 에서 만들어지며이 렌더링 대상에서 **CanvasDrawingSession** 생성 됩니다. 새 Win2D **GaussianBlurEffect** 는 **BlurAmount** 속성을 사용 하 여 초기화 됩니다 .이 속성은 [**SetProperties**](/uwp/api/windows.media.imediaextension.setproperties)를 통해 노출 됩니다. 마지막으로, 흐림 효과를 사용 하 여 **CanvasDrawingSession DrawImage** 메서드를 호출 하 여 렌더링 대상에 입력 비트맵을 그립니다.

[!code-cs[ProcessFrameWin2D](./code/VideoEffect_Win10/cs/VideoEffectComponent/ExampleVideoEffectWin2D.cs#SnippetProcessFrameWin2D)]


## <a name="adding-your-custom-effect-to-your-app"></a>앱에 사용자 지정 효과 추가


앱에서 비디오 효과를 사용 하려면 앱에 효과 프로젝트에 대 한 참조를 추가 해야 합니다.

1.  솔루션 탐색기의 앱 프로젝트에서 **참조** 를 마우스 오른쪽 단추로 클릭 하 고 **참조 추가**를 선택 합니다.
2.  **프로젝트** 탭을 확장 하 고 **솔루션**을 선택한 다음 효과 프로젝트 이름에 대 한 확인란을 선택 합니다. 이 예에서는 이름이 *VideoEffectComponent*입니다.
3.  **확인**을 클릭합니다.

### <a name="add-your-custom-effect-to-a-camera-video-stream"></a>카메라 비디오 스트림에 사용자 지정 효과 추가

[간단한 카메라 미리 보기 액세스](simple-camera-preview-access.md)문서의 단계에 따라 카메라에서 간단한 미리 보기 스트림을 설정할 수 있습니다. 이러한 단계를 수행 하면 카메라의 비디오 스트림에 액세스 하는 데 사용 되는 초기화 된 [**MediaCapture**](/uwp/api/Windows.Media.Capture.MediaCapture) 개체가 제공 됩니다.

카메라 스트림에 사용자 지정 비디오 효과를 추가 하려면 먼저 새 [**VideoEffectDefinition**](/uwp/api/Windows.Media.Effects.VideoEffectDefinition) 개체를 만들어 효과에 대 한 네임 스페이스 및 클래스 이름을 전달 합니다. 그런 다음 **MediaCapture** 개체의 [**addvideoeffect**](/uwp/api/windows.media.capture.mediacapture.addvideoeffectasync) 메서드를 호출 하 여 지정 된 스트림에 효과를 추가 합니다. 이 예에서는 [**Mediastreamtype**](/uwp/api/Windows.Media.Capture.MediaStreamType) 값을 사용 하 여 미리 보기 스트림에 효과를 추가 하도록 지정 합니다. 앱에서 비디오 캡처를 지 원하는 경우 VideoRecord를 사용 하 여 캡처 스트림에 효과를 추가할 수도 있습니다 **.** **Addvideoeffect** 사용자 지정 효과를 나타내는 [**imediaextension**](/uwp/api/Windows.Media.IMediaExtension) 개체를 반환 합니다. SetProperties 메서드를 사용 하 여 효과에 대 한 구성을 설정할 수 있습니다.

효과가 추가 된 후에는 [**StartPreviewAsync**](/uwp/api/windows.media.capture.mediacapture.startpreviewasync) 를 호출 하 여 미리 보기 스트림을 시작 합니다.

[!code-cs[AddVideoEffectAsync](./code/VideoEffect_Win10/cs/VideoEffect_Win10/MainPage.xaml.cs#SnippetAddVideoEffectAsync)]



### <a name="add-your-custom-effect-to-a-clip-in-a-mediacomposition"></a>MediaComposition의 클립에 사용자 지정 효과 추가

비디오 클립에서 미디어 컴퍼지션을 만드는 방법에 대 한 일반적인 지침은 [미디어 컴포지션 및 편집](media-compositions-and-editing.md)을 참조 하세요. 다음 코드 조각에서는 사용자 지정 비디오 효과를 사용 하는 간단한 미디어 컴포지션의 생성을 보여 줍니다. [**Mediaclip**](/uwp/api/Windows.Media.Editing.MediaClip) 개체는 [**CreateFromFileAsync**](/uwp/api/windows.media.editing.mediaclip.createfromfileasync)를 호출 하 고 [**fileopenpicker**](/uwp/api/Windows.Storage.Pickers.FileOpenPicker)를 사용 하 여 사용자가 선택한 비디오 파일을 전달 하 여 생성 되며, 새 [**mediacomposition**](/uwp/api/Windows.Media.Editing.MediaComposition)에 클립이 추가 됩니다. 그런 다음 새 [**VideoEffectDefinition**](/uwp/api/Windows.Media.Effects.VideoEffectDefinition) 개체를 만들어 효과에 대 한 네임 스페이스 및 클래스 이름을 생성자에 전달 합니다. 마지막으로 효과 정의가 **Mediaclip** 개체의 [**VideoEffectDefinitions**](/uwp/api/windows.media.editing.mediaclip.videoeffectdefinitions) 컬렉션에 추가 됩니다.


[!code-cs[AddEffectToComposition](./code/VideoEffect_Win10/cs/VideoEffect_Win10/MainPage.xaml.cs#SnippetAddEffectToComposition)]


## <a name="related-topics"></a>관련 항목
* [간단한 카메라 미리 보기 액세스](simple-camera-preview-access.md)
* [미디어 컴퍼지션 및 편집](media-compositions-and-editing.md)
* [Win2D 설명서](https://microsoft.github.io/Win2D/html/Introduction.htm)
* [미디어 재생](media-playback.md)