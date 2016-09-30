---
author: drewbatgit
ms.assetid: 9BA3F85A-970F-411C-ACB1-B65768B8548A
description: "이 문서에서는 UWP(유니버설 Windows 플랫폼) 앱에서 XAML 페이지 내의 카메라 미리 보기 스트림을 빠르게 표시하는 방법을 설명합니다."
title: "간단한 카메라 미리 보기 액세스"
translationtype: Human Translation
ms.sourcegitcommit: 72abc006de1925c3c06ecd1b78665e72e2ffb816
ms.openlocfilehash: 05e752925c07b0e3720fbdd42d785381aa08b99c

---

# 간단한 카메라 미리 보기 액세스

\[ Windows 10의 UWP 앱에 맞게 업데이트되었습니다. Windows 8.x 문서는 [보관](http://go.microsoft.com/fwlink/p/?linkid=619132)을 참조하세요. \]

이 문서에서는 UWP(유니버설 Windows 플랫폼) 앱에서 XAML 페이지 내의 카메라 미리 보기 스트림을 빠르게 표시하는 방법을 설명합니다. 카메라를 사용하여 사진 및 동영상을 캡처하는 앱을 만들려면 디바이스 및 카메라 방향을 다루거나 캡처된 파일에 대한 인코딩 옵션을 설정하는 것과 같은 작업을 수행해야 합니다. 일부 앱 시나리오의 경우 다른 고려 사항에 신경 쓰지 않고 카메라에서 미리 보기 스트림을 간단히 표시하고자 할 수 있습니다. 이 문서에서는 최소한의 코드로 이 작업을 수행하는 방법을 보여 줍니다. 아래 단계에 따라 작업을 완료했을 때 미리 보기 스트림을 항상 제대로 종료해야 합니다.

사진 또는 동영상을 캡처하는 카메라 앱을 작성하는 방법에 대한 자세한 내용은 [MediaCapture를 사용하여 사진 및 동영상 캡처](capture-photos-and-video-with-mediacapture.md)를 참조하세요.

## 앱 매니페스트에 기능 선언 추가

앱에서 디바이스의 카메라에 액세스해야 하는 경우 앱에 *webcam* and *microphone* 디바이스 기능이 사용된다고 선언해야 합니다. 캡처한 사진 또는 비디오를 사용자의 사진 라이브러리에 저장하려는 경우에도 *picturesLibrary* 및 *videosLibrary*기능을 선언해야 합니다.

**기능을 앱 매니페스트에 추가**

1.  Microsoft Visual Studio의 **솔루션 탐색기**에서 **package.appxmanifest** 항목을 두 번 클릭하여 응용 프로그램 매니페스트 디자이너를 엽니다.
2.  **접근 권한 값** 탭을 선택합니다.
3.  **웹캠** 확인란과 **마이크** 상자를 선택합니다.
4.  사진과 동영상 라이브러리에 액세스하려면 **사진 라이브러리**의 확인란과 **동영상 라이브러리**의 확인란을 선택합니다.

## CaptureElement를 페이지에 추가

[**CaptureElement**](https://msdn.microsoft.com/library/windows/apps/br209278)를 사용하여 XAML 페이지 내의 미리 보기 스트림을 표시합니다.

[!code-xml[CaptureElement](./code/SimpleCameraPreview_Win10/cs/MainPage.xaml#SnippetCaptureElement)]

## MediaCapture를 사용하여 미리 보기 스트림 시작

[**MediaCapture**](https://msdn.microsoft.com/library/windows/apps/br241124) 개체는 디바이스의 카메라에 대한 앱 인터페이스입니다. 이 클래스는 Windows.Media.Capture 네임스페이스의 멤버입니다. 이 문서의 예제에서는 기본 프로젝트 템플릿에 포함된 API뿐만 아니라 [**Windows.ApplicationModel**](https://msdn.microsoft.com/library/windows/apps/br224691) 및 [System.Threading.Tasks](https://msdn.microsoft.com/library/windows/apps/xaml/system.threading.tasks.aspx)의 API도 사용합니다.

using 지시문을 추가하여 페이지의 .cs 파일에 다음 네임스페이스를 포함합니다.

[!code-cs[SimpleCameraPreviewUsing](./code/SimpleCameraPreview_Win10/cs/MainPage.xaml.cs#SnippetSimpleCameraPreviewUsing)]

**MediaCapture** 개체에 대한 클래스 변수를 선언합니다.

[!code-cs[DeclareMediaCapture](./code/SimpleCameraPreview_Win10/cs/MainPage.xaml.cs#SnippetDeclareMediaCapture)]

**MediaCapture** 클래스의 새 인스턴스를 만들고 [**InitializeAsync**](https://msdn.microsoft.com/library/windows/apps/br226598)를 호출하여 캡처 디바이스를 초기화합니다. 예를 들어 카메라가 없는 디바이스에서는 이 메서드가 실패할 수 있으므로 **try** 블록 내에서 호출해야 합니다. 사용자가 디바이스의 개인 정보 설정에서 카메라 액세스를 사용하지 않도록 설정한 경우 카메라를 초기화하려고 할 때 **UnauthorizedAccessException**이 발생합니다. 개발하는 동안 앱 매니페스트에 적절한 기능을 추가하지 않으려는 경우에도 이 예외가 발생합니다.

**중요** 일부 디바이스 패밀리에서는 앱에 디바이스의 카메라에 대한 액세스가 허용되기 전에 사용자 동의 확인 프롬프트가 사용자에게 표시됩니다. 이러한 이유로 기본 UI 스레드에서만 [**MediaCapture.InitializeAsync**](https://msdn.microsoft.com/library/windows/apps/br226598)를 호출해야 합니다. 다른 스레드에서 카메라를 초기화하려고 하면 초기화 오류가 발생할 수 있습니다.

[**Source**](https://msdn.microsoft.com/library/windows/apps/br209280) 속성을 설정하여 **MediaCapture**를 **CaptureElement**에 연결합니다. 마지막으로 [**StartPreviewAsync**](https://msdn.microsoft.com/library/windows/apps/br226613)를 호출하여 미리 보기를 시작합니다.

[!code-cs[StartPreviewAsync](./code/SimpleCameraPreview_Win10/cs/MainPage.xaml.cs#SnippetStartPreviewAsync)]


## 미리 보기 스트림을 종료합니다.

미리 보기 스트림 사용을 완료했을 때 디바이스의 다른 앱에서 카메라를 사용할 수 있도록 항상 스트림을 종료하고 관련된 리소스를 제대로 해제해야 합니다. 미리 보기 스트림은 다음 단계에 따라 종료해야 합니다.

-   [**StopPreviewAsync**](https://msdn.microsoft.com/library/windows/apps/br226622)를 호출하여 미리 보기 스트림을 중지합니다.
-   **CaptureElement**의 [**Source**](https://msdn.microsoft.com/library/windows/apps/br209280) 속성을 null로 설정합니다.
-   **MediaCapture** 개체의 [**Dispose**](https://msdn.microsoft.com/library/windows/apps/dn278858) 메서드를 호출하여 해당 개체를 해제합니다.
-   **MediaCapture** 멤버 변수를 null로 설정합니다.

[!code-cs[CleanupCameraAsync](./code/SimpleCameraPreview_Win10/cs/MainPage.xaml.cs#SnippetCleanupCameraAsync)]

사용자가 페이지에서 벗어날 때 [**OnNavigatedFrom**](https://msdn.microsoft.com/library/windows/apps/br227507) 메서드를 재정의하여 미리 보기 스트림을 종료해야 합니다.

[!code-cs[OnNavigatedFrom](./code/SimpleCameraPreview_Win10/cs/MainPage.xaml.cs#SnippetOnNavigatedFrom)]

앱을 일시 중단하는 경우에도 미리 보기 스트림을 제대로 종료해야 합니다. 이렇게 하려면 페이지 생성자에 [**Application.Suspending**](https://msdn.microsoft.com/library/windows/apps/br205860) 이벤트에 대한 처리기를 등록합니다.

[!code-cs[RegisterSuspending](./code/SimpleCameraPreview_Win10/cs/MainPage.xaml.cs#SnippetRegisterSuspending)]

**Suspending** 이벤트 처리기에서 먼저 페이지 유형과 [**CurrentSourcePageType**](https://msdn.microsoft.com/library/windows/apps/hh702390) 속성을 비교하여 응용 프로그램의 [**Frame**](https://msdn.microsoft.com/library/windows/apps/br242682)이 페이지에 표시되고 있는지 확인해야 합니다. 현재 페이지에 표시되지 않는 경우 **OnNavigatedFrom** 이벤트가 이미 발생한 상태이므로 미리 보기 스트림을 종료합니다. 현재 페이지에 표시되는 경우 처리기에 전달된 이벤트 인수에서 [**SuspendingDeferral**](https://msdn.microsoft.com/library/windows/apps/br224684) 개체를 가져와서 미리 보기 스트림이 종료될 때까지 시스템에서 앱을 일시 중단하지 않도록 해야 합니다. 스트림을 종료한 후 지연의 [**Complete**](https://msdn.microsoft.com/library/windows/apps/br224685) 메서드를 호출하여 시스템에서 앱을 일시 중단하는 작업을 계속 진행하도록 합니다.

[!code-cs[SuspendingHandler](./code/SimpleCameraPreview_Win10/cs/MainPage.xaml.cs#SnippetSuspendingHandler)]

## 미리 보기 스트림에서 스틸 이미지 캡처

미디어 캡처 미리 보기 스트림에서 [**SoftwareBitmap**](https://msdn.microsoft.com/library/windows/apps/dn887358) 형식의 스틸 이미지를 간단히 가져올 수 있습니다. 자세한 내용은 [미리 보기 프레임 가져오기](get-a-preview-frame.md)를 참조하세요.

## 관련 항목

* [MediaCapture를 사용하여 사진 및 비디오 캡처](capture-photos-and-video-with-mediacapture.md)
* [미리 보기 프레임 가져오기](get-a-preview-frame.md)



<!--HONumber=Jun16_HO4-->


