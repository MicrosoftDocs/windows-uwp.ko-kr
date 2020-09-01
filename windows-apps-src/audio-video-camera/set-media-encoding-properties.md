---
ms.assetid: 09BA9250-A476-4803-910E-52F0A51704B1
description: 이 문서에서는 IMediaEncodingProperties 인터페이스를 사용 하 여 카메라 미리 보기 스트림과 캡처한 사진 및 비디오의 해상도 및 프레임 주기를 설정 하는 방법을 보여 줍니다.
title: MediaCapture용 형식, 해상도 및 프레임 속도 선택
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 388d1bc2af9d39d08087c7ec5b9dcbc710e74bba
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89163577"
---
# <a name="set-format-resolution-and-frame-rate-for-mediacapture"></a>MediaCapture용 형식, 해상도 및 프레임 속도 선택



이 문서에서는 [**IMediaEncodingProperties**](/uwp/api/Windows.Media.MediaProperties.IMediaEncodingProperties) 인터페이스를 사용 하 여 카메라 미리 보기 스트림과 캡처한 사진 및 비디오의 해상도 및 프레임 주기를 설정 하는 방법을 보여 줍니다. 또한 미리 보기 스트림의 가로 세로 비율이 캡처된 미디어와 일치 하는지 확인 하는 방법도 보여 줍니다.

카메라 프로필은 카메라의 스트림 속성을 검색 하 고 설정 하는 고급 방법을 제공 하지만 모든 장치에 대해 지원 되지 않습니다. 자세한 내용은 [카메라 프로필](camera-profiles.md)을 참조 하세요.

이 문서의 코드는 [CameraResolution 샘플](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/CameraResolution)에서 도입 되었습니다. 샘플을 다운로드 하 여 컨텍스트에서 사용 되는 코드를 확인 하거나 사용자 고유의 앱에 대 한 시작 지점으로 샘플을 사용할 수 있습니다.

> [!NOTE] 
> 이 문서는 기본 사진 및 비디오 캡처를 구현 하는 단계를 설명 하는 [MediaCapture을 사용 하 여 기본 사진, 비디오 및 오디오 캡처](basic-photo-video-and-audio-capture-with-MediaCapture.md)에 설명 된 개념 및 코드를 기반으로 합니다. 고급 캡처 시나리오로 전환 하기 전에 해당 문서의 기본적인 미디어 캡처 패턴을 숙지 하는 것이 좋습니다. 이 문서의 코드는 앱에 이미 올바르게 초기화 된 MediaCapture 인스턴스가 있다고 가정 합니다.

## <a name="a-media-encoding-properties-helper-class"></a>미디어 인코딩 속성 도우미 클래스

[**IMediaEncodingProperties**](/uwp/api/Windows.Media.MediaProperties.IMediaEncodingProperties) 인터페이스의 기능을 래핑하는 간단한 도우미 클래스를 만들면 특정 조건을 충족 하는 인코딩 속성 집합을 쉽게 선택할 수 있습니다. 이 도우미 클래스는 인코딩 속성 기능의 다음 동작 때문에 특히 유용 합니다.

**경고**    [**VideoDeviceController**](/uwp/api/windows.media.devices.videodevicecontroller.getavailablemediastreamproperties) 메서드는 **VideoRecord** 또는 **Photo**와 같은 [**mediastreamtype**](/uwp/api/Windows.Media.Capture.MediaStreamType) 열거의 멤버를 사용 하 고 캡처된 사진 또는 비디오의 해상도와 같이 스트림 인코딩 설정을 전달 하는 [**ImageEncodingProperties**](/uwp/api/Windows.Media.MediaProperties.ImageEncodingProperties) 또는 [**VideoEncodingProperties**](/uwp/api/Windows.Media.MediaProperties.VideoEncodingProperties) 개체의 목록을 반환 합니다. **GetAvailableMediaStreamProperties** 호출 결과에는 지정 된 **mediastreamtype** 값에 관계 없이 **ImageEncodingProperties** 또는 **VideoEncodingProperties** 가 포함 될 수 있습니다. 따라서 항상 반환 된 각 값의 형식을 확인 하 고 속성 값에 액세스 하기 전에 적절 한 형식으로 캐스팅 해야 합니다.

아래에 정의 된 도우미 클래스는 [**ImageEncodingProperties**](/uwp/api/Windows.Media.MediaProperties.ImageEncodingProperties) 또는 [**VideoEncodingProperties**](/uwp/api/Windows.Media.MediaProperties.VideoEncodingProperties) 에 대 한 형식 검사 및 캐스트를 처리 하므로 앱 코드에서 두 형식을 구분할 필요가 없습니다. 이 외에도 도우미 클래스는 속성의 가로 세로 비율, 프레임 비율 (비디오 인코딩 속성의 경우에만 해당) 및 응용 프로그램의 UI에서 인코딩 속성을 더 쉽게 표시할 수 있는 친숙 한 이름에 대 한 속성을 노출 합니다.

도우미 클래스의 소스 파일에는 [**Windows. MediaProperties**](/uwp/api/Windows.Media.MediaProperties) 네임 스페이스를 포함 해야 합니다.

[!code-cs[MediaEncodingPropertiesUsing](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetMediaEncodingPropertiesUsing)]

[!code-cs[StreamPropertiesHelper](./code/BasicMediaCaptureWin10/cs/StreamPropertiesHelper.cs#SnippetStreamPropertiesHelper)]

## <a name="determine-if-the-preview-and-capture-streams-are-independent"></a>미리 보기 및 캡처 스트림이 독립적인 지 확인

일부 장치에서는 미리 보기 스트림과 캡처 스트림 모두에 동일한 하드웨어 pin이 사용 됩니다. 이러한 장치에서 한 인코딩 속성을 설정 하는 것도 다른 장치를 설정 합니다. 캡처 및 미리 보기에 대해 다른 하드웨어 pin을 사용 하는 장치에서는 각 스트림에 대해 독립적으로 속성을 설정할 수 있습니다. 미리 보기 및 캡처 스트림이 독립적인 지 확인 하려면 다음 코드를 사용 합니다. 이 테스트의 결과에 따라 독립적으로 스트림 설정을 사용 하거나 사용 하지 않도록 UI를 조정 해야 합니다.

[!code-cs[CheckIfStreamsAreIdentical](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetCheckIfStreamsAreIdentical)]

## <a name="get-a-list-of-available-stream-properties"></a>사용 가능한 스트림 속성 목록 가져오기

앱의 [MediaCapture](./index.md) 개체에 대 한 [**VideoDeviceController**](/uwp/api/Windows.Media.Devices.VideoDeviceController) 을 가져온 다음 [**GetAvailableMediaStreamProperties**](/uwp/api/windows.media.devices.videodevicecontroller.getavailablemediastreamproperties) 를 호출 하 고 [**mediastreamtype**](/uwp/api/Windows.Media.Capture.MediaStreamType) 값, **VideoPreview**, **VideoRecord**또는 **Photo**중 하나를 전달 하 여 캡처 장치에 사용할 수 있는 스트림 속성의 목록을 가져옵니다. 이 예제에서는 Linq 구문을 사용 하 여 **GetAvailableMediaStreamProperties**에서 반환 된 각 IMediaEncodingProperties 값에 대해이 문서 앞에서 정의 된 **stream 도우미** 개체 목록을 [**IMediaEncodingProperties**](/uwp/api/Windows.Media.MediaProperties.IMediaEncodingProperties) 만듭니다. 이 예제에서는 먼저 Linq 확장 메서드를 사용 하 여 먼저 해상도에 따라 반환 된 속성을 정렬 한 다음 프레임 속도로 정렬 합니다.

앱에 특정 해상도 또는 프레임 요금 요구 사항이 있는 경우 프로그래밍 방식으로 미디어 인코딩 속성 집합을 선택할 수 있습니다. 표준 카메라 앱은 대신 UI에서 사용 가능한 속성 목록을 표시 하 고 사용자가 원하는 설정을 선택할 수 있습니다. 목록에서 **StreamComboBoxItem helper** 개체 목록의 각 항목에 대 한 **ComboBoxItem** 생성 됩니다. 콘텐츠는 도우미 클래스에서 반환 되는 이름으로 설정 되 고 태그는 도우미 클래스 자체로 설정 되므로 나중에 연결 된 인코딩 속성을 검색 하는 데 사용할 수 있습니다. 그런 다음 메서드에 전달 된 **ComboBox** 에 각 **ComboBoxItem** 추가 됩니다.

[!code-cs[PopulateStreamPropertiesUI](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetPopulateStreamPropertiesUI)]

## <a name="set-the-desired-stream-properties"></a>원하는 스트림 속성 설정

[**SetMediaStreamPropertiesAsync**](/uwp/api/windows.media.devices.videodevicecontroller.setmediastreampropertiesasync)를 호출 하 여 원하는 인코딩 속성을 사용 하도록 비디오 장치 컨트롤러에 지시 하 고 photo, video 또는 preview 속성을 설정 해야 하는지 여부를 나타내는 **mediastreamtype** 값을 전달 합니다. 이 예제에서는 사용자가 **PopulateStreamPropertiesUI** helper 메서드로 채워진 **ComboBox** 개체 중 하나에서 항목을 선택할 때 요청 된 인코딩 속성을 설정 합니다.

[!code-cs[PreviewSettingsChanged](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetPreviewSettingsChanged)]

[!code-cs[PhotoSettingsChanged](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetPhotoSettingsChanged)]

[!code-cs[VideoSettingsChanged](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetVideoSettingsChanged)]

## <a name="match-the-aspect-ratio-of-the-preview-and-capture-streams"></a>미리 보기 및 캡처 스트림의 가로 세로 비율과 일치

일반적인 카메라 앱은 사용자가 비디오 또는 사진 캡처 해상도를 선택할 수 있는 UI를 제공 하지만 프로그래밍 방식으로 미리 보기 해상도를 설정 합니다. 앱에 가장 적합 한 미리 보기 스트림 해상도를 선택 하기 위한 몇 가지 전략이 있습니다.

-   사용 가능한 가장 높은 미리 보기 해상도를 선택 하면 UI 프레임 워크가 미리 보기의 필요한 크기 조정을 수행할 수 있습니다.

-   캡처 해상도와 가장 가까운 미리 보기 해상도를 선택 하 여 미리 보기에 캡처된 최종 미디어에 가장 가까운 표현이 표시 되도록 합니다.

-   [**CaptureElement**](/uwp/api/Windows.UI.Xaml.Controls.CaptureElement) 크기에 가장 가까운 미리 보기 해상도를 선택 하 여 필요한 것 보다 많은 픽셀이 미리 보기 스트림 파이프라인을 통과 하지 않도록 합니다.

**중요**    일부 장치에서는 카메라의 미리 보기 스트림과 캡처 스트림에 대해 다른 가로 세로 비율을 설정할 수 있습니다. 이 불일치로 인해 프레임을 자르는 경우 미리 보기에 표시 되지 않는 콘텐츠가 캡처된 미디어에 표시 될 수 있으며,이로 인해 사용자 환경이 저하 될 수 있습니다. 미리 보기 및 캡처 스트림에는 작은 허용 오차 창 내에서 동일한 가로 세로 비율을 사용 하는 것이 좋습니다. 가로 세로 비율이 가깝게 일치 하는 경우 캡처 및 미리 보기에 대해 완전히 다른 해결 방법을 사용할 수 있습니다.


사진 또는 비디오 캡처 스트림이 미리 보기 스트림의 가로 세로 비율과 일치 하는지 확인 하기 위해이 예제에서는 [**VideoDeviceController**](/uwp/api/windows.media.devices.videodevicecontroller.getmediastreamproperties) 를 호출 하 고 **VideoPreview** 열거형 값을 전달 하 여 미리 보기 스트림에 대 한 현재 스트림 속성을 요청 합니다. 다음에는 작은 가로 비율 허용 오차 창이 정의 되어 있으므로 미리 보기 스트림과 정확히 일치 하지 않는 가로 세로 비율을 포함할 수 있습니다. 그런 다음, Linq 확장 메서드는 가로 세로 비율이 미리 보기 스트림의 정의 된 허용 범위 내에 있는 **Streamobjects 도우미** 개체만 선택 하는 데 사용 됩니다.

[!code-cs[MatchPreviewAspectRatio](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetMatchPreviewAspectRatio)]

 

 