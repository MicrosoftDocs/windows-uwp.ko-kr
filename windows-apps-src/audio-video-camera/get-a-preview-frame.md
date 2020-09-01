---
ms.assetid: 05E418B4-5A62-42BD-BF66-A0762216D033
description: 이 항목에서는 미디어 캡처 미리 보기 스트림에서 단일 미리 보기 프레임을 가져오는 방법을 보여 줍니다.
title: 미리 보기 프레임 가져오기
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 235a5e06a8483599b8fbf29e866e990456c1f1f1
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89163947"
---
# <a name="get-a-preview-frame"></a>미리 보기 프레임 가져오기


이 항목에서는 미디어 캡처 미리 보기 스트림에서 단일 미리 보기 프레임을 가져오는 방법을 보여 줍니다.

> [!NOTE] 
> 이 문서는 기본 사진 및 비디오 캡처를 구현 하는 단계를 설명 하는 [MediaCapture을 사용 하 여 기본 사진, 비디오 및 오디오 캡처](basic-photo-video-and-audio-capture-with-MediaCapture.md)에 설명 된 개념 및 코드를 기반으로 합니다. 고급 캡처 시나리오로 전환 하기 전에 해당 문서의 기본적인 미디어 캡처 패턴을 숙지 하는 것이 좋습니다. 이 문서의 코드는 앱에 이미 올바르게 초기화 된 MediaCapture 인스턴스가 있고 활성 비디오 미리 보기 스트림이 있는 [**CaptureElement**](/uwp/api/Windows.UI.Xaml.Controls.CaptureElement) 가 있다고 가정 합니다.

기본 미디어 캡처에 필요한 네임 스페이스 외에도 미리 보기 프레임을 캡처하려면 다음 네임 스페이스가 필요 합니다.

[!code-cs[PreviewFrameUsing](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetPreviewFrameUsing)]

미리 보기 프레임을 요청할 때 원하는 형식으로 [**videoframe**](/uwp/api/Windows.Media.VideoFrame) 개체를 만들어 프레임을 받을 형식을 지정할 수 있습니다. 이 예에서는 [**VideoDeviceController**](/uwp/api/windows.media.devices.videodevicecontroller.getmediastreamproperties) 를 호출 하 고 미리 보기 스트림의 속성을 요청 하는 [**VideoPreview**](/uwp/api/Windows.Media.Capture.MediaStreamType) 를 지정 하 여 미리 보기 스트림과 동일한 해상도의 비디오 프레임을 만듭니다. 미리 보기 스트림의 너비와 높이는 새 비디오 프레임을 만드는 데 사용 됩니다.

[!code-cs[CreateFormatFrame](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetCreateFormatFrame)]

[**MediaCapture**](/uwp/api/Windows.Media.Capture.MediaCapture) 개체가 초기화 되 고 활성 미리 보기 스트림이 있는 경우 [**GetPreviewFrameAsync**](/uwp/api/windows.media.capture.mediacapture.getpreviewframeasync) 를 호출 하 여 미리 보기 스트림을 가져옵니다. 마지막 단계에서 만든 비디오 프레임을 전달 하 여 반환 되는 프레임의 형식을 지정 합니다.

[!code-cs[GetPreviewFrameAsync](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetGetPreviewFrameAsync)]

[**Videoframe**](/uwp/api/Windows.Media.VideoFrame) 개체의 [**\bitmap**](/uwp/api/windows.media.videoframe.softwarebitmap) 속성에 액세스 하 여 미리 보기 프레임의 지 속성 [**비트맵**](/uwp/api/Windows.Graphics.Imaging.SoftwareBitmap) 표현을 가져옵니다. 소프트웨어 비트맵을 저장, 로드 및 수정 하는 방법에 대 한 자세한 내용은 [Imaging](imaging.md)을 참조 하세요.

[!code-cs[GetPreviewBitmap](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetGetPreviewBitmap)]

Direct3D Api에서 이미지를 사용 하려는 경우 미리 보기 프레임의 [**IDirect3DSurface**](/uwp/api/Windows.Graphics.DirectX.Direct3D11.IDirect3DSurface) 표현을 가져올 수도 있습니다.

[!code-cs[GetPreviewSurface](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetGetPreviewSurface)]

> [!IMPORTANT]
> **GetPreviewFrameAsync** 를 호출 하는 방법 및 앱이 실행 되는 장치에 따라 반환 된 **Videoframe** 의 [**Direct3DSurface**](/uwp/api/windows.media.videoframe.direct3dsurface) [**속성은 null**](/uwp/api/windows.media.videoframe.softwarebitmap) 일 수 있습니다.

> - **Videoframe** 인수를 허용 하는 [**GetPreviewFrameAsync**](/uwp/api/windows.media.capture.mediacapture.getpreviewframeasync) 의 오버 로드를 호출 하면 반환 되는 **videoframe** 에 null이 아닌 **tbitmap** 이 포함 되 고 **Direct3DSurface** 속성은 null이 됩니다.
> - Direct3D surface를 사용 하 여 내부적으로 프레임을 표시 하는 장치에 인수가 없는 [**GetPreviewFrameAsync**](/uwp/api/windows.media.capture.mediacapture.getpreviewframeasync) 의 오버 로드를 호출 하는 경우 **Direct3DSurface** 속성은 Null이 아니고, **\bitmap** 속성은 null이 됩니다.
> - Direct3D surface를 사용 하지 않고 내부적으로 프레임을 표시 하는 장치에 인수가 없는 [**GetPreviewFrameAsync**](/uwp/api/windows.media.capture.mediacapture.getpreviewframeasync) 의 오버 로드를 호출 하는 경우에는 Direct3DSurface **bitmap** 속성이 null이 아니고 **Direct3DSurface** 속성이 null이 됩니다.

응용 **프로그램은** **Direct3DSurface** 속성에서 반환 하는 개체에 대해 작업을 수행 하기 전에 항상 null 값을 확인 해야 합니다.

미리 보기 프레임을 사용 하 여 작업을 완료 한 후에는 해당 [**Close**](/uwp/api/windows.media.videoframe.close) 메서드 (c #에서는 Dispose로 프로젝션)를 호출 하 여 프레임에서 사용 하는 리소스를 해제 해야 합니다. 또는 개체를 자동으로 삭제 **하는 using** 패턴을 사용 합니다.

[!code-cs[CleanUpPreviewFrame](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetCleanUpPreviewFrame)]

## <a name="related-topics"></a>관련 항목

* [카메라](camera.md)
* [MediaCapture를 사용하여 기본적인 사진, 비디오 및 오디오 캡처](basic-photo-video-and-audio-capture-with-MediaCapture.md)
 

 