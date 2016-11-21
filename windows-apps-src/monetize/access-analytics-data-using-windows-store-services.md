---
author: mcleanbyron
ms.assetid: 4BF9EF21-E9F0-49DB-81E4-062D6E68C8B1
description: "Windows 스토어 분석 API를 사용하여 프로그래밍 방식으로 사용자 또는 사용자 조직의 Windows 개발자 센터 계정에 등록된 앱에 대한 분석 데이터를 검색합니다."
title: "Windows 스토어 서비스를 사용하여 분석 데이터에 액세스"
translationtype: Human Translation
ms.sourcegitcommit: 67845c76448ed13fd458cb3ee9eb2b75430faade
ms.openlocfilehash: 468be96b70d07567163b2caccebaa8e2f6ecd592

---

# Windows 스토어 서비스를 사용하여 분석 데이터에 액세스

*Windows 스토어 분석 API*를 사용하여 프로그래밍 방식으로 조직의 Windows 개발자 센터 계정에 등록된 앱에 대한 분석 데이터를 검색합니다. 이 API를 사용하면 앱 및 추가 기능(앱에서 바로 구매 제품 또는 IAP라고도 함) 구입, 오류, 앱 평점 및 리뷰에 대한 데이터를 검색할 수 있습니다. 이 API는 Azure AD(Azure Active Directory)를 사용하여 앱 또는 서비스의 호출을 인증합니다.

다음 단계에서는 종단 간 프로세스를 설명합니다.

1.  [필수 조건](#prerequisites)을 모두 완료했는지 확인합니다.
2.  Windows 스토어 분석 API에서 메서드를 호출하기 전에 [Azure AD 액세스 토큰을 가져옵니다](#obtain-an-azure-ad-access-token). 토큰을 가져온 후 만료되기 전에 이 토큰을 Windows 스토어 분석 API에 대한 호출에 사용할 수 있는 시간은 60분입니다. 토큰이 만료된 후 새 토큰을 생성할 수 있습니다.
3.  [Windows 스토어 분석 API를 호출](#call-the-windows-store-analytics-api)합니다.

<span id="prerequisites" />
## 단계 1: Windows 스토어 분석 API를 사용하기 위한 필수 조건 완료

Windows 스토어 분석 API를 호출하는 코드를 작성하기 전에 다음과 같은 필수 조건을 완료했는지 확인합니다.

* 사용자(또는 조직)에게 Azure AD 디렉터리와 해당 디렉터리에 대한 [전역 관리자](http://go.microsoft.com/fwlink/?LinkId=746654) 권한이 있어야 합니다. 이미 Office 365 또는 Microsoft의 다른 비즈니스 서비스를 사용하는 경우 이미 Azure AD 디렉터리가 있습니다. 아니면 추가 요금 없이 [개발자 센터 내에서 Azure AD를 새로 만들 수 있습니다](https://msdn.microsoft.com/windows/uwp/publish/manage-account-users).

* Azure AD 응용 프로그램을 개발자 센터 계정과 연결하고 응용 프로그램에 대한 테넌트 ID 및 클라이언트 ID를 검색하고 키를 생성해야 합니다. Azure AD 응용 프로그램은 Windows 스토어 분석 API를 호출할 앱 또는 서비스입니다. API에 전달하는 Azure AD 액세스 토큰을 가져오려면 테넌트 ID, 클라이언트 ID 및 키가 필요합니다.

  >**참고**&nbsp;&nbsp;이 작업은 한 번만 수행하면 됩니다. 테넌트 ID, 클라이언트 ID 및 키는 Azure AD 액세스 토큰을 새로 만들 때마다 다시 사용할 수 있습니다.

Azure AD 응용 프로그램을 개발자 센터 계정에 연결하고 필요한 값을 검색하려면

1.  개발자 센터에서 **계정 설정**으로 이동하여 **사용자 관리**를 클릭하고 조직의 개발자 센터 계정을 조직의 Azure AD 디렉터리와 연결합니다. 자세한 내용은 [계정 사용자 관리](https://msdn.microsoft.com/windows/uwp/publish/manage-account-users)를 참조하세요.

2.  **사용자 관리** 페이지에서 **Azure AD 응용 프로그램 추가**를 클릭하여 개발자 센터 계정에 대한 분석 데이터에 액세스하는 데 사용할 앱 또는 서비스를 나타내는 Azure AD 응용 프로그램을 추가하고 **관리자** 역할에 할당합니다. 이 응용 프로그램이 Azure AD 디렉터리에 이미 존재하는 경우 **Azure AD 응용 프로그램 추가** 페이지에서 선택하여 개발자 센터 계정에 추가할 수 있습니다. 그러지 않으면 **Azure AD 응용 프로그램 추가 페이지**에서 새 Azure AD 응용 프로그램을 만들 수 있습니다. 자세한 내용은 [Azure AD 응용 프로그램 추가 및 관리](https://msdn.microsoft.com/windows/uwp/publish/manage-account-users#add-and-manage-azure-ad-applications)를 참조하세요.

3.  **사용자 관리** 페이지로 돌아가서 Azure AD 응용 프로그램의 이름을 클릭하여 응용 프로그램 설정으로 이동하고 **테넌트 ID** 및 **클라이언트 ID** 값을 복사합니다.

4. **새 키 추가**를 클릭합니다. 다음 화면에서 **키** 값을 복사합니다. 이 페이지를 벗어난 후에는 이 정보에 다시 액세스할 수 없습니다. 자세한 내용은 [Azure AD 응용 프로그램 추가 및 관리](https://msdn.microsoft.com/windows/uwp/publish/manage-account-users#add-and-manage-azure-ad-applications)에서 키 관리에 대한 내용을 참조하세요.

<span id="obtain-an-azure-ad-access-token" />
## 단계 2: Azure AD 액세스 토큰 가져오기

Windows 스토어 분석 API에서 메서드를 호출하기 전에 먼저 API에 있는 각 메서드의 **Authorization** 헤더에 전달하는 Azure AD 액세스 토큰을 가져와야 합니다. 액세스 토큰을 얻은 후 만료되기 전에 60분 동안 사용할 수 있습니다. 토큰이 만료된 후 API에 대한 추가 호출에 계속 사용할 수 있도록 해당 토큰을 새로 고칠 수 있습니다.

액세스 토큰을 가져오려면 [클라이언트 자격 증명을 사용한 서비스 간 호출](https://azure.microsoft.com/documentation/articles/active-directory-protocols-oauth-service-to-service/)의 지시에 따라 HTTP POST를 ```https://login.microsoftonline.com/<tenant_id>/oauth2/token``` 끝점에 보냅니다. 다음은 샘플 요청입니다.

```
POST https://login.microsoftonline.com/<your_tenant_id>/oauth2/token HTTP/1.1
Host: login.microsoftonline.com
Content-Type: application/x-www-form-urlencoded; charset=utf-8

grant_type=client_credentials
&client_id=<your_client_id>
&client_secret=<your_client_secret>
&resource=https://manage.devcenter.microsoft.com
```

*tenant\_id*, *client\_id* 및 *client\_secret* 매개 변수의 경우 이전 섹션의 개발자 센터에서 검색한 응용 프로그램에 대한 테넌트 ID, 클라이언트 ID 및 키를 지정합니다. *resource* 매개 변수의 경우 ```https://manage.devcenter.microsoft.com``` URI를 지정해야 합니다.

만료된 액세스 토큰은 [여기](https://azure.microsoft.com/documentation/articles/active-directory-protocols-oauth-code/#refreshing-the-access-tokens)의 지침에 따라 새로 고칠 수 있습니다.

<span id="call-the-windows-store-analytics-api" />
## 단계 3: Windows 스토어 분석 API 호출

Azure AD 액세스 토큰이 있으면 Windows 스토어 분석 API를 호출할 준비가 된 것입니다. 각 메서드의 구문에 대한 내용은 다음 문서를 참조하세요. 액세스 토큰을 각 메서드의 **Authorization** 헤더로 전달해야 합니다.

-   [앱 획득 가져오기](get-app-acquisitions.md)
-   [추가 기능 구입 가져오기](get-in-app-acquisitions.md)
-   [오류 보고 데이터 가져오기](get-error-reporting-data.md)
-   [앱 등급 가져오기](get-app-ratings.md)
-   [앱 리뷰 가져오기](get-app-reviews.md)
-   [광고 성과 데이터 가져오기](get-ad-performance-data.md)
-   [광고 캠페인 성과 데이터 가져오기](get-ad-campaign-performance-data.md)

## 코드 예제


다음 코드 예제는 Azure AD 액세스 토큰을 얻고 C# 콘솔 앱에서 Windows 스토어 분석 API를 호출하는 방법을 보여 줍니다. 이 코드 예제를 사용하려면 시나리오에 맞는 적절한 값을 *tenantId*, *clientId*, *clientSecret* 및 *appID* 변수에 할당합니다. 이 예제에서 Windows 스토어 분석 API에서 반환된 JSON 데이터를 역직렬화하려면 Newtonsoft의 [Json.NET 패키지](http://www.newtonsoft.com/json)가 필요합니다.

```CSharp
using Newtonsoft.Json;
using System;
using System.Collections.Generic;
using System.Linq;
using System.Net.Http;
using System.Net.Http.Headers;
using System.Text;
using System.Threading.Tasks;

namespace TestAnalyticsAPI
{
    class Program
    {
        static void Main(string[] args)
        {
            string tenantId = "<your tenant ID>";
            string clientId = "<your client ID>";
            string clientSecret = "<your secret>";

            string scope = "https://manage.devcenter.microsoft.com";

            // Retrieve an Azure AD access token
            string accessToken = GetClientCredentialAccessToken(
                    tenantId,
                    clientId,
                    clientSecret,
                    scope).Result;

            // This is your app's Store ID. This ID is available on
            // the App identity page of the Dev Center dashboard.
            string appID = "<your app's Store ID>";

            DateTime startDate = DateTime.Parse("08-01-2015");
            DateTime endDate = DateTime.Parse("11-01-2015");
            int pageSize = 1000;
            int startPageIndex = 0;

            // Call the Windows Store analytics API
            CallAnalyticsAPI(accessToken, appID, startDate, endDate, pageSize, startPageIndex);

            Console.Read();
        }

        private static void CallAnalyticsAPI(string accessToken, string appID, DateTime startDate, DateTime endDate, int top, int skip)
        {
            string requestURI;

            // Get app acquisitions
            requestURI = string.Format(
                "https://manage.devcenter.microsoft.com/v1.0/my/analytics/appacquisitions?applicationId={0}&startDate={1}&endDate={2}&top={3}&skip={4}",
                appID, startDate, endDate, top, skip);

            //// Get add-on acquisitions
            //requestURI = string.Format(
            //    "https://manage.devcenter.microsoft.com/v1.0/my/analytics/inappacquisitions?applicationId={0}&startDate={1}&endDate={2}&top={3}&skip={4}",
            //    appID, startDate, endDate, top, skip);

            //// Get app failures
            //requestURI = string.Format(
            //    "https://manage.devcenter.microsoft.com/v1.0/my/analytics/failurehits?applicationId={0}&startDate={1}&endDate={2}&top={3}&skip={4}",
            //    appID, startDate, endDate, top, skip);

            //// Get app ratings
            //requestURI = string.Format(
            //    "https://manage.devcenter.microsoft.com/v1.0/my/analytics/ratings?applicationId={0}&startDate={1}&endDate={2}top={3}&skip={4}",
            //    appID, startDate, endDate, top, skip);

            //// Get app reviews
            //requestURI = string.Format(
            //    "https://manage.devcenter.microsoft.com/v1.0/my/analytics/reviews?applicationId={0}&startDate={1}&endDate={2}&top={3}&skip={4}",
            //    appID, startDate, endDate, top, skip);

            HttpRequestMessage requestMessage = new HttpRequestMessage(HttpMethod.Get, requestURI);
            requestMessage.Headers.Authorization = new AuthenticationHeaderValue("Bearer", accessToken);

            WebRequestHandler handler = new WebRequestHandler();
            HttpClient httpClient = new HttpClient(handler);

            HttpResponseMessage response = httpClient.SendAsync(requestMessage).Result;

            Console.WriteLine(response);
            Console.WriteLine(response.Content.ReadAsStringAsync().Result);

            response.Dispose();
        }

        public static async Task<string> GetClientCredentialAccessToken(string tenantId, string clientId, string clientSecret, string scope)
        {
            string tokenEndpointFormat = "https://login.microsoftonline.com/{0}/oauth2/token";
            string tokenEndpoint = string.Format(tokenEndpointFormat, tenantId);

            dynamic result;
            using (HttpClient client = new HttpClient())
            {
                string tokenUrl = tokenEndpoint;
                using (
                    HttpRequestMessage request = new HttpRequestMessage(
                        HttpMethod.Post,
                        tokenUrl))
                {
                    string content =
                        string.Format(
                            "grant_type=client_credentials&client_id={0}&client_secret={1}&resource={2}",
                            clientId,
                            clientSecret,
                            scope);

                    request.Content = new StringContent(content, Encoding.UTF8, "application/x-www-form-urlencoded");

                    using (HttpResponseMessage response = await client.SendAsync(request))
                    {
                        string responseContent = await response.Content.ReadAsStringAsync();
                        result = JsonConvert.DeserializeObject(responseContent);
                    }
                }
            }

            return result.access_token;
        }
    }
}
```

## 오류 응답


Windows 스토어 분석 API는 오류 코드와 메시지를 포함하는 오류 응답을 JSON 개체에 반환합니다. 다음 예제에서는 잘못된 매개 변수로 인해 발생한 오류 응답을 보여 줍니다.

```json
{
    "code":"BadRequest",
    "data":[],
    "details":[],
    "innererror":{
        "code":"InvalidQueryParameters",
        "data":[
            "top parameter cannot be more than 10000"
        ],
        "details":[],
        "message":"One or More Query Parameters has invalid values.",
        "source":"AnalyticsAPI"
    },
    "message":"The calling client sent a bad request to the service.",
    "source":"AnalyticsAPI"
}
```

## 관련 항목

* [앱 획득 가져오기](get-app-acquisitions.md)
* [추가 기능 구입 가져오기](get-in-app-acquisitions.md)
* [오류 보고 데이터 가져오기](get-error-reporting-data.md)
* [앱 등급 가져오기](get-app-ratings.md)
* [앱 리뷰 가져오기](get-app-reviews.md)
* [광고 성과 데이터 가져오기](get-ad-performance-data.md)
* [광고 캠페인 성과 데이터 가져오기](get-ad-campaign-performance-data.md)
 



<!--HONumber=Nov16_HO1-->


