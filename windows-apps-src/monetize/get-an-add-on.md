---
author: mcleanbyron
ms.assetid: 78278741-09A4-4406-A112-9AF3C73F5C16
description: "Windows 스토어 제출 API에서 이 메서드를 사용하여 Windows 개발자 센터 계정에 등록된 앱의 추가 기능에 대한 정보를 검색합니다."
title: "Windows 스토어 제출 API를 사용하여 추가 기능 가져오기"
ms.author: mcleans
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: "windows 10, uwp, Windows 스토어 제출 API, 추가 기능, 앱에서 바로 구매 제품, IAP"
translationtype: Human Translation
ms.sourcegitcommit: c6b64cff1bbebc8ba69bc6e03d34b69f85e798fc
ms.openlocfilehash: e000c435b8a271085872bfb1ce42dd09352847c5
ms.lasthandoff: 02/07/2017

---

# <a name="get-an-add-on-using-the-windows-store-submission-api"></a>Windows 스토어 제출 API를 사용하여 추가 기능 가져오기




Windows 스토어 제출 API에서 이 메서드를 사용하여 Windows 개발자 센터 계정에 등록된 앱의 추가 기능(앱에서 바로 구매 제품 또는 IAP라고도 함)에 대한 정보를 검색합니다.

## <a name="prerequisites"></a>필수 조건

이 메서드를 사용하려면 다음을 먼저 수행해야 합니다.

* 아직 완료하지 않은 경우 Windows 스토어 제출 API에 대한 모든 [필수 조건](create-and-manage-submissions-using-windows-store-services.md#prerequisites)을 완료합니다.
* 이 메서드에 대한 요청 헤더에 사용할 [Azure AD 액세스 토큰을 가져옵니다](create-and-manage-submissions-using-windows-store-services.md#obtain-an-azure-ad-access-token). 액세스 토큰을 얻은 후 만료되기 전에 60분 동안 사용할 수 있습니다. 토큰이 만료된 후 새 토큰을 가져올 수 있습니다.

>**참고**&nbsp;&nbsp;이 메서드는 Windows 스토어 제출 API를 사용할 수 있는 권한이 부여된 Windows 개발자 센터 계정에만 사용할 수 있습니다. 일부 계정은 이 권한을 사용할 수 없습니다.

## <a name="request"></a>요청

이 메서드에는 다음 구문이 있습니다. 헤더 및 요청 본문의 사용 예제와 설명은 다음 섹션을 참조하세요.

| 메서드 | 요청 URI                                                      |
|--------|------------------------------------------------------------------|
| GET    | ```https://manage.devcenter.microsoft.com/v1.0/my/inappproducts/{inAppProductId}``` |

<span/>
 

### <a name="request-header"></a>요청 헤더

| 헤더        | 유형   | 설명                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| 권한 부여 | 문자열 | 필수. **Bearer** &lt;*token*&gt; 형식의 Azure AD 액세스 토큰입니다. |

<span/>

### <a name="request-parameters"></a>요청 매개 변수

| 이름        | 유형   | 설명                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| id | 문자열 | 필수. 검색할 추가 기능의 스토어 ID입니다. 스토어 ID는 개발자 센터 대시보드에서 사용할 수 있습니다.  |

<span/>

### <a name="request-body"></a>요청 본문

이 메서드에 대한 요청 본문을 제공하지 않습니다.

<span/>

### <a name="request-example"></a>요청 예제

다음 예제에서는 추가 기능에 대한 정보를 검색하는 방법을 보여 줍니다.

```
GET https://manage.devcenter.microsoft.com/v1.0/my/inappproducts/9NBLGGH4TNMP HTTP/1.1
Authorization: Bearer <your access token>
```

## <a name="response"></a>응답

다음 예제에서는 이 메서드를 성공적으로 호출하기 위한 JSON 응답 본문을 보여 줍니다. 응답 본문의 값에 대한 자세한 내용은 [추가 기능 리소스](manage-add-ons.md#add-on-object)를 참조하세요.

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
  "productId": "Test-add-on",
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

## <a name="error-codes"></a>오류 코드

요청을 성공적으로 완료할 수 없으면 응답에 다음 HTTP 오류 코드 중 하나가 포함됩니다.

| 오류 코드 |  설명   |
|--------|------------------|
| 404  | 지정된 추가 기능을 찾을 수 없습니다. |
| 409  | 추가 기능이 [현재 Windows 스토어 제출 API에서 지원되지 않는](create-and-manage-submissions-using-windows-store-services.md#not_supported) 개발자 센터 대시보드 기능을 사용합니다.  |

<span/>

## <a name="related-topics"></a>관련 항목

* [Windows 스토어 서비스를 사용하여 제출 만들기 및 관리](create-and-manage-submissions-using-windows-store-services.md)
* [추가 기능 제출 관리](manage-add-on-submissions.md)
* [모든 추가 기능 가져오기](get-all-add-ons.md)
* [추가 기능 만들기](create-an-add-on.md)
* [추가 기능 삭제](delete-an-add-on.md)

