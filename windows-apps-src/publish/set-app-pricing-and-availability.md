---
description: 앱 제출 프로세스의 가격 책정 및 가용성 페이지를 사용 하 여 앱의 비용, 무료 평가판 제공 여부, 고객에 게 제공 되는 방법 및 시기를 결정할 수 있습니다.
title: 앱 가격 책정 및 가용성 설정
ms.assetid: 37BE7C25-AA74-43CD-8969-CBA3BD481575
ms.date: 10/31/2018
ms.topic: article
keywords: windows 10, uwp, 가격, 사용 가능, 검색 가능, 무료 평가판, 평가판, 평가판, 앱, 릴리스 날짜
ms.localizationpriority: medium
ms.openlocfilehash: f7373ae49b867e9fb1b59f6d7fb18e32f4d7bf65
ms.sourcegitcommit: a3bbd3dd13be5d2f8a2793717adf4276840ee17d
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/30/2020
ms.locfileid: "93029926"
---
# <a name="set-app-pricing-and-availability"></a>앱 가격 책정 및 가용성 설정


[앱 제출 프로세스](app-submissions.md) 의 **가격 책정 및 가용성** 페이지를 사용 하 여 앱의 비용, 무료 평가판 제공 여부, 고객에 게 제공 되는 방법 및 시기를 결정할 수 있습니다. 여기서는이 페이지의 옵션과이 정보를 입력할 때 고려해 야 할 사항에 대해 살펴봅니다.


## <a name="markets"></a>시장

Microsoft Store은 전 세계 240 국가의 지역에서 고객에 게 도달 합니다. 기본적으로 모든 가능한 시장에서 앱을 제공 합니다. 원한다 면 앱을 제공할 특정 시장을 선택할 수 있습니다. 

자세한 내용은 [시장 선택 정의](./define-market-selection.md)를 참조 하세요.


## <a name="visibility"></a>가시 거리

**표시 유형** 섹션에서 사용자가 스토어에서 앱을 찾을 수 있는지 여부를 비롯 하 여 앱을 검색 하 고 가져오는 방법에 대 한 제한을 설정할 수 있습니다.

자세한 내용은 [표시 여부 옵션 선택](choose-visibility-options.md)을 참조하세요.


## <a name="schedule"></a>예약

기본적으로 **이 앱을 사용 가능 하도록 설정 섹션의 스토어 옵션에서 검색 하지 않고이 앱 사용** 을 선택 하지 [Visibility](choose-visibility-options.md#discoverability) 않은 경우 앱은 인증을 전달 하 고 게시 프로세스를 완료 하는 즉시 고객에 게 제공 됩니다. 다른 날짜를 선택 하려면 **옵션 표시** 를 선택 하 여이 섹션을 확장 합니다. 

자세한 내용은 [정확한 릴리스 예약 구성](configure-precise-release-scheduling.md)을 참조 하세요.


## <a name="pricing"></a>가격 책정

**무료** 또는 사용 가능한 가격 책정 계층 중 하나를 선택 **하 여 앱** 에 대 한 기본 가격을 선택 해야 합니다 ( **이 앱을 사용할 수 있지만,이 앱을 사용할 수 있지만 저장소에서 검색할 수 없음** 옵션을 선택 하지 않은 경우). [Visibility](choose-visibility-options.md#discoverability) 응용 프로그램의 가격이 변경 되어야 하는 날짜 및 시간을 나타내도록 가격 변경을 예약할 수도 있습니다. 또한 특정 시장에 대해 이러한 변경을 사용자 지정 하는 옵션이 있습니다. Microsoft는 다양 한 시장의 통화 변동을 고려 하 여 권장 가격을 주기적으로 업데이트 합니다. 권장 가격이 변경 되 면 선택한 가격이 새 권장 값에 맞춰지지 않은 경우 가격 책정 영역에 경고 표시기가 표시 됩니다. 제품의 가격은 변경 되지 않으며 이러한 가격을 업데이트 하려는 시기와 시기를 제어할 수 있습니다. 

자세한 내용은 [앱 가격 설정 및 예약](set-and-schedule-app-pricing.md)을 참조하세요.


## <a name="free-trial"></a>평가판

대부분의 개발자는 고객이 스토어에서 제공 하는 평가판 기능을 사용 하 여 앱을 무료로 사용해 볼 수 있도록 합니다. 기본적으로 **무료 평가판** 은 선택 되지 않으며 앱에 대 한 평가판이 없습니다. 평가판을 제공 하려는 경우 **무료 평가판** 드롭다운에서 값을 선택할 수 있습니다.

선택할 수 있는 평가판에는 두 가지가 있으며, 평가판을 시작 하 고 제공 하는 것을 중지 하는 날짜 및 시간을 구성 하는 옵션을 사용할 수 있습니다.

### <a name="time-limited"></a>시간 제한

**시간 제한** 을 선택 하 여 고객이 특정 기간 (일, **7 일** , **15 일** 또는 **30 일** ) 동안 무료로 앱을 사용해 볼 수 있도록 **합니다.** [평가판 버전에서 기능을 제외 하거나 제한](../monetize/in-app-purchases-and-trials.md)하는 코드를 추가 하 여 기능을 제한 하거나, 해당 기간 동안 고객이 전체 기능에 액세스할 수 있도록 할 수 있습니다. 
> [!NOTE]
> 시간이 제한 된 평가판은 Windows 10 build 10.0.10586 또는 이전 버전의 고객 또는 Windows Phone 8.1 이전 버전의 고객에 게 표시 되지 않습니다.

### <a name="unlimited"></a>제한 없음

고객이 **무제한** 으로 무료로 앱에 액세스할 수 있도록 하려면 무제한을 선택 합니다. 정식 버전을 구입 하는 것이 좋습니다. 따라서 [평가판 버전에서 기능을 제외 하거나 제한 하는](../monetize/in-app-purchases-and-trials.md)코드를 추가 해야 합니다.

### <a name="start-and-end-dates"></a>시작 및 끝 날짜

기본적으로 평가판은 앱을 게시 하는 즉시 사용할 수 있으며, 제공 되는 것은 중단 되지 않습니다. 원하는 경우 평가판을 제공 하기 시작 하는 날짜와 시간을 지정 하 고, 평가판을 제공 하지 않을 시기를 지정할 수 있습니다. 

>[!NOTE]
> 이러한 날짜는 Windows 10 (Xbox 포함)의 고객에만 적용 됩니다. 앱이 이전 OS 버전의 고객에 게 제공 되는 경우 제품을 사용할 수 있는 동안 평가판이 고객에 게 제공 됩니다. 

Windows 10의 고객에 게 평가판을 제공 해야 하는 날짜를 설정 하려면 드롭다운 **시작** 및/또는 **종료** 날짜를 **다음으로 변경** 하 고 날짜 및 시간을 선택 합니다. 이렇게 하는 경우 선택 하는 시간이 UTC (협정 세계시)가 되도록 **utc** 를 선택 하거나 시장에 연결 된 각 표준 시간대에서 이러한 시간을 사용 하도록 **로컬** 을 선택할 수 있습니다. (둘 이상의 표준 시간대를 포함 하는 시장의 경우 해당 시장의 표준 시간대 하나만 사용 됩니다. 미국의 경우 동부 표준 시간대가 사용 됩니다. 시장에 대해 다른 날짜를 설정 하려는 경우 **특정 시장의 사용자 지정** 을 선택할 수 있습니다.


## <a name="sale-pricing"></a>판매 가격

제한 된 기간 동안 저렴 한 가격으로 앱을 제공 하려는 경우 판매를 만들고 예약할 수 있습니다.

자세한 내용은 [판매에 앱 및 추가 기능 배치](put-apps-and-add-ons-on-sale.md)를 참조 하세요.


## <a name="organizational-licensing"></a>조직 라이선스

기본적으로 앱을 조직에 제공 하 여 볼륨에서 구매할 수 있습니다. 이 섹션에서 앱을 제공할 수 있는지 여부와 방법을 지정할 수 있습니다.

자세한 내용은 [조직 라이선스 옵션](organizational-licensing.md)을 참조 하세요.


## <a name="publish-date"></a>게시 날짜

이전에는이 페이지에 **게시 날짜** 섹션이 나타났습니다. 이제이 기능을 [제출 옵션](manage-submission-options.md) 페이지의 **게시 보류 옵션** 섹션에서 찾을 수 있습니다. 앱을 스토어에 게시 해야 하는 경우를 제어 하려면 **가격 책정 및 가용성** 페이지의 [일정](configure-precise-release-scheduling.md) 섹션을 사용 하는 것이 좋습니다.
