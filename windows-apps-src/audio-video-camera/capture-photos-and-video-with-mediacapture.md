---
ms.assetid: 1361E82A-202F-40F7-9239-56F00DFCA54B
description: 이 문서에서는 MediaCapture의 초기화 및 종료, 디바이스 방향 변경 처리를 비롯하여 MediaCapture API를 사용하여 사진 및 동영상을 캡처하는 단계를 설명합니다.
title: MediaCapture를 사용하여 사진 및 비디오 캡처
---

# MediaCapture를 사용하여 사진 및 비디오 캡처

\[ Windows 10의 UWP 앱에 맞게 업데이트되었습니다. Windows 8.x 문서는 [보관](http://go.microsoft.com/fwlink/p/?linkid=619132)을 참조하세요. \]


이 문서에서는 **MediaCapture**의 초기화 및 종료, 디바이스 방향 변경 처리를 비롯하여 [**MediaCapture**](https://msdn.microsoft.com/library/windows/apps/br241124) API를 사용하여 사진 및 비디오를 캡처하는 단계를 설명합니다.

**MediaCapture**는 미디어 캡처 프로세스의 하위 수준 제어를 요구하고 고급 캡처 기능을 필요로 하는 시나리오를 구현하는 앱을 지원하기 위해 제공됩니다. **MediaCapture**를 사용하려면 자체 캡처 UI도 제공해야 합니다. 앱에서 사진이나 비디오를 캡처하기만 하면 되며 고급 캡처 기술은 필요하지 않은 경우 [**CameraCaptureUI**](https://msdn.microsoft.com/library/windows/apps/br241030)를 사용하면 몇 줄의 코드만으로 사진이나 비디오를 쉽게 캡처할 수 있습니다. 자세한 내용은 [CameraCaptureUI를 사용하여 사진 및 비디오 캡처](capture-photos-and-video-with-cameracaptureui.md)를 참조하세요.

이 문서의 코드는 [CameraStarterKit 샘플](http://go.microsoft.com/fwlink/?LinkId=619479)에서 조정되었습니다. 샘플을 다운로드하여 상황에 맞게 사용되는 코드를 참조하거나 자체 앱을 처음 빌드하기 시작할 때 샘플을 사용할 수 있습니다.

## 프로젝트 구성

### 앱 매니페스트에 기능 선언 추가

앱에서 디바이스의 카메라에 액세스해야 하는 경우 앱에 *webcam* and *microphone* 디바이스 기능이 사용된다고 선언해야 합니다. 캡처한 사진 또는 비디오를 사용자의 사진 라이브러리에 저장하려는 경우에도 *picturesLibrary* 및 *videosLibrary*기능을 선언해야 합니다.

**기능을 앱 매니페스트에 추가**

1.  Microsoft Visual Studio의 **솔루션 탐색기**에서 **package.appxmanifest** 항목을 두 번 클릭하여 응용 프로그램 매니페스트 디자이너를 엽니다.
2.  **접근 권한 값** 탭을 선택합니다.
3.  **웹캠** 확인란과 **마이크** 상자를 선택합니다.
4.  사진과 비디오 라이브러리에 액세스하려면 **사진 라이브러리**의 확인란과 **비디오 라이브러리**의 확인란을 선택합니다.

### 미디어 캡처 관련 API에 using 지시문 사용

다음 코드 목록에서는 이 문서의 샘플 코드에서 참조되는 네임스페이스를 보여 주며 각 네임스페이스가 제공하는 기능에 대해 설명합니다.

[!code-cs[Using](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetUsing)]

## MediaCapture 개체 초기화

[
            **Windows.Media.Capture**](https://msdn.microsoft.com/library/windows/apps/br226738) 네임스페이스의 [**MediaCapture**](https://msdn.microsoft.com/library/windows/apps/br241124) 클래스는 모든 미디어 캡처 작업에 대한 기본 인터페이스입니다. 앱은 일반적으로 단일 페이지로 범위가 지정된 이 형식의 변수를 선언합니다. 앱은 **MediaCapture**의 현재 상태를 추적해야 하므로 개체의 초기화, 미리 보기 및 녹화 상태에 대한 부울 변수를 선언해야 합니다.

[!code-cs[MediaCaptureVariables](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetMediaCaptureVariables)]

미리 보기 비디오 방향을 올바르게 지정하기 위해 카메라가 외부에 있는지 여부와 앱이 현재 미리 보기 스트림을 미러링하는지를 추적하는 멤버 변수를 만듭니다. 비디오 피드가 사용자를 캡처한다고 생각할 때 앱은 미리 보기 스트림을 미러링합니다. 이것이 좀 더 자연스러운 사용자 환경이기 때문입니다.

[!code-cs[PreviewVariables](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetPreviewVariables)]

다음 예제 메서드는 미디어 캡처 개체를 초기화합니다. 먼저, 이 코드는 미디어 캡처에 사용할 수 있는 비디오 캡처 장치를 검색합니다. 일단 확인되면 **MediaCapture** 개체가 초기화되고 해당 이벤트에 대한 처리기가 등록됩니다. 다음으로 비디오 캡처 장치 ID를 사용하여 [**MediaCaptureInitializationSettings**](https://msdn.microsoft.com/library/windows/desktop/hh802710) 개체가 만들어집니다. 그러면 [**InitializeAsync**](https://msdn.microsoft.com/library/windows/apps/br226598) 호출을 통해 **MediaCapture**가 초기화됩니다.

[!code-cs[InitializeCameraAsync](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetInitializeCameraAsync)]

-   [
            **DeviceInformation.FindAllAsync**](https://msdn.microsoft.com/library/windows/apps/br225432) 메서드는 지정된 형식의 모든 장치를 찾는 데 사용할 수 있습니다. 이 예제에서는 **DeviceClass.VideoCapture** 열거형 값이 전달되어 만 비디오 캡처 장치만 반환되도록 지정합니다. 사진 및 비디오 캡처에 비디오 캡처 장치가 사용됩니다.

-   **FindAllAsync**는 요청한 형식을 갖는 확인된 각 디바이스에 대해 [**DeviceInformation**](https://msdn.microsoft.com/library/windows/apps/br225393) 개체를 포함하는 [**DeviceInformationCollection**](https://msdn.microsoft.com/library/windows/apps/br225395) 개체를 반환합니다. **System.Linq** 네임스페이스의 **FirstOrDefault** 확장 메서드는 지정된 조건에 따라 목록에서 항목을 선택하기 위한 편리한 구문을 제공합니다. 첫 번째 호출은 목록에서 [**EnclosureLocation.Panel**](https://msdn.microsoft.com/library/windows/apps/br229906) 값이 **Panel.Back**인 첫 번째 **DeviceInformation**을 선택하려고 합니다. 이 값의 경우 카메라가 디바이스의 인클로저의 후방 패널에 있는 것입니다. 장치의 후방 패널에 카메라가 없으면 사용 가능한 첫 번째 카메라가 사용됩니다.

-   [
            **MediaCaptureInitializationSettings**](https://msdn.microsoft.com/library/windows/apps/br226573)를 초기화할 때 장치 ID를 지정하지 않으면 시스템은 장치의 내부 목록에서 첫 번째 장치를 선택합니다.

-   [
            **MediaCapture.InitializeAsync**](https://msdn.microsoft.com/library/windows/apps/br226598)에 대한 호출이 지정한 캡처 장치를 사용하도록 개체를 초기화합니다. 이 호출은 사용자가 카메라에 대한 호출 앱 액세스를 거부할 경우 **UnauthorizedAccessException**을 throw하므로 **try** 블록 내에서 수행됩니다. 이 호출이 성공하면 캡처 디바이스를 초기화할 수 있는지를 후속 메서드 호출을 통해 확인할 수 있도록 **\_isInitialized** 변수가 true로 설정됩니다.

- **중요** 일부 디바이스 패밀리에서는 앱에 디바이스의 카메라에 대한 액세스가 허용되기 전에 사용자 동의 확인 프롬프트가 사용자에게 표시됩니다. 이러한 이유로 기본 UI 스레드에서만 [**MediaCapture.InitializeAsync**](https://msdn.microsoft.com/library/windows/apps/br226598)를 호출해야 합니다. 다른 스레드에서 카메라를 초기화하려고 하면 초기화 오류가 발생할 수 있습니다.

-   캡처 디바이스의 초기화를 성공하면 캡처 디바이스가 외부 디바이스인지 또는 디바이스 전방 패널에 있는지를 반영하도록 변수가 설정됩니다. 이러한 값은 사용자에 맞게 캡처 미리 보기 방향을 지정하는 데 사용됩니다. 마지막으로, UI는 캡처를 사용할 수 있는지와 캡처 장치에서의 미리 보기 스트림이 시작되었는지를 반영하도록 업데이트됩니다. 이러한 모든 작업은 이 문서 뒷부분에 설명되는 도우미 메서드에서 수행됩니다.

## 캡처 미리 보기 시작

사용자가 캡처하는 항목을 볼 수 있도록 하려면 비디오 캡처 장치가 현재 UI에서 보고 있는 항목에 대한 미리 보기를 제공해야 합니다.

**중요** 캡처 디바이스에서 자동 초점, 자동 노출 및 자동 화이트 밸런스를 사용하려면 캡처 미리 보기를 시작해야 합니다.

캡처 미리 보기를 사용하도록 설정하기 위해 [**CaptureElement**](https://msdn.microsoft.com/library/windows/apps/br209278) 컨트롤이 제공됩니다. 다음은 캡처 요소를 정의하는 예제 XAML 코드를 보여 줍니다.

[!code-xml[CaptureElement](./code/BasicMediaCaptureWin10/cs/MainPage.xaml#SnippetCaptureElement)]

사용자는 비디오 캡처 화면을 미리 보는 동안 화면이 켜져 있고 비활동 중에도 꺼지지 않을 것으로 예상합니다. 이러한 예상이 충족되려면 [**DisplayRequest**](https://msdn.microsoft.com/library/windows/apps/br241816) 개체를 만들어야 합니다. 캡처 세션 전체에서 지속되도록 페이지 범위로 이 변수를 선언합니다.

[!code-cs[DisplayRequest](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetDisplayRequest)]

다음 메서드는 미디어 캡처 미리 보기를 시작합니다. 먼저 [**DisplayRequest**](https://msdn.microsoft.com/library/windows/apps/br241816)에 대해 [**RequestActive**](https://msdn.microsoft.com/library/windows/apps/br241818)를 호출하여 디스플레이를 활성 상태로 유지하도록 요청합니다. 다음으로 [**StartPreviewAsync**](https://msdn.microsoft.com/library/windows/apps/br226613)를 호출하여 미리 보기가 시작됩니다.

[!code-cs[StartPreviewAsync](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetStartPreviewAsync)]

-   시스템 화면이 켜져 있도록 요청하기 위해 **DisplayRequest** 개체에 대한 [**RequestActive**](https://msdn.microsoft.com/library/windows/apps/br241818) 메서드가 호출됩니다.

-   **CaptureElement**의 [**Source**](https://msdn.microsoft.com/library/windows/apps/br227419) 속성은 미리 보기의 소스를 정의하기 위해 앱의 **MediaCapture** 개체로 설정됩니다.

-   [
            **FlowDirection**](https://msdn.microsoft.com/library/windows/apps/br208716) 속성은 양방향 사용자 인터페이스를 지원하기 위해 XAML 프레임워크에 의해 제공됩니다. **CaptureElement**의 흐름 방향을 [**FlowDirection.RightToLeft**](https://msdn.microsoft.com/library/windows/apps/br242397)로 설정하면 미리 보기 비디오가 가로로 대칭 이동됩니다. 캡처 장치가 장치의 전방 패널에 있을 때 사용자의 관점에서 미리 보기 방향이 올바르도록 하는 데 사용됩니다.

-   [
            **StartPreviewAsync**](https://msdn.microsoft.com/library/windows/apps/br226613) 메서드는 **CaptureElement** 내에서 미리 보기 스트림의 표시를 시작합니다. 미리 보기가 성공적으로 시작되면 앱의 다른 부분에서 앱이 현재 미리 보기 중임을 알 수 있도록 **\_isPreviewing** 변수가 설정되며 미리 보기 방향을 설정하기 위한 도우미 메서드가 호출됩니다. 이 메서드는 다음 섹션에서 정의됩니다.

## 화면 및 장치 방향 감지

휴대폰 또는 태블릿 등의 모바일 장치에서 실행되는 경우 장치의 현재 방향에 따라 영향을 받는 여러 미디어 캡처 앱 영역이 있습니다. 이러한 영역에는 카메라의 미리 보기 스트림을 적절히 회전하고 캡처한 이미지 및 비디오를 적절히 인코딩하여 사용자가 볼 때 올바른 방향이 되도록 하는 작업이 포함됩니다.

"디스플레이 방향"이라는 용어는 시스템이 장치에서 XAML 페이지를 회전하여 사용자가 볼 때 똑바로 표시되도록 하는 방식을 나타냅니다. "장치 방향"은 월드 공간에서 장치의 방향, 즉 월드 공간에서 실제 카메라 장치의 방향을 나타냅니다. 두 종류의 방향은 모두 미디어 캡처 앱과 관련이 있습니다. 디스플레이 방향을 처리하려면 [**DisplayInformation**](https://msdn.microsoft.com/library/windows/apps/dn264258) 클래스에 대한 페이지 범위 변수를 선언하고 초기화합니다. [
            **DisplayOrientations**](https://msdn.microsoft.com/library/windows/apps/br226142) 형식의 또 다른 변수를 선언하여 디스플레이의 현재 방향을 추적합니다. [
            **SimpleOrientationSensor**](https://msdn.microsoft.com/library/windows/apps/br206400) 변수 및 [**SimpleOrientation**](https://msdn.microsoft.com/library/windows/apps/br206399) 변수를 선언하여 장치 방향을 추적합니다.

[!code-cs[DisplayInformationAndOrientation](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetDisplayInformationAndOrientation)]

다음 도우미 메서드는 [**DisplayInformation.OrientationChanged**](https://msdn.microsoft.com/library/windows/apps/dn264268) 및 [**SimpleOrientationSensor.OrientationChanged**](https://msdn.microsoft.com/library/windows/apps/br206407) 이벤트에 대한 이벤트 처리기를 등록 및 등록 취소하고 현재 방향에 따라 추적 변수를 초기화합니다. 모든 장치에 [**SimpleOrientationSensor**](https://msdn.microsoft.com/library/windows/apps/br206400)가 있는 것은 아니므로 처리기를 등록하거나 현재 방향을 가져오려고 하기 전에 확인이 필요합니다.

[!code-cs[RegisterOrientationEventHandlers](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetRegisterOrientationEventHandlers)]

[!code-cs[UnregisterOrientationEventHandlers](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetUnregisterOrientationEventHandlers)]

**SimpleOrientationSensor.OrientationChanged** 이벤트에 대한 이벤트 처리기에서 장치 방향 변수를 현재 방향으로 업데이트합니다. 장치가 위쪽 또는 아래쪽을 향하는 경우에는 방향을 업데이트하지 않도록 합니다.

[!code-cs[SimpleOrientationChanged](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetSimpleOrientationChanged)]

**DisplayInformation.OrientationChanged** 이벤트에 대한 이벤트 처리기에서 디스플레이 방향 변수를 현재 방향으로 업데이트합니다. 캡처 장치의 비디오 미리 보기가 현재 표시되고 있으면 미리 보기 비디오 스트림의 회전을 업데이트합니다. 다음 섹션에서 **SetPreviewRotationAsync** 도우미 메서드에 대해 설명합니다.

[!code-cs[DisplayOrientationChanged](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetDisplayOrientationChanged)]

## 미디어 캡처 미리 보기 회전 설정

사용자는 해당 모바일 장치의 방향이 변경될 때 UI 컨트롤이 회전할 것으로 기대하므로 U의 텍스트가 세로로 정렬되고 읽을 수 있게 됩니다. 그러나 **CaptureElement** 컨트롤의 경우 사용자는 일반적으로 장치가 회전할 때 비디오 미리 보기의 방향이 회전하는 것을 원하지 않습니다. 예상되는 사용자 환경을 제공하기 위해 장치 방향에 맞게 미리 보기 스트림을 회전해야 합니다.

미리 보기 스트림 방향은 각도로 표시되어야 합니다. 다음 도우미 메서드는 [**DisplayOrientations**](https://msdn.microsoft.com/library/windows/apps/br226142) 열거형 값을 도 단위로 변환합니다.

[!code-cs[ConvertDisplayOrientationToDegrees](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetConvertDisplayOrientationToDegrees)]

이 도우미 메서드는 디바이스의 회전을 표현하기 위해 [**SimpleOrientationSensor**](https://msdn.microsoft.com/library/windows/apps/br206400)에서 사용되는 [**SimpleOrientation**](https://msdn.microsoft.com/library/windows/apps/br206399) 열거형 값을 도 단위로 변환합니다.

[!code-cs[ConvertDeviceOrientationToDegrees](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetConvertDeviceOrientationToDegrees)]

하위 수준에서 스트림의 회전은 실제로 Microsoft 미디어 파운데이션 프레임워크에 의해 수행됩니다. 회전은 [MF\_MT\_VIDEO\_ROTATION](https://msdn.microsoft.com/library/windows/desktop/hh162880) 특성을 사용하여 지정됩니다. 이것은 Windows 런타임 앱이므로 특성 이름 대신 특성에 대한 GUID를 사용하여 회전을 지정합니다. 비디오 회전 특성을 식별하는 다음 GUID를 정의합니다.

[!code-cs[RotationKey](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetRotationKey)]

다음 메서드는 미리 보기 스트림의 회전을 설정합니다. 미디어 캡처 [**VideoDeviceController**](https://msdn.microsoft.com/library/windows/apps/br226825)의 [**GetMediaStreamProperties**](https://msdn.microsoft.com/library/windows/apps/br211995) 메서드는 키/값 쌍으로 이루어진 속성 집합을 반환합니다. [
            **MediaStreamType.VideoPreview**](https://msdn.microsoft.com/library/windows/apps/br226640)는 비디오 녹화 스트림 또는 오디오 스트림과 달리 비디오 미리 보기 스트림에 대한 속성이 필요함을 나타내기 위해 지정됩니다. 이 속성 집합은 스트림 속성을 설정하기 위한 일반 용도의 인터페이스이지만 이 작업의 경우, 위에 정의된 비디오 회전 GUID가 속성 키로 추가되고 비디오 스트림의 원하는 방향이 값(도)으로 지정됩니다. [
            **SetEncodingPropertiesAsync**](https://msdn.microsoft.com/library/windows/apps/dn297781)는 인코딩 속성을 새 값으로 업데이트합니다. 다시 한번, 설정되는 속성이 비디오 미리 보기 스트림에 대한 것임을 나타내기 위해 **MediaStreamType.VideoPreview**가 지정됩니다.

[!code-cs[SetPreviewRotation](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetSetPreviewRotation)]

-   외부 카메라가 있는 장치의 경우 사용자는 장치를 회전할 때 카메라 스트림이 회전할 것으로 기대하지 않습니다.

-   따라서 미리 보기가 전방 패널의 카메라에 대해 미러링되는 경우 회전 방향이 장치의 회전에 맞게 반전되어야 합니다.

-   일부 장치(일반적으로 휴대폰)는 장치에 따라 디스플레이를 강제로 회전하기 위해 [**DisplayInformation.AutoRotationPreferences**](https://msdn.microsoft.com/library/windows/apps/dn264259)를 [**DisplayOrientations.Landscape**](https://msdn.microsoft.com/library/windows/apps/br226142)와 같은 방향 값으로 설정하는 것을 지원합니다. 지원 장치에서는 적절한 환경을 제공하기 때문에 이 값을 설정해야 하지만 자동 회전 기본 설정을 지원하지 않는 장치를 지원하기 위해서는 앱에서 위의 패턴을 여전히 구현해야 합니다.

-   이전 릴리스에서 [**SetPreviewRotation**](https://msdn.microsoft.com/library/windows/apps/br226611) 메서드는 미리 보기 스트림을 회전하는 유일한 방법이었습니다. 이 메서드는 기존 앱을 지원하기 위해 API 화면에 여전히 제공되지만 비효율적이므로 새 앱에는 사용하지 말아야 합니다.

## 사진 캡처

다음 메서드는 [**CapturePhotoToStreamAsync**](https://msdn.microsoft.com/library/windows/apps/hh700840) 메서드를 사용하고 요청된 인코딩 속성과 캡처 작업의 출력이 포함될 [**InMemoryRandomAccessStream**](https://msdn.microsoft.com/library/windows/apps/br241720) 개체를 전달하여 사진을 캡처합니다. [
            **ImageEncodingProperties**](https://msdn.microsoft.com/library/windows/apps/hh700993) 클래스는 [**CreateJpeg**](https://msdn.microsoft.com/library/windows/apps/hh700994) 같은 도우미 메서드를 제공하여 미디어 캡처에서 지원하는 파일 형식에 대한 인코딩 속성을 생성합니다.

[!code-cs[TakePhotoAsync](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetTakePhotoAsync)]

파일에 사진을 저장하기 전에 사진의 올바른 방향을 확인해야 합니다. **MediaCapture** 개체는 장치 방향을 모르므로 캡처 장치가 기본 방향인 것처럼 캡처한 사진 데이터를 인코딩합니다. 따라서 사용자가 캡처한 사진을 볼 때 사진 방향이 올바르지 않은 문제가 발생할 수 있습니다. 다음 도우미 메서드는 올바른 사진 방향을 확인하고 올바른 방향으로 파일을 저장합니다.

**GetCameraOrientation** 도우미 메서드는 현재의 장치 방향으로 시작하고 장치의 기본 방향 및 장치에서의 카메라 위치에 따라 해당 값을 회전합니다. 이 예제에서 **\_mirroringPreview** 변수로 지정된 것처럼 카메라를 디바이스의 전방 패널에 탑재한 경우 카메라 방향이 반전되어야 합니다.

[!code-cs[GetCameraOrientation](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetGetCameraOrientation)]

다음 도우미 메서드는 방향 센서에 사용되는 [**SimpleOrientation**](https://msdn.microsoft.com/library/windows/apps/br206399) 열거형 값을 파일 저장에 사용되는 비트맵 인코더에 사용되는 [**PhotoOrientation**](https://msdn.microsoft.com/library/windows/apps/hh965476) 값으로 간단히 변환합니다.

[!code-cs[ConvertOrientationToPhotoOrientation](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetConvertOrientationToPhotoOrientation)]

마지막으로, 캡처한 사진을 인코딩 및 저장할 수 있습니다. 캡처한 사진 데이터를 포함하는 입력 스트림에서 [**BitmapDecoder**](https://msdn.microsoft.com/library/windows/apps/br226176) 개체를 만듭니다. 새 [**StorageFile**](https://msdn.microsoft.com/library/windows/apps/br227171)을 만들고 읽고 쓰기 위해 엽니다. [
            **BitmapEncoder**](https://msdn.microsoft.com/library/windows/apps/br226206) 개체를 만들고 출력 파일과 이미지 데이터가 포함된 디코더를 전달합니다. 새 [**BitmapPropertySet**](https://msdn.microsoft.com/library/windows/apps/hh974338)을 만들고 새 속성을 추가합니다. "System.Photo.Orientation" 속성에 대한 키는 해당 속성이 사진 방향을 나타냄을 지정합니다. 이 값은 이전에 계산된 [**PhotoOrientation**](https://msdn.microsoft.com/library/windows/apps/hh965476) 값입니다. [
            **SetPropertiesAsync**](https://msdn.microsoft.com/library/windows/apps/br226252)를 호출하여 인코더에 대한 속성을 업데이트하고 [**FlushAsync**](https://msdn.microsoft.com/library/dn237883)를 호출하여 저장소 파일에 사진을 씁니다.

[!code-cs[ReencodeAndSavePhotoAsync](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetReencodeAndSavePhotoAsync)]

-   "System.Photo.Orientation" 비트맵 속성을 설정하여 사진의 방향을 파일의 메타데이터에 인코딩합니다. 이렇게 해도 실제 이미지 데이터가 다르게 인코딩되지 않습니다. 메타데이터를 이미지 파일에 포함하는 방법에 대한 자세한 내용은 [이미지 메타데이터](image-metadata.md)를 참조하세요.

-   이미지 인코딩 및 디코딩을 비롯한 이미지 작업에 대한 자세한 내용은 [이미징](imaging.md)을 참조하세요.

## 비디오 캡처

비디오 캡처를 시작하려면 먼저 비디오가 기록될 저장소 파일을 만듭니다. 다음으로 **MediaCapture**가 비디오를 파일에 인코드하는 데 사용하는 [**MediaEncodingProfile**](https://msdn.microsoft.com/library/windows/apps/hh701026)을 만듭니다. **MediaEncodingProfile** 클래스는 [**CreateMp4**](https://msdn.microsoft.com/library/windows/apps/hh701078)와 같이 지원되는 비디오 형식에 대한 인코딩 프로필을 만드는 메서드를 제공합니다. 설명한 도우미 메서드를 사용하여 비디오의 올바른 회전 각도를 가져옵니다. 사진 시나리오와 달리 비디오 회전 정보는 **MediaCapture**에 의해 스트림으로 인코드됩니다. 회전 정보를 [**VideoEncodingProperties.Properties**](https://msdn.microsoft.com/library/windows/apps/hh701254) 컬렉션에 추가하여 인코딩 프로필에 추가합니다. 비디오 회전에 대해 이전에 정의한 GUID는 키로 사용되며 회전 각도가 해당 값이 됩니다. 마지막으로 [**MediaCapture.StartRecordToStorageFileAsync**](https://msdn.microsoft.com/library/windows/apps/hh700863)를 호출하고 인코딩 속성 및 출력 파일을 지정하여 녹화를 시작합니다.

[!code-cs[StartRecordingAsync](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetStartRecordingAsync)]

녹화를 중지하려면 [**MediaCapture.StopRecordAsync**](https://msdn.microsoft.com/library/windows/apps/br226623)를 호출하면 됩니다.

[!code-cs[StopRecordingAsync](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetStopRecordingAsync)]

[
            **MediaCapture.RecordLimitationExceeded**](https://msdn.microsoft.com/library/windows/apps/hh973312) 이벤트에 대한 처리기는 **MediaCapture**가 초기화될 때 등록되었습니다. 처리기에서 **StopRecordingAsync** 메서드를 호출하여 비디오 녹화를 중지합니다.

[!code-cs[RecordLimitationExceeded](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetRecordLimitationExceeded)]

## 비디오 캡처 일시 중지 및 다시 시작

일부 시나리오에서는 캡처를 중지한 후 새 캡처를 시작하는 것보다 진행 중인 캡처를 일시 중지했다가 다시 시작하려고 할 수 있습니다. 녹화를 일시 중지하려면 [**PauseRecordAsync**](https://msdn.microsoft.com/library/windows/apps/dn858102)를 호출합니다. [
            **MediaCapturePauseBehavior.RetainHardwareResources**](https://msdn.microsoft.com/library/windows/apps/dn926686)를 지정하는 경우 동시 비디오 및 사진 캡처를 지원하지 않는 장치에서는 비디오가 일시 중지된 동안 앱이 사진을 캡처할 수 없습니다. 장치가 동시 사진 및 비디오 캡처를 지원하는지 확인하는 방법에 대한 자세한 내용은 [카메라 프로필](camera-profiles.md)을 참조하세요.

[!code-cs[PauseRecordingAsync](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetPauseRecordingAsync)]

이전에 일시 중지된 비디오 캡처를 다시 시작하려면 [**ResumeRecordAsync**](https://msdn.microsoft.com/library/windows/apps/dn858103)를 호출합니다.

[!code-cs[ResumeRecordingAsync](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetResumeRecordingAsync)]

## 캡처 리소스 정리

미디어 캡처 리소스를 적절히 종료하고 해제하는 것은 매우 중요합니다. 이렇게 하지 않으면 앱이 닫힌 후에 카메라에게 예기치 않은 동작이 발생하여 사용자에게 짜증을 유발할 수 있습니다. 다음 섹션에서는 카메라를 종료하기 위해 수행해야 하는 여러 다른 작업 단계를 안내합니다.

### 캡처 미리 보기 종료

캡처 미리 보기를 종료하려면 먼저 [**MediaCapture.StopPreviewAsync**](https://msdn.microsoft.com/library/windows/apps/br226622)를 호출합니다. [
            **MediaElement**](https://msdn.microsoft.com/library/windows/apps/br242926)의 [**Source**](https://msdn.microsoft.com/library/windows/apps/br227419) 속성을 null로 설정합니다. 그런 다음 [**DisplayRequest**](https://msdn.microsoft.com/library/windows/apps/br241816) 변수에 대해 [**RequestRelease**](https://msdn.microsoft.com/library/windows/apps/br241819)를 호출하여 필요할 때 시스템 디스플레이가 꺼질 수 있도록 합니다.

[!code-cs[StopPreviewAsync](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetStopPreviewAsync)]

-   UI가 아닌 스레드에서 UI에 액세스할 수 없으므로 **MediaElement.Source** 속성을 설정하고 **RequestRelease**를 호출하는 작업은 UI 스레드에 대해 실행되도록 [**CoreDispatcher.RunAsync**](https://msdn.microsoft.com/library/windows/apps/hh750317) 메서드를 사용하여 수행되어야 합니다.

### MediaCapture 개체 종료 및 해제

**MediaCapture** 개체를 해제하기 전에 이전에 정의된 도우미 메서드를 호출하여 진행 중인 모든 녹화를 중지하고 미리 보기 스트림을 중지합니다. 이 작업이 완료되면 **MediaCapture**에 등록된 모든 이벤트 처리기를 제거한 다음 [**Dispose**](https://msdn.microsoft.com/library/windows/apps/dn278858)를 호출하여 개체와 관련된 리소스를 해제하고 **MediaCapture** 변수를 null로 설정합니다.

[!code-cs[CleanupCameraAsync](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetCleanupCameraAsync)]

이 메서드를 호출하여 [**MediaCapture.Failed**](https://msdn.microsoft.com/library/windows/apps/br226593) 이벤트에 대한 처리기 내에서 **MediaCapture** 개체를 종료하고 해제해야 합니다.

[!code-cs[CaptureFailed](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetCaptureFailed)]

### 앱 수명 및 페이지 탐색 이벤트 처리

앱 수명 이벤트는 앱에서 리소스를 초기화하고 해제할 수 있는 기회를 제공합니다. 이러한 기능은 앱이 미디어 캡처 리소스를 제대로 해제해야 하는 **Application.Suspending** 이벤트에서 특히 중요합니다.

페이지의 생성자에서 응용 프로그램 수명 이벤트에 대한 처리기를 등록할 수 있습니다.

[!code-cs[RegisterAppLifetimeEvents](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetRegisterAppLifetimeEvents)]

**Application.Suspending** 이벤트에 대한 처리기에서 디스플레이 및 장치 방향 이벤트에 대한 처리기를 등록 취소하고 **MediaCapture** 개체를 종료해야 합니다. 여기에 등록된 [**SystemMediaTransportControls.PropertyChanged**](https://msdn.microsoft.com/library/windows/apps/dn278720) 이벤트는 이 문서 뒷부분에 설명된 다른 응용 프로그램 수명 주기 관련 작업에 필요합니다.

**주의** 
일시 중단된 이벤트 처리기의 맨 앞에서 [**SuspendingOperation.GetDeferral**](https://msdn.microsoft.com/library/windows/apps/br224690)을 호출하여 일시 중단 지연을 요청해야 합니다. 이를 위해 앱을 닫기 전에 사용자가 작업이 완료되었다는 신호를 보낼 때까지 시스템이 대기해야 합니다. 이 작업은 **MediaCapture** 종료 작업이 비동기식이므로 **Application.Suspending** 이벤트 처리기가 카메라의 올바른 종료 전에 완료되어야 하기 때문에 필요합니다. 비동기 호출 완료를 기다린 후에 [**SuspendingDeferral.Complete**](https://msdn.microsoft.com/library/windows/apps/br224685)를 호출하여 지연을 해제해야 합니다.

[!code-cs[Suspending](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetSuspending)]

**Application.Resuming** 이벤트에 대한 처리기에서 디스플레이 및 장치 방향 이벤트에 대한 처리기를 등록하고, **SystemMediaTransportControls.PropertyChanged** 이벤트를 등록하고, **MediaCapture** 개체를 초기화해야 합니다.

[!code-cs[Resuming](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetResuming)]

[
            **OnNavigatedTo**](https://msdn.microsoft.com/library/windows/apps/br227508) 이벤트는 초기에 디스플레이 및 디바이스 방향 이벤트에 대한 처리기를 등록할 기회를 주며 **MediaCapture** 개체를 초기화합니다.

[!code-cs[OnNavigatedTo](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetOnNavigatedTo)]

앱에 여러 페이지가 포함된 경우 [**OnNavigatingFrom**](https://msdn.microsoft.com/library/windows/apps/br227509) 이벤트 처리기에서 미디어 캡처 개체를 정리해야 합니다.

[!code-cs[OnNavigatingFrom](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetOnNavigatingFrom)]

여러 개의 동시 Windows를 지원하는 장치에서 앱이 제대로 작동하려면 앱이 초기화되거나 복원될 때 응답해야 합니다. 이 작업을 수행하려면 [**SystemMediaTransportControls.PropertyChanged**](https://msdn.microsoft.com/library/windows/apps/dn278720) 이벤트를 처리해야 합니다. 멤버 변수를 초기화하여 앱용 [**SystemMediaTransportControls**](https://msdn.microsoft.com/library/windows/apps/dn278677) 개체에 대한 참조를 저장합니다.

[!code-cs[DeclareSystemMediaTransportControls](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetDeclareSystemMediaTransportControls)]

**PropertyChanged** 이벤트를 등록 및 등록 취소하는 코드는 위 예제에 표시된 대로 앱 수명 주기 이벤트에 추가되어야 합니다. 이벤트 처리기에서 이벤트를 트리거한 속성 변경이 [**SystemMediaTransportControlsProperty.SoundLevel**](https://msdn.microsoft.com/library/windows/apps/dn278721) 속성인지 확인합니다. 이 속성이 변경된 속성이면 해당 속성의 값을 확인합니다. 값이 [**SoundLevel.Muted**](https://msdn.microsoft.com/library/windows/apps/hh700852)이면 앱이 최소화되었으며 그에 따라 미디어 캡처 리소스를 적절히 정리해야 합니다. 그렇지 않은 경우 이벤트는 앱 창이 복원되었음을 신호로 알립니다. 이 경우 사용자는 미디어 캡처 리소스를 다시 초기화해야 합니다. **SoundLevel** 속성은 UI 스레드에서 액세스되어야 하므로 이 메서드의 코드는 [**Dispatcher.RunAsync**](https://msdn.microsoft.com/library/windows/apps/hh750317) 호출에 래핑됩니다.

[!code-cs[SystemMediaControlsPropertyChanged](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetSystemMediaControlsPropertyChanged)]

## 미디어 캡처에 대한 추가 UI 고려 사항

### 자동 회전 기본 설정 지정

미리 보기 스트림 회전에 대한 이전 섹션에서 언급한 것처럼 일부 장치는 장치가 회전할 때 미리 보기를 표시하는 [**CaptureElement**](https://msdn.microsoft.com/library/windows/apps/br209278)를 비롯한 페이지가 회전하지 않도록 하기 위해 [**DisplayInformation.AutoRotationPreferences**](https://msdn.microsoft.com/library/windows/apps/dn264259)를 설정하는 것을 지원합니다. 이 기능을 지원하는 장치에서 사용자는 편리하게 작업할 수 있습니다. 앱이 시작될 때 또는 미리 보기를 표시하기 시작할 때 이 값을 설정합니다. 자동 회전 기본 설정을 지원하지 않는 장치의 경우 미리 보기 회전 처리를 계속 구현해야 합니다.

[!code-cs[SetAutoRotationPreferences](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetSetAutoRotationPreferences)]

### UI 요소 방향 처리

사용자는 일반적으로 카메라 앱에서 사진 또는 비디오 캡처를 시작하기 위한 단추와 같은 UI 요소가 비디오 미리 보기에 따라 회전할 것으로 기대합니다. 다음 메서드에서는 이전에 정의한 방향 도우미 메서드를 사용하여 카메라 UI에서 단추 방향을 적절히 지정합니다. 앱이 처음 시작될 때 [**DisplayInformation.OrientationChanged**](https://msdn.microsoft.com/library/windows/apps/dn264268) 및 [**SimpleOrientationSensor.OrientationChanged**](https://msdn.microsoft.com/library/windows/apps/br206407) 이벤트 처리기에서 이 메서드를 호출해야 합니다. 구현은 앱의 UI에 따라 달라집니다.

[!code-cs[UpdateButtonOrientation](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetUpdateButtonOrientation)]

앱이 종료되고 있거나 미디어 캡처와 관련이 없는 페이지로 이동하는 경우 자동 회전 기본 설정을 [**None**](https://msdn.microsoft.com/library/windows/apps/br226142)으로 설정하여 UI가 정상적으로 회전할 수 있도록 합니다.

[!code-cs[RevertAutoRotationPreferences](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetRevertAutoRotationPreferences)]

### 사진 및 비디오 동시 캡처

[
            **Windows.Media.Capture**](https://msdn.microsoft.com/library/windows/apps/br226738) API를 사용하여 해당 기능을 지원하는 장치에서 사진 및 비디오를 동시에 캡처할 수 있습니다. 편의를 위해 이 예에서는 [**ConcurrentRecordAndPhotoSupported**](https://msdn.microsoft.com/library/windows/apps/dn278843) 속성을 사용하여 동시 비디오 및 사진 캡처가 지원되는지 확인하지만, 카메라 프로필을 사용하는 방법이 더욱 강력하고 좋습니다. 자세한 내용은 [카메라 프로필](camera-profiles.md)을 참조하세요.

다음 도우미 메서드는 앱의 현재 캡처 상태에 맞게 앱의 컨트롤을 업데이트합니다. 비디오 캡처가 시작되거나 중지될 때처럼 앱의 캡처 상태가 변경될 때마다 이 메서드를 호출합니다.

[!code-cs[UpdateCaptureControls](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetUpdateCaptureControls)]

### 모바일 전용 UI 기능 지원

이 문서에 나오는 모든 코드는 유니버설 Windows 앱에서 작동합니다. 코드 몇 줄만 추가하여 모바일 장치에만 존재하는 특수한 UI 기능을 활용할 수 있습니다. 이러한 기능을 사용하려면 유니버설 앱 플랫폼용 Microsoft 모바일 확장 SDK에 대한 참조를 프로젝트에 추가해야 합니다.

**하드웨어 카메라 단추 지원에 위해 모바일 확장 SDK에 대한 참조를 추가하려면**

1.  **솔루션 탐색기**에서 **참조**를 마우스 오른쪽 버튼으로 클릭한 다음 **참조 추가...**를 선택합니다.

2.  **Windows 유니버설** 노드를 확장하고 **확장**을 선택합니다.

3.  옆에 있는 확인란을 클릭**유니버설 앱 플랫폼용 Microsoft 모바일 확장 SDK** 옆의 확인란을 클릭합니다.

모바일 장치에는 사용자에게 장치에 대한 상태 정보를 제공하는 [**StatusBar**](https://msdn.microsoft.com/library/windows/apps/dn633864) 컨트롤이 있습니다. 이 컨트롤은 화면에서 공간을 차지하여 미디어 캡처 UI를 방해할 수 있습니다. [
            **HideAsync**](https://msdn.microsoft.com/library/windows/apps/dn610339)를 호출하여 상태 표시줄을 숨길 수 있지만 [**ApiInformation.IsTypePresent**](https://msdn.microsoft.com/library/windows/apps/dn949016) 메서드를 사용하여 API를 사용할 수 있는지 확인하는 조건부 블록 내에서 이 호출을 수행해야 합니다. 이 메서드는 상태 표시줄을 지원하는 모바일 장치에서만 true를 반환합니다. 앱이 시작되거나 카메라에서 미리 보기를 시작할 때 상태 표시줄을 숨겨야 합니다.

[!code-cs[HideStatusBar](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetHideStatusBar)]

앱이 종료되거나 사용자가 앱의 미디어 캡처 페이지에서 외부로 이동하면 이 컨트롤을 다시 보이게 합니다.

[!code-cs[ShowStatusBar](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetShowStatusBar)]

일부 모바일 장치에는 일부 사용자가 화면 컨트롤보다 선호하는 전용 하드웨어 카메라 단추가 있습니다. 하드웨어 카메라 단추를 누를 때 알림을 받으려면 [**HardwareButtons.CameraPressed**](https://msdn.microsoft.com/library/windows/apps/dn653805) 이벤트에 대한 처리기를 등록합니다. 이 API는 모바일 장치에서만 사용할 수 있으므로 **IsTypePresent**를 사용하여 액세스를 시도하기 전에 API가 현재 장치에서 지원되는지를 확인해야 합니다.

[!code-cs[PhoneUsing](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetPhoneUsing)]

[!code-cs[RegisterCameraButtonHandler](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetRegisterCameraButtonHandler)]

**CameraPressed** 이벤트에 대한 처리기에서 사진 캡처를 시작할 수 있습니다.

[!code-cs[CameraPressed](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetCameraPressed)]

앱이 종료되거나 사용자가 앱의 미디어 캡처 페이지를 벗어나 이동하면 하드웨어 단추 처리기를 등록 취소합니다.

[!code-cs[UnregisterCameraButtonHandler](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetUnregisterCameraButtonHandler)]

**참고** 이 문서는 UWP(유니버설 Windows 플랫폼) 앱을 작성하는 Windows 10 개발자용입니다. Windows 8.x 또는 Windows Phone 8.x를 개발하는 경우 [보관된 문서](http://go.microsoft.com/fwlink/p/?linkid=619132)를 참조하세요.

## 관련 항목

* [카메라 프로필](camera-profiles.md)
* [HDR(High Dynamic Range) 사진 캡처](high-dynamic-range-hdr-photo-capture.md)
* [사진 및 비디오 캡처를 위한 캡처 디바이스 컨트롤](capture-device-controls-for-photo-and-video-capture.md)
* [비디오 캡처를 위한 캡처 장치 컨트롤](capture-device-controls-for-video-capture.md)
* [비디오 캡처 효과](effects-for-video-capture.md)
* [미디어 캡처의 장면 분석](scene-analysis-for-media-capture.md)
* [가변 사진 시퀀스](variable-photo-sequence.md)
* [미리 보기 프레임 가져오기](get-a-preview-frame.md)
* [CameraStarterKit 샘플](http://go.microsoft.com/fwlink/?LinkId=619479)


<!--HONumber=Mar16_HO5-->


