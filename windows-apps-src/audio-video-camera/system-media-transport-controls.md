---
author: drewbatgit
ms.assetid: EFCF84D0-2F4C-454D-97DA-249E9EAA806C
description: "SystemMediaTransportControls 클래스를 사용하면 Windows에서 기본 제공되는 앱에서 시스템 미디어 전송 컨트롤을 사용하고 앱이 현재 재생 중인 미디어에 대해 컨트롤이 표시하는 메타데이터를 업데이트할 수 있습니다."
title: "시스템 미디어 전송 컨트롤"
translationtype: Human Translation
ms.sourcegitcommit: 6530fa257ea3735453a97eb5d916524e750e62fc
ms.openlocfilehash: 5a94ce4112f7662d3fe9bf3c8a7d3f60b1569931

---

# 시스템 미디어 전송 컨트롤

\[ Windows 10의 UWP 앱에 맞게 업데이트되었습니다. Windows 8.x 문서는 [보관](http://go.microsoft.com/fwlink/p/?linkid=619132)을 참조하세요. \]


[**SystemMediaTransportControls**](https://msdn.microsoft.com/library/windows/apps/dn278677) 클래스를 사용하면 Windows에서 기본 제공되는 앱에서 시스템 미디어 전송 컨트롤을 사용하고 앱이 현재 재생 중인 미디어에 대해 컨트롤이 표시하는 메타데이터를 업데이트할 수 있습니다.

시스템 전송 컨트롤은 [**MediaElement**](https://msdn.microsoft.com/library/windows/apps/br242926) 개체의 전송 컨트롤과 다릅니다. 시스템 전송 컨트롤은 헤드폰 볼륨 컨트롤, 키보드의 미디어 단추 등 하드웨어 미디어 키를 누를 때 팝업되는 컨트롤입니다. 사용자가 키보드에서 Pause 키를 누를 경우 앱이 [**SystemMediaTransportControls**](https://msdn.microsoft.com/library/windows/apps/dn278677)를 지원하면 앱이 알림을 받고 적절한 조치를 취할 수 있습니다.

앱은 또한 [**SystemMediaTransportControls**](https://msdn.microsoft.com/library/windows/apps/dn278677)가 표시하는 곡 제목 및 미리 보기 이미지 등의 미디어 정보를 업데이트할 수 있습니다.

**참고**  
[시스템 미디어 컨트롤 UWP 샘플](http://go.microsoft.com/fwlink/?LinkId=619488)은 이 개요에 설명된 대로 코드를 구현합니다. 샘플을 다운로드하여 상황에 따른 코드를 참조하거나 자체 앱을 처음 빌드하기 시작할 때 사용할 수 있습니다.

## 전송 컨트롤 설정

페이지의 XAML 파일에서 시스템 미디어 전송 컨트롤로 [**MediaElement**](https://msdn.microsoft.com/library/windows/apps/br242926) 정의합니다. [**CurrentStateChanged**](https://msdn.microsoft.com/library/windows/apps/br227375) 및 [**MediaOpened**](https://msdn.microsoft.com/library/windows/apps/br227394) 이벤트는 시스템 미디어 전송 컨트롤을 업데이트하는 데 사용되며 이 문서의 뒷부분에서 설명합니다.

[!code-xml[MediaElementSystemMediaTransportControls](./code/SMTCWin10/cs/MainPage.xaml#SnippetMediaElementSystemMediaTransportControls)]

사용자가 재생할 파일을 선택할 수 있게 하는 단추를 XAML 파일에 추가합니다.

[!code-xml[OpenButton](./code/SMTCWin10/cs/MainPage.xaml#SnippetOpenButton)]

코드 숨김 페이지에서 다음 네임스페이스에 대한 using 지시문을 추가합니다.

[!code-cs[네임스페이스](./code/SMTCWin10/cs/MainPage.xaml.cs#SnippetNamespace)]

사용자가 파일을 선택할 수 있게 하는 [**FileOpenPicker**](https://msdn.microsoft.com/library/windows/apps/br207847)를 사용하는 단추 클릭 처리기를 추가한 다음 [**SetSource**](https://msdn.microsoft.com/library/windows/apps/br244338)를 호출하여 이 파일을 **MediaElement**에 대한 활성 파일로 만듭니다.

[!code-cs[OpenMediaFile](./code/SMTCWin10/cs/MainPage.xaml.cs#SnippetOpenMediaFile)]

[**GetForCurrentView**](https://msdn.microsoft.com/library/windows/apps/dn278708)를 호출하여 [**SystemMediaTransportControls**](https://msdn.microsoft.com/library/windows/apps/dn278677)의 인스턴스를 가져옵니다.

**SystemMediaTransportControls** 개체의 해당 "is enabled" 속성(예: [**IsPlayEnabled**](https://msdn.microsoft.com/library/windows/apps/dn278714), [**IsPauseEnabled**](https://msdn.microsoft.com/library/windows/apps/dn278713), [**IsNextEnabled**](https://msdn.microsoft.com/library/windows/apps/dn278712), [**IsPreviousEnabled**](https://msdn.microsoft.com/library/windows/apps/dn278715))을 설정하여 앱에서 처리할 단추를 사용하도록 설정합니다. 사용 가능한 컨트롤의 전체 목록을 보려면 **SystemMediaTransportControls** 참조 문서를 참조하세요.

사용자가 단추를 누를 때 알림을 받도록 [**ButtonPressed**](https://msdn.microsoft.com/library/windows/apps/dn278706)이벤트에 대한 처리기를 등록합니다.

[!code-cs[SystemMediaTransportControlsSetup](./code/SMTCWin10/cs/MainPage.xaml.cs#SnippetSystemMediaTransportControlsSetup)]

## 시스템 미디어 전송 컨트롤 단추 누르기 처리

설정된 단추 중 하나를 누르면 시스템 전송 컨트롤에 의해 [**ButtonPressed**](https://msdn.microsoft.com/library/windows/apps/dn278706) 이벤트가 발생합니다. 이벤트 처리기에 전달되는 [**SystemMediaTransportControlsButtonPressedEventArgs**](https://msdn.microsoft.com/library/windows/apps/dn278683)의 [**Button**](https://msdn.microsoft.com/library/windows/apps/dn278685) 속성은 사용 설정된 단추 중 어느 것을 눌렀는지를 나타내는 [**SystemMediaTransportControlsButton**](https://msdn.microsoft.com/library/windows/apps/dn278681) 열거의 멤버입니다.

[**ButtonPressed**](https://msdn.microsoft.com/library/windows/apps/dn278706) 이벤트 처리기에서 UI 스레드의 개체(예: [**MediaElement**](https://msdn.microsoft.com/library/windows/apps/br242926) 개체)를 업데이트하려면 [**CoreDispatcher**](https://msdn.microsoft.com/library/windows/apps/br208211)를 통해 호출을 마샬링해야 합니다. 이렇게 하는 이유는 **ButtonPressed** 이벤트 처리기가 UI 스레드에서 호출되지 않으므로 직접 UI를 수정하려고 하면 예외가 발생하기 때문입니다.

[!code-cs[SystemMediaTransportControlsButtonPressed](./code/SMTCWin10/cs/MainPage.xaml.cs#SnippetSystemMediaTransportControlsButtonPressed)]

## 현재 미디어 상태로 시스템 미디어 전송 컨트롤 업데이트

미디어의 상태가 변경되면 시스템이 현재 상태를 반영하여 컨트롤을 업데이트할 수 있도록 [**SystemMediaTransportControls**](https://msdn.microsoft.com/library/windows/apps/dn278677)에 알려야 합니다. 이렇게 하려면 미디어 상태가 변경될 때 발생하는 [**MediaElement**](https://msdn.microsoft.com/library/windows/apps/br242926)의 [**CurrentStateChanged**](https://msdn.microsoft.com/library/windows/apps/br227375) 이벤트 내에서 [**PlaybackStatus**](https://msdn.microsoft.com/library/windows/apps/dn278719) 속성을 적절한 [**MediaPlaybackStatus**](https://msdn.microsoft.com/library/windows/apps/dn278665) 값으로 설정합니다.

[!code-cs[SystemMediaTransportControlsStateChange](./code/SMTCWin10/cs/MainPage.xaml.cs#SnippetSystemMediaTransportControlsStateChange)]

## 미디어 정보 및 미리 보기로 시스템 미디어 전송 컨트롤 업데이트

[**SystemMediaTransportControlsDisplayUpdater**](https://msdn.microsoft.com/library/windows/apps/dn278686) 클래스를 사용하여 전송 컨트롤이 표시하는 미디어 정보(예: 현재 재생 중인 미디어 항목의 곡 제목 또는 앨범 이미지)를 업데이트할 수 있습니다. [**SystemMediaTransportControls.DisplayUpdater**](https://msdn.microsoft.com/library/windows/apps/dn278707) 속성을 사용하여 이 클래스의 인스턴스를 가져옵니다. 일반적인 시나리오의 경우 메타데이터를 전달하기 위해 권장되는 방법은 [**CopyFromFileAsync**](https://msdn.microsoft.com/library/windows/apps/dn278694)를 호출하고 현재 재생 중인 미디어 파일을 전달하는 것입니다. 디스플레이 업데이트 프로그램이 자동으로 파일에서 메타데이터 및 미리 보기 이미지를 추출합니다.

[**Update**](https://msdn.microsoft.com/library/windows/apps/dn278701)를 호출하여 시스템 미디어 전송 컨트롤이 UI를 새 메타데이터 및 미리 보기로 업데이트하도록 합니다.

[!code-cs[SystemMediaTransportControlsUpdater](./code/SMTCWin10/cs/MainPage.xaml.cs#SnippetSystemMediaTransportControlsUpdater)]

시나리오에 따라 필요한 경우, [**DisplayUpdater**](https://msdn.microsoft.com/library/windows/apps/dn278707) 클래스에 의해 노출되는 [**MusicProperties**](https://msdn.microsoft.com/library/windows/apps/dn278696), [**ImageProperties**](https://msdn.microsoft.com/library/windows/apps/dn278695) 또는 [**VideoProperties**](https://msdn.microsoft.com/library/windows/apps/dn278702) 개체의 값을 수동으로 설정하여 시스템 미디어 전송 컨트롤이 표시하는 메타데이터를 업데이트할 수 있습니다.

[!code-cs[SystemMediaTransportControlsUpdaterManual](./code/SMTCWin10/cs/MainPage.xaml.cs#SystemMediaTransportControlsUpdaterManual)]

## 시스템 미디어 전송 컨트롤 타임라인 속성 업데이트

시스템 전송 컨트롤은 현재 재생 중인 미디어 항목(예: 현재 재생 위치, 미디어 항목의 시작 시간과 종료 시간)의 타임라인에 대한 정보를 표시합니다. 시스템 전송 컨트롤 타임라인 속성을 업데이트하려면 새 [**SystemMediaTransportControlsTimelineProperties**](https://msdn.microsoft.com/library/windows/apps/mt218746) 개체를 만듭니다. 재생 중인 미디어 항목의 현재 상태를 반영하도록 개체의 속성을 설정합니다. [**SystemMediaTransportControls.UpdateTimelineProperties**](https://msdn.microsoft.com/library/windows/apps/mt218760)를 호출하여 컨트롤이 타임라인을 업데이트하도록 합니다.

[!code-cs[UpdateTimelineProperties](./code/SMTCWin10/cs/MainPage.xaml.cs#SnippetUpdateTimelineProperties)]

-   시스템 컨트롤이 재생 중인 항목의 타임라인을 표시하도록 하려면 [**StartTime**](https://msdn.microsoft.com/library/windows/apps/mt218751), [**EndTime**](https://msdn.microsoft.com/library/windows/apps/mt218747) 및 [**Position**](https://msdn.microsoft.com/library/windows/apps/mt218755)의 값을 제공해야 합니다.

-   [**MinSeekTime**](https://msdn.microsoft.com/library/windows/apps/mt218749) 및 [**MaxSeekTime**](https://msdn.microsoft.com/library/windows/apps/mt218748)을 사용하면 타임라인 내에서 사용자가 찾을 수 있는 범위를 지정할 수 있습니다. 이를 위한 일반적인 시나리오는 콘텐츠 공급자가 미디어에 광고 나누기를 포함할 수 있게 만드는 것입니다.

    [**PositionChangeRequest**](https://msdn.microsoft.com/library/windows/apps/mt218755)가 발생하도록 하려면 [**MinSeekTime**](https://msdn.microsoft.com/library/windows/apps/mt218749) 및 [**MaxSeekTime**](https://msdn.microsoft.com/library/windows/apps/mt218748)을 설정해야 합니다.

-   재생하는 동안 약 5초마다 한 번씩, 그리고 재생 상태가 변경(예: 일시 중지 또는 새 위치 찾기)될 때마다 이 속성을 업데이트하여 시스템 컨트롤을 미디어 재생과 동기화되도록 유지하는 것이 좋습니다.

## 플레이어 속성 변경에 응답

재생 중인 미디어 항목의 상태가 아니라 미디어 플레이어 자체의 현재 상태와 관련 있는 시스템 전송 컨트롤 속성 집합이 있습니다. 이러한 각 속성은 사용자가 관련 컨트롤을 조정할 때 발생하는 이벤트와 일치합니다. 이러한 속성 및 이벤트에는 다음이 포함됩니다.

| 속성                                                                  | 이벤트                                                                                                   |
|---------------------------------------------------------------------------|---------------------------------------------------------------------------------------------------------|
| [**AutoRepeatMode**](https://msdn.microsoft.com/library/windows/apps/mt218753) | [**AutoRepeatModeChangeRequested**](https://msdn.microsoft.com/library/windows/apps/mt218754) |
| [**PlaybackRate**](https://msdn.microsoft.com/library/windows/apps/mt218756)     | [**PlaybackRateChangeRequested**](https://msdn.microsoft.com/library/windows/apps/mt218757)     |
| [**ShuffleEnabled**](https://msdn.microsoft.com/library/windows/apps/mt218758) | [**ShuffleEnabledChangeRequested**](https://msdn.microsoft.com/library/windows/apps/mt218759) |

 
이 컨트롤 중 하나를 사용하여 사용자 조작을 처리하려면 먼저 관련 이벤트에 대한 처리기를 등록합니다.

[!code-cs[RegisterPlaybackChangedHandler](./code/SMTCWin10/cs/MainPage.xaml.cs#SnippetRegisterPlaybackChangedHandler)]

이벤트에 대한 처리기에서 먼저 요청된 값이 유효한 예상 범위 내에 있는지 확인합니다. 범위 내에 있는 경우 [**MediaElement**](https://msdn.microsoft.com/library/windows/apps/br242926)에 대해 해당 속성을 설정한 다음 [**SystemMediaTransportControls**](https://msdn.microsoft.com/library/windows/apps/dn278677) 개체에 대해 해당 속성을 설정합니다.

[!code-cs[PlaybackChangedHandler](./code/SMTCWin10/cs/MainPage.xaml.cs#SnippetPlaybackChangedHandler)]

-   이 플레이어 속성 이벤트 중 하나가 발생하도록 하려면 속성의 초기값을 설정해야 합니다. 예를 들어 [**PlaybackRateChangeRequested**](https://msdn.microsoft.com/library/windows/apps/mt218757)는 [**PlaybackRate**](https://msdn.microsoft.com/library/windows/apps/mt218756) 속성의 값을 한 번 이상 설정할 때까지는 발생하지 않습니다.

## 백그라운드 오디오에 대한 시스템 미디어 전송 컨트롤 사용

백그라운드 오디오에 대한 시스템 미디어 전송 컨트롤을 사용하려면 [**IsPlayEnabled**](https://msdn.microsoft.com/library/windows/apps/dn278714) 및 [**IsPauseEnabled**](https://msdn.microsoft.com/library/windows/apps/dn278713)를 true로 설정하여 재생 및 일시 중지 단추를 사용하도록 설정해야 합니다. 앱은 [**ButtonPressed**](https://msdn.microsoft.com/library/windows/apps/dn278706) 이벤트도 처리해야 합니다.

앱의 백그라운드 작업 내에서 [**SystemMediaTransportControls**](https://msdn.microsoft.com/library/windows/apps/dn278677)의 인스턴스를 가져오려면 [**GetForCurrentView**](https://msdn.microsoft.com/library/windows/apps/dn278708) 대신 [**BackgroundMediaPlayer.Current.SystemMediaTransportControls**](https://msdn.microsoft.com/library/windows/apps/dn926635)를 사용해야 합니다(포그라운드 앱 내에서만 사용할 수 있음).

백그라운드에서 오디오를 재생하는 방법에 대한 자세한 내용은 [백그라운드 오디오](background-audio.md)를 참조하세요.

 

 







<!--HONumber=Jun16_HO4-->


