---
ms.assetid: 7DBEE5E2-C3EC-4305-823D-9095C761A1CD
description: 이 문서에서는 여러 이미지 프레임을 신속 하 게 캡처 하 고 각 프레임에서 다른 포커스, 플래시, ISO, 노출 및 노출이 보정 설정을 사용 하도록 구성할 수 있는 변수 사진 시퀀스를 캡처하는 방법을 보여 줍니다.
title: 변수 사진 시퀀스
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: a453aba0c8992e0df348e54620add5d65fce4403
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89175637"
---
# <a name="variable-photo-sequence"></a>변수 사진 시퀀스



이 문서에서는 여러 이미지 프레임을 신속 하 게 캡처 하 고 각 프레임에서 다른 포커스, 플래시, ISO, 노출 및 노출이 보정 설정을 사용 하도록 구성할 수 있는 변수 사진 시퀀스를 캡처하는 방법을 보여 줍니다. 이 기능을 사용 하면 HDR (High Dynamic Range) 이미지를 만드는 등의 시나리오를 사용할 수 있습니다.

HDR 이미지를 캡처 하지만 자체 처리 알고리즘을 구현 하지 않으려는 경우 [**AdvancedPhotoCapture**](/uwp/api/Windows.Media.Capture.AdvancedPhotoCapture) API를 사용 하 여 Windows에 기본 제공 되는 hdr 기능을 사용할 수 있습니다. 자세한 내용은 [HDR (High Dynamic Range) 사진 캡처](high-dynamic-range-hdr-photo-capture.md)를 참조 하세요.

> [!NOTE] 
> 이 문서는 기본 사진 및 비디오 캡처를 구현 하는 단계를 설명 하는 [MediaCapture을 사용 하 여 기본 사진, 비디오 및 오디오 캡처](basic-photo-video-and-audio-capture-with-MediaCapture.md)에 설명 된 개념 및 코드를 기반으로 합니다. 고급 캡처 시나리오로 전환 하기 전에 해당 문서의 기본적인 미디어 캡처 패턴을 숙지 하는 것이 좋습니다. 이 문서의 코드는 앱에 이미 올바르게 초기화 된 MediaCapture 인스턴스가 있다고 가정 합니다.

## <a name="set-up-your-app-to-use-variable-photo-sequence-capture"></a>가변 사진 시퀀스 캡처를 사용 하도록 앱 설정

기본 미디어 캡처에 필요한 네임 스페이스 외에도 변수 사진 시퀀스 캡처를 구현 하려면 다음 네임 스페이스가 필요 합니다.

[!code-cs[VPSUsing](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetVPSUsing)]

[**VariablePhotoSequenceCapture**](/uwp/api/Windows.Media.Capture.Core.VariablePhotoSequenceCapture) 개체를 저장 하는 데 사용 되는 멤버 변수를 선언 합니다 .이 개체는 사진 시퀀스 캡처를 시작 하는 데 사용 됩니다. 캡처한 각 이미지를 시퀀스에 저장 하 [**는 개체의**](/uwp/api/Windows.Graphics.Imaging.SoftwareBitmap) 배열을 선언 합니다. 또한 각 프레임에 대 한 [**CapturedFrameControlValues**](/uwp/api/Windows.Media.Capture.CapturedFrameControlValues) 개체를 저장 하는 배열을 선언 합니다. 이미지 처리 알고리즘에서 각 프레임을 캡처하는 데 사용 된 설정을 결정 하는 데 사용할 수 있습니다. 마지막으로, 시퀀스에서 현재 캡처되고 있는 이미지를 추적 하는 데 사용 될 인덱스를 선언 합니다.

[!code-cs[VPSMemberVariables](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetVPSMemberVariables)]

## <a name="prepare-the-variable-photo-sequence-capture"></a>사진 시퀀스 캡처 변수 준비

[MediaCapture](./index.md)를 초기화 한 후에는 미디어 캡처의 [**VideoDeviceController**](/uwp/api/Windows.Media.Devices.VideoDeviceController) 에서 [**VariablePhotoSequenceController**](/uwp/api/Windows.Media.Devices.Core.VariablePhotoSequenceController) 의 인스턴스를 가져오고 [**지원 되**](/uwp/api/windows.media.devices.core.variablephotosequencecontroller.supported) 는 속성을 확인 하 여 현재 장치에서 변수 사진 시퀀스가 지원 되는지 확인 합니다.

[!code-cs[IsVPSSupported](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetIsVPSSupported)]

변수 사진 시퀀스 컨트롤러에서 [**FrameControlCapabilities**](/uwp/api/Windows.Media.Devices.Core.FrameControlCapabilities) 개체를 가져옵니다. 이 개체에는 사진 시퀀스 프레임당 구성할 수 있는 모든 설정에 대 한 속성이 있습니다. 여기에는 다음이 포함됩니다.

-   [**노출을**](/uwp/api/windows.media.devices.core.framecontrolcapabilities.exposure)
-   [**ExposureCompensation**](/uwp/api/windows.media.devices.core.framecontrolcapabilities.exposurecompensation)
-   [**깜박임**](/uwp/api/windows.media.devices.core.framecontrolcapabilities.flash)
-   [**포커스**](/uwp/api/windows.media.devices.core.framecontrolcapabilities.focus)
-   [**IsoSpeed**](/uwp/api/windows.media.devices.core.framecontrolcapabilities.isospeed)
-   [**사진 확인**](/uwp/api/windows.media.devices.core.framecontrolcapabilities.photoconfirmationsupported)

이 예에서는 각 프레임에 대해 다른 노출 보정 값을 설정 합니다. 현재 장치의 사진 시퀀스에 대해 노출 보정이 지원 되는지 확인 하려면 **ExposureCompensation** 속성을 통해 액세스 하는 [**FrameExposureCompensationCapabilities**](/uwp/api/Windows.Media.Devices.Core.FrameExposureCompensationCapabilities) 개체의 [**지원**](/uwp/api/windows.media.devices.exposurecompensationcontrol.supported) 되는 속성을 확인 합니다.

[!code-cs[IsExposureCompensationSupported](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetIsExposureCompensationSupported)]

캡처할 각 프레임에 대 한 새 [**FrameController**](/uwp/api/Windows.Media.Devices.Core.FrameController) 개체를 만듭니다. 이 예제에서는 세 개의 프레임을 캡처합니다. 각 프레임에 대해 변경 하려는 컨트롤의 값을 설정 합니다. 그런 다음 **VariablePhotoSequenceController** 의 [**DesiredFrameControllers**](/uwp/api/windows.media.devices.core.variablephotosequencecontroller.desiredframecontrollers) 컬렉션을 지우고 각 프레임 컨트롤러를 컬렉션에 추가 합니다.

[!code-cs[InitFrameControllers](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetInitFrameControllers)]

[**ImageEncodingProperties**](/uwp/api/Windows.Media.MediaProperties.ImageEncodingProperties) 개체를 만들어 캡처된 이미지에 사용할 인코딩을 설정 합니다. 인코딩 속성을 전달 하 여 정적 메서드 [**MediaCapture**](/uwp/api/windows.media.capture.mediacapture.preparevariablephotosequencecaptureasync)를 호출 합니다. 이 메서드는 [**VariablePhotoSequenceCapture**](/uwp/api/Windows.Media.Capture.Core.VariablePhotoSequenceCapture) 개체를 반환 합니다. 마지막으로 [**캡처한**](/uwp/api/windows.media.capture.core.variablephotosequencecapture.photocaptured) 이벤트와 [**중지**](/uwp/api/windows.media.capture.core.variablephotosequencecapture.stopped) 된 이벤트에 대 한 이벤트 처리기를 등록 합니다.

[!code-cs[PrepareVPS](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetPrepareVPS)]

## <a name="start-the-variable-photo-sequence-capture"></a>가변 사진 시퀀스 캡처 시작

변수 사진 시퀀스의 캡처를 시작 하려면 [**VariablePhotoSequenceCapture. StartAsync**](/uwp/api/windows.media.capture.core.variablephotosequencecapture.startasync)를 호출 합니다. 캡처한 이미지와 프레임 컨트롤 값을 저장 하기 위한 배열을 초기화 하 고 현재 인덱스를 0으로 설정 해야 합니다. 응용 프로그램의 기록 상태 변수를 설정 하 고,이 캡처가 진행 중인 동안 다른 캡처를 시작 하지 않도록 UI를 업데이트 합니다.

[!code-cs[StartVPSCapture](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetStartVPSCapture)]

## <a name="receive-the-captured-frames"></a>캡처된 프레임 수신

캡처된 각 프레임에 대해 [**캡처된**](/uwp/api/windows.media.capture.core.variablephotosequencecapture.photocaptured) 이벤트가 발생 합니다. 프레임의 프레임 컨트롤 값과 캡처된 이미지를 저장 하 고 현재 프레임 인덱스를 늘립니다. 이 예제에서는 각 프레임의 고 대/ [**비트맵**](/uwp/api/Windows.Graphics.Imaging.SoftwareBitmap) 표현을 가져오는 방법을 보여 줍니다. **\Bitmap**사용에 대 한 자세한 내용은 [Imaging](imaging.md)을 참조 하세요.

[!code-cs[OnPhotoCaptured](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetOnPhotoCaptured)]

## <a name="handle-the-completion-of-the-variable-photo-sequence-capture"></a>사진 시퀀스 캡처 변수의 완료를 처리 합니다.

시퀀스의 모든 프레임이 캡처될 때 [**중지**](/uwp/api/windows.media.capture.core.variablephotosequencecapture.stopped) 된 이벤트가 발생 합니다. 응용 프로그램의 기록 상태를 업데이트 하 고 사용자가 새 캡처를 시작할 수 있도록 UI를 업데이트 합니다. 이 시점에서 캡처된 이미지 및 프레임 컨트롤 값을 이미지 처리 코드에 전달할 수 있습니다.

[!code-cs[OnStopped](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetOnStopped)]

## <a name="update-frame-controllers"></a>프레임 컨트롤러 업데이트

프레임별 설정 마다 다른 변수 사진 시퀀스 캡처를 수행 하려는 경우 **VariablePhotoSequenceCapture**를 완전히 다시 초기화할 필요가 없습니다. [**DesiredFrameControllers**](/uwp/api/windows.media.devices.core.variablephotosequencecontroller.desiredframecontrollers) 컬렉션을 지우거 나 새 프레임 컨트롤러를 추가 하거나 기존 프레임 컨트롤러 값을 수정할 수 있습니다. 다음 예제에서는 [**FrameFlashCapabilities**](/uwp/api/Windows.Media.Devices.Core.FrameFlashCapabilities) 개체를 검사 하 여 현재 장치가 가변 사진 시퀀스 프레임에 대 한 플래시 및 플래시 기능을 지원 하는지 확인 합니다. 그렇다면 각 프레임은 100% 전원에서 플래시를 사용 하도록 업데이트 됩니다. 각 프레임에 대해 이전에 설정 된 노출 보정 값은 여전히 활성 상태입니다.

[!code-cs[UpdateFrameControllers](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetUpdateFrameControllers)]

## <a name="clean-up-the-variable-photo-sequence-capture"></a>변수 사진 시퀀스 캡처 정리

변수 사진 시퀀스 캡처를 완료 했거나 앱이 일시 중단 되는 경우에는 done [**async**](/uwp/api/windows.media.capture.core.variablephotosequencecapture.finishasync)를 호출 하 여 변수 사진 시퀀스 개체를 정리 합니다. 개체의 이벤트 처리기를 등록 취소 하 고이를 null로 설정 합니다.

[!code-cs[CleanUpVPS](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetCleanUpVPS)]

## <a name="related-topics"></a>관련 항목

* [카메라](camera.md)
* [MediaCapture를 사용하여 기본적인 사진, 비디오 및 오디오 캡처](basic-photo-video-and-audio-capture-with-MediaCapture.md)
 

 