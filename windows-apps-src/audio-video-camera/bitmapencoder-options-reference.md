---
author: drewbatgit
ms.assetid: 98BD79B3-F420-43C5-98D3-52EBDDB479A0
description: "이 문서에서는 BitmapEncoder에서 사용할 수 있는 인코딩 옵션에 대해 설명합니다."
title: "BitmapEncoder 옵션 참조"
translationtype: Human Translation
ms.sourcegitcommit: 6530fa257ea3735453a97eb5d916524e750e62fc
ms.openlocfilehash: 510cb363b258d20688ea212856af4b7ac0311e61

---

# BitmapEncoder 옵션 참조

\[ Windows 10의 UWP 앱에 맞게 업데이트되었습니다. Windows 8.x 문서는 [보관](http://go.microsoft.com/fwlink/p/?linkid=619132)을 참조하세요. \]

이 문서에서는 [**BitmapEncoder**](https://msdn.microsoft.com/library/windows/apps/br226206)에서 사용할 수 있는 인코딩 옵션에 대해 설명합니다. 인코딩 옵션은 문자열인 이름과 특정 데이터 형식([**Windows.Foundation.PropertyType**](https://msdn.microsoft.com/library/windows/apps/br225871))의 값으로 정의됩니다. 이미지 작업에 대한 자세한 내용은 [이미징](imaging.md)을 참조하세요.

| 이름                    | PropertyType | 사용법 참고 사항                                                                                        | 유효한 형식 |
|-------------------------|--------------|----------------------------------------------------------------------------------------------------|---------------|
| ImageQuality            | single       | 유효한 값은 0에서 1.0 사이입니다. 값이 높으면 품질이 더 높습니다.                                 | JPEG, JPEG-XR |
| CompressionQuality      | single       | 유효한 값은 0에서 1.0 사이입니다. 값이 높으면 더 효율적이고 더 느린 압축 구조입니다. | TIFF          |
| Lossless                | boolean      | true로 설정되면 ImageQuality 옵션이 무시됩니다.                                        | JPEG-XR       |
| InterlaceOption         | boolean      | 이미지를 인터레이스할지 여부입니다.                                                                    | PNG           |
| FilterOption            | uint8        | [
            **PngFilterMode**](https://msdn.microsoft.com/library/windows/apps/br226389) 열거형을 사용합니다.                                | PNG           |
| TiffCompressionMethod   | uint8        | [
            **TiffCompressionMode**](https://msdn.microsoft.com/library/windows/apps/br226399) 열거형을 사용합니다.                    | TIFF          |
| Luminance               | uint32Array  | 광도 양자화 상수를 포함하는 64개 요소의 배열입니다.                               | JPEG          |
| Chrominance             | uint32Array  | 색상 양자화 상수를 포함하는 64개 요소의 배열입니다.                             | JPEG          |
| JpegYCrCbSubsampling    | uint8        | [
            **JpegSubsamplingMode**](https://msdn.microsoft.com/library/windows/apps/br226386) 열거형을 사용합니다.                    | JPEG          |
| SuppressApp0            | boolean      | App0 메타데이터 블록이 생성되지 않게 할지 여부입니다.                                        | JPEG          |
| EnableV5Header32bppBGRA | boolean      | 알파를 지원하는 버전 5 BMP로 인코딩할지 여부입니다.                                         | BMP           |

 

## 관련 항목

* [이미징](imaging.md)
 

 







<!--HONumber=Jun16_HO4-->


