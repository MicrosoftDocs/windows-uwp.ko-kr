---
title: 서비스 기본 사항 가리키기
description: 이 문서에는 PointOfService Windows 런타임 Api를 시작 하는 방법에 대 한 정보가 포함 되어 있습니다.
ms.date: 12/3/2019
ms.topic: article
keywords: windows 10, uwp, 서비스 지점, pos
ms.localizationpriority: medium
ms.openlocfilehash: cb8acf5f7dce8e81d72850a4dbd097083b240864
ms.sourcegitcommit: ef723e3d6b1b67213c78da696838a920c66d5d30
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 05/02/2020
ms.locfileid: "82730041"
---
# <a name="point-of-service-basics"></a>서비스 기본 사항 가리키기

이 섹션에는 모든 서비스 장치 범주에 공통 된 항목이 포함 되어 있습니다.

|항목 |Description |
|------|------------|
| [기능 선언](pos-basics-capability.md)      | 응용 프로그램 매니페스트에 **Pointofservice** 기능을 추가 하는 방법에 대해 알아봅니다.  이 기능은 Windows. PointOfService 네임 스페이스를 사용 하는 데 필요 합니다.  |
| [디바이스 열거](pos-basics-enumerating.md)        | 시스템에서 사용할 수 있는 장치를 쿼리 하는 데 사용 되는 장치 선택기를 정의 하 고이 선택기를 사용 하 여 서비스의 지점을 열거 하는 방법을 알아봅니다.  |
| [디바이스 개체 만들기](pos-basics-deviceobject.md)  | 주변 장치에 대 한 읽기 전용 속성에 대 한 액세스를 제공 하 고 단독 사용을 위해 주변 장치를 청구 하는 PointOfService 장치 개체를 만드는 방법에 대해 알아봅니다. |
| [클레임 및 사용](pos-basics-claim.md)  | 독점적으로 사용 하 고 i/o 작업에 사용 하도록 설정 하는 PointOfService 주변 장치를 예약 하는 방법에 대해 알아봅니다.  |
| [주변 기기를 다른 사용자와 공유](pos-basics-sharing.md) | 여러 Pc가 각 컴퓨터에 연결 된 전용 주변 장치가 아닌 공유 주변 장치를 사용 하는 환경에서 네트워크 또는 Bluetooth 연결 주변 장치를 다른 컴퓨터와 공유 하는 방법에 대해 알아봅니다.
| [PointOfService 종단 간](pos-get-started.md)  | 위의 예제를 활용 하 여 PointOfService 주변 장치와 상호 작용 하는 방법에 대 한 종단 간 예제입니다. |
|

## <a name="see-also"></a>참조

| 항목   | Description |
|:--------|:------------|
| [응용 프로그램 배포](../publish/distribute-lob-apps-to-enterprises.md) | 엔터프라이즈 고객에 게 앱을 배포 하기 위한 옵션에 대해 알아봅니다. |
| [애플리케이션 수명 주기](../launch-resume/app-lifecycle.md) | UWP 응용 프로그램의 수명 주기와 Windows가 앱을 시작, 일시 중단 및 다시 시작할 때 발생 하는 상황에 대해 알아봅니다. |
| [애플리케이션 리소스](../app-resources/index.md) | 앱의 문자열, 이미지 및 파일 리소스를 작성 하 고 패키지 하 고 사용 하는 방법에 대해 알아봅니다. |
| [데이터 바인딩](../data-binding/index.md) | 데이터 바인딩을 사용 하 여 응용 프로그램의 UI에 데이터를 표시 하는 방법을 알아봅니다. |
| [장치 열거](enumerate-devices.md) | 고급 열거 기술을 사용 하 여 주변 장치를 찾는 방법을 알아봅니다.|
| [버전 적응 응용 프로그램](../debug-test-perf/version-adaptive-apps.md) | 여러 버전의 Windows 10에서 실행 되도록 앱을 설계 하는 방법을 설명 합니다.|
|


## <a name="sample-code"></a>예제 코드
+ [바코드 스캐너 샘플](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/BarcodeScanner)
+ [현금 서랍 샘플]( https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/CashDrawer)
+ [줄 표시 샘플](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/LineDisplay)
+ [자기 줄무늬 판독기 샘플](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/MagneticStripeReader)
+ [POSPrinter 샘플](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/PosPrinter)
