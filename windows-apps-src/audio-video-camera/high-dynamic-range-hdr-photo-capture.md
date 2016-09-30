---
author: drewbatgit
ms.assetid: 0186EA01-8446-45BA-A109-C5EB4B80F368
description: "AdvancedPhotoCapture 클래스를 사용하여 HDR(High Dynamic Range) 사진을 캡처할 수 있습니다."
title: "HDR(High Dynamic Range) 사진 캡처"
translationtype: Human Translation
ms.sourcegitcommit: 6530fa257ea3735453a97eb5d916524e750e62fc
ms.openlocfilehash: 3015aa4338ddb0c0a006eb631026261a4453f376

---

# HDR(High Dynamic Range) 사진 캡처

\[ Windows 10의 UWP 앱에 맞게 업데이트되었습니다. Windows 8.x 문서는 [보관](http://go.microsoft.com/fwlink/p/?linkid=619132)을 참조하세요. \]


[**AdvancedPhotoCapture**](https://msdn.microsoft.com/library/windows/apps/mt181386) 클래스를 사용하여 HDR(High Dynamic Range) 사진을 캡처할 수 있습니다. 이 API를 사용하면 최종 이미지의 처리를 완료하기 전에 HDR 캡처에서 참조 프레임을 얻을 수 있습니다.

HDR 캡처와 관련된 기타 문서는 다음과 같습니다.

-   [**SceneAnalysisEffect**](https://msdn.microsoft.com/library/windows/apps/dn948902)를 사용할 경우 시스템은 미디어 캡처 미리 보기 스트림의 콘텐츠를 평가하여 HDR 처리로 인해 캡처 결과가 향상될 수 있는지 파악할 수 있습니다. 자세한 내용은 [미디어 캡처의 장면 분석](scene-analysis-for-media-capture.md)을 참조하세요.

-   [**HdrVideoControl**](https://msdn.microsoft.com/library/windows/apps/dn926680)을 사용하여 Windows 기본 제공 HDR 처리 알고리즘을 사용해 동영상을 캡처합니다. 자세한 내용은 [비디오 캡처를 위한 캡처 디바이스 컨트롤](capture-device-controls-for-video-capture.md)을 참조하세요.

-   [**VariablePhotoSequenceCapture**](https://msdn.microsoft.com/library/windows/apps/dn652564)를 사용하여 각각이 다른 캡처 설정을 갖는 사진 시퀀스를 캡처하고 고유한 HDR 또는 다른 처리 알고리즘을 구현할 수 있습니다. 자세한 내용은 [가변 사진 시퀀스](variable-photo-sequence.md)를 참조하세요.

**참고**
-   **AdvancedPhotoCapture**를 사용하는 동영상과 사진 캡처는 동시에 지원되지 않습니다.

-   고급 사진 캡처 중에는 카메라 플래시 사용이 지원되지 않습니다.

**참고** 이 문서는 기본 사진 및 비디오 캡처 구현 단계를 설명하는 [MediaCapture를 사용하여 사진 및 비디오 캡처](capture-photos-and-video-with-mediacapture.md)에 설명된 개념 및 코드를 토대로 작성되었습니다. 좀 더 수준 높은 캡처 시나리오를 진행하기 전에 해당 문서의 기본 미디어 캡처 패턴을 좀 더 잘 이해하는 것이 좋습니다. 이 문서의 코드는 앱에 적절히 초기화된 MediaCapture의 인스턴스가 이미 있다고 가정합니다.

## HDR 사진 캡처 네임스페이스

HDR 사진 캡처를 사용하려면 앱에 기본적인 미디어 캡처에 필요한 네임스페이스 외에도 다음 네임스페이스가 있어야 합니다.

[!code-cs[HDRPhotoUsing](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetHDRPhotoUsing)]


## 현재 디바이스에서 HDR 사진 캡처가 지원되는지 확인

이 문서에 설명된 HDR 캡처 기술은 [**AdvancedPhotoCapture**](https://msdn.microsoft.com/library/windows/apps/mt181386) 개체를 사용하여 수행됩니다. 모든 장치가 **AdvancedPhotoCapture**를 통해 HDR 캡처를 지원하는 것은 아닙니다. **MediaCapture** 개체의 [**VideoDeviceController**](https://msdn.microsoft.com/library/windows/apps/br226825)를 가져온 다음 [**AdvancedPhotoControl**](https://msdn.microsoft.com/library/windows/apps/mt147840) 속성을 가져와 앱이 현재 실행 중인 디바이스가 이 기술을 지원하는지 확인합니다. 비디오 장치 컨트롤러의 [**SupportedModes**](https://msdn.microsoft.com/library/windows/apps/mt147844) 컬렉션을 확인하여 [**AdvancedPhotoMode.Hdr**](https://msdn.microsoft.com/library/windows/apps/mt147845)이 포함되어 있는지 검토합니다. 포함되어 있으면 **AdvancedPhotoCapture**를 사용하는 HDR 캡처가 지원됩니다.

[!code-cs[HdrSupported](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetHdrSupported)]

## AdvancedPhotoCapture 개체 구성 및 준비

코드 내의 여러 위치에서 [**AdvancedPhotoCapture**](https://msdn.microsoft.com/library/windows/apps/mt181386) 인스턴스에 액세스해야 하므로 개체를 보유하도록 멤버 변수를 선언해야 합니다.

[!code-cs[DeclareAdvancedCapture](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetDeclareAdvancedCapture)]

앱에서 **MediaCapture** 개체를 초기화한 후에 새 [**AdvancedPhotoCaptureSettings**](https://msdn.microsoft.com/library/windows/apps/mt147837) 개체를 만들고 해당 모드를 [**AdvancedPhotoMode.Hdr**](https://msdn.microsoft.com/library/windows/apps/mt147845)로 설정합니다. [**AdvancedPhotoControl**](https://msdn.microsoft.com/library/windows/apps/mt147840) 개체의 [**Configure**](https://msdn.microsoft.com/library/windows/apps/mt147841) 메서드를 호출하고 만든 **AdvancedPhotoCaptureSettings** 개체를 전달합니다.

**MediaCapture** 개체의 [**PrepareAdvancedPhotoCaptureAsync**](https://msdn.microsoft.com/library/windows/apps/mt181403)를 호출하고 캡처에 사용될 인코딩 유형을 지정하는 [**ImageEncodingProperties**](https://msdn.microsoft.com/library/windows/apps/hh700993) 개체를 전달합니다. **ImageEncodingProperties** 클래스는 **MediaCapture**에서 지원되는 이미지 인코딩을 만들기 위한 정적 메서드를 제공합니다.

**PrepareAdvancedPhotoCaptureAsync**는 사진 캡처를 시작하는 데 사용할 [**AdvancedPhotoCapture**](https://msdn.microsoft.com/library/windows/apps/mt181386) 개체를 반환합니다. 이 개체를 사용하여 이 문서의 뒷부분에서 설명되는 [**OptionalReferencePhotoCaptured**](https://msdn.microsoft.com/library/windows/apps/mt181392) 및 [**AllPhotosCaptured**](https://msdn.microsoft.com/library/windows/apps/mt181387)에 대한 처리기를 등록할 수 있습니다.

[!code-cs[CreateAdvancedCaptureAsync](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetCreateAdvancedCaptureAsync)]

## HDR 사진 캡처

[**AdvancedPhotoCapture**](https://msdn.microsoft.com/library/windows/apps/mt181386) 개체의[**CaptureAsync**](https://msdn.microsoft.com/library/windows/apps/mt181388) 메서드를 호출하여 HDR 사진을 캡처합니다. 이 메서드는 해당 [**Frame**](https://msdn.microsoft.com/library/windows/apps/mt181382) 속성에 캡처한 사진을 제공하는 [**AdvancedCapturedPhoto**](https://msdn.microsoft.com/library/windows/apps/mt181378) 개체를 반환합니다.

[!code-cs[CaptureHdrPhotoAsync](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetCaptureHdrPhotoAsync)]

**ConvertOrientationToPhotoOrientation** 및 **ReencodeAndSavePhotoAsync**는 [MediaCapture를 사용하여 사진 및 비디오 캡처](capture-photos-and-video-with-mediacapture.md) 문서에서 기본 미디어 캡처 시나리오의 일부로 설명되는 도우미 메서드입니다.

## 선택적 참조 프레임 가져오기

HDR 프로세스는 여러 프레임을 캡처한 후, 모든 프레임 캡처가 완료된 다음 단일 이미지로 합성합니다. 캡처 후, 전체 HDR 프로세스를 완료하기 전에 [**OptionalReferencePhotoCaptured**](https://msdn.microsoft.com/library/windows/apps/mt181392) 이벤트를 처리하여 프레임에 대한 액세스 권한을 얻을 수 있습니다. 최종 HDR 사진 결과에만 관심이 있는 경우 이 작업을 수행할 필요가 없습니다.

**중요** [**OptionalReferencePhotoCaptured**](https://msdn.microsoft.com/library/windows/apps/mt181392)는 HDR 하드웨어를 지원하는, 즉 참조 프레임을 생성하지 않는 디바이스에서는 발생하지 않습니다. 앱은 이 이벤트가 발생하지 않는 경우를 처리해야 합니다.

참조 프레임은 **CaptureAsync** 호출에 대한 컨텍스트 정보 없이 도착하므로 **OptionalReferencePhotoCaptured** 처리기에 컨텍스트 정보를 제공하기 위한 메커니즘이 제공됩니다. 먼저 컨텍스트 정보가 포함될 개체가 있어야 합니다. 이 개체의 이름과 내용은 사용자가 결정합니다. 이 예제에서는 캡처의 파일 이름 및 카메라 방향을 추적하기 위한 멤버가 있는 개체를 정의합니다.

[!code-cs[AdvancedCaptureContext](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetAdvancedCaptureContext)]

컨텍스트 개체의 새 인스턴스를 만들고 해당 멤버를 채운 다음, 개체를 매개 변수로 수락하는 [**CaptureAsync**](https://msdn.microsoft.com/library/windows/apps/mt181388)의 오버로드에 전달합니다.

[!code-cs[CaptureWithContext](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetCaptureWithContext)]

[**OptionalReferencePhotoCaptured**](https://msdn.microsoft.com/library/windows/apps/mt181392) 이벤트 처리기에서 [**OptionalReferencePhotoCapturedEventArgs**](https://msdn.microsoft.com/library/windows/apps/mt181404) 개체의 [**Context**](https://msdn.microsoft.com/library/windows/apps/mt181405) 속성을 컨텍스트 개체 클래스로 캐스팅합니다. 이 예제에서는 최종 HDR 이미지에서 참조 프레임 이미지를 구분하기 위해 파일 이름을 수정하고 **ReencodeAndSavePhotoAsync** 도우미 메서드를 호출하여 이미지를 저장합니다.

[!code-cs[OptionalReferencePhotoCaptured](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetOptionalReferencePhotoCaptured)]

## 모든 프레임이 캡처되었을 때 알림 받기

HDR 사진 캡처는 두 단계로 진행됩니다. 먼저, 여러 프레임이 캡처된 다음, 프레임이 최종 HDR 이미지로 처리됩니다. 원본 HDR 프레임이 여전히 캡처되는 동안에는 다른 캡처를 시작할 수 없지만 모든 프레임이 캡처되고, HDR 사후 처리가 완료되기 전에는 캡처를 시작할 수 있습니다. HDR 캡처가 완료되어 다른 캡처를 시작할 수 있음을 사용자에게 알리면 [**AllPhotosCaptured**](https://msdn.microsoft.com/library/windows/apps/mt181387) 이벤트가 발생합니다. 일반적인 시나리오는 HDR 캡처가 시작되면 UI의 캡처 단추를 사용할 수 없게 설정하고, **AllPhotosCaptured**가 발생할 때는 다시 사용하도록 설정하는 것입니다.

[!code-cs[AllPhotosCaptured](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetAllPhotosCaptured)]

## AdvancedPhotoCapture 개체 정리

앱 캡처가 완료되고, **MediaCapture** 개체를 해제하기 전에, [**FinishAsync**](https://msdn.microsoft.com/library/windows/apps/mt181391)를 설정하고 멤버 변수를 null로 설정하여 [**AdvancedPhotoCapture**](https://msdn.microsoft.com/library/windows/apps/mt181386) 개체를 종료해야 합니다.

[!code-cs[CleanUpAdvancedPhotoCapture](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetCleanUpAdvancedPhotoCapture)]

## 관련 항목

* [MediaCapture를 사용하여 사진 및 비디오 캡처](capture-photos-and-video-with-mediacapture.md)



<!--HONumber=Jun16_HO4-->


