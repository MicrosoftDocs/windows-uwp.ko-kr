---
description: 고객의 특정 세그먼트를 개인 설정 된 콘텐츠로 대상으로 지정 하 여 engagement, 보존 및 수익 화를 늘립니다.
title: 대상 제공 서비스를 사용 하 여 참여 및 변환 최대화
ms.date: 10/31/2018
ms.topic: article
keywords: windows 10, uwp, 대상 제공, 제안, 알림
ms.localizationpriority: medium
ms.openlocfilehash: 744ea22f741cd3f8a0e43d3eb88b1a7c6f4ce68b
ms.sourcegitcommit: a3bbd3dd13be5d2f8a2793717adf4276840ee17d
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/30/2020
ms.locfileid: "93034936"
---
# <a name="use-targeted-offers-to-maximize-engagement-and-conversions"></a>대상 제공 서비스를 사용 하 여 참여 및 변환 최대화

고객의 특정 세그먼트를 대상으로 하는 맞춤형 콘텐츠가 있는 고객의 특정 세그먼트를 대상으로 하 여 engagement, 보존 및 수익 화를 늘립니다.

> [!IMPORTANT]
> 대상 제공 제품은 추가 기능을 포함 하는 UWP 앱 에서만 사용할 수 있습니다.

## <a name="targeted-offer-overview"></a>대상 제안 개요

높은 수준에서 대상 제품을 사용 하려면 다음 세 가지를 수행 해야 합니다.

1. **[파트너 센터](https://partner.microsoft.com/dashboard)에서 제품을 만듭니다.** 제품 **> 대상 제공** 페이지로 이동 하 여 제품을 만듭니다. 이 프로세스에 대 한 자세한 정보는 아래에 설명 되어 있습니다.
2. **앱에서 제공 하는 환경을 구현 합니다.** 앱의 코드에서 *대상 제공 제품 API Microsoft Store* 사용 하 여 지정 된 사용자에 게 제공 되는 제품을 검색 합니다. 또한 대상 제품에 대 한 앱 내 환경을 만들어야 합니다. 자세한 내용은 [Store 서비스를 사용 하 여 대상 제품 관리](../monetize/manage-targeted-offers-using-windows-store-services.md)를 참조 하세요.
3. **스토어에 앱을 제출 합니다.** 앱은 고객에 게 제공 되는 제품을 제공 하기 위해 앱에서 제공 하는 환경에서 게시 해야 합니다.

이러한 단계를 완료 하면 앱을 사용 하는 고객에 게 제품에 연결 된 세그먼트의 멤버 자격을 기준으로 해당 시점에 제공 되는 제품이 표시 됩니다. 고객에 게 제공 되는 모든 제품을 표시 하는 데 모든 노력이 필요 하지만, 경우에 따라 제품 가용성에 영향을 주는 문제가 있을 수 있습니다.


## <a name="to-create-and-send-a-targeted-offer"></a>대상 제품을 만들고 보내려면

1.  [파트너 센터](https://partner.microsoft.com/dashboard)에서 왼쪽 탐색 메뉴의 **참여** 를 확장 한 다음 **대상 제품** 을 선택 합니다.
2.  **대상 제품** 페이지에서 사용 가능한 제품을 검토 합니다. 구현 하려는 모든 제품에 대 한 **새 제품 만들기** 를 선택 합니다.

    > [!NOTE]
    > 사용 가능한 제안은 시간이 지남에 따라 그리고 계정 기준에 따라 다를 수 있습니다.

3.  사용 가능한 제품 아래에 표시 되는 새 행에서 제품을 사용할 수 있는 제품 (앱)을 선택 합니다. 그런 다음 제품에 연결 하려는 추가 기능을 선택 합니다.
4.  추가 제안을 만들려는 경우 2 단계와 3 단계를 반복 합니다. 각 제품에 대해 다른 추가 기능을 선택 하기만 하면 동일한 앱에 대해 동일한 제품 유형을 두 번 이상 구현할 수 있습니다. 또한 둘 이상의 제안 유형과 동일한 추가 기능을 연결할 수 있습니다.
5.  제안 만들기를 완료 하면 **저장** 을 클릭 합니다.

제품을 구현한 후 파트너 센터의 **대상 제공** 페이지로 돌아와서 각 제품에 대 한 총 변환을 볼 수 있습니다.

제안을 사용 하지 않기로 결정 한 경우 (또는 더 이상 사용 하지 않으려는 경우 삭제를 클릭 **합니다.**

> [!IMPORTANT]
> 지정 된 사용자에 대해 사용 가능한 제품을 검색 하 고 앱 내 환경을 만들려면 코드를 게시 해야 합니다. 자세한 내용은 [Store 서비스를 사용 하 여 대상 제품 관리](../monetize/manage-targeted-offers-using-windows-store-services.md)를 참조 하세요.
>
> 대상 제품의 콘텐츠를 고려 하는 경우 모든 앱 콘텐츠와 마찬가지로 제품의 콘텐츠가 스토어 [콘텐츠 정책을](/legal/windows/agreements/store-policies)준수 해야 한다는 점에 유의 하세요.
>
> 또한 앱을 사용 하는 고객 (세그먼트 멤버 자격이 결정 되는 시점에 해당 Microsoft 계정로 로그인 한 경우)에서 장치를 다른 사람에 게 제공 하는 경우 다른 사용자는 원래 고객을 대상으로 하는 제품을 볼 수 있습니다.
