---
description: 이 문서에서는 LowLightFusion 클래스를 사용하여 비트맵을 처리하는 방법에 대해 설명합니다.
title: Low Light Fusion API로 비트맵 처리
ms.date: 03/22/2018
ms.topic: article
keywords: windows 10, uwp, low light fusion, 비트맵, 이미지 처리
ms.localizationpriority: medium
ms.openlocfilehash: e0677d2e4ce5e412c158b8a00da3a6338bae6c46
ms.sourcegitcommit: b5c9c18e70625ab770946b8243f3465ee1013184
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/28/2018
ms.locfileid: "7964093"
---
# <a name="process-bitmaps-with-the-lowlightfusion-api"></a>LowLightFusion API로 비트맵 처리

저조도 이미지는 좋은 화질로 캡처하기가 어렵습니다. 특히 조리개과 센서 크기가 고정된 모바일 장치에서는 더욱 그렇습니다. 낮은 조도를 보완하기 위해 디바이스가 노출 시간이나 센서 이득을 늘릴 수 있는데, 그러면 모션 블러가 발생하고 이미지의 노이즈가 높아질 수 있습니다. 

[LowLightFusion 클래스](https://docs.microsoft.com/uwp/api/windows.media.core.lowlightfusion)는 시간적으로 근접한 여러 프레임, 즉 짧은 버스트 이미지에서 픽셀 정보를 샘플링하여 노이즈와 모션 블러를 줄임으로써 저조도 이미지의 품질을 향상시킵니다. 이것은 사진 편집 앱 등에 추가하기에 유용한 기능입니다.

이 기능은 [AdvancedPhotoCapture 클래스](https://docs.microsoft.com/uwp/api/Windows.Media.Capture.AdvancedPhotoCapture)를 통해 제공할 수도 있습니다. 이 클래스는 필요에 다라 이미지가 캡처된 직후 이미지 시퀀스에 Low Light Fusion 알고리즘을 적용합니다. 이 기능을 구현하는 방법에 대해 알아보려면 [저조도 사진](https://docs.microsoft.com/windows/uwp/audio-video-camera/high-dynamic-range-hdr-photo-capture#low-light-photo-capture) 캡처를 참조하세요.

## <a name="prepare-the-images-for-processing"></a>이미지 처리 준비

이 예에서는 [LowLightFusion 클래스](https://docs.microsoft.com/uwp/api/windows.media.core.lowlightfusion)와 [FileOpenPicker](https://docs.microsoft.com/uwp/api/Windows.Storage.Pickers.FileOpenPicker)를 사용하여 사용자가 여러 이미지를 선택해 Low Light Fusion을 수행하도록 만드는 방법을 보여줍니다.

먼저 알고리즘이 수용하는 이미지(프레임이라고도 함)의 수를 결정하고 이 프레임을 수록할 목록을 만들어야 합니다.

[!code-cs[SnippetGetMaxLLFFrames](./code/LowLightFusionSample/cs/MainPage.xaml.cs#SnippetGetMaxLLFFrames)]

Low Light Fusion 알고리즘이 수용하는 프레임 수를 결정했으면 [FileOpenPicker](https://docs.microsoft.com/uwp/api/Windows.Storage.Pickers.FileOpenPicker)를 사용하여 사용자가 알고리즘에 어떤 이미지를 사용할지 선택하도록 만들 수 있습니다.

[!code-cs[SnippetGetFrames](./code/LowLightFusionSample/cs/MainPage.xaml.cs#SnippetGetFrames)]

이제 프레임 수를 정학하게 선택했으므로 프레임을 [SoftwareBitmaps](https://docs.microsoft.com/uwp/api/Windows.Graphics.Imaging.SoftwareBitmap)로 디코딩하고 SoftwareBitmap이 LowLightFusion에 적합한 형식인지 확인해야 합니다.

[!code-cs[SnippetDecodeFrames](./code/LowLightFusionSample/cs/MainPage.xaml.cs#SnippetDecodeFrames)]


## <a name="fuse-the-bitmaps-into-a-single-bitmap"></a>여러 비트맵을 단일 비트맵으로 융합

이제 허용되는 형식으로 정확한 프레임 수가 정해졌으니 **[FuseAsync](https://docs.microsoft.com/uwp/api/windows.media.core.lowlightfusion.fuseasync)** 메서드를 사용하여 Low Light Fusion 알고리즘을 적용할 수 있습니다. 결과적으로, 처리를 통해 선명도가 향상된 SoftwareBitmap 형식의 이미지가 만들어집니다. 

[!code-cs[SnippetFuseFrames](./code/LowLightFusionSample/cs/MainPage.xaml.cs#SnippetFuseFrames)]

마지막으로 처음의 입력 이미지와 비슷하게 사용자 친화적인 "일반" 이미지로 인코딩하고 저장하여 결과 SoftwareBitmap을 정리하겠습니다.

[!code-cs[SnippetEncodeFrame](./code/LowLightFusionSample/cs/MainPage.xaml.cs#SnippetEncodeFrame)]


## <a name="before-and-after"></a>전과 후

다음 예는 입력 이미지와 Low Light Fusion 알고리즘을 적용한 결과 출력된 이미지를 보여줍니다.

> [!div class="mx-tableFixed"] 
| 입력된 프레임 | Low Light Fusion 출력 | 
|-------------|-------------------------|
| ![Low Light Fusion 알고리즘에 입력한 프레임](./images/LLF-Input.png) | ![Low Light Fusion 알고리즘의 결과 프레임](./images/LLF-Output.png) |

입력된 프레임에서 배너 주변 그림자의 밝기와 선명도가 향상되었음을 알 수 있습니다.

## <a name="related-topics"></a>관련 항목 
[LowLightFusion 클래스](https://docs.microsoft.com/uwp/api/windows.media.core.lowlightfusion)  
[LowLightFusionResult 클래스](https://docs.microsoft.com/uwp/api/windows.media.core.lowlightfusionresult)