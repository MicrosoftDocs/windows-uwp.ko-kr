---
author: drewbatgit
ms.assetid: 9BA3F85A-970F-411C-ACB1-B65768B8548A
description: 이 문서에서는 UWP(유니버설 Windows 플랫폼) 앱에서 XAML 페이지 내의 카메라 미리 보기 스트림을 빠르게 표시하는 방법을 설명합니다.
title: 카메라 미리 보기 표시
ms.author: drewbat
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: b258569b707f50051eaa266e2cae5b9971894cf3
ms.sourcegitcommit: 70ab58b88d248de2332096b20dbd6a4643d137a4
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/02/2018
ms.locfileid: "5947118"
---
# <a name="display-the-camera-preview"></a>카메라 미리 보기 표시


이 문서에서는 UWP(유니버설 Windows 플랫폼) 앱에서 XAML 페이지 내의 카메라 미리 보기 스트림을 빠르게 표시하는 방법을 설명합니다. 카메라를 사용하여 사진 및 동영상을 캡처하는 앱을 만들려면 디바이스 및 카메라 방향을 다루거나 캡처된 파일에 대한 인코딩 옵션을 설정하는 것과 같은 작업을 수행해야 합니다. 일부 앱 시나리오의 경우 다른 고려 사항에 신경 쓰지 않고 카메라에서 미리 보기 스트림을 간단히 표시하고자 할 수 있습니다. 이 문서에서는 최소한의 코드로 이 작업을 수행하는 방법을 보여 줍니다. 아래 단계에 따라 작업을 완료했을 때 미리 보기 스트림을 항상 제대로 종료해야 합니다.

사진 또는 동영상을 캡처하는 카메라 앱을 작성하는 방법은 [MediaCapture를 사용하여 기본 사진, 동영상 및 오디오 캡처](basic-photo-video-and-audio-capture-with-MediaCapture.md)를 참조하세요.

## <a name="add-capability-declarations-to-the-app-manifest"></a>앱 매니페스트에 접근 권한 값 선언 추가

앱에서 디바이스의 카메라에 액세스해야 하는 경우 앱에 *웹캠* 및 *마이크* 디바이스 접근 권한 값이 사용된다고 선언해야 합니다. 

**앱 매니페스트에 접근 권한 값 추가**

1.  Microsoft Visual Studio의 **솔루션 탐색기**에서 **package.appxmanifest** 항목을 두 번 클릭하여 응용 프로그램 매니페스트 디자이너를 엽니다.
2.  **접근 권한 값** 탭을 선택합니다.
3.  **웹캠** 확인란과 **마이크** 상자를 선택합니다.

## <a name="add-a-captureelement-to-your-page"></a>CaptureElement를 페이지에 추가

[**CaptureElement**](https://msdn.microsoft.com/library/windows/apps/br209278)를 사용하여 XAML 페이지 내의 미리 보기 스트림을 표시합니다.

[!code-xml[CaptureElement](./code/SimpleCameraPreview_Win10/cs/MainPage.xaml#SnippetCaptureElement)]



## <a name="use-mediacapture-to-start-the-preview-stream"></a>MediaCapture를 사용하여 미리 보기 스트림 시작

[**MediaCapture**](https://msdn.microsoft.com/library/windows/apps/br241124) 개체는 디바이스의 카메라에 대한 앱 인터페이스입니다. 이 클래스는 Windows.Media.Capture 네임스페이스의 멤버입니다. 이 문서의 예제에서는 기본 프로젝트 템플릿에 포함된 API뿐만 아니라 [**Windows.ApplicationModel**](https://msdn.microsoft.com/library/windows/apps/br224691) 및 [System.Threading.Tasks](https://msdn.microsoft.com/library/windows/apps/xaml/system.threading.tasks.aspx)의 API도 사용합니다.

using 지시문을 추가하여 페이지의 .cs 파일에 다음 네임스페이스를 포함합니다.

[!code-cs[SimpleCameraPreviewUsing](./code/SimpleCameraPreview_Win10/cs/MainPage.xaml.cs#SnippetSimpleCameraPreviewUsing)]

**MediaCapture** 개체에 대한 클래스 멤버 변수와 카메라가 현재 미리 보기 중인지 추적하는 부울을 선언합니다. 

[!code-cs[DeclareMediaCapture](./code/SimpleCameraPreview_Win10/cs/MainPage.xaml.cs#SnippetDeclareMediaCapture)]

미리 보기가 실행되는 동안 디스플레이가 꺼지지 않도록 하는 데 사용되는 형식 [**DisplayRequest**](https://msdn.microsoft.com/library/windows/apps/Windows.System.Display.DisplayRequest)의 변수를 선언합니다.

[!code-cs[DeclareDisplayRequest](./code/SimpleCameraPreview_Win10/cs/MainPage.xaml.cs#SnippetDeclareDisplayRequest)]

이 예에서는 **StartPreviewAsync**라는 도우미 메서드를 생성하여 카메라 미리 보기를 시작합니다. 앱의 시나리오에 따라 페이지가 로드될 때 호출되는 **OnNavigatedTo** 이벤트 처리기에서 이를 호출하거나, 대기한 후 UI 이벤트에 대한 응답으로 미리 보기를 시작할 수 있습니다.

**MediaCapture** 클래스의 새 인스턴스를 만들고 [**InitializeAsync**](https://msdn.microsoft.com/library/windows/apps/br226598)를 호출하여 캡처 디바이스를 초기화합니다. 예를 들어 카메라가 없는 디바이스에서는 이 메서드가 실패할 수 있으므로 **try** 블록 내에서 호출해야 합니다. 사용자가 디바이스의 개인 정보 설정에서 카메라 액세스를 사용하지 않도록 설정한 경우 카메라를 초기화하려고 할 때 **UnauthorizedAccessException**이 발생합니다. 개발하는 동안 앱 매니페스트에 적절한 기능을 추가하지 않으려는 경우에도 이 예외가 발생합니다.

**중요** 일부 디바이스 패밀리에서는 앱에 디바이스의 카메라에 대한 액세스가 허용되기 전에 사용자 동의 확인 프롬프트가 사용자에게 표시됩니다. 이러한 이유로 기본 UI 스레드에서만 [**MediaCapture.InitializeAsync**](https://msdn.microsoft.com/library/windows/apps/br226598)를 호출해야 합니다. 다른 스레드에서 카메라를 초기화하려고 하면 초기화 오류가 발생할 수 있습니다.

[**Source**](https://msdn.microsoft.com/library/windows/apps/br209280) 속성을 설정하여 **MediaCapture**를 **CaptureElement**에 연결합니다. [**StartPreviewAsync**](https://msdn.microsoft.com/library/windows/apps/br226613)를 호출하여 미리 보기를 시작합니다. 이 이렇게 하면 **FileLoadException** 다른 앱이 캡처 장치의 단독 제어를 가지게 됩니다. 단독 제어의 변경에 대한 정보는 다음 섹션을 참조하세요.

미리 보기가 실행되는 동안 디바이스가 절전 모드로 전환되지 않도록 [**RequestActive**](https://msdn.microsoft.com/library/windows/apps/Windows.System.Display.DisplayRequest.RequestActive)를 호출합니다. 마지막으로 사용자가 디바이스 방향을 변경할 때 UI 및 **CaptureElement**가 회전되지 않도록 [**DisplayInformation.AutoRotationPreferences**](https://msdn.microsoft.com/library/windows/apps/Windows.Graphics.Display.DisplayInformation.AutoRotationPreferences) 속성을 [**Landscape**](https://msdn.microsoft.com/library/windows/apps/Windows.Graphics.Display.DisplayOrientations)로 설정합니다. 디바이스 방향 변경 처리에 대한 자세한 내용은 [**MediaCapture를 사용하여 디바이스 방향 처리**](handle-device-orientation-with-mediacapture.md)를 참조하세요.  

[!code-cs[StartPreviewAsync](./code/SimpleCameraPreview_Win10/cs/MainPage.xaml.cs#SnippetStartPreviewAsync)]

## <a name="handle-changes-in-exclusive-control"></a>단독 제어의 변경 내용 처리
이전 섹션에 명시된 대로 **StartPreviewAsync**는 다른 앱이 캡처 장치의 단독 제어를 가지는 경우 **FileLoadException**을 throw합니다. Windows 10 버전 1703부터 디바이스의 단독 제어 상태가 변경될 때마다 발생하는 [MediaCapture.CaptureDeviceExclusiveControlStatusChanged](https://docs.microsoft.com/uwp/api/Windows.Media.Capture.MediaCapture.CaptureDeviceExclusiveControlStatusChanged) 이벤트에 대한 처리기를 등록할 수 있습니다. 이 이벤트에 대한 처리기에서 [MediaCaptureDeviceExclusiveControlStatusChangedEventArgs.Status](https://docs.microsoft.com/uwp/api/windows.media.capture.mediacapturedeviceexclusivecontrolstatuschangedeventargs.Status) 속성을 확인하여 현재 상태를 확인합니다. 새 상태가 **SharedReadOnlyAvailable**인 경우 미리 보기를 시작할 수 없음을 알 수 있으며 사용자에게 경고하기 위해 UI를 업데이트할 수 있습니다. 새 상태가 **ExclusiveControlAvailable**인 경우 카메라 미리 보기를 다시 시작해 봅니다.

[!code-cs[ExclusiveControlStatusChanged](./code/SimpleCameraPreview_Win10/cs/MainPage.xaml.cs#SnippetExclusiveControlStatusChanged)]

## <a name="shut-down-the-preview-stream"></a>미리 보기 스트림 종료

미리 보기 스트림 사용을 완료했을 때 디바이스의 다른 앱에서 카메라를 사용할 수 있도록 항상 스트림을 종료하고 관련된 리소스를 제대로 해제해야 합니다. 미리 보기 스트림은 다음 단계에 따라 종료해야 합니다.

-   카메라가 현재 미리 보기 상태인 경우 미리 보기 스트림을 중지하려면 [**StopPreviewAsync**](https://msdn.microsoft.com/library/windows/apps/br226622)를 호출합니다. 미리 보기가 실행 중이 아닐 때 **StopPreviewAsync**를 호출하면 예외가 발생합니다.
-   **CaptureElement**의 [**Source**](https://msdn.microsoft.com/library/windows/apps/br209280) 속성을 null로 설정합니다. 이 호출이 UI 스레드에서 실행되도록 하려면 [**CoreDispatcher.RunAsync**](https://msdn.microsoft.com/library/windows/apps/windows.ui.core.coredispatcher.runasync.aspx)를 사용합니다.
-   **MediaCapture** 개체의 [**Dispose**](https://msdn.microsoft.com/library/windows/apps/dn278858) 메서드를 호출하여 해당 개체를 해제합니다. 다시, 이 호출이 UI 스레드에서 실행되도록 하려면 [**CoreDispatcher.RunAsync**](https://msdn.microsoft.com/library/windows/apps/windows.ui.core.coredispatcher.runasync.aspx)를 사용합니다.
-   **MediaCapture** 멤버 변수를 null로 설정합니다.
-   비활성 상태일 때 화면이 꺼지도록 하려면 [**RequestRelease**](https://msdn.microsoft.com/library/windows/apps/Windows.System.Display.DisplayRequest.RequestRelease)를 호출합니다.

[!code-cs[CleanupCameraAsync](./code/SimpleCameraPreview_Win10/cs/MainPage.xaml.cs#SnippetCleanupCameraAsync)]

사용자가 페이지에서 벗어날 때 [**OnNavigatedFrom**](https://msdn.microsoft.com/library/windows/apps/br227507) 메서드를 재정의하여 미리 보기 스트림을 종료해야 합니다.

[!code-cs[OnNavigatedFrom](./code/SimpleCameraPreview_Win10/cs/MainPage.xaml.cs#SnippetOnNavigatedFrom)]

앱을 일시 중단하는 경우에도 미리 보기 스트림을 제대로 종료해야 합니다. 이렇게 하려면 페이지 생성자에 [**Application.Suspending**](https://msdn.microsoft.com/library/windows/apps/br205860) 이벤트에 대한 처리기를 등록합니다.

[!code-cs[RegisterSuspending](./code/SimpleCameraPreview_Win10/cs/MainPage.xaml.cs#SnippetRegisterSuspending)]

**Suspending** 이벤트 처리기에서 먼저 페이지 유형과 [**CurrentSourcePageType**](https://msdn.microsoft.com/library/windows/apps/hh702390) 속성을 비교하여 응용 프로그램의 [**Frame**](https://msdn.microsoft.com/library/windows/apps/br242682)이 페이지에 표시되고 있는지 확인해야 합니다. 현재 페이지에 표시되지 않는 경우 **OnNavigatedFrom** 이벤트가 이미 발생한 상태이므로 미리 보기 스트림을 종료합니다. 현재 페이지에 표시되는 경우 처리기에 전달된 이벤트 인수에서 [**SuspendingDeferral**](https://msdn.microsoft.com/library/windows/apps/br224684) 개체를 가져와서 미리 보기 스트림이 종료될 때까지 시스템에서 앱을 일시 중단하지 않도록 해야 합니다. 스트림을 종료한 후 지연의 [**Complete**](https://msdn.microsoft.com/library/windows/apps/br224685) 메서드를 호출하여 시스템에서 앱을 일시 중단하는 작업을 계속 진행하도록 합니다.

[!code-cs[SuspendingHandler](./code/SimpleCameraPreview_Win10/cs/MainPage.xaml.cs#SnippetSuspendingHandler)]


## <a name="related-topics"></a>관련 항목

* [카메라](camera.md)
* [MediaCapture를 사용하여 기본적인 사진, 비디오 및 오디오 캡처](basic-photo-video-and-audio-capture-with-MediaCapture.md)
* [미리 보기 프레임 가져오기](get-a-preview-frame.md)
