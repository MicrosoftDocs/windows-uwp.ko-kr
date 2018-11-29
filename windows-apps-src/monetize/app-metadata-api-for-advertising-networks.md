---
description: 앱 메타데이터 REST API를 사용하여 앱에 대한 특정 형식 메타데이터에 액세스하는 방법을 알아봅니다. 이 API는 광고주에게 광고 공간을 판매하는 방법을 개선할 수 있도록 Microsoft Store에서 앱에 대한 정보를 검색하는 광고 네트워크에서 사용됩니다.
title: 광고 네트워크용 앱 메타데이터 API
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp, 광고 네트워크, 앱 메타데이터
ms.assetid: f0904086-d61f-4adb-82b6-25968cbec7f3
ms.localizationpriority: medium
ms.openlocfilehash: 2fd0381d9ec8917f381cfeb045d58bfa3436de74
ms.sourcegitcommit: 89ff8ff88ef58f4fe6d3b1368fe94f62e59118ad
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/29/2018
ms.locfileid: "8194170"
---
# <a name="app-metadata-api-for-advertising-networks"></a>광고 네트워크용 앱 메타데이터 API

광고 네트워크는 *앱 메타데이터 API*를 사용하여 Microsoft Store에서 앱에 대한 메타데이터를 프로그래밍 방식으로 검색합니다. 이 메타데이터에는 앱의 Microsoft Store 목록에 대한 설명 및 범주와 같은 세부 정보와 앱이 13세 미만의 어린이를 대상으로 하는지 여부에 대한 정보가 포함됩니다. 이 API는 현재 Microsoft에서 API에 대한 권한을 부여받은 개발자로만 액세스가 제한됩니다.

이 문서는 [앱 메타데이터 API 포털](https://admetadata.portal.azure-api.net/)을 사용하여 API에 대한 액세스를 요청하는 방법, API 액세스를 위해 구독 키를 다운로드하는 방법 및 API 호출 방법에 대한 지침을 제공합니다.

## <a name="request-access"></a>액세스 요청

광고 네트워크에서는 다음 지침에 따라 앱 메타데이터 API에 대한 액세스를 요청할 수 있습니다.

1. 앱 메타데이터 API 포털의 [https://admetadata.portal.azure-api.net/signup](https://admetadata.portal.azure-api.net/signup) 페이지로 이동합니다.
2. 필요한 정보를 입력하고 **Sign up**(등록) 단추를 클릭합니다.
3. 동일한 사이트에서 **Products**(제품) 탭을 클릭한 다음 **App details for advertising**(광고를 위한 앱 세부 정보)을 클릭합니다.
4. 다음 페이지에서 **Subscribe**(구독) 단추를 클릭합니다. 이렇게 하면 요청이 제출되어 Microsoft로 앱 메타데이터 API에 액세스할 수 있습니다.

요청을 제출하고 나면 24시간 이내에 요청이 허용되었는지 거부되었는지 알려 주는 메일을 받게 됩니다.

<span id="get-key" />

## <a name="get-your-subscription-key"></a>구독 키 가져오기

앱 메타데이터 API에 대한 액세스 권한이 부여된 경우 다음 지침에 따라 구독 키 얻습니다. 이 키는 API 호출의 요청 헤더에 전달해야 합니다.

1. 앱 메타데이터 API 포털의 [https://admetadata.portal.azure-api.net/signin](https://admetadata.portal.azure-api.net/signin) 페이지로 이동하고 메일과 암호로 로그인합니다.
2. 사이트의 오른쪽 위에 있는 이름을 클릭한 다음 **Profile**(프로필)을 클릭합니다.
3. 페이지의 **Your subscriptions**(구독) 섹션에서 **Primary key**(기본 키) 옆에 있는 **Show**(표시)를 클릭합니다. 그러면 사용자 구독 키가 표시됩니다. API를 호출하는 경우 나중에 사용할 수 있도록 키를 복사합니다.

<span id="call-the-api" />

## <a name="call-the-api"></a>API 호출

구독 키를 받고 나면 선택한 프로그래밍 언어로 HTTP REST 구문을 사용하여 API를 호출할 수 있습니다. API의 구문에 대한 자세한 내용은 아래의 [API 구문](#syntax) 섹션을 참조하세요. C#, JavaScript, Python 및 다른 여러 언어의 코드 예제를 보려면 메타데이터 API 포털의 **API** 탭을 클릭하고, **App details**(앱 세부 정보)를 클릭한 다음 페이지 맨 아래에 있는 **Code samples**(코드 샘플) 섹션을 참조하세요.

또는 앱 메타데이터 API 포털에서 제공하는 UI를 사용하여 API를 호출할 수도 있습니다.
  1. 포털에서 **API** 탭을 클릭한 다음 **App details**(앱 세부 정보)를 클릭합니다.
  2. 다음 페이지에서 **app_id** 필드에서 메타데이터를 검색할 앱의 [app_id](#request-parameters)를 입력하고 **Ocp_Apim_Subscription 키** 필드에 구독 키를 입력합니다.
  3. **Send**(보내기)를 클릭합니다. 페이지의 맨 아래에 응답이 나타납니다.


<span id="syntax" />

## <a name="api-syntax"></a>API 구문

이 메서드의 요청 구문은 다음과 같습니다.

| 메서드 | 요청 URI                                                      |
|--------|------------------------------------------------------------------|
| GET   | ```https://admetadata.azure-api.net/v1/app/{app_id}``` |

<span/>
 

### <a name="request-header"></a>요청 헤더

| 헤더        | 유형   | 설명                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| Ocp-Apim-Subscription-Key | 문자열 | 필수. [앱 메타데이터 API 포털에서 검색한](#get-key) 구독 키입니다.  |

<span/>

### <a name="request-parameters"></a>요청 매개 변수

| 이름        | 유형   | 설명                                                                 |
|---------------|--------|-----------------------|
| app_id | 문자열 | 필수. 메타데이터를 검색할 앱의 ID입니다. 다음 값 중 하나일 수 있습니다.<br/><br/><ul><li>앱의 스토어 ID입니다. 예를 들어 스토어 ID는 9NBLGGH29DM8입니다.</li><li>Windows8.x 또는 Windows Phone 8.x용으로 작성한 앱의 제품 ID(*앱 ID*라고도 함)입니다. 제품 ID는 GUID입니다.</li></ul> |

<span/>

### <a name="request-example"></a>요청 예제

다음 예제에서는 스토어 ID가 9NBLGGH29DM8인 앱의 메타데이터를 검색하는 방법을 보여 줍니다.

```syntax
GET https://admetadata.azure-api.net/v1/app/9NBLGGH29DM8 HTTP/1.1
Ocp-Apim-Subscription-Key: <subscription key>
```

### <a name="response-body"></a>응답 본문

다음 예제에서는 이 메서드를 성공적으로 호출하기 위한 JSON 응답 본문을 보여 줍니다.

```json
{
    "storeId":"9NBLGGH29DM8",
    "name":"Contoso Sports App",
    "description":"Catch the latest scores and replays using Contoso Sports App!",
    "phoneStoreGuid":"920217d7-90da-4019-99e8-46e4a6bd2594",
    "windowsStoreGuid":"d7e982e7-fbf3-42b5-9dad-72b65bd4e248",
    "storeCategory":"Sports",
    "iabCategory":"Sports",
    "iabCategoryId":"IAB17",
    "coppa":false,
    "downloadUrl":"https://www.microsoft.com/store/apps/9nblggh29dm8",
    "isLive":true,
    "iconUrls":[
      "//store-images.microsoft.com/image/apps.15753.13510798883747357.b6981489-6fdd-4fec-b655-a822247d5a76.33529096-a723-4204-9da9-a5dd8b328d4e"
    ],
    "type":"App",
    "devices":[
      "PC",
      "Phone"
    ],
    "platformVersions":[
      "Windows.Universal"
    ],
    "screenshotUrls":[
      "//store-images.microsoft.com/image/apps.15901.19810723133740207.c9781069-6fef-5fba-a055-c922051d59b6.40129896-d083-5604-ab79-aba68bfa084f"
    ]
}
```

응답 본문의 값에 대한 자세한 내용은 다음 표를 참조하세요.

| 값      | 유형   | 설명    |
|------------|--------|--------------------|
| storeId           | 문자열  | 앱의 스토어 ID입니다. 예를 들어 스토어 ID는 9NBLGGH29DM8입니다.     |  
| name           | 문자열  | 앱의 이름입니다.   |
| 설명           | 문자열  | 앱의 스토어 목록에 대한 설명입니다.  |
| phoneStoreGuid           | 문자열  | 앱의 제품 ID(Windows Phone 8.x)입니다. 이 ID는 GUID입니다.  |
| windowsStoreGuid           | 문자열  | 앱의 제품 ID(Windows8.x)입니다. 이 ID는 GUID입니다. |
| storeCategory           | 문자열  | 스토어에서 앱 범주입니다. 지원되는 값은 스토어에서 앱의 [범주 및 하위 범주 테이블](../publish/category-and-subcategory-table.md)을 참조하세요.  |
| iabCategory           | 문자열  | IAB(Interactive Advertising Bureau)에서 정의한 앱의 콘텐츠 범주입니다. 예를 들면 **뉴스** 또는 **스포츠**가 됩니다. 콘텐츠 범주 목록은 IAB 웹 사이트에서[IAB Tech Lab Content Taxonom](https://www.iab.com/guidelines/iab-quality-assurance-guidelines-qag-taxonomy)(IAB 기술 랩 콘텐츠 분류) 페이지를 참조하세요.   |
| iabCategoryId           | 문자열  | 앱에 대한 콘텐츠 범주의 ID입니다. 예를 들어 **IAB12**는 뉴스 범주에 대한 ID, **IAB17**은 스포츠 범주에 대한 ID입니다. 콘텐츠 범주 ID 목록은 [OpenRTB API Specification](http://www.iab.com/wp-content/uploads/2015/05/OpenRTB_API_Specification_Version_2_3_1.pdf)(OpenRTB API 사양)의 섹션 5.1을 참조하세요. |
| coppa           | 부울  | 앱이 13세 미만 아동을 대상으로 하여 COPPA(아동에 관한 온라인 개인 정보 보호법)에 따른 의무 조항이 있으면 True, 그렇지 않으면 False입니다.  |
| downloadUrl           | 문자열  | 스토어에서 앱의 목록에 대한 링크입니다. 이 링크는 ```https://www.microsoft.com/store/apps/<Store ID>``` 형식입니다.  |
| isLive           | 부울  | 앱이 현재 스토어에 제공되면 True이고, 그렇지 않으면 false입니다.  |
| iconUrls           | 배열  |  앱과 연결된 아이콘 URL의 상대 경로를 포함하는 하나 이상의 문자열 배열입니다. 아이콘을 검색하려면 URL 앞에 *http* 또는 *https*를 추가합니다.  |
| 유형           | 문자열  | **앱** 또는 **게임** 문자열 중 하나입니다.  |
| devices           |  배열  | 앱에서 지원하는 디바이스 유형을 지정하는 **PC**, **휴대폰**, **Xbox**, **IoT**, **서버**, **홀로그램** 문자열 중 하나 이상으로 구성되는 배열입니다  |
| platformVersions           | 배열  |  앱에서 지원하는 플랫폼을 지정하는 **Windows.Universal**, **Windows.Windows8x**, **Windows.WindowsPhone8x** 문자열 중 하나 이상으로 구성되는 배열입니다.  |
| screenshotUrls           | 배열  | 이 앱의 스크린샷 URL에 대한 상대 경로를 포함하는 하나 이상의 문자열의 배열입니다. 스크린샷을 검색하려면 URL 앞에 *http* 또는 *https*를 추가합니다.  |

<span/>



 

 
