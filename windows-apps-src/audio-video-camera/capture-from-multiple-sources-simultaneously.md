---
author: drewbatgit
ms.assetid: 
description: "이 문서에서는 여러 소스에서 동시에 비디오를 캡처하여, 다수의 비디오 트랙이 내장된 하나의 파일로 보내는 방법을 설명합니다."
title: "MediaFrameSourceGroup을 사용하여 다양한 소스로부터 캡처"
ms.author: drewbat
ms.date: 09/12/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: "windows 10, uwp, 캡처, 비디오"
ms.localizationpriority: medium
ms.openlocfilehash: 56996cf9edb5aef317421e5ca8eef05153328bf9
ms.sourcegitcommit: f9a4854b6aecfda472fb3f8b4a2d3b271b327800
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 12/12/2017
---
# <a name="capture-from-multiple-sources-using-mediaframesourcegroup"></a>MediaFrameSourceGroup을 사용하여 다양한 소스로부터 캡처

이 문서에서는 여러 소스에서 동시에 비디오를 캡처하여, 다수의 비디오 트랙이 내장된 하나의 파일로 보내는 방법을 설명합니다. RS3부터는 단일 **[MediaEncodingProfile](https://docs.microsoft.com/uwp/api/windows.media.mediaproperties.mediaencodingprofile)**에 여러 **[VideoStreamDescriptor](https://docs.microsoft.com/uwp/api/windows.media.core.videostreamdescriptor)** 개체를 지정할 수 있습니다. 이를 통해 여러 스트림을 동시에 하나의 파일로 인코딩할 수 있습니다. 이 작업에서 인코딩된 비디오 스트림은 동시에 사용할 수 있는 현재 장치의 카메라 세트를 지정하는 단일 **[MediaFrameSourceGroup](https://docs.microsoft.com/uwp/api/windows.media.capture.frames.mediaframesourcegroup)**에 포함되어야 합니다. 

**[MediaFrameReader](https://docs.microsoft.com/uwp/api/windows.media.capture.frames.mediaframereader)** 클래스와 함께 **[MediaFrameSourceGroup](https://docs.microsoft.com/uwp/api/windows.media.capture.frames.mediaframesourcegroup)**을 사용하여 여러 대의 카메라를 사용하는 실시간 컴퓨터 비전 시나리오를 활성화하는 방법에 대한 정보는 [MediaFrameReader를 사용하여 미디어 프레임 처리](process-media-frames-with-mediaframereader.md)를 참조하세요.

이 문서의 나머지 부분에서는 두 개의 컬러 카메라에서 여러 비디오 트랙이 있는 단일 파일로 비디오를 녹화하는 단계를 안내합니다.

## <a name="find-available-sensor-groups"></a>사용할 수 있는 센서 그룹 찾기
**MediaFrameSourceGroup**은 동시에 액세스할 수 있는 프레임 소스(일반적으로 카메라)의 모음을 나타냅니다. 사용 가능한 프레임 소스 그룹 집합은 각 장치마다 다르기 때문에 이 예의 첫 번째 단계는 사용 가능한 프레임 소스 그룹 목록을 가져오고 시나리오에 필요한 카메라가 들어있는 그룹을 찾는 것입니다. 후자의 경우에는 두 개의 컬러 카메라가 필요합니다.

**[MediaFrameSourceGroup.FindAllAsync](https://docs.microsoft.com/uwp/api/windows.media.capture.frames.mediaframesourcegroup#Windows_Media_Capture_Frames_MediaFrameSourceGroup_FindAllAsync)** 메서드는 현재 장치에서 사용할 수 있는 모든 소스 그룹을 반환합니다. 반환된 **MediaFrameSourceGroup**에는 각각 그룹의 프레임 소스를 설명하는 **[MediaFrameSourceInfo](https://docs.microsoft.com/uwp/api/windows.media.capture.frames.mediaframesourceinfo)** 개체 목록이 있습니다. Linq 쿼리는 두 개의 컬러 카메라를 포함하는 소스 그룹을 찾는 데 사용됩니다. 하나는 전면 패널에, 다른 하나는 후면에 있습니다. 각 컬러 카메라에 대해 선택한 **MediaFrameSourceGroup** 및 **MediaFrameSourceInfo**를 포함하는 익명 개체가 반환됩니다. Linq 구문을 사용하는 대신 각 그룹과 각 **MediaFrameSourceInfo**를 검토하여 요구 사항에 부합하는 그룹을 찾을 수 있습니다.

모든 장치에 두 개의 컬러 카메라로 구성된 소스 그룹이 포함되는 것은 아니므로 비디오를 캡처하기 전에 소스 그룹을 찾았는지 확인해야 합니다.

[!code-cs[MultiRecordFindSensorGroups](./code/SimpleCameraPreview_Win10/cs/MainPage.MultiRecord.xaml.cs#SnippetMultiRecordFindSensorGroups)]

## <a name="initialize-the-mediacapture-object"></a>MediaCapture 개체 초기화
**[MediaCapture](https://docs.microsoft.com/uwp/api/windows.media.capture.mediacapture)** 클래스는 UWP 앱에서 대부분의 오디오, 비디오 및 사진 캡처 작업에 사용되는 기본 클래스입니다. **[InitializeAsync](https://docs.microsoft.com/uwp/api/windows.media.capture.mediacapture#Windows_Media_Capture_MediaCapture_InitializeAsync)**를 호출하고 초기화 매개 변수를 포함하는 **[MediaCaptureInitializationSettings](https://docs.microsoft.com/uwp/api/windows.media.capture.mediacaptureinitializationsettings)** 개체를 전달하여 개체를 초기화합니다. 이 예에서 지정된 유일한 설정은 이전 코드 예제에서 검색되었던, **MediaFrameSourceGroup**으로 설정된 **[SourceGroup](https://docs.microsoft.com/uwp/api/windows.media.capture.mediacaptureinitializationsettings#Windows_Media_Capture_MediaCaptureInitializationSettings_SourceGroup)** 속성입니다.

**MediaCapture** 및 기타 미디어 캡처를 위한 UWP 앱 기능으로 수행할 수 있는 다른 작업에 대한 정보는 [카메라](camera.md)를 참조하세요.

[!code-cs[MultiRecordInitMediaCapture](./code/SimpleCameraPreview_Win10/cs/MainPage.MultiRecord.xaml.cs#SnippetMultiRecordInitMediaCapture)]

## <a name="create-a-mediaencodingprofile"></a>MediaEncodingProfile 생성
**[MediaEncodingProfile](https://docs.microsoft.com/uwp/api/windows.media.mediaproperties.mediaencodingprofile)** 클래스는 캡처된 오디오 및 비디오를 파일에 기록할 때 어떻게 인코딩해야 하는지 미디어 캡처 파이프 라인에 알려줍니다. 일반적인 캡처 및 코드 변환 시나리오의 경우, 이 클래스는 **[CreateAvi](https://docs.microsoft.com/uwp/api/windows.media.mediaproperties.mediaencodingprofile#Windows_Media_MediaProperties_MediaEncodingProfile_CreateAvi_Windows_Media_MediaProperties_VideoEncodingQuality_)** 및 **[CreateMp3](https://docs.microsoft.com/uwp/api/windows.media.mediaproperties.mediaencodingprofile#Windows_Media_MediaProperties_MediaEncodingProfile_CreateMp3_Windows_Media_MediaProperties_AudioEncodingQuality_)** 등의 공통 프로필을 만들기 위한 정적 메서드 집합을 제공합니다. 이 예의 경우, 인코딩 프로파일은 Mpeg4 컨테이너와 H264 비디오 인코딩을 사용하여 수동으로 생성됩니다. 비디오 인코딩 설정은 **[VideoEncodingProperties](https://docs.microsoft.com/uwp/api/windows.media.mediaproperties.videoencodingproperties)** 개체를 사용하여 지정됩니다. 이 시나리오에 사용된 각 컬러 카메라에 대해 **VideoStreamDescriptor** 개체가 구성됩니다. 설명자는 인코딩을 지정하는 **VideoEncodingProperties** 개체로 구성됩니다. **VideoStreamDescriptor**의 **[Label](https://docs.microsoft.com/uwp/api/windows.media.core.videostreamdescriptor#Windows_Media_Core_VideoStreamDescriptor_Label)** 속성은 스트림에 캡처될 미디어 프레임 소스의 ID로 설정되어야 합니다. 캡처 파이프 라인은 이런 식으로 각 카메라에 대해 어떤 스트림 설명자와 인코딩 속성을 사용해야 하는지 파악합니다. **MediaFrameSourceGroup**을 선택하면 이전 섹션에서 발견된 **[MediaFrameSourceInfo](https://docs.microsoft.com/uwp/api/windows.media.capture.frames.mediaframesourceinfo)** 개체에 의해 프레임 소스의 ID가 노출됩니다.


RS3부터는 **[SetVideoTracks](https://docs.microsoft.com/uwp/api/windows.media.mediaproperties.mediaencodingprofile#Windows_Media_MediaProperties_MediaEncodingProfile_SetVideoTracks_Windows_Foundation_Collections_IIterable_Windows_Media_Core_VideoStreamDescriptor__)**를 호출하여 **MediaEncodingProfile**에 여러 인코딩 속성을 설정할 수 있습니다. **[GetVideoTracks](https://docs.microsoft.com/uwp/api/windows.media.mediaproperties.mediaencodingprofile#Windows_Media_MediaProperties_MediaEncodingProfile_GetVideoTracks)**를 호출하여 비디오 스트림 설명자의 목록을 검색할 수 있습니다. 단일 스트림 설명자를 저장하는 **[Video](https://docs.microsoft.com/uwp/api/windows.media.mediaproperties.mediaencodingprofile#Windows_Media_MediaProperties_MediaEncodingProfile_Video)** 속성을 설정하면 **SetVideoTracks**를 호출하여 설정한 설명자 목록이 사용자 지정 단일 설명자가 포함된 목록으로 바뀝니다.


[!code-cs[MultiRecordMediaEncodingProfile](./code/SimpleCameraPreview_Win10/cs/MainPage.MultiRecord.xaml.cs#SnippetMultiRecordMediaEncodingProfile)]

## <a name="record-using-the-multi-stream-mediaencodingprofile"></a>멀티 스트림 MediaEncodingProfile을 사용한 녹화
이 예의 마지막 단계는 **[StartRecordToStorageFileAsync](https://docs.microsoft.com/uwp/api/windows.media.capture.mediacapture#Windows_Media_Capture_MediaCapture_StartRecordToStorageFileAsync_Windows_Media_MediaProperties_MediaEncodingProfile_Windows_Storage_IStorageFile_)**를 호출하고, 캡처된 미디어가 기록되는 **StorageFile**과 이전 코드 예제에서 생성된 **MediaEncodingProfile**을 전달하여 비디오 캡처를 시작하는 것입니다. 잠시 후, **[StopRecordAsync](https://docs.microsoft.com/uwp/api/windows.media.capture.mediacapture#Windows_Media_Capture_MediaCapture_StopRecordAsync)**를 호출하여 녹화를 중지합니다.

[!code-cs[MultiRecordToFile](./code/SimpleCameraPreview_Win10/cs/MainPage.MultiRecord.xaml.cs#SnippetMultiRecordToFile)]

작업이 완료되면 파일 내에 별도 스트림으로 인코딩된 각 카메라에서 캡처한 비디오가 포함된 비디오 파일이 생성됩니다. 여러 개의 비디오 트랙이 포함된 미디어 파일 재생에 대한 정보는 [미디어 항목, 재생 목록 및 트랙](media-playback-with-mediasource.md)을 참조하세요.

## <a name="related-topics"></a>관련 항목

* [카메라](camera.md)
* [MediaCapture를 사용하여 기본적인 사진, 비디오 및 오디오 캡처](basic-photo-video-and-audio-capture-with-MediaCapture.md)
* [MediaFrameReader를 사용하여 미디어 프레임 처리](process-media-frames-with-mediaframereader.md)
* [미디어 항목, 재생 목록 및 트랙](media-playback-with-mediasource.md)


 

 




