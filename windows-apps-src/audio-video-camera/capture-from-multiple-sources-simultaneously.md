---
ms.assetid: ''
description: 이 문서에서는 여러 원본 simulataneously에서 포함 된 여러 비디오 트랙을 포함 하는 단일 파일로 비디오를 캡처하는 방법을 보여 줍니다.
title: MediaFrameSourceGroup을 사용하여 다양한 소스로부터 캡처
ms.date: 09/12/2017
ms.topic: article
keywords: windows 10, uwp, 캡처, 비디오
ms.localizationpriority: medium
ms.openlocfilehash: 78f12137bcb6dbb9984648ce141b4351d592902b
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89160917"
---
# <a name="capture-from-multiple-sources-using-mediaframesourcegroup"></a>MediaFrameSourceGroup을 사용하여 다양한 소스로부터 캡처

이 문서에서는 여러 소스의 비디오를 여러 개의 포함 된 비디오 트랙을 포함 하는 단일 파일로 동시에 캡처하는 방법을 보여 줍니다. RS3 부터는 단일 **[MediaEncodingProfile](/uwp/api/windows.media.mediaproperties.mediaencodingprofile)** 에 대해 여러 **[videostreamdescriptor](/uwp/api/windows.media.core.videostreamdescriptor)** 개체를 지정할 수 있습니다. 이렇게 하면 여러 스트림을 동시에 단일 파일로 인코딩할 수 있습니다. 이 작업에서 인코딩된 비디오 스트림은 현재 장치에서 동시에 사용할 수 있는 카메라 집합을 지정 하는 단일 **[Mediaframesourcegroup](/uwp/api/windows.media.capture.frames.mediaframesourcegroup)** 에 포함 되어야 합니다. 

**[MediaFrameReader](/uwp/api/windows.media.capture.frames.mediaframereader)** 클래스에서 **[Mediaf esourcegroup](/uwp/api/windows.media.capture.frames.mediaframesourcegroup)** 을 사용 하 여 여러 카메라를 사용 하는 실시간 컴퓨터 비전 시나리오를 사용 하는 방법에 대 한 자세한 내용은 [MediaFrameReader를 사용 하 여 미디어 프레임 처리](process-media-frames-with-mediaframereader.md)를 참조 하세요.

이 문서의 나머지 부분에서는 두 컬러 카메라의 비디오를 여러 비디오 트랙을 포함 하는 단일 파일로 기록 하는 단계를 안내 합니다.

## <a name="find-available-sensor-groups"></a>사용 가능한 센서 그룹 찾기
**Mediafsimulataneously Esourcegroup** 은 액세스할 수 있는 프레임 원본 (일반적으로 카메라)의 컬렉션을 나타냅니다. 사용 가능한 프레임 소스 그룹 집합은 각 장치 마다 다르며,이 예제의 첫 번째 단계는 사용 가능한 프레임 원본 그룹 목록을 가져오고 시나리오에 필요한 카메라를 포함 하는 항목을 찾는 것입니다 .이 경우에는 두 가지 컬러 카메라가 필요 합니다.

**[MediafFindAllAsync](/uwp/api/windows.media.capture.frames.mediaframesourcegroup.FindAllAsync)** 메서드는 현재 장치에서 사용할 수 있는 모든 소스 그룹을 반환 합니다. 반환 된 각 **Mediaframesourcegroup** 에는 그룹의 각 프레임 원본을 설명 하는 **[Mediaframesourcegroup](/uwp/api/windows.media.capture.frames.mediaframesourceinfo)** 개체의 목록이 있습니다. Linq 쿼리를 사용 하 여 전면 패널 및 후면에 하나씩 두 개의 컬러 카메라를 포함 하는 원본 그룹을 찾을 수 있습니다. 선택한 **Mediaframesourcegroup** 및 각 컬러 카메라의 **Mediaframesourcegroup** 를 포함 하는 익명 개체가 반환 됩니다. Linq 구문을 사용 하는 대신 각 그룹을 반복 하 고 각 **Mediaframesourceinfo** 를 사용 하 여 요구 사항을 충족 하는 그룹을 찾을 수 있습니다.

모든 장치에는 두 개의 컬러 카메라가 포함 된 원본 그룹이 포함 되지 않으므로 비디오를 캡처하기 전에 원본 그룹이 있는지 확인 해야 합니다.

[!code-cs[MultiRecordFindSensorGroups](./code/SimpleCameraPreview_Win10/cs/MainPage.MultiRecord.xaml.cs#SnippetMultiRecordFindSensorGroups)]

## <a name="initialize-the-mediacapture-object"></a>MediaCapture 개체를 초기화 합니다.
**[MediaCapture](/uwp/api/windows.media.capture.mediacapture)** 클래스는 UWP 앱의 대부분 오디오, 비디오 및 사진 캡처 작업에 사용 되는 기본 클래스입니다. 초기화 매개 변수를 포함 하는 **[MediaCaptureInitializationSettings](/uwp/api/windows.media.capture.mediacaptureinitializationsettings)** 개체를 전달 하 여 **[InitializeAsync](/uwp/api/windows.media.capture.mediacapture.InitializeAsync)** 를 호출 하 여 개체를 초기화 합니다. 이 예제에서 지정 된 유일한 설정은 **[sourcegroup](/uwp/api/windows.media.capture.mediacaptureinitializationsettings.SourceGroup)** 속성입니다 .이 속성은 이전 코드 예제에서 검색 된 **Mediaframesourcegroup** 로 설정 됩니다.

미디어 캡처를 위해 **MediaCapture** 및 기타 UWP 앱 기능으로 수행할 수 있는 다른 작업에 대 한 자세한 내용은 [카메라](camera.md)를 참조 하세요.

[!code-cs[MultiRecordInitMediaCapture](./code/SimpleCameraPreview_Win10/cs/MainPage.MultiRecord.xaml.cs#SnippetMultiRecordInitMediaCapture)]

## <a name="create-a-mediaencodingprofile"></a>MediaEncodingProfile 만들기
**[MediaEncodingProfile](/uwp/api/windows.media.mediaproperties.mediaencodingprofile)** 클래스는 캡처된 오디오 및 비디오를 파일에 쓸 때 인코딩해야 하는 방법을 미디어 캡처 파이프라인에 알려 줍니다. 일반적인 캡처 및 트랜스 코딩 시나리오의 경우이 클래스는 **[Createavi](/uwp/api/windows.media.mediaproperties.mediaencodingprofile.createavi)** 및 **[CreateMp3](/uwp/api/windows.media.mediaproperties.mediaencodingprofile.createmp3)** 와 같은 일반적인 프로필을 만들기 위한 정적 메서드 집합을 제공 합니다. 이 예제에서 인코딩 프로필은 Mpeg4 컨테이너와 H264 비디오 인코딩을 사용 하 여 수동으로 만듭니다. 비디오 인코딩 설정은 **[VideoEncodingProperties](/uwp/api/windows.media.mediaproperties.videoencodingproperties)** 개체를 사용 하 여 지정 합니다. 이 시나리오에서 사용 되는 각 컬러 카메라에 대해 **Videostreamdescriptor** 개체가 구성 됩니다. 이 설명자는 인코딩을 지정 하는 **VideoEncodingProperties** 개체를 사용 하 여 생성 됩니다. **비디오 Streamdescriptor** 의 **[Label](/uwp/api/windows.media.core.videostreamdescriptor.Label)** 속성은 스트림에 캡처되는 미디어 프레임 원본의 ID로 설정 해야 합니다. 캡처 파이프라인이 각 카메라에 사용 해야 하는 스트림 설명자와 인코딩 속성을 파악 하는 방법입니다. 프레임 원본의 ID는 이전 섹션에서 **Mediaframesourceinfo** 을 선택 했을 때 찾은 **[Mediaframesourceinfo](/uwp/api/windows.media.capture.frames.mediaframesourceinfo)** 개체에 의해 노출 됩니다.


Windows 10 버전 1709부터 **[Setvideotracks](/uwp/api/windows.media.mediaproperties.mediaencodingprofile.setvideotracks)** 을 호출 하 여 **MediaEncodingProfile** 에 여러 encoding 속성을 설정할 수 있습니다. **[Getvideotracks](/uwp/api/windows.media.mediaproperties.mediaencodingprofile.GetVideoTracks)** 을 호출 하 여 비디오 스트림 설명자 목록을 검색할 수 있습니다. 단일 스트림 설명자를 저장 하는 **[Video](/uwp/api/windows.media.mediaproperties.mediaencodingprofile.Video)** 속성을 설정 하면 **setvideotracks** 을 호출 하 여 설정한 설명자 목록이 지정한 단일 설명자를 포함 하는 목록으로 바뀝니다.


[!code-cs[MultiRecordMediaEncodingProfile](./code/SimpleCameraPreview_Win10/cs/MainPage.MultiRecord.xaml.cs#SnippetMultiRecordMediaEncodingProfile)]

### <a name="encode-timed-metadata-in-media-files"></a>미디어 파일의 시간 제한 메타 데이터 인코딩

Windows 10, 버전 1803부터 오디오 및 비디오 외에도, 데이터 형식이 지원 되는 미디어 파일로 시간 제한 된 메타 데이터를 인코딩할 수 있습니다. 예를 들어 GoPro 메타 데이터 (gpmd)를 MP4 파일에 저장 하 여 비디오 스트림과 상관 관계가 지정 된 지리적 위치를 전달할 수 있습니다. 

인코딩 메타 데이터는 오디오 또는 비디오 인코딩에 대 한 병렬 패턴을 사용 합니다. [**TimedMetadataEncodingProperties**](/uwp/api/windows.media.mediaproperties.timedmetadataencodingproperties) 클래스는 비디오에 대 한 **VideoEncodingProperties** 와 같은 메타 데이터의 형식, 하위 형식 및 인코딩 속성을 설명 합니다. [**TimedMetadataStreamDescriptor**](/uwp/api/windows.media.core.timedmetadatastreamdescriptor) 는 비디오 스트림에 대 한 **videostreamdescriptor** 와 마찬가지로 메타 데이터 스트림을 식별 합니다.  

다음 예제에서는 **TimedMetadataStreamDescriptor** 개체를 초기화 하는 방법을 보여 줍니다. 먼저 **TimedMetadataEncodingProperties** 개체가 생성 되 고 **하위 형식이** 스트림에 포함 될 메타 데이터의 형식을 식별 하는 GUID로 설정 됩니다. 이 예제에서는 GoPro 메타 데이터 (gpmd)에 대해 GUID를 사용 합니다. [**Setformatuserdata**](/uwp/api/windows.media.mediaproperties.timedmetadataencodingproperties.setformatuserdata) 메서드를 호출 하 여 서식 지정 데이터를 설정 합니다. MP4 파일의 경우 형식 관련 데이터는 stsd (SampleDescription box)에 저장 됩니다. 그런 다음 인코딩 속성에서 새 **TimedMetadataStreamDescriptor** 을 만듭니다. **레이블** 및 **이름** 속성은 인코딩할 스트림을 식별 하도록 설정 됩니다. 

[!code-cs[GetStreamDescriptor](./code/SimpleCameraPreview_Win10/cs/MainPage.MultiRecord.xaml.cs#SnippetGetStreamDescriptor)]

[**MediaEncodingProfile**](/uwp/api/windows.media.mediaproperties.mediaencodingprofile.settimedmetadatatracks) 를 호출 하 여 메타 데이터 스트림 설명자를 인코딩 프로필에 추가 합니다. 다음 예제에서는 두 개의 비디오 스트림 설명자, 하나의 오디오 스트림 설명자 및 1 개의 시간이 지정 된 메타 데이터 스트림 설명자를 사용 하 고 스트림을 인코딩하는 데 사용할 수 있는 **MediaEncodingProfile** 를 반환 하는 도우미 메서드를 보여 줍니다.

[!code-cs[GetMediaEncodingProfile](./code/SimpleCameraPreview_Win10/cs/MainPage.MultiRecord.xaml.cs#SnippetGetMediaEncodingProfile)]

## <a name="record-using-the-multi-stream-mediaencodingprofile"></a>다중 스트림 MediaEncodingProfile를 사용 하 여 기록
이 예제의 마지막 단계에서는 **[StartRecordToStorageFileAsync](/uwp/api/windows.media.capture.mediacapture.startrecordtostoragefileasync)** 를 호출 하 고 캡처된 미디어가 작성 된 **StorageFile** 을 전달 하 고 이전 코드 예제에서 만든 **MediaEncodingProfile** 을 호출 하 여 비디오 캡처를 시작 합니다. 몇 초 동안 대기한 후에는 **[Stoprecordasync](/uwp/api/windows.media.capture.mediacapture.StopRecordAsync)** 를 호출 하 여 기록이 중지 됩니다.

[!code-cs[MultiRecordToFile](./code/SimpleCameraPreview_Win10/cs/MainPage.MultiRecord.xaml.cs#SnippetMultiRecordToFile)]

작업이 완료 되 면 파일 내의 별도 스트림으로 인코딩된 각 카메라에서 캡처된 비디오가 포함 된 비디오 파일이 생성 됩니다. 여러 비디오 트랙을 포함 하는 미디어 파일을 재생 하는 방법에 대 한 자세한 내용은 [미디어 항목, 재생 목록 및 트랙](media-playback-with-mediasource.md)을 참조 하세요.

## <a name="related-topics"></a>관련 항목

* [카메라](camera.md)
* [MediaCapture를 사용하여 기본적인 사진, 비디오 및 오디오 캡처](basic-photo-video-and-audio-capture-with-MediaCapture.md)
* [MediaFrameReader를 사용하여 미디어 프레임 처리](process-media-frames-with-mediaframereader.md)
* [미디어 항목, 재생 목록 및 트랙](media-playback-with-mediasource.md)


 

 