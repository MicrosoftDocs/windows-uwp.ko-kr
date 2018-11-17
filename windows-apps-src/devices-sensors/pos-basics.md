---
author: TerryWarwick
title: 서비스 지점 시작하기
description: 이 문서는 PointOfService UWP API 시작에 대한 정보를 포함하고 있습니다.
ms.author: jken
ms.date: 06/13/2018
ms.topic: article
keywords: windows 10, uwp, 서비스 지점, pos
ms.localizationpriority: medium
ms.openlocfilehash: 46dd1f615e42f6e89ee9a92cb980299e9a0e5205
ms.sourcegitcommit: 3257416aebb5a7b1515e107866806f8bd57845a8
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/17/2018
ms.locfileid: "7151840"
---
# <a name="getting-started-with-point-of-service"></a>서비스 지점 시작하기

## <a name="pointofservice-basics"></a>PointOfService 기본 사항

이 섹션은 모든 서비스 지점 장치 범주에 공통된 항목을 포함합니다.

|항목 |설명 |
|------|------------|
| [접근 권한 값 선언](pos-basics-capability.md)      | 응용 프로그램 매니페스트에 **pointOfService** 접근 권한 값을 추가하는 방법을 알아보세요.  이 접근 권한 값은 Windows.Devices.PointOfService 네임스페이스의 사용에 필요합니다.  |
| [장치 열거](pos-basics-enumerating.md)        | 시스템에서 사용할 수 있는 장치를 쿼리하는 데 사용되는 장치 선택기를 정의하고 이 선택기를 사용하여 서비스 지점 장치를 열거하는 방법을 배웁니다.  |
| [장치 개체 만들기](pos-basics-deviceobject.md)  | 주변 장치의 읽기 전용 속성에 액세스하고 주변 장치에 대한 단독 사용을 클레임하는 PointOfService 장치 개체를 만드는 방법을 알아봅니다. |
| [클레임 및 사용 ](pos-basics-claim.md)  | PointOfService 주변 장치에 대 한 단독 사용을 예약 하 고 I/O 작업에 사용 하도록 설정 하는 방법을 알아봅니다.  |
| [주변 장치를 다른 사용자와 공유](pos-basics-sharing.md) | 각 컴퓨터에 연결 된 전용된 주변 아닌 공유 주변 장치에 여러 Pc 의존할 환경에서 다른 컴퓨터와 네트워크 또는 Bluetooth로 연결 된 주변 장치를 공유 하는 방법을 알아봅니다.
| [PointOfService 종단 간](pos-get-started.md)  | 위의 예제를 활용 하는 PointOfService 주변 장치를 조작 하는 방법의 종단 간 예제입니다. |
|

## <a name="see-also"></a>참고 항목

| 항목   | 설명 |
|:--------|:------------|
| [응용 프로그램 배포](../publish/distribute-lob-apps-to-enterprises.md) | 엔터프라이즈 고객에 게 앱을 배포 하기 위한 옵션에 알아봅니다. |
| [응용 프로그램 수명 주기](../launch-resume/app-lifecycle.md) | UWP 응용 프로그램의 수명 주기 및 Windows 시작, 일시 중단 및 앱을 다시 시작할 때 일어날 알아봅니다. |
| [응용 프로그램 리소스](../app-resources/index.md) | 패키지를 작성 하 고 앱의 문자열, 이미지 및 파일 리소스를 소비 하는 방법을 알아봅니다. |
| [데이터 바인딩](../data-binding/index.md) | 데이터 바인딩을 사용 하 여 앱의 UI에 데이터를 표시 하는 방법을 알아봅니다. |
| [장치 열거](enumerate-devices.md) | 주변 장치 찾기를 사용 하 여 고급 열거 기술을 알아봅니다.|
| [버전 적응 응용 프로그램](../debug-test-perf/version-adaptive-apps.md) | 여러 버전의 Windows 10에서 실행 되도록 앱을 디자인 하는 방법에 대해 설명 합니다.|
|


## <a name="sample-code"></a>샘플 코드
+ [바코드 스캐너 샘플](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/BarcodeScanner)
+ [현금 출납기 샘플]( https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/CashDrawer)
+ [라인 디스플레이 샘플](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/LineDisplay)
+ [자기 띠 판독기 샘플](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/MagneticStripeReader)
+ [POSPrinter 샘플](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/PosPrinter)

