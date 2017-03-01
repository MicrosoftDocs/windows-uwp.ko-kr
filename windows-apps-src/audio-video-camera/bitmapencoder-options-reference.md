---
author: laurenhughes
ms.assetid: 98BD79B3-F420-43C5-98D3-52EBDDB479A0
description: "이 문서에서는 BitmapEncoder에서 사용할 수 있는 인코딩 옵션에 대해 설명합니다."
title: "BitmapEncoder 옵션 참조"
ms.author: lahugh
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
translationtype: Human Translation
ms.sourcegitcommit: c6b64cff1bbebc8ba69bc6e03d34b69f85e798fc
ms.openlocfilehash: c29c05e7397e87851ebb0fa5fa29df3b9b58907b
ms.lasthandoff: 02/07/2017

---

# <a name="bitmapencoder-options-reference"></a>BitmapEncoder 옵션 참조

\[ Windows 10의 UWP 앱에 맞게 업데이트되었습니다. Windows 8.x 문서는 [보관](http://go.microsoft.com/fwlink/p/?linkid=619132)을 참조하세요. \]

이 문서에서는 [**BitmapEncoder**](https://msdn.microsoft.com/library/windows/apps/br226206)에서 사용할 수 있는 인코딩 옵션에 대해 설명합니다. 인코딩 옵션은 문자열인 이름과 특정 데이터 형식([**Windows.Foundation.PropertyType**](https://msdn.microsoft.com/library/windows/apps/br225871))의 값으로 정의됩니다. 이미지 작업 방법은 [비트맵 이미지 만들기, 편집 및 저장](imaging.md)을 참조하세요.

| 이름                    | PropertyType | 사용법 참고 사항                                                                                        | 유효한 형식 |
|-------------------------|--------------|----------------------------------------------------------------------------------------------------|---------------|
| ImageQuality            | single       | 유효한 값은 0에서 1.0 사이입니다. 값이 높으면 품질이 더 높습니다.                                 | JPEG, JPEG-XR |
| CompressionQuality      | single       | 유효한 값은 0에서 1.0 사이입니다. 값이 높으면 더 효율적이고 더 느린 압축 체계입니다. | TIFF          |
| Lossless                | boolean      | true로 설정되면 ImageQuality 옵션이 무시됩니다.                                        | JPEG-XR       |
| InterlaceOption         | boolean      | 이미지를 인터레이스할지 여부입니다.                                                                    | PNG           |
| FilterOption            | uint8        | [**PngFilterMode**](https://msdn.microsoft.com/library/windows/apps/br226389) 열거형을 사용합니다.                                | PNG           |
| TiffCompressionMethod   | uint8        | [**TiffCompressionMode**](https://msdn.microsoft.com/library/windows/apps/br226399) 열거형을 사용합니다.                    | TIFF          |
| Luminance               | uint32Array  | 광도 양자화 상수를 포함하는 64개 요소의 배열입니다.                               | JPEG          |
| Chrominance             | uint32Array  | 색상 양자화 상수를 포함하는 64개 요소의 배열입니다.                             | JPEG          |
| JpegYCrCbSubsampling    | uint8        | [**JpegSubsamplingMode**](https://msdn.microsoft.com/library/windows/apps/br226386) 열거형을 사용합니다.                    | JPEG          |
| SuppressApp0            | boolean      | App0 메타데이터 블록이 생성되지 않게 할지 여부입니다.                                        | JPEG          |
| EnableV5Header32bppBGRA | boolean      | 알파를 지원하는 버전 5 BMP로 인코드할지 여부입니다.                                         | BMP           |

 

## <a name="related-topics"></a>관련 항목

* [비트맵 이미지 만들기, 편집 및 저장](imaging.md)
* [지원되는 코덱](supported-codecs.md)

 





