---
ms.assetid: E0189423-1DF3-4052-AB2E-846EA18254C4
description: 이 항목에서는 카메라 미리 보기 및 녹화 비디오 스트림에 효과를 적용 하는 방법을 보여 주고, 비디오 안정화 효과를 사용 하는 방법을 보여 줍니다.
title: 비디오 캡처에 대 한 효과
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: ffb379110a42579cd5bb2427f9c851637ff191be
ms.sourcegitcommit: c3ca68e87eb06971826087af59adb33e490ce7da
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 09/02/2020
ms.locfileid: "89362676"
---
# <a name="effects-for-video-capture"></a>비디오 캡처에 대 한 효과


이 항목에서는 카메라 미리 보기 및 녹화 비디오 스트림에 효과를 적용 하는 방법을 보여 주고, 비디오 안정화 효과를 사용 하는 방법을 보여 줍니다.

> [!NOTE] 
> 이 문서는 기본 사진 및 비디오 캡처를 구현 하는 단계를 설명 하는 [MediaCapture을 사용 하 여 기본 사진, 비디오 및 오디오 캡처](basic-photo-video-and-audio-capture-with-MediaCapture.md)에 설명 된 개념 및 코드를 기반으로 합니다. 고급 캡처 시나리오로 전환 하기 전에 해당 문서의 기본적인 미디어 캡처 패턴을 숙지 하는 것이 좋습니다. 이 문서의 코드는 앱에 이미 올바르게 초기화 된 MediaCapture 인스턴스가 있다고 가정 합니다.

## <a name="adding-and-removing-effects-from-the-camera-video-stream"></a>카메라 비디오 스트림에서 효과 추가 및 제거
장치의 카메라에서 비디오를 캡처하거나 미리 보려면 [MediaCapture를 사용 하 여 기본 사진, 비디오 및 오디오 캡처](basic-photo-video-and-audio-capture-with-MediaCapture.md)에 설명 된 대로 [**MediaCapture**](/uwp/api/Windows.Media.Capture.MediaCapture) 개체를 사용 합니다. **MediaCapture** 개체를 초기화 한 후 [**AddVideoEffectAsync**](/uwp/api/windows.media.capture.mediacapture.addvideoeffectasync)를 호출 하 여 미리 보기 또는 캡처 스트림에 하나 이상의 비디오 효과를 추가 하 고, 추가할 효과를 나타내는 [**IVideoEffectDefinition**](/uwp/api/Windows.Media.Effects.IVideoEffectDefinition) 개체를 전달 하 고, 효과를 카메라의 미리 보기 스트림 또는 레코드 스트림에 추가할지 여부를 나타내는 [**mediastreamtype**](/uwp/api/Windows.Media.Capture.MediaStreamType) 열거형의 멤버를 추가할 수 있습니다.

> [!NOTE]
> 일부 장치에서는 미리 보기 스트림과 캡처 스트림이 동일 합니다. 즉, **AddVideoEffectAsync**를 호출할 때 **VideoPreview** 또는 **mediastreamtype. VideoRecord** 을 지정 하면이 효과는 미리 보기 및 레코드 스트림에 적용 됩니다. **MediaCapture** 개체에 대 한 [**MediaCaptureSettings**](/uwp/api/windows.media.capture.mediacapture.mediacapturesettings) 의 [**VideoDeviceCharacteristic**](/uwp/api/windows.media.capture.mediacapturesettings.videodevicecharacteristic) 속성을 확인 하 여 미리 보기 및 레코드 스트림이 현재 장치에서 동일한 지 여부를 확인할 수 있습니다. 이 속성의 값이 **VideoDeviceCharacteristic** 인 **경우에는**스트림이 동일 하 게 적용 되며 하나에 적용 하는 모든 효과는 다른에 영향을 줍니다.

다음 예에서는 카메라 미리 보기 및 레코드 스트림에 효과를 추가 합니다. 이 예에서는 레코드와 미리 보기 스트림이 동일한 지 여부를 확인 하는 방법을 보여 줍니다.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/SimpleCameraPreview_Win10/cs/MainPage.Effects.xaml.cs" id="SnippetBasicAddEffect":::

**AddVideoEffectAsync** 는 추가 된 비디오 효과를 나타내는 [**imediaextension**](/uwp/api/Windows.Media.IMediaExtension) 을 구현 하는 개체를 반환 합니다. 일부 효과를 사용 하면 [**SetProperties**](/uwp/api/windows.media.imediaextension.setproperties) 메서드에 [**PropertySet**](/uwp/api/Windows.Foundation.Collections.PropertySet) 를 전달 하 여 효과 설정을 변경할 수 있습니다.

Windows 10 버전 1607부터 **AddVideoEffectAsync** 에서 반환 된 개체를 사용 하 여 비디오 파이프라인에서 효과를 [**RemoveEffectAsync**](/uwp/api/windows.media.capture.mediacapture.removeeffectasync)에 전달 하 여 제거할 수도 있습니다. **RemoveEffectAsync** 는 효과 개체 매개 변수가 미리 보기 또는 record 스트림에 추가 되었는지 여부를 자동으로 확인 하므로 호출할 때 스트림 유형을 지정할 필요가 없습니다.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/SimpleCameraPreview_Win10/cs/MainPage.Effects.xaml.cs" id="SnippetRemoveOneEffect":::

[**ClearEffectsAsync**](/uwp/api/windows.media.capture.mediacapture.cleareffectsasync) 를 호출 하 고 모든 효과를 제거 해야 하는 스트림을 지정 하 여 미리 보기 또는 캡처 스트림에서 모든 효과를 제거할 수도 있습니다.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/SimpleCameraPreview_Win10/cs/MainPage.Effects.xaml.cs" id="SnippetClearAllEffects":::

## <a name="video-stabilization-effect"></a>비디오 안정화 효과

비디오 안정화 효과는 비디오 스트림의 프레임을 조작 하 여 손으로 캡처 장치를 보유 하 여 발생 하는 핸드쉐이킹을 최소화 합니다. 이 기술로 인해 픽셀은 오른쪽, 왼쪽, 위쪽 및 아래쪽으로 이동 하 고 효과는 비디오 프레임 외부의 콘텐츠를 알 수 없으므로 안정화 된 비디오는 원래 비디오에서 약간 잘립니다. 비디오 인코딩 설정을 조정 하 여 효과에 의해 수행 되는 자르기를 최적으로 관리 하는 데 사용할 수 있는 유틸리티 함수가 제공 됩니다.

이를 지 원하는 장치에서 캡처 장치를 기계적으로 조작 하 여 OIS (광학 이미지 안정화)가 안정화 된 비디오 이므로 비디오 프레임의 가장자리를 자를 필요가 없습니다. 자세한 내용은 [비디오 캡처를 위한 장치 컨트롤 캡처](capture-device-controls-for-video-capture.md)를 참조 하세요.

### <a name="set-up-your-app-to-use-video-stabilization"></a>비디오 안정화를 사용 하도록 앱 설정

기본 미디어 캡처에 필요한 네임 스페이스 외에도 비디오 안정화 효과를 사용 하려면 다음 네임 스페이스가 필요 합니다.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/SimpleCameraPreview_Win10/cs/MainPage.Effects.xaml.cs" id="SnippetVideoStabilizationEffectUsing":::

[**VideoStabilizationEffect**](/uwp/api/Windows.Media.Core.VideoStabilizationEffect) 개체를 저장 하는 멤버 변수를 선언 합니다. 효과 구현의 일부로 캡처된 비디오를 인코딩하는 데 사용 하는 인코딩 속성을 수정 합니다. 두 변수를 선언 하 여 초기 입력 및 출력 인코딩 속성의 백업 복사본을 저장 하 고 나중에 효과를 해제할 때 복원할 수 있도록 합니다. 마지막으로,이 개체는 코드 내의 여러 위치에서 액세스 되기 때문에 [**MediaEncodingProfile**](/uwp/api/Windows.Media.MediaProperties.MediaEncodingProfile) 형식의 멤버 변수를 선언 합니다.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/SimpleCameraPreview_Win10/cs/MainPage.Effects.xaml.cs" id="SnippetDeclareVideoStabilizationEffect":::

이 시나리오에서는 나중에 액세스할 수 있도록 미디어 인코딩 프로필 개체를 멤버 변수에 할당 해야 합니다.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/SimpleCameraPreview_Win10/cs/MainPage.Effects.xaml.cs" id="SnippetEncodingProfileMember":::

### <a name="initialize-the-video-stabilization-effect"></a>비디오 안정화 효과를 초기화 합니다.

**MediaCapture** 개체가 초기화 된 후 [**VideoStabilizationEffectDefinition**](/uwp/api/Windows.Media.Core.VideoStabilizationEffectDefinition) 개체의 새 인스턴스를 만듭니다. [**MediaCapture**](/uwp/api/windows.media.capture.mediacapture.addvideoeffectasync) 를 호출 하 여 비디오 파이프라인에 효과를 추가 하 고 [**VideoStabilizationEffect**](/uwp/api/Windows.Media.Core.VideoStabilizationEffect) 클래스의 인스턴스를 검색 합니다. 비디오 레코드 스트림에 효과를 적용 해야 함을 나타내려면 [**Mediastreamtype**](/uwp/api/Windows.Media.Capture.MediaStreamType) 을 지정 합니다.

[**EnabledChanged**](/uwp/api/windows.media.core.videostabilizationeffect.enabledchanged) 이벤트에 대 한 이벤트 처리기를 등록 하 고 도우미 메서드 **SetUpVideoStabilizationRecommendationAsync**를 호출 합니다 .이 두 가지 방법에 대해서는이 문서의 뒷부분에서 설명 합니다. 마지막으로 효과를 사용 하도록 설정 하려면 효과의 [**Enabled**](/uwp/api/windows.media.core.videostabilizationeffect.enabled) 속성을 true로 설정 합니다.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/SimpleCameraPreview_Win10/cs/MainPage.Effects.xaml.cs" id="SnippetCreateVideoStabilizationEffect":::

### <a name="use-recommended-encoding-properties"></a>권장 인코딩 속성 사용

이 문서 앞부분에서 설명한 것 처럼 비디오 안정화 효과에서 사용 하는 기술은 안정화 비디오가 원본 비디오에서 약간 잘라내는 것을 의미 합니다. 비디오 인코딩 속성을 조정 하 여 효과에 대 한 이러한 제한을 최적으로 처리 하려면 코드에 다음 도우미 함수를 정의 합니다. 이 단계는 비디오 안정화 효과를 사용 하기 위해 반드시 필요한 것은 아니지만이 단계를 수행 하지 않으면 결과 비디오가 약간 확장 되 고 따라서 시각적 충실도는 약간 낮습니다.

비디오 안정화 효과 인스턴스에서 [**GetRecommendedStreamConfiguration**](/uwp/api/windows.media.core.videostabilizationeffect.getrecommendedstreamconfiguration) 를 호출 하 고, [**VideoDeviceController**](/uwp/api/Windows.Media.Devices.VideoDeviceController) 개체를 전달 하 여 현재 입력 스트림 인코딩 속성에 대 한 영향을 알리고, **MediaEncodingProfile** 을 호출 하 여 현재 출력 인코딩 속성을 확인할 수 있습니다. 이 메서드는 새로운 권장 입력 및 출력 스트림 인코딩 속성을 포함 하는 [**Videostreamconfiguration**](/uwp/api/Windows.Media.Capture.VideoStreamConfiguration) 개체를 반환 합니다.

권장 되는 입력 인코딩 속성은 장치에서 지원 되는 경우 효과의 자르기가 적용 된 후 해상도를 최소화 하기 위해 제공 된 초기 설정 보다 높은 해상도를 제공 하는 것입니다.

[**VideoDeviceController**](/uwp/api/windows.media.devices.videodevicecontroller.setmediastreampropertiesasync) 를 호출 하 여 새 인코딩 속성을 설정 합니다. 새 속성을 설정 하기 전에 멤버 변수를 사용 하 여 초기 인코딩 속성을 저장 합니다. 그러면 효과를 해제할 때 설정을 다시 변경할 수 있습니다.

비디오 안정화 효과가 출력 비디오를 잘라내는 것이 좋습니다. 권장 되는 출력 인코딩 속성은 잘린 비디오의 크기입니다. 즉, 출력 해상도는 잘린 비디오 크기와 일치 하 게 됩니다. 권장 출력 속성을 사용 하지 않는 경우 비디오는 초기 출력 크기와 일치 하도록 확장 되며이로 인해 시각적 충실도가 손실 됩니다.

**MediaEncodingProfile** 개체의 [**Video**](/uwp/api/windows.media.mediaproperties.mediaencodingprofile.video) 속성을 설정 합니다. 새 속성을 설정 하기 전에 멤버 변수를 사용 하 여 초기 인코딩 속성을 저장 합니다. 그러면 효과를 해제할 때 설정을 다시 변경할 수 있습니다.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/SimpleCameraPreview_Win10/cs/MainPage.Effects.xaml.cs" id="SnippetSetUpVideoStabilizationRecommendationAsync":::

### <a name="handle-the-video-stabilization-effect-being-disabled"></a>사용 하지 않도록 설정 된 비디오 안정화 효과를 처리 합니다.

픽셀 처리량이 너무 높으면 처리할 효과가 없거나 효과가 느리게 실행 되는 것을 감지 하는 경우 시스템에서 자동으로 비디오 안정화 효과를 사용 하지 않도록 설정할 수 있습니다. 이 문제가 발생 하면 EnabledChanged 이벤트가 발생 합니다. *Sender* 매개 변수의 **VideoStabilizationEffect** 인스턴스는 효과의 새 상태 (사용 또는 사용 안 함)를 나타냅니다. [**VideoStabilizationEffectEnabledChangedEventArgs**](/uwp/api/Windows.Media.Core.VideoStabilizationEffectEnabledChangedEventArgs) 에는 효과를 사용 하거나 사용 하지 않는 이유를 나타내는 [**VideoStabilizationEffectEnabledChangedReason**](/uwp/api/Windows.Media.Core.VideoStabilizationEffectEnabledChangedReason) 값이 있습니다. 이 이벤트는 프로그래밍 방식으로 효과를 사용 하거나 사용 하지 않도록 설정 하는 경우에도 발생 합니다 .이 경우에는이 이유가 **프로그래밍**방식입니다.

일반적으로이 이벤트를 사용 하 여 앱의 UI를 조정 하 여 비디오 안정화의 현재 상태를 표시 합니다.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/SimpleCameraPreview_Win10/cs/MainPage.Effects.xaml.cs" id="SnippetVideoStabilizationEnabledChanged":::

### <a name="clean-up-the-video-stabilization-effect"></a>비디오 안정화 효과 정리

비디오 안정화 효과를 정리 하려면 [**RemoveEffectAsync**](/uwp/api/windows.media.capture.mediacapture.removeeffectasync) 을 호출 하 여 비디오 파이프라인에서 효과를 제거 합니다. 초기 인코딩 속성을 포함 하는 멤버 변수가 null이 아닌 경우이를 사용 하 여 인코딩 속성을 복원 합니다. 마지막으로 **EnabledChanged** 이벤트 처리기를 제거 하 고 효과를 null로 설정 합니다.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/SimpleCameraPreview_Win10/cs/MainPage.Effects.xaml.cs" id="SnippetCleanUpVisualStabilizationEffect":::

## <a name="related-topics"></a>관련 항목

* [카메라](camera.md)
* [MediaCapture를 사용하여 기본적인 사진, 비디오 및 오디오 캡처](basic-photo-video-and-audio-capture-with-MediaCapture.md)
 

 
