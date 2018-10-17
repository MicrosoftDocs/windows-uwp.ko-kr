---
author: JnHs
Description: Target specific segments of your customers with personalized content to increase engagement, retention, and monetization.
title: 대상 제품을 사용하여 참여 및 변환 최대화
ms.author: wdg-dev-content
ms.date: 11/10/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp, 대상 제품, 제품, 알림
ms.localizationpriority: medium
ms.openlocfilehash: 727c438bacf51fd2ead03df72421363116c4701b
ms.sourcegitcommit: 1c6325aa572868b789fcdd2efc9203f67a83872a
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/17/2018
ms.locfileid: "4745619"
---
# <a name="use-targeted-offers-to-maximize-engagement-and-conversions"></a>대상 제품을 사용하여 참여 및 변환 최대화

특정 고객 세그먼트를 대상으로 소구력이 있는 개인화된 콘텐츠를 제공해 참여와 유지, 수익 창출을 향상시킵니다.

> [!IMPORTANT]
> 대상 제품은 추가 기능이 포함된 UWP 앱에서만 사용할 수 있습니다.

## <a name="targeted-offer-overview"></a>대상 제품 개요

대상 제품을 사용하려면 상위 수준에서 세 가지를 수행해야 합니다.

1. **대시보드에서 대상 제품을 만듭니다.** **참여 > 대상 제품** 페이지로 이동해 대상 제품을 만듭니다. 이 프로세스에 대한 자세한 내용은 다음과 같습니다.
2. **앱 내 대상 제품 경험을 구현합니다.** 사용 하 여 *Microsoft Store 대상 제품 API* 앱 코드에서 특정된 사용자에 대 한 사용 가능한 제품 검색 합니다. 또 대상 제품에 대한 앱 내 환경을 만들어야 합니다. 자세한 내용은 [스토어 서비스를 사용하여 대상 제품 관리](../monetize/manage-targeted-offers-using-windows-store-services.md)를 참조하세요.
3. **스토어에 앱을 제출합니다.** 대상 제품(들)을 고객이 사용할 수 있도록 앱 내 대상 제품 경험을 구현한 상태에서 앱을 게시해야 합니다.

이 단계를 완료하면, 앱을 사용하는 고객이 대상 제품과 관련된 세그먼트(들)의 구성원 자격 여부에 따라 해당 시점에 사용 가능한 대상 제품을 확인할 수 있습니다. Microsoft는 여러분의 고객들에게 사용 가능한 모든 대상 제품을 제시하기 위해 모든 노력을 다할 것입니다. 그러나 대상 제품 사용에 영향을 주는 문제들이 간헐적으로 발생할 수 있습니다.


## <a name="to-create-and-send-a-targeted-offer"></a>대상 제품 만들기 및 보내기

다음 단계를 적용하여 대시보드에서 대상 제품을 만드세요.

1.  Windows 개발자 센터 대시보드의 왼쪽 탐색 메뉴에서 **참여**를 확장하고 **대상 제품**을 선택합니다.
2.  **대상 제품** 페이지에서 사용 가능한 대상 제품을 확인합니다. 구현하고 싶은 대상 제품에 대해 **새 대상 제품 만들기**를 선택합니다.

    > [!NOTE]
    > 확인할 수 있는 사용 가능 대상 제품은 시기와 계정 기준에 따라 달라집니다.

3.  사용 가능한 대상 제품 아래에 표시되는 새 행에서 대상 제품이 제공되는 제품(앱)을 선택합니다. 그런 다음 해당 제품과 연결할 추가 기능을 선택합니다.
4.  또 다른 대상 제품을 만들려면 2단계와 3단계를 반복합니다. 각 대상 제품에서 서로 다른 추가 기능을 선택하기만 하면 동일한 앱에 동일한 대상 제품을 한 차례 이상 구현할 수 있습니다. 또 동일한 추가 기능에 여러 개의 대상 제품 유형을 연결할 수 있습니다.
5.  대상 제품 만들기를 마쳤으면 **저장**을 클릭합니다.

대상 제품을 구현하고 나면 대시보드의 **대상 제품** 페이지로 돌아가서 각 대상 제품의 총 변환 수를 확인할 수 있습니다.

대상 제품을 사용하지 않기로 결정했다면(또는 더 이상 사용하지 않는 경우), **삭제**를 클릭합니다.

> [!IMPORTANT]
> 특정 사용자를 대상으로 사용 가능한 대상 제품을 검색하고 앱 내 경험을 구현하기 위한 코드를 게시했는지 확인합니다. 자세한 내용은 [스토어 서비스를 사용하여 대상 제품 관리](../monetize/manage-targeted-offers-using-windows-store-services.md)를 참조하세요.
>
> 대상 제품 콘텐츠를 고려할 때, 앱 콘텐츠와 마찬가지로 대상 제품의 콘텐츠는 스토어 [콘텐츠 정책](https://docs.microsoft.com/en-us/legal/windows/agreements/store-policies)을 준수해야 한다는 점에 유의하세요.
>
> 여러분의 앱을 사용하는 고객(또는 고객층 멤버 자격을 판단할 때 Microsoft 계정으로 로그인이 된) 장치를 다른 사람에게 사용하라고 주었을 경우, 원래 목표한 고객이 아닌 다른 사람이 대상 제품을 확인할 수 있음을 기억하세요.
