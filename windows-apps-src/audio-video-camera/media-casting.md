---
ms.assetid: 40B97E0C-EB1B-40C2-A022-1AB95DFB085E
description: 이 문서에서는 유니버설 Windows 앱에서 원격 장치로 미디어를 캐스팅 하는 방법을 보여 줍니다.
title: 미디어 캐스팅
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: a23e6c58325a8679a8df2ec0d3f429f75a230602
ms.sourcegitcommit: c3ca68e87eb06971826087af59adb33e490ce7da
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 09/02/2020
ms.locfileid: "89363926"
---
# <a name="media-casting"></a>미디어 캐스팅



이 문서에서는 유니버설 Windows 앱에서 원격 장치로 미디어를 캐스팅 하는 방법을 보여 줍니다.

## <a name="built-in-media-casting-with-mediaplayerelement"></a>MediaPlayerElement를 사용 하 여 기본 제공 미디어 캐스팅

유니버설 Windows 앱에서 미디어를 캐스트 하는 가장 간단한 방법은 기본 제공 되는 [**MediaPlayerElement**](/uwp/api/Windows.UI.Xaml.Controls.MediaPlayerElement) 컨트롤의 캐스팅 기능을 사용 하는 것입니다.

사용자가 **MediaPlayerElement** 컨트롤에서 재생할 비디오 파일을 열 수 있도록 하려면 다음 네임 스페이스를 프로젝트에 추가 합니다.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/MediaCasting_RS1/cs/MainPage.xaml.cs" id="SnippetBuiltInCastingUsing":::

앱의 XAML 파일에서 **MediaPlayerElement** 를 추가 하 고 [**AreTransportControlsEnabled**](/uwp/api/windows.ui.xaml.controls.mediaelement.aretransportcontrolsenabled) 를 true로 설정 합니다.

:::code language="xml" source="~/../snippets-windows/windows-uwp/audio-video-camera/MediaCasting_RS1/cs/MainPage.xaml" id="SnippetMediaElement":::

사용자가 파일을 선택 하기 시작할 수 있도록 하는 단추를 추가 합니다.

:::code language="xml" source="~/../snippets-windows/windows-uwp/audio-video-camera/MediaCasting_RS1/cs/MainPage.xaml" id="SnippetOpenButton":::

단추에 대 한 [**click**](/uwp/api/windows.ui.xaml.controls.primitives.buttonbase.click) 이벤트 처리기에서 [**fileopenpicker**](/uwp/api/Windows.Storage.Pickers.FileOpenPicker)의 새 인스턴스를 만들고, [**fil및 필터**](/uwp/api/windows.storage.pickers.fileopenpicker.filetypefilter) 컬렉션에 비디오 파일 형식을 추가 하 고, 시작 위치를 사용자의 비디오 라이브러리로 설정 합니다.

[**PickSingleFileAsync**](/uwp/api/windows.storage.pickers.fileopenpicker.picksinglefileasync) 를 호출 하 여 파일 선택 대화 상자를 시작 합니다. 이 메서드가 반환 될 때 결과는 비디오 파일을 나타내는 [**StorageFile**](/uwp/api/Windows.Storage.StorageFile) 개체입니다. 파일이 null이 아닌지 확인 합니다. 사용자가 선택 작업을 취소 하는 경우에는입니다. 파일의 [**Openasync**](/uwp/api/windows.storage.storagefile.openasync) 메서드를 호출 하 여 파일에 대 한 [**IRandomAccessStream**](/uwp/api/Windows.Storage.Streams.IRandomAccessStream) 를 가져옵니다. 마지막으로 [**CreateFromStorageFile**](/uwp/api/windows.media.core.mediasource.createfromstoragefile) 를 호출 하 여 선택한 파일에서 새 **MediaSource** 개체를 만들고 **MediaPlayerElement** 개체의 [**Source**](/uwp/api/windows.ui.xaml.controls.mediaplayerelement.source) 속성에 할당 하 여 비디오 파일에 컨트롤의 비디오 소스를 만듭니다.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/MediaCasting_RS1/cs/MainPage.xaml.cs" id="SnippetOpenButtonClick":::

비디오가 **MediaPlayerElement**에 로드 되 면 사용자는 전송 컨트롤의 캐스팅 단추를 눌러 로드 된 미디어를 캐스팅할 장치를 선택할 수 있는 기본 제공 대화 상자를 시작할 수 있습니다.

![mediaelement 캐스팅 단추](images/media-element-casting-button.png)

> [!NOTE] 
> Windows 10 버전 1607부터 **MediaPlayer** 클래스를 사용 하 여 미디어 항목을 재생 하는 것이 좋습니다. **MediaPlayerElement** 는 xaml 페이지에서 **MediaPlayer** 의 콘텐츠를 렌더링 하는 데 사용 되는 간단한 XAML 컨트롤입니다. **MediaElement** 컨트롤은 이전 버전과의 호환성을 위해 계속 지원 됩니다. **Mediaplayer** 및 **MediaPlayerElement** 를 사용 하 여 미디어 콘텐츠를 재생 하는 방법에 대 한 자세한 내용은 [Mediaplayer로 오디오 및 비디오 재생](play-audio-and-video-with-mediaplayer.md)을 참조 하세요. **MediaSource** 및 관련 api를 사용 하 여 미디어 콘텐츠로 작업 하는 방법에 대 한 자세한 내용은 [미디어 항목, 재생 목록 및 트랙](media-playback-with-mediasource.md)을 참조 하세요.

## <a name="media-casting-with-the-castingdevicepicker"></a>CastingDevicePicker를 사용 하 여 미디어 캐스팅

미디어를 장치로 캐스트 하는 두 번째 방법은 [**CastingDevicePicker**](/uwp/api/Windows.Media.Casting.CastingDevicePicker)를 사용 하는 것입니다. 이 클래스를 사용 하려면 프로젝트에 [**캐스팅이**](/uwp/api/Windows.Media.Casting) 네임 스페이스를 포함 합니다.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/MediaCasting_RS1/cs/MainPage.xaml.cs" id="SnippetCastingNamespace":::

**CastingDevicePicker** 개체에 대 한 멤버 변수를 선언 합니다.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/MediaCasting_RS1/cs/MainPage.xaml.cs" id="SnippetDeclareCastingPicker":::

페이지가 초기화 되 면 캐스팅 선택기의 새 인스턴스를 만들고 [**필터**](/uwp/api/windows.media.casting.castingdevicepicker.filter) 를 [**supportsvideo**](/uwp/api/Windows.Media.Casting.CastingDevicePickerFilter) 속성으로 설정 하 여 선택에 나열 된 캐스팅 장치가 비디오를 지원 해야 함을 나타낼 수 있습니다. 사용자가 캐스트할 장치를 선택할 때 발생 하는 [**CastingDeviceSelected**](/uwp/api/windows.media.casting.castingdevicepicker.castingdeviceselected) 이벤트에 대 한 처리기를 등록 합니다.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/MediaCasting_RS1/cs/MainPage.xaml.cs" id="SnippetInitCastingPicker":::

XAML 파일에서 사용자가 선택기를 시작할 수 있도록 하는 단추를 추가 합니다.

:::code language="xml" source="~/../snippets-windows/windows-uwp/audio-video-camera/MediaCasting_RS1/cs/MainPage.xaml" id="SnippetCastPickerButton":::

단추에 대 한 **click** 이벤트 처리기에서 [**TransformToVisual**](/uwp/api/windows.ui.xaml.uielement.transformtovisual) 를 호출 하 여 다른 요소를 기준으로 UI 요소의 변환을 가져옵니다. 이 예제에서 변환은 응용 프로그램 창의 시각적 루트를 기준으로 하는 캐스트 선택 단추의 위치입니다. [**CastingDevicePicker**](/uwp/api/Windows.Media.Casting.CastingDevicePicker) 개체의 [**Show**](/uwp/api/windows.media.casting.castingdevicepicker.show) 메서드를 호출 하 여 캐스팅 선택 대화 상자를 시작 합니다. 시스템에서 사용자가 누른 단추에서 대화 상자를 바로 이동할 수 있도록 캐스트 선택 단추의 위치와 크기를 지정 합니다.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/MediaCasting_RS1/cs/MainPage.xaml.cs" id="SnippetCastPickerButtonClick":::

**CastingDeviceSelected** 이벤트 처리기에서 사용자가 선택한 캐스팅 장치를 나타내는 이벤트 인수의 [**SelectedCastingDevice**](/uwp/api/windows.media.casting.castingdeviceselectedeventargs.selectedcastingdevice) 속성에 대 한 [**CreateCastingConnection**](/uwp/api/windows.media.casting.castingdevice.createcastingconnection) 메서드를 호출 합니다. [**Erroroccurred**](/uwp/api/windows.media.casting.castingconnection.erroroccurred) 및 [**StateChanged**](/uwp/api/windows.media.casting.castingconnection.statechanged) 이벤트에 대 한 처리기를 등록 합니다. 마지막으로 [**RequestStartCastingAsync**](/uwp/api/windows.media.casting.castingconnection.requeststartcastingasync) 를 호출 하 여 캐스팅을 시작 하 고, 결과를 **MediaPlayerElement** 컨트롤의 **MediaPlayer** 개체의 [**GetAsCastingSource**](/uwp/api/windows.ui.xaml.controls.mediaelement.getascastingsource) 메서드에 전달 하 여 캐스팅할 미디어를 **MediaPlayerElement**와 연결 된 **mediaplayer** 의 콘텐츠로 지정 합니다.

> [!NOTE] 
> 캐스팅 연결은 UI 스레드에서 시작 해야 합니다. **CastingDeviceSelected** 는 ui 스레드에서 호출 되지 않으므로 ui 스레드에서 호출 되도록 하는 [**CoreDispatcher**](/uwp/api/windows.ui.core.coredispatcher.runasync) 에 대 한 호출 내에 이러한 호출을 두어야 합니다.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/MediaCasting_RS1/cs/MainPage.xaml.cs" id="SnippetCastingDeviceSelected":::

**Erroroccurred** 및 **StateChanged** 이벤트 처리기에서 사용자에 게 현재 캐스팅 상태를 알리기 위해 UI를 업데이트 해야 합니다. 이러한 이벤트는 사용자 지정 캐스팅 장치 선택 만들기에 대 한 다음 섹션에서 자세히 설명 합니다.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/MediaCasting_RS1/cs/MainPage.xaml.cs" id="SnippetEmptyStateHandlers":::

## <a name="media-casting-with-a-custom-device-picker"></a>사용자 지정 장치 선택을 사용 하 여 미디어 캐스팅

다음 섹션에서는 캐스팅 하는 장치를 열거 하 고 코드에서 연결을 시작 하 여 사용자 고유의 캐스트 장치 선택 UI를 만드는 방법을 설명 합니다.

사용 가능한 캐스팅 장치를 열거 하려면 프로젝트에 해당 하는 [**Windows. Enumeration**](/uwp/api/Windows.Devices.Enumeration) 네임 스페이스를 포함 합니다.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/MediaCasting_RS1/cs/MainPage.xaml.cs" id="SnippetEnumerationNamespace":::

XAML 페이지에 다음 컨트롤을 추가 하 여이 예제의 기초적인 UI를 구현 합니다.

-   사용 가능한 캐스팅 장치를 찾는 장치 감시자를 시작 하는 단추입니다.
-   캐스팅이 열거 된 사용자에 게 피드백을 제공 하는 [**ProgressRing**](/uwp/api/Windows.UI.Xaml.Controls.ProgressRing) 컨트롤입니다.
-   검색 된 캐스팅 장치를 나열 하는 목록 [**상자**](/uwp/api/Windows.UI.Xaml.Controls.ListBox) 입니다. 컨트롤에 직접 캐스팅 하는 장치 개체를 할당 하 고 [**FriendlyName**](/uwp/api/windows.media.casting.castingdevice.friendlyname) 속성을 계속 표시할 수 있도록 컨트롤에 대 한 [**ItemTemplate**](/uwp/api/windows.ui.xaml.controls.itemscontrol.itemtemplate) 을 정의 합니다.
-   사용자가 캐스팅 장치를 분리 하는 데 사용할 수 있는 단추입니다.

:::code language="xml" source="~/../snippets-windows/windows-uwp/audio-video-camera/MediaCasting_RS1/cs/MainPage.xaml" id="SnippetCustomPickerXAML":::

코드 뒤에서 [**Devicewatcher**](/uwp/api/Windows.Devices.Enumeration.DeviceWatcher) 및 [**CastingConnection**](/uwp/api/Windows.Media.Casting.CastingConnection)에 대 한 멤버 변수를 선언 합니다.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/MediaCasting_RS1/cs/MainPage.xaml.cs" id="SnippetDeclareDeviceWatcher":::

*StartWatcherButton*에 대 한 **클릭** 처리기에서 먼저 단추를 사용 하지 않도록 설정 하 고 장치 열거가 진행 되는 동안 진행률 링을 활성으로 설정 하 여 UI를 업데이트 합니다. 장치를 캐스팅 하는 목록 상자의 선택을 취소 합니다.

다음으로, Deviceinformation을 호출 하 여 장치 감시자를 만듭니다 [**. CreateWatcher**](/uwp/api/windows.devices.enumeration.deviceinformation.createwatcher). 이 메서드를 사용 하 여 다양 한 유형의 장치를 감시할 수 있습니다. [**CastingDevice**](/uwp/api/windows.media.casting.castingdevice.getdeviceselector)에서 반환 된 장치 선택기 문자열을 사용 하 여 비디오 캐스팅을 지 원하는 장치를 시청 하도록 지정 합니다.

마지막으로, [**추가**](/uwp/api/windows.devices.enumeration.devicewatcher.added), [**제거**](/uwp/api/windows.devices.enumeration.devicewatcher.removed), [**enumerationcompleted**](/uwp/api/windows.devices.enumeration.devicewatcher.enumerationcompleted)및 [**중지**](/uwp/api/windows.devices.enumeration.devicewatcher.stopped) 된 이벤트에 대 한 이벤트 처리기를 등록 합니다.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/MediaCasting_RS1/cs/MainPage.xaml.cs" id="SnippetStartWatcherButtonClick":::

**추가** 된 이벤트는 감시자에서 새 장치를 검색할 때 발생 합니다. 이 이벤트에 대 한 처리기에서 [**CastingDevice**](/uwp/api/windows.media.casting.castingdevice.fromidasync) 를 호출 하 고, 검색 된 캐스팅 장치의 ID를 전달 하 여 새 [**CastingDevice**](/uwp/api/Windows.Media.Casting.CastingDevice) 개체를 만들고,이는 처리기에 전달 된 **deviceinformation** 개체에 포함 되어 있습니다.

사용자가 선택할 수 있도록 캐스팅 장치 **목록 상자** 에 **CastingDevice** 를 추가 합니다. XAML에 정의 된 [**ItemTemplate**](/uwp/api/windows.ui.xaml.controls.itemscontrol.itemtemplate) 때문에 [**FriendlyName**](/uwp/api/windows.media.casting.castingdevice.friendlyname) 속성이 목록 상자에서의 항목 텍스트로 사용 됩니다. 이 이벤트 처리기는 UI 스레드에서 호출 되지 않으므로 [**CoreDispatcher 비동기**](/uwp/api/windows.ui.core.coredispatcher.runasync)호출 내에서 ui를 업데이트 해야 합니다.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/MediaCasting_RS1/cs/MainPage.xaml.cs" id="SnippetWatcherAdded":::

**제거** 된 이벤트는 감시자에서 캐스팅 장치가 더 이상 존재 하지 않음을 감지 하면 발생 합니다. 처리기에 전달 된 **추가** 된 개체의 id 속성을 목록 상자의 [**Items**](/uwp/api/windows.ui.xaml.controls.itemscontrol.items) 컬렉션에 **추가** 된 각의 id로 비교 합니다. ID가와 일치 하면 컬렉션에서 해당 개체를 제거 합니다. UI가 업데이트 되 고 있으므로 **Runasync** 호출 내에서이 호출을 수행 해야 합니다.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/MediaCasting_RS1/cs/MainPage.xaml.cs" id="SnippetWatcherRemoved":::

감시자가 장치 검색을 완료 하면 **Enumerationcompleted** 이벤트가 발생 합니다. 이 이벤트에 대 한 처리기에서 UI를 업데이트 하 여 사용자가 장치 열거가 완료 되었음을 알 수 있도록 하 고 [**중지**](/uwp/api/windows.devices.enumeration.devicewatcher.stop)를 호출 하 여 장치 감시자를 중지 합니다.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/MediaCasting_RS1/cs/MainPage.xaml.cs" id="SnippetWatcherEnumerationCompleted":::

중지 된 이벤트는 장치 감시자의 중지를 마칠 때 발생 합니다. 이 이벤트에 대 한 처리기에서 [**ProgressRing**](/uwp/api/Windows.UI.Xaml.Controls.ProgressRing) 컨트롤을 중지 하 고 사용자가 장치 열거 프로세스를 다시 시작할 수 있도록 *startWatcherButton* 를 다시 활성화 합니다.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/MediaCasting_RS1/cs/MainPage.xaml.cs" id="SnippetWatcherStopped":::

사용자가 목록 상자에서 캐스트 장치 중 하나를 선택 하면 [**Selectionchanged**](/uwp/api/windows.ui.xaml.controls.primitives.selector.selectionchanged) 이벤트가 발생 합니다. 이 처리기 내에서 캐스팅 연결이 만들어지고 캐스팅이 시작 됩니다.

먼저 장치 열거가 미디어 캐스트를 방해 하지 않도록 장치 감시자가 중지 되었는지 확인 합니다. 사용자가 선택한 **CastingDevice** 개체에서 [**CreateCastingConnection**](/uwp/api/windows.media.casting.castingdevice.createcastingconnection) 를 호출 하 여 캐스팅 연결을 만듭니다. [**StateChanged**](/uwp/api/windows.media.casting.castingconnection.statechanged) 및 [**erroroccurred**](/uwp/api/windows.media.casting.castingconnection.erroroccurred) 이벤트에 대 한 이벤트 처리기를 추가 합니다.

[**RequestStartCastingAsync**](/uwp/api/windows.media.casting.castingconnection.requeststartcastingasync)를 호출 하 고 **MediaPlayer** 메서드 [**GetAsCastingSource**](/uwp/api/windows.ui.xaml.controls.mediaelement.getascastingsource)를 호출 하 여 반환 된 캐스팅 소스를 전달 하 여 미디어 캐스팅을 시작 합니다. 마지막으로, 사용자가 미디어 캐스팅을 중지할 수 있도록 연결 끊기 단추가 표시 되도록 합니다.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/MediaCasting_RS1/cs/MainPage.xaml.cs" id="SnippetSelectionChanged":::

상태 변경 처리기에서 수행 하는 작업은 캐스팅 연결의 새 상태에 따라 달라 집니다.

-   상태가 **연결** 됨 또는 **렌더링**인 경우 **ProgressRing** 컨트롤이 비활성화 되어 있고 연결 끊기 단추가 표시 되는지 확인 합니다.
-   상태가 **Disconnected**인 경우 목록 상자에서 현재 캐스팅 장치를 선택 취소 하 고 **ProgressRing** 컨트롤을 비활성으로 설정 하 고 연결 끊기 단추를 숨깁니다.
-   상태가 **연결 중**이면 **ProgressRing** 컨트롤을 활성으로 설정 하 고 연결 끊기 단추를 숨깁니다.
-   상태를 **끊는**경우 **ProgressRing** 컨트롤을 활성으로 설정 하 고 연결 끊기 단추를 숨깁니다.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/MediaCasting_RS1/cs/MainPage.xaml.cs" id="SnippetStateChanged":::

**Erroroccurred** 한 이벤트에 대 한 처리기에서 UI를 업데이트 하 여 캐스팅 오류가 발생 했음을 사용자에 게 표시 하 고 목록 상자에서 현재 **CastingDevice** 개체의 선택을 취소 합니다.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/MediaCasting_RS1/cs/MainPage.xaml.cs" id="SnippetErrorOccurred":::

마지막으로, 연결 끊기 단추에 대 한 처리기를 구현 합니다. **CastingConnection** 개체의 [**DisconnectAsync**](/uwp/api/windows.media.casting.castingconnection.disconnectasync) 메서드를 호출 하 여 미디어 캐스팅을 중지 하 고 캐스팅 장치에서 연결을 끊습니다. 이 호출은 [**CoreDispatcher**](/uwp/api/windows.ui.core.coredispatcher.runasync)를 호출 하 여 UI 스레드로 디스패치 되어야 합니다.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/MediaCasting_RS1/cs/MainPage.xaml.cs" id="SnippetDisconnectButton":::

 

 
