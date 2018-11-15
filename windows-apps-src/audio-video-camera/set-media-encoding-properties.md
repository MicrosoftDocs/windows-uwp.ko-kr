---
author: drewbatgit
ms.assetid: 09BA9250-A476-4803-910E-52F0A51704B1
description: 이 문서에서는 IMediaEncodingProperties 인터페이스를 사용하여 카메라 미리 보기 스트림과 캡처한 사진 및 동영상의 해상도 및 프레임 속도를 설정하는 방법을 보여 줍니다.
title: MediaCapture용 형식, 해상도 및 프레임 속도 선택
ms.author: drewbat
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: ba07f897111e27dc895aa187172841cac4b44f73
ms.sourcegitcommit: e38b334edb82bf2b1474ba686990f4299b8f59c7
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/15/2018
ms.locfileid: "6837866"
---
# <a name="set-format-resolution-and-frame-rate-for-mediacapture"></a>MediaCapture용 형식, 해상도 및 프레임 속도 선택



이 문서에서는 [**IMediaEncodingProperties**](https://msdn.microsoft.com/library/windows/apps/hh701011) 인터페이스를 사용하여 카메라 미리 보기 스트림과 캡처한 사진 및 비디오의 해상도 및 프레임 속도를 설정하는 방법을 보여 줍니다. 또한 미리 보기 스트림의 가로 세로 비율이 캡처한 미디어의 가로 세로 비율과 일치하는지 확인하는 방법을 보여 줍니다.

카메라 프로필은 카메라의 스트림 속성을 검색하고 설정하는 고급 방법을 추가로 제공하지만, 일부 장치는 카메라 프로필을 지원하지 않습니다. 자세한 내용은 [카메라 프로필](camera-profiles.md)을 참조하세요.

이 문서의 코드는 [CameraResolution 샘플](http://go.microsoft.com/fwlink/p/?LinkId=624252&clcid=0x409)에서 조정되었습니다. 샘플을 다운로드하여 상황에 맞게 사용되는 코드를 참조하거나 자체 앱을 처음 빌드하기 시작할 때 샘플을 사용할 수 있습니다.

> [!NOTE] 
> 이 문서는 기본 사진 및 비디오 캡처 구현 단계를 설명하는 [MediaCapture를 사용한 기본적인 사진, 비디오 및 오디오 캡처](basic-photo-video-and-audio-capture-with-MediaCapture.md)에 설명된 개념 및 코드를 토대로 작성되었습니다. 좀 더 수준 높은 캡처 시나리오를 진행하기 전에 해당 문서의 기본 미디어 캡처 패턴을 좀 더 잘 이해하는 것이 좋습니다. 이 문서의 코드는 앱에 적절히 초기화된 MediaCapture의 인스턴스가 이미 있다고 가정합니다.

## <a name="a-media-encoding-properties-helper-class"></a>미디어 인코딩 속성 도우미 클래스

[**IMediaEncodingProperties**](https://msdn.microsoft.com/library/windows/apps/hh701011) 인터페이스의 기능을 래핑하는 간단한 도우미 클래스를 만들면 특정 조건을 충족하는 인코딩 속성 집합을 선택하기가 쉬워집니다. 이 도우미 클래스는 다음과 같은 인코딩 속성의 동작 기능 때문에 특히 유용합니다.

**경고**  [**VideoDeviceController.GetAvailableMediaStreamProperties**](https://msdn.microsoft.com/library/windows/apps/br211994) 메서드 **VideoRecord** 또는 **사진**같은 [**MediaStreamType**](https://msdn.microsoft.com/library/windows/apps/br226640) 열거형의 멤버를 사용 하 고 두 [**의 목록을 반환 ImageEncodingProperties**](https://msdn.microsoft.com/library/windows/apps/hh700993) 또는 스트림 전달 하는 [**VideoEncodingProperties**](https://msdn.microsoft.com/library/windows/apps/hh701217) 개체 인코딩 캡처된 사진 또는 비디오의 해상도 같은 설정 합니다. **GetAvailableMediaStreamProperties** 호출의 결과에는 어떤 **MediaStreamType** 값을 지정했는지와 상관없이 **ImageEncodingProperties** 또는 **VideoEncodingProperties**가 포함될 수 있습니다. 이러한 이유로 항상 반환된 각 값의 형식을 확인하고 적절한 형식으로 캐스팅한 이후에 속성 값에 액세스해야 합니다.

아래에 정의된 도우미 클래스가 [**ImageEncodingProperties**](https://msdn.microsoft.com/library/windows/apps/hh700993) 또는 [**VideoEncodingProperties**](https://msdn.microsoft.com/library/windows/apps/hh701217)에 대한 형식 확인 및 캐스팅을 처리하므로 앱 코드가 두 형식을 구별하지 않아도 됩니다. 이 외에도 도우미 클래스는 속성의 가로 세로 비율, 프레임 속도(비디오 인코딩 속성에만 해당) 및 앱의 UI에서 인코딩 속성을 더 쉽게 표시하는 데 활용되는 식별 이름에 대한 속성을 표시합니다.

도우미 클래스의 소스 파일에 [**Windows.Media.MediaProperties**](https://msdn.microsoft.com/library/windows/apps/hh701296) 네임스페이스를 포함해야 합니다.

[!code-cs[MediaEncodingPropertiesUsing](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetMediaEncodingPropertiesUsing)]

[!code-cs[StreamPropertiesHelper](./code/BasicMediaCaptureWin10/cs/StreamPropertiesHelper.cs#SnippetStreamPropertiesHelper)]

## <a name="determine-if-the-preview-and-capture-streams-are-independent"></a>미리 보기 및 캡처 스트림이 독립적인지 확인

일부 장치에서는 미리 보기와 캡처 스트림에 동일한 하드웨어 핀이 사용됩니다. 이러한 장치에서는 하나의 인코딩 속성을 설정하면 다른 하나도 설정됩니다. 캡처와 미리 보기에 다른 하드웨어 핀을 사용하는 장치에서는 각 스트림에 대해 속성을 독립적으로 설정할 수 있습니다. 미리 보기 및 캡처 스트림이 독립적인지 확인하려면 다음 코드를 사용하세요. 이 테스트의 결과에 따라 스트림 설정을 독립적으로 사용하거나 사용하지 않으려면 UI를 조정해야 합니다.

[!code-cs[CheckIfStreamsAreIdentical](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetCheckIfStreamsAreIdentical)]

## <a name="get-a-list-of-available-stream-properties"></a>사용 가능한 스트림 속성 목록 가져오기

앱의 [MediaCapture](https://msdn.microsoft.com/library/windows/apps/br226825) 개체에 대한 [**VideoDeviceController**](capture-photos-and-video-with-mediacapture.md)를 가져온 후 [**GetAvailableMediaStreamProperties**](https://msdn.microsoft.com/library/windows/apps/br211994)를 호출하고 [**MediaStreamType**](https://msdn.microsoft.com/library/windows/apps/br226640) 값 중 하나에서 **VideoPreview**, **VideoRecord** 또는 **Photo**를 전달함으로써 캡처 장치에 대해 사용 가능한 스트림 속성 목록 가져옵니다. 이 예제에서는 Linq 구문이 **GetAvailableMediaStreamProperties**에서 반환된 각각의 [**IMediaEncodingProperties**](https://msdn.microsoft.com/library/windows/apps/hh701011) 값에 대해 **StreamPropertiesHelper** 개체(이 문서의 앞에 정의되어 있음)의 목록을 만드는 데 사용됩니다. 이 예제에서는 먼저, Linq 확장 메서드를 사용하여 해상도에 따라 반환되는 속성을 정렬한 후 프레임 속도에 따라 반환되는 속성을 순서대로 정렬합니다.

앱에 특정 해상도 또는 프레임 속도 요구 사항이 있는 경우 미디어 인코딩 속성 집합을 프로그래밍 방식으로 선택할 수 있습니다. 그러나 일반적인 카메라 앱은 UI에 사용 가능한 속성 목록을 표시하여 사용자가 원하는 설정을 선택할 수 있도록 합니다. 목록의 **StreamPropertiesHelper** 개체 목록에서 각 항목에 대해 **ComboBoxItem**을 생성합니다. 연결된 인코딩 속성을 검색하는 데 나중에 사용할 수 있도록 콘텐츠는 도우미 클래스를 통해 반환된 식별 이름으로 설정되고 태그는 도우미 클래스 자체로 설정됩니다. 그런 다음 메서드로 전달된 **ComboBox**에 각 **ComboBoxItem**을 추가합니다.

[!code-cs[PopulateStreamPropertiesUI](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetPopulateStreamPropertiesUI)]

## <a name="set-the-desired-stream-properties"></a>원하는 스트림 속성 설정

[**SetMediaStreamPropertiesAsync**](https://msdn.microsoft.com/library/windows/apps/hh700895)를 호출하고 사진, 동영상 또는 미리 보기 속성을 설정해야 하는지 여부를 나타내는 **MediaStreamType** 값을 전달함으로써 원하는 인코딩 속성을 사용하도록 비디오 장치 컨트롤러에 알려줍니다. 이 예제에서는 사용자가 **PopulateStreamPropertiesUI** 도우미 메서드로 채워진 **ComboBox** 개체 중 하나에서 항목을 선택할 때 요청된 인코딩 속성을 설정합니다.

[!code-cs[PreviewSettingsChanged](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetPreviewSettingsChanged)]

[!code-cs[PhotoSettingsChanged](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetPhotoSettingsChanged)]

[!code-cs[VideoSettingsChanged](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetVideoSettingsChanged)]

## <a name="match-the-aspect-ratio-of-the-preview-and-capture-streams"></a>미리 보기 및 캡처 스트림의 가로 세로 비율과 일치

일반적인 카메라 앱은 사용자가 동영상 또는 사진 캡처 해상도를 선택할 수 있는 UI를 제공하지만 프로그래밍 방식으로 미리 보기 해상도를 설정합니다. 앱에 가장 적합한 미리 보기 스트림 해상도를 선택하기 위한 몇 가지 전략이 있습니다.

-   사용할 수 있는 가장 높은 미리 보기 해상도를 선택하여 UI 프레임워크에서 필요에 따라 미리 보기 크기를 조정할 수 있도록 합니다.

-   미리 보기에 최종 캡처된 미디어에 가장 가까운 표현이 표시되도록 캡처 해상도에 가장 가까운 미리 보기 해상도를 선택합니다.

-   필요 이상의 픽셀이 미리 보기 스트림 파이프라인을 통과하지 않도록 [**CaptureElement**](https://msdn.microsoft.com/library/windows/apps/br209278)의 크기에 가장 가까운 미리 보기 해상도를 선택합니다.

**중요 한**  것 일부 장치에서는 카메라의 미리 보기 스트림에 대 한 다른 가로 세로 비율을 설정 하 여 캡처 스트림에 가능 합니다. 이러한 불일치 때문에 발생하는 프레임 자르기로 인해 미리 보기에는 표시되지 않았던 콘텐츠가 캡처된 미디어에 표시되어 부정적인 사용자 환경을 야기할 수 있습니다. 따라서 허용 오차 범위를 작게 해서 동일한 가로 세로 비율을 미리 보기 및 캡처 스트림에 사용하는 것이 좋습니다. 가로 세로 비율이 거의 일치하는 한 캡처 및 미리 보기에 완전히 다른 해상도를 사용하는 것도 괜찮습니다.


사진 또는 동영상 캡처 스트림이 미리 보기 스트림의 가로 세로 비율과 일치하도록 보장하기 위해 이 예제에서는 [**VideoDeviceController.GetMediaStreamProperties**](https://msdn.microsoft.com/library/windows/apps/br211995)를 호출하고 **VideoPreview** 열거형 값을 전달하여 미리 보기 스트림에 대한 현재 스트림 속성을 요청합니다. 그런 다음 미리 보기 스트림과 정확하게 같지는 않지만 거의 일치하는 가로 세로 비율을 포함할 수 있도록 가로 세로 비율의 허용 오차를 작게 정의합니다. 이제 Linq 확장 메서드를 사용하여 가로 세로 비율이 미리 보기 스트림의 정의된 허용 범위 내에 있는 **StreamPropertiesHelper** 개체만 선택합니다.

[!code-cs[MatchPreviewAspectRatio](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetMatchPreviewAspectRatio)]

 

 




