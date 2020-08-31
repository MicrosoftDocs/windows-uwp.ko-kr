---
ms.assetid: 4F9657E5-1AF8-45E0-9617-45AF64E144FC
description: Microsoft Store 제출 API에서 이러한 메서드를 사용 하 여 파트너 센터 계정에 등록 된 앱에 대 한 추가 기능을 관리 합니다.
title: 추가 기능 관리
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp, Microsoft Store 제출 API, 추가 기능, 앱 내 제품, IAP
ms.localizationpriority: medium
ms.openlocfilehash: 7f02e222cf495f56352a645ac3a366da39dc5e3a
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89158417"
---
# <a name="manage-add-ons"></a>추가 기능 관리

Microsoft Store 제출 API에서 다음 메서드를 사용 하 여 앱에 대 한 추가 기능을 관리 합니다. API를 사용 하기 위한 필수 구성 요소를 포함 하 여 Microsoft Store 제출 API에 대 한 소개는 [Microsoft Store services를 사용 하 여 전송 만들기 및 관리](create-and-manage-submissions-using-windows-store-services.md)를 참조 하세요.

이러한 메서드는 추가 기능을 가져오거나 만들거나 삭제 하는 데만 사용할 수 있습니다. 추가 기능에 대 한 제출을 만들려면 [추가 기능 제출 관리](manage-add-on-submissions.md)의 메서드를 참조 하세요.

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
<td align="left">https://manage.devcenter.microsoft.com/v1.0/my/inappproducts</td>
<td align="left"><a href="get-all-add-ons.md">앱에 대 한 모든 추가 기능 가져오기</a></td>
</tr>
<tr>
<td align="left">GET</td>
<td align="left">https://manage.devcenter.microsoft.com/v1.0/my/inappproducts/{inAppProductId}</td>
<td align="left"><a href="get-an-add-on.md">특정 추가 기능 가져오기</a></td>
</tr>
<tr>
<td align="left">POST</td>
<td align="left">https://manage.devcenter.microsoft.com/v1.0/my/inappproducts</td>
<td align="left"><a href="create-an-add-on.md">추가 기능 만들기</a></td>
</tr>
<tr>
<td align="left">Delete</td>
<td align="left">https://manage.devcenter.microsoft.com/v1.0/my/inappproducts/{inAppProductId}</td>
<td align="left"><a href="delete-an-add-on.md">추가 기능 삭제</a></td>
</tr>
</tbody>
</table>

## <a name="prerequisites"></a>필수 구성 요소

아직 수행 하지 않은 경우 이러한 메서드를 사용 하기 전에 Microsoft Store 제출 API에 대 한 모든 [필수 구성 요소](create-and-manage-submissions-using-windows-store-services.md#prerequisites) 를 완료 합니다.

## <a name="data-resources"></a>데이터 리소스

추가 기능을 관리 하기 위한 Microsoft Store 제출 API 메서드는 다음 JSON 데이터 리소스를 사용 합니다.

<span id="add-on-object" />

### <a name="add-on-resource"></a>추가 기능 리소스

이 리소스는 추가 기능을 설명 합니다.

```json
{
  "applications": {
    "value": [
      {
        "id": "9NBLGGH4R315",
        "resourceLocation": "applications/9NBLGGH4R315"
      }
    ],
    "totalCount": 1
  },
  "id": "9NBLGGH4TNMP",
  "productId": "TestAddOn",
  "productType": "Durable",
  "pendingInAppProductSubmission": {
    "id": "1152921504621243619",
    "resourceLocation": "inappproducts/9NBLGGH4TNMP/submissions/1152921504621243619"
  },
  "lastPublishedInAppProductSubmission": {
    "id": "1152921504621243705",
    "resourceLocation": "inappproducts/9NBLGGH4TNMP/submissions/1152921504621243705"
  }
}
```

이 리소스의 값은 다음과 같습니다.

| 값      | 형식   | Description        |
|------------|--------|--------------|
| 애플리케이션      | array  | 이 추가 기능이 연결 된 [응용 프로그램을 나타내는 하나의 응용 프로그램 리소스](#application-object) 를 포함 하는 배열입니다. 이 배열에서는 하나의 항목만 지원 됩니다.  |
| id | 문자열  | 추가 기능에 대 한 저장소 ID입니다. 이 값은 저장소에서 제공 됩니다. 예 매장 ID는 9NBLGGH4TNMP입니다.  |
| productId | 문자열  | 추가 기능에 대 한 제품 ID입니다. 이 ID는 추가 기능을 만들 때 개발자가 제공한 ID입니다. 자세한 내용은 [제품 유형 및 제품 ID 설정](../publish/set-your-add-on-product-id.md)을 참조 하세요. |
| productType | 문자열  | 추가 기능에 대 한 제품 유형입니다. 지 **속성** 및 사용할 수 있는 값은 다음과 **같습니다.**  |
| lastPublishedInAppProductSubmission       | 개체 | 추가 기능에 대해 마지막으로 게시 된 전송에 대 한 정보를 제공 하는 [제출 리소스](#submission-object) 입니다.         |
| Pendinginapp제품 제출        | 개체  |  추가 기능에 대 한 현재 보류 중인 전송에 대 한 정보를 제공 하는 [제출 리소스](#submission-object) 입니다.  |   |

<span id="application-object" />

### <a name="application-resource"></a>애플리케이션 리소스

이 리소스는 추가 기능이 연결 된 앱을 descries 합니다. 다음 예제에서는이 리소스의 형식을 보여 줍니다.

```json
{
  "applications": {
    "value": [
      {
        "id": "9NBLGGH4R315",
        "resourceLocation": "applications/9NBLGGH4R315"
      }
    ],
    "totalCount": 1
  },
}
```

이 리소스의 값은 다음과 같습니다.

| 값           | 형식    | Description        |
|-----------------|---------|-----------|
| 값            | object  |  다음 값을 포함 하는 개체입니다. <br/><br/> <ul><li>*id*. 앱의 저장소 id입니다. 저장소 ID에 대 한 자세한 내용은 [앱 id 세부 정보 보기](../publish/view-app-identity-details.md)를 참조 하세요.</li><li>*Resourcelocation*. ```https://manage.devcenter.microsoft.com/v1.0/my/```앱에 대 한 전체 데이터를 검색 하기 위해 기본 요청 URI에 추가할 수 있는 상대 경로입니다.</li></ul>   |
| totalCount   | int  | *응용 프로그램* 배열의 응답 본문에 있는 앱 개체의 수입니다.                                                                                                                                                 |

<span id="submission-object" />

### <a name="submission-resource"></a>제출 리소스

이 리소스는 추가 기능에 대 한 제출 정보를 제공 합니다. 다음 예제에서는이 리소스의 형식을 보여 줍니다.

```json
{
  "pendingInAppProductSubmission": {
    "id": "1152921504621243619",
    "resourceLocation": "inappproducts/9NBLGGH4TNMP/submissions/1152921504621243619"
  },
}
```

이 리소스의 값은 다음과 같습니다.

| 값           | 형식    | Description     |
|-----------------|---------|------------------|
| id            | 문자열  | 제출 ID입니다.    |
| resourceLocation   | 문자열  | ```https://manage.devcenter.microsoft.com/v1.0/my/```전송에 대 한 전체 데이터를 검색 하기 위해 기본 요청 URI에 추가할 수 있는 상대 경로입니다.     |
 
<span/>

## <a name="related-topics"></a>관련 항목

* [Microsoft Store 서비스를 사용 하 여 제출 작성 및 관리](create-and-manage-submissions-using-windows-store-services.md)
* [Microsoft Store 제출 API를 사용 하 여 추가 기능 제출 관리](manage-add-on-submissions.md)
* [모든 추가 기능 가져오기](get-all-add-ons.md)
* [추가 기능 가져오기](get-an-add-on.md)
* [추가 기능 만들기](create-an-add-on.md)
* [추가 기능 삭제](delete-an-add-on.md)