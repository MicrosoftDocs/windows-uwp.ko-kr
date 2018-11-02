---
author: drewbatgit
ms.assetid: F28162D4-AACC-4EE0-B243-5878F870F87F
description: 미디어를 재생하는 동안 시스템에서 지원하는 메타데이터 신호 처리
title: 시스템에서 지원하는 시간이 제한된 메타데이터 신호
ms.author: drewbat
ms.date: 04/18/2017
ms.topic: article
keywords: Windows 10, uwp, 메타데이터, 신호, 음성, 챕터
ms.localizationpriority: medium
ms.openlocfilehash: 1e97c913764db24c68ce7becdba0fc283e1a3b73
ms.sourcegitcommit: 70ab58b88d248de2332096b20dbd6a4643d137a4
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/01/2018
ms.locfileid: "5930621"
---
# <a name="system-supported-timed-metadata-cues"></a>시스템에서 지원하는 시간이 제한된 메타데이터 신호
이 문서에서는 미디어 파일 또는 스트림에 포함될 수 있는 시간이 지정된 여러 형식의 메타데이터를 활용하는 방법을 설명합니다. UWP 앱은 이러한 메타데이터 신호가 발견될 때마다 재생하는 동안 미디어 파이프라인에 의해 발생하는 이벤트를 등록할 수 있습니다. [**DataCue**](https://docs.microsoft.com/uwp/api/Windows.Media.Core.DataCue) 클래스를 사용하여 앱은 자체 사용자 지정 메타데이터 신호를 구현할 수 있지만 이 문서는 다음과 같은 미디어 파이프라인이 자동으로 검색하는 몇 가지 메타데이터 표준에 초점을 맞춥니다.

* VobSub 형식의 이미지 기반 자막
* 단어 범위, 문장 경계 및 SSML(Speech Synthesis Markup Language) 책갈피를 포함한 음성 신호
* 챕터 신호
* 확장된 M3U 주석
* ID3 태그
* 조각화된 mp4 emsg 상자


이 문서는 [**MediaSource**](https://docs.microsoft.com/uwp/api/windows.media.core.mediasource), [**MediaPlaybackItem**](https://docs.microsoft.com/uwp/api/windows.media.playback.mediaplaybackitem) 및 [**TimedMetadataTrack**](https://msdn.microsoft.com/library/windows/apps/dn956580) 클래스를 포함하는 [미디어 항목, 재생 목록 및 트랙](media-playback-with-mediasource.md) 문서 및 앱에서 시간이 제한된 메타데이터를 사용하는 일반 가이드에서 설명한 개념에 기초하여 작성되었습니다.

기본 구현 단계는 이 문서에서 설명하는 모든 다른 종류의 시간이 제한된 메타데이터에 대해 동일합니다.

1. [**MediaSource**](https://docs.microsoft.com/uwp/api/windows.media.core.mediasource) 및 [**MediaPlaybackItem**](https://docs.microsoft.com/uwp/api/windows.media.playback.mediaplaybackitem)을 만들어 콘텐츠를 재생할 수 있습니다.
2. 미디어 파이프라인에 의해 미디어 항목의 하위 트랙이 확인될 때 발생하는 [**MediaPlaybackItem.TimedMetadataTracksChanged**](https://docs.microsoft.com/uwp/api/windows.media.playback.mediaplaybackitem.TimedMetadataTracksChanged) 이벤트를 등록합니다.
3. 사용하고자 하는 시간이 제한된 메타데이터 트랙에 대한 [**TimedMetadataTrack.CueEntered**](https://docs.microsoft.com/uwp/api/windows.media.core.timedmetadatatrack.CueEntered) 및 [**TimedMetadataTrack.CueExited**](https://docs.microsoft.com/uwp/api/windows.media.core.timedmetadatatrack.CueExited) 이벤트를 등록합니다.
4. **CueEntered** 이벤트 처리기에서 이벤트 인수에 전달된 메타데이터를 기반으로 UI를 업데이트합니다. 예를 들어, 현재 자막 텍스트를 제거하려면 **CueExited** 이벤트에서 UI를 다시 업데이트할 수 있습니다.

이 문서에서는 고유한 시나리오로 각 유형의 메타 데이터를 처리하지만 대부분 공유 코드를 사용하여 다른 유형의 메타데이터를 처리(또는 무시)할 수 있습니다. 프로세스의 다중 포인트에서 [**TimedMetadataTrack**](https://docs.microsoft.com/uwp/api/windows.media.core.timedmetadatatrack) 개체의 [**TimedMetadataKind**](https://docs.microsoft.com/uwp/api/windows.media.core.timedmetadatatrack.TimedMetadataKind) 속성을 확인할 수 있습니다. 따라서 예를 들어, **TimedMetadataKind.ImageSubtitle** 값을 지닌 메타 데이터 트랙에 대해 **CueEntered** 이벤트를 등록하도록 선택할 수 있지만 **TimedMetadataKind.Speech** 값을 지닌 트랙에 대해서는 불가능합니다. 또는 모든 메타데이터 트랙 형식을 등록한 다음 **CueEntered** 처리기 내에 있는 **TimedMetadataKind** 값을 확인하여 신호에 대한 응답으로 수행할 작업을 결정할 수 있습니다.

## <a name="image-based-subtitles"></a>이미지 기반 자막
Windows 10 버전 1703부터 UWP 앱은 VobSub 형식의 외부 이미지 기반 자막을 지원할 수 있습니다. 이 기능을 사용하려면 먼저 이미지 자막이 표시될 미디어 콘텐츠에 대한 [**MediaSource**](https://docs.microsoft.com/uwp/api/windows.media.core.mediasource) 개체를 만듭니다. 다음으로 [**CreateFromUriWithIndex**](https://docs.microsoft.com/uwp/api/windows.media.core.timedtextsource.CreateFromUriWithIndex) 또는 [**CreateFromStreamWithIndex**](https://docs.microsoft.com/uwp/api/windows.media.core.timedtextsource.CreateFromStreamWithIndex)를 호출하여 자막 이미지 데이터가 포함된 .sub 파일 및 자막의 타이밍 정보가 포함된 .idx 파일의 URI를 전달함으로써 [**TimedTextSource**](https://docs.microsoft.com/uwp/api/windows.media.core.timedtextsource) 개체를 만듭니다. **TimedTextSource**를 소스의 [**ExternalTimedTextSources**](https://docs.microsoft.com/uwp/api/windows.media.core.mediasource.ExternalTimedTextSources) 컬렉션에 추가하여 **MediaSource**에 추가합니다. **MediaSource**에서 [**MediaPlaybackItem**](https://docs.microsoft.com/uwp/api/windows.media.playback.mediaplaybackitem)을 만듭니다.

[!code-cs[ImageSubtitleLoadContent](./code/MediaSource_RS1/cs/MainPage_Cues.xaml.cs#SnippetImageSubtitleLoadContent)]

이전 단계에서 생성한 **MediaPlaybackItem** 개체를 사용하여 이미지 자막 메타데이터 이벤트를 등록합니다. 이 예제는 이벤트에 등록하기 위해 **RegisterMetadataHandlerForImageSubtitles** 도우미 메서드를 사용합니다. 람다 식은 시스템이 **MediaPlaybackItem**과 관련된 메타데이터 트랙의 변경을 발견할 때 발생하는 [**TimedMetadataTracksChanged**](https://docs.microsoft.com/uwp/api/windows.media.playback.mediaplaybackitem.TimedMetadataTracksChanged) 이벤트에 대한 처리기를 구현하는 데 사용됩니다. 경우에 따라 메타데이터 트랙은 재생 항목이 처음 지정될 때 사용 가능하기 때문에 **TimedMetadataTracksChanged** 처리기 외에서도 사용 가능한 메타데이터 트랙을 루핑하고 **RegisterMetadataHandlerForImageSubtitles**를 호출합니다.

[!code-cs[ImageSubtitleTracksChanged](./code/MediaSource_RS1/cs/MainPage_Cues.xaml.cs#SnippetImageSubtitleTracksChanged)]

이미지 자막 메타데이터 이벤트를 등록하면 **MediaItem**은 [**MediaPlayerElement**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.mediaplayerelement) 내 재생을 위해 [**MediaPlayer**](https://docs.microsoft.com/uwp/api/windows.media.playback.mediaplayer)에 할당됩니다.

[!code-cs[ImageSubtitlePlay](./code/MediaSource_RS1/cs/MainPage_Cues.xaml.cs#SnippetImageSubtitlePlay)]

**RegisterMetadataHandlerForImageSubtitles** 도우미 메서드에서 **MediaPlaybackItem**의 **TimedMetadataTracks** 컬렉션으로 인덱싱하여 [**TimedMetadataTrack**](https://docs.microsoft.com/uwp/api/windows.media.core.timedmetadatatrack) 클래스의 인스턴스를 가져옵니다. [**CueEntered**](https://docs.microsoft.com/uwp/api/windows.media.core.timedmetadatatrack.CueEntered) 이벤트 및 [**CueExited**](https://docs.microsoft.com/uwp/api/windows.media.core.timedmetadatatrack.CueExited) 이벤트를 등록합니다. 그런 다음 시스템에 앱이 재생 항목에 대한 메타데이터 신호 이벤트를 수신하기 원함을 지시하기 위해 재생 항목의 **TimedMetadataTracks** 컬렉션에 [**SetPresentationMode**](https://docs.microsoft.com/uwp/api/windows.media.playback.mediaplaybacktimedmetadatatracklist.SetPresentationMode)를 호출해야 합니다.

[!code-cs[RegisterMetadataHandlerForImageSubtitles](./code/MediaSource_RS1/cs/MainPage_Cues.xaml.cs#SnippetRegisterMetadataHandlerForImageSubtitles)]

**CueEntered** 이벤트 처리기에서 처리기에 전달되는 [**TimedMetadataTrack**](https://docs.microsoft.com/uwp/api/windows.media.core.timedmetadatatrack) 개체의 [**TimedMetadataKind**](https://docs.microsoft.com/uwp/api/windows.media.core.timedmetadatatrack.TimedMetadataKind) 속성을 살펴 이미지 자막에 대한 메타데이터를 확인할 수 있습니다. 여러 유형의 메타데이터에 대해 동일한 데이터 신호 이벤트 처리기를 사용하는 경우 필요합니다. 연결된 메타데이터 트랙이 **TimedMetadataKind.ImageSubtitle** 유형인 경우 [**MediaCueEventArgs**](https://docs.microsoft.com/uwp/api/windows.media.core.mediacueeventargs)의 **신호** 속성에 포함된 데이터 신호를 [**ImageCue**](https://docs.microsoft.com/uwp/api/windows.media.core.imagecue)에 캐스팅합니다. **ImageCue**의 [**SoftwareBitmap**](https://docs.microsoft.com/uwp/api/windows.media.core.imagecue.SoftwareBitmap) 속성은 자막 이미지의 [**SoftwareBitmap**](https://docs.microsoft.com/uwp/api/windows.graphics.imaging.softwarebitmap) 표시를 포함합니다. [**SoftwareBitmapSource**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.imaging.softwarebitmapsource)를 만들고 [**SetBitmapAsync**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.imaging.softwarebitmapsource.SetBitmapAsync)를 호출하여 이미지를 XAML [**이미지**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.image) 컨트롤에 할당합니다. **ImageCue**의 [**범위**](https://docs.microsoft.com/uwp/api/windows.media.core.imagecue.Extent) 및 [**위치**](https://docs.microsoft.com/uwp/api/windows.media.core.imagecue.Position) 속성은 자막 이미지의 크기와 위치에 대한 정보를 제공합니다.

[!code-cs[ImageSubtitleCueEntered](./code/MediaSource_RS1/cs/MainPage_Cues.xaml.cs#SnippetImageSubtitleCueEntered)]

## <a name="speech-cues"></a>음성 신호
Windows 10 버전 1703부터 UWP 앱은 재생되는 미디어의 단어 경계, 문장 경계 및 SSML(Speech Synthesis Markup Language) 책갈피에 응답하는 이벤트를 수신하도록 등록할 수 있습니다. 이를 사용하여 [**SpeechSynthesizer**](https://docs.microsoft.com/uwp/api/Windows.Media.SpeechSynthesis.SpeechSynthesizer) 클래스로 생성되는 오디오 스트림을 재생하여 현재 재생되는 단어 또는 문자의 텍스트를 표시하는 등 이벤트에 따라 UI를 업데이트할 수 있습니다.

이 섹션에 표시된 예제는 클래스 멤버 변수를 사용하여 합성되고 재생될 텍스트 문자열을 저장합니다.

[!code-cs[SpeechInputText](./code/MediaSource_RS1/cs/MainPage_Cues.xaml.cs#SnippetSpeechInputText)]

**SpeechSynthesizer** 클래스의 새 인스턴스를 만듭니다. 신시사이저에 대한 [**IncludeWordBoundaryMetadata**](https://docs.microsoft.com/uwp/api/windows.media.speechsynthesis.speechsynthesizeroptions.IncludeWordBoundaryMetadata) 및 [**IncludeSentenceBoundaryMetadata**](https://docs.microsoft.com/uwp/api/windows.media.speechsynthesis.speechsynthesizeroptions.IncludeSentenceBoundaryMetadata) 옵션을 **true**로 설정하여 메타데이터가 생성된 미디어 스트림에 포함되도록 지정합니다. [**SynthesizeTextToStreamAsync**](https://docs.microsoft.com/uwp/api/Windows.Media.SpeechSynthesis.SpeechSynthesizer.SynthesizeTextToStreamAsync)를 호출하여 합성된 음성 및 해당 메타데이터를 포함하는 스트림을 생성합니다. 합성 스트림에서 [**MediaSource**](https://docs.microsoft.com/uwp/api/windows.media.core.mediasource)와 [**MediaPlaybackItem**](https://docs.microsoft.com/uwp/api/windows.media.playback.mediaplaybackitem)을 만듭니다.

[!code-cs[SynthesizeSpeech](./code/MediaSource_RS1/cs/MainPage_Cues.xaml.cs#SnippetSynthesizeSpeech)]

**MediaPlaybackItem** 개체를 사용하여 음성 메타데이터 이벤트를 등록합니다. 이 예제는 이벤트에 등록하기 위해 **RegisterMetadataHandlerForSpeech** 도우미 메서드를 사용합니다. 람다 식은 시스템이 **MediaPlaybackItem**과 관련된 메타데이터 트랙의 변경을 발견할 때 발생하는 [**TimedMetadataTracksChanged**](https://docs.microsoft.com/uwp/api/windows.media.playback.mediaplaybackitem.TimedMetadataTracksChanged) 이벤트에 대한 처리기를 구현하는 데 사용됩니다.  경우에 따라 메타데이터 트랙은 재생 항목이 처음 지정될 때 사용 가능하기 때문에 **TimedMetadataTracksChanged** 처리기 외에서도 사용 가능한 메타데이터 트랙을 루핑하고 **RegisterMetadataHandlerForSpeech**를 호출합니다.

[!code-cs[SpeechTracksChanged](./code/MediaSource_RS1/cs/MainPage_Cues.xaml.cs#SnippetSpeechTracksChanged)]

음성 메타데이터 이벤트를 등록하면 **MediaItem**은 [**MediaPlayerElement**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.mediaplayerelement) 내 재생을 위해 [**MediaPlayer**](https://docs.microsoft.com/uwp/api/windows.media.playback.mediaplayer)에 할당됩니다.

[!code-cs[SpeechPlay](./code/MediaSource_RS1/cs/MainPage_Cues.xaml.cs#SnippetSpeechPlay)]

**RegisterMetadataHandlerForSpeech** 도우미 메서드에서 **MediaPlaybackItem**의 **TimedMetadataTracks** 컬렉션으로 인덱싱하여 [**TimedMetadataTrack**](https://docs.microsoft.com/uwp/api/windows.media.core.timedmetadatatrack) 클래스의 인스턴스를 가져옵니다. [**CueEntered**](https://docs.microsoft.com/uwp/api/windows.media.core.timedmetadatatrack.CueEntered) 이벤트 및 [**CueExited**](https://docs.microsoft.com/uwp/api/windows.media.core.timedmetadatatrack.CueExited) 이벤트를 등록합니다. 그런 다음 시스템에 앱이 재생 항목에 대한 메타데이터 신호 이벤트를 수신하기 원함을 지시하기 위해 재생 항목의 **TimedMetadataTracks** 컬렉션에 [**SetPresentationMode**](https://docs.microsoft.com/uwp/api/windows.media.playback.mediaplaybacktimedmetadatatracklist.SetPresentationMode)를 호출해야 합니다.

[!code-cs[RegisterMetadataHandlerForWords](./code/MediaSource_RS1/cs/MainPage_Cues.xaml.cs#SnippetRegisterMetadataHandlerForWords)]

**CueEntered** 이벤트 처리기에서 처리기에 전달되는 [**TimedMetadataTrack**](https://docs.microsoft.com/uwp/api/windows.media.core.timedmetadatatrack) 개체의 [**TimedMetadataKind**](https://docs.microsoft.com/uwp/api/windows.media.core.timedmetadatatrack.TimedMetadataKind) 속성을 살펴 음성에 대한 메타데이터를 확인할 수 있습니다. 여러 유형의 메타데이터에 대해 동일한 데이터 신호 이벤트 처리기를 사용하는 경우 필요합니다. 연결된 메타데이터 트랙이 **TimedMetadataKind.Speech** 유형인 경우 [**MediaCueEventArgs**](https://docs.microsoft.com/uwp/api/windows.media.core.mediacueeventargs)의 **신호** 속성에 포함된 데이터 신호를 [**SpeechCue**](https://docs.microsoft.com/uwp/api/windows.media.core.speechcue)에 캐스팅합니다. 음성 신호의 경우 메타데이터 트랙에 포함된 음성 신호의 유형은 **레이블** 속성을 통해 확인할 수 있습니다. 이 속성 값은 문장 경계에 대해 "SpeechWord", 문장 경계에 대해 "SpeechSentence", 또는 SSML 책갈피에 대해 "SpeechBookmark"입니다. 이 예제에서는 "SpeechWord" 값을 확인하고 값이 발견되는 경우 **SpeechCue**의 [**StartPositionInInput**](https://docs.microsoft.com/uwp/api/windows.media.core.speechcue.StartPositionInInput) 및 [**EndPositionInInput**](https://docs.microsoft.com/uwp/api/windows.media.core.speechcue.EndPositionInInput) 속성은 현재 재생되는 단어의 입력 텍스트 내 위치를 결정하는 데 사용됩니다. 이 예제는 단순히 각 단어를 디버그 출력으로 출력합니다.

[!code-cs[SpeechWordCueEntered](./code/MediaSource_RS1/cs/MainPage_Cues.xaml.cs#SnippetSpeechWordCueEntered)]

## <a name="chapter-cues"></a>챕터 신호
Windows 10 버전 1703부터 UWP 앱은 미디어 항목 내의 챕터에 해당하는 신호를 등록할 수 있습니다. 이 기능을 사용하려면 미디어 콘텐츠에 대한 [**MediaSource**](https://docs.microsoft.com/uwp/api/windows.media.core.mediasource) 개체를 만든 다음 **MediaSource** 에서 [**MediaPlaybackItem**](https://docs.microsoft.com/uwp/api/windows.media.playback.mediaplaybackitem)을 만듭니다.

[!code-cs[ChapterCueLoadContent](./code/MediaSource_RS1/cs/MainPage_Cues.xaml.cs#SnippetChapterCueLoadContent)]

이전 단계에서 생성한 **MediaPlaybackItem** 개체를 사용하여 챕터 메타데이터 이벤트를 등록합니다. 이 예제는 이벤트에 등록하기 위해 **RegisterMetadataHandlerForChapterCues** 도우미 메서드를 사용합니다. 람다 식은 시스템이 **MediaPlaybackItem**과 관련된 메타데이터 트랙의 변경을 발견할 때 발생하는 [**TimedMetadataTracksChanged**](https://docs.microsoft.com/uwp/api/windows.media.playback.mediaplaybackitem.TimedMetadataTracksChanged) 이벤트에 대한 처리기를 구현하는 데 사용됩니다. 경우에 따라 메타데이터 트랙은 재생 항목이 처음 지정될 때 사용 가능하기 때문에 **TimedMetadataTracksChanged** 처리기 외에서도 사용 가능한 메타데이터 트랙을 루핑하고 **RegisterMetadataHandlerForChapterCues**를 호출합니다.

[!code-cs[ChapterCueTracksChanged](./code/MediaSource_RS1/cs/MainPage_Cues.xaml.cs#SnippetChapterCueTracksChanged)]

챕터 메타데이터 이벤트를 등록하면 **MediaItem**은 [**MediaPlayerElement**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.mediaplayerelement) 내 재생에 대해 [**MediaPlayer**](https://docs.microsoft.com/uwp/api/windows.media.playback.mediaplayer)에 할당됩니다.

[!code-cs[ChapterCuePlay](./code/MediaSource_RS1/cs/MainPage_Cues.xaml.cs#SnippetChapterCuePlay)]

**RegisterMetadataHandlerForChapterCues** 도우미 메서드에서 **MediaPlaybackItem**의 **TimedMetadataTracks** 컬렉션으로 인덱싱하여 [**TimedMetadataTrack**](https://docs.microsoft.com/uwp/api/windows.media.core.timedmetadatatrack) 클래스의 인스턴스를 가져옵니다. [**CueEntered**](https://docs.microsoft.com/uwp/api/windows.media.core.timedmetadatatrack.CueEntered) 이벤트 및 [**CueExited**](https://docs.microsoft.com/uwp/api/windows.media.core.timedmetadatatrack.CueExited) 이벤트를 등록합니다. 그런 다음 시스템에 앱이 재생 항목에 대한 메타데이터 신호 이벤트를 수신하기 원함을 지시하기 위해 재생 항목의 **TimedMetadataTracks** 컬렉션에 [**SetPresentationMode**](https://docs.microsoft.com/uwp/api/windows.media.playback.mediaplaybacktimedmetadatatracklist.SetPresentationMode)를 호출해야 합니다.

[!code-cs[RegisterMetadataHandlerForChapterCues](./code/MediaSource_RS1/cs/MainPage_Cues.xaml.cs#SnippetRegisterMetadataHandlerForChapterCues)]

**CueEntered** 이벤트 처리기에서 처리기에 전달되는 [**TimedMetadataTrack**](https://docs.microsoft.com/uwp/api/windows.media.core.timedmetadatatrack) 개체의 [**TimedMetadataKind**](https://docs.microsoft.com/uwp/api/windows.media.core.timedmetadatatrack.TimedMetadataKind) 속성을 살펴 챕터 신호에 대한 메타데이터를 확인할 수 있습니다. 이것은 여러 유형의 메타데이터에 대해 동일한 데이터 신호 이벤트 처리기를 사용하는 경우 필요합니다. 연결된 메타데이터 트랙이 **TimedMetadataKind.Chapter** 유형인 경우 [**MediaCueEventArgs**](https://docs.microsoft.com/uwp/api/windows.media.core.mediacueeventargs)의 **신호** 속성에 포함된 데이터 신호를 [**ChapterCue**](https://docs.microsoft.com/uwp/api/windows.media.core.chaptercue)에 캐스팅합니다. **ChapterCue**의 [**제목**](https://docs.microsoft.com/uwp/api/windows.media.core.chaptercue.Title) 속성은 재생에 막 도달한 챕터 제목을 포함합니다.

[!code-cs[ChapterCueEntered](./code/MediaSource_RS1/cs/MainPage_Cues.xaml.cs#SnippetChapterCueEntered)]

### <a name="seek-to-the-next-chapter-using-chapter-cues"></a>챕터 신호를 사용하여 다음 챕터 찾기
재생 중인 항목에서 현재 챕터가 변경될 때 알림을 수신하는 것 외에 챕터 신호를 사용하여 재생 중인 항목의 다음 챕터를 검색할 수 있습니다. 아래에 표시된 예제 메서드는 현재 재생 중인 미디어 항목을 나타내는 [**MediaPlayer**](https://docs.microsoft.com/uwp/api/windows.media.playback.mediaplayer) 및 [**MediaPlaybackItem**](https://docs.microsoft.com/uwp/api/windows.media.playback.mediaplaybackitem)을 인수로 나타냅니다. 어떤 트랙이라도 **TimedMetadataKind.Chapter**의 [**TimedMetadataTrack**](https://docs.microsoft.com/uwp/api/windows.media.core.timedmetadatatrack) 값의 [**TimedMetadataKind**](https://docs.microsoft.com/uwp/api/windows.media.core.timedmetadatatrack.TimedMetadataKind) 속성을 지니는지 확인하기 위해 [**TimedMetadataTracks**](https://docs.microsoft.com/uwp/api/windows.media.playback.mediaplaybackitem.TimedMetadataTracks) 컬렉션이 검색됩니다.  챕터 트랙이 발견되면 메서드는 트랙의 [**신호**](https://docs.microsoft.com/uwp/api/windows.media.core.timedmetadatatrack.Cues) 컬렉션을 루핑하여 미디어 플레이어의 재생 세션의 [**StartTime**](https://docs.microsoft.com/uwp/api/windows.media.core.chaptercue.StartTime)이 현재 [**위치**](https://docs.microsoft.com/uwp/api/windows.media.playback.mediaplaybacksession.Position)보다 큰 처음 신호를 찾습니다. 올바른 신호가 발견되면 재생 세션의 위치가 업데이트되며 UI에서 챕터 제목이 업데이트됩니다.

[!code-cs[GoToNextChapter](./code/MediaSource_RS1/cs/MainPage_Cues.xaml.cs#SnippetGoToNextChapter)]

## <a name="extended-m3u-comments"></a>확장된 M3U 주석
Windows 10 버전 1703부터 UWP 앱은 확장된 M3U 매니페스트 파일 내에서 주석에 해당하는 신호를 등록할 수 있습니다. 이 예제는 [**AdaptiveMediaSource**](https://docs.microsoft.com/uwp/api/windows.media.streaming.adaptive.adaptivemediasource)를 사용하여 미디어 콘텐츠를 재생합니다. 자세한 내용은 [적응 스트리밍](adaptive-streaming.md)을 참조하세요. [**CreateFromUriAsync**](https://docs.microsoft.com/uwp/api/windows.media.streaming.adaptive.adaptivemediasource.CreateFromUriAsync) 또는 [**CreateFromStreamAsync**](https://docs.microsoft.com/uwp/api/windows.media.streaming.adaptive.adaptivemediasource.CreateFromStreamAsync)를 호출하여 콘텐츠에 대한 **AdaptiveMediaSource**를 만듭니다. [**CreateFromAdaptiveMediaSource**](https://docs.microsoft.com/uwp/api/windows.media.core.mediasource.CreateFromAdaptiveMediaSource)를 호출하여 [**MediaSource**](https://docs.microsoft.com/uwp/api/windows.media.core.mediasource) 개체를 만든 다음 **MediaSource**에서 [**MediaPlaybackItem**](https://docs.microsoft.com/uwp/api/windows.media.playback.mediaplaybackitem)을 만듭니다.

[!code-cs[EXTM3ULoadContent](./code/MediaSource_RS1/cs/MainPage_Cues.xaml.cs#SnippetEXTM3ULoadContent)]

이전 단계에서 생성한 **MediaPlaybackItem** 개체를 사용하여 M3U 메타데이터 이벤트를 등록합니다. 이 예제는 이벤트에 등록하기 위해 **RegisterMetadataHandlerForEXTM3UCues** 도우미 메서드를 사용합니다. 람다 식은 시스템이 **MediaPlaybackItem**과 관련된 메타데이터 트랙의 변경을 발견할 때 발생하는 [**TimedMetadataTracksChanged**](https://docs.microsoft.com/uwp/api/windows.media.playback.mediaplaybackitem.TimedMetadataTracksChanged) 이벤트에 대한 처리기를 구현하는 데 사용됩니다. 경우에 따라 메타데이터 트랙은 재생 항목이 처음 지정될 때 사용 가능하기 때문에 **TimedMetadataTracksChanged** 처리기 외에서도 사용 가능한 메타데이터 트랙을 루핑하고 **RegisterMetadataHandlerForEXTM3UCues**를 호출합니다.

[!code-cs[EXTM3UCueTracksChanged](./code/MediaSource_RS1/cs/MainPage_Cues.xaml.cs#SnippetEXTM3UCueTracksChanged)]

M3U 메타데이터 이벤트를 등록하면 **MediaItem**은 [**MediaPlayerElement**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.mediaplayerelement) 내 재생에 대해 [**MediaPlayer**](https://docs.microsoft.com/uwp/api/windows.media.playback.mediaplayer)에 할당됩니다.

[!code-cs[EXTM3UCuePlay](./code/MediaSource_RS1/cs/MainPage_Cues.xaml.cs#SnippetEXTM3UCuePlay)]

**RegisterMetadataHandlerForEXTM3UCues** 도우미 메서드에서 **MediaPlaybackItem**의 **TimedMetadataTracks** 컬렉션으로 인덱싱하여 [**TimedMetadataTrack**](https://docs.microsoft.com/uwp/api/windows.media.core.timedmetadatatrack) 클래스의 인스턴스를 가져옵니다. 트랙이 M3U 주석을 나타내는 경우 "EXTM3U" 값을 갖는 메타데이터 트랙의 DispatchType 속성을 확인합니다. [**CueEntered**](https://docs.microsoft.com/uwp/api/windows.media.core.timedmetadatatrack.CueEntered) 이벤트 및 [**CueExited**](https://docs.microsoft.com/uwp/api/windows.media.core.timedmetadatatrack.CueExited) 이벤트를 등록합니다. 그런 다음 시스템에 앱이 재생 항목에 대한 메타데이터 신호 이벤트를 수신하기 원함을 지시하기 위해 재생 항목의 **TimedMetadataTracks** 컬렉션에 [**SetPresentationMode**](https://docs.microsoft.com/uwp/api/windows.media.playback.mediaplaybacktimedmetadatatracklist.SetPresentationMode)를 호출해야 합니다.

[!code-cs[RegisterMetadataHandlerForEXTM3UCues](./code/MediaSource_RS1/cs/MainPage_Cues.xaml.cs#SnippetRegisterMetadataHandlerForEXTM3UCues)]

**CueEntered** 이벤트에 대한 처리기에서 [**MediaCueEventArgs**](https://docs.microsoft.com/uwp/api/windows.media.core.mediacueeventargs)의 **신호** 속성에 포함된 데이터 신호를 [**DataCue**](https://docs.microsoft.com/uwp/api/windows.media.core.datacue)로 캐스팅합니다.  신호의 **DataCue** 및 [**데이터**](https://docs.microsoft.com/uwp/api/windows.media.core.datacue.Data) 속성이 null이 아닌지 확인합니다. 확장된 EMU 주석은 UTF-16 형식으로, Little-Endian, null로 끝나는 문자열로 제공됩니다. [**DataReader.FromBuffer**](https://docs.microsoft.com/uwp/api/windows.storage.streams.datareader.FromBuffer)를 호출하여 새 **DataReader**를 만들어 신호 데이터를 읽을 수 있습니다. 리더의 [**UnicodeEncoding**](https://docs.microsoft.com/uwp/api/windows.storage.streams.datareader.UnicodeEncoding) 속성을 [**Utf16LE**](https://docs.microsoft.com/uwp/api/windows.storage.streams.unicodeencoding)로 설정하여 올바른 유형의 데이터를 읽을 수 있습니다. [**ReadString**](https://docs.microsoft.com/uwp/api/windows.storage.streams.datareader.ReadString)을 호출하여 데이터를 읽고 각 문자의 크기는 2바이트기 때문에 **데이터** 필드의 길이를 반으로 지정하고 하나를 빼 후행 null 문자를 제거할 수 있습니다. 여기에서 M3U 주석은 단순히 디버그 출력에 기록됩니다.

[!code-cs[EXTM3UCueEntered](./code/MediaSource_RS1/cs/MainPage_Cues.xaml.cs#SnippetEXTM3UCueEntered)]

## <a name="id3-tags"></a>ID3 태그
Windows 10 버전 1703부터 UWP 앱은 HLS(HTTP 라이브 스트리밍) 콘텐츠 내의 ID3 태그에 해당하는 신호를 등록할 수 있습니다. 이 예제는 [**AdaptiveMediaSource**](https://docs.microsoft.com/uwp/api/windows.media.streaming.adaptive.adaptivemediasource)를 사용하여 미디어 콘텐츠를 재생합니다. 자세한 내용은 [적응 스트리밍](adaptive-streaming.md)을 참조하세요. [**CreateFromUriAsync**](https://docs.microsoft.com/uwp/api/windows.media.streaming.adaptive.adaptivemediasource.CreateFromUriAsync) 또는 [**CreateFromStreamAsync**](https://docs.microsoft.com/uwp/api/windows.media.streaming.adaptive.adaptivemediasource.CreateFromStreamAsync)를 호출하여 콘텐츠에 대한 **AdaptiveMediaSource**를 만듭니다. [**CreateFromAdaptiveMediaSource**](https://docs.microsoft.com/uwp/api/windows.media.core.mediasource.CreateFromAdaptiveMediaSource)를 호출하여 [**MediaSource**](https://docs.microsoft.com/uwp/api/windows.media.core.mediasource) 개체를 만든 다음 **MediaSource**에서 [**MediaPlaybackItem**](https://docs.microsoft.com/uwp/api/windows.media.playback.mediaplaybackitem)을 만듭니다.

[!code-cs[EXTM3ULoadContent](./code/MediaSource_RS1/cs/MainPage_Cues.xaml.cs#SnippetEXTM3ULoadContent)]

이전 단계에서 생성한 **MediaPlaybackItem** 개체를 사용하여 ID3 태그 이벤트를 등록합니다. 이 예제는 이벤트에 등록하기 위해 **RegisterMetadataHandlerForID3Cues** 도우미 메서드를 사용합니다. 람다 식은 시스템이 **MediaPlaybackItem**과 관련된 메타데이터 트랙의 변경을 발견할 때 발생하는 [**TimedMetadataTracksChanged**](https://docs.microsoft.com/uwp/api/windows.media.playback.mediaplaybackitem.TimedMetadataTracksChanged) 이벤트에 대한 처리기를 구현하는 데 사용됩니다. 경우에 따라 메타데이터 트랙은 재생 항목이 처음 지정될 때 사용 가능하기 때문에 **TimedMetadataTracksChanged** 처리기 외에서도 사용 가능한 메타데이터 트랙을 루핑하고 **RegisterMetadataHandlerForID3Cues**를 호출합니다.

[!code-cs[ID3LoadContent](./code/MediaSource_RS1/cs/MainPage_Cues.xaml.cs#SnippetID3LoadContent)]

ID3 메타데이터 이벤트를 등록하면 **MediaItem**은 [**MediaPlayerElement**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.mediaplayerelement) 내 재생에 대해 [**MediaPlayer**](https://docs.microsoft.com/uwp/api/windows.media.playback.mediaplayer)에 할당됩니다.

[!code-cs[ID3CuePlay](./code/MediaSource_RS1/cs/MainPage_Cues.xaml.cs#SnippetID3CuePlay)]


**RegisterMetadataHandlerForID3Cues** 도우미 메서드에서 **MediaPlaybackItem**의 **TimedMetadataTracks** 컬렉션으로 인덱싱하여 [**TimedMetadataTrack**](https://docs.microsoft.com/uwp/api/windows.media.core.timedmetadatatrack) 클래스의 인스턴스를 가져옵니다. 트랙이 ID3 태그를 나타내는 경우 "15260DFFFF49443320FF49443320000F" GUID 문자열을 포함하는 값을 지닌 메타데이터 트랙의 DispatchType 속성을 확인합니다. [**CueEntered**](https://docs.microsoft.com/uwp/api/windows.media.core.timedmetadatatrack.CueEntered) 이벤트 및 [**CueExited**](https://docs.microsoft.com/uwp/api/windows.media.core.timedmetadatatrack.CueExited) 이벤트를 등록합니다. 그런 다음 시스템에 앱이 재생 항목에 대한 메타데이터 신호 이벤트를 수신하기 원함을 지시하기 위해 재생 항목의 **TimedMetadataTracks** 컬렉션에 [**SetPresentationMode**](https://docs.microsoft.com/uwp/api/windows.media.playback.mediaplaybacktimedmetadatatracklist.SetPresentationMode)를 호출해야 합니다.

[!code-cs[RegisterMetadataHandlerForID3Cues](./code/MediaSource_RS1/cs/MainPage_Cues.xaml.cs#SnippetRegisterMetadataHandlerForID3Cues)]

**CueEntered** 이벤트에 대한 처리기에서 [**MediaCueEventArgs**](https://docs.microsoft.com/uwp/api/windows.media.core.mediacueeventargs)의 **신호** 속성에 포함된 데이터 신호를 [**DataCue**](https://docs.microsoft.com/uwp/api/windows.media.core.datacue)로 캐스팅합니다.  신호의 **DataCue** 및 [**데이터**](https://docs.microsoft.com/uwp/api/windows.media.core.datacue.Data) 속성이 null이 아닌지 확인합니다. 확장된 EMU 주석은 전송 스트림에서 원시 바이트 형태로 제공됩니다([http://id3.org/id3v2.4.0-structure](http://id3.org/id3v2.4.0-structure) 참조). [**DataReader.FromBuffer**](https://docs.microsoft.com/uwp/api/windows.storage.streams.datareader.FromBuffer)를 호출하여 새 **DataReader**를 만들어 신호 데이터를 읽을 수 있습니다.  이 예제에서 ID3 태그의 헤더 값은 신호 데이터에서 읽고 디버그 출력에 기록됩니다.

[!code-cs[ID3CueEntered](./code/MediaSource_RS1/cs/MainPage_Cues.xaml.cs#SnippetID3CueEntered)]

## <a name="fragmented-mp4-emsg-boxes"></a>조각화된 mp4 emsg 상자
Windows 10 버전 1703부터 UWP 앱은 조각화된 mp4 스트림 내 emsg 상자에 해당하는 신호를 등록할 수 있습니다. 이러한 유형의 메타데이터를 사용하는 예는 콘텐츠 공급자를 위해 클라이언트 응용 프로그램에 라이브 스트리밍 콘텐츠 동안 광고를 재생하도록 신호를 주는 경우입니다. 이 예제는 [**AdaptiveMediaSource**](https://docs.microsoft.com/uwp/api/windows.media.streaming.adaptive.adaptivemediasource)를 사용하여 미디어 콘텐츠를 재생합니다. 자세한 내용은 [적응 스트리밍](adaptive-streaming.md)을 참조하세요. [**CreateFromUriAsync**](https://docs.microsoft.com/uwp/api/windows.media.streaming.adaptive.adaptivemediasource.CreateFromUriAsync) 또는 [**CreateFromStreamAsync**](https://docs.microsoft.com/uwp/api/windows.media.streaming.adaptive.adaptivemediasource.CreateFromStreamAsync)를 호출하여 콘텐츠에 대한 **AdaptiveMediaSource**를 만듭니다. [**CreateFromAdaptiveMediaSource**](https://docs.microsoft.com/uwp/api/windows.media.core.mediasource.CreateFromAdaptiveMediaSource)를 호출하여 [**MediaSource**](https://docs.microsoft.com/uwp/api/windows.media.core.mediasource) 개체를 만든 다음 **MediaSource**에서 [**MediaPlaybackItem**](https://docs.microsoft.com/uwp/api/windows.media.playback.mediaplaybackitem)을 만듭니다.

[!code-cs[EmsgLoadContent](./code/MediaSource_RS1/cs/MainPage_Cues.xaml.cs#SnippetEmsgLoadContent)]

이전 단계에서 생성한 **MediaPlaybackItem** 개체를 사용하여 emsg 상자 이벤트를 등록합니다. 이 예제는 이벤트에 등록하기 위해 **RegisterMetadataHandlerForEmsgCues** 도우미 메서드를 사용합니다. 람다 식은 시스템이 **MediaPlaybackItem**과 관련된 메타데이터 트랙의 변경을 발견할 때 발생하는 [**TimedMetadataTracksChanged**](https://docs.microsoft.com/uwp/api/windows.media.playback.mediaplaybackitem.TimedMetadataTracksChanged) 이벤트에 대한 처리기를 구현하는 데 사용됩니다. 경우에 따라 메타데이터 트랙은 재생 항목이 처음 지정될 때 사용 가능하기 때문에 **TimedMetadataTracksChanged** 처리기 외에서도 사용 가능한 메타데이터 트랙을 루핑하고 **RegisterMetadataHandlerForEmsgCues**를 호출합니다.

[!code-cs[ID3LoadContent](./code/MediaSource_RS1/cs/MainPage_Cues.xaml.cs#SnippetID3LoadContent)]

emsg 상자 메타데이터 이벤트를 등록하면 **MediaItem**은 [**MediaPlayerElement**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.mediaplayerelement) 내 재생에 대해 [**MediaPlayer**](https://docs.microsoft.com/uwp/api/windows.media.playback.mediaplayer)에 할당됩니다.

[!code-cs[EmsgCuePlay](./code/MediaSource_RS1/cs/MainPage_Cues.xaml.cs#SnippetEmsgCuePlay)]


**RegisterMetadataHandlerForEmsgCues** 도우미 메서드에서 **MediaPlaybackItem**의 **TimedMetadataTracks** 컬렉션으로 인덱싱하여 [**TimedMetadataTrack**](https://docs.microsoft.com/uwp/api/windows.media.core.timedmetadatatrack) 클래스의 인스턴스를 가져옵니다. 트랙이 emsg 상자를 나타내는 경우 "emsg:mp4" 값을 갖는 메타데이터 트랙의 DispatchType 속성을 확인합니다. [**CueEntered**](https://docs.microsoft.com/uwp/api/windows.media.core.timedmetadatatrack.CueEntered) 이벤트 및 [**CueExited**](https://docs.microsoft.com/uwp/api/windows.media.core.timedmetadatatrack.CueExited) 이벤트를 등록합니다. 그런 다음 시스템에 앱이 재생 항목에 대한 메타데이터 신호 이벤트를 수신하기 원함을 지시하기 위해 재생 항목의 **TimedMetadataTracks** 컬렉션에 [**SetPresentationMode**](https://docs.microsoft.com/uwp/api/windows.media.playback.mediaplaybacktimedmetadatatracklist.SetPresentationMode)를 호출해야 합니다.


[!code-cs[RegisterMetadataHandlerForEmsgCues](./code/MediaSource_RS1/cs/MainPage_Cues.xaml.cs#SnippetRegisterMetadataHandlerForEmsgCues)]


**CueEntered** 이벤트에 대한 처리기에서 [**MediaCueEventArgs**](https://docs.microsoft.com/uwp/api/windows.media.core.mediacueeventargs)의 **신호** 속성에 포함된 데이터 신호를 [**DataCue**](https://docs.microsoft.com/uwp/api/windows.media.core.datacue)로 캐스팅합니다.  **DataCue** 개체가 null이 아닌지 확인합니다. emsg 상자의 속성은 미디어 파이프라인에 의해 DataCue 개체의 [**속성**](https://docs.microsoft.com/uwp/api/windows.media.core.datacue.Properties) 컬렉션 내에 사용자 지정 속성으로 제공됩니다. 이 예제는 **[TryGetValue](https://docs.microsoft.com/uwp/api/windows.foundation.collections.propertyset.trygetvalue)** 메서드를 사용하여 여러 다른 속성 값을 추출해 봅니다. 이 메서드가 null을 반환하면 요청한 속성이 emsg 상자에 나타나지 않는다는 의미이므로 대신 기본값이 설정됩니다.

예제의 다음 부분은 이전 단계에서 얻은 *scheme_id_uri* 속성이 "urn: scte:scte35:2013:xml" 값을 가진 경우 광고 재생이 트리거되는 시나리오를 보여 줍니다([http://dashif.org/identifiers/event-schemes/](http://dashif.org/identifiers/event-schemes/) 참조). 표준 방법으로 중복성에 대해 이 emsg를 여러 번 전송하는 것이 권장되므로 이 예제는 이미 처리된 emsg ID의 목록을 유지하고 새 메시지만으르 처리합니다. [**DataReader.FromBuffer**](https://docs.microsoft.com/uwp/api/windows.storage.streams.datareader.FromBuffer)를 호출하여 새 **DataReader**를 만들어 신호 데이터를 읽고 [**UnicodeEncoding**](https://docs.microsoft.com/uwp/api/windows.storage.streams.datareader.UnicodeEncoding) 속성을 설정하여 인코딩을 UTF-8로 설정한 다음 데이터를 읽을 수 있습니다. 이 예제에서 메시지 페이로드는 디버그 출력에 기록됩니다. 실제 앱은 페이로드 데이터를 사용하여 광고 재생 일정을 예약합니다.

[!code-cs[EmsgCueEntered](./code/MediaSource_RS1/cs/MainPage_Cues.xaml.cs#SnippetEmsgCueEntered)]


## <a name="related-topics"></a>관련 항목

* [미디어 재생](media-playback.md)
* [미디어 항목, 재생 목록 및 트랙](media-playback-with-mediasource.md)


 




