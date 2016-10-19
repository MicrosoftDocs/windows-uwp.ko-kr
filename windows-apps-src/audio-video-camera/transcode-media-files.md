---
author: drewbatgit
ms.assetid: A1A0D99A-DCBF-4A14-80B9-7106BEF045EC
description: "동영상 파일을 한 형식에서 다른 형식으로 코드 변환하려면 Windows.Media.Transcoding API를 사용할 수 있습니다."
title: "미디어 파일 코드 변환"
translationtype: Human Translation
ms.sourcegitcommit: 6530fa257ea3735453a97eb5d916524e750e62fc
ms.openlocfilehash: 7e96f12881e4f210a1bba57d2a9c298dbb1c32e3

---

# 미디어 파일 코드 변환

\[ Windows 10의 UWP 앱에 맞게 업데이트되었습니다. Windows 8.x 문서는 [보관](http://go.microsoft.com/fwlink/p/?linkid=619132)을 참조하세요. \]


동영상 파일을 한 형식에서 다른 형식으로 코드 변환하려면 [**Windows.Media.Transcoding**](https://msdn.microsoft.com/library/windows/apps/br207105) API를 사용할 수 있습니다.

*코드 변환*은 동영상 또는 오디오 파일과 같은 디지털 미디어 파일을 한 형식에서 다른 형식으로 변환하는 작업입니다. 이 작업은 일반적으로 파일을 디코드한 후 다시 인코드하여 수행됩니다. 예를 들어 MP4 형식을 지원하는 휴대용 디바이스에서 재생할 수 있도록 Windows Media 파일을 MP4로 변환할 수 있습니다. 또는 HD 동영상 파일을 저해상도로 변환할 수 있습니다. 이 경우 다시 인코딩된 파일은 원본 파일과 동일한 코덱을 사용할 수 있지만 다른 인코딩 프로필을 갖게 됩니다.

## 코드 변환에 대한 프로젝트 설정

기본 프로젝트 템플릿이 참조하는 네임스페이스 외에도, 이 문서에서 제공되는 코드를 사용하여 미디어 파일의 코드를 변환하려면 이러한 네임스페이스를 참조해야 합니다.

[!code-cs[사용](./code/TranscodeWin10/cs/MainPage.xaml.cs#SnippetUsing)]

## 원본 파일 및 대상 파일 선택

앱이 코드 변환을 위해 원본 파일 및 대상 파일을 결정하는 방법은 구현에 따라 다릅니다. 이 예제에서는 사용자가 원본 파일 및 대상 파일을 선택할 수 있도록 만들기 위해 [**FileOpenPicker**](https://msdn.microsoft.com/library/windows/apps/br207847) 및 [**FileSavePicker**](https://msdn.microsoft.com/library/windows/apps/br207871)를 사용합니다.

[!code-cs[TranscodeGetFile](./code/TranscodeWin10/cs/MainPage.xaml.cs#SnippetTranscodeGetFile)]

## 미디어 인코딩 프로필 만들기

인코딩 프로필에는 대상 파일의 인코드 방법을 결정하는 설정이 포함되어 있습니다. 파일을 코드 변환하는 경우 여기서 가장 많은 옵션을 사용할 수 있습니다.

[**MediaEncodingProfile**](https://msdn.microsoft.com/library/windows/apps/hh701026) 클래스는 미리 정의된 인코딩 프로필을 만들기 위한 정적 메서드를 제공합니다.

-   Wav
-   AAC 오디오(M4A)
-   MP3 오디오
-   WMA(Windows Media 오디오)
-   Avi
-   MP4 비디오(H.264 비디오 및 AAC 오디오)
-   WMV(Windows Media 비디오)

이 목록의 처음 네 개 프로필에는 오디오만 포함되어 있습니다. 다른 세 개의 프로필에는 비디오와 오디오가 포함되어 있습니다.

다음 코드는 MP4 동영상용 프로필을 만듭니다.

[!code-cs[TranscodeMediaProfile](./code/TranscodeWin10/cs/MainPage.xaml.cs#SnippetTranscodeMediaProfile)]

정적 [**CreateMp4**](https://msdn.microsoft.com/library/windows/apps/hh701078) 메서드는 MP4 인코딩 프로필을 만듭니다. 이 메서드의 매개 변수는 동영상의 대상 해상도를 제공합니다. 이 경우 [**VideoEncodingQuality.hd720p**](https://msdn.microsoft.com/library/windows/apps/hh701290)는 초당 30 프레임 속도에서 1280 x 720 픽셀을 의미합니다. "720p"는 프레임당 720 프로그레시브 스캔 선을 나타냅니다. 미리 정의된 프로필을 만드는 다른 모든 메서드도 이 패턴을 따릅니다.

또는 [**MediaEncodingProfile.CreateFromFileAsync**](https://msdn.microsoft.com/library/windows/apps/hh701047) 메서드를 사용하여 기존 미디어 파일과 일치하는 프로필을 만들 수 있습니다. 또는 원하는 인코딩 설정을 정확하게 알고 있는 경우 새 [**MediaEncodingProfile**](https://msdn.microsoft.com/library/windows/apps/hh701026) 개체를 만들고 프로필 세부 정보를 채울 수 있습니다.

## 파일 코드 변환

파일을 트랜스코딩하려면 새 [**MediaTranscoder**](https://msdn.microsoft.com/library/windows/apps/br207080) 개체를 만들고 [**MediaTranscoder.PrepareFileTranscodeAsync**](https://msdn.microsoft.com/library/windows/apps/hh700936) 메서드를 호출합니다. 원본 파일, 대상 파일 및 인코딩 프로필을 전달합니다. 그런 다음 비동기 코드 변환 작업에서 반환된 [**PrepareTranscodeResult**](https://msdn.microsoft.com/library/windows/apps/hh700941) 개체의 [**TranscodeAsync**](https://msdn.microsoft.com/library/windows/apps/hh700946) 메서드를 호출합니다.

[!code-cs[TranscodeTranscodeFile](./code/TranscodeWin10/cs/MainPage.xaml.cs#SnippetTranscodeTranscodeFile)]

## 코드 변환 진행률에 응답

비동기 [**TranscodeAsync**](https://msdn.microsoft.com/library/windows/apps/hh700946)의 진행이 변경되면 응답하도록 이벤트를 등록할 수 있습니다. 이러한 이벤트는 UWP(유니버설 Windows 플랫폼) 앱에 대한 비동기 프로그래밍 프레임워크의 일부이며 코드 변환 API와 관련이 없습니다.

[!code-cs[TranscodeCallbacks](./code/TranscodeWin10/cs/MainPage.xaml.cs#SnippetTranscodeCallbacks)]

 

 







<!--HONumber=Aug16_HO3-->


