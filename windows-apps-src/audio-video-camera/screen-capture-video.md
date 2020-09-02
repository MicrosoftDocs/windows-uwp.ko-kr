---
title: 비디오에 화면 캡처
description: 이 문서에서는 Windows. Graphics. Capture Api를 사용 하 여 화면에서 캡처한 프레임을 비디오 파일로 인코딩하는 방법을 설명 합니다.
ms.date: 07/28/2020
ms.topic: article
dev_langs:
- csharp
keywords: windows 10, uwp, 화면 캡처, 비디오
ms.localizationpriority: medium
ms.openlocfilehash: ae1eb68e480b4c9b4b4fc88452a68f39f8461a79
ms.sourcegitcommit: 14c0b1ea2447a81ddf31982b40e19a74ecc6d59e
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 09/02/2020
ms.locfileid: "89310091"
---
# <a name="screen-capture-to-video"></a>비디오에 화면 캡처

이 문서에서는 Windows. Graphics. Capture Api를 사용 하 여 화면에서 캡처한 프레임을 비디오 파일로 인코딩하는 방법을 설명 합니다. 화면 캡처에 대 한 자세한 내용은 [Screeen 캡처](screen-capture-video)를 참조 하세요. 

## <a name="overview-of-the-video-capture-process"></a>비디오 캡처 프로세스 개요
이 문서에서는 비디오 파일에 창의 콘텐츠를 기록 하는 예제 앱의 연습을 제공 합니다. 이 시나리오를 구현 하는 데 필요한 많은 코드가 있는 것 처럼 보일 수 있지만, 화면 레코더 앱의 상위 수준 구조는 매우 간단 합니다. 화면 캡처 프로세스에서는 세 가지 기본 UWP 기능을 사용 합니다.

- [GraphicsCapture](/uwp/api/windows.graphics.capture) api는 실제로 화면에서 픽셀을 가져오는 작업을 수행 합니다. [GraphicsCaptureItem](/uwp/api/windows.graphics.capture.graphicscaptureitem) 클래스는 캡처 중인 창이 나 표시를 나타냅니다. [GraphicsCaptureSession](/uwp/api/windows.graphics.capture.graphicscapturesession) 는 캡처 작업을 시작 하 고 중지 하는 데 사용 됩니다. [Direct3D11CaptureFramePool](/uwp/api/windows.graphics.capture.direct3d11captureframepool) 클래스는 화면 내용이 복사 되는 프레임의 버퍼를 유지 관리 합니다.
- [Mediastreamsource](/uwp/api/windows.media.core.mediastreamsource) 클래스는 캡처된 프레임을 받고 비디오 스트림을 생성 합니다.
- [MediaTranscoder](/uwp/api/windows.media.transcoding.mediatranscoder) 클래스는 **mediastreamsource** 에 의해 생성 된 스트림을 받아서 비디오 파일로 인코딩합니다.

이 문서에 표시 된 예제 코드는 몇 가지 다른 작업으로 분류 될 수 있습니다.

- **초기화** -여기에는 위에 설명 된 UWP 클래스를 구성 하 고, 그래픽 장치 인터페이스를 초기화 하 고, 캡처할 창을 선택 하 고, 해상도 및 프레임 속도와 같은 인코딩 매개 변수를 설정 하는 작업이 포함 됩니다.
- **이벤트 처리기 및 스레딩** -기본 캡처 루프의 기본 드라이버는 [SampleRequested](/uwp/api/windows.media.core.mediastreamsource.samplerequested) 이벤트를 통해 정기적으로 프레임을 요청 하는 **mediastreamsource** 입니다. 이 예제에서는 이벤트를 사용 하 여 예제의 여러 구성 요소 간의 새 프레임에 대 한 요청을 조정 합니다. 동기화는 프레임을 동시에 캡처하고 인코딩할 수 있도록 하는 데 중요 합니다.
- 프레임 **복사** 는 캡처 프레임 버퍼에서 **mediastreamsource** 에 전달 될 수 있는 별도의 Direct3D 화면으로 복사 되어 인코딩할 때 리소스를 덮어쓰지 않도록 합니다. Direct3D Api는이 복사 작업을 빠르게 수행 하는 데 사용 됩니다. 

### <a name="about-the-direct3d-apis"></a>Direct3D Api 정보
위에서 설명한 것 처럼 캡처된 각 프레임의 복사는이 문서에 표시 된 구현의 가장 복잡 한 부분입니다. 낮은 수준에서이 작업은 Direct3D를 사용 하 여 수행 됩니다. 이 예제에서는 [SharpDX](http://sharpdx.org/) 라이브러리를 사용 하 여 c #에서 Direct3D 작업을 수행 합니다. 이 라이브러리는 더 이상 공식적으로 지원 되지 않지만, 낮은 수준의 복사 작업에서 성능이이 시나리오에 적합 하기 때문에 선택 되었습니다. 이러한 작업을 위해 고유한 코드나 기타 라이브러리를 쉽게 대체할 수 있도록 Direct3D 작업을 가능한 한 불연속으로 유지 하려고 했습니다.

## <a name="setting-up-your-project"></a>프로젝트 설정
이 연습의 예제 코드는 Visual Studio 2019의 **비어 있는 앱 (유니버설 Windows)** c # 프로젝트 템플릿을 사용 하 여 만들었습니다. 앱에서 **Appxmanifest.xml api를** 사용 하려면 프로젝트의 파일에 **그래픽 캡처** 기능을 포함 해야 합니다. 이 예제에서는 생성 된 비디오 파일을 장치의 비디오 라이브러리에 저장 합니다. 이 폴더에 액세스 하려면 **비디오 라이브러리** 기능을 포함 해야 합니다.

SharpDX Nuget 패키지를 설치 하려면 Visual Studio에서 **Nuget 패키지 관리**를 선택 합니다. 찾아보기 탭에서 "SharpDX. Direct3D11" 패키지를 검색 하 고 **설치**를 클릭 합니다.

이 문서에 있는 코드 목록의 크기를 줄이기 위해 아래 연습의 코드에서는 명시적 네임 스페이스 참조와 선행 밑줄 "_"로 명명 된 MainPage 클래스 멤버 변수의 선언이 생략 됩니다.

## <a name="setup-for-encoding"></a>인코딩에 대 한 설정

이 섹션에 설명 된 **Setupencoding** 메서드는 비디오 프레임을 캡처하고 인코딩하고 캡처된 비디오에 대 한 인코딩 매개 변수를 설정 하는 데 사용할 몇 가지 주요 개체를 초기화 합니다. 이 메서드는 프로그래밍 방식으로 또는 단추 클릭과 같은 사용자 상호 작용에 대 한 응답으로 호출할 수 있습니다. **Setupencoding** 에 대 한 코드 목록은 초기화 단계에 대 한 설명 뒤에 표시 됩니다.

- **캡처 지원 확인** 캡처 프로세스를 시작 하기 전에 [GraphicsCaptureSession](/uwp/api/windows.graphics.capture.graphicscapturesession.issupported) 를 호출 하 여 화면 캡처 기능이 현재 장치에서 지원 되는지 확인 해야 합니다.

- **Direct3D 인터페이스를 초기화 합니다.** 이 샘플에서는 Direct3D를 사용 하 여 화면에서 캡처한 픽셀을 비디오 프레임으로 인코딩된 질감으로 복사 합니다. **CreateD3DDevice** 및 **CreateSharpDXDevice**Direct3D 인터페이스를 초기화 하는 데 사용 되는 도우미 메서드는이 문서의 뒷부분에 나와 있습니다.

- **GraphicsCaptureItem를 초기화 합니다.** [GraphicsCaptureItem](/uwp/api/windows.graphics.capture.graphicscaptureitem) 은 창이 나 전체 화면 중에서 캡처할 화면에 있는 항목을 나타냅니다. 사용자가 [GraphicsCapturePicker](/uwp/api/windows.graphics.capture.graphicscapturepicker) 을 만들고 [PickSingleItemAsync](/uwp/api/windows.graphics.capture.graphicscapturepicker.picksingleitemasync)를 호출 하 여 캡처할 항목을 선택할 수 있습니다.

- **컴퍼지션 질감을 만듭니다.** 각 비디오 프레임을 복사 하는 데 사용 되는 질감 리소스 및 연결 된 렌더링 대상 뷰를 만듭니다. **GraphicsCaptureItem** 을 만들고 해당 차원을 알고 있을 때까지이 질감을 만들 수 없습니다. 이 컴퍼지션 질감이 사용 되는 방법을 보려면 **Waitfornewframe** 의 설명을 참조 하세요. 이 질감을 만들기 위한 도우미 메서드는이 문서의 뒷부분에도 표시 됩니다.

- **MediaEncodingProfile 및 VideoStreamDescriptor를 만듭니다.** [Mediastreamsource](/uwp/api/windows.media.core.mediastreamsource) 클래스의 인스턴스는 화면에서 캡처된 이미지를 가져와 비디오 스트림으로 인코딩합니다. 그런 다음 [MediaTranscoder](/uwp/api/windows.media.transcoding.mediatranscoder) 클래스에 의해 비디오 스트림이 비디오 파일로 트랜스 코딩 됩니다. [VideoStreamDecriptor](/uwp/api/windows.media.core.videostreamdescriptor) 는 **mediastreamsource**에 대해 해상도 및 프레임 비율과 같은 인코딩 매개 변수를 제공 합니다. **MediaTranscoder** 에 대 한 비디오 파일 인코딩 매개 변수는 [MediaEncodingProfile](/uwp/api/Windows.Media.MediaProperties.MediaEncodingProfile)로 지정 됩니다. 비디오 인코딩에 사용 되는 크기는 캡처 중인 창의 크기와 동일할 필요는 없지만,이 예제를 단순하게 유지 하기 위해 인코딩 설정은 캡처 항목의 실제 차원을 사용 하도록 하드 코딩 됩니다.

- **MediaStreamSource 및 MediaTranscoder 개체를 만듭니다.** 위에서 설명한 것 처럼 **Mediastreamsource** 개체는 개별 프레임을 비디오 스트림으로 인코딩합니다. 이전 단계에서 만든 **MediaEncodingProfile** 를 전달 하 여이 클래스에 대 한 생성자를 호출 합니다. 버퍼 시간을 0으로 설정 하 고 [시작](uwp/api/windows.media.core.mediastreamsource.starting) 및 [SampleRequested](/uwp/api/windows.media.core.mediastreamsource.samplerequested) 이벤트에 대 한 처리기를 등록 합니다 .이 내용은이 문서의 뒷부분에 나와 있습니다. 그런 다음 **MediaTranscoder** 클래스의 새 인스턴스를 생성 하 고 하드웨어 가속을 사용 하도록 설정 합니다.

- **출력 파일 만들기** 이 메서드의 마지막 단계는 비디오를 트랜스 코딩 하는 파일을 만드는 것입니다. 이 예제에서는 장치의 비디오 라이브러리 폴더에 고유 하 게 명명 된 파일을 만듭니다. 이 폴더에 액세스 하기 위해 앱은 앱 매니페스트에서 "비디오 라이브러리" 기능을 지정 해야 합니다. 파일이 만들어진 후에는 읽기 및 쓰기를 위해 열고 결과 스트림을 다음에 표시 되는 **EncodeAsync** 메서드로 전달 합니다.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/ScreenRecorderExample/cs/MainPage.xaml.cs" id="snippet_SetupEncoding":::

## <a name="start-encoding"></a>인코딩 시작
이제 주 개체가 초기화 되었으므로 캡처 작업을 시작 하도록 **EncodeAsync** 메서드가 구현 됩니다. 이 메서드는 먼저 기록 되지 않았는지 확인 하 고, 그렇지 않은 경우 도우미 메서드 **Startcapture** 를 호출 하 여 화면에서 프레임 캡처를 시작 합니다. 이 방법은이 문서의 뒷부분에 나와 있습니다. 그런 다음, [PrepareMediaStreamSourceTranscodeAsync](/uwp/api/windows.media.transcoding.mediatranscoder.preparemediastreamsourcetranscodeasync) 를 호출 **하 여 이전** 섹션에서 만든 인코딩 프로필을 사용 하 여 **mediastreamsource** 개체에 의해 생성 된 비디오 스트림을 출력 파일 스트림으로 트랜스 코딩 합니다. 코드 변환기 준비 되 면 [TranscodeAsync](/uwp/api/windows.media.transcoding.preparetranscoderesult.transcodeasync) 를 호출 하 여 트랜스 코딩을 시작 합니다. **MediaTranscoder**사용에 대 한 자세한 내용은 [미디어 파일 트랜스 코드](/windows/uwp/audio-video-camera/transcode-media-files)를 참조 하세요.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/ScreenRecorderExample/cs/MainPage.xaml.cs" id="snippet_EncodeAsync":::

## <a name="handle-mediastreamsource-events"></a>MediaStreamSource 이벤트를 처리 합니다.
**Mediastreamsource** 개체는 화면에서 캡처한 프레임을 가져와 **MediaTranscoder**를 사용 하 여 파일에 저장할 수 있는 비디오 스트림으로 변환 합니다. 개체의 이벤트에 대 한 처리기를 통해 **Mediastreamsource** 에 프레임을 전달 합니다.

[SampleRequested](/uwp/api/windows.media.core.mediastreamsource.samplerequested) 이벤트는 **mediastreamsource** 가 새 비디오 프레임을 사용할 준비가 되었을 때 발생 합니다. 현재 기록 하 고 있는지 확인 한 후 도우미 메서드 **Waitfornewframe** 을 호출 하 여 화면에서 캡처된 새 프레임을 가져옵니다. 이 문서의 뒷부분에 나오는이 메서드는 캡처된 프레임을 포함 하는 [ID3D11Surface](/uwp/api/Windows.Graphics.DirectX.Direct3D11.IDirect3DSurface) 개체를 반환 합니다. 이 예제에서는 프레임을 캡처한 시스템 시간을 저장 하는 도우미 클래스에서 **IDirect3DSurface** 인터페이스를 래핑합니다. 프레임과 시스템 시간은 모두 [mediastreamsample. CreateFromDirect3D11Surface](/uwp/api/windows.media.core.mediastreamsample.createfromdirect3d11surface) factory 메서드에 전달 되 고 결과 [Mediastreamsample](/uwp/api/windows.media.core.mediastreamsample) 은 [MediaStreamSourceSampleRequestedEventArgs](/uwp/api/windows.media.core.mediastreamsourcesamplerequestedeventargs)의 [MediaStreamSourceSampleRequest](MediaStreamSourceSampleRequest.Sample) 속성으로 설정 됩니다. 이는 캡처된 프레임이 **Mediastreamsource**에 제공 되는 방법입니다.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/ScreenRecorderExample/cs/MainPage.xaml.cs" id="snippet_OnMediaStreamSourceSampleRequested":::

[시작](/uwp/api/windows.media.core.mediastreamsource.starting) 이벤트에 대 한 처리기에서 **waitfornewframe**을 호출 하지만 프레임을 캡처한 시스템 시간만 전달 합니다 .이 메서드는 **mediastreamsource** 에서 후속 프레임의 타이밍을 제대로 인코딩하는 데 사용 하는 [SetActualStartPosition MediaStreamSourceStartingRequest.](/uwp/api/windows.media.core.mediastreamsourcestartingrequest.setactualstartposition)

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/ScreenRecorderExample/cs/MainPage.xaml.cs" id="snippet_OnMediaStreamSourceStarting":::

## <a name="start-capturing"></a>캡처 시작
이 단계에 표시 된 **Startcapture** 메서드는 이전 단계에 표시 된 **EncodeAsync** 도우미 메서드에서 호출 됩니다. 첫째,이 메서드는 캡처 작업의 흐름을 제어 하는 데 사용 되는 이벤트 개체 집합을 초기화 합니다.

- **_multithread** 은 복사 되는 동안 다른 스레드가 SharpDX 질감에 액세스 하지 않도록 하는 데 사용 되는 SharpDX 라이브러리의 **다중 스레드** 개체를 래핑하는 도우미 클래스입니다.
- **_frameEvent** 는 새 프레임이 캡처되고 **mediastreamsource** 에 전달 될 수 있음을 알리는 데 사용 됩니다.
- **_closedEvent** 기록이 중지 되었으며 새 프레임을 기다릴 필요가 없다는 신호를 보냅니다.

프레임 이벤트와 닫힌 이벤트는 배열에 추가 되므로 캡처 루프에서 둘 중 하나를 기다릴 수 있습니다.

**Startcapture** 메서드의 나머지 부분에서는 실제 화면 캡처를 수행 하는 Windows Graphics. 캡처 api를 설정 합니다. 먼저 이벤트가 **CaptureItem** 이벤트에 등록 됩니다. 다음으로 한 번에 여러 개의 캡처된 프레임을 버퍼링 할 수 있는 [Direct3D11CaptureFramePool](/uwp/api/windows.graphics.capture.direct3d11captureframepool) 를 만듭니다. [Createfreethreaded](/uwp/api/windows.graphics.capture.direct3d11captureframepool.createfreethreaded) 메서드는 프레임 풀을 만드는 데 사용 되며, [FrameArrived](/uwp/api/windows.graphics.capture.direct3d11captureframepool.framearrived) 이벤트는 응용 프로그램의 주 스레드가 아니라 풀의 자체 작업자 스레드에서 호출 됩니다. 그런 다음 **FrameArrived** 이벤트에 대 한 처리기가 등록 됩니다. 마지막으로, 선택 된 **CaptureItem** 에 대 한 [GraphicsCaptureSession](/uwp/api/windows.graphics.capture.graphicscapturesession) 생성 되 고, 프레임 캡처는 [startcapture](/uwp/api/windows.graphics.capture.graphicscapturesession)를 호출 하 여 시작 됩니다.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/ScreenRecorderExample/cs/MainPage.xaml.cs" id="snippet_StartCapture":::

## <a name="handle-graphics-capture-events"></a>그래픽 캡처 이벤트 처리
이전 단계에서는 그래픽 캡처 이벤트에 대해 두 개의 처리기를 등록 하 고 캡처 루프의 흐름을 관리 하는 데 도움이 되는 몇 가지 이벤트를 설정 했습니다.

**FrameArrived** 이벤트는 **Direct3D11CaptureFramePool** 에 사용할 수 있는 새 캡처된 프레임이 있을 때 발생 합니다. 이 이벤트에 대 한 처리기에서 보낸 사람에 대해 [TryGetNextFrame](/uwp/api/windows.graphics.capture.direct3d11captureframepool.trygetnextframe) 를 호출 하 여 캡처된 다음 프레임을 가져옵니다. 프레임을 검색 한 후에는 캡처 루프에서 사용할 수 있는 새 프레임이 있음을 인식 하도록 **_frameEvent** 를 설정 합니다.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/ScreenRecorderExample/cs/MainPage.xaml.cs" id="snippet_OnFrameArrived":::

**Closed** 이벤트 처리기에서 캡처 루프가 중지할 시기를 알 수 있도록 **_closedEvent** 에 알립니다.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/ScreenRecorderExample/cs/MainPage.xaml.cs" id="snippet_OnClosed":::

## <a name="wait-for-new-frames"></a>새 프레임 대기
이 섹션에서 설명 하는 **Waitfornewframe** 도우미 메서드는 캡처 루프의 많은 부분을 발생 하는 위치입니다. 이 메서드는 **Mediastreamsource** 가 비디오 스트림에 새 프레임을 추가할 준비가 될 때마다 **OnMediaStreamSourceSampleRequested** 이벤트 처리기에서 호출 됩니다. 개략적인 수준에서이 함수는 새 프레임을 캡처하는 동안 인코딩에 대해 **Mediastreamsource** 에 전달할 수 있도록 각 화면 캡처 비디오 프레임을 하나의 Direct3D 화면에서 다른 Direct3D 화면으로 복사 하기만 합니다. 이 예제에서는 SharpDX 라이브러리를 사용 하 여 실제 복사 작업을 수행 합니다.

새 프레임을 대기 하기 전에 메서드는 클래스 변수에 저장 된 이전 프레임을 삭제 하 고 **_currentFrame**하 고 **_frameEvent**를 다시 설정 합니다. 그런 다음 메서드는 **_frameEvent** 또는 **_closedEvent** 신호를 받을 때까지 대기 합니다. Closed 이벤트가 설정 되 면 앱에서 도우미 메서드를 호출 하 여 캡처 리소스를 정리 합니다. 이 방법은이 문서의 뒷부분에 나와 있습니다.

Frame 이벤트가 설정 되 면 이전 단계에서 정의 된 **FrameArrived** 이벤트 처리기가 호출 되었음을 알 수 있으며, 캡처한 프레임 데이터를 **mediastreamsource**에 전달 되는 Direct3D 11 화면으로 복사 하는 프로세스를 시작 합니다.

이 예제에서는 도우미 클래스 **SurfaceWithInfo**를 사용 합니다 .이 클래스는 **mediastreamsource** 에 필요한 프레임의 비디오 프레임 및 시스템 시간을 단일 개체로 전달 하는 것만 허용 합니다. 프레임 복사 프로세스의 첫 번째 단계는이 클래스를 인스턴스화하고 시스템 시간을 설정 하는 것입니다.

다음 단계는 특히 SharpDX 라이브러리에 의존 하는이 예제의 일부입니다. 여기에 사용 된 도우미 함수는이 문서의 끝에 정의 되어 있습니다. 먼저 **Multithreadlock** 을 사용 하 여 복사를 수행 하는 동안 다른 스레드가 비디오 프레임 버퍼에 액세스 하지 않도록 합니다. 그런 다음 도우미 메서드 **CreateSharpDXTexture2D** 를 호출 하 여 비디오 프레임에서 SharpDX **Texture2D** 개체를 만듭니다. 복사 작업의 원본 질감이 됩니다. 

다음으로 이전 단계에서 만든 **Texture2D** 개체에서 이전에 프로세스에서 만든 컴퍼지션 질감으로 복사 합니다. 이 컴퍼지션 질감은 다음 프레임이 캡처될 때 인코딩 프로세스가 픽셀에 대해 작동할 수 있도록 스왑 버퍼 역할을 합니다. 복사를 수행 하기 위해 컴퍼지션 텍스처와 연결 된 렌더링 대상 뷰를 지운 다음,이 경우에는 복사 하려는 질감 내에서 영역을 정의 하 고 **CopySubresourceRegion** 를 호출 하 여 실제로 픽셀을 컴포지션 질감에 복사 합니다.

대상 텍스처를 만들 때 사용할 질감 설명의 복사본을 만들지만, 설명이 수정 되 면 새 질감에 쓰기 권한이 있도록 **Bindflags** 를 **rendertarget** 으로 설정 합니다. **CpuAccessFlags** 을 **None** 으로 설정 하면 시스템에서 복사 작업을 최적화할 수 있습니다. 질감 설명은 새 질감 리소스를 만드는 데 사용 되 고, 컴퍼지션 질감 리소스는 **copyresource**를 호출 하 여이 새 리소스에 복사 됩니다. 마지막으로이 메서드에서 반환 되는 **IDirect3DSurface** 개체를 만들기 위해 **CreateDirect3DSurfaceFromSharpDXTexture** 가 호출 됩니다.


:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/ScreenRecorderExample/cs/MainPage.xaml.cs" id="snippet_WaitForNewFrame":::

## <a name="stop-capture-and-clean-up-resources"></a>리소스 캡처 및 정리 중지
**Stop** 메서드는 캡처 작업을 중지 하는 방법을 제공 합니다. 앱은 단추 클릭과 같은 사용자 상호 작용에 대 한 응답으로 또는이를 프로그래밍 방식으로 호출할 수 있습니다. 이 메서드는 단순히 **_closedEvent**설정 합니다. 이전 단계에서 정의한 **Waitfornewframe** 메서드는이 이벤트를 검색 하 고 설정 된 경우 캡처 작업을 종료 합니다.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/ScreenRecorderExample/cs/MainPage.xaml.cs" id="snippet_Stop":::

**정리** 메서드는 복사 작업 중에 만들어진 리소스를 적절 하 게 삭제 하는 데 사용 됩니다. 다음 내용이 포함됩니다.

- 캡처 세션에서 사용 하는 **Direct3D11CaptureFramePool** 개체입니다.
- **GraphicsCaptureSession** 및 **GraphicsCaptureItem**
- Direct3D 및 SharpDX 장치
- 복사 작업에 사용 되는 SharpDX 텍스처 및 렌더링 대상 뷰입니다.
- 현재 프레임을 저장 하는 데 사용 되는 **Direct3D11CaptureFrame** 입니다.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/ScreenRecorderExample/cs/MainPage.xaml.cs" id="snippet_Cleanup":::

## <a name="helper-wrapper-classes"></a>도우미 래퍼 클래스
다음 도우미 클래스는이 문서의 예제 코드에 도움이 되도록 정의 되었습니다.

**Multithreadlock** 도우미 클래스는 다른 스레드가 복사 되는 동안 질감 리소스에 액세스 하지 않도록 하는 SharpDX **다중 스레드** 클래스를 래핑합니다.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/ScreenRecorderExample/cs/MainPage.xaml.cs" id="snippet_MultithreadLock":::

**SurfaceWithInfo** 은 캡처된 프레임 및 캡처된 시간을 나타내는 **SystemRelativeTime** 와 **IDirect3DSurface** 를 연결 하는 데 사용 됩니다.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/ScreenRecorderExample/cs/MainPage.xaml.cs" id="snippet_SurfaceWithInfo":::

## <a name="direct3d-and-sharpdx-helper-apis"></a>Direct3D 및 SharpDX helper Api
다음 도우미 Api는 Direct3D 및 SharpDX 리소스 만들기를 추상화 하도록 정의 되어 있습니다. 이러한 기술에 대 한 자세한 설명은이 문서의 범위를 벗어나지만 연습에 표시 된 예제 코드를 구현 하는 데 사용할 수 있는 코드를 여기에서 제공 합니다.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/ScreenRecorderExample/cs/Direct3D11Helpers.cs" id="snippet_Direct3D11Helpers":::

## <a name="see-also"></a>추가 정보

* [Windows. Graphics. Capture 네임 스페이스](https://docs.microsoft.com/uwp/api/windows.graphics.capture)
* [화면 캡처](screen-capture.md)