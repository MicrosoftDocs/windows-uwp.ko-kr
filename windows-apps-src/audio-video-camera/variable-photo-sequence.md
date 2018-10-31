---
author: drewbatgit
ms.assetid: 7DBEE5E2-C3EC-4305-823D-9095C761A1CD
description: 이 문서에서는 가변 사진 시퀀스를 캡처하여 여러 이미지 프레임을 빠른 속도로 연속으로 캡처하고 서로 다른 초점, 플래시, ISO, 노출 및 노출 보정 설정을 사용하도록 각 프레임을 구성할 수 있도록 하는 방법을 보여 줍니다.
title: 가변 사진 시퀀스
ms.author: drewbat
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 91a7d69d945b2ba2452d5bc477b6c17bf1dc6845
ms.sourcegitcommit: cd00bb829306871e5103db481cf224ea7fb613f0
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/31/2018
ms.locfileid: "5888782"
---
# <a name="variable-photo-sequence"></a>가변 사진 시퀀스



이 문서에서는 가변 사진 시퀀스를 캡처하여 여러 이미지 프레임을 빠른 속도로 연속으로 캡처하고 서로 다른 초점, 플래시, ISO, 노출 및 노출 보정 설정을 사용하도록 각 프레임을 구성할 수 있도록 하는 방법을 보여 줍니다. 이 기능을 사용하면 HDR(High Dynamic Range) 이미지 만들기 같은 시나리오가 가능합니다.

HDR 이미지를 캡처하지만 고유한 처리 알고리즘을 구현하지 않으려는 경우에는 [**AdvancedPhotoCapture**](https://msdn.microsoft.com/library/windows/apps/mt181386) API를 사용하여 Windows에서 기본 제공되는 HDR 기능을 사용할 수 있습니다. 자세한 내용은 [HDR(High Dynamic Range) 사진 캡처](high-dynamic-range-hdr-photo-capture.md)를 참조하세요.

> [!NOTE] 
> 이 문서는 기본 사진 및 비디오 캡처 구현 단계를 설명하는 [MediaCapture를 사용한 기본적인 사진, 비디오 및 오디오 캡처](basic-photo-video-and-audio-capture-with-MediaCapture.md)에 설명된 개념 및 코드를 토대로 작성되었습니다. 좀 더 수준 높은 캡처 시나리오를 진행하기 전에 해당 문서의 기본 미디어 캡처 패턴을 좀 더 잘 이해하는 것이 좋습니다. 이 문서의 코드는 앱에 적절히 초기화된 MediaCapture의 인스턴스가 이미 있다고 가정합니다.

## <a name="set-up-your-app-to-use-variable-photo-sequence-capture"></a>가변 사진 시퀀스 캡처를 사용하도록 앱 설정

기본 미디어 캡처에 필요한 네임스페이스 외에도, 가변 사진 시퀀스 캡처를 구현하려면 다음 네임스페이스가 필요합니다.

[!code-cs[VPSUsing](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetVPSUsing)]

사진 시퀀스 캡처를 시작하는 데 사용되는 [**VariablePhotoSequenceCapture**](https://msdn.microsoft.com/library/windows/apps/dn652564) 개체를 저장하는 멤버 변수를 선언합니다. 시퀀스의 각 캡처된 이미지를 저장하는 [**SoftwareBitmap**](https://msdn.microsoft.com/library/windows/apps/dn887358) 개체의 배열을 선언합니다. 또한, 각 프레임에 대한 [**CapturedFrameControlValues**](https://msdn.microsoft.com/library/windows/apps/dn608020) 개체를 저장하는 배열을 선언합니다. 이 배열은 각 프레임을 캡처하는 데 사용된 설정을 확인하기 위해 이미지 처리 알고리즘에서 사용될 수 있습니다. 마지막으로, 시퀀스에서 현재 캡처되는 이미지를 추적하는 데 사용되는 인덱스를 선언합니다.

[!code-cs[VPSMemberVariables](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetVPSMemberVariables)]

## <a name="prepare-the-variable-photo-sequence-capture"></a>가변 사진 시퀀스 캡처 준비

[MediaCapture](capture-photos-and-video-with-mediacapture.md)를 초기화한 후에는 [**VariablePhotoSequenceController**](https://msdn.microsoft.com/library/windows/apps/dn640573) 미디어 캡처의 [**VideoDeviceController**](https://msdn.microsoft.com/library/windows/apps/br226825)에서 인스턴스를 가져오고 [**Supported**](https://msdn.microsoft.com/library/windows/apps/dn640580) 속성을 확인하여 현재 디바이스에서 가변 사진 시퀀스가 지원되는지 확인합니다.

[!code-cs[IsVPSSupported](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetIsVPSSupported)]

가변 사진 시퀀스 컨트롤러에서 [**FrameControlCapabilities**](https://msdn.microsoft.com/library/windows/apps/dn652548) 개체를 가져옵니다. 이 개체에는 모든 설정에 대해 사진 시퀀스의 프레임별로 구성할 수 있는 속성이 있습니다. 다음이 포함됩니다.

-   [**Exposure**](https://msdn.microsoft.com/library/windows/apps/dn652552)
-   [**ExposureCompensation**](https://msdn.microsoft.com/library/windows/apps/dn652560)
-   [**Flash**](https://msdn.microsoft.com/library/windows/apps/dn652566)
-   [**Focus**](https://msdn.microsoft.com/library/windows/apps/dn652570)
-   [**IsoSpeed**](https://msdn.microsoft.com/library/windows/apps/dn652574)
-   [**PhotoConfirmation**](https://msdn.microsoft.com/library/windows/apps/dn652578)

이 예제에서는 각 프레임에 대해 다른 노출 보정 값을 설정합니다. 현재 디바이스에서 사진 시퀀스에 대해 노출 보정이 지원되는지 확인하려면 **ExposureCompensation** 속성을 통해 액세스되는 [**FrameExposureCompensationCapabilities**](https://msdn.microsoft.com/library/windows/apps/dn652628) 개체의 [**Supported**](https://msdn.microsoft.com/library/windows/apps/dn278905) 속성을 확인합니다.

[!code-cs[IsExposureCompensationSupported](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetIsExposureCompensationSupported)]

캡처하려는 각 프레임에 대해 새 [**FrameController**](https://msdn.microsoft.com/library/windows/apps/dn652582) 개체를 만듭니다. 이 예제에서는 세 개의 프레임을 캡처합니다. 각 프레임에 따라 다르게 할 컨트롤의 값을 설정합니다. 그런 다음, **VariablePhotoSequenceController**의 [**DesiredFrameControllers**](https://msdn.microsoft.com/library/windows/apps/dn640574) 컬렉션을 선택 취소하고 컬렉션에 각 프레임 컨트롤러를 추가합니다.

[!code-cs[InitFrameControllers](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetInitFrameControllers)]

[**ImageEncodingProperties**](https://msdn.microsoft.com/library/windows/apps/hh700993) 개체를 만들어 캡처된 이미지에 대해 사용하려는 인코딩을 설정합니다. 정적 메서드 [**MediaCapture.PrepareVariablePhotoSequenceCaptureAsync**](https://msdn.microsoft.com/library/windows/apps/dn608097)를 호출하고 인코딩 속성을 전달합니다. 이 메서드는 [**VariablePhotoSequenceCapture**](https://msdn.microsoft.com/library/windows/apps/dn652564) 개체를 반환합니다. 마지막으로, [**PhotoCaptured**](https://msdn.microsoft.com/library/windows/apps/dn652573) 및 [**Stopped**](https://msdn.microsoft.com/library/windows/apps/dn652585) 이벤트에 대한 이벤트 처리기를 등록합니다.

[!code-cs[PrepareVPS](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetPrepareVPS)]

## <a name="start-the-variable-photo-sequence-capture"></a>가변 사진 시퀀스 캡처 시작

가변 사진 시퀀스 캡처를 시작하려면 [**VariablePhotoSequenceCapture.StartAsync**](https://msdn.microsoft.com/library/windows/apps/dn652577)를 호출합니다. 캡처된 이미지 및 프레임 컨트롤 값을 저장하기 위한 배열을 초기화하고 현재 인덱스를 0으로 설정해야 합니다. 앱의 기록 상태 변수를 설정하고 이 캡처가 진행 중인 동안 다른 캡처를 시작하지 않도록 UI를 업데이트합니다.

[!code-cs[StartVPSCapture](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetStartVPSCapture)]

## <a name="receive-the-captured-frames"></a>캡처된 프레임 수신

각 캡처된 프레임에 대해 [**PhotoCaptured**](https://msdn.microsoft.com/library/windows/apps/dn652573) 이벤트가 발생합니다. 프레임 컨트롤 값과 프레임에 대해 캡처된 이미지를 저장한 다음 현재 프레임 인덱스를 증분합니다. 이 예제에서는 각 프레임의 [**SoftwareBitmap**](https://msdn.microsoft.com/library/windows/apps/dn887358) 표현을 가져오는 방법을 보여 줍니다. **SoftwareBitmap** 사용에 대한 자세한 내용은 [이미징](imaging.md)을 참조하세요.

[!code-cs[OnPhotoCaptured](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetOnPhotoCaptured)]

## <a name="handle-the-completion-of-the-variable-photo-sequence-capture"></a>가변 사진 시퀀스 캡처의 완료 처리

시퀀스의 모든 프레임 캡처가 완료되면 [**Stopped**](https://msdn.microsoft.com/library/windows/apps/dn652585) 이벤트가 발생합니다. 앱의 기록 상태를 업데이트하고 사용자가 새 캡처를 시작할 수 있도록 UI를 업데이트합니다. 이 시점에 이미지 처리 코드에 캡처된 이미지 및 프레임 컨트롤 값을 전달할 수 있습니다.

[!code-cs[OnStopped](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetOnStopped)]

## <a name="update-frame-controllers"></a>프레임 컨트롤러 업데이트

다른 프레임당 설정을 사용하여 다른 가변 사진 시퀀스 캡처를 수행하려는 경우 **VariablePhotoSequenceCapture**를 완전히 다시 초기화할 필요가 없습니다. [**DesiredFrameControllers**](https://msdn.microsoft.com/library/windows/apps/dn640574) 컬렉션을 해제하고 새 프레임 컨트롤러를 추가하거나 기존 프레임 컨트롤러 값을 수정할 수 있습니다. 다음 예제에서는 [**FrameFlashCapabilities**](https://msdn.microsoft.com/library/windows/apps/dn652657) 개체를 검사하여 현재 디바이스가 가변 사진 시퀀스 프레임에 대해 플래시 및 플래시 전원을 지원하는지 확인합니다. 지원하는 경우 각 프레임은 100% 전원으로 플래시를 사용하도록 업데이트됩니다. 각 프레임에 대해 이전에 설정된 노출 보정 값은 여전히 활성 상태입니다.

[!code-cs[UpdateFrameControllers](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetUpdateFrameControllers)]

## <a name="clean-up-the-variable-photo-sequence-capture"></a>가변 사진 시퀀스 캡처 정리

가변 사진 시퀀스 캡처를 완료하거나 앱을 일시 중단하는 경우 [**FinishAsync**](https://msdn.microsoft.com/library/windows/apps/dn652569)를 호출하여 가변 사진 시퀀스 개체를 정리합니다. 개체의 이벤트 처리기를 등록 취소하고 null로 설정합니다.

[!code-cs[CleanUpVPS](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetCleanUpVPS)]

## <a name="related-topics"></a>관련 항목

* [카메라](camera.md)
* [MediaCapture를 사용하여 기본적인 사진, 비디오 및 오디오 캡처](basic-photo-video-and-audio-capture-with-MediaCapture.md)
 

 




