---
author: drewbatgit
ms.assetid: 05E418B4-5A62-42BD-BF66-A0762216D033
description: 이 항목은 미디어 캡처 미리 보기 스트림에서 단일 미리 보기 프레임을 가져오는 방법을 보여 줍니다.
title: 미리 보기 프레임 가져오기
ms.author: drewbat
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 211bd4ce660726030f8b90d29c4ea4d8a14564de
ms.sourcegitcommit: b7e3d222e229cdbf04e837fcb94fb7d84a93de09
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/26/2018
ms.locfileid: "5597095"
---
# <a name="get-a-preview-frame"></a>미리 보기 프레임 가져오기


이 항목은 미디어 캡처 미리 보기 스트림에서 단일 미리 보기 프레임을 가져오는 방법을 보여 줍니다.

> [!NOTE] 
> 이 문서는 기본 사진 및 비디오 캡처 구현 단계를 설명하는 [MediaCapture를 사용한 기본적인 사진, 비디오 및 오디오 캡처](basic-photo-video-and-audio-capture-with-MediaCapture.md)에 설명된 개념 및 코드를 토대로 작성되었습니다. 보다 수준 높은 캡처 시나리오를 진행하기 전에 해당 문서의 기본적인 미디어 캡처 패턴을 파악하는 것이 좋습니다. 이 문서의 코드는 앱에 적절히 초기화된 MediaCapture의 인스턴스가 이미 있으며 활성 비디오 미리 보기 스트림의 [**CaptureElement**](https://msdn.microsoft.com/library/windows/apps/br209278)가 있다고 가정합니다.

기본 미디어 캡처에 필요한 네임스페이스 외에도, 미리 보기 프레임을 캡처하려면 다음 네임스페이스가 필요합니다.

[!code-cs[PreviewFrameUsing](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetPreviewFrameUsing)]

미리 보기 프레임을 요청할 때 원하는 형식의 [**VideoFrame**](https://msdn.microsoft.com/library/windows/apps/dn930917) 개체를 만들어 프레임을 수신할 형식을 지정할 수 있습니다. 이 예제에서는 [**VideoDeviceController.GetMediaStreamProperties**](https://msdn.microsoft.com/library/windows/apps/br211995)를 호출하고 [**MediaStreamType.VideoPreview**](https://msdn.microsoft.com/library/windows/apps/br226640)를 지정하여 미리 보기 스트림에 대한 속성을 요청함으로써, 미리 보기 스트림과 같은 해상도인 비디오 프레임을 만듭니다. 미리 보기 스트림의 너비와 높이는 새 비디오 프레임을 만드는 데 사용됩니다.

[!code-cs[CreateFormatFrame](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetCreateFormatFrame)]

[**MediaCapture**](https://msdn.microsoft.com/library/windows/apps/br241124) 개체가 초기화되고 활성 현재 미리 보기 스트림이 있는 경우 [**GetPreviewFrameAsync**](https://msdn.microsoft.com/library/windows/apps/dn926711)를 호출하여 미리 보기 스트림을 가져옵니다. 마지막 단계에서 만든 비디오 프레임을 전달하여 반환된 프레임의 형식을 지정합니다.

[!code-cs[GetPreviewFrameAsync](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetGetPreviewFrameAsync)]

[**VideoFrame**](https://msdn.microsoft.com/library/windows/apps/dn930917) 개체의 [**SoftwareBitmap**](https://msdn.microsoft.com/library/windows/apps/dn930926) 속성에 액세스하여 미리 보기 프레임의 [**SoftwareBitmap**](https://msdn.microsoft.com/library/windows/apps/dn887358) 표현을 가져옵니다. 소프트웨어 비트맵 저장, 로드 및 수정에 대한 자세한 내용은 [이미징](imaging.md)을 참조하세요.

[!code-cs[GetPreviewBitmap](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetGetPreviewBitmap)]

Direct3D API에서 이미지를 사용하려면 미리 보기 프레임의 [**IDirect3DSurface**](https://msdn.microsoft.com/library/windows/apps/dn965505) 표현을 가져올 수도 있습니다.

[!code-cs[GetPreviewSurface](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetGetPreviewSurface)]

> [!IMPORTANT]
> 반환된 **VideoFrame**의 [**SoftwareBitmap**](https://msdn.microsoft.com/library/windows/apps/dn930926) 속성 또는 [**Direct3DSurface**](https://msdn.microsoft.com/library/windows/apps/dn930920) 속성은 **GetPreviewFrameAsync** 호출 방법 및 앱이 실행 중인 디바이스에 따라 null이 될 수 있습니다.

> - **VideoFrame** 인수를 허용하는 [**GetPreviewFrameAsync**](https://msdn.microsoft.com/library/windows/apps/dn926713)의 오버로드를 호출하면 반환된 **VideoFrame**의 **SoftwareBitmap**은 null이 아니고 **Direct3DSurface** 속성은 null이 됩니다.
> - 내부적으로 프레임을 표시하기 위해 Direct3D 화면을 사용하는 디바이스에서 인수가 없는 [**GetPreviewFrameAsync**](https://msdn.microsoft.com/library/windows/apps/dn926712)의 오버로드를 호출하면 **Direct3DSurface** 속성은 null이 아닌 값이 되고 **SoftwareBitmap** 속성은 null이 됩니다.
> - 내부적으로 프레임을 표시하기 위해 Direct3D 화면을 사용하지 않는 디바이스에서 인수가 없는 [**GetPreviewFrameAsync**](https://msdn.microsoft.com/library/windows/apps/dn926712)의 오버로드를 호출하면 **SoftwareBitmap** 속성은 null이 아닌 값이 되고 **Direct3DSurface** 속성은 null이 됩니다.

**SoftwareBitmap** 또는 **Direct3DSurface** 속성에서 반환하는 개체에 대해 작업하기 전에 항상 null 값을 확인해야 합니다.

미리 보기 프레임 사용이 끝나면 해당 [**Close**](https://msdn.microsoft.com/library/windows/apps/dn930918)메서드(C#에서 Dispose에 프로젝션됨)를 호출하여 프레임에 사용되는 리소스를 해제해야 합니다. 또는 개체를 자동으로 해제하는 **using** 패턴을 사용하세요.

[!code-cs[CleanUpPreviewFrame](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetCleanUpPreviewFrame)]

## <a name="related-topics"></a>관련 항목

* [카메라](camera.md)
* [MediaCapture를 사용하여 기본적인 사진, 비디오 및 오디오 캡처](basic-photo-video-and-audio-capture-with-MediaCapture.md)
 

 




