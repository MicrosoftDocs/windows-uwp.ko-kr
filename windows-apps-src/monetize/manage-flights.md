---
author: mcleanbyron
ms.assetid: 37F2C162-4910-4336-BEED-8536C88DCA65
description: "Windows 스토어 제출 API에서 다음 메서드를 사용하여 Windows 개발자 센터 계정에 등록된 앱을 위한 패키지 플라이트를 관리합니다."
title: "Windows 스토어 제출 API를 사용하여 패키지 플라이트 관리"
ms.author: mcleans
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: "Windows 10,uwp, Windows 스토어 제출 API, 플라이트"
translationtype: Human Translation
ms.sourcegitcommit: c6b64cff1bbebc8ba69bc6e03d34b69f85e798fc
ms.openlocfilehash: 51d7481d0491c85bddcae906a846cb8773f33417
ms.lasthandoff: 02/07/2017

---

# <a name="manage-package-flights-using-the-windows-store-submission-api"></a>Windows 스토어 제출 API를 사용하여 패키지 플라이트 관리

Windows 스토어 제출 API에서 다음 메서드를 사용하여 앱의 패키지 플라이트를 관리합니다. API 사용을 위한 필수 조건을 비롯하여 Windows 스토어 제출 API에 대한 자세한 내용은 [Windows 스토어 서비스를 사용하여 제출 만들기 및 관리](create-and-manage-submissions-using-windows-store-services.md)를 참조하세요.

>**참고**&nbsp;&nbsp;이러한 메서드는 Windows 스토어 제출 API를 사용할 수 있는 권한을 지닌 Windows 개발자 센터 계정에 대해서만 사용할 수 있습니다. 이 권한은 개발자 계정에서 단계별로 사용할 수 있으며, 현재로서는 이 권한을 모든 계정에서 사용할 수 있는 것은 아닙니다. 이전 액세스를 요청하려면 개발자 센터 대시보드에 로그온하고 대시보드의 아래쪽에서 **피드백**을 클릭하여 피드백 영역의 **제출 API**를 선택한 다음 요청을 제출합니다. 계정에 대해 이 권한을 사용할 수 있는 경우 메일을 받게 됩니다.

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
<td align="left">```https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/flights/{flightId}```</td>
<td align="left">[패키지 플라이트 가져오기](get-a-flight.md)</td>
</tr>
<tr>
<td align="left">POST</td>
<td align="left">```https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/flights```</td>
<td align="left">[패키지 플라이트 만들기](create-a-flight.md)</td>
</tr>
<tr>
<td align="left">DELETE</td>
<td align="left">```https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/flights/{flightId}```</td>
<td align="left">[패키지 플라이트 삭제](delete-a-flight.md)</td>
</tr>
</tbody>
</table>

## <a name="prerequisites"></a>필수 조건

아직 완료하지 않은 경우 이러한 메서드를 사용하기 전에 Windows 스토어 제출 API를 위한 [필수 조건](create-and-manage-submissions-using-windows-store-services.md#prerequisites)을 모두 완료합니다.

## <a name="related-topics"></a>관련 항목

* [Windows 스토어 서비스를 사용하여 제출 만들기 및 관리](create-and-manage-submissions-using-windows-store-services.md)
* [패키지 플라이트 제출 관리](manage-flight-submissions.md)

