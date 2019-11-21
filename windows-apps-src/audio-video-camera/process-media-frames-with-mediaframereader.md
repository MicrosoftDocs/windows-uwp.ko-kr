---
ms.assetid: a128edc8-8a80-4645-ac29-908ede2d1c72
description: '이 문서에서는 MediaFrameReader와 MediaCapture를 사용하여 색, 깊이, 적외선 카메라, 오디오 장치, 사용자 지정 프레임 원본(예: 골격 추적 프레임을 생성하는 프레임 원본) 등 하나 이상의 사용 가능한 원본에서 미디어 프레임을 가져오는 방법을 보여 줍니다.'
title: MediaFrameReader를 사용하여 미디어 프레임 처리
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 2a13f0779414f60784ac1703fa32ac1ef5c89635
ms.sourcegitcommit: b52ddecccb9e68dbb71695af3078005a2eb78af1
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/20/2019
ms.locfileid: "74256547"
---
# <a name="process-media-frames-with-mediaframereader"></a>MediaFrameReader를 사용하여 미디어 프레임 처리

이 문서에서는 [**MediaFrameReader**](https://docs.microsoft.com/uwp/api/Windows.Media.Capture.Frames.MediaFrameReader)와 [**MediaCapture**](https://docs.microsoft.com/uwp/api/Windows.Media.Capture.MediaCapture)를 사용하여 색, 깊이, 적외선 카메라, 오디오 장치, 사용자 지정 프레임 원본(예: 골격 추적 프레임을 생성하는 프레임 원본) 등 하나 이상의 사용 가능한 원본에서 미디어 프레임을 가져오는 방법을 보여 줍니다. 이 기능은 증강 현실 및 깊이 인식 카메라 앱과 같이 미디어 프레임의 실시간 처리를 수행하는 앱에 사용되도록 설계되었습니다.

일반적인 사진 앱 등 단순히 비디오 또는 사진을 캡처하려면 [**MediaCapture**](https://docs.microsoft.com/uwp/api/Windows.Media.Capture.MediaCapture)에서 지원하는 다른 캡처 기술 중 하나를 사용할 수 있습니다. 미디어 캡처 기술 및 사용 방법을 보여 주는 문서 목록은 [**카메라**](camera.md)를 참조하세요.

> [!NOTE] 
> 이 문서에 설명된 기능은 Windows 10 버전 1607부터 사용할 수 있습니다.

> [!NOTE] 
> 유니버설 Windows 앱 샘플에서는 **MediaFrameReader**를 사용하여 색, 깊이, 적외선 카메라 등 다른 프레임 원본의 프레임을 표시하는 방법을 보여 줍니다. 자세한 내용은 [카메라 프레임 샘플](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/CameraFrames)을 참조하세요.

> [!NOTE] 
> 오디오 데이터가 포함된 **MediaFrameReader**를 사용하기 위한 새로운 API 세트가 Windows 10, 1803 버전에 도입되었습니다. 자세한 내용은 [MediaFrameReader를 사용하여 오디오 프레임 처리](process-audio-frames-with-mediaframereader.md)를 참조하세요.


## <a name="setting-up-your-project"></a>프로젝트 설정
**MediaCapture**를 사용하는 앱의 경우 카메라 장치에 액세스하기 전에 앱에서 *웹캠* 기능을 사용하도록 선언해야 합니다. 앱이 오디오 장치에서 캡처하는 경우 *마이크* 장치 기능도 선언해야 합니다. 

**Add capabilities to the app manifest**

1.  Microsoft Visual Studio의 **솔루션 탐색기**에서 **package.appxmanifest** 항목을 두 번 클릭하여 응용 프로그램 매니페스트 디자이너를 엽니다.
2.  **접근 권한 값** 탭을 선택합니다.
3.  **웹캠** 확인란과 **마이크** 상자를 선택합니다.
4.  사진과 비디오 라이브러리에 액세스하려면 **사진 라이브러리**의 확인란과 **비디오 라이브러리**의 확인란을 선택합니다.

이 문서의 예제 코드에서는 기본 프로젝트 템플릿에 포함된 API뿐만 아니라 다음 네임스페이스의 API도 사용합니다.

[!code-cs[FramesUsing](./code/Frames_Win10/Frames_Win10/MainPage.xaml.cs#SnippetFramesUsing)]

## <a name="select-frame-sources-and-frame-source-groups"></a>프레임 원본과 프레임 원본 그룹 선택
미디어 프레임을 처리하는 대부분의 앱은 장치의 색과 깊이 카메라 등 여러 원본의 프레임을 한 번에 가져와야 합니다. The [**MediaFrameSourceGroup**](https://docs.microsoft.com/uwp/api/Windows.Media.Capture.Frames.MediaFrameSourceGroup) object represents a set of media frame sources that can be used simultaneously. 정적 메서드 [**MediaFrameSourceGroup.FindAllAsync**](https://docs.microsoft.com/uwp/api/windows.media.capture.frames.mediaframesourcegroup.findallasync)를 호출하여 현재 장치에서 지원하는 모든 프레임 원본 그룹의 목록을 가져옵니다.

[!code-cs[FindAllAsync](./code/Frames_Win10/Frames_Win10/MainPage.xaml.cs#SnippetFindAllAsync)]

You can also create a [**DeviceWatcher**](https://docs.microsoft.com/uwp/api/Windows.Devices.Enumeration.DeviceWatcher) using [**DeviceInformation.CreateWatcher**](https://docs.microsoft.com/uwp/api/windows.devices.enumeration.deviceinformation.createwatcher) and the value returned from [**MediaFrameSourceGroup.GetDeviceSelector**](https://docs.microsoft.com/uwp/api/windows.media.capture.frames.mediaframesourcegroup.getdeviceselector) to receive notifications when the available frame source groups on the device changes, such as when an external camera is plugged in. 자세한 내용은 [**장치 열거**](https://docs.microsoft.com/windows/uwp/devices-sensors/enumerate-devices)를 참조하세요.

[  **MediaFrameSourceGroup**](https://docs.microsoft.com/uwp/api/Windows.Media.Capture.Frames.MediaFrameSourceGroup)에는 그룹에 포함된 프레임 원본을 설명하는 [**MediaFrameSourceInfo**](https://docs.microsoft.com/uwp/api/Windows.Media.Capture.Frames.MediaFrameSourceInfo) 개체 컬렉션이 있습니다. 장치에서 사용할 수 있는 프레임 원본 그룹을 검색한 후에는 원하는 프레임 원본을 노출하는 그룹을 선택할 수 있습니다.

다음 예제에서는 프레임 원본 그룹을 간단히 선택하는 방법을 보여 줍니다. 이 코드는 단순히 사용 가능한 모든 그룹을 반복한 다음 [**SourceInfos**](https://docs.microsoft.com/uwp/api/windows.media.capture.frames.mediaframesourcegroup.sourceinfos) 컬렉션 내에서 각 항목을 반복합니다. 각 **MediaFrameSourceInfo**에서 검색 중인 기능을 지원하는지 확인합니다. 이 경우 [**MediaStreamType**](https://docs.microsoft.com/uwp/api/windows.media.capture.frames.mediaframesourceinfo.mediastreamtype) 속성 값이 [**VideoPreview**](https://docs.microsoft.com/uwp/api/Windows.Media.Capture.MediaStreamType)(장치에서 비디오 미리 보기 스트림 제공)와 [**SourceKind**](https://docs.microsoft.com/uwp/api/windows.media.capture.frames.mediaframesourceinfo.sourcekind) 속성 값이 [**Color**](https://docs.microsoft.com/uwp/api/Windows.Media.Capture.Frames.MediaFrameSourceKind)(원본에서 색 프레임 제공)인지 확인합니다.

[!code-cs[SimpleSelect](./code/Frames_Win10/Frames_Win10/MainPage.xaml.cs#SnippetSimpleSelect)]

이 방법은 간단한 경우에는 원하는 프레임 원본 그룹과 프레임 원본을 식별할 수 있지만 더 복잡한 기준을 기반으로 프레임 원본을 선택하려는 경우에는 까다로울 수 있습니다. 또 다른 방법은 Linq 구문과 익명 개체를 사용하여 선택하는 것입니다. 다음 예제에서는 **Select** 확장 메서드를 사용하여 *frameSourceGroups* 목록의 **MediaFrameSourceGroup** 개체를 두 필드가 포함된 익명 개체로 변환합니다. 이 두 필드는 그룹 자체를 나타내는 *sourceGroup*과 그룹의 색 프레임 원본을 나타내는 *colorSourceInfo*입니다. *colorSourceInfo* 필드는 제공된 조건자가 true로 확인되는 첫 번째 개체를 선택하는 **FirstOrDefault**의 결과로 설정됩니다. 이 경우 스트림 형식이 **VideoPreview**이고, 원본 종류가 **Color**이며, 카메라가 장치의 앞면에 있으면 조건자가 true입니다.

위에서 설명한 쿼리에서 반환한 익명 개체 목록에서 **Where** 확장 메서드는 *colorSourceInfo* 필드가 null이 아닌 개체만 선택하는 데 사용됩니다. 마지막으로 **FirstOrDefault**가 목록의 첫 번째 항목을 선택하기 위해 호출됩니다.

이제 선택한 개체의 필드를 사용하여 선택한 **MediaFrameSourceGroup** 및 컬러 카메라를 나타내는 **MediaFrameSourceInfo** 개체에 대한 참조를 가져올 수 있습니다. 이러한 참조는 나중에 **MediaCapture** 개체를 초기화하고 선택한 원본에 대한 **MediaFrameReader**를 만드는 데 사용됩니다. 마지막으로 원본 그룹이 null인지, 즉 현재 장치에 요청된 캡처 원본이 있는지 테스트해야 합니다.

[!code-cs[SelectColor](./code/Frames_Win10/Frames_Win10/MainPage.xaml.cs#SnippetSelectColor)]

다음 예제에서는 위에서 설명한 대로 유사한 기술을 사용하여 색, 깊이 및 적외선 카메라를 포함하는 원본 그룹을 선택합니다.

[!code-cs[ColorInfraredDepth](./code/Frames_Win10/Frames_Win10/MainPage.xaml.cs#SnippetColorInfraredDepth)]

> [!NOTE]
> Windows 10 버전 1803부터 [**MediaCaptureVideoProfile**](https://docs.microsoft.com/uwp/api/Windows.Media.Capture.MediaCaptureVideoProfile) 클래스를 사용하여 원하는 기능 세트와 함께 미디어 프레임 원본을 선택할 수 있습니다. 자세한 내용은 이 문서의 후반부에 나오는 **비디오 프로필을 사용하여 프레임 원본 선택** 섹션을 참조하세요.


## <a name="initialize-the-mediacapture-object-to-use-the-selected-frame-source-group"></a>선택한 프레임 원본 그룹을 사용하기 위해 MediaCapture 개체 초기화
다음 단계는 이전 단계에서 선택한 프레임 원본 그룹을 사용하기 위해 **MediaCapture**를 초기화하는 것입니다.

**MediaCapture** 개체는 일반적으로 앱 내의 여러 위치에서 사용되므로 이 개체를 포함하는 클래스 멤버 변수를 선언해야 합니다.

[!code-cs[DeclareMediaCapture](./code/Frames_Win10/Frames_Win10/MainPage.xaml.cs#SnippetDeclareMediaCapture)]

생성자를 호출하여 **MediaCapture** 개체의 인스턴스를 만듭니다. Next, create a [**MediaCaptureInitializationSettings**](https://docs.microsoft.com/uwp/api/windows.media.capture.mediacaptureinitializationsettings) object that will be used to initialize the **MediaCapture** object. 이 예제에서는 다음 설정이 사용됩니다.

* [**SourceGroup**](https://docs.microsoft.com/uwp/api/windows.media.capture.mediacaptureinitializationsettings.sourcegroup) - This tells the system which source group you will be using to get frames. 원본 그룹은 동시에 사용할 수 있는 미디어 프레임 원본 집합을 정의합니다.
* [**SharingMode**](https://docs.microsoft.com/uwp/api/windows.media.capture.mediacaptureinitializationsettings.sharingmode) - This tells the system whether you need exclusive control over the capture source devices. 이 값을 [**ExclusiveControl**](https://docs.microsoft.com/uwp/api/Windows.Media.Capture.MediaCaptureSharingMode)로 설정하면 생성할 프레임 형식 등의 캡처 장치 설정을 변경할 수 있다는 의미입니다. 그러나 다른 앱에서 이미 단독으로 제어하는 경우 미디어 캡처 장치를 초기화하려고 하면 오류가 발생합니다. 이 값을 [**SharedReadOnly**](https://docs.microsoft.com/uwp/api/Windows.Media.Capture.MediaCaptureSharingMode)로 설정하면 다른 앱에서 사용되어도 프레임 원본의 프레임을 수신할 수 있지만 장치의 설정을 변경할 수는 없습니다.
* [**MemoryPreference**](https://docs.microsoft.com/uwp/api/windows.media.capture.mediacaptureinitializationsettings.memorypreference) - [**cpu**](https://docs.microsoft.com/uwp/api/Windows.Media.Capture.MediaCaptureMemoryPreference)를 지정 하는 경우 시스템은 cpu 메모리를 사용하여 프레임이 도착할 때 이를 보장 하는 [**SoftwareBitmap**](https://docs.microsoft.com/uwp/api/Windows.Graphics.Imaging.SoftwareBitmap) 개체를 사용할 수 있습니다. [  **Auto**](https://docs.microsoft.com/uwp/api/Windows.Media.Capture.MediaCaptureMemoryPreference)로 지정할 경우 시스템에서는 프레임을 저장하는 데 가장 적합한 메모리 위치를 동적으로 선택합니다. 시스템에서 GPU 메모리를 사용하도록 선택한 경우 미디어 프레임이 **SoftwareBitmap**이 아닌 [**IDirect3DSurface**](https://docs.microsoft.com/uwp/api/Windows.Graphics.DirectX.Direct3D11.IDirect3DSurface) 개체로 수신됩니다.
* [**StreamingCaptureMode**](https://docs.microsoft.com/uwp/api/windows.media.capture.mediacaptureinitializationsettings.streamingcapturemode) - Set this to [**Video**](https://docs.microsoft.com/uwp/api/Windows.Media.Capture.StreamingCaptureMode) to indicate that audio doesn't need to be streamed.

[  **InitializeAsync**](https://docs.microsoft.com/uwp/api/windows.media.capture.mediacapture.initializeasync)를 호출하여 **MediaCapture**를 원하는 설정으로 초기화합니다. 초기화가 실패하는 경우 *try* 블록 내에서 호출해야 합니다.

[!code-cs[InitMediaCapture](./code/Frames_Win10/Frames_Win10/MainPage.xaml.cs#SnippetInitMediaCapture)]

## <a name="set-the-preferred-format-for-the-frame-source"></a>프레임 원본에 대한 기본 형식 설정
프레임 원본에 대한 기본 형식을 설정하려면 원본을 나타내는 [**MediaFrameSource**](https://docs.microsoft.com/uwp/api/Windows.Media.Capture.Frames.MediaFrameSource) 개체를 가져와야 합니다. 이 개체를 가져오려면 초기화된 **MediaCapture** 개체의 [**Frames**](https://docs.microsoft.com/previous-versions/windows/apps/phone/jj207578(v=win.10)) 사전에 액세스하여 사용할 프레임 원본의 식별자를 지정합니다. 이러한 이유로 프레임 원본 그룹을 선택할 때 [**MediaFrameSourceInfo**](https://docs.microsoft.com/uwp/api/Windows.Media.Capture.Frames.MediaFrameSourceInfo) 개체를 저장한 것입니다.

[  **MediaFrameSource.SupportedFormats**](https://docs.microsoft.com/uwp/api/windows.media.capture.frames.mediaframesource.supportedformats) 속성에는 프레임 원본에 대해 지원되는 형식을 설명하는 [**MediaFrameFormat**](https://docs.microsoft.com/uwp/api/Windows.Media.Capture.Frames.MediaFrameFormat) 개체 목록이 있습니다. **Where** Linq 확장 메서드를 사용하여 원하는 속성을 기반으로 형식을 선택합니다. 이 예제에서는 너비가 1080픽셀이고 32비트 RGB 형식으로 제공할 수 있는 형식이 선택됩니다. **FirstOrDefault** 확장 메서드는 목록의 첫 번째 항목을 선택합니다. 선택한 형식이 null이면 요청한 형식이 프레임 원본에서 지원되지 않습니다. 형식이 지원되는 경우 [**SetFormatAsync**](https://docs.microsoft.com/windows/uwp/develop/)를 호출하여 원본에서 이 형식을 사용하도록 요청할 수 있습니다.

[!code-cs[GetPreferredFormat](./code/Frames_Win10/Frames_Win10/MainPage.xaml.cs#SnippetGetPreferredFormat)]

## <a name="create-a-frame-reader-for-the-frame-source"></a>프레임 원본에 대한 프레임 읽기 프로그램 만들기
미디어 프레임 원본에 대한 프레임을 수신하려면 [**MediaFrameReader**](https://docs.microsoft.com/uwp/api/Windows.Media.Capture.Frames.MediaFrameReader)를 사용합니다.

[!code-cs[DeclareMediaFrameReader](./code/Frames_Win10/Frames_Win10/MainPage.xaml.cs#SnippetDeclareMediaFrameReader)]

초기화한 **MediaCapture** 개체에서 [**CreateFrameReaderAsync**](https://docs.microsoft.com/uwp/api/windows.media.capture.mediacapture.createframereaderasync)를 호출하여 프레임 읽기 프로그램을 인스턴스화합니다. 이 메서드에 대한 첫 번째 인수는 프레임을 수신하려는 프레임 원본입니다. 사용하려는 각 프레임 원본에 대해 개별 프레임 읽기 프로그램을 만들 수 있습니다. 두 번째 인수는 도착할 프레임의 출력 형식을 지정합니다. 이렇게 하면 프레임이 도착한 후 직접 변환할 필요가 없습니다. 프레임 원본에서 지원하지 않는 형식을 지정하려는 경우 예외가 발생하므로 이 변수는 [**SupportedFormats**](https://docs.microsoft.com/uwp/api/windows.media.capture.frames.mediaframesource.supportedformats) 컬렉션에 있어야 합니다.  

프레임 읽기 프로그램을 만든 다음 원본에서 새 프레임을 사용할 수 있을 때마다 발생하는 [**FrameArrived**](https://docs.microsoft.com/uwp/api/windows.media.capture.frames.mediaframereader.framearrived) 이벤트에 대한 처리기를 등록합니다.

[  **StartAsync**](https://docs.microsoft.com/uwp/api/windows.media.capture.frames.mediaframereader.startasync)를 호출하여 원본에서 프레임 읽기를 시작하도록 지정합니다.

[!code-cs[CreateFrameReader](./code/Frames_Win10/Frames_Win10/MainPage.xaml.cs#SnippetCreateFrameReader)]

## <a name="handle-the-frame-arrived-event"></a>프레임 도착 이벤트 처리
새 프레임을 사용할 수 있을 때마다 [**MediaFrameReader.FrameArrived**](https://docs.microsoft.com/uwp/api/windows.media.capture.frames.mediaframereader.framearrived) 이벤트가 발생합니다. 도착하는 모든 프레임을 처리하거나 필요한 프레임만 사용할 수 있습니다. 프레임 읽기 프로그램은 해당 자체 스레드에서 이벤트를 발생시키므로 여러 스레드에서 동일한 데이터에 액세스하지 않도록 일부 동기화 논리를 구현해야 합니다. 이 섹션에서는 그리기 색 프레임을 XAML 페이지의 이미지 컨트롤과 동기화하는 방법을 보여 줍니다. 이 시나리오에서는 XAML 컨트롤에 대한 모든 업데이트가 UI 스레드에서 수행되어야 하는 추가 동기화 제약 조건을 설명합니다.

XAML에서 프레임을 표시하는 첫 번째 단계는 이미지 컨트롤을 만드는 것입니다. 

[!code-xml[ImageElementXAML](./code/Frames_Win10/Frames_Win10/MainPage.xaml#SnippetImageElementXAML)]

코드 숨김 페이지에서 들어오는 모든 이미지가 복사되는 백 버퍼로 사용할 **SoftwareBitmap**의 클래스 멤버 변수를 선언합니다. 이미지 데이터 자체가 아닌 개체 참조만 복사됩니다. 또한 UI 작업이 현재 실행 중인지를 추적하는 부울을 선언합니다.

[!code-cs[DeclareBackBuffer](./code/Frames_Win10/Frames_Win10/MainPage.xaml.cs#SnippetDeclareBackBuffer)]

프레임은 **SoftwareBitmap** 개체로 도착하므로 **SoftwareBitmap**을 XAML **Control**의 원본으로 사용할 수 있도록 [**SoftwareBitmapSource**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Imaging.SoftwareBitmapSource) 개체를 만들어야 합니다. 프레임 읽기 프로그램을 시작하기 전에 코드 내에 이미지 원본을 설정해야 합니다.

[!code-cs[ImageElementSource](./code/Frames_Win10/Frames_Win10/MainPage.xaml.cs#SnippetImageElementSource)]

이제 **FrameArrived** 이벤트 처리기를 구현해야 합니다. 처리기가 호출되면 *sender* 매개 변수에 이벤트를 발생시킬 **MediaFrameReader** 개체에 대한 참조가 포함됩니다. 이 개체에서 [**TryAcquireLatestFrame**](https://docs.microsoft.com/uwp/api/windows.media.capture.frames.mediaframereader.tryacquirelatestframe)을 호출하여 최신 프레임을 가져옵니다. 이름에서 알 수 있듯이 **TryAcquireLatestFrame**은 프레임을 반환하지 못할 수 있습니다. 따라서 VideoMediaFrame에 액세스한 다음 SoftwareBitmap 속성에 액세스할 때는 null인지 테스트해야 합니다. 이 예제에서는 null 조건부 연산자 ?를 사용하여 **SoftwareBitmap**에 액세스하고 검색된 개체가 null인지 확인합니다.

**Image** 컨트롤은 프리멀티플라이되거나 알파가 없는 BRGA8 형식으로만 이미지를 표시할 수 있습니다. 도착하는 프레임이 해당 형식이 아니면 정적 메서드 [**Convert**](https://docs.microsoft.com/uwp/api/windows.graphics.imaging.softwarebitmap.convert)가 소프트웨어 비트맵을 올바른 형식으로 변환합니다.

다음으로 [**Interlocked.Exchange**](https://docs.microsoft.com/dotnet/api/system.threading.interlocked.exchange#System_Threading_Interlocked_Exchange__1___0____0_) 메서드를 사용하여 도착하는 비트맵의 참조를 백 버퍼 비트맵으로 바꿉니다. 이 메서드는 스레드로부터 안전한 원자성 작업에서 이러한 참조를 바꿉니다. 바꾼 후에는 이제 *softwareBitmap* 변수에 있는 이전 백 버퍼 이미지가 삭제되어 해당 리소스가 정리됩니다.

다음으로 **Image** 요소와 연관된 [**CoreDispatcher**](https://docs.microsoft.com/uwp/api/Windows.UI.Core.CoreDispatcher)를 사용하여 [**RunAsync**](https://docs.microsoft.com/uwp/api/windows.ui.core.coredispatcher.runasync)를 호출하여 UI 스레드에서 실행할 작업을 만듭니다. 작업 내에서 비동기 작업이 수행되므로 *async* 키워드를 사용하여 **RunAsync**로 전달된 람다 식이 선언됩니다.

작업 내에서 *_taskRunning* 변수를 검사하여 한 번에 작업의 한 인스턴스만 실행 중인지 확인합니다. 작업이 실행되고 있지 않은 경우 작업이 다시 실행되지 않도록 *_taskRunning*이 true로 설정됩니다. *while* 루프에서 백 버퍼 이미지가 null이 될 때까지 백 버퍼에서 임시 **SoftwareBitmap**으로 복사하기 위해 **Interlocked.Exchange**를 호출합니다. 임시 비트맵을 채울 때마다 **Image**의 **Source** 속성이 **SoftwareBitmapSource**로 캐스트된 다음 [**SetBitmapAsync**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.imaging.softwarebitmapsource.setbitmapasync)가 호출되어 이미지의 원본을 설정합니다.

마지막으로, 다음에 처리기를 호출할 때 작업이 다시 실행될 수 있도록 *_taskRunning* 변수를 다시 false로 설정합니다.

> [!NOTE] 
> [  **MediaFrameReference**](https://docs.microsoft.com/uwp/api/windows.media.capture.frames.videomediaframe.softwarebitmap)의 [**VideoMediaFrame**](https://docs.microsoft.com/uwp/api/windows.media.capture.frames.videomediaframe.direct3dsurface) 속성에서 제공하는 [**SoftwareBitmap**](https://docs.microsoft.com/uwp/api/windows.media.capture.frames.mediaframereference.videomediaframe) 또는 [**Direct3DSurface**](https://docs.microsoft.com/uwp/api/Windows.Media.Capture.Frames.MediaFrameReference) 개체에 액세스하는 경우 시스템에서 해당 개체에 대한 강력한 참조를 만듭니다. 즉, 이러한 참조는 포함하는 **MediaFrameReference**에서 [**Dispose**](https://docs.microsoft.com/uwp/api/windows.media.capture.frames.mediaframereference.close)를 호출할 때 삭제되지 않습니다. 개체를 즉시 삭제하려면 **SoftwareBitmap** 또는 **Direct3DSurface**의 **Dispose** 메서드를 직접 명시적으로 호출해야 합니다. 그러지 않으면 가비지 수집기에서 결국 이러한 개체의 메모리를 해제하지만 언제 수행될지 알 수 없으며, 할당된 비트맵 또는 화면 수가 시스템에서 허용된 최대 크기를 초과할 경우 새 프레임의 흐름이 중단됩니다. 예를 들어 [**SoftwareBitmap.Copy**](https://docs.microsoft.com/en-us/uwp/api/windows.graphics.imaging.softwarebitmap.copy) 메서드를 사용하여 검색된 프레임을 복사한 후 원래 프레임을 해제하여 이 제한을 해결할 수 있습니다. 또한 오버로드 [CreateFrameReaderAsync(Windows.Media.Capture.Frames.MediaFrameSource inputSource, System.String outputSubtype, Windows.Graphics.Imaging.BitmapSize outputSize)](https://docs.microsoft.com/uwp/api/windows.media.capture.mediacapture.createframereaderasync#Windows_Media_Capture_MediaCapture_CreateFrameReaderAsync_Windows_Media_Capture_Frames_MediaFrameSource_System_String_Windows_Graphics_Imaging_BitmapSize_) 또는 [CreateFrameReaderAsync(Windows.Media.Capture.Frames.MediaFrameSource inputSource, System.String outputSubtype)](https://docs.microsoft.com/uwp/api/windows.media.capture.mediacapture.createframereaderasync#Windows_Media_Capture_MediaCapture_CreateFrameReaderAsync_Windows_Media_Capture_Frames_MediaFrameSource_System_String_)을 사용하여 **MediaFrameReader**를 만들 경우 반환된 프레임은 원래 프레임 데이터의 사본이므로 이것을 유지할 때에도 프레임 획득을 중지하지 않습니다. 


[!code-cs[FrameArrived](./code/Frames_Win10/Frames_Win10/MainPage.xaml.cs#SnippetFrameArrived)]

## <a name="cleanup-resources"></a>리소스 정리
프레임 읽기가 완료된 후에는 [**StopAsync**](https://docs.microsoft.com/uwp/api/windows.media.capture.frames.mediaframereader.stopasync)를 호출하고, **FrameArrived** 처리기의 등록을 취소하고, **MediaCapture** 개체를 삭제하여 미디어 프레임 읽기 프로그램을 중지해야 합니다.

[!code-cs[Cleanup](./code/Frames_Win10/Frames_Win10/MainPage.xaml.cs#SnippetCleanup)]

응용 프로그램이 일시 중지될 때 미디어 캡처 개체 정리에 대한 자세한 내용은 [**카메라 미리 보기 표시**](simple-camera-preview-access.md)를 참조하세요.

## <a name="the-framerenderer-helper-class"></a>FrameRenderer 도우미 클래스
유니버설 Windows [카메라 프레임 샘플](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/CameraFrames)은 앱의 색, 적외선 및 깊이 원본을 통해 프레임을 쉽게 표시할 수 있는 도우미 클래스를 제공합니다. 일반적으로 단순히 화면에 표시하는 대신 색과 적외선 데이터를 사용하여 더 많은 작업을 수행하려고 하지만 이 도우미 클래스를 사용하면 프레임 읽기 프로그램 기능을 보여 주고 프레임 읽기 프로그램 구현을 디버깅할 수 있습니다.

**FrameRenderer** 도우미 클래스는 다음 메서드를 구현합니다.

* **FrameRenderer** 생성자 - 생성자는 미디어 프레임 표시를 위해 전달한 XAML [**Image**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Image) 요소를 사용하도록 도우미 클래스를 초기화합니다.
* **ProcessFrame** - 이 메서드는 생성자로 전달한 **Image** 요소에서 미디어 프레임을 [**MediaFrameReference**](https://docs.microsoft.com/uwp/api/Windows.Media.Capture.Frames.MediaFrameReference)로 표시합니다. 일반적으로 [**FrameArrived**](https://docs.microsoft.com/uwp/api/windows.media.capture.frames.mediaframereader.framearrived) 이벤트 처리기에서 이 메서드를 호출하고 [**TryAcquireLatestFrame**](https://docs.microsoft.com/uwp/api/windows.media.capture.frames.mediaframereader.tryacquirelatestframe)에서 반환한 프레임을 전달합니다.
* **ConvertToDisplayableImage** - 이 메서드는 미디어 프레임의 형식을 확인하고 필요한 경우 표시 가능한 형식으로 변환합니다. 컬러 이미지의 경우 색 형식이 BGRA8이고 비트맵 알파 모드가 프리멀티플라이되어야 함을 의미입니다. 깊이 또는 적외선 프레임의 경우 샘플에 포함되고 아래에 나열된 **PsuedoColorHelper** 클래스를 사용하여 깊이 또는 적외선 값을 의사 색 그라데이션으로 변환하도록 각 스캐닝선이 처리됩니다.

> [!NOTE] 
> **SoftwareBitmap** 이미지에서 픽셀 조작을 수행하려면 원시 메모리 버퍼에 액세스해야 합니다. 이렇게 하려면 아래 나열된 코드에 포함된 IMemoryBufferByteAccess COM 인터페이스를 사용하고 안전하지 않은 코드의 컴파일을 허용하도록 프로젝트 속성을 업데이트해야 합니다. 자세한 내용은 [비트맵 이미지 만들기, 편집 및 저장](imaging.md)을 참조하세요.

[!code-cs[IMemoryBufferByteAccess](./code/Frames_Win10/Frames_Win10/FrameRenderer.cs#SnippetIMemoryBufferByteAccess)]

[!code-cs[FrameArrived](./code/Frames_Win10/Frames_Win10/FrameRenderer.cs#SnippetFrameRenderer)]

## <a name="use-multisourcemediaframereader-to-get-time-corellated-frames-from-multiple-sources"></a>MultiSourceMediaFrameReader를 사용하여 여러 원본에서 시간 연관 프레임 가져오기
Windows 10 버전 1607부터 [**MultiSourceMediaFrameReader**](https://docs.microsoft.com/uwp/api/windows.media.capture.frames.multisourcemediaframereader)를 사용하여 다양한 원본에서 시간 연관 프레임을 가져올 수 있습니다. 이 API를 사용하면 [**DepthCorrelatedCoordinateMapper**](https://docs.microsoft.com/uwp/api/windows.media.devices.core.depthcorrelatedcoordinatemapper) 클래스를 사용한 임시 근접 연결에서 얻은 여러 원본의 프레임을 필요로 하는 프로세싱을 더 쉽게 처리할 수 있습니다. 이 새로운 메서드의 한 가지 제한은 프레임 도착 이벤트가 가장 느린 캡처 원본의 속도로만 발생한다는 점입니다. 더 빠른 원본의 추가 프레임은 삭제됩니다. 또한 시스템이 다른 원본의 프레임이 다른 속도로 도착할 것으로 예상하기 때문에, 원본에서 프레임 생성을 모두 함께 중단한 경우에도 자동으로 인식하지 못합니다. 이 섹션의 예제 코드에서는 시간 연관 프레임이 앱에서 정의한 시간 제한 내에 도착하지 않으면 호출되는 자체 시간 초과 논리를 만들기 위해 이벤트를 사용하는 방법을 보여 줍니다.

[  **MultiSourceMediaFrameReader**](https://docs.microsoft.com/uwp/api/windows.media.capture.frames.multisourcemediaframereader)를 사용하는 단계는 이 문서의 앞에서 설명한 [**MediaFrameReader**](https://docs.microsoft.com/uwp/api/Windows.Media.Capture.Frames.MediaFrameReader)를 사용하는 단계와 유사합니다. 이 예에서는 색상 원본과 깊이 원본을 사용합니다. 몇몇 문자열 변수를 정의하여 각 원본의 프레임을 선택하는 데 사용되는 미디어 프레임 소스 ID를 저장합니다. 다음, 이 예의 시간 초과 논리를 구현하는 데 사용되는 [**ManualResetEventSlim**](https://docs.microsoft.com/dotnet/api/system.threading.manualreseteventslim), [**CancellationTokenSource**](https://docs.microsoft.com/dotnet/api/system.threading.cancellationtokensource) 및 [**EventHandler**](https://docs.microsoft.com/dotnet/api/system.eventhandler)를 선언합니다. 

[!code-cs[MultiFrameDeclarations](./code/Frames_Win10/Frames_Win10/MainPage.xaml.cs#SnippetMultiFrameDeclarations)]

이 문서에서 설명한 기술을 사용하여, 이 예제 시나리오에 필요한 색상 및 깊이 원본을 포함하는 [**MediaFrameSourceGroup**](https://docs.microsoft.com/uwp/api/Windows.Media.Capture.Frames.MediaFrameSourceGroup)을 쿼리합니다. 원하는 프레임 원본 그룹을 선택한 후, 각 프레임 원본에서 [**MediaFrameSourceInfo**](https://docs.microsoft.com/uwp/api/Windows.Media.Capture.Frames.MediaFrameSourceInfo)를 가져옵니다.

[!code-cs[SelectColorAndDepth](./code/Frames_Win10/Frames_Win10/MainPage.xaml.cs#SnippetSelectColorAndDepth)]

**MediaCapture** 개체를 만들어 초기화하고, 초기화 설정에서 선택된 프레임 원본 그룹에 전달합니다.

[!code-cs[MultiFrameInitMediaCapture](./code/Frames_Win10/Frames_Win10/MainPage.xaml.cs#SnippetMultiFrameInitMediaCapture)]

**MediaCapture** 개체를 초기화한 후, 색상 및 깊이 카메라에 대한 [**MediaFrameSource**](https://docs.microsoft.com/uwp/api/Windows.Media.Capture.Frames.MediaFrameSource) 개체를 가져옵니다. 각 원본의 ID를 저장하여 해당 원본에서 도착하는 프레임을 선택할 수 있도록 합니다.

[!code-cs[GetColorAndDepthSource](./code/Frames_Win10/Frames_Win10/MainPage.xaml.cs#SnippetGetColorAndDepthSource)]

[  **CreateMultiSourceFrameReaderAsync**](https://docs.microsoft.com/uwp/api/windows.media.capture.mediacapture.createmultisourceframereaderasync)를 호출하고 읽기 프로그램이 사용할 프레임 원본 배열을 전달하여 **MultiSourceMediaFrameReader**를 만들고 초기화합니다. [  **FrameArrived**](https://docs.microsoft.com/uwp/api/windows.media.capture.frames.multisourcemediaframereader.FrameArrived) 이벤트에 대한 이벤트 처리기를 등록합니다. 이 예에서는 이 문서에서 이미 설명한 **FrameRenderer** 도우미 클래스의 인스턴스를 만들어 프레임을 **Image** 컨트롤로 렌더링합니다. [  **StartAsync**](https://docs.microsoft.com/uwp/api/windows.media.capture.frames.multisourcemediaframereader.StartAsync)를 호출하여 프레임 읽기 프로그램을 시작합니다.

이 예의 앞에서 선언한 **CorellationFailed** 이벤트에 대한 이벤트 처리기를 등록합니다. 사용 중인 미디어 프레임 원본 중 하나가 프레임 생성을 중단하면 이 이벤트에 신호를 보낼 것입니다. 마지막으로, [**Task.Run**](https://docs.microsoft.com/dotnet/api/system.threading.tasks.task.run#System_Threading_Tasks_Task_Run_System_Action_)을 호출하여 시간 초과 도우미 메서드인 **NotifyAboutCorrelationFailure**를 별도 스레드에서 호출합니다. 이 메서드의 구현 방법은 이 문서의 뒷부분에서 설명합니다.

[!code-cs[InitMultiFrameReader](./code/Frames_Win10/Frames_Win10/MainPage.xaml.cs#SnippetInitMultiFrameReader)]

**MultiSourceMediaFrameReader**에서 관리하는 모든 미디어 프레임 원본에서 새 프레임을 사용할 수 있게 되면 **FrameArrived** 이벤트가 발생합니다. 즉, 이벤트가 가장 느린 미디어 원본의 흐름에 따라 발생하게 됩니다. 한 원본에서 더 느린 원본이 한 프레임을 생성하는 동안 여러 프레임을 생성한 경우, 빠른 원본의 추가 프레임은 삭제됩니다. 

[  **TryAcquireLatestFrame**](https://docs.microsoft.com/uwp/api/windows.media.capture.frames.multisourcemediaframereader.TryAcquireLatestFrame)을 호출하여 이벤트와 관련된 [**MultiSourceMediaFrameReference**](https://docs.microsoft.com/uwp/api/windows.media.capture.frames.multisourcemediaframereference)를 가져옵니다. [  **TryGetFrameReferenceBySourceId**](https://docs.microsoft.com/uwp/api/windows.media.capture.frames.multisourcemediaframereference.trygetframereferencebysourceid)를 호출하고 프레임 읽기 프로그램이 초기화되었을 때 저장된 ID 문자열을 전달하여 각 미디어 프레임 원본에 관련된 **MediaFrameReference**를 가져옵니다.

**ManualResetEventSlim** 개체의 [**Set**](https://docs.microsoft.com/dotnet/api/system.threading.manualreseteventslim.set#System_Threading_ManualResetEventSlim_Set) 메서드를 호출하여 프레임이 도착했다는 신호를 보냅니다. 이 이벤트는 별도의 스레드에서 실행 중인 **NotifyCorrelationFailure** 메서드에서 확인합니다. 

마지막으로, 시간 연관 프레임을 위한 처리를 수행합니다. 이 예에서는 간단히 깊이 원본의 프레임만 표시합니다.

[!code-cs[MultiFrameArrived](./code/Frames_Win10/Frames_Win10/MainPage.xaml.cs#SnippetMultiFrameArrived)]

프레임 읽기 프로그램이 다시 시작된 후 **NotifyCorrelationFailure** 도우미 메서드가 별도 스레드에서 실행됩니다. 이 메서드는 프레임 수신 이벤트의 신호를 받았는지 확인합니다. **FrameArrived** 처리기에서 상호 관련 프레임의 집합이 도착할 때마다 이 이벤트가 발생하도록 설정했습니다. 앱에서 정의한 기간(5초 정도가 적당) 동안 이벤트에서 신호를 보내지 않을 경우나 **CancellationToken**을 사용하여 작업이 취소된 경우는 미디어 프레임 중 하나에서 읽기를 중지한 것일 수 있습니다. 이런 경우 일반적으로 프레임 읽기 프로그램을 종료해야 하므로, 앱에서 정의한 **CorrelationFailed** 이벤트를 발생시킵니다. 이 이벤트의 처리기에서 프레임 읽기 프로그램을 중지하고 이 문서 앞부분에서 설명한 방법으로 관련된 리소스를 정리할 수 있습니다.

[!code-cs[NotifyCorrelationFailure](./code/Frames_Win10/Frames_Win10/MainPage.xaml.cs#SnippetNotifyCorrelationFailure)]

[!code-cs[CorrelationFailure](./code/Frames_Win10/Frames_Win10/MainPage.xaml.cs#SnippetCorrelationFailure)]

## <a name="use-buffered-frame-acquisition-mode-to-preserve-the-sequence-of-acquired-frames"></a>버퍼링된 프레임 획득 모드를 사용하여 획득된 프레임의 시퀀스를 유지합니다.
Windows 10 버전 1709부터 **MediaFrameReader** 또는 **MultiSourceMediaFrameReader**의 **[AcquisitionMode](https://docs.microsoft.com/uwp/api/windows.media.capture.frames.mediaframereader.AcquisitionMode)** 속성을 **Buffered**로 설정하여 프레임 소스에서 앱으로 전달되는 프레임의 시퀀스를 유지할 수 있습니다.

[!code-cs[SetBufferedFrameAcquisitionMode](./code/Frames_Win10/Frames_Win10/MainPage.xaml.cs#SnippetSetBufferedFrameAcquisitionMode)]

기본 획득 모드 **Realtime**에서 앱이 이전 프레임에 대해 **FrameArrived** 이벤트를 처리하는 동안 소스에서 여러 프레임을 얻으면 시스템은 앱에 가장 최근에 획득한 프레임을 보내고 버퍼에서 대기하고 있는 프레임을 삭제합니다. 따라서 앱에 항상 가장 최근에 사용 가능한 프레임이 제공됩니다. 이것은 일반적으로 실시간 컴퓨터 비전 응용 프로그램에 가장 유용한 모드입니다. 

**Buffered** 획득 모드에서 시스템은 모든 프레임을 버퍼에 보관하고 **FrameArrived** 이벤트를 통해 받은 순서대로 앱에 제공합니다. 이 모드에서 프레임에 대한 시스템의 버퍼가 채워지면 앱이 이전 프레임에 대해 **FrameArrived** 이벤트를 완료할 때까지 새로운 프레임 획득을 중지하여 버퍼에서 더 많은 공간을 확보합니다.

## <a name="use-mediasource-to-display-frames-in-a-mediaplayerelement"></a>MediaSource를 사용하여 MediaPlayerElement에 프레임 표시
Windows 버전 1709부터 XAML 페이지의 **[MediaPlayerElement](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.mediaplayerelement)** 컨트롤에서 직접 **MediaFrameReader**에서 획득한 프레임을 표시할 수 있습니다. 이 작업을 하려면 **[MediaSource.CreateFromMediaFrameSource](https://docs.microsoft.com/uwp/api/windows.media.core.mediasource.createfrommediaframesource)** 를 사용하여 **MediaPlayerElement**와 관련된 **[MediaPlayer](https://docs.microsoft.com/uwp/api/windows.media.playback.mediaplayer)** 가 직접 사용할 수 있는 **[MediaSource](https://docs.microsoft.com/uwp/api/windows.media.core.mediasource)** 개체를 만듭니다. **MediaPlayer** 및 **MediaPlayerElement** 작업에 대한 자세한 내용은 [MediaPlayer를 사용하여 오디오 및 비디오 재생](play-audio-and-video-with-mediaplayer.md)을 참조하세요.

다음 코드 예제는 XAML 페이지에서 정면 및 후면 카메라의 프레임을 동시에 표시하는 간단한 구현을 보여줍니다.

첫째, **MediaPlayerElement** 컨트롤 2개를 XAML 페이지에 추가합니다.

[!code-xml[MediaPlayerElement1XAML](./code/Frames_Win10/Frames_Win10/MainPage.xaml#SnippetMediaPlayerElement1XAML)]

[!code-xml[MediaPlayerElement2XAML](./code/Frames_Win10/Frames_Win10/MainPage.xaml#SnippetMediaPlayerElement2XAML)]

그런 다음 이 문서의 이전 섹션에 나오는 기술을 사용하여 전면 패널 및 후면 패널에서 컬러 카메라용 **MediaFrameSourceInfo** 개체를 포함하는 **MediaFrameSourceGroup**을 선택합니다. **MediaPlayer**은 프레임을 깊이 또는 적외선 데이터와 같은 색상 이외의 형식에서 색 데이터로 자동 변환하지 않습니다. 다른 센서 유형을 사용하면 예상치 못한 결과가 발생할 수 있습니다. 

[!code-cs[MediaSourceSelectGroup](./code/Frames_Win10/Frames_Win10/MainPage.xaml.cs#SnippetMediaSourceSelectGroup)]

선택한 **MediaFrameSourceGroup**을 사용하기 위해 **MediaCapture** 개체를 초기화합니다.

[!code-cs[MediaSourceInitMediaCapture](./code/Frames_Win10/Frames_Win10/MainPage.xaml.cs#SnippetMediaSourceInitMediaCapture)]

마지막으로 **[MediaSource.CreateFromMediaFrameSource](https://docs.microsoft.com/uwp/api/windows.media.core.mediasource.createfrommediaframesource)** 를 호출하고 관련 **MediaFrameSourceInfo** 개체의 **[Id](https://docs.microsoft.com/uwp/api/windows.media.capture.frames.mediaframesourceinfo.Id)** 속성을 사용하여 각 프레임 원본별로 **MediaSource**를 만들고 나서 **MediaCapture** object's **[FrameSources](https://docs.microsoft.com/uwp/api/windows.media.capture.mediacapture.FrameSources)** 컬렉션에서 프레임 원본 중 하나를 선택합니다. **[SetMediaPlayer](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.mediaplayerelement.MediaPlayer)** 를 호출하여 새로운 **MediaPlayer** 개체를 초기화하고 이것을 **MediaPlayerElement**에 할당합니다. 그리고 나서 **[Source](https://docs.microsoft.com/uwp/api/windows.media.playback.mediaplayer.Source)** 속성을 새로 만든  **MediaSource** 개체로 설정합니다.

[!code-cs[MediaSourceMediaPlayer](./code/Frames_Win10/Frames_Win10/MainPage.xaml.cs#SnippetMediaSourceMediaPlayer)]

## <a name="use-video-profiles-to-select-a-frame-source"></a>비디오 프로필을 사용하여 프레임 원본 선택

[  **MediaCaptureVideoProfile**](https://docs.microsoft.com/uwp/api/Windows.Media.Capture.MediaCaptureVideoProfile) 개체로 표시되는 카메라 프로필은 프레임 속도, 해상도 또는 HDR 캡처와 같은 고급 기능 등 특정 캡처 디바이스에서 제공하는 기능 세트를 나타냅니다. 캡처 디바이스는 여러 프로필을 지원할 수 있으므로 캡처 시나리오에 맞게 최적화된 프로필을 선택할 수 있습니다. Windows 10 버전 1803부터 **MediaCaptureVideoProfile**을 사용하여 특정 기능과 함께 미디어 프레임 원본을 선택한 후 **MediaCapture** 개체를 초기화할 수 있습니다. 다음 예제 메서드는 WCG(Wide Color Gamut)로 HDR을 지원하는 비디오 프로필을 찾고 선택한 디바이스와 프로필을 사용하기 위해 **MediaCapture**를 초기화하는 데 사용할 수 있는 **MediaCaptureInitializationSettings** 개체를 반환합니다.

먼저, [**MediaFrameSourceGroup.FindAllAsync**](https://docs.microsoft.com/uwp/api/windows.media.capture.frames.mediaframesourcegroup.findallasync)를 호출하여 현재 디바이스에서 사용할 수 있는 전체 미디어 프레임 소스 그룹 목록을 가져옵니다. 각 소스 그룹을 반복 실행하고 [**MediaCapture.FindKnownVideoProfiles**](https://docs.microsoft.com/uwp/api/windows.media.capture.mediacapture.findknownvideoprofiles)를 호출하여 지정된 프로필을 지원하는 현재 소스 그룹에 대한 모든 비디오 프로필 목록을 가져옵니다(이 경우는 WCG 사진이 있는 HDR). 기준을 충족하는 프로필이 발견되면 새로운 **MediaCaptureInitializationSettings** 개체를 만들고 **VideoProfile**을 프로필 선택으로 설정하고  **VideoDeviceId**를 현재 미디어 프레임 소스 그룹의 **Id** 속성으로 설정합니다.

[!code-cs[GetSettingsWithProfile](./code/Frames_Win10/Frames_Win10/MainPage.xaml.cs#SnippetGetSettingsWithProfile)]

카메라 프로필 사용에 대한 자세한 내용은 [카메라 프로필](camera-profiles.md)을 참조하세요.

## <a name="related-topics"></a>관련 항목

* [카메라](camera.md)
* [Basic photo, video, and audio capture with MediaCapture](basic-photo-video-and-audio-capture-with-MediaCapture.md)
* [Camera frames sample](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/CameraFrames)
 

 




