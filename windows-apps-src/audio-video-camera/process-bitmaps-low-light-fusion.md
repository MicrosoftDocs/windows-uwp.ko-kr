---
description: 이 문서에서는 LowLightFusion 클래스를 사용 하 여 비트맵을 처리 하는 방법을 설명 합니다.
title: 낮은 밝은 Fusion API를 사용 하 여 비트맵 처리
ms.date: 03/22/2018
ms.topic: article
keywords: windows 10, uwp, 낮은 밝은 fusion, 비트맵, 이미지 처리
ms.localizationpriority: medium
ms.openlocfilehash: 4e82eb780efe83125a09417f349f84ee9451c1f0
ms.sourcegitcommit: c3ca68e87eb06971826087af59adb33e490ce7da
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 09/02/2020
ms.locfileid: "89363836"
---
# <a name="process-bitmaps-with-the-lowlightfusion-api"></a>LowLightFusion API로 비트맵 처리

낮은 밝은 이미지는 특히 수정 된 애퍼처 및 센서 크기가 있는 모바일 장치에서 좋은 이미지 품질로 캡처 하기 어렵습니다. 낮은 조명에 맞게 보정 하기 위해 장치가 노출 시간 또는 센서 이득을 늘릴 수 있으며,이로 인해 이미지의 동작 흐림 효과와 노이즈가 늘어날 수 있습니다. 

[LowLightFusion 클래스](/uwp/api/windows.media.core.lowlightfusion) 는 짧은 버스트 이미지 (즉, 짧은 버스트 이미지)에서 여러 프레임의 픽셀 정보를 샘플링 하 여 소량 이미지의 품질을 향상 시켜 노이즈 및 동작 흐림을 줄입니다. 예를 들어 사진 편집 앱에 추가 하는 데 유용한 기능입니다.

이 기능은 [AdvancedPhotoCapture 클래스](/uwp/api/Windows.Media.Capture.AdvancedPhotoCapture)를 통해서도 사용할 수 있으며, 필요한 경우 이미지를 캡처한 직후 이미지 시퀀스에 저 라이트 Fusion 알고리즘을 적용 합니다. 이 기능을 구현 하는 방법을 알아보려면 [낮은 밝은 사진](./high-dynamic-range-hdr-photo-capture.md#low-light-photo-capture) 캡처를 참조 하세요.

## <a name="prepare-the-images-for-processing"></a>처리할 이미지 준비

이 예제에서는 [LowLightFusion 클래스](/uwp/api/windows.media.core.lowlightfusion)를 사용 하는 방법과 [fileopenpicker](/uwp/api/Windows.Storage.Pickers.FileOpenPicker) 를 사용 하 여 사용자가 더 낮은 빛 퓨전을 수행할 여러 이미지를 선택할 수 있도록 하는 방법을 설명 합니다.

먼저 알고리즘이 수락 하는 이미지 (프레임) 수를 확인 하 고 이러한 프레임을 저장할 목록을 만듭니다.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/LowLightFusionSample/cs/MainPage.xaml.cs" id="SnippetGetMaxLLFFrames":::

저 라이트 Fusion 알고리즘이 허용 하는 프레임 수를 결정 한 후에는 [Fileopenpicker](/uwp/api/Windows.Storage.Pickers.FileOpenPicker) 를 사용 하 여 사용자가 알고리즘에 사용할 이미지를 선택할 수 있습니다.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/LowLightFusionSample/cs/MainPage.xaml.cs" id="SnippetGetFrames":::

올바른 개수의 프레임을 선택 했으므로 프레임을이 [비트맵](/uwp/api/Windows.Graphics.Imaging.SoftwareBitmap) 으로 디코드 하 고 LowLightFusion에 대 한 올바른 형식 인지 확인 해야 합니다.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/LowLightFusionSample/cs/MainPage.xaml.cs" id="SnippetDecodeFrames":::


## <a name="fuse-the-bitmaps-into-a-single-bitmap"></a>비트맵을 단일 비트맵으로 퓨즈

적절 한 형식으로 올바른 프레임 수를 만들었으므로 **[FuseAsync](/uwp/api/windows.media.core.lowlightfusion.fuseasync)** 메서드를 사용 하 여 낮은 밝은 Fusion 알고리즘을 적용할 수 있습니다. 결과는 명확 하 게 이해 하기 쉽게 해 주는 처리 된 이미지입니다. 

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/LowLightFusionSample/cs/MainPage.xaml.cs" id="SnippetFuseFrames":::

마지막으로, 사용자가 시작한 입력 이미지와 비슷한 방식으로 사용자에 게 친숙 한 "일반" 이미지로 인코딩 및 저장 하 여 결과를 제거 합니다.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/LowLightFusionSample/cs/MainPage.xaml.cs" id="SnippetEncodeFrame":::


## <a name="before-and-after"></a>이전 및 이후

낮은 광원 Fusion 알고리즘을 적용 한 후 입력 이미지 및 결과 출력 이미지의 예는 다음과 같습니다.

> [!div class="mx-tableFixed"] 
| 입력 프레임 | 낮은 밝은 Fusion 출력 | 
|-------------|-------------------------|
| ![낮은 밝은 Fusion 알고리즘에 대 한 입력 프레임](./images/LLF-Input.png) | ![낮은 밝은 Fusion 알고리즘의 결과 프레임](./images/LLF-Output.png) |

입력 프레임에서 배너 주위의 그림자에 대 한 조명이 향상 되었음을 알 수 있습니다.

## <a name="related-topics"></a>관련 항목 
[LowLightFusion 클래스](/uwp/api/windows.media.core.lowlightfusion)  
[LowLightFusionResult 클래스](/uwp/api/windows.media.core.lowlightfusionresult)
