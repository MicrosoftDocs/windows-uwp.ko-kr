---
ms.assetid: A1A0D99A-DCBF-4A14-80B9-7106BEF045EC
description: 비디오 파일을 한 형식에서 다른 형식으로 트랜스 코딩 하는 데에는 Windows를 사용할 수 있습니다.
title: 미디어 파일 코드 변환
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 927ff8ceb8dc29400f5a7d0ede42b3ee8b703efb
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89175667"
---
# <a name="transcode-media-files"></a>미디어 파일 코드 변환



비디오 파일을 한 형식에서 다른 형식으로 트랜스 코딩 하는 데에는 [**Windows**](/uwp/api/Windows.Media.Transcoding) 를 사용할 수 있습니다.

*트랜스 코딩* 은 비디오 또는 오디오 파일과 같은 디지털 미디어 파일을 한 형식에서 다른 형식으로 변환 하는 것입니다. 일반적으로이 작업은 파일을 디코딩 한 다음 다시 인코딩하여 수행 됩니다. 예를 들어, MP4 형식을 지 원하는 휴대용 장치에서 재생할 수 있도록 Windows Media 파일을 MP4로 변환할 수 있습니다. 또는 고화질 비디오 파일을 더 낮은 해상도로 변환할 수 있습니다. 이 경우 다시 인코딩된 파일은 원본 파일과 동일한 코덱을 사용할 수 있지만 다른 인코딩 프로필을 사용 합니다.

## <a name="set-up-your-project-for-transcoding"></a>트랜스 코딩을 위한 프로젝트 설정

기본 프로젝트 템플릿에서 참조 하는 네임 스페이스 외에도이 문서의 코드를 사용 하 여 미디어 파일을 트랜스 코딩 하기 위해 이러한 네임 스페이스를 참조 해야 합니다.

[!code-cs[Using](./code/TranscodeWin10/cs/MainPage.xaml.cs#SnippetUsing)]

## <a name="select-source-and-destination-files"></a>원본 및 대상 파일 선택

앱이 코드를 변환 하는 소스 및 대상 파일을 결정 하는 방식은 구현에 따라 달라 집니다. 이 예제에서는 [**Fileopenpicker**](/uwp/api/Windows.Storage.Pickers.FileOpenPicker) 와 [**FileSavePicker**](/uwp/api/Windows.Storage.Pickers.FileSavePicker) 를 사용 하 여 사용자가 원본과 대상 파일을 선택할 수 있도록 합니다.

[!code-cs[TranscodeGetFile](./code/TranscodeWin10/cs/MainPage.xaml.cs#SnippetTranscodeGetFile)]

## <a name="create-a-media-encoding-profile"></a>미디어 인코딩 프로필 만들기

인코딩 프로필에는 대상 파일이 인코딩 되는 방법을 결정 하는 설정이 포함 되어 있습니다. 이 경우 파일을 트랜스 코딩 하는 경우 가장 많은 옵션이 있습니다.

[**MediaEncodingProfile**](/uwp/api/Windows.Media.MediaProperties.MediaEncodingProfile) 클래스는 미리 정의 된 인코딩 프로필을 만드는 정적 메서드를 제공 합니다.

### <a name="methods-for-creating-audio-only-encoding-profiles"></a>오디오 전용 인코딩 프로필을 만드는 방법

메서드  |프로필  |
---------|---------|
[**CreateAlac**](/uwp/api/windows.media.mediaproperties.mediaencodingprofile.createalac)     |Apple ALAC (무손실 오디오 코덱) 오디오         |
[**CreateFlac**](/uwp/api/windows.media.mediaproperties.mediaencodingprofile.createflac)     |무료 무손실 오디오 코덱 (FLAC) 오디오.         |
[**CreateM4a**](/uwp/api/windows.media.mediaproperties.mediaencodingprofile.createm4a)     |AAC audio (M4A)         |
[**CreateMp3**](/uwp/api/windows.media.mediaproperties.mediaencodingprofile.createmp3)     |MP3 오디오         |
[**CreateWav**](/uwp/api/windows.media.mediaproperties.mediaencodingprofile.createwav)     |WAV 오디오         |
[**CreateWmv**](/uwp/api/windows.media.mediaproperties.mediaencodingprofile.createwmv)     |Windows Media 오디오 (WMA)         |

### <a name="methods-for-creating-audio--video-encoding-profiles"></a>오디오/비디오 인코딩 프로필을 만드는 방법

메서드  |프로필  |
---------|---------|
[**CreateAvi**](/uwp/api/windows.media.mediaproperties.mediaencodingprofile.createavi) |기본값으로 |
[**CreateHevc**](/uwp/api/windows.media.mediaproperties.mediaencodingprofile.createhevc) |HEVC (High High Video 코딩) 비디오 (265 비디오 라고도 함) |
[**CreateMp4**](/uwp/api/windows.media.mediaproperties.mediaencodingprofile.createmp4) |MP4 비디오 (h.264 비디오 plus AAC audio) |
[**CreateWmv**](/uwp/api/windows.media.mediaproperties.mediaencodingprofile.createwmv) |WMV(Windows Media Video) |


다음 코드에서는 MP4 비디오에 대 한 프로필을 만듭니다.

[!code-cs[TranscodeMediaProfile](./code/TranscodeWin10/cs/MainPage.xaml.cs#SnippetTranscodeMediaProfile)]

Static [**CreateMp4**](/uwp/api/windows.media.mediaproperties.mediaencodingprofile.createmp4) 메서드는 MP4 인코딩 프로필을 만듭니다. 이 메서드에 대 한 매개 변수는 비디오에 대 한 대상 해상도를 제공 합니다. 이 경우 [**VideoEncodingQuality hd720p**](/uwp/api/Windows.Media.MediaProperties.VideoEncodingQuality) 는 초당 30 개 프레임에서 1280 x 720 픽셀을 의미 합니다. ("720p"는 프레임 당 프로그레시브 스캔 라인 720을 나타냅니다.) 미리 정의 된 프로필을 만드는 다른 방법은 모두이 패턴을 따릅니다.

또는 [**MediaEncodingProfile. CreateFromFileAsync**](/uwp/api/windows.media.mediaproperties.mediaencodingprofile.createfromfileasync) 메서드를 사용 하 여 기존 미디어 파일과 일치 하는 프로필을 만들 수 있습니다. 또는 원하는 정확한 인코딩 설정을 알고 있는 경우 새 [**MediaEncodingProfile**](/uwp/api/Windows.Media.MediaProperties.MediaEncodingProfile) 개체를 만들고 프로필 세부 정보를 입력할 수 있습니다.

## <a name="transcode-the-file"></a>파일 트랜스 코딩

파일을 트랜스 코딩 하려면 새 [**MediaTranscoder**](/uwp/api/Windows.Media.Transcoding.MediaTranscoder) 개체를 만들고 [**PrepareFileTranscodeAsync**](/uwp/api/windows.media.transcoding.mediatranscoder.preparefiletranscodeasync) 메서드를 호출 합니다. 원본 파일, 대상 파일 및 인코딩 프로필을 전달 합니다. 그런 다음 비동기 코드 변환 작업에서 반환 된 [**PrepareTranscodeResult**](/uwp/api/Windows.Media.Transcoding.PrepareTranscodeResult) 개체에 대해 [**TranscodeAsync**](/uwp/api/windows.media.transcoding.preparetranscoderesult.transcodeasync) 메서드를 호출 합니다.

[!code-cs[TranscodeTranscodeFile](./code/TranscodeWin10/cs/MainPage.xaml.cs#SnippetTranscodeTranscodeFile)]

## <a name="respond-to-transcoding-progress"></a>트랜스 코딩 진행률에 응답

비동기 [**TranscodeAsync**](/uwp/api/windows.media.transcoding.preparetranscoderesult.transcodeasync) 의 진행률이 변경 될 때 응답 하도록 이벤트를 등록할 수 있습니다. 이러한 이벤트는 UWP (비동기 프로그래밍 프레임 유니버설 Windows 플랫폼 워크) 앱의 일부 이며 트랜스 코딩 API에 국한 되지 않습니다.

[!code-cs[TranscodeCallbacks](./code/TranscodeWin10/cs/MainPage.xaml.cs#SnippetTranscodeCallbacks)]