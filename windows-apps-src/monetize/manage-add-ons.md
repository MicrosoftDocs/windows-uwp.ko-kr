---
author: mcleanbyron
ms.assetid: 4F9657E5-1AF8-45E0-9617-45AF64E144FC
description: "Windows 스토어 제출 API에서 다음 메서드를 사용하여 Windows 개발자 센터 계정에 등록된 앱을 위한 추가 기능을 관리합니다."
title: "Windows 스토어 제출 API를 사용하여 추가 기능 관리"
ms.author: mcleans
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: "windows 10, uwp, Windows 스토어 제출 API, 추가 기능, 앱에서 바로 구매 제품, IAP"
translationtype: Human Translation
ms.sourcegitcommit: c6b64cff1bbebc8ba69bc6e03d34b69f85e798fc
ms.openlocfilehash: 55a6b548246e801c9fcc0392265263123f24de00
ms.lasthandoff: 02/07/2017

---

# <a name="manage-add-ons-using-the-windows-store-submission-api"></a>Windows 스토어 제출 API를 사용하여 추가 기능 관리

Windows 스토어 제출 API에서는 다음 메서드를 사용하여 앱의 추가 기능(앱에서 바로 구매 제품 또는 IAP라고도 함)을 관리합니다. API 사용을 위한 필수 조건을 비롯하여 Windows 스토어 제출 API에 대한 자세한 내용은 [Windows 스토어 서비스를 사용하여 제출 만들기 및 관리](create-and-manage-submissions-using-windows-store-services.md)를 참조하세요.

>**참고**&nbsp;&nbsp;이러한 메서드는 Windows 스토어 제출 API를 사용할 수 있는 권한을 지닌 Windows 개발자 센터 계정에 대해서만 사용할 수 있습니다. 이 권한은 개발자 계정에서 단계별로 사용할 수 있으며, 현재로서는 이 권한을 모든 계정에서 사용할 수 있는 것은 아닙니다. 이전 액세스를 요청하려면 개발자 센터 대시보드에 로그온하고 대시보드의 아래쪽에서 **피드백**을 클릭하여 피드백 영역의 **제출 API**를 선택한 다음 요청을 제출합니다. 계정에 대해 이 권한을 사용할 수 있는 경우 메일을 받게 됩니다.

이러한 메서드는 추가 기능을 가져오거나, 만들거나, 삭제하는 데만 사용될 수 있습니다. 추가 기능 제출을 만들려면 [추가 기능 제출 관리](manage-add-on-submissions.md)의 메서드를 참조하세요.

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
<td align="left">```https://manage.devcenter.microsoft.com/v1.0/my/inappproducts```</td>
<td align="left">[앱에 대한 모든 추가 기능 가져오기](get-all-add-ons.md)</td>
</tr>
<tr>
<td align="left">GET</td>
<td align="left">```https://manage.devcenter.microsoft.com/v1.0/my/inappproducts/{inAppProductId}```</td>
<td align="left">[특정 추가 기능 가져오기](get-an-add-on.md)</td>
</tr>
<tr>
<td align="left">POST</td>
<td align="left">```https://manage.devcenter.microsoft.com/v1.0/my/inappproducts```</td>
<td align="left">[추가 기능 만들기](create-an-add-on.md)</td>
</tr>
<tr>
<td align="left">DELETE</td>
<td align="left">```https://manage.devcenter.microsoft.com/v1.0/my/inappproducts/{inAppProductId}```</td>
<td align="left">[추가 기능 삭제](delete-an-add-on.md)</td>
</tr>
</tbody>
</table>

## <a name="prerequisites"></a>필수 조건

아직 완료하지 않은 경우 이러한 메서드를 사용하기 전에 Windows 스토어 제출 API를 위한 [필수 조건](create-and-manage-submissions-using-windows-store-services.md#prerequisites)을 모두 완료합니다.

## <a name="data-resources"></a>데이터 리소스

추가 기능을 관리하는 Windows 스토어 제출 API 메서드는 다음과 같은 JSON 데이터 리소스를 사용합니다.

<span id="add-on-object" />
### <a name="add-on-resource"></a>추가 기능 리소스

이 리소스는 추가 기능을 설명합니다.

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

| 값      | 유형   | 설명        |
|------------|--------|--------------|
| 응용 프로그램      | 배열  | 이 추가 기능이 연결된 앱을 나타내는 하나의 [응용 프로그램 리소스](#application-object)가 포함된 배열입니다. 이 배열에서는 한 개의 항목만 지원됩니다.  |
| id | 문자열  | 추가 기능의 스토어 ID입니다. 이 값은 스토어에서 제공됩니다. 예를 들어 스토어 ID는 9NBLGGH4TNMP입니다.  |
| productId | 문자열  | 추가 기능의 제품 ID입니다. 이 ID는 추가 기능을 만들 때 개발자가 제공한 ID입니다. 자세한 내용은 [제품 유형 및 제품 ID 설정](https://msdn.microsoft.com/windows/uwp/publish/set-your-iap-product-id)을 참조하세요. |
| productType | 문자열  | 추가 기능의 제품 유형입니다. **Durable** 및 **Consumable** 값이 지원됩니다.  |
| lastPublishedInAppProductSubmission       | object | 추가 기능의 마지막 게시된 제출에 대한 정보를 제공하는 [제출 리소스](#submission-object)입니다.         |
| pendingInAppProductSubmission        | object  |  추가 기능의 현재 보류 중인 제출에 대한 정보를 제공하는 [제출 리소스](#submission-object)입니다.  |   |

<span id="application-object" />
### <a name="application-resource"></a>응용 프로그램 리소스

이 리소스는 추가 기능이 연결된 앱을 설명합니다. 다음 예제에서는 이 리소스의 형식을 보여 줍니다.

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

| 값           | 유형    | 설명        |
|-----------------|---------|-----------|
| value            | object  |  다음 값을 포함하는 개체입니다. <br/><br/> <ul><li>*id*. 앱의 스토어 ID입니다. 스토어 ID에 대한 자세한 내용은 [앱 ID 세부 정보 보기](https://msdn.microsoft.com/windows/uwp/publish/view-app-identity-details)를 참조하세요.</li><li>*resourceLocation*. 기본 ```https://manage.devcenter.microsoft.com/v1.0/my/``` 요청 URI에 추가하여 앱에 대한 전체 데이터를 검색할 수 있는 상대 경로입니다.</li></ul>   |
| totalCount   | int  | 응답 본문의 *applications* 배열에서 앱 개체의 수입니다.                                                                                                                                                 |

<span id="submission-object" />
### <a name="submission-resource"></a>제출 리소스

이 리소스는 추가 기능의 제출에 대한 정보를 제공합니다. 다음 예제에서는 이 리소스의 형식을 보여 줍니다.

```json
{
  "pendingInAppProductSubmission": {
    "id": "1152921504621243619",
    "resourceLocation": "inappproducts/9NBLGGH4TNMP/submissions/1152921504621243619"
  },
}
```

이 리소스의 값은 다음과 같습니다.

| 값           | 유형    | 설명     |
|-----------------|---------|------------------|
| id            | 문자열  | 제출의 ID입니다.    |
| resourceLocation   | 문자열  | 기본 ```https://manage.devcenter.microsoft.com/v1.0/my/``` 요청 URI에 추가하여 제출에 대한 전체 데이터를 검색할 수 있는 상대 경로입니다.     |
 
<span/>
## <a name="related-topics"></a>관련 항목

* [Windows 스토어 서비스를 사용하여 제출 만들기 및 관리](create-and-manage-submissions-using-windows-store-services.md)
* [Windows 스토어 제출 API를 사용하여 추가 기능 제출 관리](manage-add-on-submissions.md)
* [모든 추가 기능 가져오기](get-all-add-ons.md)
* [추가 기능 가져오기](get-an-add-on.md)
* [추가 기능 만들기](create-an-add-on.md)
* [추가 기능 삭제](delete-an-add-on.md)

