---
ms.assetid: 9146212C-8480-4C16-B74C-D7F08C7086AF
description: 이 문서에서는 MIDI(Musical Instrument Digital Interface) 디바이스를 열거하고 유니버설 Windows 앱에서 MIDI 메시지를 보내고 받는 방법을 보여 줍니다.
title: MIDI
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 2737713030d68dbc19aaad3df767cea103b53f35
ms.sourcegitcommit: ac7f3422f8d83618f9b6b5615a37f8e5c115b3c4
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 05/29/2019
ms.locfileid: "66361603"
---
# <a name="midi"></a>MIDI



이 문서에서는 MIDI(Musical Instrument Digital Interface) 디바이스를 열거하고 유니버설 Windows 앱에서 MIDI 메시지를 보내고 받는 방법을 보여 줍니다. Windows 10 USB (클래스와 호환 하 고 가장 독점적인 드라이버), Bluetooth LE 통해 MIDI 통해 MIDI 지원 (Windows 10 Anniversary Edition 이상), 및 over Ethernet MIDI 및 라우트된 MIDI 자유롭게 사용할 수 있는 타사 제품을 통해.

## <a name="enumerate-midi-devices"></a>MIDI 장치 열거

MIDI 디바이스를 열거하고 사용하기 전에 먼저 프로젝트에 다음 네임스페이스를 추가합니다.

[!code-cs[Using](./code/MIDIWin10/cs/MainPage.xaml.cs#SnippetUsing)]

사용자가 시스템에 연결된 MIDI 입력 디바이스 중 하나를 선택할 수 있게 하는 [**ListBox**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ListBox) 컨트롤을 XAML 페이지에 추가합니다. MIDI 출력 디바이스를 나열하는 다른 컨트롤을 추가합니다.

[!code-xml[MidiListBoxes](./code/MIDIWin10/cs/MainPage.xaml#SnippetMidiListBoxes)]

[  **FindAllAsync**](https://docs.microsoft.com/uwp/api/windows.devices.enumeration.deviceinformation.findallasync) 메서드 [**DeviceInformation**](https://docs.microsoft.com/uwp/api/Windows.Devices.Enumeration.DeviceInformation) 클래스는 Windows에서 인식되는 다양한 유형의 디바이스를 열거하는 데 사용됩니다. 메서드가 MIDI 입력 디바이스만 찾도록 지정하려면 [**MidiInPort.GetDeviceSelector**](https://docs.microsoft.com/uwp/api/windows.devices.midi.midiinport.getdeviceselector)에서 반환되는 선택기 문자열을 사용합니다. **FindAllAsync**는 시스템에 등록된 각 MIDI 입력 디바이스에 대한 **DeviceInformation**이 포함된 [**DeviceInformationCollection**](https://docs.microsoft.com/uwp/api/Windows.Devices.Enumeration.DeviceInformationCollection)을 반환합니다. 반환된 컬렉션에 항목이 없다면 사용할 수 있는 MIDI 입력 디바이스가 없는 것입니다. 컬렉션에 항목이 있는 경우에는 **DeviceInformation** 개체를 반복하고 MIDI 입력 디바이스 **ListBox**에 각 디바이스의 이름을 추가합니다.

[!code-cs[EnumerateMidiInputDevices](./code/MIDIWin10/cs/MainPage.xaml.cs#SnippetEnumerateMidiInputDevices)]

MIDI 출력 디바이스 열거는 **FindAllAsync**를 호출할 때 [**MidiOutPort.GetDeviceSelector**](https://docs.microsoft.com/uwp/api/windows.devices.midi.midioutport.getdeviceselector)에서 반환된 선택기 문자열을 지정해야 한다는 점을 제외하고는 입력 디바이스 열거와 정확히 동일한 방식으로 작동합니다.

[!code-cs[EnumerateMidiOutputDevices](./code/MIDIWin10/cs/MainPage.xaml.cs#SnippetEnumerateMidiOutputDevices)]



## <a name="create-a-device-watcher-helper-class"></a>디바이스 감시자 도우미 클래스 만들기

[  **Windows.Devices.Enumeration**](https://docs.microsoft.com/uwp/api/Windows.Devices.Enumeration) 네임스페이스는 디바이스가 시스템에서 추가되거나 제거되는 경우 또는 디바이스에 대한 정보가 업데이트되는 경우 이를 앱에 알릴 수 있는 [**DeviceWatcher**](https://docs.microsoft.com/uwp/api/Windows.Devices.Enumeration.DeviceWatcher)를 제공합니다. MIDI 지원 앱은 일반적으로 입력 디바이스와 출력 디바이스 모두에 관심이 있으므로, 이 예제에서는 복제할 필요 없이 동일한 코드를 MIDI 입력 디바이스와 MIDI 출력 디바이스에 모두 사용할 수 있도록 **DeviceWatcher** 패턴을 구현하는 도우미 클래스를 만듭니다.

디바이스 감시자 역할을 할 새 클래스를 프로젝트에 추가합니다. 이 예제의 경우 **MyMidiDeviceWatcher**라는 클래스입니다. 이 섹션에 나온 코드의 나머지 부분은 도우미 클래스를 구현하는 데 사용됩니다.

클래스에 다음과 같은 멤버 변수를 추가합니다.

-   장치 변경 사항을 모니터링할 [**DeviceWatcher**](https://docs.microsoft.com/uwp/api/Windows.Devices.Enumeration.DeviceWatcher) 개체
-   한 인스턴스에 대한 MIDI 입력 포트 선택기 문자열과 다른 인스턴스에 대한 MIDI 출력 포트 선택기 문자열이 포함된 장치 선택기 문자열
-   사용 가능한 장치의 이름으로 채워질 [**ListBox**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ListBox) 컨트롤
-   UI 스레드가 아닌 스레드에서 UI를 업데이트하는 데 필요한 [**CoreDispatcher**](https://docs.microsoft.com/uwp/api/Windows.UI.Core.CoreDispatcher)

[!code-cs[WatcherVariables](./code/MIDIWin10/cs/MyMidiDeviceWatcher.cs#SnippetWatcherVariables)]

도우미 클래스 외부에서 현재 디바이스 목록에 액세스하는 데 사용되는 [**DeviceInformationCollection**](https://docs.microsoft.com/uwp/api/Windows.Devices.Enumeration.DeviceInformationCollection) 속성을 추가합니다.

[!code-cs[DeclareDeviceInformationCollection](./code/MIDIWin10/cs/MyMidiDeviceWatcher.cs#SnippetDeclareDeviceInformationCollection)]

클래스 생성자에서 호출자는 MIDI 디바이스 선택기 문자열, 디바이스를 나열하기 위한 **ListBox** 및 UI를 업데이트하는 데 필요한 **Dispatcher**를 전달합니다.

[  **DeviceInformation.CreateWatcher**](https://docs.microsoft.com/uwp/api/windows.devices.enumeration.deviceinformation.createwatcher)를 호출하여 **DeviceWatcher** 클래스의 새 인스턴스를 만들고 MIDI 디바이스 선택기 문자열을 전달합니다.

감시자의 이벤트 처리기에 대한 처리기를 등록합니다.

[!code-cs[WatcherConstructor](./code/MIDIWin10/cs/MyMidiDeviceWatcher.cs#SnippetWatcherConstructor)]

**DeviceWatcher**에는 다음과 같은 이벤트가 있습니다.

-   [**추가** ](https://docs.microsoft.com/uwp/api/windows.devices.enumeration.devicewatcher.added) -시스템에 새 장치가 추가 될 때 발생 합니다.
-   [**제거** ](https://docs.microsoft.com/uwp/api/windows.devices.enumeration.devicewatcher.removed) -장치를 시스템에서 제거 될 때 발생 합니다.
-   [**업데이트할** ](https://docs.microsoft.com/uwp/api/windows.devices.enumeration.devicewatcher.updated) -기존 장치를 사용 하 여 연결 정보를 업데이트 될 때 발생 합니다.
-   [**EnumerationCompleted** ](https://docs.microsoft.com/uwp/api/windows.devices.enumeration.devicewatcher.enumerationcompleted) -감시자 요청된 장치 유형의 열거 완료 되 면 발생 합니다.

이러한 각 이벤트에 대한 이벤트 처리기에서 도우미 메서드 **UpdateDevices**가 호출되어 **ListBox**를 현재 디바이스 목록으로 업데이트합니다. **UpdateDevices**는 UI 요소를 업데이트하며 이러한 이벤트 처리기는 UI 스레드에서 호출되지 않기 때문에, 각 호출은 지정된 코드를 UI 스레드에서 실행시키는 [**RunAsync**](https://docs.microsoft.com/uwp/api/windows.ui.core.coredispatcher.windows) 호출에 래핑해야 합니다.

[!code-cs[WatcherEventHandlers](./code/MIDIWin10/cs/MyMidiDeviceWatcher.cs#SnippetWatcherEventHandlers)]

**UpdateDevices** 도우미 메서드는 이 문서 앞부분에서 설명한 대로 [**DeviceInformation.FindAllAsync**](https://docs.microsoft.com/uwp/api/windows.devices.enumeration.deviceinformation.findallasync)를 호출하고 반환된 디바이스의 이름으로 **ListBox**를 업데이트합니다.

[!code-cs[WatcherUpdateDevices](./code/MIDIWin10/cs/MyMidiDeviceWatcher.cs#SnippetWatcherUpdateDevices)]

**DeviceWatcher** 개체의 [**Start**](https://docs.microsoft.com/uwp/api/windows.devices.enumeration.devicewatcher.start) 메서드를 사용하여 감시자를 시작하고 [**Stop**](https://docs.microsoft.com/uwp/api/windows.devices.enumeration.devicewatcher.stop) 메서드를 사용하여 감시자를 중지하는 메서드를 추가합니다.

[!code-cs[WatcherStopStart](./code/MIDIWin10/cs/MyMidiDeviceWatcher.cs#SnippetWatcherStopStart)]

감시자 이벤트 처리기를 등록 취소하고 디바이스 감시자를 null로 설정하는 소멸자를 제공합니다.

[!code-cs[WatcherDestructor](./code/MIDIWin10/cs/MyMidiDeviceWatcher.cs#SnippetWatcherDestructor)]

## <a name="create-midi-ports-to-send-and-receive-messages"></a>메시지를 보내고 받는 MIDI 포트 만들기

페이지에 대한 코드 숨김에서 **MyMidiDeviceWatcher** 도우미 클래스의 인스턴스 두 개(입력 디바이스용과 출력 디바이스용)를 보유하도록 멤버 변수를 선언합니다.

[!code-cs[DeclareDeviceWatchers](./code/MIDIWin10/cs/MainPage.xaml.cs#SnippetDeclareDeviceWatchers)]

감시자 도우미 클래스의 새 인스턴스를 만들고 디바이스 선택기 문자열, 채울 **ListBox** 및 페이지의 **Dispatcher** 속성을 통해 액세스할 수 있는 **CoreDispatcher** 개체를 전달합니다. 그런 다음, 각 개체의 **DeviceWatcher**를 시작하는 메서드를 호출합니다.

각 **DeviceWatcher**가 시작되면 곧바로 시스템에 연결된 현재 디바이스 열거가 완료되고 해당 [**EnumerationCompleted**](https://docs.microsoft.com/uwp/api/windows.devices.enumeration.devicewatcher.enumerationcompleted) 이벤트가 발생하며, 이에 따라 각 **ListBox**가 현재 MIDI 디바이스로 업데이트됩니다.

[!code-cs[StartWatchers](./code/MIDIWin10/cs/MainPage.xaml.cs#SnippetStartWatchers)]

사용자가 MIDI 입력 **ListBox**에서 항목을 선택하면 [**SelectionChanged**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.primitives.selector.selectionchanged) 이벤트가 발생합니다. 이 이벤트에 대한 처리기에서 도우미 클래스의 **DeviceInformationCollection** 속성에 액세스하여 현재 디바이스 목록을 가져옵니다. 선택 목록에 항목이 있는 경우 **ListBox** 컨트롤의 [**SelectedIndex**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.primitives.selector.selectedindex)와 일치하는 인덱스를 가진 **DeviceInformation** 개체를 선택합니다.

[  **MidiInPort.FromIdAsync**](https://docs.microsoft.com/uwp/api/windows.devices.midi.midiinport.fromidasync)를 호출하고 선택한 디바이스의 [**Id**](https://docs.microsoft.com/uwp/api/windows.devices.enumeration.deviceinformation.id) 속성을 전달하여 선택한 입력 디바이스를 나타내는 [**MidiInPort**](https://docs.microsoft.com/uwp/api/Windows.Devices.Midi.MidiInPort) 개체를 만듭니다.

지정된 디바이스를 통해 MIDI 메시지를 수신할 때마다 발생하는 [**MessageReceived**](https://docs.microsoft.com/uwp/api/windows.devices.midi.midiinport.messagereceived) 이벤트에 대한 처리기를 등록합니다.

[!code-cs[DeclareMidiPorts](./code/MIDIWin10/cs/MainPage.xaml.cs#SnippetDeclareMidiPorts)]

[!code-cs[InPortSelectionChanged](./code/MIDIWin10/cs/MainPage.xaml.cs#SnippetInPortSelectionChanged)]

**MessageReceived** 처리기가 호출되면 메시지는 **MidiMessageReceivedEventArgs**의 [**Message**](https://docs.microsoft.com/uwp/api/Windows.Devices.Midi.MidiMessageReceivedEventArgs) 속성에 포함됩니다. 메시지 개체의 [**Type**](https://docs.microsoft.com/uwp/api/windows.devices.midi.imidimessage.type)은 수신된 메시지의 유형을 나타내는 [**MidiMessageType**](https://docs.microsoft.com/uwp/api/Windows.Devices.Midi.MidiMessageType)의 값입니다. 메시지의 데이터는 메시지의 유형에 따라 다릅니다. 이 예제에서는 메시지가 메시지에 대한 노트인지 여부를 확인하고 그럴 경우 메시지의 MIDI 채널, 노트, 속도를 출력합니다.

[!code-cs[MessageReceived](./code/MIDIWin10/cs/MainPage.xaml.cs#SnippetMessageReceived)]

출력 디바이스 **ListBox**에 대한 [**SelectionChanged**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.primitives.selector.selectionchanged) 처리기는 이벤트 처리기가 등록되지 않는다는 점을 제외하고는 입력 디바이스에 대한 처리기와 동일하게 작동합니다.

[!code-cs[OutPortSelectionChanged](./code/MIDIWin10/cs/MainPage.xaml.cs#SnippetOutPortSelectionChanged)]

출력 디바이스가 만들어지면 보내려는 메시지 유형에 대한 새 [**IMidiMessage**](https://docs.microsoft.com/uwp/api/Windows.Devices.Midi.IMidiMessage)를 만들어 메시지를 보낼 수 있습니다. 이 예제에서 메시지는 [**NoteOnMessage**](https://docs.microsoft.com/uwp/api/Windows.Devices.Midi.MidiNoteOnMessage)입니다. [  **IMidiOutPort**](https://docs.microsoft.com/uwp/api/Windows.Devices.Midi.IMidiOutPort) 개체의 [**SendMessage**](https://docs.microsoft.com/uwp/api/windows.devices.midi.imidioutport.sendmessage) 메서드가 호출되어 메시지를 보냅니다.

[!code-cs[SendMessage](./code/MIDIWin10/cs/MainPage.xaml.cs#SnippetSendMessage)]

앱이 비활성화되면 앱 리소스를 정리해야 합니다. 이벤트 처리기를 등록 취소하고 MIDI 입력 포트 및 출력 포트 개체를 null로 설정합니다. 디바이스 감시자를 중지하고 null로 설정합니다.

[!code-cs[CleanUp](./code/MIDIWin10/cs/MainPage.xaml.cs#SnippetCleanUp)]

## <a name="using-the-built-in-windows-general-midi-synth"></a>기본 제공 Windows 일반 MIDI 신시사이저 사용

위에서 설명한 기술을 사용하여 출력 MIDI 디바이스를 열거하면 앱이 "Microsoft GS Wavetable Synth"라는 MIDI 디바이스를 검색합니다. 이는 앱에서 재생할 수 있는 기본 제공 일반 MIDI 신시사이저입니다. 그러나 기본 제공 신시사이저에 대한 SDK 확장을 프로젝트에 포함하지 않은 경우 이 디바이스에 MIDI 출력을 만들려고 하면 실패합니다.

**앱 프로젝트에서 일반적인 MIDI 신시사이저 SDK 확장을 포함 하도록**

1.  **솔루션 탐색기**에서 해당 프로젝트 아래에 있는 **참조**를 마우스 오른쪽 단추로 클릭하고 **참조 추가...** 를 선택합니다.
2.  **유니버설 Windows** 노드를 확장합니다.
3.  **확장**을 선택합니다.
4.  확장 목록에서 **유니버설 Windows 앱에 대한 Microsoft 일반 MIDI DLS**를 선택합니다.
    > [!NOTE] 
    > 여러 버전의 확장이 있는 경우 앱의 대상에 맞는 버전을 선택해야 합니다. 프로젝트 속성의 **응용 프로그램** 탭에서 앱이 대상으로 하는 SDK 버전을 확인할 수 있습니다.

 

 




