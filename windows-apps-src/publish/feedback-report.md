---
author: jnHs
Description: The Feedback report in the Windows Dev Center dashboard lets you see the problems, suggestions, and upvotes that your Windows 10 customers have submitted through Feedback Hub.
title: 피드백 보고서
ms.assetid: 9EA8B456-CA57-40CE-A55B-7BFDC55CA8A8
ms.author: wdg-dev-content
ms.date: 11/3/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: bceb1d2cc6682698d0ad06ed4b1865f3d6510442
ms.sourcegitcommit: 5c9a47b135c5f587214675e39c1ac058c0380f4c
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/04/2018
ms.locfileid: "4352182"
---
# <a name="feedback-report"></a>피드백 보고서

Windows 개발자 센터 대시보드의 **피드백 보고서**에서 Windows 10 고객이 피드백 허브를 통해 제출한 문제, 제안, 찬성을 볼 수 있습니다. 대시보드에서 이 데이터를 보거나 데이터를 내보내서 오프라인으로 볼 수 있습니다.

> [!NOTE]
> 이 보고서에서 직접 [피드백에 응답](respond-to-customer-feedback.md)하여 고객의 의견을 경청하고 있음을 고객에게 알릴 수도 있습니다.

고객에게 앱에 대한 피드백을 제공하도록 권유하는 것은 고객이 가장 중요한 문제와 기능을 알게 하는 좋은 방법입니다. 고객이 직접 피드백을 보낼 수 있다는 사실을 알게 되면 스토어에서 해당 피드백을 부정적인 리뷰로 남길 가능성은 적습니다.

[Microsoft Store Services SDK](http://aka.ms/store-em-sdk)의 피드백 API를 사용하여 고객이 [직접 앱에서 피드백 허브를 시작](../monetize/launch-feedback-hub-from-your-app.md)하도록 할 수 있습니다. 피드백 허브를 지원하는 Windows 10 장치에 앱을 다운로드한 모든 고객은 피드백 허브 앱을 사용하여 해당 피드백을 남길 수 있습니다. 이 인해 하면 특별히 요청 하지 않은의 피드백에 대 한 앱 내에서 하는 경우에이 보고서에서 고객 의견을 볼 수 있습니다.

피드백 보고서는 피드백을 남겼을 때 각 고객이 장치에 설치한 특정 패키지를 보여준다는 점에서 [패키지 플라이팅](package-flights.md)을 사용할 때 피드백이 도움이 될 수 있습니다.

> [!TIP]
> 지난 30 일 동안에서 리뷰, 평점 및 앱의 모든 사용자 피드백에 빠르게 확인, 왼쪽된 탐색 메뉴에서 **참여** 를 확장 하 고 선택 **리뷰 및 피드백.** 


## <a name="apply-filters"></a>필터 적용

페이지 위쪽에서 데이터가 표시되는 기간을 선택할 수 있습니다. 기본 설정은 **수명**이지만, 30일, 3개월, 6개월, 12개월 등 데이터 표시 기간을 선택할 수 있습니다.

**필터**를 확장하여 다음과 같은 옵션으로 이 페이지의 모든 데이터를 필터링할 수 있습니다.

- **피드백 유형**: 기본 설정은 **모두**입니다. 해당 유형의 피드백만 표시하려면 **문제** 또는 **제안**을 선택할 수 있습니다.
- **장치 유형**: 기본 설정은 **모든 장치**입니다. 이 페이지에 특정 유형의 장치를 사용하는 고객이 남긴 피드백만 표시하려면 특정 장치 유형을 선택합니다.
- **패키지 버전**: 기본 설정은 **모든 패키지**입니다. 피드백을 남길 때 특정 패키지를 사용하는 고객이 남긴 피드백만 표시하려면 패키지 중 하나를 선택할 수 있습니다.
- **지역/국가**: 기본 설정은 **모든 시장**입니다. 해당 지역/국가 고객의 피드백만 표시하려면 특정 지역/국가를 선택할 수 있습니다.
- **그룹**: 기본 설정은 **모두**입니다. [Windows 참가자](http://insider.windows.com)가 제출한 피드백만 보도록 선택할 수 있습니다.

> [!TIP]
> 페이지에 피드백이 표시되지 않으면 필터로 모든 피드백을 제외하지 않았는지 확인해야 합니다. 예를 들어 앱이 지원하지 않는 **장치 유형**을 기준으로 필터링하면 피드백이 표시되지 않습니다.


## <a name="viewing-feedback-details"></a>피드백 세부 정보 보기

이 보고서에서 고객이 남긴 개별 피드백을 찾을 수 있습니다. 각 항목에 대한 피드백 텍스트 왼쪽에서 피드백 허브의 다른 고객이 해당 피드백을 찬성한 횟수를 볼 수 있습니다. 세 가지 방법으로 피드백을 정렬할 수 있습니다.

- **찬성**(기본값): 가장 많은 찬성을 받은 피드백으로 시작하여 다른 고객이 찬성한 피드백을 표시합니다.
- **인기 앱**: 최근 활동이 있는 피드백으로 시작하여 최근 7일 이내에 다른 고객이 찬성한 피드백을 표시합니다.
- **최근**: 최근에 남겨진 피드백으로 시작하여 모든 피드백을 표시합니다.

각 메모 옆에 피드백이 남겨진 날짜와 피드백 유형이 표시됩니다. 고객의 지역/국가 남길 때 피드백, 디바이스 및 **Windows 참가자** 해당 유형의 피드백을 제출 하는 고객이 Windows 참가자의 구성원 인 경우 사용 하는 장치에 설치 된 특정 패키지도 표시 됩니다. 프로그램입니다.

[피드백에 응답](respond-to-customer-feedback.md)하기 위한 옵션도 여기에 표시됩니다.


## <a name="translating-feedback"></a>피드백 번역

기본적으로 원하는 언어로 작성 되지 않은 피드백은 번역. 필요하면 페이지 필터 근처 오른쪽 위에서 **번역 피드백** 확인란을 선택 취소하여 피드백 번역을 사용하지 않을 수 있습니다.

피드백은 자동 번역 시스템에 의해 번역되므로 번역 결과가 항상 정확하지는 않을 수 있습니다. 원본 텍스트를 번역과 비교하거나 다른 의미로 번역하고 싶은 경우를 대비해서 원본 텍스트가 제공됩니다.


## <a name="launching-feedback-hub-directly-from-your-app"></a>앱에서 직접 피드백 허브 시작

위에서 설명한 대로 고객에게 피드백을 제공하도록 권유하기 위해 앱에서 직접 피드백 허브에 대한 링크를 통합하는 것이 좋습니다. 자세한 내용은 [앱에서 피드백 허브 시작](../monetize/launch-feedback-hub-from-your-app.md)을 참조하세요.
