---
ms.assetid: CB924E17-C726-48E7-A445-364781F4CCA1
description: 이 문서에서는 Windows.Media.Audio 네임스페이스의 API를 사용하여 오디오 라우팅, 믹싱 및 처리 시나리오에 대한 오디오 그래프를 만드는 방법을 보여 줍니다.
title: 오디오 그래프
---

# 오디오 그래프

\[ Windows 10의 UWP 앱에 맞게 업데이트되었습니다. Windows 8.x 문서는 [보관](http://go.microsoft.com/fwlink/p/?linkid=619132)을 참조하세요. \]


이 문서에서는 [**Windows.Media.Audio**](https://msdn.microsoft.com/library/windows/apps/dn914341) 네임스페이스의 API를 사용하여 오디오 라우팅, 믹싱 및 처리 시나리오에 대한 오디오 그래프를 만드는 방법을 보여 줍니다.

오디오 그래프는 오디오 데이터가 흐르는 상호 연결된 오디오 노드 집합입니다. 오디오 입력 노드는 오디오 입력 장치, 오디오 파일 또는 사용자 지정 코드에서 그래프로 오디오 데이터를 제공합니다. 오디오 출력 노드는 그래프에 의해 처리되는 오디오의 대상입니다. 오디오는 그래프에서 오디오 출력 장치, 오디오 파일 또는 사용자 지정 코드로 라우팅될 수 있습니다. 노드의 마지막 유형은 하나 이상의 노드에서 오디오를 가져온 후 그래프의 다른 노드로 라우팅될 수 있는 단일 출력으로 결합하는 서브믹스 노드입니다. 모든 노드가 만들어지고 노드 간 연결이 설정된 후에는 오디오 그래프와 입력 노드로부터 서브믹스 노드를 통해 출력 노드로 진행되는 오디오 데이터 흐름을 시작하면 됩니다. 이 모델을 사용하면 장치의 마이크에서 오디오 파일로의 기록, 파일에서 장치의 스피커로 오디오 재생 또는 여러 소스의 오디오 혼합 등의 시나리오를 빠르고 쉽게 구현할 수 있습니다.

오디오 그래프에 오디오 효과를 추가하면 추가 시나리오도 가능해집니다. 오디오 그래프의 모든 노드는 노드를 통해 지나가는 오디오를 처리하는 0개 이상의 오디오 효과로 채울 수 있습니다. 단지 몇 줄의 코드로 오디오 노드에 연결될 수 있는 에코, 이퀄라이저, 제한, 반향 등의 몇 가지 기본 제공 효과가 있습니다. 기본 제공 효과와 정확히 동일하게 작동하는 고유한 사용자 지정 오디오 효과를 만들 수도 있습니다.

**참고**  
[AudioGraph UWP 샘플](http://go.microsoft.com/fwlink/?LinkId=619481)은 이 개요에서 설명한 코드를 구현합니다. 샘플을 다운로드하여 상황에 따른 코드를 참조하거나 자체 앱을 처음 빌드하기 시작할 때 사용할 수 있습니다.

## Windows 런타임 AudioGraph 또는 XAudio2 선택

Windows 런타임 오디오 그래프 API는 COM 기반 [XAudio2 API](https://msdn.microsoft.com/library/windows/desktop/hh405049)를 통해서도 구현될 수 있는 기능을 제공합니다. 다음은 XAudio2와는 다른 Windows 런타임 오디오 그래프 프레임워크의 기능입니다.

-   Windows 런타임 오디오 그래프 API는 XAudio2보다 훨씬 더 쉽게 사용할 수 있습니다.
-   Windows 런타임 오디오 그래프 API는 C++에 대해 지원될 뿐 아니라 C#에서도 사용할 수 있습니다.
-   Windows 런타임 오디오 그래프 API는 압축된 파일 형식을 포함하여 오디오 파일을 직접 사용할 수 있습니다. XAudio2는 오디오 버퍼에만 작동하며 파일 I/O 기능을 제공하지 않습니다.
-   Windows 런타임 오디오 그래프 API는 Windows 10의 짧은 대기 시간 오디오 파이프라인을 사용할 수 있습니다.
-   Windows 런타임 오디오 그래프 API는 기본 끝점 매개 변수를 사용할 때 자동 끝점 자동 전환을 지원합니다. 예를 들어 사용자가 장치의 스피커에서 헤드셋으로 전환하면 오디오는 새 입력으로 자동 리디렉션됩니다.

## AudioGraph 클래스

[
            **AudioGraph**](https://msdn.microsoft.com/library/windows/apps/dn914176) 클래스는 그래프를 구성하는 모든 노드의 부모입니다. 이 개체를 사용하여 모든 오디오 노드 유형의 인스턴스를 만들 수 있습니다. [
            **AudioGraphSettings**](https://msdn.microsoft.com/library/windows/apps/dn914185) 개체를 초기화하고, 그래프의 구성 설정을 포함한 후 [**AudioGraph.CreateAsync**](https://msdn.microsoft.com/library/windows/apps/dn914216)를 호출하여 **AudioGraph** 클래스의 인스턴스를 만듭니다. 반환된 [**CreateAudioGraphResult**](https://msdn.microsoft.com/library/windows/apps/dn914273)는 만들어진 오디오 그래프에 대한 액세스를 제공하고, 오디오 그래프 만들기가 실패하는 경우 오류 값을 제공합니다.

[!code-cs[DeclareAudioGraph](./code/AudioGraph/cs/MainPage.xaml.cs#SnippetDeclareAudioGraph)]

[!code-cs[InitAudioGraph](./code/AudioGraph/cs/MainPage.xaml.cs#SnippetInitAudioGraph)]

-   모든 오디오 노드 유형은 **AudioGraph** 클래스의 Create\* 메서드를 사용하여 만들어집니다.
-   [
            **AudioGraph.Start**](https://msdn.microsoft.com/library/windows/apps/dn914244) 메서드를 사용하면 오디오 그래프가 오디오 데이터를 처리하기 시작합니다. [
            **AudioGraph.Stop**](https://msdn.microsoft.com/library/windows/apps/dn914245) 메서드는 오디오 처리를 중지합니다. 그래프의 각 노드는 그래프가 실행되는 동안 독립적으로 시작 및 중지할 수 있지만 그래프가 중지되면 노드가 활성화되지 않습니다. [
            **ResetAllNodes**](https://msdn.microsoft.com/library/windows/apps/dn914242)를 사용하면 그래프의 모든 노드는 현재 오디오 버퍼에 있는 모든 데이터를 삭제합니다.
-   그래프가 오디오 데이터의 새 퀀텀 처리를 시작하면 [**QuantumStarted**](https://msdn.microsoft.com/library/windows/apps/dn914241) 이벤트가 발생합니다. 퀀텀 처리가 완료되면 [**QuantumProcessed**](https://msdn.microsoft.com/library/windows/apps/dn914240) 이벤트가 발생합니다.

-   필요한 유일한 [**AudioGraphSettings**](https://msdn.microsoft.com/library/windows/apps/dn914185) 속성은 [**AudioRenderCategory**](https://msdn.microsoft.com/library/windows/apps/dn297724)입니다. 이 값을 지정하면 시스템은 지정된 범주에 대해 오디오 파이프라인을 최적화할 수 있습니다.
-   오디오 그래프의 퀀텀 크기는 한 번에 처리되는 샘플 수를 결정합니다. 기본적으로 퀀텀 크기는 기본 샘플 속도에 따라 10밀리초입니다. [
            **DesiredSamplesPerQuantum**](https://msdn.microsoft.com/library/windows/apps/dn914205) 속성을 설정하여 사용자 지정 퀀텀 크기를 지정하는 경우 [**QuantumSizeSelectionMode**](https://msdn.microsoft.com/library/windows/apps/dn914208) 속성도 **ClosestToDesired**로 설정해야 합니다. 그러지 않으면 제공된 값이 무시됩니다. 이 값을 사용하는 경우 시스템은 사용자가 지정한 값에 최대한 가까운 퀀텀 크기를 선택합니다. 실제 퀀텀 크기를 확인하려면 만들어진 후 **AudioGraph**의 [**SamplesPerQuantum**](https://msdn.microsoft.com/library/windows/apps/dn914243)을 확인합니다.
-   오디오 그래프를 파일로만 사용하고 오디오 장치로 출력하지 않으려는 경우 [**DesiredSamplesPerQuantum**](https://msdn.microsoft.com/library/windows/apps/dn914205) 속성을 설정하지 말고 기본 퀀텀 크기를 사용하는 것이 좋습니다.
-   [
            **DesiredRenderDeviceAudioProcessing**](https://msdn.microsoft.com/library/windows/apps/dn958522) 속성은 장치가 오디오 그래프의 출력에 대해 수행하는 기본 렌더 처리량을 결정합니다. **Default** 설정을 사용하면 시스템은 통해 지정된 오디오 렌더 범주에 대해 기본 오디오 처리를 사용할 수 있습니다. 이러한 처리 방식을 사용하면 특히 작은 스피커가 달린 모바일 장치와 같은 일부 장치에서 오디오 사운드가 크게 향상될 수 있습니다. **Raw** 설정은 수행되는 신호 처리량을 최소화하여 성능을 향상시킬 수 있지만 일부 장치에서 사운드 품질이 저하될 수 있습니다.
-   [
            **QuantumSizeSelectionMode**](https://msdn.microsoft.com/library/windows/apps/dn914208)가 **LowestLatency**로 설정되면 오디오 그래프는 [**DesiredRenderDeviceAudioProcessing**](https://msdn.microsoft.com/library/windows/apps/dn958522)에 대해 자동으로 **Raw**를 사용합니다.
-   [
            **EncodingProperties**](https://msdn.microsoft.com/library/windows/apps/dn958523)는 그래프에서 사용되는 오디오 형식을 결정합니다. 32비트 부동 소수점 형식만이 지원됩니다.
-   [
            **PrimaryRenderDevice**](https://msdn.microsoft.com/library/windows/apps/dn958524)는 오디오 그래프에 대한 기본 렌더 장치를 설정합니다. 이를 설정하지 않으면 기본 시스템 장치가 사용됩니다. 기본 렌더 장치는 그래프의 다른 노드에 대한 퀀텀 크기를 계산하는 데 사용됩니다. 시스템에 오디오 렌더 장치가 없으면 오디오 그래프가 만들어지지 않습니다.

오디오 그래프에서 기본 오디오 렌더 디바이스를 사용하도록 하거나 [**Windows.Devices.Enumeration.DeviceInformation**](https://msdn.microsoft.com/library/windows/apps/br225393) 클래스를 통해 [**FindAllAsync**](https://msdn.microsoft.com/library/windows/apps/br225432)를 호출하고 [**Windows.Media.Devices.MediaDevice.GetAudioRenderSelector**](https://msdn.microsoft.com/library/windows/apps/br226817)에서 반환된 오디오 렌더 디바이스 선택기를 제공하여 시스템의 사용 가능한 오디오 렌더 디바이스 목록을 가져올 수 있습니다. 반환된 **DeviceInformation** 개체 중 하나를 프로그래밍 방식으로 선택하거나 사용자가 디바이스를 선택한 후 해당 디바이스를 사용하여 [**PrimaryRenderDevice**](https://msdn.microsoft.com/library/windows/apps/dn958524) 속성을 설정하도록 허용하는 UI를 표시할 수 있습니다.

[!code-cs[EnumerateAudioRenderDevices](./code/AudioGraph/cs/MainPage.xaml.cs#SnippetEnumerateAudioRenderDevices)]

##  디바이스 입력 노드

장치 입력 노드는 마이크와 같이 시스템에 연결된 오디오 캡처 장치에서 그래프로 오디오를 공급합니다. [
            **CreateDeviceInputNodeAsync**](https://msdn.microsoft.com/library/windows/apps/dn914218)를 호출하여 시스템의 기본 오디오 캡처 장치를 사용하는 [**DeviceInputNode**](https://msdn.microsoft.com/library/windows/apps/dn914082) 개체를 만듭니다. [
            **AudioRenderCategory**](https://msdn.microsoft.com/library/windows/apps/dn297724)를 제공하여 시스템에서 지정된 범주에 대해 오디오 파이프라인을 최적화할 수 있도록 합니다.

[!code-cs[DeclareDeviceInputNode](./code/AudioGraph/cs/MainPage.xaml.cs#SnippetDeclareDeviceInputNode)]


[!code-cs[CreateDeviceInputNode](./code/AudioGraph/cs/MainPage.xaml.cs#SnippetCreateDeviceInputNode)]

디바이스 입력 노드에 대해 특정 오디오 캡처 장치를 지정하려는 경우 [**Windows.Devices.Enumeration.DeviceInformation**](https://msdn.microsoft.com/library/windows/apps/br225393) 클래스를 통해 [**FindAllAsync**](https://msdn.microsoft.com/library/windows/apps/br225432)를 호출하고 [**Windows.Media.Devices.MediaDevice.GetAudioRenderSelector**](https://msdn.microsoft.com/library/windows/apps/br226817)에서 반환된 오디오 렌더 장치 선택기를 제공하여 시스템의 사용 가능한 오디오 캡처 장치 목록을 가져올 수 있습니다. 반환된 **DeviceInformation** 개체 중 하나를 프로그래밍 방식으로 선택하거나 사용자가 디바이스를 선택한 후 해당 디바이스를 [**CreateDeviceInputNodeAsync**](https://msdn.microsoft.com/library/windows/apps/dn914218)에 전달할 수 있도록 하는 UI를 표시할 수 있습니다.

[!code-cs[EnumerateAudioCaptureDevices](./code/AudioGraph/cs/MainPage.xaml.cs#SnippetEnumerateAudioCaptureDevices)]

##  장치 출력 노드

장치 출력 노드는 그래프의 오디오를 스피커 또는 헤드셋과 같은 오디오 렌더 장치로 밀어넣습니다. [
            **CreateDeviceOutputNodeAsync**](https://msdn.microsoft.com/library/windows/apps/dn958525)를 호출하여 [**DeviceOutputNode**](https://msdn.microsoft.com/library/windows/apps/dn914098)를 만듭니다. 출력 노드는 오디오 그래프의 [**PrimaryRenderDevice**](https://msdn.microsoft.com/library/windows/apps/dn958524)를 사용합니다.

[!code-cs[DeclareDeviceOutputNode](./code/AudioGraph/cs/MainPage.xaml.cs#SnippetDeclareDeviceOutputNode)]

[!code-cs[CreateDeviceOutputNode](./code/AudioGraph/cs/MainPage.xaml.cs#SnippetCreateDeviceOutputNode)]

##  파일 입력 노드

파일 입력 노드를 사용하여 오디오 파일의 데이터를 그래프에 공급할 수 있습니다. [
            **CreateFileInputNodeAsync**](https://msdn.microsoft.com/library/windows/apps/dn914226)를 호출하여 [**AudioFileInputNode**](https://msdn.microsoft.com/library/windows/apps/dn914108)를 만듭니다.

[!code-cs[DeclareFileInputNode](./code/AudioGraph/cs/MainPage.xaml.cs#SnippetDeclareFileInputNode)]


[!code-cs[CreateFileInputNode](./code/AudioGraph/cs/MainPage.xaml.cs#SnippetCreateFileInputNode)]

-   파일 입력 노드는 wav, mp3, wma, m4a 파일 형식을 지원합니다.
-   [
            **StartTime**](https://msdn.microsoft.com/library/windows/apps/dn914130) 속성을 설정하여 재생을 시작할 파일에 시간 오프셋을 지정합니다. 이 속성이 null이면 파일 시작 부분이 사용됩니다. [
            **EndTime**](https://msdn.microsoft.com/library/windows/apps/dn914118) 속성을 설정하여 재생을 끝낼 파일에 시간 오프셋을 지정합니다. 이 속성이 null이면 파일 끝 부분이 사용됩니다. 시작 시간 값은 종료 시간 값보다 작아야 하고 종료 시간 값은 [**Duration**](https://msdn.microsoft.com/library/windows/apps/dn914116) 속성 값을 확인하여 알 수 있는 오디오 파일의 기간보다 작거나 같아야 합니다.
-   [
            **Seek**](https://msdn.microsoft.com/library/windows/apps/dn914127)를 호출하고 재생 위치를 이동할 파일에 시간 오프셋을 지정하여 오디오 파일에서 특정 위치를 검색합니다. 지정된 값은 [**StartTime**](https://msdn.microsoft.com/library/windows/apps/dn914130) 및 [**EndTime**](https://msdn.microsoft.com/library/windows/apps/dn914118) 범위 내에 있어야 합니다. 읽기 전용 [**Position**](https://msdn.microsoft.com/library/windows/apps/dn914124) 속성으로 노드의 현재 재생 위치를 가져옵니다.
-   [
            **LoopCount**](https://msdn.microsoft.com/library/windows/apps/dn914120) 속성을 설정하여 오디오 파일의 루핑을 사용하도록 설정합니다. 이 값은 Null이 아닌 경우 초기 재생 이후에 파일을 재생할 횟수를 나타냅니다. 따라서 예를 들어 **LoopCount**를 1로 설정하면 파일은 총 2번 재생되고, 5로 설정하면 파일은 총 6번 재생됩니다. **LoopCount**를 null로 설정하면 파일이 무한 루핑됩니다. 루핑을 중지하려면 이 값을 0으로 설정합니다.
-   [
            **PlaybackSpeedFactor**](https://msdn.microsoft.com/library/windows/apps/dn914123)를 설정하여 오디오 파일이 재생되는 속도를 조정합니다. 이 값이 1이면 파일의 원래 속도를 나타내고 .5는 절반 속도이고, 2는 2배 속도입니다.

##  파일 출력 노드

파일 출력 노드를 사용하여 그래프의 오디오 데이터를 오디오 파일로 보낼 수 있습니다. [
            **CreateFileOutputNodeAsync**](https://msdn.microsoft.com/library/windows/apps/dn914227)를 호출하여 [**AudioFileOutputNode**](https://msdn.microsoft.com/library/windows/apps/dn914133)를 만듭니다.

[!code-cs[DeclareFileOutputNode](./code/AudioGraph/cs/MainPage.xaml.cs#SnippetDeclareFileOutputNode)]


[!code-cs[CreateFileOutputNode](./code/AudioGraph/cs/MainPage.xaml.cs#SnippetCreateFileOutputNode)]

-   파일 출력 노드는 wav, mp3, wma, m4a 파일 형식을 지원합니다.
-   [
            **AudioFileOutputNode.FinalizeAsync**](https://msdn.microsoft.com/library/windows/apps/dn914140)를 호출하기 전에 [**AudioFileOutputNode.Stop**](https://msdn.microsoft.com/library/windows/apps/dn914144)을 호출하여 노드 처리를 중지해야 합니다. 그러지 않으면 예외가 발생합니다.

##  오디오 프레임 입력 노드

오디오 프레임 입력 노드를 사용하면 자체 코드에서 생성하는 오디오 데이터를 오디오 그래프에 밀어넣을 수 있습니다. 이를 통해 주문형 소프트웨어 신시사이저를 만들 수 있습니다. [
            **CreateFrameInputNode**](https://msdn.microsoft.com/library/windows/apps/dn914230)를 호출하여 [**AudioFrameInputNode**](https://msdn.microsoft.com/library/windows/apps/dn914147)를 만듭니다.

[!code-cs[DeclareFrameInputNode](./code/AudioGraph/cs/MainPage.xaml.cs#SnippetDeclareFrameInputNode)]


[!code-cs[CreateFrameInputNode](./code/AudioGraph/cs/MainPage.xaml.cs#SnippetCreateFrameInputNode)]

오디오 그래프가 오디오 데이터의 다음 퀀텀을 처리할 준비가 되면 [**FrameInputNode.QuantumStarted**](https://msdn.microsoft.com/library/windows/apps/dn958507) 이벤트가 발생합니다. 처리기 내에서 이 이벤트로 사용자 생성 오디오 데이터를 제공합니다.

[!code-cs[QuantumStarted](./code/AudioGraph/cs/MainPage.xaml.cs#SnippetQuantumStarted)]

-   **QuantumStarted** 이벤트 처리기에 전달된 [**FrameInputNodeQuantumStartedEventArgs**](https://msdn.microsoft.com/library/windows/apps/dn958533) 개체는 오디오 그래프가 처리를 위해 퀀텀을 채워야 하는 샘플 수를 나타내는 [**RequiredSamples**](https://msdn.microsoft.com/library/windows/apps/dn958534) 속성을 노출합니다.
-   [
            **AudioFrameInputNode.AddFrame**](https://msdn.microsoft.com/library/windows/apps/dn914148)을 호출하여 오디오 데이터로 채워진 [**AudioFrame**](https://msdn.microsoft.com/library/windows/apps/dn930871) 개체를 그래프에 전달합니다.
-   **GenerateAudioData** 도우미 메서드의 구현 예제는 다음과 같습니다.

오디오 데이터로 [**AudioFrame**](https://msdn.microsoft.com/library/windows/apps/dn930871)을 채우려면 오디오 프레임의 기본 메모리 버퍼에 대한 액세스 권한을 얻어야 합니다. 이렇게 하려면 네임스페이스 내에 다음 코드를 추가하여 **IMemoryBufferByteAccess** COM 인터페이스를 초기화해야 합니다.

[!code-cs[ComImportIMemoryBufferByteAccess](./code/AudioGraph/cs/MainPage.xaml.cs#SnippetComImportIMemoryBufferByteAccess)]

다음 코드에서는 [**AudioFrame**](https://msdn.microsoft.com/library/windows/apps/dn930871)을 만든 후 오디오 데이터로 채우는 **GenerateAudioData** 도우미 메서드의 구현 예제를 보여 줍니다.

[!code-cs[GenerateAudioData](./code/AudioGraph/cs/MainPage.xaml.cs#SnippetGenerateAudioData)]

-   이 메서드는 Windows 런타임 형식의 기반이 되는 원시 버퍼에 액세스하므로 **unsafe** 키워드를 사용하여 선언해야 합니다. 또한, 안전하지 않은 코드의 컴파일을 허용하도록 Microsoft Visual Studio에서 프로젝트를 구성해야 합니다. 그러려면 프로젝트의 **속성** 페이지를 열고 **빌드** 속성 페이지를 클릭한 후 **안전하지 않은 코드 허용** 확인란을 선택합니다.
-   원하는 버퍼 크기를 생성자로 전달하여 **Windows.Media** 네임스페이스에서 [**AudioFrame**](https://msdn.microsoft.com/library/windows/apps/dn930871)의 새 인스턴스를 초기화합니다. 버퍼 크기는 샘플 수에 각 샘플의 크기를 곱한 값입니다.
-   [
            **LockBuffer**](https://msdn.microsoft.com/library/windows/apps/dn930878)를 호출하여 오디오 프레임의 [**AudioBuffer**](https://msdn.microsoft.com/library/windows/apps/dn958454)를 가져옵니다.
-   [
            **CreateReference**](https://msdn.microsoft.com/library/windows/apps/dn958457)를 호출하여 오디오 버퍼에서 [**IMemoryBufferByteAccess**](https://msdn.microsoft.com/library/windows/desktop/mt297505) COM 인터페이스의 인스턴스를 가져옵니다.
-   [
            **IMemoryBufferByteAccess.GetBuffer**](https://msdn.microsoft.com/library/windows/desktop/mt297506)를 호출한 후 오디오 데이터의 샘플 데이터 형식으로 캐스팅하여 원시 오디오 버퍼 데이터에 대한 포인터를 가져옵니다.
-   데이터로 버퍼를 채우고 오디오 그래프로 제출하기 위해 [**AudioFrame**](https://msdn.microsoft.com/library/windows/apps/dn930871)을 반환합니다.

##  오디오 프레임 출력 노드

오디오 프레임 출력 노드를 사용하여 오디오 그래프에서 오디오 데이터 출력을 받은 후 사용자가 만든 사용자 지정 코드로 처리할 수 있습니다. 이에 대한 예제 시나리오는 오디오 출력에 대해 신호 분석을 수행하는 것입니다. [
            **CreateFrameOutputNode**](https://msdn.microsoft.com/library/windows/apps/dn914233)를 호출하여 [**AudioFrameOutputNode**](https://msdn.microsoft.com/library/windows/apps/dn914166)를 만듭니다.

[!code-cs[DeclareFrameOutputNode](./code/AudioGraph/cs/MainPage.xaml.cs#SnippetDeclareFrameOutputNode)]

[!code-cs[CreateFrameOutputNode](./code/AudioGraph/cs/MainPage.xaml.cs#SnippetCreateFrameOutputNode)]

오디오 그래프가 오디오 데이터의 퀀텀 처리를 끝내면 [**AudioGraph.QuantumProcessed**](https://msdn.microsoft.com/library/windows/apps/dn914240) 이벤트가 발생합니다. 이 이벤트의 처리기 내에서 오디오 데이터에 액세스할 수 있습니다.

[!code-cs[QuantumProcessed](./code/AudioGraph/cs/MainPage.xaml.cs#SnippetQuantumProcessed)]

-   [
            **GetFrame**](https://msdn.microsoft.com/library/windows/apps/dn914171)을 호출하여 그래프의 오디오 데이터로 채워진 [**AudioFrame**](https://msdn.microsoft.com/library/windows/apps/dn930871) 개체를 가져옵니다.
-   **ProcessFrameOutput** 도우미 메서드의 구현 예제는 다음과 같습니다.

[!code-cs[ProcessFrameOutput](./code/AudioGraph/cs/MainPage.xaml.cs#SnippetProcessFrameOutput)]

-   위의 오디오 프레임 입력 노드 예제와 마찬가지로, **IMemoryBufferByteAccess** COM 인터페이스를 선언하고 안전하지 않은 코드를 허용하도록 프로젝트를 구성하여 기본 오디오 버퍼에 액세스할 수 있도록 해야 합니다.
-   [
            **LockBuffer**](https://msdn.microsoft.com/library/windows/apps/dn930878)를 호출하여 오디오 프레임의 [**AudioBuffer**](https://msdn.microsoft.com/library/windows/apps/dn958454)를 가져옵니다.
-   [
            **CreateReference**](https://msdn.microsoft.com/library/windows/apps/dn958457)를 호출하여 오디오 버퍼에서 **IMemoryBufferByteAccess** COM 인터페이스의 인스턴스를 가져옵니다.
-   **IMemoryBufferByteAccess.GetBuffer**를 호출한 후 오디오 데이터의 샘플 데이터 형식으로 캐스팅하여 원시 오디오 버퍼 데이터에 대한 포인터를 가져옵니다.

## 노드 연결 및 서브믹스 노드

모든 입력 노드 형식은 노드에 의해 생성된 오디오를 메서드로 전달되는 노드로 라우트하는 **AddOutgoingConnection** 메서드를 노출합니다. 다음 예제에서는 [**AudioFileInputNode**](https://msdn.microsoft.com/library/windows/apps/dn914108)를 [**AudioDeviceOutputNode**](https://msdn.microsoft.com/library/windows/apps/dn914098)에 연결합니다. 이것은 장치 스피커에서 오디오 파일을 재생하기 위한 간단한 설정입니다.

[!code-cs[AddOutgoingConnection1](./code/AudioGraph/cs/MainPage.xaml.cs#SnippetAddOutgoingConnection1)]

입력 노드에서 다른 노드로의 연결을 두 개 이상 만들 수 있습니다. 다음 예제에서는 [**AudioFileInputNode**](https://msdn.microsoft.com/library/windows/apps/dn914108)에서 [**AudioFileOutputNode**](https://msdn.microsoft.com/library/windows/apps/dn914133)로의 또 다른 연결을 추가합니다. 이제 오디오 파일의 오디오는 장치 스피커로 재생된 후 오디오 파일에도 쓰여집니다.

[!code-cs[AddOutgoingConnection2](./code/AudioGraph/cs/MainPage.xaml.cs#SnippetAddOutgoingConnection2)]

출력 노드는 다른 노드에서 둘 이상의 연결을 받을 수도 있습니다. 다음 예제에서는 [**AudioDeviceInputNode**](https://msdn.microsoft.com/library/windows/apps/dn914082)에서 [**AudioDeviceOutput**](https://msdn.microsoft.com/library/windows/apps/dn914098) 노드로 연결이 설정됩니다. 출력 노드는 파일 입력 노드와 디바이스 입력 노드에서 연결되므로 출력에는 두 소스의 오디오가 혼합되어 포함됩니다. **AddOutgoingConnection**은 연결을 통과하는 신호의 게인 값을 지정할 수 있는 오버로드를 제공합니다.

[!code-cs[AddOutgoingConnection3](./code/AudioGraph/cs/MainPage.xaml.cs#SnippetAddOutgoingConnection3)]

출력 노드는 여러 노드의 연결을 수락할 수 있지만 신호 믹스를 출력에 전달하기 전에 하나 이상의 노드에서 제공되는 신호의 중간 믹스를 만들려고 할 수 있습니다. 예를 들어 수준을 설정하거나 그래프의 오디오 신호 하위 집합에 효과를 적용하려고 할 수 있습니다. 이렇게 하려면 [**AudioSubmixNode**](https://msdn.microsoft.com/library/windows/apps/dn914247)를 사용합니다. 하나 이상의 입력 노드 또는 다른 서브믹스 노드에서 서브믹스 노드로 연결할 수 있습니다. 다음 예제에서는 [**AudioGraph.CreateSubmixNode**](https://msdn.microsoft.com/library/windows/apps/dn914236)를 사용하여 새 서브믹스 노드를 만듭니다. 그러면 파일 입력 노드 및 프레임 출력 노드에서 서브믹스 노드로의 연결이 추가됩니다. 마지막으로 서브믹스 노드가 파일 출력 노드에 연결됩니다.

[!code-cs[CreateSubmixNode](./code/AudioGraph/cs/MainPage.xaml.cs#SnippetCreateSubmixNode)]

## 오디오 그래프 노드 시작 및 중지

[
            **AudioGraph.Start**](https://msdn.microsoft.com/library/windows/apps/dn914244)가 호출되면 오디오 그래프는 오디오 데이터 처리를 시작합니다. 모든 노드 형식은 개별 노드가 데이터 처리를 시작 또는 중지하게 하는 **Start** 및 **Stop** 메서드를 제공합니다. [
            **AudioGraph.Stop**](https://msdn.microsoft.com/library/windows/apps/dn914245)이 호출되면 개별 노드의 상태에 관계 없이 모든 노드의 모든 오디오 처리가 중지되지만 오디오 그래프가 중지된 동안에도 각 노드의 상태를 설정할 수 있습니다. 예를 들어 그래프가 중지된 동안 개별 노드에 대해 **Stop**을 호출한 다음 **AudioGraph.Start**를 호출할 수 있습니다. 그러면 개별 노드는 중지된 상태를 유지합니다.

모든 노드 형식은 false로 설정된 경우 노드가 오디오를 계속 처리할 수 있도록 하지만 다른 노드에서 입력되는 오디오 데이터를 사용할 수 없게 만드는 **ConsumeInput** 속성을 노출합니다.

모든 노드 형식은 노드가 현재 해당 버퍼의 모든 오디오 데이터를 삭제하도록 만드는 **Reset** 메서드를 노출합니다.

## 오디오 효과 추가

오디오 그래프 API를 사용하면 그래프의 모든 유형의 노드에 오디오 효과를 추가할 수 있습니다. 출력 노드, 입력 노드 및 서브믹스 노드 각각은 하드웨어 기능에 의해서만 제한되는 제한 없는 수의 오디오 효과를 가질 수 있습니다. 다음 예제에서는 서브믹스 노드에 기본 제공 에코 효과를 추가하는 방법을 보여 줍니다.

[!code-cs[AddEffect](./code/AudioGraph/cs/MainPage.xaml.cs#SnippetAddEffect)]

-   모든 오디오 효과는 [**IAudioEffectDefinition**](https://msdn.microsoft.com/library/windows/apps/dn608044)을 구현합니다. 모든 노드는 해당 노드에 적용되는 효과 목록을 나타내는 **EffectDefinitions** 속성을 노출합니다. 목록에 해당 정의 개체를 추가하여 효과를 추가합니다.
-   **Windows.Media.Audio** 네임스페이스에 제공되는 여러 효과 정의 클래스가 있습니다. 다음이 포함됩니다.
    -   [**EchoEffectDefinition**](https://msdn.microsoft.com/library/windows/apps/dn914276)
    -   [**EqualizerEffectDefinition**](https://msdn.microsoft.com/library/windows/apps/dn914287)
    -   [**LimiterEffectDefinition**](https://msdn.microsoft.com/library/windows/apps/dn914306)
    -   [**ReverbEffectDefinition**](https://msdn.microsoft.com/library/windows/apps/dn914313)
-   [
            **IAudioEffectDefinition**](https://msdn.microsoft.com/library/windows/apps/dn608044)을 구현하고 오디오 그래프의 임의 노드에 적용하는 자체 오디오 효과를 만들 수 있습니다.
-   모든 노드 형식은 지정된 정의를 사용하여 추가된 노드의 **EffectDefinitions** 목록에 포함된 모든 효과를 사용하지 않도록 설정하는 **DisableEffectsByDefinition** 메서드를 노출합니다. **EnableEffectsByDefinition**은 지정된 정의를 갖는 효과를 사용하도록 설정합니다.

 

 






<!--HONumber=Mar16_HO1-->


