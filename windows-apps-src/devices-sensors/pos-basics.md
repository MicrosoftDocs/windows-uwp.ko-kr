---
author: TerryWarwick
title: 서비스 지점 시작하기
description: 이 문서는 PointOfService UWP API 시작에 대한 정보를 포함하고 있습니다.
ms.author: jken
ms.date: 05/1/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp, 서비스 지점, pos
ms.localizationpriority: medium
ms.openlocfilehash: a0583adbcef9e45dfe0b2e56e03ce7e0451ac5bb
ms.sourcegitcommit: ce45a2bc5ca6794e97d188166172f58590e2e434
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 06/06/2018
ms.locfileid: "1983546"
---
# <a name="getting-started-with-point-of-service"></a>서비스 지점 시작하기

이 섹션은 모든 서비스 지점 장치 범주에 공통된 항목을 포함합니다.

|항목 |설명 |
|------|------------|
| [접근 권한 값 선언](pos-basics-capability.md)      | 응용 프로그램 매니페스트에 **pointOfService** 접근 권한 값을 추가하는 방법을 알아보세요.  이 접근 권한 값은 Windows.Devices.PointOfService 네임스페이스의 사용에 필요합니다.  |
| [장치 열거](pos-basics-enumerating.md)        | 시스템에서 사용할 수 있는 장치를 쿼리하는 데 사용되는 장치 선택기를 정의하고 이 선택기를 사용하여 서비스 지점 장치를 열거하는 방법을 배웁니다.  |
| [장치 개체 만들기](pos-basics-deviceobject.md)  | 주변 장치의 읽기 전용 속성에 액세스하고 주변 장치에 대한 단독 사용을 클레임하는 PointOfService 장치 개체를 만드는 방법을 알아봅니다. |
| [장치에 대한 단독 사용 클레임 ](pos-basics-claim.md)  | 단독으로 사용해야 할 경우 다른 응용 프로그램이 동일한 컴퓨터에서 PointOfService 주변 장치에 액세스하는 것을 허용하면서도 PointOfService 클레임 모델로 PointOfService 주변 장치에 대한 단독 사용을 예약하는 방법을 알아보세요.  |
|

## <a name="see-also"></a>기타 참조
[Windows.Devices.PointOfService 시작](pos-get-started.md)


## <a name="sample-code"></a>샘플 코드
+ [바코드 스캐너 샘플](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/BarcodeScanner)
+ [현금 출납기 샘플]( https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/CashDrawer)
+ [라인 디스플레이 샘플](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/LineDisplay)
+ [자기 띠 판독기 샘플](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/MagneticStripeReader)
+ [POSPrinter 샘플](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/PosPrinter)

