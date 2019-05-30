---
ms.assetid: 98BD79B3-F420-43C5-98D3-52EBDDB479A0
description: 이 문서에서는 BitmapEncoder에서 사용할 수 있는 인코딩 옵션에 대해 설명합니다.
title: BitmapEncoder 옵션 참조
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 34e2ac1531b496f076abbe8e23ffa1c0cb318373
ms.sourcegitcommit: ac7f3422f8d83618f9b6b5615a37f8e5c115b3c4
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 05/29/2019
ms.locfileid: "66359049"
---
# <a name="bitmapencoder-options-reference"></a>BitmapEncoder 옵션 참조


이 문서에서는 [**BitmapEncoder**](https://docs.microsoft.com/uwp/api/Windows.Graphics.Imaging.BitmapEncoder)에서 사용할 수 있는 인코딩 옵션에 대해 설명합니다. 인코딩 옵션은 문자열인 이름과 특정 데이터 형식([**Windows.Foundation.PropertyType**](https://docs.microsoft.com/uwp/api/Windows.Foundation.PropertyType))의 값으로 정의됩니다. 이미지 작업 방법은 [비트맵 이미지 만들기, 편집 및 저장](imaging.md)을 참조하세요.

| 이름                    | PropertyType | 사용 정보                                                                                        | 유효한 형식 |
|-------------------------|--------------|----------------------------------------------------------------------------------------------------|---------------|
| ImageQuality            | single       | 유효한 값은 0에서 1.0 사이입니다. 값이 높으면 품질이 더 높습니다.                                 | JPEG, JPEG-XR |
| CompressionQuality      | single       | 유효한 값은 0에서 1.0 사이입니다. 값이 높으면 더 효율적이고 더 느린 압축 구조입니다. | TIFF          |
| Lossless                | boolean      | true로 설정되면 ImageQuality 옵션이 무시됩니다.                                        | JPEG-XR       |
| InterlaceOption         | boolean      | 이미지를 인터레이스할지 여부입니다.                                                                    | PNG           |
| FilterOption            | uint8        | [  **PngFilterMode**](https://docs.microsoft.com/uwp/api/Windows.Graphics.Imaging.PngFilterMode) 열거형을 사용합니다.                                | PNG           |
| TiffCompressionMethod   | uint8        | [  **TiffCompressionMode**](https://docs.microsoft.com/uwp/api/Windows.Graphics.Imaging.TiffCompressionMode) 열거형을 사용합니다.                    | TIFF          |
| Luminance               | uint32Array  | 광도 양자화 상수를 포함하는 64개 요소의 배열입니다.                               | JPEG          |
| Chrominance             | uint32Array  | 색상 양자화 상수를 포함하는 64개 요소의 배열입니다.                             | JPEG          |
| JpegYCrCbSubsampling    | uint8        | [  **JpegSubsamplingMode**](https://docs.microsoft.com/uwp/api/Windows.Graphics.Imaging.JpegSubsamplingMode) 열거형을 사용합니다.                    | JPEG          |
| SuppressApp0            | boolean      | App0 메타데이터 블록이 생성되지 않게 할지 여부입니다.                                        | JPEG          |
| EnableV5Header32bppBGRA | boolean      | 알파를 지원하는 버전 5 BMP로 인코드할지 여부입니다.                                         | BMP           |

 

## <a name="related-topics"></a>관련 항목

* [비트맵 이미지 만들기, 편집 및 저장](imaging.md)
* [지원되는 코덱](supported-codecs.md)

 




