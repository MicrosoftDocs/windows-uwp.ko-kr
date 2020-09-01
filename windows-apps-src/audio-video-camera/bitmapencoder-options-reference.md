---
ms.assetid: 98BD79B3-F420-43C5-98D3-52EBDDB479A0
description: 이 문서에서는 BitmapEncoder와 함께 사용할 수 있는 인코딩 옵션을 나열 합니다.
title: BitmapEncoder 옵션 참조
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 212f405c943354618a4ae2bf6f2ab9c4925e7085
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89161087"
---
# <a name="bitmapencoder-options-reference"></a>BitmapEncoder 옵션 참조


이 문서에서는 [**BitmapEncoder**](/uwp/api/Windows.Graphics.Imaging.BitmapEncoder)와 함께 사용할 수 있는 인코딩 옵션을 나열 합니다. 인코딩 옵션은 해당 이름 (문자열) 및 특정 데이터 형식 ([**PropertyType**](/uwp/api/Windows.Foundation.PropertyType))의 값으로 정의 됩니다. 이미지 작업에 대 한 자세한 내용은 [비트맵 이미지 만들기, 편집 및 저장](imaging.md)을 참조 하세요.

| 이름                    | PropertyType | 사용 메모                                                                                        | 유효한 형식 |
|-------------------------|--------------|----------------------------------------------------------------------------------------------------|---------------|
| ImageQuality            | single       | 유효한 값은 0에서 1.0 까지입니다. 값이 높을수록 품질이 높아집니다.                                 | JPEG, JPEG-XR |
| CompressionQuality      | single       | 유효한 값은 0에서 1.0 까지입니다. 값이 높을수록 더 효율적이 고 느린 압축 체계를 의미 합니다. | TIFF          |
| 무손실                | boolean      | True로 설정 되 면 ImageQuality 옵션이 무시 됩니다.                                        | JPEG-XR       |
| InterlaceOption         | boolean      | 이미지의 인터레이스 여부                                                                    | PNG           |
| FilterOption            | uint8        | [**PngFilterMode**](/uwp/api/Windows.Graphics.Imaging.PngFilterMode) 열거 사용                                | PNG           |
| TiffCompressionMethod   | uint8        | [**TiffCompressionMode**](/uwp/api/Windows.Graphics.Imaging.TiffCompressionMode) 열거 사용                    | TIFF          |
| 광도               | uint32Array  | 광도 양자화 상수가 포함 된 64 요소의 배열입니다.                               | JPEG          |
| 색차             | uint32Array  | 색차 양자화 상수를 포함 하는 64 요소의 배열입니다.                             | JPEG          |
| JpegYCrCbSubsampling    | uint8        | [**JpegSubsamplingMode**](/uwp/api/Windows.Graphics.Imaging.JpegSubsamplingMode) 열거 사용                    | JPEG          |
| SuppressApp0            | boolean      | App0 메타 데이터 블록 만들기를 표시 하지 않을 지 여부                                        | JPEG          |
| EnableV5Header32bppBGRA | boolean      | 알파를 지 원하는 버전 5 BMP로 인코딩할 지 여부                                         | BMP           |

 

## <a name="related-topics"></a>관련 항목

* [비트맵 이미지 만들기, 편집 및 저장](imaging.md)
* [지원되는 코덱](supported-codecs.md)

 