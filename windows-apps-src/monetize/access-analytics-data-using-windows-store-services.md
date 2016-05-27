---
author: mcleanbyron
ms.assetid: 4BF9EF21-E9F0-49DB-81E4-062D6E68C8B1
description: Windows 스토어 분석 API를 사용하여 프로그래밍 방식으로 사용자 또는 사용자 조직의 Windows 개발자 센터 계정에 등록된 앱에 대한 분석 데이터를 검색합니다.
title: Windows 스토어 서비스를 사용하여 분석 데이터에 액세스
---

# Windows 스토어 서비스를 사용하여 분석 데이터에 액세스


\[ Windows 10의 UWP 앱에 맞게 업데이트되었습니다. Windows 8.x 문서는 [보관](http://go.microsoft.com/fwlink/p/?linkid=619132)을 참조하세요. \]


*Windows 스토어 분석 API*를 사용하여 프로그래밍 방식으로 조직의 Windows 개발자 센터 계정에 등록된 앱에 대한 분석 데이터를 검색합니다. 이 API를 사용하면 앱 및 IAP 구입, 오류, 앱 평점 및 리뷰 데이터를 검색할 수 있습니다. 이 API는 Azure AD(Azure Active Directory)를 사용하여 앱 또는 서비스의 호출을 인증합니다.

## Windows 스토어 분석 API를 사용하기 위한 필수 조건


-   사용자 또는 사용자 조직에 Azure AD 디렉터리가 있어야 합니다. 이미 Office 365 또는 Microsoft의 다른 비즈니스 서비스를 사용하는 경우 이미 Azure AD 디렉터리가 있습니다. 그렇지 않은 경우 [무료로 얻을](http://go.microsoft.com/fwlink/p/?LinkId=703757) 수 있습니다.
-   Windows 개발자 센터 계정과 연결할 Azure AD 디렉터리에 [사용자 계정](https://azure.microsoft.com/documentation/articles/active-directory-create-users/)이 있어야 합니다.

## Windows 스토어 분석 API 사용


Windows 스토어 분석 API를 사용하려면 먼저 Azure AD 응용 프로그램을 개발자 센터 계정과 연결하고 Azure AD 액세스 토큰을 가져와야 합니다. Azure AD 응용 프로그램은 Windows 스토어 분석 API를 호출할 앱 또는 서비스입니다. 액세스 토큰이 있으면 앱 또는 서비스에서 Windows 스토어 분석 API를 호출할 수 있습니다.

다음 단계에서는 종단 간 프로세스를 설명합니다.

1.  [Azure AD 응용 프로그램을 Windows 개발자 센터 계정과 연결합니다](#associate-an-azure-ad-application-with-your-windows-dev-center-account).
2.  [Azure AD 액세스 토큰을 가져옵니다](#obtain-an-azure-ad-access-token).
3.  [Windows 스토어 분석 API를 호출](#call-the-windows-store-analytics-api)합니다.


### Azure AD 응용 프로그램을 Windows 개발자 센터 계정과 연결

1.  개발자 센터에서 **계정 설정**으로 이동하여 **사용자 관리**를 클릭하고 조직의 개발자 센터 계정을 조직의 Azure AD 디렉터리와 연결합니다. 자세한 내용은 [계정 사용자 관리](https://msdn.microsoft.com/library/windows/apps/mt489008)를 참조하세요. 개발자 센터 계정에 액세스할 수 있도록 조직의 Azure AD 디렉터리에서 다른 사용자를 선택적으로 추가할 수 있습니다.

    **참고** Azure Active Directory에는 단 하나의 개발자 센터 계정만 연결할 수 있습니다. 마찬가지로 개발자 센터 계정은 단 하나의 Azure Active Directory만 연결할 수 있습니다. 이 연결을 설정한 후에는 제거할 때 고객 지원에 반드시 문의해야 합니다.

     

2.  **사용자 관리** 페이지에서 **Azure AD 응용 프로그램 추가**를 클릭하여 개발자 센터 계정에 대한 분석 데이터에 액세스하는 데 사용할 Azure AD 응용 프로그램을 추가하고 **관리자** 역할에 할당합니다. 이 응용 프로그램이 Azure AD 디렉터리에 이미 존재하는 경우 **Azure AD 응용 프로그램 추가**에서 선택하여 개발자 센터 계정에 추가할 수 있습니다. 그러지 않으면 **Azure AD 응용 프로그램 추가 페이지**에서 새 Azure AD 응용 프로그램을 만들 수 있습니다. 자세한 내용은 [계정 사용자 관리](https://msdn.microsoft.com/library/windows/apps/mt489008)에서 Azure AD 응용 프로그램을 관리하는 방법에 대한 섹션을 참조하세요.

3.  **사용자 관리** 페이지로 돌아가서 Azure AD 응용 프로그램의 이름을 클릭하여 응용 프로그램 설정으로 이동하고 **새 키 추가**를 클릭합니다. 다음 화면에서 **클라이언트 ID** 및 **키** 값을 복사합니다. 자세한 내용은 [계정 사용자 관리](https://msdn.microsoft.com/library/windows/apps/mt489008)에서 Azure AD 응용 프로그램을 관리하는 방법에 대한 섹션을 참조하세요. Windows 스토어 분석 API를 호출할 때 사용할 Azure AD 액세스 토큰을 가져오려면 이러한 클라이언트 ID 및 키가 필요합니다. 이 페이지를 벗어난 후에는 이 정보에 다시 액세스할 수 없습니다.


### Azure AD 액세스 토큰 가져오기

Azure AD 응용 프로그램을 개발자 센터 계정과 연결하고 응용 프로그램에 대한 클라이언트 ID 및 키를 검색한 후 이 정보를 사용하여 Azure AD 액세스 토큰을 가져올 수 있습니다. Windows 스토어 분석 API에서 메서드를 호출하려면 액세스 토큰이 필요합니다.

액세스 토큰을 가져오려면 [클라이언트 자격 증명을 사용한 서비스 간 호출](https://msdn.microsoft.com/library/azure/dn645543.aspx)의 지시에 따라 HTTP POST를 다음 Azure AD 끝점에 보냅니다.

```syntax
https://login.microsoftonline.com/<tenant id>/oauth2/token
```

-   테넌트 ID를 가져오려면 [Azure 관리 포털](http://manage.windowsazure.com/)에 로그인하고 **Active Directory**로 이동한 다음 개발자 센터 계정에 연결된 디렉터리를 클릭합니다. 이 디렉터리에 대한 테넌트 ID는 아래 예제의 *your\_tenant\_ID* 문자열에서처럼 이 페이지의 URL에 포함되어 있습니다.

  ```syntax
  https://manage.windowsazure.com/@<your_tenant_name>#Workspaces/ActiveDirectoryExtension/Directory/<your_tenant_ID>/directoryQuickStart
  ```

-   *client\_id* 및 *client\_secret* 매개 변수의 경우 개발자 센터에서 이전에 검색한 응용 프로그램의 클라이언트 ID 및 키를 지정합니다.
-   *리소스* 매개 변수의 경우 다음 URI를 지정합니다. ```https://manage.devcenter.microsoft.com```.


### Windows 스토어 분석 API 호출

Azure AD 액세스 토큰이 있으면 Windows 스토어 분석 API를 호출할 준비가 된 것입니다. 각 메서드의 구문에 대한 내용은 다음 문서를 참조하세요. 액세스 토큰을 각 메서드의 **Authorization** 헤더로 전달해야 합니다.

-   [앱 구입 가져오기](get-app-acquisitions.md)
-   [IAP 구입 가져오기](get-in-app-acquisitions.md)
-   [오류 보고 데이터 가져오기](get-error-reporting-data.md)
-   [앱 등급 가져오기](get-app-ratings.md)
-   [앱 리뷰 가져오기](get-app-reviews.md)

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

            // This is your app's product ID. This ID is embedded in the app's listing link
            // on the App identity page of the Dev Center dashboard.
            string appID = "<your app's product ID>";

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

            //// Get IAP acquisitions
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

* [앱 구입 가져오기](get-app-acquisitions.md)
* [IAP 구입 가져오기](get-in-app-acquisitions.md)
* [오류 보고 데이터 가져오기](get-error-reporting-data.md)
* [앱 등급 가져오기](get-app-ratings.md)
* [앱 리뷰 가져오기](get-app-reviews.md)
 


<!--HONumber=May16_HO2-->


