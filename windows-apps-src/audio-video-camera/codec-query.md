---
author: drewbatgit
ms.assetid: 0A360481-B649-4E90-9BC4-4449BA7445EF
description: "장치에 설치된 오디오와 비디오 인코더 및 디코더에 대해 쿼리합니다."
title: "설치된 코덱에 대한 쿼리"
ms.author: drewbat
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: "windows 10, uwp, 코덱, 인코더, 디코더, 쿼리"
ms.openlocfilehash: 1bde2c24db6faa843d9118f330867e7f66b22e7e
ms.sourcegitcommit: 64cfb79fd27b09d49df99e8c9c46792c884593a7
translationtype: HT
---
# <a name="query-for-installed-codecs"></a>설치된 코덱에 대한 쿼리
이 문서에서는 장치에 설치된 오디오와 비디오 인코더 및 디코더에 대해 쿼리하는 방법을 보여줍니다. 각 제품군의 OS에 포함되어 있는 코덱의 목록은 [지원되는 코덱](supported-codecs.md) 문서에 나열되어 있습니다. 개별 장치에서 사용자나 앱이 추가 코덱을 설치할 수 있습니다. [CodecQuery](https://docs.microsoft.com/en-us/uwp/api/windows.media.core.codecquery) 클래스를 사용하면 앱이 실행 중인 장치에 현재 설치되어 있는 인코더와 디코더를 나열할 수 있습니다.

**CodecQuery** 클래스는 [windows.media.core](https://docs.microsoft.com/en-us/uwp/api/windows.media.core) 네임스페이스에 포함되어 있으며 인수가 없는 생성자를 제공합니다.

[!code-cs[CodecQueryUsing](./code/TranscodeWin10/cs/MainPage.xaml.cs#SnippetCodecQueryUsing)]

[!code-cs[NewCodecQuery](./code/TranscodeWin10/cs/MainPage.xaml.cs#SnippetNewCodecQuery)]

**CodecQuery** 클래스의 [FindAllAsync](https://docs.microsoft.com/en-us/uwp/api/windows.media.core.codecquery#Windows_Media_Core_CodecQuery_FindAllAsync_Windows_Media_Core_CodecKind_Windows_Media_Core_CodecCategory_System_String_) 메서드는 지정된 매개 변수에 부합하는 설치된 모든 코덱을 반환합니다. 이러한 매개 변수는 오디오 코덱과 비디오 코덱 중 무엇을 반환할지 지정하는 [CodecKind](https://docs.microsoft.com/en-us/uwp/api/windows.media.core.codeckind) 열거형과 인코더와 디코더 중 무엇을 반환할지 지정하는 [CodecCategory](https://docs.microsoft.com/en-us/uwp/api/windows.media.core.codeccategory) 열거형의 값을 포함합니다.

최종 매개는 H264 비디오 또는 FLAC 오디오와 같은 미디어 하위 형식을 나타내는 문자열이며, **FindAllAsync**가 반환한 코덱에서 지원되어야 합니다. 이 매개 변수에 Null 또는 빈 문자열을 지정하면 모든 미디어 하위 형식에 대한 코덱을 반환합니다. 다음 예제는 현재 장치에 설치된 모든 비디오 인코더를 나열합니다.

[!code-cs[FindAllEncoders](./code/TranscodeWin10/cs/MainPage.xaml.cs#SnippetFindAllEncoders)]

미디어 하위 형식을 지정할 경우, [오디오 하위 형식 GUID](https://msdn.microsoft.com/library/windows/desktop/aa372553(v=vs.85).aspx) 또는 [비디오 하위 형식 GUID](https://msdn.microsoft.com/library/windows/desktop/aa370819(v=vs.85).aspx)에 나열된 하위 형식 GUID 중 하나의 문자열 표현을 사용해야 합니다. [CodecSubtypes](https://docs.microsoft.com/en-us/uwp/api/windows.media.core.codecsubtypes) 클래스는 대부분의 지원되는 하위 형식에 대한 하위 형식 GUID의 문자열 표현을 반환하는 속성을 제공합니다. 미디어 하위 형식 매개 변수로 FOURCC 코드를 지정할 수도 있습니다. 자세한 내용은 [FOURCC 코드](https://msdn.microsoft.com/library/windows/desktop/dd375802(v=vs.85).aspx)를 참조하세요. 

다음 예제에서는 H.264 비디오를 지원하는 비디오 디코더를 쿼리하는 FOURCC 코드를 사용합니다. 이 작업이 사용되는 시나리오의 예로는 비디오 파일을 재생하려고 시도하기 전에 지원되는 형식인지 확인하려는 경우가 있습니다.

[!code-cs[IsH264Supported](./code/TranscodeWin10/cs/MainPage.xaml.cs#SnippetIsH264Supported)]

다음 예제에서는 FLAC 미디어 하위 형식을 지원하는 오디오 인코더를 쿼리합니다. 그런 다음 이 예제에서는 미디어 하위 형식에 대한 AudioEncodinAudioEncodingProperties 개체와 MediaEncodingProfile을 만듭니다. 이는 오디오를 지정된 형식으로 녹음하거나 다른 형식으로 오디오의 코드를 변환하는 데 사용할 수 있습니다. 자세한 내용은 [MediaCapture를 사용하여 기본적인 사진, 비디오 및 오디오 캡처](basic-photo-video-and-audio-capture-with-MediaCapture.md)와 [미디어 파일 코드 변환](transcode-media-files.md)을 참조하세요.

[!code-cs[IsFLACSupported](./code/TranscodeWin10/cs/MainPage.xaml.cs#SnippetIsFLACSupported)]

## <a name="related-topics"></a>관련 항목

* [지원되는 코덱](supported-codecs.md)
* [MediaCapture를 사용하여 기본적인 사진, 비디오 및 오디오 캡처](basic-photo-video-and-audio-capture-with-MediaCapture.md)
* [미디어 파일 코드 변환](transcode-media-files.md)
 

 




