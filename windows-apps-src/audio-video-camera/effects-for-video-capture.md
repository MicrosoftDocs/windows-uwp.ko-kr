---
author: drewbatgit
ms.assetid: E0189423-1DF3-4052-AB2E-846EA18254C4
description: 이 항목에서는 카메라 미리 보기에 효과를 적용하는 방법과 비디오 녹화 스트림을 보여 주고, 동영상 보정 효과를 사용하는 방법을 보여 줍니다.
title: 비디오 캡처 효과
ms.author: drewbat
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: d427a532e9821b81b6f23d08babecd692c8c95e1
ms.sourcegitcommit: e38b334edb82bf2b1474ba686990f4299b8f59c7
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/15/2018
ms.locfileid: "6837816"
---
# <a name="effects-for-video-capture"></a>비디오 캡처 효과


이 항목에서는 카메라 미리 보기에 효과를 적용하는 방법과 비디오 녹화 스트림을 보여 주고, 동영상 보정 효과를 사용하는 방법을 보여 줍니다.

> [!NOTE] 
> 이 문서는 기본 사진 및 비디오 캡처 구현 단계를 설명하는 [MediaCapture를 사용한 기본적인 사진, 비디오 및 오디오 캡처](basic-photo-video-and-audio-capture-with-MediaCapture.md)에 설명된 개념 및 코드를 토대로 작성되었습니다. 보다 수준 높은 캡처 시나리오를 진행하기 전에 해당 문서의 기본적인 미디어 캡처 패턴을 파악하는 것이 좋습니다. 이 문서의 코드는 앱에 적절히 초기화된 MediaCapture의 인스턴스가 이미 있다고 가정합니다.

## <a name="adding-and-removing-effects-from-the-camera-video-stream"></a>카메라 비디오 스트림에서 효과 추가 및 제거
디바이스의 카메라에서 비디오를 캡처하거나 미리 보려면 [MediaCapture를 사용하여 기본적인 사진, 비디오 및 오디오 캡처](basic-photo-video-and-audio-capture-with-MediaCapture.md)에서 설명한 대로 [**MediaCapture**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.MediaCapture) 개체를 사용하면 됩니다. **MediaCapture** 개체를 초기화하고 나면 [**AddVideoEffectAsync**](https://msdn.microsoft.com/library/windows/apps/dn878035)를 호출하여 미리 보기 또는 캡처 스트림에 비디오 효과를 하나 이상 추가하여 추가할 효과를 나타내는 [**IVideoEffectDefinition**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Effects.IVideoEffectDefinition) 개체로 전달하고, 카메라의 미리 보기 스트림 또는 녹화 스트림에 효과를 추가할지 여부를 나타내는 [**MediaStreamType**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.MediaStreamType) 열거 구성원을 호출합니다.

> [!NOTE]
> 일부 디바이스에서 미리 보기 스트림과 캡처 스트림은 같습니다. 즉 **AddVideoEffectAsync**를 호출할 때 **MediaStreamType.VideoPreview** 또는 **MediaStreamType.VideoRecord**를 지정하는 경우 효과가 미리 보기와 녹화 스트림에 모두 적용됩니다. **MediaCapture** 개체에 대해 [**MediaCaptureSettings**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.MediaCapture.MediaCaptureSettings)의 [**VideoDeviceCharacteristic**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.MediaCaptureSettings.VideoDeviceCharacteristic) 속성을 확인하여 현재 디바이스에서 미리 보기 및 녹화 스트림이 동일한지 여부를 확인할 수 있습니다. 이 속성의 값이 **VideoDeviceCharacteristic.AllStreamsIdentical** 또는 **VideoDeviceCharacteristic.PreviewRecordStreamsIdentical**인 경우 스트림은 동일하며 한 스트림에 적용할 효과가 다른 스트림에도 영향을 미칩니다.

다음 예제에서는 카메라 미리 보기 및 녹화 스트림 모두에 효과를 추가합니다. 이 예제에서는 녹화 및 미리 보기 스트림이 동일한지 여부를 확인하는 방법에 대해 보여 줍니다.

[!code-cs[BasicAddEffect](./code/SimpleCameraPreview_Win10/cs/MainPage.Effects.xaml.cs#SnippetBasicAddEffect)]

**AddVideoEffectAsync**는 추가된 비디오 효과를 나타내는 [**IMediaExtension**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.IMediaExtension)을 구현하는 개체를 반환합니다. 몇 가지 효과를 사용하면 [**PropertySet**](https://msdn.microsoft.com/library/windows/apps/Windows.Foundation.Collections.PropertySet)를 [**SetProperties**](https://msdn.microsoft.com/library/windows/apps/br240986) 메서드로 전달하여 효과 설정을 변경할 수 있습니다.

Windows10 버전 1607부터는 **AddVideoEffectAsync**에서 반환된 개체를 통해 [**RemoveEffectAsync**](https://msdn.microsoft.com/library/windows/apps/mt667957)로 전달하여 비디오 파이프라인에서 효과를 제거할 수도 있습니다. **RemoveEffectAsync**는 미리 보기 또는 녹화 스트림에 효과 개체 매개 변수를 추가했는지 자동으로 확인하므로 호출할 때 스트림 형식을 지정할 필요가 없습니다.

[!code-cs[RemoveOneEffect](./code/SimpleCameraPreview_Win10/cs/MainPage.Effects.xaml.cs#SnippetRemoveOneEffect)]

[**ClearEffectsAsync**](https://msdn.microsoft.com/library/windows/apps/br226592)를 호출하고 모든 효과를 제거해야 하는 스트림을 지정하여 미리 보기 또는 캡처 스트림에서 모든 효과를 제거할 수도 있습니다.

[!code-cs[ClearAllEffects](./code/SimpleCameraPreview_Win10/cs/MainPage.Effects.xaml.cs#SnippetClearAllEffects)]

## <a name="video-stabilization-effect"></a>동영상 손떨림 보정 효과

동영상 손떨림 보정 효과는 비디오 스트림 프레임을 조작하여 손으로 캡처 디바이스를 잡고 있을 때 발생하는 흔들림을 최소화합니다. 이 기술을 사용하면 픽셀이 오른쪽, 왼쪽, 위쪽 및 아래쪽으로 이동되고, 해당 효과가 비디오 프레임 바로 밖에 있는 콘텐츠가 무엇인지 파악할 수 없으므로 원본 비디오에서 손떨림이 보정된 비디오는 약간 잘리는 결과를 가져옵니다. 효과를 통해 수행되는 자르기를 최적으로 관리하도록 비디오 인코딩 설정을 조정할 수 있는 유틸리티 함수가 제공됩니다.

지원하는 디바이스에서 OIS(광학 이미지 손떨림 보정)는 캡처 디바이스를 기계적으로 조작하여 동영상 손떨림을 보정하므로 비디오 프레임의 가장자리를 자를 필요가 없습니다. 자세한 내용은 [비디오 캡처를 위한 캡처 디바이스 컨트롤](capture-device-controls-for-video-capture.md)을 참조하세요.

### <a name="set-up-your-app-to-use-video-stabilization"></a>동영상 손떨림 보정을 사용하도록 앱 설정

기본 미디어 캡처에 필요한 네임스페이스 외에도, 동영상 손떨림 보정 효과를 사용하려면 다음 네임스페이스가 필요합니다.

[!code-cs[VideoStabilizationEffectUsing](./code/SimpleCameraPreview_Win10/cs/MainPage.Effects.xaml.cs#SnippetVideoStabilizationEffectUsing)]

[**VideoStabilizationEffect**](https://msdn.microsoft.com/library/windows/apps/dn926760) 개체를 저장하기 위한 멤버 변수를 선언합니다. 효과 구현의 일부로, 캡처된 비디오를 인코딩하는 데 사용하는 인코딩 속성을 수정합니다. 나중에 효과를 사용하지 않도록 설정할 때 복원할 수 있도록 초기 입력 및 출력 인코딩 속성의 백업 복사본을 저장하기 위한 2개의 변수를 선언합니다. 마지막으로 [**MediaEncodingProfile**](https://msdn.microsoft.com/library/windows/apps/hh701026) 형식의 멤버 변수를 선언합니다. 이 개체는 코드 내의 여러 위치에서 액세스되므로 이 작업이 필요합니다.

[!code-cs[DeclareVideoStabilizationEffect](./code/SimpleCameraPreview_Win10/cs/MainPage.Effects.xaml.cs#SnippetDeclareVideoStabilizationEffect)]

이 시나리오에서 나중에 액세스할 수 있도록 미디어 인코딩 프로파일 개체를 멤버 변수에 할당해야 합니다.

[!code-cs[EncodingProfileMember](./code/SimpleCameraPreview_Win10/cs/MainPage.Effects.xaml.cs#SnippetEncodingProfileMember)]

### <a name="initialize-the-video-stabilization-effect"></a>동영상 손떨림 보정 효과 초기화

**MediaCapture** 개체가 초기화된 후에 [**VideoStabilizationEffectDefinition**](https://msdn.microsoft.com/library/windows/apps/dn926762) 개체의 새 인스턴스를 만듭니다. [**MediaCapture.AddVideoEffectAsync**](https://msdn.microsoft.com/library/windows/apps/dn878035)를 호출하여 비디오 파이프라인에 효과를 추가하고 [**VideoStabilizationEffect**](https://msdn.microsoft.com/library/windows/apps/dn926760) 클래스의 인스턴스를 검색합니다. [**MediaStreamType.VideoRecord**](https://msdn.microsoft.com/library/windows/apps/br226640)를 지정하여 해당 효과가 비디오 녹화 스트림에 적용되어야 함을 나타냅니다.

[**EnabledChanged**](https://msdn.microsoft.com/library/windows/apps/dn948982) 이벤트에 대한 이벤트 처리기를 등록하고 도우미 메서드 **SetUpVideoStabilizationRecommendationAsync**를 호출합니다. 이 두 작업은 이 문서 뒷부분 설명되어 있습니다. 마지막으로 효과의 [**Enabled**](https://msdn.microsoft.com/library/windows/apps/dn926775) 속성을 true로 설정하여 효과를 사용하도록 설정합니다.

[!code-cs[CreateVideoStabilizationEffect](./code/SimpleCameraPreview_Win10/cs/MainPage.Effects.xaml.cs#SnippetCreateVideoStabilizationEffect)]

### <a name="use-recommended-encoding-properties"></a>권장 인코딩 속성 사용

이 문서의 앞부분에서 설명한 것처럼 동영상 손떨림 보정 효과에 사용되는 기술을 적용하면 보정된 비디오가 원본 비디오에서 약간 잘릴 수밖에 없습니다. 비디오 인코딩 속성을 조정하여 이러한 효과의 제한 요인을 최적으로 처리하려면 코드에 다음 도우미 함수를 정의합니다. 이 단계가 동영상 손떨림 보정 효과를 사용하는 데 필요한 것은 아니지만 이 단계를 수행하지 않으면 결과 비디오의 크기가 약간 커지므로 시각적 품질이 저하됩니다.

동영상 손떨림 보정 효과 인스턴스에 대해 [**GetRecommendedStreamConfiguration**](https://msdn.microsoft.com/library/windows/apps/dn948983)을 호출하고, 효과에 현재 입력 인코딩 속성에 대한 정보를 제공하는 [**VideoDeviceController**](https://msdn.microsoft.com/library/windows/apps/br226825) 개체와 효과에 현재 출력 인코딩 속성에 대한 정보를 저장하는 아 를 전달합니다. 이 개체는 인코딩 속성에 대한 정보를 제공하는 **MediaEncodingProfile** 개체를 전달합니다. 이 메서드는 새로운 권장 입력 및 출력 스트림 인코딩 속성을 포함하는 [**VideoStreamConfiguration**](https://msdn.microsoft.com/library/windows/apps/dn926727) 개체를 반환합니다.

디바이스에서 지원할 경우 권장되는 입력 인코딩 속성은 사용자가 제공한 초기 설정보다 더 높은 해상도이므로, 효과로 인한 자르기가 적용된 후에 해상도가 최소로 손실됩니다.

[**VideoDeviceController.SetMediaStreamPropertiesAsync**](https://msdn.microsoft.com/library/windows/apps/hh700895)를 호출하여 새 인코딩 속성을 설정합니다. 효과를 사용하지 않도록 설정할 경우 설정을 다시 원래대로 변경할 수 있도록 새 속성을 설정하기 전에 멤버 변수를 사용하여 초기 인코딩 속성을 저장합니다.

동영상 손떨림 보정 효과로 인해 출력 비디오가 잘려야 할 경우 권장되는 출력 인코딩 속성은 잘린 비디오의 크기가 됩니다. 즉, 출력 해상도가 잘린 비디오 크기에 적절히 맞게 됩니다. 권장되는 출력 속성을 사용하지 않는 경우 비디오는 초기 출력 크기에 맞게 커지므로 시각적 품질이 저하됩니다.

**MediaEncodingProfile** 개체의 [**Video**](https://msdn.microsoft.com/library/windows/apps/hh701124) 속성을 설정합니다. 효과를 사용하지 않도록 설정할 경우 설정을 다시 원래대로 변경할 수 있도록 새 속성을 설정하기 전에 멤버 변수를 사용하여 초기 인코딩 속성을 저장합니다.

[!code-cs[SetUpVideoStabilizationRecommendationAsync](./code/SimpleCameraPreview_Win10/cs/MainPage.Effects.xaml.cs#SnippetSetUpVideoStabilizationRecommendationAsync)]

### <a name="handle-the-video-stabilization-effect-being-disabled"></a>동영상 손떨림 보정 효과를 사용할 수 없게 처리

시스템은 픽셀 처리량이 너무 높아 해당 효과가 처리할 수 없거나, 효과가 느리게 실행되는 것이 감지될 경우 동영상 손떨림 보정 효과를 자동으로 사용하지 않도록 설정할 수 있습니다. 이 경우 EnabledChanged 이벤트가 발생합니다. *sender* 매개 변수의 **VideoStabilizationEffect** 인스턴스는 효과의 새로운 상태가 사용 또는 사용 안 함임을 나타냅니다. [**VideoStabilizationEffectEnabledChangedEventArgs**](https://msdn.microsoft.com/library/windows/apps/dn948979)는 효과가 사용되거나 사용되지 않도록 설정된 이유를 나타내는 [**VideoStabilizationEffectEnabledChangedReason**](https://msdn.microsoft.com/library/windows/apps/dn948981) 값을 갖습니다. 프로그래밍 방식으로 효과를 사용하거나 사용하지 않도록 설정하는 경우에도 **Programmatic** 이유로 인해 이 이벤트가 발생합니다.

일반적으로 이 이벤트를 사용하여 동영상 손 떨림 보정의 현재 상태를 나타내도록 앱의 UI를 조정합니다.

[!code-cs[VideoStabilizationEnabledChanged](./code/SimpleCameraPreview_Win10/cs/MainPage.Effects.xaml.cs#SnippetVideoStabilizationEnabledChanged)]

### <a name="clean-up-the-video-stabilization-effect"></a>동영상 손떨림 보정 효과 정리

동영상 손떨림 보정 효과를 정리하려면 [**RemoveEffectAsync**](https://msdn.microsoft.com/library/windows/apps/mt667957)를 호출하여 비디오 파이프라인의 효과를 제거합니다. 초기 인코딩 속성을 포함하는 멤버 변수가 null이 아닌 경우 인코딩 속성을 복원하는 데 사용합니다. 마지막으로 **EnabledChanged** 이벤트 처리기를 제거하고 효과를 null로 설정합니다.

[!code-cs[CleanUpVisualStabilizationEffect](./code/SimpleCameraPreview_Win10/cs/MainPage.Effects.xaml.cs#SnippetCleanUpVisualStabilizationEffect)]

## <a name="related-topics"></a>관련 항목

* [카메라](camera.md)
* [MediaCapture를 사용하여 기본적인 사진, 비디오 및 오디오 캡처](basic-photo-video-and-audio-capture-with-MediaCapture.md)
 

 




