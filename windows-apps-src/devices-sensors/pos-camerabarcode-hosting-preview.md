---
title: 바코드 스캐너 카메라에 대한 미리 보기 호스팅
description: 응용 프로그램에서 카메라 바코드 스캐너에 대한 미리 보기를 호스팅하는 방법에 대한 자세한 내용
ms.date: 05/1/2018
ms.topic: article
keywords: windows 10, uwp, 서비스 지점, pos
ms.localizationpriority: medium
ms.openlocfilehash: 0eb7b1b620fcfa16576d84eaa2564408394d59de
ms.sourcegitcommit: d7613c791107f74b6a3dc12a372d9de916c0454b
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 12/05/2018
ms.locfileid: "8730603"
---
# <a name="hosting-a-camera-barcode-scanner-preview-in-your-application"></a>응용 프로그램에서 카메라 바코드 스캐너에 대한 미리 보기 호스팅
## <a name="step-1-setup-your-camera-preview"></a>1단계: 카메라 미리 보기 설정
[카메라 미리 보기 표시](../audio-video-camera/simple-camera-preview-access.md) 항목의 지침에 따라 카메라 바코드 스캐너에서 응용 프로그램에 미리 보기를 추가하는 첫 단계를 수행할 수 있습니다.  이 단계를 완료한 후, 카메라 바코드 스캐너 고유의 수정 작업을 위해 이 항목으로 돌아갑니다.

## <a name="step-2-update-capability-declarations"></a>2단계: 기능 선언 업데이트
사용자가 마이크에 대한 동의 확인 프롬프트를 수신하지 않도록 하려면 앱 매니페스트에 나열된 기능에서 이를 제외할 수 있습니다.

1. Microsoft Visual Studio의 **솔루션 탐색기**에서 **package.appxmanifest** 항목을 두 번 클릭하여 응용 프로그램 매니페스트 디자이너를 엽니다.
2. **기능** 탭을 선택합니다.
3. **마이크** 확인란 선택 취소

 ## <a name="step-3-add-additional-using-directive-for-media-capture"></a>3단계: 미디어 캡처를 위한 지시문을 사용하여 추가

```Csharp
using Windows.Media.Capture;
```

## <a name="step-4-set-up-your-mediacapture-initialization-settings"></a>4단계: MediaCapture 초기화 설정
다음 예제에서는 [**MediaCaptureInitializationSettings**](https://docs.microsoft.com/uwp/api/windows.media.capture.mediacaptureinitializationsettings)를 초기화합니다. 

```Csharp
 private void InitCaptureSettings()
{
    _captureInitSettings = new MediaCaptureInitializationSettings();
    _captureInitSettings.VideoDeviceId = BarcodeScanner.VideoDeviceId;
    _captureInitSettings.StreamingCaptureMode = StreamingCaptureMode.Video;
    _captureInitSettings.PhotoCaptureSource = PhotoCaptureSource.VideoPreview;
}
```
## <a name="step-5-associate-your-mediacapture-object-with-the-camera-barcode-scanner"></a>5단계: MediaCapture 개체를 스캐너 바코드 스캐너에 연결
*StartPreviewAsync()* 의 기존 mediaCapture.InitializeAsync()를 다음으로 교체합니다.

```Csharp
try
    {

        mediaCapture = new MediaCapture();
        await mediaCapture.InitializeAsync(InitCaptureSettings());

        displayRequest.RequestActive();
        DisplayInformation.AutoRotationPreferences = DisplayOrientations.Landscape;
    }
```

> [!TIP]
> 응용 프로그램에서의 카메라 미리 보기 호스팅에 대한 고급 항목은 [카메라 미리 보기 표시](https://docs.microsoft.com/windows/uwp/audio-video-camera/simple-camera-preview-access#add-capability-declarations-to-the-app-manifest)를 참조하세요.
