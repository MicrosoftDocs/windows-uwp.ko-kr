---
author: stevewhims
description: HttpClient와 Windows.Web.Http 네임스페이스 API의 나머지를 사용하여 HTTP 2.0 및 HTTP 1.1 프로토콜을 통해 정보를 보내고 받습니다.
title: HttpClient
ms.assetid: EC9820D3-3A46-474F-8A01-AE1C27442750
ms.author: stwhi
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: c874c690826dfa74b8dcb2312204cd549db3db2b
ms.sourcegitcommit: cbe7cf620622a5e4df7414f9e38dfecec1cfca99
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/21/2018
ms.locfileid: "7436990"
---
# <a name="httpclient"></a>HttpClient


**중요 API**

-   [**HttpClient**](https://msdn.microsoft.com/library/windows/apps/dn298639)
-   [**Windows.Web.Http**](https://msdn.microsoft.com/library/windows/apps/dn279692)
-   [**Windows.Web.Http.HttpResponseMessage**](https://msdn.microsoft.com/library/windows/apps/dn279631)

[**HttpClient**](https://msdn.microsoft.com/library/windows/apps/dn298639)와 [**Windows.Web.Http**](https://msdn.microsoft.com/library/windows/apps/dn279692) 네임스페이스 API의 나머지를 사용하여 HTTP 2.0 및 HTTP 1.1 프로토콜을 통해 정보를 보내고 받습니다.

## <a name="overview-of-httpclient-and-the-windowswebhttp-namespace"></a>HttpClient 및 Windows.Web.Http 네임스페이스 개요

[**Windows.Web.Http**](https://msdn.microsoft.com/library/windows/apps/dn279692) 네임스페이스와 관련 [**Windows.Web.Http.Headers**](https://msdn.microsoft.com/library/windows/apps/dn252713) 및 [**Windows.Web.Http.Filters**](https://msdn.microsoft.com/library/windows/apps/dn298623) 네임스페이스의 클래스는 HTTP 클라이언트 역할을 수행하여 기본 GET 요청을 수행하거나 아래에 나열된 고급 HTTP 기능을 구현하는 UWP(유니버설 Windows 플랫폼) 앱의 프로그래밍 인터페이스를 제공합니다.

-   일반적인 동사에 대한 메서드(**DELETE**, **GET**, **PUT** 및 **POST**) 이러한 각 요청은 비동기 작업으로 전송됩니다.

-   일반적인 인증 설정 및 패턴에 대한 지원

-   전송 시 SSL(Secure Sockets Layer) 세부 정보에 대한 액세스

-   고급 앱에 사용자 지정 필터를 포함하는 기능

-   쿠키를 가져오고 설정하고 삭제하는 기능

-   비동기 메서드에서 사용할 수 있는 HTTP 요청 진행 정보

[**Windows.Web.Http.HttpRequestMessage**](https://msdn.microsoft.com/library/windows/apps/dn279617) 클래스는 [**Windows.Web.Http.HttpClient**](https://msdn.microsoft.com/library/windows/apps/dn298639)에서 전송된 HTTP 요청 메시지를 나타냅니다. [**Windows.Web.Http.HttpResponseMessage**](https://msdn.microsoft.com/library/windows/apps/dn279631) 클래스는 HTTP 요청에서 받은 HTTP 응답 메시지를 나타냅니다. HTTP 메시지는 IETF에 의해 [RFC 2616](http://go.microsoft.com/fwlink/p/?linkid=241642)에서 정의됩니다.

[**Windows.Web.Http**](https://msdn.microsoft.com/library/windows/apps/dn279692) 네임스페이스는 HTTP 콘텐츠를 쿠키가 포함된 HTTP 엔터티 본문 및 헤더로 나타냅니다. HTTP 콘텐츠를 HTTP 요청 또는 HTTP 응답에 연결할 수 있습니다. **Windows.Web.Http** 네임스페이스는 HTTP 컨텐츠를 나타내는 여러 클래스를 제공합니다.

-   [**HttpBufferContent**](https://msdn.microsoft.com/library/windows/apps/dn298625). 콘텐츠를 버퍼로
-   [**HttpFormUrlEncodedContent**](https://msdn.microsoft.com/library/windows/apps/dn298685). 콘텐츠를 **application/x-www-form-urlencoded** MIME 형식으로 인코드된 이름/값 튜플로
-   [**HttpMultipartContent**](https://msdn.microsoft.com/library/windows/apps/dn298708). **multipart/\*** MIME 형식의 콘텐츠
-   [**HttpMultipartFormDataContent**](https://msdn.microsoft.com/library/windows/apps/dn279596). **multipart/form-data** MIME 형식으로 인코딩된 콘텐츠
-   [**HttpStreamContent**](https://msdn.microsoft.com/library/windows/apps/dn279649). 콘텐츠를 스트림으로(내부 형식은 HTTP GET 메서드가 데이터를 받고 HTTP POST 메서드가 데이터를 업로드하는 데 사용됨)
-   [**HttpStringContent**](https://msdn.microsoft.com/library/windows/apps/dn279661). 콘텐츠를 문자열로
-   [**IHttpContent**](https://msdn.microsoft.com/library/windows/apps/dn279684) - 개발자가 해당 콘텐츠 개체를 만들 수 있는 기본 인터페이스

"HTTP를 통해 간단한 GET 요청 보내기" 섹션의 코드 조각에서는 [**HttpStringContent**](https://msdn.microsoft.com/library/windows/apps/dn279661) 클래스를 사용하여 HTTP GET 요청의 HTTP 응답을 문자열로 나타냅니다.

[**Windows.Web.Http.Headers**](https://msdn.microsoft.com/library/windows/apps/dn252713) 네임스페이스에서는 HTTP 머리글 및 쿠키 만들기를 지원하며 이러한 항목이 이후 속성으로 [**HttpRequestMessage**](https://msdn.microsoft.com/library/windows/apps/dn279617) 및 [**HttpResponseMessage**](https://msdn.microsoft.com/library/windows/apps/dn279631) 개체와 연결됩니다.

## <a name="send-a-simple-get-request-over-http"></a>HTTP를 통해 간단한 GET 요청 보내기

이 문서의 앞부분에서 설명한 것처럼 [**Windows.Web.Http**](https://msdn.microsoft.com/library/windows/apps/dn279692) 네임스페이스는 UWP 앱에서 GET 요청을 보낼 수 있도록 합니다. 다음 코드 조각에 GET 요청을 전송 하는 방법을 보여 줍니다 http://www.contoso.com [**Windows.Web.Http.HttpClient**](https://msdn.microsoft.com/library/windows/apps/dn298639) 클래스와 [**Windows.Web.Http.HttpResponseMessage**](https://msdn.microsoft.com/library/windows/apps/dn279631) 클래스를 사용 하 여 GET 요청에서 응답을 읽을 수 있습니다.

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

## <a name="exceptions-in-windowswebhttp"></a>Windows.Web.Http의 예외

잘못된 URI(Uniform Resource Identifier) 문자열이 [**Windows.Foundation.Uri**](https://msdn.microsoft.com/library/windows/apps/br225998) 개체에 대한 생성자에 전달되면 예외가 발생합니다.

**.NET:** [**Windows.Foundation.Uri**](https://msdn.microsoft.com/library/windows/apps/br225998) 유형 C# 및 VB. [**System.Uri**](https://msdn.microsoft.com/library/windows/apps/xaml/system.uri.aspx) 표시 됩니다.

C# 및 Visual Basic에서는 .NET 4.5의 [**System.Uri**](https://msdn.microsoft.com/library/windows/apps/xaml/system.uri.aspx) 클래스와 [**System.Uri.TryCreate**](https://msdn.microsoft.com/library/windows/apps/xaml/system.uri.trycreate.aspx) 메서드 중 하나를 통해 URI가 생성되기 전에 사용자로부터 받은 문자열을 테스트하여 이 오류를 방지할 수 있습니다.

C++에는 URI에 대한 문자열을 시도 및 구문 분석할 메서드가 없습니다. 앱이 [**Windows.Foundation.Uri**](https://msdn.microsoft.com/library/windows/apps/br225998)에 대해 사용자 입력을 받으면 생성자는 try/catch 블록에 있게 됩니다. 예외가 발생하면 앱에서 사용자에게 알리고 새 호스트 이름을 요청할 수 있습니다.

[**Windows.Web.Http**](https://msdn.microsoft.com/library/windows/apps/dn279692)에는 편의 기능이 부족합니다. 따라서 이 네임스페이스에서 [**HttpClient**](https://msdn.microsoft.com/library/windows/apps/dn298639) 및 다른 클래스를 사용하는 앱은 **HRESULT** 값을 사용해야 합니다.

앱에서 사용 하 여 C#, VB.NET, [System.Exception](http://msdn.microsoft.com/library/system.exception.aspx) .NET Framework4.5 나타냅니다 오류가 앱 실행 중 예외가 발생 합니다. [System.Exception.HResult](http://msdn.microsoft.com/library/system.exception.hresult.aspx) 속성은 특정 예외에 할당된 **HRESULT**를 반환합니다. [System.Exception.Message](http://msdn.microsoft.com/library/system.exception.message.aspx) 속성은 예외를 설명하는 메시지를 반환합니다. 가능한 **HRESULT** 값은 *Winerror.h* 헤더 파일에 나열되어 있습니다. 앱은 특정 **HRESULT** 값을 필터링하여 예외의 원인에 따라 앱 동작을 수정할 수 있습니다.

관리되는 C++을 사용하는 앱에서 [Platform::Exception](http://msdn.microsoft.com/library/windows/apps/hh755825.aspx)은 앱 실행 중 예외가 발생하는 경우의 오류를 나타냅니다. [Platform::Exception::HResult](http://msdn.microsoft.com/library/windows/apps/hh763371.aspx) 속성은 특정 예외에 할당된 **HRESULT**를 반환합니다. [Platform::Exception::Message](http://msdn.microsoft.com/library/windows/apps/hh763375.aspx) 속성은 **HRESULT** 값과 연결된 시스템 제공 문자열을 반환합니다. 가능한 **HRESULT** 값은 *Winerror.h* 헤더 파일에 나열되어 있습니다. 앱은 특정 **HRESULT** 값을 필터링하여 예외의 원인에 따라 앱 동작을 수정할 수 있습니다.

대부분의 매개 변수 유효성 검사 오류에서 반환되는 **HRESULT**는 **E\_INVALIDARG**입니다. 일부 잘못된 메서드 호출의 경우 반환되는 **HRESULT**는 **E\_ILLEGAL\_METHOD\_CALL**입니다.

