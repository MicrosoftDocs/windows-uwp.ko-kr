---
author: drewbatgit
ms.assetid: C5623861-6280-4352-8F22-80EB009D662C
description: MediaSource 클래스는 로컬 또는 원격 파일과 같은 여러 원본에서 미디어를 참조하고 재생하는 일반적인 방법을 제공하며 기본 미디어 형식에 상관없이 미디어 데이터에 액세스하기 위한 공통 모델을 공개합니다.
title: MediaSource를 사용하여 미디어 재생
---

# MediaSource를 사용하여 미디어 재생

\[ Windows 10의 UWP 앱에 맞게 업데이트되었습니다. Windows 8.x 문서는 [보관](http://go.microsoft.com/fwlink/p/?linkid=619132)을 참조하세요. \]


\[일부 정보는 상업용으로 출시되기 전에 상당 부분 수정될 수 있는 시험판 제품과 관련이 있습니다. Microsoft는 여기에 제공된 정보에 대해 명시적 또는 묵시적 보증을 하지 않습니다.\]

[
            **MediaSource**](https://msdn.microsoft.com/library/windows/apps/dn930905) 클래스는 로컬 또는 원격 파일과 같은 여러 원본에서 미디어를 참조하고 재생하는 일반적인 방법을 제공하며 기본 미디어 형식에 상관없이 미디어 데이터에 액세스하기 위한 공통 모델을 공개합니다. [
            **MediaPlaybackItem**](https://msdn.microsoft.com/library/windows/apps/dn930939) 클래스는 **MediaSource**의 기능을 확장하여 미디어 항목에 포함된 여러 오디오, 비디오 및 메타데이터 트랙에서 관리하고 선택할 수 있습니다. [
            **MediaPlaybackList**](https://msdn.microsoft.com/library/windows/apps/dn930955)를 사용하면 하나 이상의 미디어 재생 항목에서 재생 목록을 만들 수 있습니다.

이 문서의 코드는 [비디오 재생 SDK](http://go.microsoft.com/fwlink/p/?LinkId=620020&clcid=0x409) 샘플에서 조정되었습니다. 이 샘플을 다운로드하여 상황에 따라 사용된 코드를 참조하거나 자체 앱을 처음 빌드하기 시작할 때 사용할 수 있습니다.

## MediaSource 만들기 및 재생

클래스에서 제공하는 팩터리 메서드 중 하나를 호출하여 **MediaSource**의 새 인스턴스를 만듭니다.

-   [**CreateFromAdaptiveMediaSource**](https://msdn.microsoft.com/library/windows/apps/dn930906)
-   [**CreateFromIMediaSource**](https://msdn.microsoft.com/library/windows/apps/dn965527)
-   [**CreateFromMediaStreamSource**](https://msdn.microsoft.com/library/windows/apps/dn930907)
-   [**CreateFromMseStreamSource**](https://msdn.microsoft.com/library/windows/apps/dn930908)
-   [**CreateFromStorageFile**](https://msdn.microsoft.com/library/windows/apps/dn930909)
-   [**CreateFromStream**](https://msdn.microsoft.com/library/windows/apps/dn930910)
-   [**CreateFromStreamReference**](https://msdn.microsoft.com/library/windows/apps/dn930911)
-   [**CreateFromUri**](https://msdn.microsoft.com/library/windows/apps/dn930912)

**MediaSource**를 만든 후에 [**SetPlaybackSource**](https://msdn.microsoft.com/library/windows/apps/dn899085)를 호출하여 **[MediaElement](https://msdn.microsoft.com/library/windows/apps/br242926)**로 또는 [**Source**](https://msdn.microsoft.com/library/windows/apps/dn987010) 속성을 설정하여 [**MediaPlayer**](https://msdn.microsoft.com/library/windows/apps/dn652535)로 원본을 직접 재생할 수 있습니다. 다음 예제에서는 **MediaSource**를 사용하여 사용자가 선택한 **MediaElement**의 미디어 파일을 재생하는 방법을 보여 줍니다.

이 시나리오를 완료하기 위해서는 [**Windows.Media.Core**](https://msdn.microsoft.com/library/windows/apps/dn278962) 및 [**Windows.Media.Playback**](https://msdn.microsoft.com/library/windows/apps/dn640562) 네임스페이스를 포함해야 합니다.

[!code-cs[사용](./code/MediaSource_Win10/cs/MainPage.xaml.cs#SnippetUsing)]

형식 **MediaSource**의 변수를 선언합니다. 이 문서의 예제에서는 미디어 원본이 클래스 멤버로 선언되므로 여러 위치에서 액세스할 수 있습니다.

[!code-cs[DeclareMediaSource](./code/MediaSource_Win10/cs/MainPage.xaml.cs#SnippetDeclareMediaSource)]

사용자가 재생할 미디어 파일을 선택할 수 있도록 [**FileOpenPicker**](https://msdn.microsoft.com/library/windows/apps/br207847)를 사용합니다. 선택기의 [**PickSingleFileAsync**](https://msdn.microsoft.com/library/windows/apps/jj635275) 메서드에서 반환된 [**StorageFile**](https://msdn.microsoft.com/library/windows/apps/br227171) 개체를 통해 [**MediaSource.CreateFromStorageFile**](https://msdn.microsoft.com/library/windows/apps/dn930909)을 호출하여 새 MediaObject를 초기화합니다. 마지막으로 [**SetPlaybackSource**](https://msdn.microsoft.com/library/windows/apps/dn899085) 메서드를 호출하여 미디어 원본을 **MediaElement**에 대한 재생 원본으로 설정합니다.

[!code-cs[PlayMediaSource](./code/MediaSource_Win10/cs/MainPage.xaml.cs#SnippetPlayMediaSource)]

## MediaPlaybackItem을 사용하여 여러 오디오, 비디오 및 메타데이터 트랙 처리

여러 종류의 원본에서 미디어를 재생하는 것은 일반적인 방법이므로 재생하기 위해 [**MediaSource**](https://msdn.microsoft.com/library/windows/apps/dn930905)를 편하게 사용할 수 있지만 고급 동작을 액세스하려면 [**MediaPlaybackItem**](https://msdn.microsoft.com/library/windows/apps/dn930939)을 사용해야 할 수 있습니다. 미디어 항목에 대한 여러 오디오, 비디오 및 데이터 트랙을 액세스하고 관리하는 기능이 포함됩니다.

**MediaPlaybackItem**을 저장하기 위한 변수를 선언합니다.

[!code-cs[DeclareMediaPlaybackItem](./code/MediaSource_Win10/cs/MainPage.xaml.cs#SnippetDeclareMediaPlaybackItem)]

초기화된 **MediaSource** 개체에서 생성자를 호출하고 전달하여 **MediaPlaybackItem**을 만듭니다.

앱이 미디어 재생 항목의 여러 오디오, 비디오 또는 데이터 트랙을 지원하는 경우 [**AudioTracksChanged**](https://msdn.microsoft.com/library/windows/apps/dn930948), [**VideoTracksChanged**](https://msdn.microsoft.com/library/windows/apps/dn930954) 또는 [**TimedMetadataTracksChanged**](https://msdn.microsoft.com/library/windows/apps/dn930952) 이벤트에 대한 이벤트 처리기를 등록합니다.

마지막으로 **MediaElement** 또는 **MediaPlayer**의 재생 원본을 **MediaPlaybackItem**으로 설정합니다.

[!code-cs[PlayMediaPlaybackItem](./code/MediaSource_Win10/cs/MainPage.xaml.cs#SnippetPlayMediaPlaybackItem)]

**참고**  
**MediaSource**는 단일 **MediaPlaybackItem**에만 연결될 수 있습니다. 원본에서 **MediaPlaybackItem**을 만든 후에 동일한 원본에서 다른 재생 항목 만들려고 하면 오류가 발생합니다. 또한 미디어 원본에서 **MediaPlaybackItem**을 만든 후에는 **MediaSource** 개체를 **MediaElement** 또는 **MediaPlayer**에 대한 원본으로 직접 설정할 수 없지만 **MediaPlaybackItem**를 대신 사용해야 합니다.

[
            **VideoTracksChanged**](https://msdn.microsoft.com/library/windows/apps/dn930954) 이벤트는 여러 비디오 트랙을 포함하는 **MediaPlaybackItem**이 임의 재생 원본에 할당된 후 발생하며 비디오 트랙 목록이 항목 변경으로 인해 변경되면 다시 발생할 수 있습니다. 이 이벤트에 대한 처리기를 사용하면 사용자가 사용 가능한 트랙 간에 전환할 수 있도록 UI를 업데이트할 수 있습니다. 이 예제에서는 [**ComboBox**](https://msdn.microsoft.com/library/windows/apps/br209348)를 사용하여 사용 가능한 비디오 트랙을 표시합니다.

[!code-xml[VideoComboBox](./code/MediaSource_Win10/cs/MainPage.xaml#SnippetVideoComboBox)]

**VideoTracksChanged** 처리기에서 재생 항목의 **[VideoTracks](https://msdn.microsoft.com/library/windows/apps/dn930953)** 목록에 있는 모든 트랙을 루핑합니다. 각 트랙에 대해 새 [**ComboBoxItem**](https://msdn.microsoft.com/library/windows/apps/br209349)이 만들어집니다. 트랙에 레이블이 아직 없는 경우 트랙 인덱스에서 레이블을 생성합니다. 콤보 상자 항목의 [**Tag**](https://msdn.microsoft.com/library/windows/apps/br208745) 속성은 트랙 인덱스로 설정되므로 나중에 식별할 수 있습니다. 마지막으로 콤보 상자에 항목이 추가됩니다. 모든 UI 변경 사항이 UI 스레드에서 만들어져야 하며 이 이벤트가 다른 스레드에서 발생하므로 이러한 작업은 [**CoreDispatcher.RunAsync**](https://msdn.microsoft.com/library/windows/apps/hh750317) 호출 내에서 수행됩니다.

[!code-cs[VideoTracksChanged](./code/MediaSource_Win10/cs/MainPage.xaml.cs#SnippetVideoTracksChanged)]

콤보 상자에 대한 [**SelectionChanged**](https://msdn.microsoft.com/library/windows/apps/br209776) 처리기에서 트랙 인덱스가 선택한 항목의 **Tag** 속성에서 검색됩니다. 미디어 재생 항목 [**VideoTracks**](https://msdn.microsoft.com/library/windows/apps/dn930953) 목록의 [**SelectedIndex**](https://msdn.microsoft.com/library/windows/apps/dn956634) 속성을 설정하면 **MediaElement** 또는 **MediaPlayer**가 활성 비디오 트랙을 지정한 인덱스로 전환합니다.

[!code-cs[VideoTracksSelectionChanged](./code/MediaSource_Win10/cs/MainPage.xaml.cs#SnippetVideoTracksSelectionChanged)]

여러 오디오 트랙을 사용한 미디어 항목 관리는 비디오 트랙과 정확히 동일하게 작동합니다. [
            **AudioTracksChanged**](https://msdn.microsoft.com/library/windows/apps/dn930948)를 처리하여 재생 항목의 [**AudioTracks**](https://msdn.microsoft.com/library/windows/apps/dn930947) 목록에 있는 오디오 트랙으로 UI를 업데이트합니다. 사용자가 오디오 트랙을 선택하는 경우 **AudioTracks** 목록의 [**SelectedIndex**](https://msdn.microsoft.com/library/windows/apps/dn930937) 속성을 설정하여 **MediaElement** 또는 **MediaPlayer**가 활성 오디오 트랙을 지정한 인덱스로 전환하도록 합니다.

[!code-xml[AudioComboBox](./code/MediaSource_Win10/cs/MainPage.xaml#SnippetAudioComboBox)]

[!code-cs[AudioTracksChanged](./code/MediaSource_Win10/cs/MainPage.xaml.cs#SnippetAudioTracksChanged)]

[!code-cs[AudioTracksSelectionChanged](./code/MediaSource_Win10/cs/MainPage.xaml.cs#SnippetAudioTracksSelectionChanged)]

오디오 및 비디오 이외에 **MediaPlaybackItem** 개체에는 0개 이상의 [**TimedMetadataTrack**](https://msdn.microsoft.com/library/windows/apps/dn956580) 개체가 포함될 수 있습니다. 시간이 제한된 메타데이터 트랙에는 자막 또는 자막 텍스트가 포함되거나 앱의 소유에 속하는 사용자 지정 데이터가 포함될 수 있습니다. 시간이 제한된 메타데이터 트랙에는 [**DataCue**](https://msdn.microsoft.com/library/windows/apps/dn930892) 또는 [**TimedTextCue**](https://msdn.microsoft.com/library/windows/apps/dn956655)와 같은 [**IMediaCue**](https://msdn.microsoft.com/library/windows/apps/dn930899)에서 상속되는 개체가 나타내는 신호 목록이 포함됩니다. 각 신호의 시작 시간 및 기간은 신호가 언제 활성화되고 얼마나 오래 활성화되는지를 결정합니다.

오디오 트랙 및 비디오 트랙과 유사하게 미디어 항목에 대한 시간이 제한된 메타데이터 트랙은 **MediaPlaybackItem**의 [**TimedMetadataTracksChanged**](https://msdn.microsoft.com/library/windows/apps/dn930952) 이벤트를 처리하여 검색될 수 있습니다. 그러나 시간이 제한된 메타데이터 트랙을 사용하여 사용자가 한 번에 둘 이상의 메타데이터 트랙을 사용하려고 할 수 있습니다. 또한 앱 시나리오에 따라 사용자의 개입 없이 메타데이터 트랙을 자동으로 활성화하거나 비활성화하려고 할 수 있습니다. 설명하자면 이 예제에서는 미디어 항목의 각 메타데이터에 대해 [**ToggleButton**](https://msdn.microsoft.com/library/windows/apps/br209795)을 추가하여 사용자가 트랙을 활성화하거나 비활성화하도록 합니다. 각 단추의 **Tag** 속성은 관련된 메타데이터 트랙의 인덱스로 설정되므로 단추를 전환할 때 식별할 수 있습니다.

[!code-xml[MetaStackPanel](./code/MediaSource_Win10/cs/MainPage.xaml#SnippetMetaStackPanel)]

[!code-cs[TimedMetadataTracksChanged](./code/MediaSource_Win10/cs/MainPage.xaml.cs#SnippetTimedMetadataTrackschanged)]

둘 이상의 메타데이터 트랙이 한 번에 활성화될 수 있으므로 메타데이터 트랙 목록에 대한 활성 인덱스를 간단히 설정하지 않습니다. 대신 **MediaPlaybackItem** 개체의 [**SetPresentationMode**](https://msdn.microsoft.com/library/windows/apps/dn986977) 메서드를 호출하여 전환하려는 트랙의 인덱스를 전달한 다음 [**TimedMetadataTrackPresentationMode**](https://msdn.microsoft.com/library/windows/apps/dn987016) 열거에서 값을 제공합니다. 선택하는 프레젠테이션 모드는 앱의 구현에 따라 다릅니다. 이 예제에서는 메타데이터 트랙이 활성화될 때 **PlatformPresented**로 설정됩니다. 텍스트 기반 트랙의 경우 이는 시스템이 트랙에서 자동으로 텍스트 신호를 표시하는 것을 의미합니다. 토글 단추가 꺼지면 프레젠테이션 모드가 **Disabled**로 설정되며 이는 표시될 텍스트 및 발행할 신호 이벤트가 없다는 의미입니다. 이 문서의 뒷부분에서 신호 이벤트를 설명합니다.

[!code-cs[ToggleChecked](./code/MediaSource_Win10/cs/MainPage.xaml.cs#SnippetToggleChecked)]

[!code-cs[ToggleUnchecked](./code/MediaSource_Win10/cs/MainPage.xaml.cs#SnippetToggleUnchecked)]

## TimedTextSource를 사용하여 시간이 제한된 외부 텍스트 추가

일부 시나리오의 경우에는 여러 로캘의 자막이 있는 별도 파일과 같이 미디어 항목과 관련된 시간이 지정된 텍스트가 있는 외부 파일이 있을 수 있습니다. [
            **TimedTextSource**](https://msdn.microsoft.com/library/windows/apps/dn956679) 클래스를 사용하여 스트림이나 URI에서 시간이 지정된 외부 텍스트 파일을 로드합니다.

이 예제에서는 트랙을 확인한 후 식별하기 위해 원본 URI 및 **TimedTextSource** 개체를 키/값 쌍으로 사용하는 미디어 항목에 대해 시간이 지정된 텍스트 원본 목록을 저장하도록 **Dictionary** 컬렉션을 사용합니다.

[!code-cs[TimedTextSourceMap](./code/MediaSource_Win10/cs/MainPage.xaml.cs#SnippetTimedTextSourceMap)]

[
            **CreateFromUri**](https://msdn.microsoft.com/library/windows/apps/dn708190)를 호출하여 시간이 지정된 각 외부 텍스트 파일에 대해 새 **TimedTextSource**를 만듭니다. 시간이 지정된 텍스트 원본에 대한 **Dictionary**에 항목을 추가합니다. 항목을 로드하지 못하거나 해당 항목이 성공적으로 로드된 후에 추가 속성을 설정하는 데 실패한 경우 처리할 [**TimedTextSource.Resolved**](https://msdn.microsoft.com/library/windows/apps/dn965540) 이벤트에 대한 처리기를 추가합니다.

모든 **TimedTextSource** 개체를 [**ExternalTimedTextSources**](https://msdn.microsoft.com/library/windows/apps/dn930916) 컬렉션에 추가하여 **MediaSource**에 등록합니다. 시간이 지정된 외부 텍스트 원본을 해당 원본에서 만든 **MediaPlaybackItem**이 아니라 **MediaSource**에 직접 추가합니다. UI를 업데이트하여 외부 텍스트 트랙을 반영하려면 이 문서의 앞부분에서 설명한 대로 **TimedMetadataTracksChanged** 이벤트를 등록하고 처리합니다.

[!code-cs[TimedTextSource](./code/MediaSource_Win10/cs/MainPage.xaml.cs#SnippetTimedTextSource)]

[
            **TimedTextSource.Resolved**](https://msdn.microsoft.com/library/windows/apps/dn965540) 이벤트에 대한 처리기에서 처리기에 전달된 [**TimedTextSourceResolveResultEventArgs**](https://msdn.microsoft.com/library/windows/apps/dn965537)의 **Error** 속성을 확인하여 시간이 지정된 텍스트 데이터 로드를 시도하는 동안 오류가 발생했는지 확인합니다. 항목이 성공적으로 확인된 경우 이 처리기를 사용하여 확인된 트랙의 추가 속성을 업데이트할 수 있습니다. 이 예제에서는 이전에 **Dictionary**에 저장된 URI에 따라 각 트랙에 대한 레이블을 추가합니다.

[!code-cs[TimedTextSourceResolved](./code/MediaSource_Win10/cs/MainPage.xaml.cs#SnippetTimedTextSourceResolved)]

## 추가 메타데이터 트랙 추가

동적으로 코드에서 사용자 지정 메타데이터 트랙을 만들고 미디어 원본에 연결할 수 있습니다. 만들어진 트랙에는 자막 또는 자막 텍스트가 포함되거나 독점 앱 데이터가 포함될 수 있습니다.

생성자를 호출하고 ID, 언어 식별자 및 [**TimedMetadataKind**](https://msdn.microsoft.com/library/windows/apps/dn956578) 열거의 값을 지정하여 새 [**TimedMetadataTrack**](https://msdn.microsoft.com/library/windows/apps/dn956580)을 만듭니다. [
            **CueEntered**](https://msdn.microsoft.com/library/windows/apps/dn956583) 및 [**CueExited**](https://msdn.microsoft.com/library/windows/apps/dn956584) 이벤트에 대한 처리기를 등록합니다. 이러한 이벤트는 신호 시작 시간에 도달할 때 및 신호 기간이 만료될 때 각각 발생합니다.

만들어진 메타데이터 트랙의 형식에 적합한 새 신호 개체를 만들고 해당 트랙에 대한 ID, 시작 시간 및 기간을 설정합니다. 이 예제에서는 데이터 트랙이 만들어지므로 [**DataCue**](https://msdn.microsoft.com/library/windows/apps/dn930892) 개체 집합이 생성되고 앱별 데이터를 포함하는 버퍼가 각 신호에 대해 제공됩니다. 새 트랙을 등록하려면 **MediaSource** 개체의 [**ExternalTimedMetadataTracks**](https://msdn.microsoft.com/library/windows/apps/dn930915) 컬렉션에 추가합니다.

[!code-cs[AddDataTrack](./code/MediaSource_Win10/cs/MainPage.xaml.cs#SnippetAddDataTrack)]

**CueEntered** 이벤트는 관련된 트랙에 **ApplicationPresented**, **Hidden** 또는 **PlatformPresented**의 프레젠테이션 모드가 있는 한 신호 시작 시간에 도달할 때 발생합니다. 트랙에 대한 프레젠테이션 모드가 **Disabled**인 동안에는 신호 이벤트는 메타데이터 트랙에 대해 발생하지 않습니다. 이 예제에서는 디버그 창에 신호와 관련된 사용자 지정 데이터를 단순히 출력합니다.

[!code-cs[DataCueEntered](./code/MediaSource_Win10/cs/MainPage.xaml.cs#SnippetDataCueEntered)]

이 예제에서는 트랙을 만들고 [**TimedTextCue**](https://msdn.microsoft.com/library/windows/apps/dn956655) 개체를 사용하여 트랙에 신호를 추가할 때 **TimedMetadataKind.Caption**을 지정하여 사용자 지정 텍스트를 추가합니다.

[!code-cs[AddTextTrack](./code/MediaSource_Win10/cs/MainPage.xaml.cs#SnippetAddTextTrack)]

[!code-cs[TextCueEntered](./code/MediaSource_Win10/cs/MainPage.xaml.cs#SnippetTextCueEntered)]

## MediaPlaybackList를 사용하여 미디어 항목 목록 재생

[
            **MediaPlaybackList**](https://msdn.microsoft.com/library/windows/apps/dn930955)를 사용하면 **MediaPlaybackItem** 개체가 표시하는 미디어 항목의 재생 목록을 만들 수 있습니다.

**참고** [**MediaPlaybackList**](https://msdn.microsoft.com/library/windows/apps/dn930955)의 항목이 매끄러운 재생을 사용하여 렌더링됩니다. 시스템은 MP3 또는 AAC로 인코딩된 파일에 제공된 메타데이터를 사용하여 매끄러운 재생에 필요한 지연 또는 패딩 보정을 결정합니다. MP3 또는 AAC로 인코딩된 파일이 이 메타데이터를 제공하지 않는 경우 시스템에서 지연이나 패딩을 스스로 결정합니다. 이러한 인코더는 지연 또는 패딩을 도입하지 않으므로 PCM, FLAC, ALAC 등 무손실 형식에 대해 시스템은 어떠한 조치도 취하지 않습니다.

시작하려면 변수를 선언하여 **MediaPlaybackList**를 저장합니다.

[!code-cs[DeclareMediaPlaybackList](./code/MediaSource_Win10/cs/MainPage.xaml.cs#SnippetDeclareMediaPlaybackList)]

이 문서의 앞부분에서 설명한 동일한 절차를 사용하여 목록에 추가하려는 각 미디어 항목에 대한 **MediaPlaybackItem**을 만듭니다. **MediaPlaybackList** 개체를 초기화하고 이 개체에 미디어 재생 항목을 추가합니다. [
            **CurrentItemChanged**](https://msdn.microsoft.com/library/windows/apps/dn930957) 이벤트에 대한 처리기를 등록합니다. 이 이벤트를 사용하면 UI를 업데이트하여 현재 재생 중인 미디어 항목을 반영할 수 있습니다. 마지막으로 **MediaElement** 또는 **MediaPlayer**의 재생 원본을 **MediaPlaybackList**로 설정합니다.

[!code-cs[PlayMediaPlaybackList](./code/MediaSource_Win10/cs/MainPage.xaml.cs#SnippetPlayMediaPlaybackList)]

**CurrentItemChanged** 이벤트 처리기에서 UI를 업데이트하여 이벤트로 전달된 [**CurrentMediaPlaybackItemChangedEventArgs**](https://msdn.microsoft.com/library/windows/apps/dn930929) 개체의 [**NewItem**](https://msdn.microsoft.com/library/windows/apps/dn930930) 속성을 사용하여 검색될 수 있는 현재 재생 중인 항목을 반영합니다. 이 이벤트에서 UI를 업데이트하는 경우 UI 스레드에서 업데이트되도록 [**CoreDispatcher.RunAsync**](https://msdn.microsoft.com/library/windows/apps/hh750317)에 대한 호출 안에서 수행해야 합니다.

[!code-cs[MediaPlaybackListItemChanged](./code/MediaSource_Win10/cs/MainPage.xaml.cs#SnippetMediaPlaybackListItemChanged)]

[
            **MovePrevious**](https://msdn.microsoft.com/library/windows/apps/mt146455) 또는 [**MoveNext**](https://msdn.microsoft.com/library/windows/apps/mt146454)를 호출하여 미디어 플레이어가 **MediaPlaybackList**에서 이전 또는 이후 항목을 재생하도록 합니다.

[!code-cs[PrevButton](./code/MediaSource_Win10/cs/MainPage.xaml.cs#SnippetPrevButton)]

[!code-cs[NextButton](./code/MediaSource_Win10/cs/MainPage.xaml.cs#SnippetNextButton)]

[
            **ShuffleEnabled**](https://msdn.microsoft.com/library/windows/apps/mt146457) 속성을 설정하여 미디어 플레이어가 임의 순서로 목록의 항목을 재생해야 하는지 여부를 지정합니다.

[!code-cs[ShuffleButton](./code/MediaSource_Win10/cs/MainPage.xaml.cs#SnippetShuffleButton)]

[
            **AutoRepeatEnabled**](https://msdn.microsoft.com/library/windows/apps/mt146452) 속성을 설정하여 미디어 플레이어가 목록을 반복 재생해야 하는지 여부를 지정합니다.

[!code-cs[RepeatButton](./code/MediaSource_Win10/cs/MainPage.xaml.cs#SnippetRepeatButton)]

 

 






<!--HONumber=May16_HO2-->


