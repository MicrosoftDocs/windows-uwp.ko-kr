---
title: 카메라 바코드 스캐너 시스템 요구 사항
description: 이 문서에는 UWP 앱에서 카메라 바코드 스캐너를 사용하기 위한 요구 사항이 나와 있습니다.
ms.date: 05/02/2018
ms.topic: article
keywords: windows 10, uwp, 서비스 지점, pos
ms.localizationpriority: medium
ms.openlocfilehash: 4eed59e302bc34e93d21adef794de02427f2933e
ms.sourcegitcommit: 0dec04de501a3db6b22dfd4a320fc09b5c4a21b5
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 09/04/2019
ms.locfileid: "70243278"
---
# <a name="camera-barcode-scanner-system-requirements"></a>카메라 바코드 스캐너 시스템 요구 사항
Windows 10, 버전 1803부터는 유니버설 Windows 응용 프로그램에서 기준 카메라 렌즈를 통해 바코드를 읽을 수 있습니다.

## <a name="supported-windows-editions"></a>지원되는 Windows 버전
- S 모드의 Windows 10 Professional
- Windows 10 Professional
- Windows 10 Enterprise
- Windows 10 IOT Core


## <a name="webcam-requirements"></a>웹캠 요구 사항
| 범주      | 권장           | 주석 |
| ------------- | ------------------------ | -------- |
| 포커스         | 자동 초점               | 고정 초점은 권장되지 않음 |
| 해결 방법    | 1920 x 1440 이상    | 1920 x 1440 이상의 해상도를 처리할 수 있는 카메라를 통해 최상의 환경을 구축했습니다.  일부 저해상도 카메라에서는 바코드가 충분히 크게 인쇄되는 경우에 표준 바코드를 읽을 수 있습니다. 슬림한 요소를 가진 바코드에서는 고해상도 카메라가 필요할 수 있습니다. |
|

## <a name="see-also"></a>참조

### <a name="samples"></a>샘플

- [바코드 스캐너 샘플](https://github.com/microsoft/Windows-universal-samples/tree/master/Samples/BarcodeScanner)
