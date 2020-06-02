---
description: 이 문서에서는 audio Playbackconnection을 사용 하 여 Bluetooth 연결 원격 장치가 로컬 컴퓨터에서 오디오를 재생할 수 있도록 하는 방법을 보여 줍니다.
title: 원격 Bluetooth 연결 디바이스에서 오디오 재생 사용
ms.date: 05/03/2020
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 3d4a4ab7664833308fe059e8bf07f68adea82b3e
ms.sourcegitcommit: cc645386b996f6e59f1ee27583dcd4310f8fb2a6
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 06/01/2020
ms.locfileid: "84262754"
---
# <a name="enable-audio-playback-from-remote-bluetooth-connected-devices"></a>원격 Bluetooth 연결 디바이스에서 오디오 재생 사용

이 문서에서는 audio [Playbackconnection](/uwp/api/windows.media.audio.audioplaybackconnection) 을 사용 하 여 Bluetooth 연결 원격 장치가 로컬 컴퓨터에서 오디오를 재생할 수 있도록 하는 방법을 보여 줍니다.

Windows 10부터 Windows 10 버전 2004 원격 오디오 원본은 Windows 장치에 오디오를 스트리밍할 수 있습니다 .이 경우 Bluetooth 스피커 처럼 작동 하도록 PC를 구성 하 고 사용자가 휴대폰에서 오디오를 소리로 볼 수 있습니다. 이 구현에서는 OS의 Bluetooth 구성 요소를 사용 하 여 들어오는 오디오 데이터를 처리 하 고 시스템의 시스템 오디오 끝점 (예: 기본 제공 PC 스피커 또는 유선 헤드폰)에서 재생 합니다. 기본 Bluetooth A2DP 싱크를 사용 하도록 설정 하는 것은 시스템이 아니라 최종 사용자 시나리오를 담당 하는 앱을 통해 관리 됩니다.

Audio [Playbackconnection](/uwp/api/windows.media.audio.audioplaybackconnection) 클래스는 원격 장치에서 연결을 사용 하거나 사용 하지 않도록 설정 하 고 원격 오디오 재생을 시작할 수 있도록 하는 데 사용 됩니다.

## <a name="add-a-user-interface"></a>사용자 인터페이스 추가

이 문서의 예제에서는 사용 가능한 원격 장치를 표시 하는 **ListView** 컨트롤을 정의 하는 다음과 같은 간단한 XAML UI, 연결 상태를 표시 하기 위한 **TextBlock** 및 연결을 사용 하거나 사용 하지 않도록 설정 하 고 여는 세 가지 단추를 사용 합니다.

:::code language="xml" source="~/../snippets-windows/windows-uwp/audio-video-camera/AudioPlaybackConnectionExample/cs/MainPage.xaml" id="snippet_AudioPlaybackConnectionXAML":::

## <a name="use-devicewatcher-to-monitor-for-remote-devices"></a>DeviceWatcher를 사용 하 여 원격 장치 모니터링

[Devicewatcher](/uwp/api/windows.devices.enumeration.devicewatcher) 클래스를 사용 하 여 연결 된 장치를 검색할 수 있습니다. [Audioplaybackconnection. GetDeviceSelector](/uwp/api/windows.media.audio.audioplaybackconnection.getdeviceselector) 메서드는 감시 할 장치의 종류를 장치 감시자에 게 알리는 문자열을 반환 합니다. **Devicewatcher** 생성자에이 문자열을 전달 합니다. 

장치 감시자를 시작할 때 연결 되는 장치 및 장치 감시자를 실행 하는 동안 연결 된 모든 장치에 대해 [Devicewatcher. 추가](/uwp/api/windows.devices.enumeration.devicewatcher.added) 된 이벤트가 발생 합니다. 이전에 연결 된 장치의 연결을 끊으면 [Devicewatcher. 제거](/uwp/api/windows.devices.enumeration.devicewatcher.removed) 된 이벤트가 발생 합니다. 

[Devicewatcher를 호출 합니다.](/uwp/api/windows.devices.enumeration.devicewatcher.start) 오디오 재생 연결을 지 원하는 연결 된 장치를 감시 하기 시작 합니다. 이 예제에서는 UI의 주 **그리드** 컨트롤이 로드 될 때 장치 관리자를 시작 합니다. **Devicewatcher**를 사용 하는 방법에 대 한 자세한 내용은 [장치 열거](/windows/uwp/devices-sensors/enumerate-devices)를 참조 하세요.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/AudioPlaybackConnectionExample/cs/MainPage.xaml.cs" id="snippet_MainGridLoaded":::


장치 감시자의 **추가** 된 이벤트에서 검색 된 각 장치는 [deviceinformation](/uwp/api/Windows.Devices.Enumeration.DeviceInformation) 개체로 표시 됩니다. 검색 된 각 장치를 UI의 **ListView** 컨트롤에 바인딩된 관찰 가능 컬렉션에 추가 합니다.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/AudioPlaybackConnectionExample/cs/MainPage.xaml.cs" id="snippet_DeclareDevices":::


:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/AudioPlaybackConnectionExample/cs/MainPage.xaml.cs" id="snippet_DeviceWatcher_Added":::


## <a name="enable-and-release-audio-playback-connections"></a>오디오 재생 연결 사용 및 해제

장치와의 연결을 열기 전에 연결을 사용 하도록 설정 해야 합니다. 그러면 원격 장치의 오디오를 PC에서 재생 하려는 새 응용 프로그램이 있음을 시스템에 알리고, 연결이 열리기 전까지 오디오가 재생을 시작 하지 않습니다 .이는 이후 단계에서 표시 됩니다.

**오디오 재생 연결 사용** 단추에 대 한 클릭 처리기에서 **ListView** 컨트롤에서 현재 선택한 장치와 연결 된 장치 ID를 가져옵니다. 이 예에서는 사용 하도록 설정 된 **오디오 연결** 개체의 사전을 유지 관리 합니다. 이 메서드는 먼저 선택한 장치에 대 한 사전에 항목이 이미 있는지 확인 합니다. 그런 다음,이 메서드는 [TryCreateFromId](/uwp/api/windows.media.audio.audioplaybackconnection.trycreatefromid) 을 호출 하 고 선택한 장치 ID를 전달 하 여 선택한 장치에 대 한 **오디오 연결** 을 만들려고 합니다. 

연결이 성공적으로 만들어지면 새 **오디오 Playbackconnection** 개체를 앱의 사전에 추가 하 고, 개체의 [StateChanged](/uwp/api/windows.media.audio.audioplaybackconnection.statechanged) 이벤트에 대 한 처리기를 등록 하 고,[StartAsync](/uwp/api/windows.media.audio.audioplaybackconnection.startasync) 를 호출 하 여 새 연결이 사용 하도록 설정 되었음을 시스템에 알립니다. 

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/AudioPlaybackConnectionExample/cs/MainPage.xaml.cs" id="snippet_DeclareConnections":::

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/AudioPlaybackConnectionExample/cs/MainPage.xaml.cs" id="snippet_EnableAudioPlaybackConnection":::


## <a name="open-the-audio-playback-connection"></a>오디오 재생 연결 열기

이전 단계에서 오디오 재생 연결이 만들어졌지만 [Open](/uwp/api/windows.media.audio.audioplaybackconnection.open) 또는 [openasync](/uwp/api/windows.media.audio.audioplaybackconnection.openasync)를 호출 하 여 연결을 열 때까지 소리 재생이 시작 되지 않습니다. **오디오 재생 연결 열기** 단추에서 처리기를 클릭 하 고 현재 선택한 장치를 가져온 다음 ID를 사용 하 여 앱의 연결 사전에서 **오디오 연결** 을 검색 합니다. **Openasync** 에 대 한 호출을 대기 하 [고 반환](/uwp/api/windows.media.audio.audioplaybackconnectionopenresult) **된 값을 확인 하 여 연결** 을 성공적으로 열었는지 확인 하 고 연결 상태 텍스트 상자를 업데이트 합니다.


:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/AudioPlaybackConnectionExample/cs/MainPage.xaml.cs" id="snippet_OpenAudioPlaybackConnectionButton":::

## <a name="monitor-audio-playback-connection-state"></a>오디오 재생 연결 상태 모니터링

연결 상태가 변경 될 때마다 [ConnectionStateChanged](/uwp/api/windows.media.audio.audioplaybackconnection.statechanged) 이벤트가 발생 합니다. 이 예제에서이 이벤트에 대 한 처리기는 상태 텍스트 상자를 업데이트 합니다. UI 스레드에서 업데이트를 수행 하려면 [디스패처. RunAsync](/uwp/api/windows.ui.core.coredispatcher.runasync) 호출 내에서 ui를 업데이트 해야 합니다.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/AudioPlaybackConnectionExample/cs/MainPage.xaml.cs" id="snippet_ConnectionStateChanged":::

## <a name="release-connections-and-handle-removed-devices"></a>연결 해제 및 제거 되는 장치 처리

이 예제에서는 사용자가 오디오 재생 연결을 해제할 수 있는 **릴리스 오디오 재생 연결** 단추를 제공 합니다. 이 이벤트에 대 한 처리기에서 현재 선택 된 장치를 가져오고 해당 장치 ID를 사용 하 여 사전에서 **오디오 및 오디오 연결** 을 조회 합니다. **Dispose** 를 호출 하 여 참조를 해제 하 고 연결 된 모든 리소스를 해제 하 고 사전에서 연결을 제거 합니다.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/AudioPlaybackConnectionExample/cs/MainPage.xaml.cs" id="snippet_ReleaseAudioPlaybackConnectionButton":::

연결이 설정 되거나 열려 있는 동안 장치가 제거 되는 경우를 처리 해야 합니다. 이렇게 하려면 장치 감시자의 Devicewatcher에 대 한 처리기를 구현 합니다 [. 제거](/uwp/api/windows.devices.enumeration.devicewatcher.removed) 된 이벤트입니다. 먼저 제거 된 장치의 ID를 사용 하 여 앱의 **ListView** 컨트롤에 바인딩된 관찰 가능한 컬렉션에서 장치를 제거 합니다. 그런 다음이 장치와 연결 된 연결이 앱의 사전에 있으면 **Dispose** 를 호출 하 여 연결 된 리소스를 해제 한 다음 사전에서 연결을 제거 합니다. 이 모든 작업은 ui 스레드에서 ui 업데이트가 수행 되도록 하기 위해 **디스패처. RunAsync** 호출 내에서 수행 됩니다.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/AudioPlaybackConnectionExample/cs/MainPage.xaml.cs" id="snippet_DeviceWatcher_Removed":::

## <a name="related-topics"></a>관련 항목

[미디어 재생](media-playback.md)


 




