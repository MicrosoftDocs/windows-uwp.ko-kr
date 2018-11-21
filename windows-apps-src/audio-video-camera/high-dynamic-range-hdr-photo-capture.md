---
author: drewbatgit
ms.assetid: 0186EA01-8446-45BA-A109-C5EB4B80F368
description: 이 문서에서는 AdvancedPhotoCapture 클래스를 사용하여 HDR(High Dynamic Range) 및 낮은 조명 사진을 캡처하는 방법을 보여 줍니다.
title: HDR(High Dynamic Range) 및 어두운 조명 사진 캡처
ms.author: drewbat
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: cc81fdc03096dd8cac5e844a496b38da78eb6136
ms.sourcegitcommit: cbe7cf620622a5e4df7414f9e38dfecec1cfca99
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/21/2018
ms.locfileid: "7435798"
---
# <a name="high-dynamic-range-hdr-and-low-light-photo-capture"></a>HDR(High Dynamic Range) 및 어두운 조명 사진 캡처



이 문서에서는 [**AdvancedPhotoCapture**](https://msdn.microsoft.com/library/windows/apps/mt181386) 클래스를 사용하여 HDR(High Dynamic Range) 사진을 캡처하는 방법을 보여 줍니다. 이 API를 사용하면 최종 이미지의 처리를 완료하기 전에 HDR 캡처에서 참조 프레임을 얻을 수 있습니다.

HDR 캡처와 관련된 기타 문서는 다음과 같습니다.

-   [**SceneAnalysisEffect**](https://msdn.microsoft.com/library/windows/apps/dn948902)를 사용할 경우 시스템은 미디어 캡처 미리 보기 스트림의 콘텐츠를 평가하여 HDR 처리로 인해 캡처 결과가 향상될 수 있는지 파악할 수 있습니다. 자세한 내용은 [MediaCapture의 장면 분석](scene-analysis-for-media-capture.md)을 참조하세요.

-   [**HdrVideoControl**](https://msdn.microsoft.com/library/windows/apps/dn926680)을 사용하여 Windows 기본 제공 HDR 처리 알고리즘을 사용해 동영상을 캡처합니다. 자세한 내용은 [비디오 캡처를 위한 캡처 디바이스 컨트롤](capture-device-controls-for-video-capture.md)을 참조하세요.

-   [**VariablePhotoSequenceCapture**](https://msdn.microsoft.com/library/windows/apps/dn652564)를 사용하여 각각이 다른 캡처 설정을 갖는 사진 시퀀스를 캡처하고 고유한 HDR 또는 다른 처리 알고리즘을 구현할 수 있습니다. 자세한 내용은 [가변 사진 시퀀스](variable-photo-sequence.md)를 참조하세요.



> [!NOTE] 
> Windows 10, 버전 1709부터는 비디오 녹화와 **AdvancedPhotoCapture** 동시 사용이 지원됩니다.  이 기능은 이전 버전에서 지원되지 않습니다. 이러한 변화 덕분에 **[LowLagMediaRecording](https://docs.microsoft.com/uwp/api/windows.media.capture.lowlagmediarecording)** 및 **[AdvancedPhotoCapture](https://docs.microsoft.com/uwp/api/windows.media.capture.advancedphotocapture)** 를 동시에 준비할 수 있습니다. **[MediaCapture.PrepareAdvancedPhotoCaptureAsync](https://docs.microsoft.com/uwp/api/windows.media.capture.mediacapture.prepareadvancedphotocaptureasync)** 호출과 **[AdvancedPhotoCapture.FinishAsync](https://docs.microsoft.com/uwp/api/windows.media.capture.advancedphotocapture.FinishAsync)** 호출 사이에 비디오 녹화를 시작하거나 중지할 수 있습니다. 또한 비디오를 녹화하는 동안 **[AdvancedPhotoCapture.CaptureAsync](https://docs.microsoft.com/uwp/api/windows.media.capture.advancedphotocapture.CaptureAsync)** 를 호출할 수도 있습니다. 그러나 비디오 녹화 중에 HDR 사진 캡처와 같은 일부 **AdvancedPhotoCapture** 시나리오를 사용하면 HDR 캡처로 인해 일부 비디오 프레임이 변경되어 부정적인 사용자 환경을 유발할 수 있습니다. 이러한 이유로 **[AdvancedPhotoControl.SupportedModes](https://docs.microsoft.com/uwp/api/windows.media.devices.advancedphotocontrol.SupportedModes)** 에서 반환한 모드 목록은 비디오가 녹화되는 동안 다릅니다. 비디오 녹화를 시작하거나 중지한 직후에 이 값을 확인하여 현재 비디오 녹화 상태에서 원하는 모드가 지원되고 있는지 확인해야 합니다.


> [!NOTE] 
> Windows 10, 버전 1709부터 **AdvancedPhotoCapture**가 HDR 모드로 설정되면 [**FlashControl.Enabled**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Devices.FlashControl.Enabled) 속성의 설정이 무시되고 플래시가 절대 시작되지 않습니다. 다른 캡처 모드에서 **FlashControl.Enabled**의 경우 **AdvancedPhotoCapture** 설정을 덮어쓰고 일반 사진이 플래시로 캡처됩니다. [**Auto**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Devices.FlashControl.Auto)가 true로 설정되어 있으면 현재 장면의 조건에 대한 카메라 드라이버의 기본 동작에 따라 **AdvancedPhotoCapture**이 플래시를 사용하거나 사용하지 않습니다. 이전 릴리스에서 **AdvancedPhotoCapture** 플래시 설정은 항상 **FlashControl.Enabled** 설정을 재정의합니다.

> [!NOTE] 
> 이 문서는 기본 사진 및 비디오 캡처 구현 단계를 설명하는 [MediaCapture를 사용한 기본적인 사진, 비디오 및 오디오 캡처](basic-photo-video-and-audio-capture-with-MediaCapture.md)에 설명된 개념 및 코드를 토대로 작성되었습니다. 보다 수준 높은 캡처 시나리오를 진행하기 전에 해당 문서의 기본적인 미디어 캡처 패턴을 파악하는 것이 좋습니다. 이 문서의 코드는 앱에 적절히 초기화된 MediaCapture의 인스턴스가 이미 있다고 가정합니다.

컨텍스트에서 또는 해당 앱의 시작점으로 사용된 API를 확인하는 데 사용할 수 있는 **AdvancedPhotoCapture** 클래스의 사용을 보여 주는 범용 Windows 샘플이 있습니다. 자세한 내용은 [카메라 고급 캡처 샘플](http://go.microsoft.com/fwlink/?LinkID=620517)을 참조하세요.

## <a name="advanced-photo-capture-namespaces"></a>고급 사진 캡처 네임스페이스

이 문서의 코드 예제에서는 기본적인 미디어 캡처에 필요한 네임스페이스 외에도 다음 네임스페이스의 API를 사용합니다.

[!code-cs[HDRPhotoUsing](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetHDRPhotoUsing)]

## <a name="hdr-photo-capture"></a>HDR 사진 캡처

### <a name="determine-if-hdr-photo-capture-is-supported-on-the-current-device"></a>현재 디바이스에서 HDR 사진 캡처가 지원되는지 확인

이 문서에 설명된 HDR 캡처 기술은 [**AdvancedPhotoCapture**](https://msdn.microsoft.com/library/windows/apps/mt181386) 개체를 사용하여 수행됩니다. 모든 장치가 **AdvancedPhotoCapture**를 통해 HDR 캡처를 지원하는 것은 아닙니다. **MediaCapture** 개체의 [**VideoDeviceController**](https://msdn.microsoft.com/library/windows/apps/br226825)를 가져온 다음 [**AdvancedPhotoControl**](https://msdn.microsoft.com/library/windows/apps/mt147840) 속성을 가져와 앱이 현재 실행 중인 디바이스가 이 기술을 지원하는지 확인합니다. 비디오 디바이스 컨트롤러의 [**SupportedModes**](https://msdn.microsoft.com/library/windows/apps/mt147844) 컬렉션을 확인하여 [**AdvancedPhotoMode.Hdr**](https://msdn.microsoft.com/library/windows/apps/mt147845)가 포함되어 있는지 확인합니다. 포함된 경우 **AdvancedPhotoCapture**를 사용한 HDR 캡처가 지원됩니다.

[!code-cs[HdrSupported](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetHdrSupported)]

### <a name="configure-and-prepare-the-advancedphotocapture-object"></a>AdvancedPhotoCapture 개체 구성 및 준비

코드 내의 여러 위치에서 [**AdvancedPhotoCapture**](https://msdn.microsoft.com/library/windows/apps/mt181386) 인스턴스에 액세스해야 하므로 개체를 저장할 멤버 변수를 선언해야 합니다.

[!code-cs[DeclareAdvancedCapture](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetDeclareAdvancedCapture)]

앱에서 **MediaCapture** 개체를 초기화한 후 [**AdvancedPhotoCaptureSettings**](https://msdn.microsoft.com/library/windows/apps/mt147837) 개체를 만들고 이 모드를 [**AdvancedPhotoMode.Hdr**](https://msdn.microsoft.com/library/windows/apps/mt147845)로 설정합니다. [**AdvancedPhotoControl**](https://msdn.microsoft.com/library/windows/apps/mt147840) 개체의 [**Configure**](https://msdn.microsoft.com/library/windows/apps/mt147841) 메서드에 호출하여 만든 **AdvancedPhotoCaptureSettings** 개체를 전달합니다.

**MediaCapture** 개체의 [**PrepareAdvancedPhotoCaptureAsync**](https://msdn.microsoft.com/library/windows/apps/mt181403)를 호출하고 캡처에 사용될 인코딩 유형을 지정하는 [**ImageEncodingProperties**](https://msdn.microsoft.com/library/windows/apps/hh700993) 개체를 전달합니다. **ImageEncodingProperties** 클래스는 **MediaCapture**에서 지원되는 이미지 인코딩을 만들기 위한 정적 메서드를 제공합니다.

**PrepareAdvancedPhotoCaptureAsync**는 사진 캡처를 시작하는 데 사용할 [**AdvancedPhotoCapture**](https://msdn.microsoft.com/library/windows/apps/mt181386) 개체를 반환합니다. 이 개체를 사용하여 이 문서의 뒷부분에서 설명되는 [**OptionalReferencePhotoCaptured**](https://msdn.microsoft.com/library/windows/apps/mt181392) 및 [**AllPhotosCaptured**](https://msdn.microsoft.com/library/windows/apps/mt181387)에 대한 처리기를 등록할 수 있습니다.

[!code-cs[CreateAdvancedCaptureAsync](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetCreateAdvancedCaptureAsync)]

### <a name="capture-an-hdr-photo"></a>HDR 사진 캡처

[**AdvancedPhotoCapture**](https://msdn.microsoft.com/library/windows/apps/mt181386) 개체의 [**CaptureAsync**](https://msdn.microsoft.com/library/windows/apps/mt181388) 메서드를 호출하여 HDR 사진을 캡처합니다. 이 메서드는 해당 [**Frame**](https://msdn.microsoft.com/library/windows/apps/mt181382) 속성에 캡처한 사진을 제공하는 [**AdvancedCapturedPhoto**](https://msdn.microsoft.com/library/windows/apps/mt181378) 개체를 반환합니다.

[!code-cs[CaptureHdrPhotoAsync](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetCaptureHdrPhotoAsync)]

대부분의 사진 앱은 다른 앱과 디바이스에서 제대로 표시될 수 있도록 캡처한 사진의 회전을 이미지 파일로 인코드하려고 합니다. 이 예제에서는 도우미 클래스 **CameraRotationHelper**를 사용하여 파일의 적절한 방향을 계산하는 방법을 보여 줍니다. 이 클래스는 [**MediaCapture를 사용하여 디바이스 방향 처리**](handle-device-orientation-with-mediacapture.md) 문서에서 자세히 설명하고 보여 줍니다.

이미지를 디스크에 저장하는 **SaveCapturedFrameAsync** 도우미 메서드는 이 문서의 뒷부분에서 설명합니다.

### <a name="get-optional-reference-frame"></a>선택적 참조 프레임 가져오기

HDR 프로세스는 여러 프레임을 캡처한 후, 모든 프레임 캡처가 완료된 다음 단일 이미지로 합성합니다. 캡처 후, 전체 HDR 프로세스를 완료하기 전에 [**OptionalReferencePhotoCaptured**](https://msdn.microsoft.com/library/windows/apps/mt181392) 이벤트를 처리하여 프레임에 대한 액세스 권한을 얻을 수 있습니다. 최종 HDR 사진 결과에만 관심이 있는 경우 이 작업을 수행할 필요가 없습니다.

> [!IMPORTANT]
> [**OptionalReferencePhotoCaptured**](https://msdn.microsoft.com/library/windows/apps/mt181392)는 HDR 하드웨어를 지원하는, 즉 참조 프레임을 생성하지 않는 장치에서는 발생하지 않습니다. 앱은 이 이벤트가 발생하지 않는 경우를 처리해야 합니다.

참조 프레임은 **CaptureAsync** 호출에 대한 컨텍스트 정보 없이 도착하므로 **OptionalReferencePhotoCaptured** 처리기에 컨텍스트 정보를 제공하기 위한 메커니즘이 제공됩니다. 먼저 컨텍스트 정보가 포함될 개체를 호출해야 합니다. 이 개체의 이름과 내용은 사용자가 결정합니다. 이 예제에서는 캡처의 파일 이름 및 카메라 방향을 추적하기 위한 멤버가 있는 개체를 정의합니다.

[!code-cs[AdvancedCaptureContext](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetAdvancedCaptureContext)]

컨텍스트 개체의 새 인스턴스를 만들고 해당 멤버를 채운 다음, 개체를 매개 변수로 수락하는 [**CaptureAsync**](https://msdn.microsoft.com/library/windows/apps/mt181388)의 오버로드에 전달합니다.

[!code-cs[CaptureWithContext](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetCaptureWithContext)]

[**OptionalReferencePhotoCaptured**](https://msdn.microsoft.com/library/windows/apps/mt181392) 이벤트 처리기에서 [**OptionalReferencePhotoCapturedEventArgs**](https://msdn.microsoft.com/library/windows/apps/mt181404) 개체의 [**Context**](https://msdn.microsoft.com/library/windows/apps/mt181405) 속성을 컨텍스트 개체 클래스로 캐스팅합니다. 이 예제에서는 최종 HDR 이미지에서 참조 프레임 이미지를 구분하기 위해 파일 이름을 수정하고 **SaveCapturedFrameAsync** 도우미 메서드를 호출하여 이미지를 저장합니다.

[!code-cs[OptionalReferencePhotoCaptured](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetOptionalReferencePhotoCaptured)]

### <a name="receive-a-notification-when-all-frames-have-been-captured"></a>모든 프레임이 캡처되었을 때 알림 받기

HDR 사진 캡처는 두 단계로 진행됩니다. 먼저, 여러 프레임이 캡처된 다음, 프레임이 최종 HDR 이미지로 처리됩니다. 원본 HDR 프레임이 여전히 캡처되는 동안에는 다른 캡처를 시작할 수 없지만 모든 프레임이 캡처되고, HDR 사후 처리가 완료되기 전에는 캡처를 시작할 수 있습니다. HDR 캡처가 완료되어 다른 캡처를 시작할 수 있음을 사용자에게 알리면 [**AllPhotosCaptured**](https://msdn.microsoft.com/library/windows/apps/mt181387) 이벤트가 발생합니다. 일반적인 시나리오는 HDR 캡처가 시작되면 UI의 캡처 단추를 사용할 수 없게 설정하고, **AllPhotosCaptured**가 발생할 때는 다시 사용하도록 설정하는 것입니다.

[!code-cs[AllPhotosCaptured](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetAllPhotosCaptured)]

### <a name="clean-up-the-advancedphotocapture-object"></a>AdvancedPhotoCapture 개체 정리

앱 캡처가 완료되고, **MediaCapture** 개체를 해제하기 전에, [**FinishAsync**](https://msdn.microsoft.com/library/windows/apps/mt181391)를 설정하고 멤버 변수를 null로 설정하여 [**AdvancedPhotoCapture**](https://msdn.microsoft.com/library/windows/apps/mt181386) 개체를 종료해야 합니다.

[!code-cs[CleanUpAdvancedPhotoCapture](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetCleanUpAdvancedPhotoCapture)]


## <a name="low-light-photo-capture"></a>낮은 조명 사진 캡처
Windows 10 버전 1607부터 **AdvancedPhotoCapture**를 사용하여 낮은 조명 설정에서 캡처된 사진의 품질을 향상하는 기본 제공 알고리즘으로 사진을 캡처할 수 있습니다. [**AdvancedPhotoCapture**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.AdvancedPhotoCapture) 클래스의 낮은 조명 기능을 사용하는 경우 시스템에서 현재 장면을 평가하고, 필요한 경우 낮은 조명 조건을 보정하는 알고리즘을 적용합니다. 시스템에서 알고리즘이 필요하지 않다고 결정하면 일반 캡처가 대신 수행됩니다.

낮은 조명 사진 캡처를 사용하기 전에 **MediaCapture** 개체의 [**VideoDeviceController**](https://msdn.microsoft.com/library/windows/apps/br226825)를 가져온 다음 [**AdvancedPhotoControl**](https://msdn.microsoft.com/library/windows/apps/mt147840) 속성을 가져와 현재 앱을 실행 중인 디바이스가 이 기술을 지원하는지 확인합니다. 비디오 디바이스 컨트롤러의 [**SupportedModes**](https://msdn.microsoft.com/library/windows/apps/mt147844) 컬렉션을 검사하여 [**AdvancedPhotoMode.LowLight**](https://msdn.microsoft.com/library/windows/apps/mt147845)가 포함되어 있는지 확인합니다. 포함되어 있으면 **AdvancedPhotoCapture**를 사용한 낮은 조명 캡처가 지원됩니다. 
[!code-cs[LowLightSupported1](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetLowLightSupported1)]

[!code-cs[LowLightSupported2](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetLowLightSupported2)]

**AdvancedPhotoCapture** 개체를 저장할 멤버 변수를 선언합니다. 

[!code-cs[DeclareAdvancedCapture](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetDeclareAdvancedCapture)]

**MediaCapture** 개체를 초기화한 후 앱에서 [**AdvancedPhotoCaptureSettings**](https://msdn.microsoft.com/library/windows/apps/mt147837) 개체를 만들고 해당 모드를 [**AdvancedPhotoMode.LowLight**](https://msdn.microsoft.com/library/windows/apps/mt147845)로 설정합니다. [**AdvancedPhotoControl**](https://msdn.microsoft.com/library/windows/apps/mt147840) 개체의 [**Configure**](https://msdn.microsoft.com/library/windows/apps/mt147841) 메서드를 호출하고 만든 **AdvancedPhotoCaptureSettings** 개체를 전달합니다.

**MediaCapture** 개체의 [**PrepareAdvancedPhotoCaptureAsync**](https://msdn.microsoft.com/library/windows/apps/mt181403)를 호출하고 캡처에 사용될 인코딩 유형을 지정하는 [**ImageEncodingProperties**](https://msdn.microsoft.com/library/windows/apps/hh700993) 개체를 전달합니다. 

[!code-cs[CreateAdvancedCaptureLowLightAsync](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetCreateAdvancedCaptureLowLightAsync)]

사진을 캡처하려면 [**CaptureAsync**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.AdvancedPhotoCapture.CaptureAsync)를 호출합니다.

[!code-cs[CaptureLowLight](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetCaptureLowLight)]

이 예제에서는 위의 HDR 예제와 마찬가지로 **CameraRotationHelper**라는 도우미 클래스를 사용하여 회전 값을 확인하며, 다른 앱과 디바이스에서 제대로 표시될 수 있도록 이 값을 이미지로 인코드해야 합니다. 이 클래스는 [**MediaCapture를 사용하여 디바이스 방향 처리**](handle-device-orientation-with-mediacapture.md) 문서에서 자세히 설명하고 보여 줍니다.

이미지를 디스크에 저장하는 **SaveCapturedFrameAsync** 도우미 메서드는 이 문서의 뒷부분에서 설명합니다.

**AdvancedPhotoCapture** 개체를 다시 구성하지 않고 여러 개의 낮은 조명 사진을 캡처할 수 있지만 캡처가 완료되면 [**FinishAsync**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.AdvancedPhotoCapture.FinishAsync)를 호출하여 개체 및 관련 리소스를 정리해야 합니다.

[!code-cs[CleanUpAdvancedPhotoCapture](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetCleanUpAdvancedPhotoCapture)]

## <a name="working-with-advancedcapturedphoto-objects"></a>AdvancedCapturedPhoto 개체 작업
[**AdvancedPhotoCapture.CaptureAsync**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.AdvancedPhotoCapture.CaptureAsync)는 캡처한 사진을 나타내는 [**AdvancedCapturedPhoto**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.AdvancedCapturedPhoto) 개체를 반환합니다. 이 개체는 이미지를 나타내는 [**CapturedFrame**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.CapturedFrame) 개체를 반환하는 [**Frame**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.AdvancedCapturedPhoto.Frame) 속성을 표시합니다. 또한 [**OptionalReferencePhotoCaptured**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.AdvancedPhotoCapture.OptionalReferencePhotoCaptured) 이벤트는 해당 이벤트 인수에 **CapturedFrame** 개체를 제공합니다. 이 유형의 개체를 가져온 후 [**SoftwareBitmap**](https://msdn.microsoft.com/library/windows/apps/Windows.Graphics.Imaging.SoftwareBitmap) 만들기, 파일에 이미지 저장 등 다양한 작업을 수행할 수 있습니다. 

## <a name="get-a-softwarebitmap-from-a-capturedframe"></a>CapturedFrame에서 SoftwareBitmap 가져오기
개체의 [**SoftwareBitmap**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.CapturedFrame.SoftwareBitmap) 속성에 액세스하기만 하면 **CapturedFrame** 개체에서 **SoftwareBitmap**을 쉽게 가져올 수 있습니다. 그러나 대부분의 인코딩 형식은 **AdvancedPhotoCapture**에서 **SoftwareBitmap**을 지원하지 않으므로 사용하기 전에 속성이 null이 아닌지 확인해야 합니다.

[!code-cs[SoftwareBitmapFromCapturedFrame](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetSoftwareBitmapFromCapturedFrame)]

현재 릴리스에서 **AdvancedPhotoCapture**에 대해 **SoftwareBitmap**을 지원하는 유일한 인코딩 형식은 압축되지 않은 NV12입니다. 따라서 이 기능을 사용하려면 [**PrepareAdvancedPhotoCaptureAsync**](https://msdn.microsoft.com/library/windows/apps/mt181403)를 호출할 때 해당 인코딩을 지정해야 합니다. 

[!code-cs[UncompressedNv12](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetUncompressedNv12)]

물론, 언제든지 이미지를 파일에 저장한 다음 별도 단계에서 파일을 **SoftwareBitmap**에 로드할 수 있습니다. **SoftwareBitmap** 작업 방법에 대한 자세한 내용은 [**비트맵 이미지 만들기, 편집 및 저장**](imaging.md)을 참조하세요.

## <a name="save-a-capturedframe-to-a-file"></a>파일에 CapturedFrame 저장
[**CapturedFrame**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.CapturedFrame) 클래스는 [**BitmapDecoder**](https://msdn.microsoft.com/library/windows/apps/Windows.Graphics.Imaging.BitmapDecoder)에 대한 입력으로 사용할 수 있도록 IInputStream 인터페이스를 구현하며, [**BitmapEncoder**](https://msdn.microsoft.com/library/windows/apps/Windows.Graphics.Imaging.BitmapEncoder)를 사용하여 이미지 데이터를 디스크에 쓸 수 있습니다.

다음 예제에서는 사용자의 사진 라이브러리에 새 폴더를 만들고 이 폴더 안에 파일을 만듭니다. 앱에서 이 디렉터리에 액세스하려면 앱 매니페스트 파일에 **사진 라이브러리** 기능을 포함해야 합니다. 그러면 지정한 파일에 대한 파일 스트림이 열립니다. [**BitmapDecoder.CreateAsync**](https://msdn.microsoft.com/library/windows/apps/Windows.Graphics.Imaging.BitmapDecoder.CreateAsync)를 호출하여 **CapturedFrame**에서 디코더를 만듭니다. [**CreateForTranscodingAsync**](https://msdn.microsoft.com/library/windows/apps/br226214)가 파일 스트림과 디코더에서 인코더를 만듭니다.

다음 단계에서는 인코더의 [**BitmapProperties**](https://msdn.microsoft.com/library/windows/apps/Windows.Graphics.Imaging.BitmapEncoder.BitmapProperties)를 사용하여 사진 방향을 이미지 파일로 인코드합니다. 이미지를 캡처할 때 방향을 처리하는 방법에 대한 자세한 내용은 [**MediaCapture를 사용하여 디바이스 방향 처리**](handle-device-orientation-with-mediacapture.md)를 참조하세요.

마지막으로, [**FlushAsync**](https://msdn.microsoft.com/library/windows/apps/Windows.Graphics.Imaging.BitmapEncoder.FlushAsync)를 호출하여 이미지를 파일에 씁니다.

[!code-cs[SaveCapturedFrameAsync](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetSaveCapturedFrameAsync)]

## <a name="related-topics"></a>관련 항목

* [카메라](camera.md)
* [MediaCapture를 사용하여 기본적인 사진, 비디오 및 오디오 캡처](basic-photo-video-and-audio-capture-with-MediaCapture.md)
