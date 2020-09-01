---
title: 카메라 바코드 스캐너에 대 한 호스팅 미리 보기
description: 응용 프로그램에서 카메라 바코드 스캐너 미리 보기를 호스트 하는 방법을 알아봅니다.
ms.date: 05/02/2018
ms.topic: article
keywords: windows 10, uwp, 서비스 지점, pos
ms.localizationpriority: medium
ms.openlocfilehash: 6657fa52a5e3cac0821def7102b4bc93089b2ffe
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89159637"
---
# <a name="hosting-a-camera-barcode-scanner-preview-in-your-application"></a>응용 프로그램에서 카메라 바코드 스캐너 미리 보기 호스팅
## <a name="step-1-setup-your-camera-preview"></a>1 단계: 카메라 미리 보기 설정
카메라 바코드 스캐너에 대 한 미리 보기를 응용 프로그램에 추가 하는 첫 번째 단계는 [카메라 미리 보기 표시](../audio-video-camera/simple-camera-preview-access.md) 항목의 지침을 수행 하 여 수행할 수 있습니다.  이 단계를 완료 한 후에는 카메라 바코드 스캐너 관련 수정에 대 한이 항목으로 돌아옵니다.

## <a name="step-2-update-capability-declarations"></a>2 단계: 기능 선언 업데이트
사용자가 마이크에 대 한 동의 확인 프롬프트를 받지 못하도록 하려면 앱 매니페스트에 나열 된 기능에서 제외할 수 있습니다.

1. Microsoft Visual Studio의 **솔루션 탐색기**에서 **package.appxmanifest** 항목을 두 번 클릭하여 응용 프로그램 매니페스트 디자이너를 엽니다.
2. **기능** 탭을 선택합니다.
3. **마이크** 확인란의 선택을 취소 합니다.

 ## <a name="step-3-add-additional-using-directive-for-media-capture"></a>3 단계: 미디어 캡처에 대 한 using 지시문 추가

```Csharp
using Windows.Media.Capture;
```

## <a name="step-4-set-up-your-mediacapture-initialization-settings"></a>4 단계: MediaCapture 초기화 설정 설정
다음 예에서는 [**MediaCaptureInitializationSettings**](/uwp/api/windows.media.capture.mediacaptureinitializationsettings)를 초기화 합니다. 

```Csharp
 private void InitCaptureSettings()
{
    _captureInitSettings = new MediaCaptureInitializationSettings();
    _captureInitSettings.VideoDeviceId = BarcodeScanner.VideoDeviceId;
    _captureInitSettings.StreamingCaptureMode = StreamingCaptureMode.Video;
    _captureInitSettings.PhotoCaptureSource = PhotoCaptureSource.VideoPreview;
}
```
## <a name="step-5-associate-your-mediacapture-object-with-the-camera-barcode-scanner"></a>5 단계: MediaCapture 개체를 카메라 바코드 스캐너와 연결
*StartPreviewAsync ()* 에서 기존 mediaCapture.InitializeAsync ()를 다음으로 바꿉니다.

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
> 응용 프로그램에서 카메라 미리 보기를 호스트 하는 방법에 대 한 고급 항목 [은 카메라 미리 보기 표시](../audio-video-camera/simple-camera-preview-access.md#add-capability-declarations-to-the-app-manifest) 를 참조 하세요.

## <a name="see-also"></a>참고 항목

### <a name="samples"></a>샘플

- [바코드 스캐너 샘플](https://github.com/microsoft/Windows-universal-samples/tree/master/Samples/BarcodeScanner)