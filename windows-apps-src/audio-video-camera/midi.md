---
ms.assetid: 9146212C-8480-4C16-B74C-D7F08C7086AF
description: 이 문서에서는 MIDI (음악 계기 디지털 인터페이스) 장치를 열거 하 고 유니버설 Windows 앱에서 MIDI 메시지를 보내고 받는 방법을 보여 줍니다.
title: MIDI
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 26026a0aadb52de27f98dea8a7c21bdc2d1c44f1
ms.sourcegitcommit: c3ca68e87eb06971826087af59adb33e490ce7da
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 09/02/2020
ms.locfileid: "89363856"
---
# <a name="midi"></a>MIDI



이 문서에서는 MIDI (음악 계기 디지털 인터페이스) 장치를 열거 하 고 유니버설 Windows 앱에서 MIDI 메시지를 보내고 받는 방법을 보여 줍니다. Windows 10은 USB (클래스 규격 및 대부분의 소유 드라이버), midi를 통한 midi (Windows 10 기념일 Edition 이상) 및 무료로 사용할 수 있는 타사 제품, MIDI over 이더넷 및 라우팅된 MIDI를 통해 midi를 지원 합니다.

## <a name="enumerate-midi-devices"></a>MIDI 장치 열거

MIDI 장치를 열거 하 고 사용 하기 전에 프로젝트에 다음 네임 스페이스를 추가 합니다.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/MIDIWin10/cs/MainPage.xaml.cs" id="SnippetUsing":::

사용자가 시스템에 연결 된 MIDI 입력 장치 중 하나를 선택할 수 있는 XAML 페이지에 [**ListBox**](/uwp/api/Windows.UI.Xaml.Controls.ListBox) 컨트롤을 추가 합니다. 다른 항목을 추가 하 여 MIDI 출력 장치를 나열 합니다.

:::code language="xml" source="~/../snippets-windows/windows-uwp/audio-video-camera/MIDIWin10/cs/MainPage.xaml" id="SnippetMidiListBoxes":::

[**FindAllAsync**](/uwp/api/windows.devices.enumeration.deviceinformation.findallasync) Method [**Deviceinformation**](/uwp/api/Windows.Devices.Enumeration.DeviceInformation) 클래스는 Windows에서 인식 되는 다양 한 종류의 장치를 열거 하는 데 사용 됩니다. 메서드만 MIDI 입력 장치를 찾도록 지정 하려면 [**MidiInPort**](/uwp/api/windows.devices.midi.midiinport.getdeviceselector)에서 반환 하는 선택기 문자열을 사용 합니다. **FindAllAsync** 는 시스템에 등록 된 각 MIDI 입력 장치에 대 한 **deviceinformation** 을 포함 하는 [**deviceinformationcollection**](/uwp/api/Windows.Devices.Enumeration.DeviceInformationCollection) 을 반환 합니다. 반환 된 컬렉션에 항목이 없는 경우에는 사용할 수 있는 MIDI 입력 장치가 없습니다. 컬렉션에 항목이 있는 경우 **Deviceinformation** 개체를 반복 하 고 각 장치의 이름을 MIDI 입력 장치 **목록 상자**에 추가 합니다.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/MIDIWin10/cs/MainPage.xaml.cs" id="SnippetEnumerateMidiInputDevices":::

MIDI 출력 장치를 열거 하는 것은 **FindAllAsync**를 호출할 때 [**Midioutport. getdeviceselector**](/uwp/api/windows.devices.midi.midioutport.getdeviceselector) 에서 반환 된 선택기 문자열을 지정 해야 한다는 점을 제외 하 고 입력 장치를 열거 하는 것과 정확 하 게 작동 합니다.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/MIDIWin10/cs/MainPage.xaml.cs" id="SnippetEnumerateMidiOutputDevices":::



## <a name="create-a-device-watcher-helper-class"></a>장치 감시자 도우미 클래스 만들기

[**Windows. Enumeration**](/uwp/api/Windows.Devices.Enumeration) 네임 스페이스는 장치를 시스템에서 추가 또는 제거 하는 경우 또는 장치에 대 한 정보가 업데이트 된 경우 앱에 알릴 수 있는 [**devicewatcher**](/uwp/api/Windows.Devices.Enumeration.DeviceWatcher) 를 제공 합니다. MIDI 지원 앱은 일반적으로 입력 및 출력 장치에 모두 관심이 있으므로이 예제에서는 **Devicewatcher** 패턴을 구현 하는 도우미 클래스를 만듭니다. 따라서 중복 없이도 midi 입력 및 midi 출력 장치 모두에 동일한 코드를 사용할 수 있습니다.

프로젝트에 새 클래스를 추가 하 여 장치 감시자 역할을 수행 합니다. 이 예제에서 클래스 이름은 **Mymididevicewatcher**입니다. 이 단원의 나머지 코드는 도우미 클래스를 구현 하는 데 사용 됩니다.

클래스에 멤버 변수를 추가 합니다.

-   장치 변경을 모니터링 하는 [**Devicewatcher**](/uwp/api/Windows.Devices.Enumeration.DeviceWatcher) 개체입니다.
-   한 인스턴스에 대해 포트 선택기 문자열에 MIDI를 포함 하 고 다른 인스턴스의 MIDI out 포트 선택기 문자열을 포함 하는 장치 선택기 문자열입니다.
-   사용 가능한 장치의 이름으로 채워질 [**ListBox**](/uwp/api/Windows.UI.Xaml.Controls.ListBox) 컨트롤입니다.
-   Ui 스레드가 아닌 스레드에서 UI를 업데이트 하는 데 필요한 [**CoreDispatcher**](/uwp/api/Windows.UI.Core.CoreDispatcher) 입니다.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/MIDIWin10/cs/MyMidiDeviceWatcher.cs" id="SnippetWatcherVariables":::

도우미 클래스 외부에서 현재 장치 목록에 액세스 하는 데 사용 되는 [**Deviceinformationcollection**](/uwp/api/Windows.Devices.Enumeration.DeviceInformationCollection) 속성을 추가 합니다.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/MIDIWin10/cs/MyMidiDeviceWatcher.cs" id="SnippetDeclareDeviceInformationCollection":::

클래스 생성자에서 호출자는 MIDI 장치 선택기 문자열, 장치를 나열 하는 목록 **상자** 및 UI를 업데이트 하는 데 필요한 **디스패처** 를 전달 합니다.

[**Deviceinformation을 호출 합니다. createwatcher**](/uwp/api/windows.devices.enumeration.deviceinformation.createwatcher) **클래스의** 새 인스턴스를 만들어 MIDI 장치 선택기 문자열을 전달 합니다.

감시자의 이벤트 처리기에 대 한 처리기를 등록 합니다.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/MIDIWin10/cs/MyMidiDeviceWatcher.cs" id="SnippetWatcherConstructor":::

**Devicewatcher** 에는 다음과 같은 이벤트가 있습니다.

-   새 장치를 시스템에 추가할 때 [**추가 됨**](/uwp/api/windows.devices.enumeration.devicewatcher.added) -발생 합니다.
-   [**제거 됨**](/uwp/api/windows.devices.enumeration.devicewatcher.removed) -장치가 시스템에서 제거 될 때 발생 합니다.
-   [**업데이트**](/uwp/api/windows.devices.enumeration.devicewatcher.updated) 됨-기존 장치와 연결 된 정보를 업데이트할 때 발생 합니다.
-   [**Enumerationcompleted**](/uwp/api/windows.devices.enumeration.devicewatcher.enumerationcompleted) -감시자가 요청 된 장치 유형의 열거를 완료 했을 때 발생 합니다.

이러한 각 이벤트에 대 한 이벤트 처리기에서 도우미 메서드 **Updatedevices**를 호출 하 여 현재 장치 목록으로 **ListBox** 를 업데이트 합니다. **Updatedevices** 는 ui 요소를 업데이트 하 고 이러한 이벤트 처리기는 ui 스레드에서 호출 되지 않으므로 각 호출은 [**runasync**](/uwp/api/windows.ui.core.coredispatcher.runasync)호출에서 래핑해야 합니다. 그러면 지정 된 코드가 ui 스레드에서 실행 됩니다.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/MIDIWin10/cs/MyMidiDeviceWatcher.cs" id="SnippetWatcherEventHandlers":::

**Updatedevices** 도우미 메서드는이 문서의 앞에서 설명한 대로 [**FindAllAsync**](/uwp/api/windows.devices.enumeration.deviceinformation.findallasync) 를 호출 하 고 반환 된 장치의 이름으로 **ListBox** 를 업데이트 합니다.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/MIDIWin10/cs/MyMidiDeviceWatcher.cs" id="SnippetWatcherUpdateDevices":::

**Devicewatcher** 개체의 [**시작**](/uwp/api/windows.devices.enumeration.devicewatcher.start) 메서드를 사용 하 여 감시자를 시작 하 고 [**stop**](/uwp/api/windows.devices.enumeration.devicewatcher.stop) 메서드를 사용 하 여 감시자를 중지 하는 메서드를 추가 합니다.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/MIDIWin10/cs/MyMidiDeviceWatcher.cs" id="SnippetWatcherStopStart":::

감시자 이벤트 처리기의 등록을 취소 하 고 장치 감시자를 null로 설정 하는 소멸자를 제공 합니다.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/MIDIWin10/cs/MyMidiDeviceWatcher.cs" id="SnippetWatcherDestructor":::

## <a name="create-midi-ports-to-send-and-receive-messages"></a>메시지를 보내고 받을 MIDI 포트 만들기

페이지에 대 한 코드 숨김으로 **Mymididevicewatcher** 도우미 클래스의 두 인스턴스를 포함 하도록 멤버 변수를 선언 합니다. 하나는 입력 장치이 고 하나는 출력 장치입니다.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/MIDIWin10/cs/MainPage.xaml.cs" id="SnippetDeclareDeviceWatchers":::

감시자 도우미 클래스의 새 인스턴스를 만들어 장치 선택기 문자열, 채울 **ListBox** 및 페이지의 **디스패처** 속성을 통해 액세스할 수 있는 **CoreDispatcher** 개체를 전달 합니다. 그런 다음 메서드를 호출 하 여 각 개체의 **Devicewatcher**를 시작 합니다.

각 **Devicewatcher** 가 시작 된 직후에는 시스템에 연결 된 현재 장치의 열거를 완료 하 고 [**enumerationcompleted**](/uwp/api/windows.devices.enumeration.devicewatcher.enumerationcompleted) 이벤트를 발생 시킵니다. 그러면 각 **ListBox** 가 현재 MIDI 장치로 업데이트 됩니다.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/MIDIWin10/cs/MainPage.xaml.cs" id="SnippetStartWatchers":::

사용자가 MIDI 입력 **목록 상자**에서 항목을 선택 하면 [**selectionchanged**](/uwp/api/windows.ui.xaml.controls.primitives.selector.selectionchanged) 이벤트가 발생 합니다. 이 이벤트에 대 한 처리기에서 도우미 클래스의 **Deviceinformationcollection** 속성에 액세스 하 여 현재 장치 목록을 가져옵니다. 목록에 항목이 있으면 **ListBox** 컨트롤의 [**SelectedIndex**](/uwp/api/windows.ui.xaml.controls.primitives.selector.selectedindex)에 해당 하는 인덱스를 사용 하 여 **deviceinformation** 개체를 선택 합니다.

선택한 장치의 [**Id**](/uwp/api/windows.devices.enumeration.deviceinformation.id) 속성을 전달 하 여 [**MidiInPort. FromIdAsync**](/uwp/api/windows.devices.midi.midiinport.fromidasync)를 호출 하 여 선택한 입력 장치를 나타내는 [**MidiInPort**](/uwp/api/Windows.Devices.Midi.MidiInPort) 개체를 만듭니다.

지정 된 장치를 통해 MIDI 메시지를 받을 때마다 발생 하는 [**MessageReceived**](/uwp/api/windows.devices.midi.midiinport.messagereceived) 이벤트에 대 한 처리기를 등록 합니다.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/MIDIWin10/cs/MainPage.xaml.cs" id="SnippetDeclareMidiPorts":::

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/MIDIWin10/cs/MainPage.xaml.cs" id="SnippetInPortSelectionChanged":::

**MessageReceived** 처리기가 호출 되 면 메시지는 **MidiMessageReceivedEventArgs**의 [**메시지**](/uwp/api/Windows.Devices.Midi.MidiMessageReceivedEventArgs) 속성에 포함 됩니다. 메시지 개체의 [**형식은**](/uwp/api/windows.devices.midi.imidimessage.type) 받은 메시지 유형을 나타내는 [**midimessagetype**](/uwp/api/Windows.Devices.Midi.MidiMessageType) 열거형의 값입니다. 메시지의 데이터는 메시지의 유형에 따라 달라 집니다. 이 예에서는 메시지가 메시지에 대 한 메모 인지 확인 하 고 메시지가 표시 되 면 midi 채널, 메모 및 메시지 속도를 출력 합니다.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/MIDIWin10/cs/MainPage.xaml.cs" id="SnippetMessageReceived":::

출력 장치 **목록 상자** 에 대 한 [**selectionchanged**](/uwp/api/windows.ui.xaml.controls.primitives.selector.selectionchanged) 처리기는 등록 된 이벤트 처리기를 제외 하 고 입력 장치에 대 한 처리기와 동일 하 게 작동 합니다.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/MIDIWin10/cs/MainPage.xaml.cs" id="SnippetOutPortSelectionChanged":::

출력 장치를 만든 후에는 보낼 메시지 유형에 대 한 새 [**IMidiMessage**](/uwp/api/Windows.Devices.Midi.IMidiMessage) 을 만들어 메시지를 보낼 수 있습니다. 이 예제에서 메시지는 [**메모 Onmessage**](/uwp/api/Windows.Devices.Midi.MidiNoteOnMessage)입니다. [**IMidiOutPort**](/uwp/api/Windows.Devices.Midi.IMidiOutPort) 개체의 [**SendMessage**](/uwp/api/windows.devices.midi.imidioutport.sendmessage) 메서드를 호출 하 여 메시지를 보냅니다.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/MIDIWin10/cs/MainPage.xaml.cs" id="SnippetSendMessage":::

앱이 비활성화 되 면 앱 리소스를 정리 해야 합니다. 이벤트 처리기의 등록을 취소 하 고 MIDI in port 및 out port 개체를 null로 설정 합니다. 감시자 장치를 중지 하 고이를 null로 설정 합니다.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/MIDIWin10/cs/MainPage.xaml.cs" id="SnippetCleanUp":::

## <a name="using-the-built-in-windows-general-midi-synth"></a>기본 제공 Windows 일반 MIDI 신시사이저 사용

위에 설명 된 기법을 사용 하 여 출력 MIDI 장치를 열거 하면 앱에서 "Microsoft GS Wavetable 신시사이저" 라는 MIDI 장치를 검색 합니다. 앱에서 재생할 수 있는 기본 제공 일반 MIDI 신시사이저입니다. 그러나 프로젝트에 기본 제공 신시사이저 용 SDK 확장을 포함 하지 않은 경우이 장치에 대 한 MIDI outport를 만들려는 시도가 실패 합니다.

**앱 프로젝트에 일반 MIDI 신시사이저 SDK 확장을 포함 하려면**

1.  **솔루션 탐색기**의 프로젝트 아래에서 **참조** 를 마우스 오른쪽 단추로 클릭 하 고 **참조 추가** ...를 선택 합니다.
2.  **유니버설 Windows** 노드를 확장 합니다.
3.  **확장**을 선택합니다.
4.  확장 목록에서 **유니버설 Windows 앱 용 Microsoft 일반 MIDI dl**을 선택 합니다.
    > [!NOTE] 
    > 여러 버전의 확장이 있는 경우 앱의 대상과 일치 하는 버전을 선택 해야 합니다. 프로젝트 속성의 **응용 프로그램** 탭에서 앱이 대상으로 하는 SDK 버전을 확인할 수 있습니다.

 

 
