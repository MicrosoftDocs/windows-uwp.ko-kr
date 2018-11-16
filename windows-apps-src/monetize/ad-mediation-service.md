---
author: Xansky
description: Microsoft 광고 조정 서비스로 여러 광고 네트워크에서 광고를 표시해 광고 수익과 앱 홍보 기능을 극대화 할 수 있습니다.
title: Microsoft 광고 조정 서비스
ms.author: mhopkins
ms.date: 06/05/2018
ms.topic: article
keywords: Windows 10 uwp, 광고, 광고, 광고 조정
ms.localizationpriority: medium
ms.openlocfilehash: 9adae5b000277b774536c8b307cc1bc055ce3bc4
ms.sourcegitcommit: e38b334edb82bf2b1474ba686990f4299b8f59c7
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/15/2018
ms.locfileid: "6837683"
---
# <a name="microsoft-ad-mediation-service"></a>Microsoft 광고 조정 서비스

[Microsoft Advertising SDK](http://aka.ms/ads-sdk-uwp)를 사용하여 [앱에서 광고를 표시](display-ads-in-your-app.md)할 때, 필요에 따라 Microsoft 광고 조정 서비스를 사용하여 광고 수익을 최대화할 수 있습니다. 이 문서에서는 광고 조정 서비스와 그 목표에 대해 설명합니다.

광고 조정 서비스는 [Microsoft 광고 수익 창출 플랫폼](https://developer.microsoft.com/windows/ad-monetization-platform)의 일부입니다. 이 플랫폼은 다음 부분으로 구성됩니다.

![addreferences](images/ad-mediation-service.png)

광고 조정 서비스는 이러한 기능을 구축하여 앱에 대한 광고 수익을 최대화하는 것을 목표로 합니다.

## <a name="diversity-of-demand-by-market-and-format"></a>다양한 지역/국가 및 형식

광고 조정 서비스는 광고 네트워크의 다양한 여러 광고 형식(배너, 중간 배너, 중간 비디오 및 기본 광고)을 통합합니다. 광고 조정 서비스는 최종 사용자의 참여 가능성이 가장 높은 광고를 제공할 수 있도록 각 광고 네트워크에서 작동합니다. 다른 광고 파트너를 우수한 품질과 다양한 수요를 확장하기 위해 계속 파트너를 추가할 예정입니다.

## <a name="manage-complexity-of-ad-network-relationships"></a>광고 네트워크 관계의 복잡성 관리  

이 작업을 할 필요가 없도록 광고 조정 서비스는 다양한 광고 네트워크에 통합되어 있습니다. 광고 조정 설정을 수정할 수 후 Microsoft Advertising SDK를 사용 하 여 앱에서 광고를 표시 하려면 [파트너 센터에서](../publish/in-app-ads.md#mediation-settings) 여러 광고 네트워크의 광고를 표시 합니다. 코드를 변경하지 않고 새 광고 네트워크에서 광고를 표시하는 혜택을 얻을 수 있습니다.

귀하를 대신하여 광고 네트워크와의 종단 간 관계를 관리합니다. 광고 네트워크 통합에서 광고 제공까지 귀하가 더 이상 추가 작업할 필요 없이 Microsoft가 보고 및 지급을 모두 담당합니다.

## <a name="machine-learning-based-yield-optimization-algorithms"></a>기계 학습 기반 산출 최적화 알고리즘

광고 조정 서비스는 개발자를 위한 최고의 산출을 얻기 위해 작동합니다. 이를 위해 기계 학습 기반 산출 최적화 알고리즘을 채택합니다. 모든 광고 호출에 대해 알고리즘은 고객의 수익을 최대화하기 위해 호출해야 할 광고 네트워크 중 최고의 *일련의* 주문을 결정합니다. 이는 다음을 포함한 여러 가지 요소를 고려하여 이루어집니다.

* 요청하는 사용자.
* 요청의 대상인 응용 프로그램.
* 광고 네트워크의 이전 성능.
* 요청이 이루어지는 광고 형식 및 지역/국가.
* 광고를 호출한 시간.
* 앱 콘텐츠의 범주.
* 사용자 세그먼트.
* 사용자 위치.

자동으로 새로운 광고 네트워크가 포함되며 학습 예산을 통해 성능이 평가됩니다. 짧은 시간 내에 일련의 광고에서 위치를 찾습니다. 이렇게 하면 광고 네트워크의 경쟁력이 놓아지며 개발자는 앱을 통해 가장 높은 수익을 창출할 수 있습니다.

우리의 [권장 조정 설정](../publish/in-app-ads.md#mediation-settings)을 사용하여 앱의 광고에서 수익을 극대화하는 것이 좋습니다. 이를 사용하면 알고리즘이 앱을 위한 최상의 수익을 산출할 수 있습니다. 그러나 마음껏 제어를 강화 하 고 광고를 수행 하는 순서는 광고 네트워크에 파트너 센터에서 자체 조정 설정을 선택할 수도 있습니다.

## <a name="rich-data-and-signals"></a>풍부한 데이터와 신호

광고 조정 서비스는 각 사용자에게 보다 관련성 있는 광고를 표시하도록 사용자 대상 지정을 개선하기 위해 다양한 광고 네트워크에서 작동합니다. 이는 개인 정보 보호 요구 사항에 주의하면서 광고 네트워크에 사용자와 앱에 대한 보다 풍부한 신호를 전송하여 수행됩니다.

## <a name="related-topics"></a>관련 항목

* [Microsoft Advertising SDK](http://aka.ms/ads-sdk-uwp)
* [조정 설정](../publish/in-app-ads.md#mediation-settings)
* [광고 성과 보고서](../publish/advertising-performance-report.md)
