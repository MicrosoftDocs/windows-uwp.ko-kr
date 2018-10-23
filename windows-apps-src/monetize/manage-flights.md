---
author: Xansky
ms.assetid: 37F2C162-4910-4336-BEED-8536C88DCA65
description: Microsoft Store 제출 API에서 다음 메서드를 사용하여 Windows 개발자 센터 계정에 등록된 앱을 위한 패키지 플라이트를 관리합니다.
title: 패키지 플라이트 관리
ms.author: mhopkins
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp, Microsoft Store 제출 API, 플라이트
ms.localizationpriority: medium
ms.openlocfilehash: 6a761edf50888fb7f3130886a2c7e6e65b7c0a1d
ms.sourcegitcommit: c4d3115348c8b54fcc92aae8e18fdabc3deb301d
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/22/2018
ms.locfileid: "5406994"
---
# <a name="manage-package-flights"></a>패키지 플라이트 관리

Microsoft Store 제출 API에서 다음 메서드를 사용하여 앱의 패키지 플라이트를 관리합니다. API 사용을 위한 필수 조건을 비롯하여 Microsoft Store 제출 API에 대한 자세한 내용은 [Microsoft Store 서비스를 사용하여 제출 만들기 및 관리](create-and-manage-submissions-using-windows-store-services.md)를 참조하세요.

이러한 메서드는 패키지 플라이트를 가져오거나 만들거나 삭제하는 데만 사용할 수 있습니다. 패키지 플라이트에 대한 제출을 만들려면 [패키지 플라이트 제출 관리](manage-flight-submissions.md)를 참조하세요.

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
<td align="left"><a href="get-a-flight.md">패키지 플라이트 가져오기</a></td>
</tr>
<tr>
<td align="left">POST</td>
<td align="left">https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/flights</td>
<td align="left"><a href="create-a-flight.md">패키지 플라이트 만들기</a></td>
</tr>
<tr>
<td align="left">DELETE</td>
<td align="left">https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/flights/{flightId}</td>
<td align="left"><a href="delete-a-flight.md">패키지 플라이트 삭제</a></td>
</tr>
</tbody>
</table>

## <a name="prerequisites"></a>필수 조건

아직 완료하지 않은 경우 이러한 메서드를 사용하기 전에 Microsoft Store 제출 API에 대한 [필수 조건](create-and-manage-submissions-using-windows-store-services.md#prerequisites)을 모두 완료합니다.

## <a name="related-topics"></a>관련 항목

* [Microsoft Store 서비스를 사용하여 제출 만들기 및 관리](create-and-manage-submissions-using-windows-store-services.md)
* [패키지 플라이트 제출 관리](manage-flight-submissions.md)
