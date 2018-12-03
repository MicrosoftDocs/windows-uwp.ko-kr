---
title: /users/{requestorId}/permission/validate
assetID: 400a9721-bf43-76df-4cd1-9f2ae6ca5035
permalink: en-us/docs/xboxlive/rest/uri-privacyusersrequestoridpermissionvalidate.html
description: " /users/{requestorId}/permission/validate"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, 게임, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 4a062fd417bae37fd66c944e0e534ef7a50de5fa
ms.sourcegitcommit: d2517e522cacc5240f7dffd5bc1eaa278e3f7768
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 12/02/2018
ms.locfileid: "8325726"
---
# <a name="usersrequestoridpermissionvalidate"></a>/users/{requestorId}/permission/validate
 
  * [URI 매개 변수](#ID4EQ)
 
<a id="ID4EQ"></a>

 
## <a name="uri-parameters"></a>URI 매개 변수
 
| 매개 변수| 유형| 설명| 
| --- | --- | --- | 
| requestorId| string| 필수. 작업을 수행 하는 사용자의 식별자입니다. 가능한 값은 <code>xuid({xuid})</code> 및 <code>me</code>. 로그인 한 사용자 여야 합니다. 예제 값: <code>xuid(0987654321)</code>.| 
  
<a id="ID4ETB"></a>

 
## <a name="valid-methods"></a>유효한 메서드

[GET (/users/{requestorId}/permission/validate)](uri-privacyusersrequestoridpermissionvalidateget.md)

&nbsp;&nbsp;대상 사용자를 사용 하 여 지정 된 작업을 수행 하는 사용자가 허용 되는지 여부에 대 한 예 또는 아니요 응답을 가져옵니다.

[POST (/users/{requestorId}/permission/validate)](uri-privacyusersrequestoridpermissionvalidatepost.md)

&nbsp;&nbsp;사용자가 대상 사용자의 집합을 사용 하 여 지정 된 작업을 수행할 수 있는지 여부에 대 한 예 또는 아니요 답변의 집합을 가져옵니다.
 
<a id="ID4EAC"></a>

 
## <a name="see-also"></a>참고 항목
 
<a id="ID4ECC"></a>

   [개인 정보 URI](atoc-reference-privacyv2.md)

 [PermissionId 열거형](../../enums/privacy-enum-permissionid.md)

   