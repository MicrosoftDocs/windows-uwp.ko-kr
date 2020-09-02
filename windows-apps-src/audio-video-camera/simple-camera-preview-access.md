---
ms.assetid: 9BA3F85A-970F-411C-ACB1-B65768B8548A
description: 이 문서에서는 유니버설 Windows 플랫폼 (UWP) 앱의 XAML 페이지 내에서 카메라 미리 보기 스트림을 신속 하 게 표시 하는 방법을 설명 합니다.
title: 카메라 미리 보기 표시
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 3487e79b689e5c47cc94ffc29a559a333fe66f47
ms.sourcegitcommit: c3ca68e87eb06971826087af59adb33e490ce7da
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 09/02/2020
ms.locfileid: "89363776"
---
# <a name="display-the-camera-preview"></a>카메라 미리 보기 표시


이 문서에서는 유니버설 Windows 플랫폼 (UWP) 앱의 XAML 페이지 내에서 카메라 미리 보기 스트림을 신속 하 게 표시 하는 방법을 설명 합니다. 카메라를 사용 하 여 사진과 비디오를 캡처하는 앱을 만들려면 장치 및 카메라 방향을 처리 하거나 캡처된 파일의 인코딩 옵션을 설정 하는 등의 작업을 수행 해야 합니다. 일부 앱 시나리오의 경우 이러한 기타 고려 사항에 대해 걱정 하지 않고 카메라의 미리 보기 스트림만 표시 하는 것이 좋습니다. 이 문서에서는 최소한의 코드를 사용 하 여이 작업을 수행 하는 방법을 보여 줍니다. 이 작업을 완료 한 후에는 다음 단계에 따라 미리 보기 스트림을 정상적으로 종료 해야 합니다.

사진 또는 비디오를 캡처하는 카메라 앱을 작성 하는 방법에 대 한 자세한 내용은 [MediaCapture를 사용 하 여 기본 사진, 비디오 및 오디오 캡처](basic-photo-video-and-audio-capture-with-MediaCapture.md)를 참조 하세요.

## <a name="add-capability-declarations-to-the-app-manifest"></a>앱 매니페스트에 기능 선언 추가

앱이 장치 카메라에 액세스 하려면 앱에서 *웹캠* 및 *마이크* 장치 기능을 사용 하도록 선언 해야 합니다. 

**앱 매니페스트에 기능 추가**

1.  Microsoft Visual Studio의 **솔루션 탐색기**에서 **package.appxmanifest** 항목을 두 번 클릭하여 응용 프로그램 매니페스트 디자이너를 엽니다.
2.  **기능** 탭을 선택합니다.
3.  **웹캠** 확인란과 **마이크** 상자를 선택합니다.

## <a name="add-a-captureelement-to-your-page"></a>페이지에 CaptureElement 추가

[**CaptureElement**](/uwp/api/Windows.UI.Xaml.Controls.CaptureElement) 를 사용 하 여 XAML 페이지 내에 미리 보기 스트림을 표시 합니다.

:::code language="xml" source="~/../snippets-windows/windows-uwp/audio-video-camera/SimpleCameraPreview_Win10/cs/MainPage.xaml" id="SnippetCaptureElement":::



## <a name="use-mediacapture-to-start-the-preview-stream"></a>MediaCapture를 사용 하 여 미리 보기 스트림 시작

[**MediaCapture**](/uwp/api/Windows.Media.Capture.MediaCapture) 개체는 장치의 카메라에 대 한 앱의 인터페이스입니다. 이 클래스는 Windows. Media. Capture 네임 스페이스의 멤버입니다. 이 문서의 예제에서는 기본 프로젝트 템플릿에 포함 [된 네임 스페이스](/dotnet/api/system.threading.tasks) 외에도 [**Windows applicationmodel**](/uwp/api/Windows.ApplicationModel) 및 System.xml 네임 스페이스의 api를 사용 합니다.

Using 지시문을 추가 하 여 페이지의 .cs 파일에 다음 네임 스페이스를 포함 합니다.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/SimpleCameraPreview_Win10/cs/MainPage.xaml.cs" id="SnippetSimpleCameraPreviewUsing":::

**MediaCapture** 개체에 대 한 클래스 멤버 변수를 선언 하 고 카메라가 현재 미리 보기 중인지 여부를 추적 하는 부울을 선언 합니다. 

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/SimpleCameraPreview_Win10/cs/MainPage.xaml.cs" id="SnippetDeclareMediaCapture":::

미리 보기가 실행 되는 동안 디스플레이가 해제 되지 않도록 하는 데 사용 되는 [**Displayrequest**](/uwp/api/Windows.System.Display.DisplayRequest) 형식의 변수를 선언 합니다.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/SimpleCameraPreview_Win10/cs/MainPage.xaml.cs" id="SnippetDeclareDisplayRequest":::

이 예제에서 **StartPreviewAsync** 이라는 카메라 미리 보기를 시작 하는 도우미 메서드를 만듭니다. 앱의 시나리오에 따라 페이지를 로드 하거나 대기 하 고 UI 이벤트에 대 한 응답으로 미리 보기를 시작할 때 호출 되는 **OnNavigatedTo** 이벤트 처리기에서 호출 하는 것이 좋습니다.

**MediaCapture** 클래스의 새 인스턴스를 만들고 [**InitializeAsync**](/uwp/api/windows.media.capture.mediacapture.initializeasync) 를 호출 하 여 캡처 장치를 초기화 합니다. 예를 들어 카메라를 포함 하지 않는 장치에서는이 메서드가 실패할 수 있으므로 **try** 블록 내에서 호출 해야 합니다. 사용자가 장치의 개인 정보 설정에서 카메라 액세스를 사용 하지 않도록 설정한 경우 카메라를 초기화 하려고 하면 **system.unauthorizedaccessexception** 이 throw 됩니다. 응용 프로그램 매니페스트에 적절 한 기능을 추가 하는 작업을 중단 한 경우 개발 중에도이 예외가 표시 됩니다.

**중요** 일부 장치 제품군에서는 앱에 장치 카메라에 대 한 액세스 권한이 부여 되기 전에 사용자 동의 프롬프트가 사용자에 게 표시 됩니다. 따라서 주 UI 스레드에서만 [**MediaCapture.InitializeAsync**](/uwp/api/windows.media.capture.mediacapture.initializeasync) 를 호출 해야 합니다. 다른 스레드에서 카메라를 초기화 하려고 하면 초기화 오류가 발생할 수 있습니다.

[**Source**](/uwp/api/windows.ui.xaml.controls.captureelement.source) 속성을 설정 하 여 **MediaCapture** 을 **CaptureElement** 에 연결 합니다. [**StartPreviewAsync**](/uwp/api/windows.media.capture.mediacapture.startpreviewasync)를 호출 하 여 미리 보기를 시작 합니다. 다른 앱에 캡처 장치에 대 한 독점적 제어 권한이 있는 경우이 메서드는 **FileLoadException** 을 throw 합니다. 단독 제어의 변경 내용에 대 한 수신 대기 정보는 다음 섹션을 참조 하세요.

미리 보기가 실행 되는 동안 장치가 절전 모드로 전환 되지 않도록 [**Requestactive**](/uwp/api/windows.system.display.displayrequest.requestactive) 를 호출 합니다. 마지막으로 DisplayInformation 속성을 [**displayproperties.autorotationpreferences**](/uwp/api/windows.graphics.display.displayinformation.autorotationpreferences) [**로 설정 하 여**](/uwp/api/Windows.Graphics.Display.DisplayOrientations) 사용자가 장치 방향을 변경할 때 UI 및 **CaptureElement** 회전 하지 않도록 합니다. 장치 방향 변경을 처리 하는 방법에 대 한 자세한 내용은 [**MediaCapture를 사용 하 여 장치 방향 처리**](handle-device-orientation-with-mediacapture.md)를 참조 하세요.  

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/SimpleCameraPreview_Win10/cs/MainPage.xaml.cs" id="SnippetStartPreviewAsync":::

## <a name="handle-changes-in-exclusive-control"></a>배타적 컨트롤의 변경 내용 처리
이전 섹션에서 설명한 것 처럼 다른 앱에 캡처 장치에 대 한 독점적 제어 권한이 있는 경우 **StartPreviewAsync** 은 **FileLoadException** 을 throw 합니다. Windows 10, 버전 1703부터 장치의 전용 제어 상태가 변경 될 때마다 발생 하는 [MediaCapture CaptureDeviceExclusiveControlStatusChanged](/uwp/api/Windows.Media.Capture.MediaCapture.CaptureDeviceExclusiveControlStatusChanged) 이벤트에 대 한 처리기를 등록할 수 있습니다. 이 이벤트에 대 한 처리기에서 [MediaCaptureDeviceExclusiveControlStatusChangedEventArgs](/uwp/api/windows.media.capture.mediacapturedeviceexclusivecontrolstatuschangedeventargs.Status) 속성을 확인 하 여 현재 상태를 확인 합니다. 새 상태가 **SharedReadOnlyAvailable**인 경우 현재 미리 보기를 시작할 수 없다는 것을 알 수 있으며 사용자에 게 경고 하기 위해 UI를 업데이트 하는 것이 좋습니다. 새 상태가 **ExclusiveControlAvailable**인 경우 카메라 미리 보기를 다시 시작 해 볼 수 있습니다.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/SimpleCameraPreview_Win10/cs/MainPage.xaml.cs" id="SnippetExclusiveControlStatusChanged":::

## <a name="shut-down-the-preview-stream"></a>미리 보기 스트림 종료

미리 보기 스트림 사용을 완료 한 후에는 항상 스트림을 종료 하 고 연결 된 리소스를 적절 하 게 삭제 하 여 카메라를 장치의 다른 앱에서 사용할 수 있도록 해야 합니다. 미리 보기 스트림을 종료 하는 데 필요한 단계는 다음과 같습니다.

-   카메라가 현재 미리 보기 상태인 경우 [**StopPreviewAsync**](/uwp/api/windows.media.capture.mediacapture.stoppreviewasync) 를 호출 하 여 미리 보기 스트림을 중지 합니다. 미리 보기가 실행 되지 않는 동안 **StopPreviewAsync** 를 호출 하면 예외가 throw 됩니다.
-   **CaptureElement** 의 [**Source**](/uwp/api/windows.ui.xaml.controls.captureelement.source) 속성을 null로 설정 합니다. [**CoreDispatcher**](/uwp/api/windows.ui.core.coredispatcher.runasync) 를 사용 하 여이 호출이 UI 스레드에서 실행 되는지 확인 합니다.
-   **MediaCapture** 개체의 [**Dispose**](/uwp/api/windows.media.capture.mediacapture.dispose) 메서드를 호출 하 여 개체를 해제 합니다. 다시 CoreDispatcher을 사용 하 여이 호출이 UI 스레드에서 실행 되는지 확인 [**합니다**](/uwp/api/windows.ui.core.coredispatcher.runasync) .
-   **MediaCapture** 멤버 변수를 null로 설정 합니다.
-   [**Requestrelease**](/uwp/api/windows.system.display.displayrequest.requestrelease) 를 호출 하 여 비활성 상태인 경우 화면을 끌 수 있도록 합니다.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/SimpleCameraPreview_Win10/cs/MainPage.xaml.cs" id="SnippetCleanupCameraAsync":::

사용자가 [**OnNavigatedFrom**](/uwp/api/windows.ui.xaml.controls.page.onnavigatedfrom) 메서드를 재정의 하 여 페이지에서 벗어나면 미리 보기 스트림을 종료 해야 합니다.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/SimpleCameraPreview_Win10/cs/MainPage.xaml.cs" id="SnippetOnNavigatedFrom":::

또한 앱이 일시 중단 되 면 미리 보기 스트림을 제대로 종료 해야 합니다. 이렇게 하려면 페이지의 생성자에서 [**응용 프로그램을 일시 중단**](/uwp/api/windows.applicationmodel.core.coreapplication.suspending) 하는 이벤트에 대 한 처리기를 등록 합니다.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/SimpleCameraPreview_Win10/cs/MainPage.xaml.cs" id="SnippetRegisterSuspending":::

**일시 중단** 이벤트 처리기에서 먼저 페이지 유형을 [**CurrentSourcePageType**](/uwp/api/windows.ui.xaml.controls.frame.currentsourcepagetype) 속성과 비교 하 여 페이지가 응용 프로그램의 [**프레임**](/uwp/api/Windows.UI.Xaml.Controls.Frame) 에 표시 되는지 확인 합니다. 페이지가 현재 표시 되지 않는 경우에는 **OnNavigatedFrom** 이벤트가 이미 발생 했 고 미리 보기 스트림이 종료 되었습니다. 페이지가 현재 표시 되는 경우 미리 보기 스트림이 종료 될 때까지 시스템에서 앱을 일시 중단 하지 않도록 하기 위해 처리기에 전달 된 이벤트 인수에서 [**SuspendingDeferral**](/uwp/api/Windows.ApplicationModel.SuspendingDeferral) 개체를 가져옵니다. 스트림을 종료한 후 지연의 [**Complete**](/uwp/api/windows.applicationmodel.suspendingdeferral.complete) 메서드를 호출하여 시스템에서 앱을 일시 중단하는 작업을 계속 진행하도록 합니다.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/SimpleCameraPreview_Win10/cs/MainPage.xaml.cs" id="SnippetSuspendingHandler":::


## <a name="related-topics"></a>관련 항목

* [카메라](camera.md)
* [MediaCapture를 사용하여 기본적인 사진, 비디오 및 오디오 캡처](basic-photo-video-and-audio-capture-with-MediaCapture.md)
* [미리 보기 프레임 가져오기](get-a-preview-frame.md)
