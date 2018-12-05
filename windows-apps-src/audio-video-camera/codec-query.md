---
ms.assetid: 0A360481-B649-4E90-9BC4-4449BA7445EF
description: 장치에 설치된 오디오와 비디오 인코더 및 디코더에 대해 쿼리합니다.
title: 설치된 코덱에 대한 쿼리
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp, 코덱, 인코더, 디코더, 쿼리
ms.localizationpriority: medium
ms.openlocfilehash: 4241aad5a01617d6a002c6f5d6da0a4bb1455616
ms.sourcegitcommit: d7613c791107f74b6a3dc12a372d9de916c0454b
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 12/05/2018
ms.locfileid: "8758693"
---
# <a name="query-for-codecs-installed-on-a-device"></a>디바이스에 설치된 코덱에 대한 쿼리
**[CodecQuery](https://docs.microsoft.com/uwp/api/windows.media.core.codecquery)** 클래스를 통해 현재 디바이스에 설치된 코덱에 대해 쿼리할 수 있습니다. 다른 장치 제품군에 대한 Windows 10에 포함되어 있는 코덱 목록은 [지원되는 코덱](supported-codecs.md)문서에 나열되어 있지만, 사용자와 앱은 추가 코덱을 장치에 설치할 수 있습니다. 따라서 런타임 시 현재 디바이스에서 사용할 수 있는 코덱을 파악하기 위해 코덱 지원에 대한 쿼리를 수행할 수 있습니다.

CodecQuery API는 **[Windows.Media.Core](https://docs.microsoft.com/uwp/api/windows.media.core)** 네임스페이스의 구성원이므로 이 네임스페이스를 앱에 포함해야 합니다.

CodecQuery API는 **[Windows.Media.Core](https://docs.microsoft.com/uwp/api/windows.media.core)** 네임스페이스의 구성원이므로 이 네임스페이스를 앱에 포함해야 합니다.

[!code-cs[CodecQueryUsing](./code/TranscodeWin10/cs/MainPage.xaml.cs#SnippetCodecQueryUsing)]

생성자를 호출하여 **CodecQuery** 클래스의 새로운 인스턴스를 초기화합니다.

[!code-cs[NewCodecQuery](./code/TranscodeWin10/cs/MainPage.xaml.cs#SnippetNewCodecQuery)]

**[FindAllAsync](https://docs.microsoft.com/uwp/api/windows.media.core.codecquery.findallasync)** 메서드는 제공된 매개 변수와 일치하는 모든 설치된 코덱을 반환합니다. 이러한 매개 변수는 오디오 또는 비디오 코덱 또는 둘 다에 대해 쿼리하는지 여부를 지정하는 **[CodecKind](https://docs.microsoft.com/uwp/api/windows.media.core.codeckind)** 값, 인코더 또는 디코더에 대해 쿼리하는지 여부를 지정하는 **[CodecCategory](https://docs.microsoft.com/uwp/api/windows.media.core.codeccategory)** 값, 그리고 H.264 비디오 또는 MP3 오디오와 같이 쿼리하는 미디어 인코딩 하위 형식을 나타내는 문자열을 포함합니다.

하위 형식 값에 대해 빈 문자열 또는 null을 지정하여 모든 하위 형식에 대해 코덱을 반환합니다. 다음 예제는 장치에 설치된 모든 비디오 인코더를 나열합니다.

[!code-cs[FindAllEncoders](./code/TranscodeWin10/cs/MainPage.xaml.cs#SnippetFindAllEncoders)]

**FindAllAsync**에 전달한 하위 형식 문자열은 시스템에서 정의한 하위 형식 GUID 또는 하위 형식에 대한 FOURCC 코드의 문자열 표시일 수 있습니다. 지원되는 미디어 하위 형식 GUID 세트는 [Audio Subtype GUIDs](https://msdn.microsoft.com/library/windows/desktop/aa372553(v=vs.85).aspx) 및 [Video Subtype GUIDs](https://msdn.microsoft.com/library/windows/desktop/aa370819(v=vs.85).aspx) 문서에 나와 있지만, **[CodecSubtypes](https://docs.microsoft.com/uwp/api/windows.media.core.codecsubtypes)** 클래스는 지원되는 각 하위 형식에 대해 GUID 값을 반환하는 속성을 제공합니다. FOURCC 코드에 대한 자세한 내용은 [FOURCC 코드](https://msdn.microsoft.com/library/windows/desktop/dd375802(v=vs.85).aspx)를 참조하세요. 

다음 예제에서는 FOURCC 코드 "H264"를 지정하여 디바이스에 설치된 H.264 비디오 디코더가 있는지 확인합니다. H.264 동영상 콘텐츠를 재생하기 전에 이 쿼리를 수행할 수 있습니다. 지원되지 않는 코덱을 재생 시 처리할 수도 있습니다. 자세한 내용은 [미디어 항목을 열 때 지원되지 않는 코덱 및 알 수 없는 오류 처리](https://docs.microsoft.com/windows/uwp/audio-video-camera/media-playback-with-mediasource#handle-unsupported-codecs-and-unknown-errors-when-opening-media-items)를 참조하세요.

[!code-cs[IsH264Supported](./code/TranscodeWin10/cs/MainPage.xaml.cs#SnippetIsH264Supported)]

다음 예제는 FLAC 오디오 인코더가 현재 디바이스에 설치되어 있는지 쿼리하고 설치되어 있다면 오디오를 파일에 캡처하거나 다른 형식에서 FLAC 오디오 파일로 오디오를 코드 변환하는 데 사용할 수 있는 하위 형식에 대해 **[MediaEncodingProfile](https://docs.microsoft.com/uwp/api/Windows.Media.MediaProperties.MediaEncodingProfile)** 이 생성됩니다.

[!code-cs[IsFLACSupported](./code/TranscodeWin10/cs/MainPage.xaml.cs#SnippetIsFLACSupported)]

## <a name="related-topics"></a>관련 항목

* [미디어 재생](media-playback.md)
* [MediaCapture를 사용하여 기본적인 사진, 비디오 및 오디오 캡처](basic-photo-video-and-audio-capture-with-MediaCapture.md)
* [미디어 파일 코드 변환](transcode-media-files.md)
* [지원되는 코덱](supported-codecs.md)
 

 




