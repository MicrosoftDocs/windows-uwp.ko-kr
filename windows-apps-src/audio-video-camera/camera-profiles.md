---
ms.assetid: 42A06423-670F-4CCC-88B7-3DCEEDDEBA57
description: 이 문서에서는 카메라 프로필을 사용 하 여 다양 한 비디오 캡처 장치의 기능을 검색 하 고 관리 하는 방법을 설명 합니다. 여기에는 특정 해상도 또는 프레임 속도를 지 원하는 프로필 선택, 여러 카메라에 대 한 동시 액세스를 지 원하는 프로필 및 HDR를 지 원하는 프로필 등의 작업이 포함 됩니다.
title: 카메라 프로필을 사용하여 카메라 기능 검색 및 선택
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 08b5bda92535bc324589105ce37f4a6458c523be
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89161047"
---
# <a name="discover-and-select-camera-capabilities-with-camera-profiles"></a>카메라 프로필을 사용하여 카메라 기능 검색 및 선택



이 문서에서는 카메라 프로필을 사용 하 여 다양 한 비디오 캡처 장치의 기능을 검색 하 고 관리 하는 방법을 설명 합니다. 여기에는 특정 해상도 또는 프레임 속도를 지 원하는 프로필 선택, 여러 카메라에 대 한 동시 액세스를 지 원하는 프로필 및 HDR를 지 원하는 프로필 등의 작업이 포함 됩니다.

> [!NOTE] 
> 이 문서는 기본 사진 및 비디오 캡처를 구현 하는 단계를 설명 하는 [MediaCapture을 사용 하 여 기본 사진, 비디오 및 오디오 캡처](basic-photo-video-and-audio-capture-with-MediaCapture.md)에 설명 된 개념 및 코드를 기반으로 합니다. 고급 캡처 시나리오로 전환 하기 전에 해당 문서의 기본적인 미디어 캡처 패턴을 숙지 하는 것이 좋습니다. 이 문서의 코드는 앱에 이미 올바르게 초기화 된 MediaCapture 인스턴스가 있다고 가정 합니다.

 

## <a name="about-camera-profiles"></a>카메라 프로필 정보

서로 다른 장치의 카메라는 지원 되는 캡처 해상도 집합, 비디오 캡처의 프레임 요금 및 HDR 또는 가변 프레임 요금 캡처가 지원 되는지 여부를 비롯 한 다양 한 기능을 지원 합니다. UWP (유니버설 Windows 플랫폼) 미디어 캡처 프레임 워크는이 기능 집합을 [**MediaCaptureVideoProfileMediaDescription**](/uwp/api/Windows.Media.Capture.MediaCaptureVideoProfileMediaDescription)에 저장 합니다. [**MediaCaptureVideoProfile**](/uwp/api/Windows.Media.Capture.MediaCaptureVideoProfile) 개체로 표시 되는 카메라 프로필에는 세 가지 미디어 설명 모음이 있습니다. 하나는 사진 캡처, 비디오 캡처의 경우, 다른 하나는 비디오 미리 보기용입니다.

[MediaCapture](./index.md) 개체를 초기화 하기 전에 현재 장치에서 캡처 장치를 쿼리하여 어떤 프로필이 지원 되는지 확인할 수 있습니다. 지원 되는 프로필을 선택 하면 캡처 장치가 프로필의 미디어 설명에 있는 모든 기능을 지원 한다는 것을 알 수 있습니다. 이를 통해 특정 장치에서 지원 되는 기능의 조합을 결정 하는 데 필요한 평가 및 오류 방법이 제거 됩니다.

[!code-cs[BasicInitExample](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetBasicInitExample)]

이 문서의 코드 예제에서는이 최소 초기화를 다양 한 기능을 지 원하는 카메라 프로필의 검색으로 대체 합니다. 그러면 미디어 캡처 장치를 초기화 하는 데 사용 됩니다.

## <a name="find-a-video-device-that-supports-camera-profiles"></a>카메라 프로필을 지 원하는 비디오 장치 찾기

지원 되는 카메라 프로필을 검색 하기 전에 카메라 프로필의 사용을 지 원하는 캡처 장치를 찾아야 합니다. 아래 예제에 정의 된 **GetVideoProfileSupportedDeviceIdAsync** 도우미 메서드는 [**DeviceInformaion FindAllAsync**](/uwp/api/windows.devices.enumeration.deviceinformation.findallasync) 메서드를 사용 하 여 사용 가능한 모든 비디오 캡처 장치 목록을 검색 합니다. 목록에 있는 모든 장치를 반복 하 여 각 장치에 대해 비디오 프로필을 지원 하는지 여부를 확인 하는 정적 메서드인 [**Isvideoprofiles Up포팅**](/uwp/api/windows.media.capture.mediacapture.isvideoprofilesupported)를 호출 합니다. 또한 각 장치에 대 한 [**EnclosureLocation**](/uwp/api/windows.devices.enumeration.enclosurelocation.panel) 속성을 통해 장치의 전면 또는 후면에 카메라를 원하는 대로 지정할 수 있습니다.

지정 된 패널에서 카메라 프로필을 지 원하는 장치를 찾으면 장치의 ID 문자열이 포함 된 [**id**](/uwp/api/windows.devices.enumeration.deviceinformation.id) 값이 반환 됩니다.

[!code-cs[GetVideoProfileSupportedDeviceIdAsync](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetGetVideoProfileSupportedDeviceIdAsync)]

**GetVideoProfileSupportedDeviceIdAsync** helper 메서드에서 반환 된 장치 ID가 null 또는 빈 문자열인 경우 지정 된 패널에 카메라 프로필을 지 원하는 장치가 없습니다. 이 경우 프로필을 사용 하지 않고 미디어 캡처 장치를 초기화 해야 합니다.

[!code-cs[GetDeviceWithProfileSupport](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetGetDeviceWithProfileSupport)]

## <a name="select-a-profile-based-on-supported-resolution-and-frame-rate"></a>지원 되는 해상도 및 프레임 속도로 프로필을 선택 합니다.

특정 해상도 및 프레임 요금을 얻을 수 있는 기능과 같이 특정 기능을 가진 프로필을 선택 하려면 먼저 위에 정의 된 도우미 메서드를 호출 하 여 카메라 프로필 사용을 지 원하는 캡처 장치의 ID를 가져와야 합니다.

새 [**MediaCaptureInitializationSettings**](/uwp/api/Windows.Media.Capture.MediaCaptureInitializationSettings) 개체를 만들고 선택한 장치 ID를 전달 합니다. 그런 다음 정적 메서드 MediaCapture를 호출 하 여 장치에서 지 원하는 모든 카메라 프로필 목록을 가져옵니다 [**.**](/uwp/api/windows.media.capture.mediacapture.findallvideoprofiles)

이 예제 **에서는 using system.xml** 네임 스페이스에 포함 된 linq 쿼리 메서드를 사용 하 여 [**Width**](/uwp/api/windows.media.capture.mediacapturevideoprofilemediadescription.width), [**Height**](/uwp/api/windows.media.capture.mediacapturevideoprofilemediadescription.height)및 [**프레임 속도**](/uwp/api/windows.media.capture.mediacapturevideoprofilemediadescription.framerate) 속성이 요청 된 값과 일치 하는 [**supportedrecordmediadescription**](/uwp/api/windows.media.capture.mediacapturevideoprofile.supportedrecordmediadescription) 개체를 포함 하는 프로필을 선택 합니다. 일치 항목이 발견 되 면 **MediaCaptureInitializationSettings** 에 대 한 [**Videoprofile**](/uwp/api/windows.media.capture.mediacaptureinitializationsettings.videoprofile) 및 [**recordmediadescription**](/uwp/api/windows.media.capture.mediacaptureinitializationsettings.recordmediadescription) 이 Linq 쿼리에서 반환 된 무명 형식의 값으로 설정 됩니다. 일치 하는 항목이 없으면 기본 프로필이 사용 됩니다.

[!code-cs[FindWVGA30FPSProfile](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetFindWVGA30FPSProfile)]

원하는 카메라 프로필로 **MediaCaptureInitializationSettings** 을 채운 후에는 미디어 캡처 개체의 [**InitializeAsync**](/uwp/api/windows.media.capture.mediacapture.initializeasync) 를 호출 하 여 원하는 프로필로 구성 하면 됩니다.

[!code-cs[InitCaptureWithProfile](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetInitCaptureWithProfile)]

## <a name="use-media-frame-source-groups-to-get-profiles"></a>미디어 프레임 원본 그룹을 사용 하 여 프로필 가져오기

Windows 10, 버전 1803부터 **MediaCapture** 개체를 초기화 하기 전에 [**Mediaframesourcegroup**](/uwp/api/windows.media.capture.frames.mediaframesourcegroup) 클래스를 사용 하 여 특정 기능을 포함 하는 카메라 프로필을 가져올 수 있습니다. 프레임 원본 그룹을 사용 하면 장치 제조업체에서 센서의 그룹이 나 캡처 기능을 단일 가상 장치로 나타낼 수 있습니다. 이를 통해 깊이 및 컬러 카메라를 함께 사용 하는 것과 같은 계산 사진 시나리오를 사용할 수 있지만 간단한 캡처 시나리오에 대 한 카메라 프로필을 선택할 수도 있습니다. **Mediaframesourcegroup**사용에 대 한 자세한 내용은 [MediaFrameReader를 사용 하 여 미디어 프레임 처리](process-media-frames-with-mediaframereader.md)를 참조 하세요.

아래 예제 메서드는 **Mediaframesourcegroup** 개체를 사용 하 여 HDR 또는 변수 사진 시퀀스를 지 원하는 것과 같은 알려진 비디오 프로필을 지 원하는 카메라 프로필을 찾는 방법을 보여 줍니다. 먼저, [**MediaFrameSourceGroup.FindAllAsync**](/uwp/api/windows.media.capture.frames.mediaframesourcegroup.findallasync)를 호출하여 현재 디바이스에서 사용할 수 있는 전체 미디어 프레임 소스 그룹 목록을 가져옵니다. 각 소스 그룹을 반복 실행하고 [**MediaCapture.FindKnownVideoProfiles**](/uwp/api/windows.media.capture.mediacapture.findknownvideoprofiles)를 호출하여 지정된 프로필을 지원하는 현재 소스 그룹에 대한 모든 비디오 프로필 목록을 가져옵니다(이 경우는 WCG 사진이 있는 HDR). 기준을 충족하는 프로필이 발견되면 새로운 **MediaCaptureInitializationSettings** 개체를 만들고 **VideoProfile**을 프로필 선택으로 설정하고  **VideoDeviceId**를 현재 미디어 프레임 소스 그룹의 **Id** 속성으로 설정합니다. 따라서 예를 들어 **HdrWithWcgVideo** 값을이 메서드에 전달 하 여 HDR 비디오를 지 원하는 미디어 캡처 설정을 가져올 수 있습니다. VariablePhotoSequence를 전달 하 여 가변 사진 시퀀스를 지 원하는 설정을 가져옵니다 **.**

 [!code-cs[FindKnownVideoProfile](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetFindKnownVideoProfile)]

## <a name="use-known-profiles-to-find-a-profile-that-supports-hdr-video-legacy-technique"></a>알려진 프로필을 사용 하 여 HDR 비디오를 지 원하는 프로필 찾기 (레거시 기술)

> [!NOTE] 
> 이 섹션에서 설명 하는 Api는 Windows 10 버전 1803부터 사용 되지 않습니다. 이전 섹션인 **미디어 프레임 소스 그룹을 사용 하 여 프로필 가져오기**를 참조 하세요.

HDR를 지 원하는 프로필을 선택 하는 것은 다른 시나리오와 같은 방식으로 시작 됩니다. 캡처 장치 ID를 보관할 **MediaCaptureInitializationSettings** 및 문자열을 만듭니다. HDR 비디오가 지원 되는지 여부를 추적 하는 부울 변수를 추가 합니다.

[!code-cs[GetHdrProfileSetup](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetGetHdrProfileSetup)]

위에 정의 된 **GetVideoProfileSupportedDeviceIdAsync** 도우미 메서드를 사용 하 여 카메라 프로필을 지 원하는 캡처 장치에 대 한 장치 ID를 가져옵니다.

[!code-cs[FindDeviceHDR](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetFindDeviceHDR)]

정적 메서드인 [**MediaCapture**](/uwp/api/windows.media.capture.mediacapture.findknownvideoprofiles) 는 지정 된 [**knownvideoprofile**](/uwp/api/Windows.Media.Capture.KnownVideoProfile) 값으로 분류 된 지정 된 장치에서 지원 되는 카메라 프로필을 반환 합니다. 이 시나리오에서 **VideoRecording** 값은 반환 된 카메라 프로필을 비디오 녹화를 지 원하는 항목으로 제한 하기 위해 지정 됩니다.

반환 된 카메라 프로필 목록을 반복 합니다. 각 카메라 프로필에 대해 프로필 확인의 각 [**VideoProfileMediaDescription**](/uwp/api/Windows.Media.Capture.MediaCaptureVideoProfileMediaDescription) 를 반복 하 여 [**Ishdrvideosupported**](/uwp/api/windows.media.capture.mediacapturevideoprofilemediadescription.ishdrvideosupported) 속성이 true 인지 확인 합니다. 적절 한 미디어 설명을 찾았으면 루프에서 중단 하 고 **MediaCaptureInitializationSettings** 개체에 프로필 및 설명 개체를 할당 합니다.

[!code-cs[FindHDRProfile](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetFindHDRProfile)]

## <a name="determine-if-a-device-supports-simultaneous-photo-and-video-capture"></a>장치에서 동시 사진 및 비디오 캡처를 지원 하는지 확인

많은 장치에서 사진 및 비디오 캡처를 동시에 지원 합니다. 캡처 장치에서이를 지원 하는지 확인 하려면 [**MediaCapture. FindAllVideoProfiles**](/uwp/api/windows.media.capture.mediacapture.findallvideoprofiles) 를 호출 하 여 장치에서 지 원하는 모든 카메라 프로필을 가져옵니다. 링크 쿼리를 사용 하 여 [**supportedphotomediadescription**](/uwp/api/windows.media.capture.mediacapturevideoprofile.supportedphotomediadescription) 및 [**supportedphotomediadescription**](/uwp/api/windows.media.capture.mediacapturevideoprofile.supportedrecordmediadescription) 둘 다에 대 한 항목을 하나 이상 포함 하는 프로필을 찾을 수 있습니다 .이는 프로필에서 동시 캡처를 지원함을 의미 합니다.

[!code-cs[GetPhotoAndVideoSupport](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetGetPhotoAndVideoSupport)]

이 쿼리를 구체화 하 여 동시 비디오 레코드 외에 특정 해결 방법이 나 기타 기능을 지 원하는 프로필을 찾을 수 있습니다. [**MediaCapture**](/uwp/api/windows.media.capture.mediacapture.findknownvideoprofiles) 를 사용 하 여 동시 캡처를 지 원하는 프로필을 검색 하는 [**BalancedVideoAndPhoto**](/uwp/api/Windows.Media.Capture.KnownVideoProfile) 값을 지정할 수도 있지만 모든 프로필을 쿼리하면 전체 결과를 얻을 수 있습니다.

## <a name="related-topics"></a>관련 항목

* [카메라](camera.md)
* [MediaCapture를 사용하여 기본적인 사진, 비디오 및 오디오 캡처](basic-photo-video-and-audio-capture-with-MediaCapture.md)
 

 