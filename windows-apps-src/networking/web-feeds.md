---
author: stevewhims
description: Windows.Web.Syndication 네임스페이스의 기능을 사용하여 RSS 및 Atom 표준에 따라 생성된 신디케이티드 피드로 인기 있는 최신 웹 콘텐츠를 검색하거나 만듭니다.
title: RSS/Atom 피드
ms.assetid: B196E19B-4610-4EFA-8FDF-AF9B10D78843
ms.author: stwhi
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 309dd2aedb2195362652da93c13648d07e5ea9f8
ms.sourcegitcommit: e2fca6c79f31e521ba76f7ecf343cf8f278e6a15
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/15/2018
ms.locfileid: "6977034"
---
# <a name="rssatom-feeds"></a>RSS/Atom 피드


**중요 API**

-   [**Windows.Data.Xml.Dom**](https://msdn.microsoft.com/library/windows/apps/br240819)
-   [**Windows.Web.AtomPub**](https://msdn.microsoft.com/library/windows/apps/br210609)
-   [**Windows.Web.Syndication**](https://msdn.microsoft.com/library/windows/apps/br243632)

[**Windows.Web.Syndication**](https://msdn.microsoft.com/library/windows/apps/br243632) 네임스페이스의 기능을 사용하여 RSS 및 Atom 표준에 따라 생성된 신디케이티드 피드로 인기 있는 최신 웹 콘텐츠를 검색하거나 만듭니다.

## <a name="what-is-a-feed"></a>피드란?

웹 피드는 텍스트, 링크 및 이미지로 구성된 임의 개수의 개별 항목이 포함되어 있는 문서입니다. 피드의 업데이트는 최신 콘텐츠를 웹 전체에 홍보하는 데 사용되는 새 항목 형식으로 표시됩니다. 콘텐츠 소비자는 피드 뷰어 앱을 사용하여 여러 개별 콘텐츠 만든 이의 피드를 집계하고 모니터링하여 최신 콘텐츠에 빠르고 편리하게 액세스할 수 있습니다.

## <a name="which-feed-format-standards-are-supported"></a>지원되는 피드 형식 표준

UWP(유니버설 Windows 플랫폼)는 0.91에서 RSS 2.0까지의 RSS 형식 표준과 0.3에서 1.0까지의 Atom 표준에 대한 피드 검색을 지원합니다. [**Windows.Web.Syndication**](https://msdn.microsoft.com/library/windows/apps/br243632) 네임스페이스의 클래스는 RSS 및 Atom 요소를 모두 나타낼 수 있는 피드 및 피드 항목을 정의할 수 있습니다.

또한 Atom 1.0 및 RSS 2.0 형식은 모두 공식 사양에 정의되지 않은 요소 또는 특성을 피드 문서에 포함하도록 허용합니다. 시간이 지나면서 이러한 사용자 지정 요소 및 특성은 GData 및 OData 등의 다른 웹 서비스 데이터 형식에서 사용하는 도메인별 정보를 정의하는 방법이 되었습니다. 이 추가된 기능을 지원하기 위해 [**SyndicationNode**](https://msdn.microsoft.com/library/windows/apps/br243585) 클래스는 일반 XML 요소를 나타냅니다. [**Windows.Data.Xml.Dom**](https://msdn.microsoft.com/library/windows/apps/br240819) 네임스페이스의 클래스와 함께 **SyndicationNode**를 사용하면 앱이 특성, 확장 및 앱에 포함될 수 있는 모든 콘텐츠에 액세스할 수 있습니다.

신디케이티드 콘텐츠 게시물의 경우 Atom 게시물 프로토콜의 UWP 구현([**Windows.Web.AtomPub**](https://msdn.microsoft.com/library/windows/apps/br210609))은 Atom 및 Atom 게시물 표준에 따라서만 피드 콘텐츠 작업을 지원합니다.

## <a name="using-syndicated-content-with-network-isolation"></a>네트워크 격리와 함께 신디케이티드 콘텐츠 사용

개발자는 UWP의 네트워크 격리 기능을 사용하여 UWP 앱의 네트워크 액세스를 제어하고 제한할 수 있습니다. 일부 앱은 네트워크에 대한 액세스 권한이 필요하지 않을 수 있습니다. 그러나 네트워크에 대한 액세스 권한이 필요한 앱의 경우 UWP에서 적절한 접근 권한 값을 선택하여 사용할 수 있는 다양한 수준의 네트워크 액세스 권한을 제공합니다.

개발자는 네트워크 격리를 통해 각 앱에 대해 필요한 네트워크 액세스 범위를 정의할 수 있습니다. 적절한 범위가 정의되지 않은 앱은 지정된 유형의 네트워크와 특정 유형의 네트워크 요청(클라이언트가 시작한 아웃바운드 요청 또는 원치 않는 인바운드 요청과 클라이언트가 시작한 아웃바운드 요청 모두)에 액세스할 수 없습니다. 네트워크 격리를 설정하고 적용할 수 있게 되면 앱이 손상되었을 경우 명시적으로 액세스가 허용된 네트워크에만 액세스하도록 할 수 있습니다. 이 경우 다른 응용 프로그램 및 Windows에 영향을 미치는 범위가 현저히 줄어듭니다.

네트워크 격리는 네트워크에 액세스하려고 하는 [**Windows.Web.Syndication**](https://msdn.microsoft.com/library/windows/apps/br243632) 및 [**Windows.Web.AtomPub**](https://msdn.microsoft.com/library/windows/apps/br210609) 네임스페이스의 모든 클래스 요소에 영향을 미칩니다. Windows에서는 적극적으로 네트워크 격리가 적용됩니다. 적절한 네트워크 접근 권한 값을 사용하지 않은 경우 결과적으로 네트워크에 액세스하게 되는 **Windows.Web.Syndication** 또는 **Windows.Web.AtomPub** 네임스페이스의 클래스 요소 호출이 네트워크 격리로 인해 실패할 수 있습니다.

앱의 네트워크 접근 권한 값은 앱을 빌드할 때 앱 매니페스트에 구성됩니다. 네트워크 접근 권한 값은 일반적으로 앱을 개발할 때 Microsoft Visual Studio2015를 사용 하 여 추가 됩니다. 텍스트 편집기를 사용하여 앱 매니페스트 파일에서 수동으로 네트워크 접근 권한 값을 설정할 수도 있습니다.

네트워크 격리 및 네트워킹 기능에 대한 자세한 내용은 [네트워킹기본 사항](networking-basics.md) 항목의 "기능" 섹션을 참조하세요.

## <a name="how-to-access-a-web-feed"></a>웹 피드에 액세스하는 방법

이 섹션에서는 C# 또는 JavaScript로 작성한 UWP 앱에서 [**Windows.Web.Syndication**](https://msdn.microsoft.com/library/windows/apps/br243632) 네임스페이스의 클래스를 사용하여 웹 피드를 검색하고 표시하는 방법을 보여 줍니다.

**필수 조건**

UWP 앱에서 네트워크에 대비하려면 프로젝트 **Package.appxmanifest** 파일에 필요한 네트워크 접근 권한 값을 설정해야 합니다. 앱이 인터넷의 원격 서비스에 클라이언트로 연결해야 하는 경우 **인터넷(클라이언트)** 접근 권한 값이 필요합니다. 자세한 내용은 [네트워킹 기본 사항](networking-basics.md) 항목의 "기능" 섹션을 참조하세요.

**웹 피드에서 신디케이티드 콘텐츠 검색**

이제 피드를 검색한 다음 피드에 포함된 개별 항목을 표시하는 방법을 보여 주는 일부 코드를 검토합니다. 요청을 구성하여 보내려면 먼저 작업 중 사용할 몇 가지 변수를 정의하고 피드를 검색 및 표시할 때 사용할 메서드와 속성을 정의하는 [**SyndicationClient**](https://msdn.microsoft.com/library/windows/apps/br243456)의 인스턴스를 초기화합니다.

생성자에 전달된 *uriString*이 유효한 URI가 아니면 [**Uri**](https://msdn.microsoft.com/library/windows/apps/br226017) 생성자에서 예외가 발생합니다. 따라서 try/catch 블록을 사용하여 *uriString*의 유효성을 검사합니다.

> [!div class="tabbedCodeSnippets"]
```csharp
Windows.Web.Syndication.SyndicationClient client = new Windows.Web.Syndication.SyndicationClient();
Windows.Web.Syndication.SyndicationFeed feed;
// The URI is validated by catching exceptions thrown by the Uri constructor.
Uri uri = null;
// Use your own uriString for the feed you are connecting to.
string uriString = "";
try
{
    uri = new Uri(uriString);
}
catch (Exception ex)
{
    // Handle the invalid URI here.
}
```
```javascript
var currentFeed = null;
var currentItemIndex = 0;
var client = new Windows.Web.Syndication.SyndicationClient();
// The URI is validated by catching exceptions thrown by the Uri constructor.
var uri = null;
try {
    uri = new Windows.Foundation.Uri(uriString);
} catch (error) {
    WinJS.log && WinJS.log("Error: Invalid URI");
    return;
}
```

다음에는 필요한 서버 자격 증명([**ServerCredential**](https://msdn.microsoft.com/library/windows/apps/br243461) 속성), 프록시 자격 증명([**ProxyCredential**](https://msdn.microsoft.com/library/windows/apps/br243459) 속성) 및 HTTP 헤더([**SetRequestHeader**](https://msdn.microsoft.com/library/windows/apps/br243462) 메서드)를 설정하여 요청을 구성합니다. 기본 요청 매개 변수를 구성하면 앱에서 제공한 피드 URI 문자열을 사용하여 유효한 [**Uri**](https://msdn.microsoft.com/library/windows/apps/br226017) 개체가 만들어집니다. 그런 다음 피드를 요청하기 위해 **Uri** 개체가 [**RetrieveFeedAsync**](https://msdn.microsoft.com/library/windows/apps/br243460) 함수에 전달됩니다.

필요한 피드 콘텐츠가 반환된 경우 예제 코드는 다음에 정의하는 **displayCurrentItem**을 호출하여 각 피드 항목을 반복하여 UI를 통해 항목과 해당 콘텐츠를 목록으로 표시합니다.

따라서 대부분의 비동기 네트워크 메서드를 호출할 때 예외를 처리하는 코드를 작성해야 합니다. 예외 처리기는 예외의 원인에 대해 보다 자세한 정보를 검색하므로 오류를 더 잘 이해하고 적절한 의사 결정을 내릴 수 있습니다.

HTTP 서버에 연결할 수 없거나 [**Uri**](https://msdn.microsoft.com/library/windows/apps/br243460) 개체가 유효한 AtomPub 또는 RSS 피드를 가리키지 않는 경우 [**RetrieveFeedAsync**](https://msdn.microsoft.com/library/windows/apps/br226017) 메서드에서 예외가 발생합니다. JavaScript 샘플 코드는 **onError** 함수를 사용하여 예외를 catch하고, 오류가 발생할 경우 예외에 대한 자세한 정보를 출력합니다.

> [!div class="tabbedCodeSnippets"]
```csharp
try
{
    // Although most HTTP servers do not require User-Agent header, 
    // others will reject the request or return a different response if this header is missing.
    // Use the setRequestHeader() method to add custom headers.
    client.SetRequestHeader("User-Agent", "Mozilla/5.0 (compatible; MSIE 10.0; Windows NT 6.2; WOW64; Trident/6.0)");
    feed = await client.RetrieveFeedAsync(uri);
    // Retrieve the title of the feed and store it in a string.
    string title = feed.Title.Text;
    // Iterate through each feed item.
    foreach (Windows.Web.Syndication.SyndicationItem item in feed.Items)
    {
        displayCurrentItem(item);
    }
}
catch (Exception ex)
{
    // Handle the exception here.
}
```
```javascript
function onError(err) {
    WinJS.log && WinJS.log(err, "sample", "error");
    // Match error number with a ErrorStatus value.
    // Use Windows.Web.WebErrorStatus.getStatus() to retrieve HTTP error status codes.
    var errorStatus = Windows.Web.Syndication.SyndicationError.getStatus(err.number);
    if (errorStatus === Windows.Web.Syndication.SyndicationErrorStatus.invalidXml) {
        displayLog("An invalid XML exception was thrown. Please make sure to use a URI that points to a RSS or Atom feed.");
    }
}
// Retrieve and display feed at given feed address.
function retreiveFeed(uri) {
    // Although most HTTP servers do not require User-Agent header, 
    // others will reject the request or return a different response if this header is missing.
    // Use the setRequestHeader() method to add custom headers.
    client.setRequestHeader("User-Agent", "Mozilla/5.0 (compatible; MSIE 10.0; Windows NT 6.2; WOW64; Trident/6.0)");
    client.retrieveFeedAsync(uri).done(function (feed) {
        currentFeed = feed;
        WinJS.log && WinJS.log("Feed download complete.", "sample", "status");
        var title = "(no title)";
        if (currentFeed.title) {
            title = currentFeed.title.text;
        }
        document.getElementById("CurrentFeedTitle").innerText = title;
        currentItemIndex = 0;
        if (currentFeed.items.size > 0) {
            displayCurrentItem();
        }
        // List the items.
        displayLog("Items: " + currentFeed.items.size);
     }, onError);
}
```

이전 단계에서 [**RetrieveFeedAsync**](https://msdn.microsoft.com/library/windows/apps/br243460)는 요청된 피드 콘텐츠를 반환했고 예제 코드에서는 사용 가능한 피드 항목을 반복했습니다. 이러한 각 항목은 관련 신디케이션 표준(RSS 또는 Atom)에서 제공하는 모든 항목 속성과 콘텐츠를 포함하는 [**SyndicationItem**](https://msdn.microsoft.com/library/windows/apps/br243533) 개체를 사용하여 나타냅니다. 다음 예제에서는 각 항목을 처리하고 다양한 이름의 UI 요소를 통해 항목의 콘텐츠를 표시하는 **displayCurrentItem** 함수를 관찰합니다.

> [!div class="tabbedCodeSnippets"]
```csharp
private void displayCurrentItem(Windows.Web.Syndication.SyndicationItem item)
{
    string itemTitle = item.Title == null ? "No title" : item.Title.Text;
    string itemLink = item.Links == null ? "No link" : item.Links.FirstOrDefault().ToString();
    string itemContent = item.Content == null ? "No content" : item.Content.Text;
    //displayCurrentItem is continued below.
```
```javascript
function displayCurrentItem() {
    var item = currentFeed.items[currentItemIndex];
    // Display item number.
    document.getElementById("Index").innerText = (currentItemIndex + 1) + " of " + currentFeed.items.size;
    // Display title.
    var title = "(no title)";
    if (item.title) {
        title = item.title.text;
    }
    document.getElementById("ItemTitle").innerText = title;
    // Display the main link.
    var link = "";
    if (item.links.size > 0) {
        link = item.links[0].uri.absoluteUri;
    }
    var link = document.getElementById("Link");
    link.innerText = link;
    link.href = link;
    // Display the body as HTML.
    var content = "(no content)";
    if (item.content) {
        content = item.content.text;
    }
    else if (item.summary) {
        content = item.summary.text;
    }
    document.getElementById("WebView").innerHTML = window.toStaticHTML(content);
                //displayCurrentItem is continued below.
```

앞에서 설명한 대로 [**SyndicationItem**](https://msdn.microsoft.com/library/windows/apps/br243533) 개체로 표시되는 콘텐츠 유형은 피드를 게시하는 데 사용된 피드 표준(RSS 또는 Atom)에 따라 달라집니다. 예를 들어 Atom 피드는 [**Contributors**](https://msdn.microsoft.com/library/windows/apps/br243540) 목록을 제공할 수 있지만 RSS 피드는 제공할 수 없습니다. 그러나 한쪽 표준에서 지원되지 않는 피드 항목에 포함된 확장 요소(예: 더블린 코어 확장 요소)는 다음 예제 코드에 나온 대로 [**SyndicationItem.ElementExtensions**](https://msdn.microsoft.com/library/windows/apps/br243543) 속성을 사용하여 액세스한 다음 표시할 수 있습니다.

> [!div class="tabbedCodeSnippets"]
```csharp
    //displayCurrentItem continued.
    string extensions = "";
    foreach (Windows.Web.Syndication.SyndicationNode node in item.ElementExtensions)
    {
        string nodeName = node.NodeName;
        string nodeNamespace = node.NodeNamespace;
        string nodeValue = node.NodeValue;
        extensions += nodeName + "\n" + nodeNamespace + "\n" + nodeValue + "\n";
    }
    this.listView.Items.Add(itemTitle + "\n" + itemLink + "\n" + itemContent + "\n" + extensions);
}
```
```javascript
    // displayCurrentItem function continued.
    var bindableNodes = [];
    for (var i = 0; i < item.elementExtensions.size; i++) {
        var bindableNode = {
            nodeName: item.elementExtensions[i].nodeName,
             nodeNamespace: item.elementExtensions[i].nodeNamespace,
             nodeValue: item.elementExtensions[i].nodeValue,
        };
        bindableNodes.push(bindableNode);
    }
    var dataList = new WinJS.Binding.List(bindableNodes);
    var listView = document.getElementById("extensionsListView").winControl;
    WinJS.UI.setOptions(listView, {
        itemDataSource: dataList.dataSource
    });
}
```
