---
description: HttpClient와 Windows.Web.Http 네임스페이스 API의 나머지를 사용하여 HTTP 2.0 및 HTTP 1.1 프로토콜을 통해 정보를 보내고 받습니다.
title: HttpClient
ms.assetid: EC9820D3-3A46-474F-8A01-AE1C27442750
ms.date: 6/5/2019
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: bb098aae346c7a81771262793f5f6a042d62d5a3
ms.sourcegitcommit: 1f39b67f2711b96c6b4e7ed7107a9a47127d4e8f
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 06/05/2019
ms.locfileid: "66721613"
---
# <a name="httpclient"></a>HttpClient

**중요 한 Api**

-   [**HttpClient**](https://docs.microsoft.com/uwp/api/Windows.Web.Http.HttpClient)
-   [**Windows.Web.Http**](https://docs.microsoft.com/uwp/api/Windows.Web.Http)
-   [**Windows.Web.Http.HttpResponseMessage**](https://docs.microsoft.com/uwp/api/Windows.Web.Http.HttpResponseMessage)

[  **HttpClient**](https://docs.microsoft.com/uwp/api/Windows.Web.Http.HttpClient)와 [**Windows.Web.Http**](https://docs.microsoft.com/uwp/api/Windows.Web.Http) 네임스페이스 API의 나머지를 사용하여 HTTP 2.0 및 HTTP 1.1 프로토콜을 통해 정보를 보내고 받습니다.

## <a name="overview-of-httpclient-and-the-windowswebhttp-namespace"></a>HttpClient 및 Windows.Web.Http 네임스페이스 개요

[  **Windows.Web.Http**](https://docs.microsoft.com/uwp/api/Windows.Web.Http) 네임스페이스와 관련 [**Windows.Web.Http.Headers**](https://docs.microsoft.com/uwp/api/Windows.Web.Http.Headers) 및 [**Windows.Web.Http.Filters**](https://docs.microsoft.com/uwp/api/Windows.Web.Http.Filters) 네임스페이스의 클래스는 HTTP 클라이언트 역할을 수행하여 기본 GET 요청을 수행하거나 아래에 나열된 고급 HTTP 기능을 구현하는 UWP(유니버설 Windows 플랫폼) 앱의 프로그래밍 인터페이스를 제공합니다.

-   일반적인 동사에 대한 메서드(**DELETE**, **GET**, **PUT** 및 **POST**) 이러한 각 요청은 비동기 작업으로 전송됩니다.

-   일반적인 인증 설정 및 패턴에 대한 지원

-   전송 시 SSL(Secure Sockets Layer) 세부 정보에 대한 액세스

-   고급 앱에 사용자 지정 필터를 포함하는 기능

-   쿠키를 가져오고 설정하고 삭제하는 기능

-   비동기 메서드에서 사용할 수 있는 HTTP 요청 진행 정보

[  **Windows.Web.Http.HttpRequestMessage**](https://docs.microsoft.com/uwp/api/Windows.Web.Http.HttpRequestMessage) 클래스는 [**Windows.Web.Http.HttpClient**](https://docs.microsoft.com/uwp/api/Windows.Web.Http.HttpClient)에서 전송된 HTTP 요청 메시지를 나타냅니다. [  **Windows.Web.Http.HttpResponseMessage**](https://docs.microsoft.com/uwp/api/Windows.Web.Http.HttpResponseMessage) 클래스는 HTTP 요청에서 받은 HTTP 응답 메시지를 나타냅니다. HTTP 메시지는 IETF에 의해 [RFC 2616](https://go.microsoft.com/fwlink/p/?linkid=241642)에서 정의됩니다.

[  **Windows.Web.Http**](https://docs.microsoft.com/uwp/api/Windows.Web.Http) 네임스페이스는 HTTP 콘텐츠를 쿠키가 포함된 HTTP 엔터티 본문 및 헤더로 나타냅니다. HTTP 콘텐츠를 HTTP 요청 또는 HTTP 응답에 연결할 수 있습니다. **Windows.Web.Http** 네임스페이스는 HTTP 컨텐츠를 나타내는 여러 클래스를 제공합니다.

-   [**HttpBufferContent**](https://docs.microsoft.com/uwp/api/Windows.Web.Http.HttpBufferContent). 콘텐츠를 버퍼로
-   [**HttpFormUrlEncodedContent**](https://docs.microsoft.com/uwp/api/Windows.Web.Http.HttpFormUrlEncodedContent). 콘텐츠를 **application/x-www-form-urlencoded** MIME 형식으로 인코드된 이름/값 튜플로
-   [**HttpMultipartContent**](https://docs.microsoft.com/uwp/api/Windows.Web.Http.HttpMultipartContent). 형식의 콘텐츠를 **다중 파트 /\***  MIME 형식입니다.
-   [**HttpMultipartFormDataContent**](https://docs.microsoft.com/uwp/api/Windows.Web.Http.HttpMultipartFormDataContent). **multipart/form-data** MIME 형식으로 인코딩된 콘텐츠
-   [**HttpStreamContent**](https://docs.microsoft.com/uwp/api/Windows.Web.Http.HttpStreamContent). 콘텐츠를 스트림으로(내부 형식은 HTTP GET 메서드가 데이터를 받고 HTTP POST 메서드가 데이터를 업로드하는 데 사용됨)
-   [**HttpStringContent**](https://docs.microsoft.com/uwp/api/Windows.Web.Http.HttpStringContent). 콘텐츠를 문자열로
-   [**IHttpContent** ](https://docs.microsoft.com/uwp/api/Windows.Web.Http.IHttpContent) -자체 콘텐츠 개체를 만드는 개발자를 위한 기본 인터페이스

"HTTP를 통해 간단한 GET 요청 보내기" 섹션의 코드 조각에서는 [**HttpStringContent**](https://docs.microsoft.com/uwp/api/Windows.Web.Http.HttpStringContent) 클래스를 사용하여 HTTP GET 요청의 HTTP 응답을 문자열로 나타냅니다.

[  **Windows.Web.Http.Headers**](https://docs.microsoft.com/uwp/api/Windows.Web.Http.Headers) 네임스페이스에서는 HTTP 머리글 및 쿠키 만들기를 지원하며 이러한 항목이 이후 속성으로 [**HttpRequestMessage**](https://docs.microsoft.com/uwp/api/Windows.Web.Http.HttpRequestMessage) 및 [**HttpResponseMessage**](https://docs.microsoft.com/uwp/api/Windows.Web.Http.HttpResponseMessage) 개체와 연결됩니다.

## <a name="send-a-simple-get-request-over-http"></a>HTTP를 통해 간단한 GET 요청 보내기

이 문서의 앞부분에서 설명한 것처럼 [**Windows.Web.Http**](https://docs.microsoft.com/uwp/api/Windows.Web.Http) 네임스페이스는 UWP 앱에서 GET 요청을 보낼 수 있도록 합니다. 다음 코드 조각에는 GET 요청을 보내는 방법을 보여 줍니다 http://www.contoso.com 를 사용 하는 [ **Windows.Web.Http.HttpClient** ](https://docs.microsoft.com/uwp/api/Windows.Web.Http.HttpClient) 클래스와 [  **Windows.Web.Http.HttpResponseMessage** ](https://docs.microsoft.com/uwp/api/Windows.Web.Http.HttpResponseMessage) GET 요청에서 응답을 읽는 클래스입니다.

```csharp
//Create an HTTP client object
Windows.Web.Http.HttpClient httpClient = new Windows.Web.Http.HttpClient();

//Add a user-agent header to the GET request. 
var headers = httpClient.DefaultRequestHeaders;

//The safe way to add a header value is to use the TryParseAdd method and verify the return value is true,
//especially if the header value is coming from user input.
string header = "ie";
if (!headers.UserAgent.TryParseAdd(header))
{
    throw new Exception("Invalid header value: " + header);
}

header = "Mozilla/5.0 (compatible; MSIE 10.0; Windows NT 6.2; WOW64; Trident/6.0)";
if (!headers.UserAgent.TryParseAdd(header))
{
    throw new Exception("Invalid header value: " + header);
}

Uri requestUri = new Uri("http://www.contoso.com");

//Send the GET request asynchronously and retrieve the response as a string.
Windows.Web.Http.HttpResponseMessage httpResponse = new Windows.Web.Http.HttpResponseMessage();
string httpResponseBody = "";

try
{
    //Send the GET request
    httpResponse = await httpClient.GetAsync(requestUri);
    httpResponse.EnsureSuccessStatusCode();
    httpResponseBody = await httpResponse.Content.ReadAsStringAsync();
}
catch (Exception ex)
{
    httpResponseBody = "Error: " + ex.HResult.ToString("X") + " Message: " + ex.Message;
}
```

```cppwinrt
// pch.h
#pragma once
#include <winrt/Windows.Foundation.h>
#include <winrt/Windows.Web.Http.Headers.h>

// main.cpp : Defines the entry point for the console application.
#include "pch.h"
#include <iostream>
using namespace winrt;
using namespace Windows::Foundation;

int main()
{
    init_apartment();

    // Create an HttpClient object.
    Windows::Web::Http::HttpClient httpClient;

    // Add a user-agent header to the GET request.
    auto headers{ httpClient.DefaultRequestHeaders() };

    // The safe way to add a header value is to use the TryParseAdd method, and verify the return value is true.
    // This is especially important if the header value is coming from user input.
    std::wstring header{ L"ie" };
    if (!headers.UserAgent().TryParseAdd(header))
    {
        throw L"Invalid header value: " + header;
    }

    header = L"Mozilla/5.0 (compatible; MSIE 10.0; Windows NT 6.2; WOW64; Trident/6.0)";
    if (!headers.UserAgent().TryParseAdd(header))
    {
        throw L"Invalid header value: " + header;
    }

    Uri requestUri{ L"http://www.contoso.com" };

    // Send the GET request asynchronously, and retrieve the response as a string.
    Windows::Web::Http::HttpResponseMessage httpResponseMessage;
    std::wstring httpResponseBody;

    try
    {
        // Send the GET request.
        httpResponseMessage = httpClient.GetAsync(requestUri).get();
        httpResponseMessage.EnsureSuccessStatusCode();
        httpResponseBody = httpResponseMessage.Content().ReadAsStringAsync().get();
    }
    catch (winrt::hresult_error const& ex)
    {
        httpResponseBody = ex.message();
    }
    std::wcout << httpResponseBody;
}
```

## <a name="post-binary-data-over-http"></a>HTTP 통해 게시 이진 데이터

합니다 [ C++/WinRT](/windows/uwp/cpp-and-winrt-apis) 아래 코드 예제에서는 양식 데이터 및 POST 요청을 사용 하 여 웹 서버에 적은 양의 이진 데이터 파일 업로드로 전송 하는 방법을 보여 줍니다. 코드를 사용 하는 [ **HttpBufferContent** ](/uwp/api/windows.web.http.httpbuffercontent) 이진 데이터를 나타내는 클래스 및 [ **HttpMultipartFormDataContent** ](/uwp/api/windows.web.http.httpmultipartformdatacontent) 클래스 다중 파트 양식 데이터를 나타냅니다.

> [!NOTE]
> 호출 **가져올** (아래 코드 예제에 표시)으로 UI 스레드에 대 한 적절 한 없습니다. 이 경우 사용 하는 올바른 기법을 참조 하세요 [동시성 및 비동기 작업을 C++/WinRT](/windows/uwp/cpp-and-winrt-apis/concurrency)합니다.

```cppwinrt
// pch.h
#pragma once
#include <winrt/Windows.Foundation.h>
#include <winrt/Windows.Security.Cryptography.h>
#include <winrt/Windows.Storage.Streams.h>
#include <winrt/Windows.Web.Http.Headers.h>

// main.cpp : Defines the entry point for the console application.
#include "pch.h"
#include <iostream>
#include <sstream>
using namespace winrt;
using namespace Windows::Foundation;
using namespace Windows::Storage::Streams;

int main()
{
    init_apartment();

    auto buffer{
        Windows::Security::Cryptography::CryptographicBuffer::ConvertStringToBinary(
            L"A sentence of text to encode into binary to serve as sample data.",
            Windows::Security::Cryptography::BinaryStringEncoding::Utf8
        )
    };
    Windows::Web::Http::HttpBufferContent binaryContent{ buffer };
    // You can use the 'image/jpeg' content type to represent any binary data;
    // it's not necessarily an image file.
    binaryContent.Headers().Append(L"Content-Type", L"image/jpeg");

    Windows::Web::Http::Headers::HttpContentDispositionHeaderValue disposition{ L"form-data" };
    binaryContent.Headers().ContentDisposition(disposition);
    // The 'name' directive contains the name of the form field representing the data.
    disposition.Name(L"fileForUpload");
    // Here, the 'filename' directive is used to indicate to the server a file name
    // to use to save the uploaded data.
    disposition.FileName(L"file.dat");

    Windows::Web::Http::HttpMultipartFormDataContent postContent;
    postContent.Add(binaryContent); // Add the binary data content as a part of the form data content.

    // Send the POST request asynchronously, and retrieve the response as a string.
    Windows::Web::Http::HttpResponseMessage httpResponseMessage;
    std::wstring httpResponseBody;

    try
    {
        // Send the POST request.
        Uri requestUri{ L"https://www.contoso.com/post" };
        Windows::Web::Http::HttpClient httpClient;
        httpResponseMessage = httpClient.PostAsync(requestUri, postContent).get();
        httpResponseMessage.EnsureSuccessStatusCode();
        httpResponseBody = httpResponseMessage.Content().ReadAsStringAsync().get();
    }
    catch (winrt::hresult_error const& ex)
    {
        httpResponseBody = ex.message();
    }
    std::wcout << httpResponseBody;
}
```

실제 이진 파일 (위에서 사용 된 명시적 이진 데이터 대신)의 콘텐츠를 게시 하기 위해 찾을 수 있습니다 보다 쉽게 사용할 수는 [HttpStreamContent](/uwp/api/windows.web.http.httpstreamcontent) 개체입니다. 하나를 생성 하 고 해당 생성자에 인수로 전달에 대 한 호출에서 반환 된 값 [StorageFile.OpenReadAsync](/uwp/api/windows.storage.storagefile.openreadasync)합니다. 해당 메서드는 이진 파일 내에 있는 데이터에 대 한 스트림을 반환합니다.

또한 (약 10MB 보다 큰) 큰 파일을 업로드 하는 경우 다음 권장 Windows 런타임을 사용 하는 [백그라운드 전송](/uwp/api/windows.networking.backgroundtransfer) Api.

## <a name="post-json-data-over-http"></a>HTTP 통한 POST JSON 데이터

다음 예제에서는 일부 JSON 끝점을 게시 한 다음 응답 본문을 작성 합니다.

```cs
using System;
using System.Diagnostics;
using System.Threading.Tasks;
using Windows.Storage.Streams;
using Windows.Web.Http;

private async Task TryPostJsonAsync()
{
    try
    {
        // Construct the HttpClient and Uri. This endpoint is for test purposes only.
        HttpClient httpClient = new HttpClient();
        Uri uri = new Uri("https://www.contoso.com/post");

        // Construct the JSON to post.
        HttpStringContent content = new HttpStringContent(
            "{ \"firstName\": \"Eliot\" }",
            UnicodeEncoding.Utf8,
            "application/json");

        // Post the JSON and wait for a response.
        HttpResponseMessage httpResponseMessage = await httpClient.PostAsync(
            uri,
            content);

        // Make sure the post succeeded, and write out the response.
        httpResponseMessage.EnsureSuccessStatusCode();
        var httpResponseBody = await httpResponseMessage.Content.ReadAsStringAsync();
        Debug.WriteLine(httpResponseBody);
    }
    catch (Exception ex)
    {
        // Write out any exceptions.
        Debug.WriteLine(ex);
    }
}
```

## <a name="exceptions-in-windowswebhttp"></a>Windows.Web.Http의 예외

잘못된 URI(Uniform Resource Identifier) 문자열이 [**Windows.Foundation.Uri**](https://docs.microsoft.com/uwp/api/Windows.Foundation.Uri) 개체에 대한 생성자에 전달되면 예외가 발생합니다.

**.NET:**   는 [ **Windows.Foundation.Uri** ](https://docs.microsoft.com/uwp/api/Windows.Foundation.Uri) 형식으로 표시 됩니다 [ **System.Uri** ](https://docs.microsoft.com/dotnet/api/system.uri?redirectedfrom=MSDN) 에서 C# 및 VB.

C# 및 Visual Basic에서는 .NET 4.5의 [**System.Uri**](https://docs.microsoft.com/dotnet/api/system.uri?redirectedfrom=MSDN) 클래스와 [**System.Uri.TryCreate**](https://docs.microsoft.com/dotnet/api/system.uri.trycreate?redirectedfrom=MSDN#overloads) 메서드 중 하나를 통해 URI가 생성되기 전에 사용자로부터 받은 문자열을 테스트하여 이 오류를 방지할 수 있습니다.

C++에는 URI에 대한 문자열을 시도 및 구문 분석할 메서드가 없습니다. 앱이 [**Windows.Foundation.Uri**](https://docs.microsoft.com/uwp/api/Windows.Foundation.Uri)에 대해 사용자 입력을 받으면 생성자는 try/catch 블록에 있게 됩니다. 예외가 발생하면 앱에서 사용자에게 알리고 새 호스트 이름을 요청할 수 있습니다.

[  **Windows.Web.Http**](https://docs.microsoft.com/uwp/api/Windows.Web.Http)에는 편의 기능이 부족합니다. 따라서 이 네임스페이스에서 [**HttpClient**](https://docs.microsoft.com/uwp/api/Windows.Web.Http.HttpClient) 및 다른 클래스를 사용하는 앱은 **HRESULT** 값을 사용해야 합니다.

.NET Framework 4.5를 사용 하 여 앱에서 C#, VB.NET 합니다 [System.Exception](https://docs.microsoft.com/dotnet/api/system.exception?redirectedfrom=MSDN) 예외가 발생할 때 앱을 실행 하는 동안 오류를 나타냅니다. [System.Exception.HResult](https://docs.microsoft.com/dotnet/api/system.exception.hresult?redirectedfrom=MSDN#System_Exception_HResult) 속성은 특정 예외에 할당된 **HRESULT**를 반환합니다. [System.Exception.Message](https://docs.microsoft.com/dotnet/api/system.exception.message?redirectedfrom=MSDN#System_Exception_Message) 속성은 예외를 설명하는 메시지를 반환합니다. 가능한 **HRESULT** 값은 *Winerror.h* 헤더 파일에 나열되어 있습니다. 앱은 특정 **HRESULT** 값을 필터링하여 예외의 원인에 따라 앱 동작을 수정할 수 있습니다.

관리되는 C++을 사용하는 앱에서 [Platform::Exception](https://docs.microsoft.com/cpp/cppcx/platform-exception-class)은 앱 실행 중 예외가 발생하는 경우의 오류를 나타냅니다. [Platform::Exception::HResult](https://docs.microsoft.com/cpp/cppcx/platform-exception-class#hresult) 속성은 특정 예외에 할당된 **HRESULT**를 반환합니다. [Platform::Exception::Message](https://docs.microsoft.com/cpp/cppcx/platform-exception-class#message) 속성은 **HRESULT** 값과 연결된 시스템 제공 문자열을 반환합니다. 가능한 **HRESULT** 값은 *Winerror.h* 헤더 파일에 나열되어 있습니다. 앱은 특정 **HRESULT** 값을 필터링하여 예외의 원인에 따라 앱 동작을 수정할 수 있습니다.

대부분의 매개 변수 유효성 검사 오류에 대 한 합니다 **HRESULT** 반환 된 **E\_INVALIDARG**합니다. 일부 잘못 된 메서드 호출을 **HRESULT** 반환 되 **E\_잘못 된\_메서드\_호출**합니다.

