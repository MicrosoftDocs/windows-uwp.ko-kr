---
ms.assetid: F28162D4-AACC-4EE0-B243-5878F870F87F
description: 미디어 파일 또는 스트림에 포함 될 수 있는 몇 가지 형식의 시간 메타 데이터를 활용 하는 방법에 대해 알아봅니다.
title: 시스템에서 지원하는 시간이 제한된 메타데이터 신호
ms.date: 04/18/2017
ms.topic: article
keywords: windows 10, uwp, 메타 데이터, 큐, 음성, 챕터
ms.localizationpriority: medium
ms.openlocfilehash: 8991a2044caa20a4441d4b30d5359b6d4c6a8fa1
ms.sourcegitcommit: 5d34eb13c7b840c05e5394910a22fa394097dc36
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/28/2020
ms.locfileid: "89053603"
---
# <a name="system-supported-timed-metadata-cues"></a>시스템에서 지원하는 시간이 제한된 메타데이터 신호
이 문서에서는 미디어 파일 또는 스트림에 포함 될 수 있는 몇 가지 형식의 시간 메타 데이터를 활용 하는 방법을 설명 합니다. UWP 앱은 이러한 메타 데이터 큐가 나타날 때마다 재생 하는 동안 미디어 파이프라인에서 발생 하는 이벤트를 등록할 수 있습니다. 응용 프로그램은 [**Datacue**](https://docs.microsoft.com/uwp/api/Windows.Media.Core.DataCue) 클래스를 사용 하 여 고유한 사용자 지정 메타 데이터 큐를 구현할 수 있지만이 문서에서는 다음을 포함 하 여 미디어 파이프라인이 자동으로 검색 하는 몇 가지 메타 데이터 표준에 중점을 둘 수 있습니다

* VobSub 형식의 이미지 기반 자막
* 단어 경계, 문장 경계 및 SSML (음성 합성 마크업 언어) 책갈피를 비롯 한 음성 신호
* 장 큐
* 확장 된 M3U 주석
* ID3 태그
* 조각화 된 mp4 emsg 상자


이 문서는 [미디어 항목, 재생 목록 및 트랙](media-playback-with-mediasource.md)문서에 설명 된 개념을 기반으로 합니다. 여기에는 [**MediaSource**](https://docs.microsoft.com/uwp/api/windows.media.core.mediasource), [**Mediaplaybackitem**](https://docs.microsoft.com/uwp/api/windows.media.playback.mediaplaybackitem)및 [**TimedMetadataTrack**](https://docs.microsoft.com/uwp/api/Windows.Media.Core.TimedMetadataTrack) 클래스를 사용 하는 기본 사항과 앱에서 시간 제한 메타 데이터를 사용 하기 위한 일반 지침이 포함 되어 있습니다.

기본 구현 단계는이 문서에서 설명 하는 다양 한 유형의 시간 제한 메타 데이터에 대해 동일 합니다.

1. [**MediaSource**](https://docs.microsoft.com/uwp/api/windows.media.core.mediasource) 하 고 재생할 콘텐츠에 대 한 [**Mediaplaybackitem**](https://docs.microsoft.com/uwp/api/windows.media.playback.mediaplaybackitem) 을 만듭니다.
2. 미디어 파이프라인에서 미디어 항목의 하위 트랙이 확인 될 때 발생 하는 [**Mediaplaybackitem**](https://docs.microsoft.com/uwp/api/windows.media.playback.mediaplaybackitem.TimedMetadataTracksChanged) 이벤트에 등록 합니다.
3. 사용 하려는 시간이 지정 된 메타 데이터 트랙에 대해 [**TimedMetadataTrack**](https://docs.microsoft.com/uwp/api/windows.media.core.timedmetadatatrack.CueEntered) 및 [**CueExited**](https://docs.microsoft.com/uwp/api/windows.media.core.timedmetadatatrack.CueExited) 이벤트를 등록 합니다.
4. **CueEntered** 이벤트 처리기에서 이벤트 인수에 전달 된 메타 데이터를 기반으로 UI를 업데이트 합니다. **CueExited** 이벤트에서 예를 들어, UI를 다시 업데이트 하 여 현재 부제목 텍스트를 제거할 수 있습니다.

이 문서에서는 각 유형의 메타 데이터를 처리 하는 것이 고유한 시나리오로 표시 되지만 대부분 공유 코드를 사용 하 여 다양 한 유형의 메타 데이터를 처리 (또는 무시) 할 수 있습니다. 프로세스의 여러 지점에서 [**TimedMetadataTrack**](https://docs.microsoft.com/uwp/api/windows.media.core.timedmetadatatrack) 개체의 [**TimedMetadataKind**](https://docs.microsoft.com/uwp/api/windows.media.core.timedmetadatatrack.TimedMetadataKind) 속성을 확인할 수 있습니다. 예를 들어 TimedMetadataKind **값을**포함 하는 **CueEntered** 값을 포함 하는 메타 데이터 트랙에 대해 **ImageSubtitle**이벤트를 등록 하도록 선택할 수 있습니다. 또는 대신 모든 메타 데이터 트랙 형식에 대 한 처리기를 등록 한 다음 **CueEntered** 처리기 내에서 **TimedMetadataKind** 값을 확인 하 여 큐에 대 한 응답으로 수행할 동작을 결정할 수 있습니다.

## <a name="image-based-subtitles"></a>이미지 기반 자막
Windows 10, 버전 1703부터 UWP 앱은 VobSub 형식의 외부 이미지 기반 자막을 지원할 수 있습니다. 이 기능을 사용 하려면 먼저 이미지 자막이 표시 될 미디어 콘텐츠에 대 한 [**MediaSource**](https://docs.microsoft.com/uwp/api/windows.media.core.mediasource) 개체를 만듭니다. 다음으로 [**Createfromuriwithindex**](https://docs.microsoft.com/uwp/api/windows.media.core.timedtextsource.CreateFromUriWithIndex) 또는 [**createfromuriwithindex**](https://docs.microsoft.com/uwp/api/windows.media.core.timedtextsource.CreateFromStreamWithIndex)를 호출 하 여, 자막 이미지 데이터를 포함 하는 하위 파일의 Uri를 전달 하 고, 자막에 대 한 타이밍 정보를 포함 하는. idx 파일을 전달 하 여 [**timedtextsource**](https://docs.microsoft.com/uwp/api/windows.media.core.timedtextsource) 개체를 만듭니다. **MediaSource** 에 **Timedtextsource** 를 추가 하 여 원본 [**externaltimedtextsources**](https://docs.microsoft.com/uwp/api/windows.media.core.mediasource.ExternalTimedTextSources) 컬렉션에 추가 합니다. **MediaSource**에서 [**Mediaplaybackitem**](https://docs.microsoft.com/uwp/api/windows.media.playback.mediaplaybackitem) 을 만듭니다.

[!code-cs[ImageSubtitleLoadContent](./code/MediaSource_RS1/cs/MainPage_Cues.xaml.cs#SnippetImageSubtitleLoadContent)]

이전 단계에서 만든 **Mediaplaybackitem** 개체를 사용 하 여 이미지 부제목 메타 데이터 이벤트를 등록 합니다. 이 예제에서는 도우미 메서드인 **RegisterMetadataHandlerForImageSubtitles**를 사용 하 여 이벤트에 등록 합니다. 람다 식은 [**TimedMetadataTracksChanged**](https://docs.microsoft.com/uwp/api/windows.media.playback.mediaplaybackitem.TimedMetadataTracksChanged) 이벤트에 대 한 처리기를 구현 하는 데 사용 됩니다 .이 이벤트는 시스템에서 **Mediaplaybackitem**과 연결 된 메타 데이터 트랙의 변경을 감지할 때 발생 합니다. 일부 경우에는 재생 항목이 처음에 해결 될 때 메타 데이터 트랙을 사용할 수 있습니다. 따라서 **TimedMetadataTracksChanged** 처리기 외부에서 사용 가능한 메타 데이터 트랙을 반복 하 고 **RegisterMetadataHandlerForImageSubtitles**를 호출 합니다.

[!code-cs[ImageSubtitleTracksChanged](./code/MediaSource_RS1/cs/MainPage_Cues.xaml.cs#SnippetImageSubtitleTracksChanged)]

이미지 부제목 메타 데이터 이벤트를 등록 한 후에는 **미디어 항목** 을 [**MediaPlayerElement**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.mediaplayerelement)내에서 재생 하기 위해 [**MediaPlayer**](https://docs.microsoft.com/uwp/api/windows.media.playback.mediaplayer) 에 할당 합니다.

[!code-cs[ImageSubtitlePlay](./code/MediaSource_RS1/cs/MainPage_Cues.xaml.cs#SnippetImageSubtitlePlay)]

**RegisterMetadataHandlerForImageSubtitles** Helper 메서드에서 **Mediaplaybackitem**의 **TimedMetadataTracks** 컬렉션으로 인덱싱하여 [**TimedMetadataTrack**](https://docs.microsoft.com/uwp/api/windows.media.core.timedmetadatatrack) 클래스의 인스턴스를 가져옵니다. [**CueEntered**](https://docs.microsoft.com/uwp/api/windows.media.core.timedmetadatatrack.CueEntered) 이벤트 및 [**CueExited**](https://docs.microsoft.com/uwp/api/windows.media.core.timedmetadatatrack.CueExited) 이벤트에 등록 합니다. 그런 다음 재생 항목의 **TimedMetadataTracks** 컬렉션에서 [**SetPresentationMode**](https://docs.microsoft.com/uwp/api/windows.media.playback.mediaplaybacktimedmetadatatracklist.SetPresentationMode) 를 호출 하 여 앱이이 재생 항목에 대 한 메타 데이터 큐 이벤트를 수신 하려고 함을 시스템에 지시 해야 합니다.

[!code-cs[RegisterMetadataHandlerForImageSubtitles](./code/MediaSource_RS1/cs/MainPage_Cues.xaml.cs#SnippetRegisterMetadataHandlerForImageSubtitles)]

**CueEntered** 이벤트에 대 한 처리기에서 처리기에 전달 된 [**TimedMetadataTrack**](https://docs.microsoft.com/uwp/api/windows.media.core.timedmetadatatrack) 개체의 [**TimedMetadataKind**](https://docs.microsoft.com/uwp/api/windows.media.core.timedmetadatatrack.TimedMetadataKind) 속성이를 확인 하 여 메타 데이터가 이미지 자막 용 인지 확인할 수 있습니다. 이는 여러 유형의 메타 데이터에 대해 동일한 데이터 큐 이벤트 처리기를 사용 하는 경우에 필요 합니다. 연결 된 메타 데이터 트랙이 ImageSubtitle 유형인 경우 [**Mediacueeventargs**](https://docs.microsoft.com/uwp/api/windows.media.core.mediacueeventargs) 의 **cue** 속성에 포함 된 데이터 큐를 [**Imagecue**](https://docs.microsoft.com/uwp/api/windows.media.core.imagecue)로 캐스팅 **TimedMetadataKind**. **Imagecue** 의 [**\bitmap**](https://docs.microsoft.com/uwp/api/windows.media.core.imagecue.SoftwareBitmap) 속성에는 부제목 이미지의 [**\bitmap**](https://docs.microsoft.com/uwp/api/windows.graphics.imaging.softwarebitmap) 표현이 포함 되어 있습니다. [**SoftwareBitmapSource**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.imaging.softwarebitmapsource) 을 만들고 [**SetBitmapAsync**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.imaging.softwarebitmapsource.SetBitmapAsync) 를 호출 하 여 이미지를 XAML [**이미지**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.image) 컨트롤에 할당 합니다. **Imagecue** 의 [**범위**](https://docs.microsoft.com/uwp/api/windows.media.core.imagecue.Extent) 및 [**위치**](https://docs.microsoft.com/uwp/api/windows.media.core.imagecue.Position) 속성은 부제목 이미지의 크기 및 위치에 대 한 정보를 제공 합니다.

[!code-cs[ImageSubtitleCueEntered](./code/MediaSource_RS1/cs/MainPage_Cues.xaml.cs#SnippetImageSubtitleCueEntered)]

## <a name="speech-cues"></a>음성 신호
Windows 10, 버전 1703부터 UWP 앱은 재생 된 미디어의 단어 경계, 문장 경계 및 SSML (음성 합성 마크업 언어) 책갈피에 대 한 응답으로 이벤트를 받도록 등록할 수 있습니다. 이렇게 하면 [**SpeechSynthesizer**](https://docs.microsoft.com/uwp/api/Windows.Media.SpeechSynthesis.SpeechSynthesizer) 클래스를 사용 하 여 생성 된 오디오 스트림을 재생 하 고 현재 진행 중인 단어나 문장의 텍스트를 표시 하는 등 이러한 이벤트에 따라 UI를 업데이트할 수 있습니다.

이 섹션에 표시 된 예제에서는 클래스 멤버 변수를 사용 하 여 합성 및 재생 될 텍스트 문자열을 저장 합니다.

[!code-cs[SpeechInputText](./code/MediaSource_RS1/cs/MainPage_Cues.xaml.cs#SnippetSpeechInputText)]

**SpeechSynthesizer** 클래스의 새 인스턴스를 만듭니다. 신시사이저의 [**IncludeWordBoundaryMetadata**](https://docs.microsoft.com/uwp/api/windows.media.speechsynthesis.speechsynthesizeroptions.IncludeWordBoundaryMetadata) 및 [**IncludeSentenceBoundaryMetadata**](https://docs.microsoft.com/uwp/api/windows.media.speechsynthesis.speechsynthesizeroptions.IncludeSentenceBoundaryMetadata) 옵션을 **true** 로 설정 하 여 메타 데이터를 생성 된 미디어 스트림에 포함 하도록 지정 합니다. [**SynthesizeTextToStreamAsync**](https://docs.microsoft.com/uwp/api/Windows.Media.SpeechSynthesis.SpeechSynthesizer.SynthesizeTextToStreamAsync) 를 호출 하 여 합성 된 음성 및 해당 메타 데이터를 포함 하는 스트림을 생성 합니다. 합성 된 스트림에서 [**MediaSource**](https://docs.microsoft.com/uwp/api/windows.media.core.mediasource) 및 [**Mediaplaybackitem**](https://docs.microsoft.com/uwp/api/windows.media.playback.mediaplaybackitem) 을 만듭니다.

[!code-cs[SynthesizeSpeech](./code/MediaSource_RS1/cs/MainPage_Cues.xaml.cs#SnippetSynthesizeSpeech)]

**Mediaplaybackitem** 개체를 사용 하 여 음성 메타 데이터 이벤트를 등록 합니다. 이 예제에서는 도우미 메서드인 **Registermetadatahandlerforspeech**를 사용 하 여 이벤트를 등록 합니다. 람다 식은 [**TimedMetadataTracksChanged**](https://docs.microsoft.com/uwp/api/windows.media.playback.mediaplaybackitem.TimedMetadataTracksChanged) 이벤트에 대 한 처리기를 구현 하는 데 사용 됩니다 .이 이벤트는 시스템에서 **Mediaplaybackitem**과 연결 된 메타 데이터 트랙의 변경을 감지할 때 발생 합니다.  일부 경우에는 재생 항목이 처음에 해결 될 때 메타 데이터 트랙을 사용할 수 있습니다. 따라서 **TimedMetadataTracksChanged** 처리기 외부에서 사용 가능한 메타 데이터 추적을 반복 하 여 **Registermetadatahandlerforspeech**를 호출 합니다.

[!code-cs[SpeechTracksChanged](./code/MediaSource_RS1/cs/MainPage_Cues.xaml.cs#SnippetSpeechTracksChanged)]

Speech metadata 이벤트를 등록 한 후에는 **Mediaitem** 이 [**MediaPlayerElement**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.mediaplayerelement)내에서 재생을 위해 [**MediaPlayer**](https://docs.microsoft.com/uwp/api/windows.media.playback.mediaplayer) 에 할당 됩니다.

[!code-cs[SpeechPlay](./code/MediaSource_RS1/cs/MainPage_Cues.xaml.cs#SnippetSpeechPlay)]

**Registermetadatahandlerforspeech** 도우미 메서드에서 **Mediaplaybackitem**의 **TimedMetadataTracks** 컬렉션으로 인덱싱하여 [**TimedMetadataTrack**](https://docs.microsoft.com/uwp/api/windows.media.core.timedmetadatatrack) 클래스의 인스턴스를 가져옵니다. [**CueEntered**](https://docs.microsoft.com/uwp/api/windows.media.core.timedmetadatatrack.CueEntered) 이벤트 및 [**CueExited**](https://docs.microsoft.com/uwp/api/windows.media.core.timedmetadatatrack.CueExited) 이벤트에 등록 합니다. 그런 다음 재생 항목의 **TimedMetadataTracks** 컬렉션에서 [**SetPresentationMode**](https://docs.microsoft.com/uwp/api/windows.media.playback.mediaplaybacktimedmetadatatracklist.SetPresentationMode) 를 호출 하 여 앱이이 재생 항목에 대 한 메타 데이터 큐 이벤트를 수신 하려고 함을 시스템에 지시 해야 합니다.

[!code-cs[RegisterMetadataHandlerForWords](./code/MediaSource_RS1/cs/MainPage_Cues.xaml.cs#SnippetRegisterMetadataHandlerForWords)]

**CueEntered** 이벤트에 대 한 처리기에서 처리기에 전달 된 [**TimedMetadataTrack**](https://docs.microsoft.com/uwp/api/windows.media.core.timedmetadatatrack) 개체의 [**TimedMetadataKind**](https://docs.microsoft.com/uwp/api/windows.media.core.timedmetadatatrack.TimedMetadataKind) 속성이를 확인 하 여 메타 데이터가 음성 인지 확인할 수 있습니다. 이는 여러 유형의 메타 데이터에 대해 동일한 데이터 큐 이벤트 처리기를 사용 하는 경우에 필요 합니다. 연결 된 메타 데이터 트랙이 **TimedMetadataKind**형식이 면 [**Mediacueeventargs**](https://docs.microsoft.com/uwp/api/windows.media.core.mediacueeventargs) 의 **cue** 속성에 포함 된 데이터 큐를 [**SpeechCue**](https://docs.microsoft.com/uwp/api/windows.media.core.speechcue)로 캐스팅 합니다. 음성 큐의 경우 메타 데이터 트랙에 포함 된 음성 신호 유형은 **레이블** 속성을 선택 하 여 결정 됩니다. 이 속성의 값은 단어 경계의 경우 "SpeechWord"이 고, 문장의 경우에는 "SpeechSentence"이 고, SSML 책갈피의 경우 "SpeechBookmark"입니다. 이 예에서는 "SpeechWord" 값을 확인 하 고,이 값이 있으면 **SpeechCue** 의 [**startpositionininput**](https://docs.microsoft.com/uwp/api/windows.media.core.speechcue.StartPositionInInput) 및 [**endpositionininput**](https://docs.microsoft.com/uwp/api/windows.media.core.speechcue.EndPositionInInput) 속성이 현재 재생 중인 단어의 입력 텍스트 내에서 위치를 확인 하는 데 사용 됩니다. 이 예제에서는 단순히 각 단어를 디버그 출력으로 출력 합니다.

[!code-cs[SpeechWordCueEntered](./code/MediaSource_RS1/cs/MainPage_Cues.xaml.cs#SnippetSpeechWordCueEntered)]

## <a name="chapter-cues"></a>장 큐
Windows 10, 버전 1703부터 UWP 앱은 미디어 항목 내의 챕터에 해당 하는 큐를 등록할 수 있습니다. 이 기능을 사용 하려면 미디어 콘텐츠에 대 한 [**MediaSource**](https://docs.microsoft.com/uwp/api/windows.media.core.mediasource) 개체를 만든 다음 **MediaSource**에서 [**mediaplaybackitem**](https://docs.microsoft.com/uwp/api/windows.media.playback.mediaplaybackitem) 을 만듭니다.

[!code-cs[ChapterCueLoadContent](./code/MediaSource_RS1/cs/MainPage_Cues.xaml.cs#SnippetChapterCueLoadContent)]

이전 단계에서 만든 **Mediaplaybackitem** 개체를 사용 하 여 장 메타 데이터 이벤트를 등록 합니다. 이 예제에서는 도우미 메서드인 **RegisterMetadataHandlerForChapterCues**를 사용 하 여 이벤트에 등록 합니다. 람다 식은 [**TimedMetadataTracksChanged**](https://docs.microsoft.com/uwp/api/windows.media.playback.mediaplaybackitem.TimedMetadataTracksChanged) 이벤트에 대 한 처리기를 구현 하는 데 사용 됩니다 .이 이벤트는 시스템에서 **Mediaplaybackitem**과 연결 된 메타 데이터 트랙의 변경을 감지할 때 발생 합니다. 일부 경우에는 재생 항목이 처음에 해결 될 때 메타 데이터 트랙을 사용할 수 있습니다. 따라서 **TimedMetadataTracksChanged** 처리기 외부에서 사용 가능한 메타 데이터 트랙을 반복 하 고 **RegisterMetadataHandlerForChapterCues**를 호출 합니다.

[!code-cs[ChapterCueTracksChanged](./code/MediaSource_RS1/cs/MainPage_Cues.xaml.cs#SnippetChapterCueTracksChanged)]

장 메타 데이터 이벤트를 등록 한 후에는 **미디어 항목** 을 [**MediaPlayerElement**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.mediaplayerelement)내에서 재생 하기 위해 [**MediaPlayer**](https://docs.microsoft.com/uwp/api/windows.media.playback.mediaplayer) 에 할당 합니다.

[!code-cs[ChapterCuePlay](./code/MediaSource_RS1/cs/MainPage_Cues.xaml.cs#SnippetChapterCuePlay)]

**RegisterMetadataHandlerForChapterCues** Helper 메서드에서 **Mediaplaybackitem**의 **TimedMetadataTracks** 컬렉션으로 인덱싱하여 [**TimedMetadataTrack**](https://docs.microsoft.com/uwp/api/windows.media.core.timedmetadatatrack) 클래스의 인스턴스를 가져옵니다. [**CueEntered**](https://docs.microsoft.com/uwp/api/windows.media.core.timedmetadatatrack.CueEntered) 이벤트 및 [**CueExited**](https://docs.microsoft.com/uwp/api/windows.media.core.timedmetadatatrack.CueExited) 이벤트에 등록 합니다. 그런 다음 재생 항목의 **TimedMetadataTracks** 컬렉션에서 [**SetPresentationMode**](https://docs.microsoft.com/uwp/api/windows.media.playback.mediaplaybacktimedmetadatatracklist.SetPresentationMode) 를 호출 하 여 앱이이 재생 항목에 대 한 메타 데이터 큐 이벤트를 수신 하려고 함을 시스템에 지시 해야 합니다.

[!code-cs[RegisterMetadataHandlerForChapterCues](./code/MediaSource_RS1/cs/MainPage_Cues.xaml.cs#SnippetRegisterMetadataHandlerForChapterCues)]

**CueEntered** 이벤트에 대 한 처리기에서 처리기에 전달 된 [**TimedMetadataTrack**](https://docs.microsoft.com/uwp/api/windows.media.core.timedmetadatatrack) 개체의 [**TimedMetadataKind**](https://docs.microsoft.com/uwp/api/windows.media.core.timedmetadatatrack.TimedMetadataKind) 속성이를 확인 하 여 해당 메타 데이터가 챕터 큐에 있는지 확인할 수 있습니다. 이는 여러 유형의 메타 데이터에 대해 동일한 데이터 큐 이벤트 처리기를 사용 하는 경우에 필요 합니다. 연결 된 메타 데이터 트랙이 **TimedMetadataKind**유형인 경우 [**Mediacueeventargs**](https://docs.microsoft.com/uwp/api/windows.media.core.mediacueeventargs) 의 **cue** 속성에 포함 된 데이터 큐를 [**ChapterCue**](https://docs.microsoft.com/uwp/api/windows.media.core.chaptercue)로 캐스팅 합니다. **ChapterCue** 의 [**title**](https://docs.microsoft.com/uwp/api/windows.media.core.chaptercue.Title) 속성에는 재생 중에 방금 도달한 챕터의 제목이 포함 됩니다.

[!code-cs[ChapterCueEntered](./code/MediaSource_RS1/cs/MainPage_Cues.xaml.cs#SnippetChapterCueEntered)]

### <a name="seek-to-the-next-chapter-using-chapter-cues"></a>챕터 신호를 사용 하 여 다음 챕터로 이동
현재 챕터가 재생 항목에서 변경 될 때 알림을 수신 하는 것 외에도 장 큐를 사용 하 여 게임 항목 내의 다음 챕터를 검색할 수 있습니다. 아래에 표시 된 예제 메서드는 현재 재생 중인 미디어 항목을 나타내는 [**MediaPlayer**](https://docs.microsoft.com/uwp/api/windows.media.playback.mediaplayer) 및 [**mediaplaybackitem**](https://docs.microsoft.com/uwp/api/windows.media.playback.mediaplaybackitem) 을 인수로 사용 합니다. [**TimedMetadataTracks**](https://docs.microsoft.com/uwp/api/windows.media.playback.mediaplaybackitem.TimedMetadataTracks) 컬렉션을 검색 하 여 [**TimedMetadataKind**](https://docs.microsoft.com/uwp/api/windows.media.core.timedmetadatatrack.TimedMetadataKind) 속성이 TimedMetadataTrack 값의 [**TimedMetadataTrack**](https://docs.microsoft.com/uwp/api/windows.media.core.timedmetadatatrack) 값이 있는지 확인 합니다 **.**  챕터 트랙이 발견 되 면 메서드는 트랙의 [**큐 컬렉션에**](https://docs.microsoft.com/uwp/api/windows.media.core.timedmetadatatrack.Cues) 있는 각 큐를 반복 하 여 [**StartTime**](https://docs.microsoft.com/uwp/api/windows.media.core.chaptercue.StartTime) 이 미디어 플레이어의 재생 세션의 현재 [**위치**](https://docs.microsoft.com/uwp/api/windows.media.playback.mediaplaybacksession.Position) 보다 큰 첫 번째 큐를 찾습니다. 올바른 큐를 찾으면 재생 세션의 위치가 업데이트 되 고 챕터 제목이 UI에서 업데이트 됩니다.

[!code-cs[GoToNextChapter](./code/MediaSource_RS1/cs/MainPage_Cues.xaml.cs#SnippetGoToNextChapter)]

## <a name="extended-m3u-comments"></a>확장 된 M3U 주석
Windows 10 버전 1703부터 UWP 앱은 확장 된 M3U 매니페스트 파일 내의 주석에 해당 하는 신호를 등록할 수 있습니다. 이 예제에서는 [**AdaptiveMediaSource**](https://docs.microsoft.com/uwp/api/windows.media.streaming.adaptive.adaptivemediasource) 를 사용 하 여 미디어 콘텐츠를 재생 합니다. 자세한 내용은 [적응 스트리밍](adaptive-streaming.md)을 참조 하세요. [**CreateFromUriAsync**](https://docs.microsoft.com/uwp/api/windows.media.streaming.adaptive.adaptivemediasource.CreateFromUriAsync) 또는 [**CreateFromStreamAsync**](https://docs.microsoft.com/uwp/api/windows.media.streaming.adaptive.adaptivemediasource.CreateFromStreamAsync)를 호출 하 여 콘텐츠에 대 한 **AdaptiveMediaSource** 를 만듭니다. [**CreateFromAdaptiveMediaSource**](https://docs.microsoft.com/uwp/api/windows.media.core.mediasource.CreateFromAdaptiveMediaSource) 를 호출 하 여 [**MediaSource**](https://docs.microsoft.com/uwp/api/windows.media.core.mediasource) 개체를 만든 다음 **MediaSource**에서 [**mediaplaybackitem**](https://docs.microsoft.com/uwp/api/windows.media.playback.mediaplaybackitem) 을 만듭니다.

[!code-cs[EXTM3ULoadContent](./code/MediaSource_RS1/cs/MainPage_Cues.xaml.cs#SnippetEXTM3ULoadContent)]

이전 단계에서 만든 **Mediaplaybackitem** 개체를 사용 하 여 M3U 메타 데이터 이벤트를 등록 합니다. 이 예제에서는 도우미 메서드인 **RegisterMetadataHandlerForEXTM3UCues**를 사용 하 여 이벤트에 등록 합니다. 람다 식은 [**TimedMetadataTracksChanged**](https://docs.microsoft.com/uwp/api/windows.media.playback.mediaplaybackitem.TimedMetadataTracksChanged) 이벤트에 대 한 처리기를 구현 하는 데 사용 됩니다 .이 이벤트는 시스템에서 **Mediaplaybackitem**과 연결 된 메타 데이터 트랙의 변경을 감지할 때 발생 합니다. 일부 경우에는 재생 항목이 처음에 해결 될 때 메타 데이터 트랙을 사용할 수 있습니다. 따라서 **TimedMetadataTracksChanged** 처리기 외부에서 사용 가능한 메타 데이터 트랙을 반복 하 고 **RegisterMetadataHandlerForEXTM3UCues**를 호출 합니다.

[!code-cs[EXTM3UCueTracksChanged](./code/MediaSource_RS1/cs/MainPage_Cues.xaml.cs#SnippetEXTM3UCueTracksChanged)]

M3U 메타 데이터 이벤트를 등록 한 후에는 **미디어 항목** 을 [**MediaPlayerElement**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.mediaplayerelement)내에서 재생 하기 위해 [**MediaPlayer**](https://docs.microsoft.com/uwp/api/windows.media.playback.mediaplayer) 에 할당 합니다.

[!code-cs[EXTM3UCuePlay](./code/MediaSource_RS1/cs/MainPage_Cues.xaml.cs#SnippetEXTM3UCuePlay)]

**RegisterMetadataHandlerForEXTM3UCues** Helper 메서드에서 **Mediaplaybackitem**의 **TimedMetadataTracks** 컬렉션으로 인덱싱하여 [**TimedMetadataTrack**](https://docs.microsoft.com/uwp/api/windows.media.core.timedmetadatatrack) 클래스의 인스턴스를 가져옵니다. 트랙이 M3U 주석을 나타내는 경우 "EXTM3U" 값을 포함 하는 메타 데이터 트랙의 DispatchType 속성을 선택 합니다. [**CueEntered**](https://docs.microsoft.com/uwp/api/windows.media.core.timedmetadatatrack.CueEntered) 이벤트 및 [**CueExited**](https://docs.microsoft.com/uwp/api/windows.media.core.timedmetadatatrack.CueExited) 이벤트에 등록 합니다. 그런 다음 재생 항목의 **TimedMetadataTracks** 컬렉션에서 [**SetPresentationMode**](https://docs.microsoft.com/uwp/api/windows.media.playback.mediaplaybacktimedmetadatatracklist.SetPresentationMode) 를 호출 하 여 앱이이 재생 항목에 대 한 메타 데이터 큐 이벤트를 수신 하려고 함을 시스템에 지시 해야 합니다.

[!code-cs[RegisterMetadataHandlerForEXTM3UCues](./code/MediaSource_RS1/cs/MainPage_Cues.xaml.cs#SnippetRegisterMetadataHandlerForEXTM3UCues)]

**CueEntered** 이벤트에 대 한 처리기에서 [**Mediacueeventargs**](https://docs.microsoft.com/uwp/api/windows.media.core.mediacueeventargs) 의 **cue** 속성에 포함 된 데이터 큐를 [**datacue**](https://docs.microsoft.com/uwp/api/windows.media.core.datacue)로 캐스팅 합니다.  큐의 **Datacue** 및 [**Data**](https://docs.microsoft.com/uwp/api/windows.media.core.datacue.Data) 속성이 null이 아닌지 확인 합니다. 확장 된 EMU 주석은 u t f-16, little endian, null 종료 문자열 형식으로 제공 됩니다. [**Datareader. FromBuffer**](https://docs.microsoft.com/uwp/api/windows.storage.streams.datareader.FromBuffer)를 호출 하 여 큐 데이터를 읽을 새 **DataReader** 를 만듭니다. 판독기의 [**UnicodeEncoding**](https://docs.microsoft.com/uwp/api/windows.storage.streams.datareader.UnicodeEncoding) 속성을 [**Utf16LE**](https://docs.microsoft.com/uwp/api/windows.storage.streams.unicodeencoding) 로 설정 하 여 데이터를 올바른 형식으로 읽습니다. [**ReadString**](https://docs.microsoft.com/uwp/api/windows.storage.streams.datareader.ReadString) 를 호출 하 여 데이터 필드 길이의 절반을 지정 하 고, 각 문자의 크기가 2 바이트 이기 때문 **에 데이터를** 읽고, 후행 null 문자를 제거 하려면 1을 뺍니다. 이 예제에서 M3U 주석은 단순히 디버그 출력에 기록 됩니다.

[!code-cs[EXTM3UCueEntered](./code/MediaSource_RS1/cs/MainPage_Cues.xaml.cs#SnippetEXTM3UCueEntered)]

## <a name="id3-tags"></a>ID3 태그
Windows 10, 버전 1703부터 UWP 앱은 HLS (Http 라이브 스트리밍) 콘텐츠 내의 ID3 태그에 해당 하는 큐를 등록할 수 있습니다. 이 예제에서는 [**AdaptiveMediaSource**](https://docs.microsoft.com/uwp/api/windows.media.streaming.adaptive.adaptivemediasource) 를 사용 하 여 미디어 콘텐츠를 재생 합니다. 자세한 내용은 [적응 스트리밍](adaptive-streaming.md)을 참조 하세요. [**CreateFromUriAsync**](https://docs.microsoft.com/uwp/api/windows.media.streaming.adaptive.adaptivemediasource.CreateFromUriAsync) 또는 [**CreateFromStreamAsync**](https://docs.microsoft.com/uwp/api/windows.media.streaming.adaptive.adaptivemediasource.CreateFromStreamAsync)를 호출 하 여 콘텐츠에 대 한 **AdaptiveMediaSource** 를 만듭니다. [**CreateFromAdaptiveMediaSource**](https://docs.microsoft.com/uwp/api/windows.media.core.mediasource.CreateFromAdaptiveMediaSource) 를 호출 하 여 [**MediaSource**](https://docs.microsoft.com/uwp/api/windows.media.core.mediasource) 개체를 만든 다음 **MediaSource**에서 [**mediaplaybackitem**](https://docs.microsoft.com/uwp/api/windows.media.playback.mediaplaybackitem) 을 만듭니다.

[!code-cs[EXTM3ULoadContent](./code/MediaSource_RS1/cs/MainPage_Cues.xaml.cs#SnippetEXTM3ULoadContent)]

이전 단계에서 만든 **Mediaplaybackitem** 개체를 사용 하 여 ID3 태그 이벤트를 등록 합니다. 이 예제에서는 도우미 메서드인 **RegisterMetadataHandlerForID3Cues**를 사용 하 여 이벤트에 등록 합니다. 람다 식은 [**TimedMetadataTracksChanged**](https://docs.microsoft.com/uwp/api/windows.media.playback.mediaplaybackitem.TimedMetadataTracksChanged) 이벤트에 대 한 처리기를 구현 하는 데 사용 됩니다 .이 이벤트는 시스템에서 **Mediaplaybackitem**과 연결 된 메타 데이터 트랙의 변경을 감지할 때 발생 합니다. 일부 경우에는 재생 항목이 처음에 해결 될 때 메타 데이터 트랙을 사용할 수 있습니다. 따라서 **TimedMetadataTracksChanged** 처리기 외부에서 사용 가능한 메타 데이터 트랙을 반복 하 고 **RegisterMetadataHandlerForID3Cues**를 호출 합니다.

[!code-cs[ID3LoadContent](./code/MediaSource_RS1/cs/MainPage_Cues.xaml.cs#SnippetID3LoadContent)]

ID3 메타 데이터 이벤트를 등록 한 후에는 **미디어 항목** 을 [**MediaPlayerElement**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.mediaplayerelement)내에서 재생 하기 위해 [**MediaPlayer**](https://docs.microsoft.com/uwp/api/windows.media.playback.mediaplayer) 에 할당 합니다.

[!code-cs[ID3CuePlay](./code/MediaSource_RS1/cs/MainPage_Cues.xaml.cs#SnippetID3CuePlay)]


**RegisterMetadataHandlerForID3Cues** Helper 메서드에서 **Mediaplaybackitem**의 **TimedMetadataTracks** 컬렉션으로 인덱싱하여 [**TimedMetadataTrack**](https://docs.microsoft.com/uwp/api/windows.media.core.timedmetadatatrack) 클래스의 인스턴스를 가져옵니다. 트랙이 ID3 태그를 나타내는 경우 "15260DFFFF49443320FF49443320000F" 라는 GUID 문자열을 포함 하는 값을 포함 하는 메타 데이터 트랙의 DispatchType 속성을 선택 합니다. [**CueEntered**](https://docs.microsoft.com/uwp/api/windows.media.core.timedmetadatatrack.CueEntered) 이벤트 및 [**CueExited**](https://docs.microsoft.com/uwp/api/windows.media.core.timedmetadatatrack.CueExited) 이벤트에 등록 합니다. 그런 다음 재생 항목의 **TimedMetadataTracks** 컬렉션에서 [**SetPresentationMode**](https://docs.microsoft.com/uwp/api/windows.media.playback.mediaplaybacktimedmetadatatracklist.SetPresentationMode) 를 호출 하 여 앱이이 재생 항목에 대 한 메타 데이터 큐 이벤트를 수신 하려고 함을 시스템에 지시 해야 합니다.

[!code-cs[RegisterMetadataHandlerForID3Cues](./code/MediaSource_RS1/cs/MainPage_Cues.xaml.cs#SnippetRegisterMetadataHandlerForID3Cues)]

**CueEntered** 이벤트에 대 한 처리기에서 [**Mediacueeventargs**](https://docs.microsoft.com/uwp/api/windows.media.core.mediacueeventargs) 의 **cue** 속성에 포함 된 데이터 큐를 [**datacue**](https://docs.microsoft.com/uwp/api/windows.media.core.datacue)로 캐스팅 합니다.  큐의 **Datacue** 및 [**Data**](https://docs.microsoft.com/uwp/api/windows.media.core.datacue.Data) 속성이 null이 아닌지 확인 합니다. 확장 된 EMU 주석은 전송 스트림의 원시 바이트 형식으로 제공 됩니다 (참조 [http://id3.org/id3v2.4.0-structure](https://id3.org/id3v2.4.0-structure) ). [**Datareader. FromBuffer**](https://docs.microsoft.com/uwp/api/windows.storage.streams.datareader.FromBuffer)를 호출 하 여 큐 데이터를 읽을 새 **DataReader** 를 만듭니다.  이 예제에서는 ID3 태그의 헤더 값을 큐 데이터에서 읽고 디버그 출력에 기록 합니다.

[!code-cs[ID3CueEntered](./code/MediaSource_RS1/cs/MainPage_Cues.xaml.cs#SnippetID3CueEntered)]

## <a name="fragmented-mp4-emsg-boxes"></a>조각화 된 mp4 emsg 상자
Windows 10, 버전 1703부터 UWP 앱은 조각화 된 mp4 스트림 내의 emsg 상자에 해당 하는 신호를 등록할 수 있습니다. 이러한 형식의 메타 데이터를 사용 하는 예제는 콘텐츠 공급자가 라이브 스트리밍 콘텐츠 중에 광고를 재생 하도록 클라이언트 응용 프로그램에 신호를 보내는 데 사용 됩니다. 이 예제에서는 [**AdaptiveMediaSource**](https://docs.microsoft.com/uwp/api/windows.media.streaming.adaptive.adaptivemediasource) 를 사용 하 여 미디어 콘텐츠를 재생 합니다. 자세한 내용은 [적응 스트리밍](adaptive-streaming.md)을 참조 하세요. [**CreateFromUriAsync**](https://docs.microsoft.com/uwp/api/windows.media.streaming.adaptive.adaptivemediasource.CreateFromUriAsync) 또는 [**CreateFromStreamAsync**](https://docs.microsoft.com/uwp/api/windows.media.streaming.adaptive.adaptivemediasource.CreateFromStreamAsync)를 호출 하 여 콘텐츠에 대 한 **AdaptiveMediaSource** 를 만듭니다. [**CreateFromAdaptiveMediaSource**](https://docs.microsoft.com/uwp/api/windows.media.core.mediasource.CreateFromAdaptiveMediaSource) 를 호출 하 여 [**MediaSource**](https://docs.microsoft.com/uwp/api/windows.media.core.mediasource) 개체를 만든 다음 **MediaSource**에서 [**mediaplaybackitem**](https://docs.microsoft.com/uwp/api/windows.media.playback.mediaplaybackitem) 을 만듭니다.

[!code-cs[EmsgLoadContent](./code/MediaSource_RS1/cs/MainPage_Cues.xaml.cs#SnippetEmsgLoadContent)]

이전 단계에서 만든 **Mediaplaybackitem** 개체를 사용 하 여 emsg box 이벤트를 등록 합니다. 이 예제에서는 도우미 메서드인 **RegisterMetadataHandlerForEmsgCues**를 사용 하 여 이벤트에 등록 합니다. 람다 식은 [**TimedMetadataTracksChanged**](https://docs.microsoft.com/uwp/api/windows.media.playback.mediaplaybackitem.TimedMetadataTracksChanged) 이벤트에 대 한 처리기를 구현 하는 데 사용 됩니다 .이 이벤트는 시스템에서 **Mediaplaybackitem**과 연결 된 메타 데이터 트랙의 변경을 감지할 때 발생 합니다. 일부 경우에는 재생 항목이 처음에 해결 될 때 메타 데이터 트랙을 사용할 수 있습니다. 따라서 **TimedMetadataTracksChanged** 처리기 외부에서 사용 가능한 메타 데이터 트랙을 반복 하 고 **RegisterMetadataHandlerForEmsgCues**를 호출 합니다.

[!code-cs[ID3LoadContent](./code/MediaSource_RS1/cs/MainPage_Cues.xaml.cs#SnippetID3LoadContent)]

Emsg box 메타 데이터 이벤트를 등록 한 후에는 **Mediaitem** 이 [**MediaPlayerElement**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.mediaplayerelement)내에서 재생을 위해 [**MediaPlayer**](https://docs.microsoft.com/uwp/api/windows.media.playback.mediaplayer) 에 할당 됩니다.

[!code-cs[EmsgCuePlay](./code/MediaSource_RS1/cs/MainPage_Cues.xaml.cs#SnippetEmsgCuePlay)]


**RegisterMetadataHandlerForEmsgCues** Helper 메서드에서 **Mediaplaybackitem**의 **TimedMetadataTracks** 컬렉션으로 인덱싱하여 [**TimedMetadataTrack**](https://docs.microsoft.com/uwp/api/windows.media.core.timedmetadatatrack) 클래스의 인스턴스를 가져옵니다. 트랙이 emsg 상자를 나타내는 경우 값이 "emsg: mp4" 인 메타 데이터 트랙의 DispatchType 속성을 선택 합니다. [**CueEntered**](https://docs.microsoft.com/uwp/api/windows.media.core.timedmetadatatrack.CueEntered) 이벤트 및 [**CueExited**](https://docs.microsoft.com/uwp/api/windows.media.core.timedmetadatatrack.CueExited) 이벤트에 등록 합니다. 그런 다음 재생 항목의 **TimedMetadataTracks** 컬렉션에서 [**SetPresentationMode**](https://docs.microsoft.com/uwp/api/windows.media.playback.mediaplaybacktimedmetadatatracklist.SetPresentationMode) 를 호출 하 여 앱이이 재생 항목에 대 한 메타 데이터 큐 이벤트를 수신 하려고 함을 시스템에 지시 해야 합니다.


[!code-cs[RegisterMetadataHandlerForEmsgCues](./code/MediaSource_RS1/cs/MainPage_Cues.xaml.cs#SnippetRegisterMetadataHandlerForEmsgCues)]


**CueEntered** 이벤트에 대 한 처리기에서 [**Mediacueeventargs**](https://docs.microsoft.com/uwp/api/windows.media.core.mediacueeventargs) 의 **cue** 속성에 포함 된 데이터 큐를 [**datacue**](https://docs.microsoft.com/uwp/api/windows.media.core.datacue)로 캐스팅 합니다.  **Datacue** 개체가 null이 아닌지 확인 합니다. Emsg 상자의 속성는 미디어 파이프라인에서 DataCue 개체의 [**properties**](https://docs.microsoft.com/uwp/api/windows.media.core.datacue.Properties) 컬렉션에서 사용자 지정 속성으로 제공 됩니다. 이 예제에서는 **[TryGetValue](https://docs.microsoft.com/uwp/api/windows.foundation.collections.propertyset.trygetvalue)** 메서드를 사용 하 여 여러 가지 속성 값을 추출 하려고 시도 합니다. 이 메서드가 null을 반환 하는 경우 요청 된 속성이가 emsg 상자에 없음을 의미 하므로 기본값이 설정 됩니다.

이 예제의 다음 부분에서는 ad 재생이 트리거되는 시나리오를 보여 줍니다 .이 경우 이전 단계에서 얻은 *scheme_id_uri* 속성의 값은 "urn: scte: scte35:2013: xml"입니다 (참조 [http://dashif.org/identifiers/event-schemes/](https://dashif.org/identifiers/event-schemes/) ). 표준에서는 중복성을 위해이 emsg를 여러 번 보낼 것을 권장 하므로이 예에서는 이미 처리 되었으며 새 메시지만 처리 하는 emsg Id의 목록을 유지 관리 합니다. [**Datareader**](https://docs.microsoft.com/uwp/api/windows.storage.streams.datareader.FromBuffer) 를 호출 하 여 큐 데이터를 읽고 [**UnicodeEncoding**](https://docs.microsoft.com/uwp/api/windows.storage.streams.datareader.UnicodeEncoding) 속성을 설정 하 여 인코딩을 u t f-8로 설정한 다음 데이터를 읽어 새 **DataReader** 를 만듭니다. 이 예제에서는 메시지 페이로드가 디버그 출력에 기록 됩니다. 실제 앱은 페이로드 데이터를 사용 하 여 광고의 재생을 예약 합니다.

[!code-cs[EmsgCueEntered](./code/MediaSource_RS1/cs/MainPage_Cues.xaml.cs#SnippetEmsgCueEntered)]


## <a name="related-topics"></a>관련 항목

* [미디어 재생](media-playback.md)
* [미디어 항목, 재생 목록 및 트랙](media-playback-with-mediasource.md)


 




