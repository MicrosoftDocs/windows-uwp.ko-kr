---
author: drewbatgit
ms.assetid: A1A0D99A-DCBF-4A14-80B9-7106BEF045EC
description: 동영상 파일을 한 형식에서 다른 형식으로 코드 변환하려면 Windows.Media.Transcoding API를 사용할 수 있습니다.
title: 미디어 파일 코드 변환
ms.author: drewbat
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: babf91e681004942bb3b66eb43622742fa183125
ms.sourcegitcommit: e38b334edb82bf2b1474ba686990f4299b8f59c7
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/15/2018
ms.locfileid: "6851637"
---
# <a name="transcode-media-files"></a>미디어 파일 코드 변환



동영상 파일을 한 형식에서 다른 형식으로 코드 변환하려면 [**Windows.Media.Transcoding**](https://msdn.microsoft.com/library/windows/apps/br207105) API를 사용할 수 있습니다.

*코드 변환*은 동영상 또는 오디오 파일과 같은 디지털 미디어 파일을 한 형식에서 다른 형식으로 변환하는 작업입니다. 이 작업은 일반적으로 파일을 디코드한 후 다시 인코드하여 수행됩니다. 예를 들어 MP4 형식을 지원하는 휴대용 디바이스에서 재생할 수 있도록 Windows Media 파일을 MP4로 변환할 수 있습니다. 또는 HD 동영상 파일을 저해상도로 변환할 수 있습니다. 이 경우 다시 인코딩된 파일은 원본 파일과 동일한 코덱을 사용할 수 있지만 다른 인코딩 프로필을 갖게 됩니다.

## <a name="set-up-your-project-for-transcoding"></a>코드 변환에 대한 프로젝트 설정

기본 프로젝트 템플릿이 참조하는 네임스페이스 외에도, 이 문서에서 제공되는 코드를 사용하여 미디어 파일의 코드를 변환하려면 이러한 네임스페이스를 참조해야 합니다.

[!code-cs[Using](./code/TranscodeWin10/cs/MainPage.xaml.cs#SnippetUsing)]

## <a name="select-source-and-destination-files"></a>원본 파일 및 대상 파일 선택

앱이 코드 변환을 위해 원본 파일 및 대상 파일을 결정하는 방법은 구현에 따라 다릅니다. 이 예제에서는 사용자가 원본 파일 및 대상 파일을 선택할 수 있도록 만들기 위해 [**FileOpenPicker**](https://msdn.microsoft.com/library/windows/apps/br207847) 및 [**FileSavePicker**](https://msdn.microsoft.com/library/windows/apps/br207871)를 사용합니다.

[!code-cs[TranscodeGetFile](./code/TranscodeWin10/cs/MainPage.xaml.cs#SnippetTranscodeGetFile)]

## <a name="create-a-media-encoding-profile"></a>미디어 인코딩 프로필 만들기

인코딩 프로필에는 대상 파일의 인코드 방법을 결정하는 설정이 포함되어 있습니다. 파일을 코드 변환하는 경우 여기서 가장 많은 옵션을 사용할 수 있습니다.

[**MediaEncodingProfile**](https://msdn.microsoft.com/library/windows/apps/hh701026) 클래스는 미리 정의된 인코딩 프로필을 만들기 위한 정적 메서드를 제공합니다.

### <a name="methods-for-creating-audio-only-encoding-profiles"></a>오디오 전용 인코딩 프로필 만들기 위한 메서드

메서드  |프로필  |
---------|---------|
[**CreateAlac**](https://docs.microsoft.com/uwp/api/windows.media.mediaproperties.mediaencodingprofile.createalac)     |ALAC(Apple 무손실 오디오 코덱) 오디오         |
[**CreateFlac**](https://docs.microsoft.com/uwp/api/windows.media.mediaproperties.mediaencodingprofile.createflac)     |FLAC(무료 무손실 오디오 코덱) 오디오.         |
[**CreateM4a**](https://docs.microsoft.com/uwp/api/windows.media.mediaproperties.mediaencodingprofile.createm4a)     |AAC 오디오(M4A)         |
[**CreateMp3**](https://docs.microsoft.com/uwp/api/windows.media.mediaproperties.mediaencodingprofile.createmp3)     |MP3 오디오         |
[**CreateWav**](https://docs.microsoft.com/uwp/api/windows.media.mediaproperties.mediaencodingprofile.createwav)     |WAV 오디오         |
[**CreateWmv**](https://docs.microsoft.com/uwp/api/windows.media.mediaproperties.mediaencodingprofile.createwmv)     |WMA(Windows Media 오디오)         |

### <a name="methods-for-creating-audio--video-encoding-profiles"></a>오디오/비디오 인코딩 프로필을 만들기 위한 메서드

메서드  |프로필  |
---------|---------|
[**CreateAvi**](https://docs.microsoft.com/uwp/api/windows.media.mediaproperties.mediaencodingprofile.createavi) |AVI |
[**CreateHevc**](https://docs.microsoft.com/uwp/api/windows.media.mediaproperties.mediaencodingprofile.createhevc) |HEVC(고효율성 비디오 코딩) 비디오, H.265 비디오라고도 함 |
[**CreateMp4**](https://docs.microsoft.com/uwp/api/windows.media.mediaproperties.mediaencodingprofile.createmp4) |MP4 비디오(H.264 비디오 및 AAC 오디오) |
[**CreateWmv**](https://docs.microsoft.com/uwp/api/windows.media.mediaproperties.mediaencodingprofile.createwmv) |WMV(Windows Media 비디오) |


다음 코드는 MP4 동영상용 프로필을 만듭니다.

[!code-cs[TranscodeMediaProfile](./code/TranscodeWin10/cs/MainPage.xaml.cs#SnippetTranscodeMediaProfile)]

정적 [**CreateMp4**](https://docs.microsoft.com/uwp/api/windows.media.mediaproperties.mediaencodingprofile.createmp4) 메서드는 MP4 인코딩 프로필을 만듭니다. 이 메서드의 매개 변수는 동영상의 대상 해상도를 제공합니다. 이 경우 [**VideoEncodingQuality.hd720p**](https://msdn.microsoft.com/library/windows/apps/hh701290)는 초당 30 프레임 속도에서 1280 x 720 픽셀을 의미합니다. "720p"는 프레임당 720 프로그레시브 스캔 선을 나타냅니다. 미리 정의된 프로필을 만드는 다른 모든 메서드도 이 패턴을 따릅니다.

또는 [**MediaEncodingProfile.CreateFromFileAsync**](https://msdn.microsoft.com/library/windows/apps/hh701047) 메서드를 사용하여 기존 미디어 파일과 일치하는 프로필을 만들 수 있습니다. 또는 원하는 인코딩 설정을 정확하게 알고 있는 경우 새 [**MediaEncodingProfile**](https://msdn.microsoft.com/library/windows/apps/hh701026) 개체를 만들고 프로필 세부 정보를 채울 수 있습니다.

## <a name="transcode-the-file"></a>파일 코드 변환

파일을 트랜스코딩하려면 새 [**MediaTranscoder**](https://msdn.microsoft.com/library/windows/apps/br207080) 개체를 만들고 [**MediaTranscoder.PrepareFileTranscodeAsync**](https://msdn.microsoft.com/library/windows/apps/hh700936) 메서드를 호출합니다. 원본 파일, 대상 파일 및 인코딩 프로필을 전달합니다. 그런 다음 비동기 코드 변환 작업에서 반환된 [**PrepareTranscodeResult**](https://msdn.microsoft.com/library/windows/apps/hh700941) 개체의 [**TranscodeAsync**](https://msdn.microsoft.com/library/windows/apps/hh700946) 메서드를 호출합니다.

[!code-cs[TranscodeTranscodeFile](./code/TranscodeWin10/cs/MainPage.xaml.cs#SnippetTranscodeTranscodeFile)]

## <a name="respond-to-transcoding-progress"></a>코드 변환 진행률에 응답

비동기 [**TranscodeAsync**](https://msdn.microsoft.com/library/windows/apps/hh700946)의 진행이 변경되면 응답하도록 이벤트를 등록할 수 있습니다. 이러한 이벤트는 UWP(유니버설 Windows 플랫폼) 앱에 대한 비동기 프로그래밍 프레임워크의 일부이며 코드 변환 API와 관련이 없습니다.

[!code-cs[TranscodeCallbacks](./code/TranscodeWin10/cs/MainPage.xaml.cs#SnippetTranscodeCallbacks)]


## <a name="encode-a-metadata-stream"></a>메타데이터 스트림 인코딩
Windows 10, 버전 1803부터 시간이 제한 된 메타 데이터를 포함할 수 때 미디어 파일 코드 변환 합니다. 위의 비디오 코드 변환 예제와 달리 인코딩 프로필 만들기 메서드 [**MediaEncodingProfile.CreateMp4**](https://docs.microsoft.com/uwp/api/windows.media.mediaproperties.mediaencodingprofile.createmp4)같은 기본 제공 미디어를 사용 하는 수동으로 만들어야 인코딩하는 메타 데이터의 형식을 지원 하도록 메타 데이터 인코딩 프로필 .

메타 데이터 incoding 프로필을 만드는 첫 번째 단계가 코드 변환 되도록 메타 데이터의 인코딩을 설명 하는 [**TimedMetadataEncodingProperties**] 개체를 만드는 것입니다. 하위 속성에는 메타 데이터의 형식을 지정 하는 GUID입니다. 각 메타 데이터 형식에 대 한 인코딩 세부 독점 이며 Windows에서 제공 되지 않습니다. 이 예제는 GoPro 메타 데이터 (gprs)에 대 한 GUID 사용 됩니다. 다음으로 [**SetFormatUserData**](https://docs.microsoft.com/uwp/api/windows.media.mediaproperties.timedmetadataencodingproperties.setformatuserdata) 메타 데이터 형식에 관련 된 스트림 형식을 설명 하는 데이터의 이진 blob을 설정 하 라고 합니다. 다음을 **TimedMetadataStreamDescriptor**(https://docs.microsoft.com/uwp/api/windows.media.core.timedmetadatastreamdescriptor) 인코딩 속성에서 만든 트랙 레이블 이름과 메타 데이터 스트림을 식별 하 고 필요에 따라 스트림 이름 UI에 표시할 endcoded 스트림을 읽는 응용 프로그램이 허용 하 고 합니다. 
 
[!code-cs[GetStreamDescriptor](./code/TranscodeWin10/cs/MainPage.xaml.cs#SnippetGetStreamDescriptor)]

**TimedMetadataStreamDescriptor**를 만든 후 비디오, 오디오 및 메타 데이터 파일에 인코딩할 수를 설명 하는 **MediaEncodingProfile** 을 만들 수 있습니다. 마지막 예제에서 만든 **TimedMetadataStreamDescriptor** 이 예제에서는 도우미 함수에 전달 하 고 [**SetTimedMetadataTracks**](https://docs.microsoft.com/en-us/uwp/api/windows.media.mediaproperties.mediaencodingprofile.settimedmetadatatracks)를 호출 하 여 **MediaEncodingProfile** 에 추가 됩니다.

[!code-cs[GetMediaEncodingProfile](./code/TranscodeWin10/cs/MainPage.xaml.cs#SnippetGetMediaEncodingProfile)]
 

 




