---
author: drewbatgit
ms.assetid: 40B97E0C-EB1B-40C2-A022-1AB95DFB085E
description: "이 문서에서는 유니버설 Windows 앱에서 원격 디바이스로 미디어를 캐스팅하는 방법을 보여 줍니다."
title: "미디어 캐스팅"
ms.author: drewbat
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: Windows 10, uwp
ms.openlocfilehash: 8ba90e1538962fdb7ef1434698ea52845713c634
ms.sourcegitcommit: 909d859a0f11981a8d1beac0da35f779786a6889
translationtype: HT
---
# <a name="media-casting"></a>미디어 캐스팅

\[ Windows 10의 UWP 앱에 맞게 업데이트되었습니다. Windows 8.x 문서는 [보관](http://go.microsoft.com/fwlink/p/?linkid=619132)을 참조하세요. \]


이 문서에서는 유니버설 Windows 앱에서 원격 디바이스로 미디어를 캐스팅하는 방법을 보여 줍니다.

## <a name="built-in-media-casting-with-mediaplayerelement"></a>MediaPlayerElement를 사용하는 기본 제공 미디어 캐스팅

유니버설 Windows 앱에서 미디어를 캐스팅하는 가장 간단한 방법은 [**MediaPlayerElement**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Xaml.Controls.MediaPlayerElement) 컨트롤의 기본 제공 캐스팅 접근 권한 값을 사용하는 것입니다.

사용자가 재생할 비디오 파일을 **MediaPlayerElement** 컨트롤에서 열 수 있게 하려면 프로젝트에 다음 네임스페이스를 추가합니다.

[!code-cs[BuiltInCastingUsing](./code/MediaCasting_RS1/cs/MainPage.xaml.cs#SnippetBuiltInCastingUsing)]

앱의 XAML 파일에서 **MediaPlayerElement**를 추가하고 [**AreTransportControlsEnabled**](https://msdn.microsoft.com/library/windows/apps/dn298977)를 true로 설정합니다.

[!code-xml[MediaElement](./code/MediaCasting_RS1/cs/MainPage.xaml#SnippetMediaElement)]

사용자가 파일 선택을 시작할 수 있게 하는 단추를 추가합니다.

[!code-xml[OpenButton](./code/MediaCasting_RS1/cs/MainPage.xaml#SnippetOpenButton)]

단추에 대한 [**Click**](https://msdn.microsoft.com/library/windows/apps/br227737) 이벤트 처리기에서 새 [**FileOpenPicker**](https://msdn.microsoft.com/library/windows/apps/br207847) 인스턴스를 만들고, [**FileTypeFilter**](https://msdn.microsoft.com/library/windows/apps/br207850) 컬렉션에 동영상 파일 형식을 추가하고, 시작 위치를 사용자의 비디오 라이브러리로 설정합니다.

[**PickSingleFileAsync**](https://msdn.microsoft.com/library/windows/apps/jj635275)를 호출하여 파일 선택기 대화 상자를 시작합니다. 이 메서드에서 반환되는 결과는 동영상 파일을 나타내는 [**StorageFile**](https://msdn.microsoft.com/library/windows/apps/br227171) 개체입니다. 파일이 null이 아닌지 확인합니다(사용자가 선택 작업을 취소하는 경우에 null이 됨). 파일의 [**OpenAsync**](https://msdn.microsoft.com/library/windows/apps/br227221.aspx) 메서드를 호출하여 파일에 대한 [**IRandomAccessStream**](https://msdn.microsoft.com/library/windows/apps/br241731)을 가져옵니다. 마지막으로 [**CreateFromStorageFile**](https://msdn.microsoft.com/library/windows/apps/dn930909)을 호출하여 선택된 파일을 통해 새 **MediaSource** 개체를 만들고 이를 **MediaPlayerElement** 개체의 [**Source**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Xaml.Controls.MediaPlayerElement.Source) 속성에 할당하여 비디오 파일을 컨트롤의 비디오 원본으로 만듭니다.

[!code-cs[OpenButtonClick](./code/MediaCasting_RS1/cs/MainPage.xaml.cs#SnippetOpenButtonClick)]

**MediaPlayerElement**에서 비디오가 로드된 후, 사용자는 전송 컨트롤에서 캐스팅 단추를 누르기만 하면 로드된 미디어를 캐스팅할 대상 디바이스를 선택할 수 있는 기본 제공 대화 상자를 시작할 수 있습니다.

![MediaElement 캐스팅 단추](images/media-element-casting-button.png)

> [!NOTE] 
> Windows 10 버전 1607부터는 **MediaPlayer** 클래스를 사용하여 미디어 항목을 재생하는 것이 좋습니다. **MediaPlayerElement**는 XAML 페이지의 **MediaPlayer** 콘텐츠를 렌더링하는 데 사용되는 간단한 XAML 컨트롤입니다. **MediaElement** 컨트롤은 이전 버전과의 호환성을 위해 계속 지원됩니다. **MediaPlayer** 및 **MediaPlayerElement**를 사용하여 미디어 콘텐츠를 재생하는 방법은 [MediaPlayer를 사용하여 오디오 및 비디오 재생](play-audio-and-video-with-mediaplayer.md)을 참조하세요. **MediaSource** 및 관련 API를 미디어 콘텐츠와 함께 사용하는 방법은 [미디어 항목, 재생 목록 및 트랙](media-playback-with-mediasource.md)을 참조하세요.

## <a name="media-casting-with-the-castingdevicepicker"></a>CastingDevicePicker를 사용하여 미디어 캐스팅

디바이스에 미디어를 캐스팅하는 두 번째 방법은 [**CastingDevicePicker**](https://msdn.microsoft.com/library/windows/apps/dn972525)를 사용하는 것입니다. 이 클래스를 사용하려면 프로젝트에 [**Windows.Media.Casting**](https://msdn.microsoft.com/library/windows/apps/dn972568) 네임스페이스를 포함합니다.

[!code-cs[CastingNamespace](./code/MediaCasting_RS1/cs/MainPage.xaml.cs#SnippetCastingNamespace)]

**CastingDevicePicker** 개체에 대한 멤버 변수를 선언합니다.

[!code-cs[DeclareCastingPicker](./code/MediaCasting_RS1/cs/MainPage.xaml.cs#SnippetDeclareCastingPicker)]

페이지가 초기화되면 캐스팅 선택기의 새 인스턴스를 만들고 [**Filter**](https://msdn.microsoft.com/library/windows/apps/dn972540)를 선택기에 나열된 캐스팅 디바이스가 비디오를 지원한다는 것을 나타내는 [**SupportsVideo**](https://msdn.microsoft.com/library/windows/apps/dn972526) 속성으로 설정합니다. [**CastingDeviceSelected**](https://msdn.microsoft.com/library/windows/apps/dn972539) 이벤트에 대한 처리기를 등록합니다. 이 이벤트는 사용자가 캐스팅을 위해 디바이스를 선택하면 발생합니다.

[!code-cs[InitCastingPicker](./code/MediaCasting_RS1/cs/MainPage.xaml.cs#SnippetInitCastingPicker)]

XAML 파일에 사용자가 선택기를 시작할 수 있게 하는 단추를 추가합니다.

[!code-xml[CastPickerButton](./code/MediaCasting_RS1/cs/MainPage.xaml#SnippetCastPickerButton)]

단추에 대한 **Click** 이벤트 처리기에서 [**TransformToVisual**](https://msdn.microsoft.com/library/windows/apps/br208986)을 호출하여 다른 요소에 대해 상대적인 UI 요소의 변환을 가져옵니다. 이 예제에서 변환은 응용 프로그램 창의 시각적 루트에 대해 상대적인 캐스트 선택기 단추의 위치입니다. [**CastingDevicePicker**](https://msdn.microsoft.com/library/windows/apps/dn972525) 개체의 [**Show**](https://msdn.microsoft.com/library/windows/apps/dn972542) 메서드를 호출하여 캐스팅 선택기 대화 상자를 시작합니다. 시스템이 사용자가 누른 단추에서 대화 상자를 펼칠 수 있도록 캐스트 선택기 단추의 크기와 위치를 지정합니다.

[!code-cs[CastPickerButtonClick](./code/MediaCasting_RS1/cs/MainPage.xaml.cs#SnippetCastPickerButtonClick)]

**CastingDeviceSelected** 이벤트 처리기에서 이벤트 인수 중 사용자가 선택한 캐스팅 디바이스를 나타내는 [**SelectedCastingDevice**](https://msdn.microsoft.com/library/windows/apps/dn972546) 속성의 [**CreateCastingConnection**](https://msdn.microsoft.com/library/windows/apps/dn972547) 메서드를 호출합니다. [**ErrorOccurred**](https://msdn.microsoft.com/library/windows/apps/dn972519) 및 [**StateChanged**](https://msdn.microsoft.com/library/windows/apps/dn972523) 이벤트에 대한 처리기를 등록합니다. 마지막으로 [**RequestStartCastingAsync**](https://msdn.microsoft.com/library/windows/apps/dn972520)를 호출하여 캐스팅을 시작하고 **MediaPlayerElement** 컨트롤의 **MediaPlayer** 개체에 대한 [**GetAsCastingSource**](https://msdn.microsoft.com/library/windows/apps/dn920012) 메서드로 결과를 전달하여 캐스팅할 미디어가 **MediaPlayerElement**와 연관된 **MediaPlayer**의 콘텐츠임을 지정합니다.

> [!NOTE] 
> UI 스레드에서 캐스팅 연결을 시작해야 합니다. **CastingDeviceSelected**는 UI 스레드에서 호출되지 않으므로 이 호출을 [**CoreDispatcher.RunAsync**](https://msdn.microsoft.com/library/windows/apps/hh750317) 호출 내에 배치해야 UI 스레드에서 호출될 수 있습니다.

[!code-cs[CastingDeviceSelected](./code/MediaCasting_RS1/cs/MainPage.xaml.cs#SnippetCastingDeviceSelected)]

**ErrorOccurred** 및 **StateChanged** 이벤트 처리기에서 UI를 업데이트하여 사용자에게 현재 캐스팅 상태에 대해 알려야 합니다. 이러한 이벤트에 대해서는 사용자 지정 캐스팅 디바이스 선택기 만들기에 대해 다루는 다음 섹션에서 자세히 설명합니다.

[!code-cs[EmptyStateHandlers](./code/MediaCasting_RS1/cs/MainPage.xaml.cs#SnippetEmptyStateHandlers)]

## <a name="media-casting-with-a-custom-device-picker"></a>사용자 지정 디바이스 선택기를 사용하여 미디어 캐스팅

다음 섹션에서는 캐스팅 디바이스를 열거하고 코드에서 연결을 시작하여 고유한 캐스팅 디바이스 선택기 UI를 만드는 방법을 설명합니다.

사용 가능한 캐스팅 디바이스를 열거하려면 프로젝트에 [**Windows.Devices.Enumeration**](https://msdn.microsoft.com/library/windows/apps/br225459) 네임스페이스를 포함합니다.

[!code-cs[EnumerationNamespace](./code/MediaCasting_RS1/cs/MainPage.xaml.cs#SnippetEnumerationNamespace)]

이 예제를 위한 기본적인 UI를 구현하기 위해 XAML 페이지에 다음 컨트롤을 추가합니다.

-   사용 가능한 캐스팅 디바이스를 찾는 디바이스 감시자를 시작하는 단추
-   캐스팅 열거가 진행 중이라는 피드백을 사용자에게 제공하는 [**ProgressRing**](https://msdn.microsoft.com/library/windows/apps/br227538) 컨트롤
-   검색된 캐스팅 디바이스를 나열하는 [**ListBox**](https://msdn.microsoft.com/library/windows/apps/br242868). 캐스팅 디바이스 개체를 컨트롤에 직접 할당하고 [**FriendlyName**](https://msdn.microsoft.com/library/windows/apps/dn972549) 속성을 계속 표시할 수 있도록 컨트롤에 대한 [**ItemTemplate**](https://msdn.microsoft.com/library/windows/apps/br242830)을 정의합니다.
-   사용자가 캐스팅 디바이스 연결을 끊을 수 있게 하는 단추.

[!code-xml[CustomPickerXAML](./code/MediaCasting_RS1/cs/MainPage.xaml#SnippetCustomPickerXAML)]

코드 숨김에서 [**DeviceWatcher**](https://msdn.microsoft.com/library/windows/apps/br225446) 및 [**CastingConnection**](https://msdn.microsoft.com/library/windows/apps/dn972510)에 대한 멤버 변수를 선언합니다.

[!code-cs[DeclareDeviceWatcher](./code/MediaCasting_RS1/cs/MainPage.xaml.cs#SnippetDeclareDeviceWatcher)]

*startWatcherButton*에 대한 **Click** 처리기에서 먼저, 단추를 사용하지 않도록 설정하고 디바이스 열거가 진행 중일 때 진행률 표시원이 활성화되도록 만들어 UI를 업데이트합니다. 캐스팅 디바이스 목록 상자를 지웁니다.

그런 다음, [**DeviceInformation.CreateWatcher**](https://msdn.microsoft.com/library/windows/apps/br225427)를 호출하여 디바이스 감시자를 만듭니다. 이 방법은 다양한 유형의 디바이스를 확인하는 데 사용 사용할 수 있습니다. 비디오 캐스팅을 지원하는 디바이스를 확인하려는 경우 [**CastingDevice.GetDeviceSelector**](https://msdn.microsoft.com/library/windows/apps/dn972551)에서 반환되는 디바이스 선택기 문자열을 사용하여 이를 지정합니다.

마지막으로, [**Added**](https://msdn.microsoft.com/library/windows/apps/br225450), [**Removed**](https://msdn.microsoft.com/library/windows/apps/br225453), [**EnumerationCompleted**](https://msdn.microsoft.com/library/windows/apps/br225451) 및 [**Stopped**](https://msdn.microsoft.com/library/windows/apps/br225457) 이벤트에 대한 이벤트 처리기를 등록합니다.

[!code-cs[StartWatcherButtonClick](./code/MediaCasting_RS1/cs/MainPage.xaml.cs#SnippetStartWatcherButtonClick)]

감시자에서 새 디바이스를 발견하면 **Added** 이벤트가 발생합니다. 이 이벤트에 대한 처리기에서 [**CastingDevice.FromIdAsync**](https://msdn.microsoft.com/library/windows/apps/dn972550)를 호출하고 검색된 캐스팅 디바이스의 ID(처리기에 전달된 **DeviceInformation** 개체에 포함됨)를 전달하여 새 [**CastingDevice**](https://msdn.microsoft.com/library/windows/apps/dn972524) 개체를 만듭니다.

사용자가 선택할 수 있도록 캐스팅 디바이스 **ListBox**에 **CastingDevice**를 추가합니다. XAML에 정의된 [**ItemTemplate**](https://msdn.microsoft.com/library/windows/apps/br242830)으로 인해 [**FriendlyName**](https://msdn.microsoft.com/library/windows/apps/dn972549) 속성은 목록 상자에서 해당 항목 텍스트로 사용됩니다. 이 이벤트 처리기는 UI 스레드에서 호출되지 않기 때문에 [**CoreDispatcher.RunAsync**](https://msdn.microsoft.com/library/windows/apps/hh750317) 호출 내에서 UI를 업데이트해야 합니다.

[!code-cs[WatcherAdded](./code/MediaCasting_RS1/cs/MainPage.xaml.cs#SnippetWatcherAdded)]

감시자가 캐스팅 디바이스가 더 이상 존재하지 않는다는 것을 검색하면 **Removed** 이벤트가 발생합니다. 처리기에 전달된 **Added** 개체의 ID 속성을 목록 상자 [**Items**](https://msdn.microsoft.com/library/windows/apps/br242823) 컬렉션에 나온 각 **Added**의 ID와 비교합니다. ID가 일치하는 경우 해당 개체를 컬렉션에서 제거합니다. 이번에도, UI가 업데이트되기 때문에 이 호출은 **RunAsync** 호출 내에서 수행해야 합니다.

[!code-cs[WatcherRemoved](./code/MediaCasting_RS1/cs/MainPage.xaml.cs#SnippetWatcherRemoved)]

감시자가 디바이스 검색을 마치면 **EnumerationCompleted** 이벤트가 발생합니다. 이 이벤트의 처리기에서 디바이스 열거가 완료되었음을 사용자가 알 수 있도록 UI를 업데이트하고 [**Stop**](https://msdn.microsoft.com/library/windows/apps/br225456)을 호출하여 디바이스 감시자를 중지합니다.

[!code-cs[WatcherEnumerationCompleted](./code/MediaCasting_RS1/cs/MainPage.xaml.cs#SnippetWatcherEnumerationCompleted)]

디바이스 감시자가 중지를 완료하면 Stopped 이벤트가 발생합니다. 이 이벤트에 대한 처리기에서 사용자가 디바이스 열거 프로세스를 다시 시작할 수 있도록 [**ProgressRing**](https://msdn.microsoft.com/library/windows/apps/br227538) 컨트롤을 중지하고 *startWatcherButton*을 다시 사용하도록 설정합니다.

[!code-cs[WatcherStopped](./code/MediaCasting_RS1/cs/MainPage.xaml.cs#SnippetWatcherStopped)]

사용자가 목록 상자에서 캐스팅 디바이스 중 하나를 선택하면 [**SelectionChanged**](https://msdn.microsoft.com/library/windows/apps/br209776) 이벤트가 발생합니다. 캐스팅 연결이 생성되고 캐스팅이 시작되는 것은 이 처리기 내에서 이루어집니다.

먼저, 디바이스 열거가 미디어 캐스팅을 방해하지 않도록 디바이스 감시자가 중지되었는지 확인합니다. 사용자가 선택한 **CastingDevice** 개체에 대해 [**CreateCastingConnection**](https://msdn.microsoft.com/library/windows/apps/dn972547)을 호출하여 캐스팅 연결을 만듭니다. [**StateChanged**](https://msdn.microsoft.com/library/windows/apps/dn972523) 및 [**ErrorOccurred**](https://msdn.microsoft.com/library/windows/apps/dn972519) 이벤트에 대한 이벤트 처리기를 추가합니다.

[**RequestStartCastingAsync**](https://msdn.microsoft.com/library/windows/apps/dn972520)를 호출하고 **MediaPlayer** 메서드 [**GetAsCastingSource**](https://msdn.microsoft.com/library/windows/apps/dn920012) 호출에서 반환되는 캐스팅 원본을 전달하여 미디어 캐스팅을 시작합니다. 마지막으로, 사용자가 미디어 캐스팅을 중지할 수 있도록 연결 끊기 단추를 표시합니다.

[!code-cs[SelectionChanged](./code/MediaCasting_RS1/cs/MainPage.xaml.cs#SnippetSelectionChanged)]

상태 변경 처리기에서 수행하는 작업은 캐스팅 연결의 새 상태에 따라 다릅니다.

-   상태가 **Connected** 또는 **Rendering**인 경우에는 **ProgressRing** 컨트롤이 비활성 상태이며 연결 끊기 단추가 표시되는지 확인합니다.
-   상태가 **Disconnected**인 경우에는 목록 상자에서 현재 캐스팅 디바이스를 선택 취소하고 **ProgressRing** 컨트롤을 비활성화하고 연결 끊기 단추를 숨깁니다.
-   상태가 **Connecting**인 경우에는 **ProgressRing** 컨트롤을 활성화하고 연결 끊기 단추를 숨깁니다.
-   상태가 **Disconnecting**인 경우에는 **ProgressRing** 컨트롤을 활성화하고 연결 끊기 단추를 숨깁니다.

[!code-cs[StateChanged](./code/MediaCasting_RS1/cs/MainPage.xaml.cs#SnippetStateChanged)]

**ErrorOccurred** 이벤트에 대한 처리기에서 사용자에게 캐스팅 오류가 발생했음을 알리고 사용자가 목록 상자에서 현재 **CastingDevice** 개체를 선택 취소할 수 있도록 UI를 업데이트합니다.

[!code-cs[ErrorOccurred](./code/MediaCasting_RS1/cs/MainPage.xaml.cs#SnippetErrorOccurred)]

마지막으로, 연결 끊기 단추에 대한 처리기를 구현합니다. 미디어 캐스팅을 중지하고 **CastingConnection** 개체의 [**DisconnectAsync**](https://msdn.microsoft.com/library/windows/apps/dn972518) 메서드를 호출하여 캐스팅 디바이스에서 연결을 끊습니다. 이 호출은 [**CoreDispatcher.RunAsync**](https://msdn.microsoft.com/library/windows/apps/hh750317) 호출을 통해 UI 스레드에 디스패치해야 합니다.

[!code-cs[DisconnectButton](./code/MediaCasting_RS1/cs/MainPage.xaml.cs#SnippetDisconnectButton)]

 

 




