---
ms.assetid: 0186EA01-8446-45BA-A109-C5EB4B80F368
description: 이 문서에서는 AdvancedPhotoCapture 클래스를 사용 하 여 HDR (high dynamic range) 및 낮은 밝은 사진을 캡처하는 방법을 보여 줍니다.
title: HDR (High dynamic range) 및 낮은 밝은 사진 캡처
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 2072f1e7fad5c9652200fe067de8abe0afaede2a
ms.sourcegitcommit: c3ca68e87eb06971826087af59adb33e490ce7da
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 09/02/2020
ms.locfileid: "89362656"
---
# <a name="high-dynamic-range-hdr-and-low-light-photo-capture"></a>HDR (High dynamic range) 및 낮은 밝은 사진 캡처



이 문서에서는 [**AdvancedPhotoCapture**](/uwp/api/Windows.Media.Capture.AdvancedPhotoCapture) 클래스를 사용 하 여 HDR (high dynamic range) 사진을 캡처하는 방법을 보여 줍니다. 이 API를 사용 하면 최종 이미지 처리가 완료 되기 전에는 HDR 캡처에서 참조 프레임을 가져올 수도 있습니다.

HDR 캡처와 관련 된 다른 문서는 다음과 같습니다.

-   [**SceneAnalysisEffect**](/uwp/api/Windows.Media.Core.SceneAnalysisEffect) 를 사용 하면 시스템에서 미디어 캡처 미리 보기 스트림의 콘텐츠를 평가 하 여 HDR 처리로 인해 캡처 결과가 향상 되는지 확인할 수 있습니다. 자세한 내용은 [MediaCapture에 대 한 장면 분석](scene-analysis-for-media-capture.md)을 참조 하세요.

-   [**HdrVideoControl**](/uwp/api/Windows.Media.Devices.HdrVideoControl) 를 사용 하 여 Windows 기본 제공 HDR 처리 알고리즘을 통해 비디오를 캡처합니다. 자세한 내용은 [비디오 캡처를 위한 장치 컨트롤 캡처](capture-device-controls-for-video-capture.md)를 참조 하세요.

-   [**VariablePhotoSequenceCapture**](/uwp/api/Windows.Media.Capture.Core.VariablePhotoSequenceCapture) 를 사용 하 여 각각 서로 다른 캡처 설정이 있는 일련의 사진을 캡처하고 사용자 고유의 HDR 또는 다른 처리 알고리즘을 구현할 수 있습니다. 자세한 내용은 [변수 사진 시퀀스](variable-photo-sequence.md)를 참조 하세요.



> [!NOTE] 
> Windows 10, 버전 1709부터 비디오 기록 및 **AdvancedPhotoCapture** 동시 사용이 지원 됩니다.  이전 버전에서는 지원 되지 않습니다. 이 변경 내용은 준비 된 **[Lowlagmediarecording](/uwp/api/windows.media.capture.lowlagmediarecording)** **[AdvancedPhotoCapture](/uwp/api/windows.media.capture.advancedphotocapture)** 을 동시에 사용할 수 있음을 의미 합니다. **[MediaCapture](/uwp/api/windows.media.capture.mediacapture.prepareadvancedphotocaptureasync)** 및 **[AdvancedPhotoCapture async](/uwp/api/windows.media.capture.advancedphotocapture.FinishAsync)** 호출 사이에 비디오 기록을 시작 하거나 중지할 수 있습니다. 비디오를 기록 하는 동안 **[AdvancedPhotoCapture. CaptureAsync](/uwp/api/windows.media.capture.advancedphotocapture.CaptureAsync)** 를 호출할 수도 있습니다. 그러나 비디오를 기록 하는 동안 HDR 사진을 캡처하는 것과 같은 일부 **AdvancedPhotoCapture** 시나리오에서는 일부 비디오 프레임을 hdr 캡처로 변경 하 여 부정적인 사용자 환경을 생성 합니다. 이러한 이유로, 비디오를 기록 하는 동안 **[AdvancedPhotoControl 모드](/uwp/api/windows.media.devices.advancedphotocontrol.SupportedModes)** 에서 반환 되는 모드 목록이 다릅니다. 현재 비디오 녹화 상태에서 원하는 모드가 지원 되는지 확인 하려면 비디오 녹화를 시작 하거나 중지 한 후 즉시이 값을 확인 해야 합니다.


> [!NOTE] 
> Windows 10, 버전 1709부터 **AdvancedPhotoCapture** 이 HDR 모드로 설정 된 경우 [**FlashControl**](/uwp/api/windows.media.devices.flashcontrol.enabled) 속성의 설정이 무시 되 고 플래시는 실행 되지 않습니다. 다른 캡처 모드의 경우 FlashControl이 **사용 하도록**설정 되 면 **AdvancedPhotoCapture** 설정을 재정의 하 고 flash로 일반적인 사진을 캡처합니다. [**Auto**](/uwp/api/windows.media.devices.flashcontrol.auto) 를 true로 설정 하면 현재 장면에서 조건에 대 한 카메라 드라이버의 기본 동작에 따라 **AdvancedPhotoCapture** 가 flash를 사용할 수도 있고 사용 하지 않을 수도 있습니다. 이전 릴리스에서는 **AdvancedPhotoCapture** flash 설정이 항상 **FlashControl** 설정을 재정의 합니다.

> [!NOTE] 
> 이 문서는 기본 사진 및 비디오 캡처를 구현 하는 단계를 설명 하는 [MediaCapture을 사용 하 여 기본 사진, 비디오 및 오디오 캡처](basic-photo-video-and-audio-capture-with-MediaCapture.md)에 설명 된 개념 및 코드를 기반으로 합니다. 고급 캡처 시나리오로 전환 하기 전에 해당 문서의 기본적인 미디어 캡처 패턴을 숙지 하는 것이 좋습니다. 이 문서의 코드는 앱에 이미 올바르게 초기화 된 MediaCapture 인스턴스가 있다고 가정 합니다.

컨텍스트에서 사용 되는 API를 확인 하거나 사용자 고유의 앱에 대 한 시작 점으로 사용할 수 있는 **AdvancedPhotoCapture** 클래스의 사용을 보여 주는 유니버설 Windows 샘플이 있습니다. 자세한 내용은 [카메라 고급 캡처 샘플](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/CameraAdvancedCapture)을 참조 하세요.

## <a name="advanced-photo-capture-namespaces"></a>고급 사진 캡처 네임 스페이스

이 문서의 코드 예제에서는 기본 미디어 캡처에 필요한 네임 스페이스 외에도 다음 네임 스페이스의 Api를 사용 합니다.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/BasicMediaCaptureWin10/cs/MainPage.xaml.cs" id="SnippetHDRPhotoUsing":::

## <a name="hdr-photo-capture"></a>HDR 사진 캡처

### <a name="determine-if-hdr-photo-capture-is-supported-on-the-current-device"></a>현재 장치에서 HDR 사진 캡처가 지원 되는지 확인

이 문서에서 설명 하는 HDR 캡처 기술은 [**AdvancedPhotoCapture**](/uwp/api/Windows.Media.Capture.AdvancedPhotoCapture) 개체를 사용 하 여 수행 됩니다. 모든 장치에서 **AdvancedPhotoCapture**를 사용 하는 HDR 캡처를 지 원하는 것은 아닙니다. 앱이 현재 실행 되 고 있는 장치가 **MediaCapture** 개체의 [**VideoDeviceController**](/uwp/api/Windows.Media.Devices.VideoDeviceController) 을 가져온 다음 [**AdvancedPhotoControl**](/uwp/api/Windows.Media.Devices.AdvancedPhotoControl) 속성을 가져와 기술을 지원 하는지 확인 합니다. 비디오 장치 컨트롤러의 [**supportedmodes**](/uwp/api/windows.media.devices.advancedphotocontrol.supportedmodes) 컬렉션을 확인 하 여 [**AdvancedPhotoMode**](/uwp/api/Windows.Media.Devices.AdvancedPhotoMode)이 포함 되어 있는지 확인 합니다. 이 경우 **AdvancedPhotoCapture** 를 사용 하는 HDR 캡처가 지원 됩니다.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/BasicMediaCaptureWin10/cs/MainPage.xaml.cs" id="SnippetHdrSupported":::

### <a name="configure-and-prepare-the-advancedphotocapture-object"></a>AdvancedPhotoCapture 개체 구성 및 준비

코드 내의 여러 위치에서 [**AdvancedPhotoCapture**](/uwp/api/Windows.Media.Capture.AdvancedPhotoCapture) 인스턴스에 액세스 해야 하므로 개체를 보유 하는 멤버 변수를 선언 해야 합니다.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/BasicMediaCaptureWin10/cs/MainPage.xaml.cs" id="SnippetDeclareAdvancedCapture":::

앱에서 **MediaCapture** 개체를 초기화 한 후 [**AdvancedPhotoCaptureSettings**](/uwp/api/Windows.Media.Devices.AdvancedPhotoCaptureSettings) 개체를 만들고 모드를 [**AdvancedPhotoMode**](/uwp/api/Windows.Media.Devices.AdvancedPhotoMode)로 설정 합니다. [**AdvancedPhotoControl**](/uwp/api/Windows.Media.Devices.AdvancedPhotoControl) 개체의 [**Configure**](/uwp/api/windows.media.devices.advancedphotocontrol.configure) 메서드를 호출 하 여 사용자가 만든 **AdvancedPhotoCaptureSettings** 개체를 전달 합니다.

**MediaCapture** 개체의 [**PrepareAdvancedPhotoCaptureAsync**](/uwp/api/windows.media.capture.mediacapture.prepareadvancedphotocaptureasync)를 호출 하 여 캡처를 사용 해야 하는 인코딩 형식을 지정 하는 [**ImageEncodingProperties**](/uwp/api/Windows.Media.MediaProperties.ImageEncodingProperties) 개체를 전달 합니다. **ImageEncodingProperties** 클래스는 **MediaCapture**에서 지 원하는 이미지 인코딩을 만드는 정적 메서드를 제공 합니다.

**PrepareAdvancedPhotoCaptureAsync** 는 사진 캡처를 시작 하는 데 사용 하는 [**AdvancedPhotoCapture**](/uwp/api/Windows.Media.Capture.AdvancedPhotoCapture) 개체를 반환 합니다. 이 개체를 사용 하 여이 문서의 뒷부분에서 설명 하는 [**OptionalReferencePhotoCaptured**](/uwp/api/windows.media.capture.advancedphotocapture.optionalreferencephotocaptured) 및 [**AllPhotosCaptured**](/uwp/api/windows.media.capture.advancedphotocapture.allphotoscaptured) 에 대 한 처리기를 등록할 수 있습니다.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/BasicMediaCaptureWin10/cs/MainPage.xaml.cs" id="SnippetCreateAdvancedCaptureAsync":::

### <a name="capture-an-hdr-photo"></a>HDR 사진 캡처

[**AdvancedPhotoCapture**](/uwp/api/Windows.Media.Capture.AdvancedPhotoCapture) 개체의 [**CaptureAsync**](/uwp/api/windows.media.capture.advancedphotocapture.captureasync) 메서드를 호출 하 여 HDR 사진을 캡처합니다. 이 메서드는 [**프레임**](/uwp/api/windows.media.capture.advancedcapturedphoto.frame) 속성에서 캡처된 사진을 제공 하는 [**AdvancedCapturedPhoto**](/uwp/api/Windows.Media.Capture.AdvancedCapturedPhoto) 개체를 반환 합니다.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/BasicMediaCaptureWin10/cs/MainPage.xaml.cs" id="SnippetCaptureHdrPhotoAsync":::

대부분의 사진 앱은 캡처된 사진의 회전을 이미지 파일로 인코딩하여 다른 앱 및 장치에서 올바르게 표시 되도록 할 수 있습니다. 이 예제에서는 도우미 클래스 **CameraRotationHelper** 를 사용 하 여 파일에 대 한 적절 한 방향을 계산 하는 방법을 보여 줍니다. 이 클래스는 [**MediaCapture를 사용 하 여 장치 방향 처리**](handle-device-orientation-with-mediacapture.md)문서에 설명 되어 있으며 전체에 나열 되어 있습니다.

이미지를 디스크에 저장 하는 **SaveCapturedFrameAsync** 도우미 메서드는이 문서의 뒷부분에 설명 되어 있습니다.

### <a name="get-optional-reference-frame"></a>선택적 참조 프레임 가져오기

모든 프레임을 캡처한 후에는 HDR 프로세스가 여러 프레임을 캡처한 다음 단일 이미지로 합성 합니다. 프레임을 캡처한 후 [**OptionalReferencePhotoCaptured**](/uwp/api/windows.media.capture.advancedphotocapture.optionalreferencephotocaptured) 이벤트를 처리 하 여 전체 HDR 프로세스가 완료 되기 전에 프레임에 대 한 액세스 권한을 얻을 수 있습니다. 최종 HDR 사진 결과에만 관심이 있는 경우에는이 작업을 수행할 필요가 없습니다.

> [!IMPORTANT]
> [**OptionalReferencePhotoCaptured**](/uwp/api/windows.media.capture.advancedphotocapture.optionalreferencephotocaptured) 는 하드웨어 HDR를 지 원하는 장치에서 발생 하지 않으므로 참조 프레임을 생성 하지 않습니다. 앱에서이 이벤트가 발생 하지 않는 경우를 처리 해야 합니다.

참조 프레임은 **CaptureAsync**에 대 한 호출의 컨텍스트에서 수신 되므로 컨텍스트 정보를 **OptionalReferencePhotoCaptured** 처리기에 전달 하기 위한 메커니즘이 제공 됩니다. 먼저 컨텍스트 정보를 포함 하는 개체를 호출 해야 합니다. 이 개체의 이름과 내용은 사용자에 게 있습니다. 이 예제에서는 캡처의 파일 이름 및 카메라 방향을 추적 하는 멤버를 포함 하는 개체를 정의 합니다.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/BasicMediaCaptureWin10/cs/MainPage.xaml.cs" id="SnippetAdvancedCaptureContext":::

Context 개체의 새 인스턴스를 만들고 해당 멤버를 채운 다음 개체를 매개 변수로 허용 하는 [**CaptureAsync**](/uwp/api/windows.media.capture.advancedphotocapture.captureasync) 의 오버 로드에 전달 합니다.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/BasicMediaCaptureWin10/cs/MainPage.xaml.cs" id="SnippetCaptureWithContext":::

[**OptionalReferencePhotoCaptured**](/uwp/api/windows.media.capture.advancedphotocapture.optionalreferencephotocaptured) 이벤트 처리기에서 [**OptionalReferencePhotoCapturedEventArgs**](/uwp/api/Windows.Media.Capture.OptionalReferencePhotoCapturedEventArgs) 개체의 [**컨텍스트**](/uwp/api/windows.media.capture.optionalreferencephotocapturedeventargs.context) 속성을 컨텍스트 개체 클래스로 캐스팅 합니다. 이 예제에서는 파일 이름을 수정 하 여 최종 HDR 이미지와 참조 프레임 이미지를 구분 하 고 **SaveCapturedFrameAsync** helper 메서드를 호출 하 여 이미지를 저장 합니다.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/BasicMediaCaptureWin10/cs/MainPage.xaml.cs" id="SnippetOptionalReferencePhotoCaptured":::

### <a name="receive-a-notification-when-all-frames-have-been-captured"></a>모든 프레임이 캡처된 경우 알림 받기

HDR 사진 캡처에는 두 단계가 있습니다. 먼저 여러 프레임이 캡처되고 프레임은 최종 HDR 이미지로 처리 됩니다. 원본 HDR 프레임을 계속 캡처하는 동안에는 다른 캡처를 시작할 수 없지만, 모든 프레임을 캡처한 후에는 HDR 사후 처리가 완료 되기 전에 캡처를 시작할 수 있습니다. [**AllPhotosCaptured**](/uwp/api/windows.media.capture.advancedphotocapture.allphotoscaptured) 이벤트는 HDR 캡처가 완료 될 때 발생 하며,이를 통해 다른 캡처를 시작할 수 있다는 것을 알 수 있습니다. 일반적인 시나리오는 HDR 캡처가 시작 될 때 UI의 캡처 단추를 사용 하지 않도록 설정 하 고 **AllPhotosCaptured** 이 발생할 때 다시 사용 하도록 설정 하는 것입니다.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/BasicMediaCaptureWin10/cs/MainPage.xaml.cs" id="SnippetAllPhotosCaptured":::

### <a name="clean-up-the-advancedphotocapture-object"></a>AdvancedPhotoCapture 개체 정리

앱이 캡처를 완료 한 후 **MediaCapture** 개체를 삭제 하기 전에, [**AdvancedPhotoCapture**](/uwp/api/Windows.Media.Capture.AdvancedPhotoCapture) 개체를 종료 [**하 고 멤버**](/uwp/api/windows.media.capture.advancedphotocapture.finishasync) 변수를 null로 설정 하 여 해당 개체를 종료 해야 합니다.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/BasicMediaCaptureWin10/cs/MainPage.xaml.cs" id="SnippetCleanUpAdvancedPhotoCapture":::


## <a name="low-light-photo-capture"></a>낮은 밝은 사진 캡처
Windows 10, 버전 1607부터 **AdvancedPhotoCapture** 은 낮은 조명 설정에서 캡처된 사진의 품질을 향상 시키는 기본 제공 알고리즘을 사용 하 여 사진을 캡처하는 데 사용할 수 있습니다. [**AdvancedPhotoCapture**](/uwp/api/Windows.Media.Capture.AdvancedPhotoCapture) 클래스의 낮은 밝은 기능을 사용 하는 경우 시스템은 현재 장면을 평가 하 고 필요한 경우 낮은 조명 조건에 맞게 보정 하는 알고리즘을 적용 합니다. 시스템에서 알고리즘이 필요 하지 않은 것으로 확인 되는 경우 대신 일반 캡처가 수행 됩니다.

낮은 밝은 사진 캡처를 사용 하기 전에 **MediaCapture** 개체의 [**VideoDeviceController**](/uwp/api/Windows.Media.Devices.VideoDeviceController) 를 가져온 다음 [**AdvancedPhotoControl**](/uwp/api/Windows.Media.Devices.AdvancedPhotoControl) 속성을 가져와 앱이 현재 실행 중인 장치에서 기술을 지원 하는지 확인 합니다. 비디오 장치 컨트롤러의 [**supportedmodes**](/uwp/api/windows.media.devices.advancedphotocontrol.supportedmodes) 컬렉션을 확인 하 여 [**AdvancedPhotoMode**](/uwp/api/Windows.Media.Devices.AdvancedPhotoMode)이 포함 되어 있는지 확인 합니다. 이 경우 **AdvancedPhotoCapture** 를 사용 하는 낮은 빛 캡처가 지원 됩니다. 
:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/BasicMediaCaptureWin10/cs/MainPage.xaml.cs" id="SnippetLowLightSupported1":::

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/BasicMediaCaptureWin10/cs/MainPage.xaml.cs" id="SnippetLowLightSupported2":::

그런 다음 **AdvancedPhotoCapture** 개체를 저장 하는 멤버 변수를 선언 합니다. 

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/BasicMediaCaptureWin10/cs/MainPage.xaml.cs" id="SnippetDeclareAdvancedCapture":::

앱에서 **MediaCapture** 개체를 초기화 한 후 [**AdvancedPhotoCaptureSettings**](/uwp/api/Windows.Media.Devices.AdvancedPhotoCaptureSettings) 개체를 만들고 모드를 [**AdvancedPhotoMode**](/uwp/api/Windows.Media.Devices.AdvancedPhotoMode)로 설정 합니다. [**AdvancedPhotoControl**](/uwp/api/Windows.Media.Devices.AdvancedPhotoControl) 개체의 [**Configure**](/uwp/api/windows.media.devices.advancedphotocontrol.configure) 메서드를 호출 하 여 사용자가 만든 **AdvancedPhotoCaptureSettings** 개체를 전달 합니다.

**MediaCapture** 개체의 [**PrepareAdvancedPhotoCaptureAsync**](/uwp/api/windows.media.capture.mediacapture.prepareadvancedphotocaptureasync)를 호출 하 여 캡처를 사용 해야 하는 인코딩 형식을 지정 하는 [**ImageEncodingProperties**](/uwp/api/Windows.Media.MediaProperties.ImageEncodingProperties) 개체를 전달 합니다. 

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/BasicMediaCaptureWin10/cs/MainPage.xaml.cs" id="SnippetCreateAdvancedCaptureLowLightAsync":::

사진을 캡처하려면 [**CaptureAsync**](/uwp/api/windows.media.capture.advancedphotocapture.captureasync)를 호출 합니다.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/BasicMediaCaptureWin10/cs/MainPage.xaml.cs" id="SnippetCaptureLowLight":::

이 예제에서는 위의 HDR 예제와 마찬가지로 **CameraRotationHelper**라는 도우미 클래스를 사용하여 회전 값을 확인하며, 다른 앱과 디바이스에서 제대로 표시될 수 있도록 이 값을 이미지로 인코드해야 합니다. 이 클래스는 [**MediaCapture를 사용 하 여 장치 방향 처리**](handle-device-orientation-with-mediacapture.md)문서에 설명 되어 있으며 전체에 나열 되어 있습니다.

이미지를 디스크에 저장 하는 **SaveCapturedFrameAsync** 도우미 메서드는이 문서의 뒷부분에 설명 되어 있습니다.

**AdvancedPhotoCapture** 개체를 다시 구성 하지 않고 적은 수의 작은 사진을 캡처할 수 있지만 캡처가 완료 되 면, 개체 및 연결 된 리소스를 정리 하기 위해 다음 [**비동기**](/uwp/api/windows.media.capture.advancedphotocapture.finishasync) 를 호출 해야 합니다.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/BasicMediaCaptureWin10/cs/MainPage.xaml.cs" id="SnippetCleanUpAdvancedPhotoCapture":::

## <a name="working-with-advancedcapturedphoto-objects"></a>AdvancedCapturedPhoto 개체 작업
[**AdvancedPhotoCapture CaptureAsync**](/uwp/api/windows.media.capture.advancedphotocapture.captureasync) 는 캡처된 사진을 나타내는 [**AdvancedCapturedPhoto**](/uwp/api/Windows.Media.Capture.AdvancedCapturedPhoto) 개체를 반환 합니다. 이 개체는 이미지를 나타내는 [**CapturedFrame**](/uwp/api/Windows.Media.Capture.CapturedFrame) 개체를 반환 하는 [**Frame**](/uwp/api/windows.media.capture.advancedcapturedphoto.frame) 속성을 노출 합니다. 또한 [**OptionalReferencePhotoCaptured**](/uwp/api/windows.media.capture.advancedphotocapture.optionalreferencephotocaptured) 이벤트는 해당 이벤트 인수에 **CapturedFrame** 개체를 제공 합니다. 이 형식의 개체를 가져온 후 [**에는 해당**](/uwp/api/Windows.Graphics.Imaging.SoftwareBitmap) 개체를 사용 하 여 다양 한 작업을 수행할 수 있습니다. 

## <a name="get-a-softwarebitmap-from-a-capturedframe"></a>CapturedFrame에서에서의 비트맵 가져오기
단순히 개체의 [**\bitmap**](/uwp/api/windows.media.capture.capturedframe.softwarebitmap) 속성에 액세스 하 여 **CapturedFrame** 개체에서 지 속성 **비트맵** 을 가져오는 것이 간단 합니다. 그러나 대부분의 인코딩 형식은 **AdvancedPhotoCapture**에서 **\bitmap** 을 지원 하지 않으므로이 속성을 사용 하기 전에 해당 속성이 null이 아닌지 확인 해야 합니다.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/BasicMediaCaptureWin10/cs/MainPage.xaml.cs" id="SnippetSoftwareBitmapFromCapturedFrame":::

현재 릴리스에서는 **AdvancedPhotoCapture** 에 대 한 **\bitmap 비트맵** 을 지 원하는 인코딩 형식만 압축 NV12 됩니다. 따라서이 기능을 사용 하려면 [**PrepareAdvancedPhotoCaptureAsync**](/uwp/api/windows.media.capture.mediacapture.prepareadvancedphotocaptureasync)를 호출할 때 해당 인코딩을 지정 해야 합니다. 

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/BasicMediaCaptureWin10/cs/MainPage.xaml.cs" id="SnippetUncompressedNv12":::

물론, 언제 든 지 이미지를 파일에 저장 한 다음 **별도의 단계로** 파일을 파일에 로드할 수 있습니다. **\Bitmap**으로 작업 하는 방법에 대 한 자세한 내용은 [**비트맵 이미지 만들기, 편집 및 저장**](imaging.md)을 참조 하세요.

## <a name="save-a-capturedframe-to-a-file"></a>파일에 CapturedFrame 저장
[**CapturedFrame**](/uwp/api/Windows.Media.Capture.CapturedFrame) 클래스는 IInputStream 인터페이스를 구현 하므로 [**bitmapdecoder에서**](/uwp/api/Windows.Graphics.Imaging.BitmapDecoder)에 대 한 입력으로 사용할 수 있습니다. 그런 다음 [**BitmapEncoder**](/uwp/api/Windows.Graphics.Imaging.BitmapEncoder) 를 사용 하 여 이미지 데이터를 디스크에 쓸 수 있습니다.

다음 예제에서는 사용자의 그림 라이브러리에서 새 폴더를 만들고이 폴더 내에 파일을 만듭니다. 앱은이 디렉터리에 액세스 하기 위해 앱 매니페스트 파일에 **그림 라이브러리** 기능을 포함 해야 합니다. 그러면 지정 된 파일에 대 한 파일 스트림이 열립니다. 그런 다음 [**bitmapdecoder에서**](/uwp/api/windows.graphics.imaging.bitmapdecoder.createasync) 를 호출 하 여 **CapturedFrame**에서 디코더를 만듭니다. 그런 다음 [**CreateForTranscodingAsync**](/uwp/api/windows.graphics.imaging.bitmapencoder.createfortranscodingasync) 는 파일 스트림과 디코더에 인코더를 만듭니다.

다음 단계에서는 인코더의 [**BitmapProperties**](/uwp/api/windows.graphics.imaging.bitmapencoder.bitmapproperties) 를 사용 하 여 사진의 방향을 이미지 파일로 인코딩합니다. 이미지를 캡처할 때의 처리 방향에 대 한 자세한 내용은 [**MediaCapture를 사용 하 여 장치 방향 처리**](handle-device-orientation-with-mediacapture.md)를 참조 하세요.

마지막으로 [**FlushAsync**](/uwp/api/windows.graphics.imaging.bitmapencoder.flushasync)를 호출 하 여 이미지를 파일에 씁니다.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/BasicMediaCaptureWin10/cs/MainPage.xaml.cs" id="SnippetSaveCapturedFrameAsync":::

## <a name="related-topics"></a>관련 항목

* [카메라](camera.md)
* [MediaCapture를 사용하여 기본적인 사진, 비디오 및 오디오 캡처](basic-photo-video-and-audio-capture-with-MediaCapture.md)
