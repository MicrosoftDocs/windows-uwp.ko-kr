---
ms.assetid: 37F2C162-4910-4336-BEED-8536C88DCA65
description: 파트너 센터 계정에 등록 된 앱에 대 한 패키지 항공편을 관리 하려면 Microsoft Store 제출 API에서에서 이러한 메서드를 사용 합니다.
title: 패키지 플라이트 관리
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp, Microsoft Store 제출 API, 플라이트
ms.localizationpriority: medium
ms.openlocfilehash: 8678ee4d73f13e241a2c72d6dac532289af13ced
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57601248"
---
# <a name="manage-package-flights"></a>패키지 플라이트 관리

Microsoft Store 제출 API에서 다음 메서드를 사용하여 앱의 패키지 플라이트를 관리합니다. API 사용을 위한 필수 조건을 비롯하여 Microsoft Store 제출 API에 대한 자세한 내용은 [Microsoft Store 서비스를 사용하여 제출 만들기 및 관리](create-and-manage-submissions-using-windows-store-services.md)를 참조하세요.

이러한 메서드는 패키지 플라이트를 가져오거나, 만들거나, 삭제하는 데만 사용될 수 있습니다. 패키지 플라이트에 대한 제출을 만들려면 [패키지 플라이트 제출 관리](manage-flight-submissions.md)를 참조하세요.

<table>
<colgroup>
<col width="10%" />
<col width="30%" />
<col width="60%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">메서드</th>
<th align="left">URI</th>
<th align="left">설명</th>
</tr>
</thead>
<tbody>
<tr>
<td align="left">GET</td>
<td align="left">https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/flights/{flightId}</td>
<td align="left"><a href="get-a-flight.md">패키지 항공편 가져오기</a></td>
</tr>
<tr>
<td align="left">POST</td>
<td align="left">https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/flights</td>
<td align="left"><a href="create-a-flight.md">패키지 항공편 만들기</a></td>
</tr>
<tr>
<td align="left">DELETE</td>
<td align="left">https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/flights/{flightId}</td>
<td align="left"><a href="delete-a-flight.md">패키지 항공편 삭제</a></td>
</tr>
</tbody>
</table>

## <a name="prerequisites"></a>필수 구성 요소

아직 완료하지 않은 경우 이러한 메서드를 사용하기 전에 Microsoft Store 제출 API에 대한 [필수 조건](create-and-manage-submissions-using-windows-store-services.md#prerequisites)을 모두 완료합니다.

## <a name="related-topics"></a>관련 항목

* [Microsoft Store 서비스를 사용 하 여 서브 미션을 만들고 설정 합니다.](create-and-manage-submissions-using-windows-store-services.md)
* [비행 서브 미션 패키지를 관리 합니다.](manage-flight-submissions.md)
