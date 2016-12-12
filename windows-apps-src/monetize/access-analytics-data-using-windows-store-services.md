---
author: mcleanbyron
ms.assetid: 4BF9EF21-E9F0-49DB-81E4-062D6E68C8B1
description: Use the Windows Store analytics API to programmatically retrieve analytics data for apps that are registered to your or your organization&quot;&quot;s Windows Dev Center account.
title: Access analytics data using Windows Store services
translationtype: Human Translation
ms.sourcegitcommit: dcf4c263ff3fd8df846d1d5620ba31a9da7a5e6c
ms.openlocfilehash: 5ae5dcbe6684aa34a1428760cd5c7e9b8f599ebf

---

# <a name="access-analytics-data-using-windows-store-services"></a>Access analytics data using Windows Store services

Use the *Windows Store analytics API* to programmatically retrieve analytics data for apps that are registered to your or your organization's Windows Dev Center account. This API enables you to retrieve data for app and add-on (also known as in-app product or IAP) acquisitions, errors, app ratings and reviews. This API uses Azure Active Directory (Azure AD) to authenticate the calls from your app or service.

The following steps describe the end-to-end process:

1.  Make sure that you have completed all the [prerequisites](#prerequisites).
2.  Before you call a method in the Windows Store analytics API, [obtain an Azure AD access token](#obtain-an-azure-ad-access-token). After you obtain a token, you have 60 minutes to use this token in calls to the Windows Store analytics API before the token expires. After the token expires, you can generate a new token.
3.  [Call the Windows Store analytics API](#call-the-windows-store-analytics-api).

<span id="prerequisites" />
## <a name="step-1-complete-prerequisites-for-using-the-windows-store-analytics-api"></a>Step 1: Complete prerequisites for using the Windows Store analytics API

Before you start writing code to call the Windows Store analytics API, make sure that you have completed the following prerequisites.

* You (or your organization) must have an Azure AD directory and you must have [Global administrator](http://go.microsoft.com/fwlink/?LinkId=746654) permission for the directory. If you already use Office 365 or other business services from Microsoft, you already have Azure AD directory. Otherwise, you can [create a new Azure AD in Dev Center](https://msdn.microsoft.com/windows/uwp/publish/manage-account-users) for no additional charge.

* You must associate an Azure AD application with your Dev Center account, retrieve the tenant ID and client ID for the application and generate a key. The Azure AD application represents the app or service from which you want to call the Windows Store analytics API. You need the tenant ID, client ID and key to obtain an Azure AD access token that you pass to the API.

  >**Note**&nbsp;&nbsp;You only need to perform this task one time. After you have the tenant ID, client ID and key, you can reuse them any time you need to create a new Azure AD access token.

To associate an Azure AD application with your Dev Center account and retrieve the required values:

1.  In Dev Center, go to your **Account settings**, click **Manage users**, and associate your organization's Dev Center account with your organization's Azure AD directory. For detailed instructions, see [Manage account users](https://msdn.microsoft.com/windows/uwp/publish/manage-account-users).

2.  In the **Manage users** page, click **Add Azure AD applications**, add the Azure AD application that represents the app or service that you will use to access analytics data for your Dev Center account, and assign it the **Manager** role. If this application already exists in your Azure AD directory, you can select it on the **Add Azure AD applications** page to add it to your Dev Center account. Otherwise, you can create a new Azure AD application on the **Add Azure AD applications** page. For more information, see [Add and manage Azure AD applications](https://msdn.microsoft.com/windows/uwp/publish/manage-account-users#add-and-manage-azure-ad-applications).

3.  Return to the **Manage users** page, click the name of your Azure AD application to go to the application settings, and copy down the **Tenant ID** and **Client ID** values.

4. Click **Add new key**. On the following screen, copy down the **Key** value. You won't be able to access this info again after you leave this page. For more information, see the information about managing keys in [Add and manage Azure AD applications](https://msdn.microsoft.com/windows/uwp/publish/manage-account-users#add-and-manage-azure-ad-applications).

<span id="obtain-an-azure-ad-access-token" />
## <a name="step-2-obtain-an-azure-ad-access-token"></a>Step 2: Obtain an Azure AD access token

Before you call any of the methods in the Windows Store analytics API, you must first obtain an Azure AD access token that you pass to the **Authorization** header of each method in the API. After you obtain an access token, you have 60 minutes to use it before it expires. After the token expires, you can refresh the token so you can continue to use it in further calls to the API.

To obtain the access token, follow the instructions in [Service to Service Calls Using Client Credentials](https://azure.microsoft.com/documentation/articles/active-directory-protocols-oauth-service-to-service/) to send an HTTP POST to the ```https://login.microsoftonline.com/<tenant_id>/oauth2/token``` endpoint. Here is a sample request.

```
POST https://login.microsoftonline.com/<your_tenant_id>/oauth2/token HTTP/1.1
Host: login.microsoftonline.com
Content-Type: application/x-www-form-urlencoded; charset=utf-8

grant_type=client_credentials
&client_id=<your_client_id>
&client_secret=<your_client_secret>
&resource=https://manage.devcenter.microsoft.com
```

For the *tenant\_id*, *client\_id* and *client\_secret* parameters, specify the tenant ID, client ID and the key for your application that you retrieved from Dev Center in the previous section. For the *resource* parameter, you must specify the ```https://manage.devcenter.microsoft.com``` URI.

After your access token expires, you can refresh it by following the instructions [here](https://azure.microsoft.com/documentation/articles/active-directory-protocols-oauth-code/#refreshing-the-access-tokens).

<span id="call-the-windows-store-analytics-api" />
## <a name="step-3-call-the-windows-store-analytics-api"></a>Step 3: Call the Windows Store analytics API

After you have an Azure AD access token, you are ready to call the Windows Store analytics API. For information about the syntax of each method, see the following articles. You must pass the access token to the **Authorization** header of each method.

* [Get app acquisitions](get-app-acquisitions.md)
* [Get add-on acquisitions](get-in-app-acquisitions.md)
* [Get error reporting data](get-error-reporting-data.md)
* [Get details for an error in your app](get-details-for-an-error-in-your-app.md)
* [Get the stack trace for an error in your app](get-the-stack-trace-for-an-error-in-your-app.md)
* [Get app ratings](get-app-ratings.md)
* [Get app reviews](get-app-reviews.md)
* [Get ad performance data](get-ad-performance-data.md)
* [Get ad campaign performance data](get-ad-campaign-performance-data.md)

## <a name="code-example"></a>Code example


The following code example demonstrates how to obtain an Azure AD access token and call the Windows Store analytics API from a C# console app. To use this code example, assign the *tenantId*, *clientId*, *clientSecret*, and *appID* variables to the appropriate values for your scenario. This example requires the [Json.NET package](http://www.newtonsoft.com/json) from Newtonsoft to deserialize the JSON data returned by the Windows Store analytics API.

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

## <a name="error-responses"></a>Error responses


The Windows Store analytics API returns error responses in a JSON object that contains error codes and messages. The following example demonstrates an error response caused by an invalid parameter.

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

## <a name="related-topics"></a>Related topics

* [Get app acquisitions](get-app-acquisitions.md)
* [Get add-on acquisitions](get-in-app-acquisitions.md)
* [Get error reporting data](get-error-reporting-data.md)
* [Get details for an error in your app](get-details-for-an-error-in-your-app.md)
* [Get the stack trace for an error in your app](get-the-stack-trace-for-an-error-in-your-app.md)
* [Get app ratings](get-app-ratings.md)
* [Get app reviews](get-app-reviews.md)
* [Get ad performance data](get-ad-performance-data.md)
* [Get ad campaign performance data](get-ad-campaign-performance-data.md)
 



<!--HONumber=Dec16_HO1-->


