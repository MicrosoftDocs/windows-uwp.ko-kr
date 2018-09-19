---
author: jnHs
Description: You can respond directly to reviews of your app to let customers know you’re listening to their feedback.
title: 고객 리뷰에 응답
ms.assetid: 96AA2108-E793-4DD0-8CDA-0D115423C68D
ms.author: wdg-dev-content
ms.date: 7/17/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp, 응답을 응답 검토
ms.localizationpriority: medium
ms.openlocfilehash: 2a043a0b721ee6eabdc3520960ae6da253587c33
ms.sourcegitcommit: 68fcac3288d5698a13dbcbd57f51b30592f24860
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 09/19/2018
ms.locfileid: "4051907"
---
# <a name="respond-to-customer-reviews"></a>고객 리뷰에 응답


고객이 해당 피드백을 듣고는 알 수 있도록 앱의 리뷰에 응답할 수 있습니다. 리뷰 응답에서는 고객 의견을 기반으로 추가한 기능이나 수정한 버그에 대해 고객에게 알려 주거나, 앱 개선 방도에 대한 구체적인 의견을 얻을 수 있습니다. 응답은 모든 Windows 10 고객에 게 표시에 대 한 Microsoft Store에 표시 됩니다. 또한 (하지 않은 옵트아웃 하 고 Windows 10 버전 1803 이상을 실행 하는 장치를 사용 하 는) 경우 전자 메일로 응답을 고객에 게 보낼 수 있습니다.

앱 리뷰를 보고 응답을 제공하려면 Windows 개발자 센터 대시보드에서 해당 앱을 찾습니다. 왼쪽 탐색 메뉴에서 **분석**을 확장하고 **리뷰**를 클릭하여 [리뷰 보고서](reviews-report.md)를 표시합니다. 응답을 제공 하기 위해 **검토에 응답** 을 선택 합니다.

> [!TIP]
> 대시보드를 사용 하 여 리뷰에 응답을 하는 것 외에도 [개발자 센터 앱](https://www.microsoft.com/store/apps/dev-center/9nblggh4r5ws)을 사용 하 여 또는를 리뷰 [프로그래밍 방식으로](../monetize/submit-responses-to-app-reviews.md)응답할 수 있습니다.

기본적으로 응답 원래 고객 리뷰 바로 아래 스토어에 게시 됩니다. 이러한 응답은 Windows 10 장치에서 스토어를 보는 모든 고객에 게 표시 됩니다. 리뷰를 남긴 고객은 Windows 10 버전 1803 이상을 실행 하는 장치를 사용 하 여 메일 응답을 받지 거부 하지 않은 경우 메일에서 응답의 복사본을 해당 고객에 게도 보낼 수 됩니다.  고객에 게 전자 메일에 포함 된 응답을 제출 하려면 유효한 메일 주소를 제공 해야 합니다. 그런 다음 직접 문의할 때이 전자 메일 주소를 사용할 수 있습니다.

스토어에 표시 하 고 고객에 게 메일을 통해 응답 하려면 대신에 대 한 응답 하지 않으려는 경우에 **이 응답을 공개로 설정** 확인란의 선택을 취소 합니다. Note 고객은 메일 응답 수신을 옵트아웃 및/또는 Windows 10 버전 1803 이상을 실행 하지 않는 경우 장치를 사용 하는 경우이 확인란의 선택을 취소 수 없습니다.

## <a name="guidelines-for-responses"></a>응답 지침

고객 리뷰에 응답할 때 다음 지침에 따라야 합니다. 이러한 위치 또는 공개 하는 모든 응답에 적용 됩니다.

> [!IMPORTANT]
> (하지 않은 경우 고객이 원래 리뷰를 수정) 스토어에 게시할 응답을 변경 하려면 하므로 응답을 신중히 검토 하세요 수 없습니다. 고객이 원래 리뷰를 수정 하는 경우 응답 앱의 스토어 목록 페이지에서에서 제거 됩니다. 그런 다음 **응답 업데이트**를 선택 하 여 수정 된 검토에 새 응답을 제출 하는 옵션이 있습니다.

-   응답은 1,000자 이내여야 합니다.
-   앱 평가 변경을 위해 디지털 앱 항목을 포함하여 어떠한 유형의 보상도 사용자에게 제공할 수 없습니다. [앱 개발자 계약](https://docs.microsoft.com/legal/windows/agreements/app-developer-agreement)에 따라 평점 조작은 허용되지 않습니다.
-   응답에 마케팅 콘텐츠나 광고를 포함하지 마세요. 리뷰 작성자는 이미 여러분의 고객입니다.니다.
-   응답에 다른 앱이나 서비스를 홍보하지 마세요.
-   응답은 해당 앱 및 리뷰와 직접적인 관련이 있어야 합니다. 준비된 응답이 동일한 질문을 해결하지 않는다면 동일한 응답을 많은 사용자에게 중복하여 보내면 안 됩니다.
-   응답에 비속어나 공격적, 개인적 또는 악의적인 내용을 포함하지 마세요. 항상 공손하게 응답하고, 앱에 만족하는 고객이 앱을 가장 잘 홍보할 수 있다는 점을 염두에 두세요.

> [!NOTE]
> 고객은 개발자의 부적절한 리뷰 응답을 Microsoft에 보고할 수 있습니다. 메일로 리뷰 응답을 받지 않도록 선택할 수도 있습니다.
>
> Microsoft는 부적절한 응답 보고서가 너무 많이 발생하거나 리뷰 응답을 받지 않겠다고 선택한 고객이 너무 많은 경우를 포함하여 어떤 이유로든 개발자가 응답을 보낼 권한을 해지할 권리를 가지고 있습니다.

고객과의 관계는 여러분의 책임입니다. 개발자와 고객 간에 분쟁이 있을 경우 Microsoft는 관여하지 않습니다. 그러나 앱의 리뷰에 불쾌 한 비속어, 나 욕설이 있으면 여세요 [요청을 지원](http://go.microsoft.com/fwlink/p/?LinkID=401178)합니다.


## <a name="use-customer-reviews-to-improve-your-app"></a>고객 리뷰를 참고하여 앱 향상

고객의 의견에 귀를 기울이고 응답하는 것은 시작에 불과합니다. 의견을 반영하여 조치를 취하는 것이 중요합니다. 파격적으로 개선된 경우에는 앱을 업데이트하는 [새 제출을 만들어](app-submissions.md) 스토어에서 자신 있게 선보이세요.
