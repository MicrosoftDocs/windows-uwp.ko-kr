---
ms.assetid: 066711E0-D5C4-467E-8683-3CC64EDBCC83
title: C# 또는Visual Basic에서 비동기식 API 호출
description: UWP (유니버설 Windows 플랫폼)에는 시간이 오래 걸릴 수 있는 작업을 수행할 때 앱이 응답성을 유지할 수 있도록 하는 많은 비동기 Api가 포함 되어 있습니다.
ms.date: 02/08/2017
ms.topic: article
keywords: 'windows 10, uwp, c #, Visual Basic, 비동기'
ms.localizationpriority: medium
ms.openlocfilehash: 67037395e0505c0fce22da5ed8f5fe62a39340e2
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89155277"
---
# <a name="call-asynchronous-apis-in-c-or-visual-basic"></a>C# 또는Visual Basic에서 비동기식 API 호출



UWP (유니버설 Windows 플랫폼)에는 시간이 오래 걸릴 수 있는 작업을 수행할 때 앱이 응답성을 유지할 수 있도록 하는 많은 비동기 Api가 포함 되어 있습니다. 이 항목에서는 c # 또는 Microsoft Visual Basic에서 UWP의 비동기 메서드를 사용 하는 방법에 대해 설명 합니다.

비동기 Api는 실행을 계속 하기 전에 응용 프로그램이 대량 작업이 완료 될 때까지 기다리지 않도록 합니다. 예를 들어 인터넷에서 정보를 다운로드 하는 앱은 정보가 도착할 때까지 몇 초 정도 걸릴 수 있습니다. 동기 메서드를 사용 하 여 정보를 검색 하는 경우에는 메서드가 반환 될 때까지 앱이 차단 됩니다. 앱은 사용자 상호 작용에 응답 하지 않으며 응답 하지 않는 것 같습니다. 사용자는 불편 해질 수 있습니다. UWP는 비동기 Api를 제공 하 여 긴 작업을 수행할 때 앱이 사용자에 게 응답성을 유지 하도록 도와줍니다.

UWP의 대부분의 비동기 Api에는 동기 대응 항목이 없으므로 UWP (유니버설 Windows 플랫폼) 앱에서 c # 또는 Visual Basic와 함께 비동기 Api를 사용 하는 방법을 이해 해야 합니다. 여기서는 UWP의 비동기 Api를 호출 하는 방법을 보여 줍니다.

## <a name="using-asynchronous-apis"></a>비동기 Api 사용


규칙에 따라 비동기 메서드에는 "Async"로 끝나는 이름이 지정 됩니다. 사용자가 단추를 클릭 하는 경우와 같이 일반적으로 사용자 작업에 대 한 응답으로 비동기 Api를 호출 합니다. 비동기 Api를 사용 하는 가장 간단한 방법 중 하나는 이벤트 처리기에서 비동기 메서드를 호출 하는 것입니다. 여기서는 **wait** 연산자를 예로 사용 합니다.

특정 위치에서 블로그 게시물의 제목을 나열 하는 앱이 있다고 가정 합니다. 앱에는 사용자가 제목을 가져오기 위해 클릭 하는 [**단추가**](/uwp/api/Windows.UI.Xaml.Controls.Button) 있습니다. 제목은 [**TextBlock**](/uwp/api/Windows.UI.Xaml.Controls.TextBlock)에 표시 됩니다. 사용자가 단추를 클릭 하면 블로그의 웹 사이트에서 정보를 기다리는 동안 앱이 응답성을 유지 하는 것이 중요 합니다. 이 응답성을 보장 하기 위해 UWP에서는 SyndicationClient를 제공 하는 비동기 메서드인 RetrieveFeedAsync을 제공 합니다 [**.**](/uwp/api/windows.web.syndication.syndicationclient.retrievefeedasync)

이 예제에서는 비동기 메서드인 [**SyndicationClient RetrieveFeedAsync**](/uwp/api/windows.web.syndication.syndicationclient.retrievefeedasync)를 호출 하 고 결과를 대기 하 여 블로그의 블로그 게시물 목록을 가져옵니다.

> [!div class="tabbedCodeSnippets" data-resources="OutlookServices.Calendar"]
[!code-csharp[Main](./AsyncSnippets/csharp/MainPage.xaml.cs#SnippetDownloadRSS)]
[!code-vb[Main](./AsyncSnippets/vbnet/MainPage.xaml.vb#SnippetDownloadRSS)]

이 예에 대 한 몇 가지 중요 한 사항이 있습니다. 첫 번째 줄에서는 `SyndicationFeed feed = await client.RetrieveFeedAsync(feedUri)` [**RetrieveFeedAsync**](/uwp/api/windows.web.syndication.syndicationclient.retrievefeedasync)비동기 메서드를 호출 하는 **wait** 연산자를 사용 합니다. **Wait** 연산자는 컴파일러에 비동기 메서드를 호출 하는 것으로 간주할 수 있습니다. 이렇게 하면 컴파일러가 추가 작업을 수행 하 게 됩니다. 그런 다음 이벤트 처리기의 선언에 **async**키워드가 포함 됩니다. **Wait** 연산자를 사용 하는 메서드의 메서드 선언에이 키워드를 포함 해야 합니다.

이 항목에서는 컴파일러가 **wait** 연산자를 사용 하 여 수행 하는 작업의 세부 정보를 많이 다루지 않지만 비동기 및 응답성을 위해 앱에서 수행 하는 작업을 살펴보겠습니다. 동기 코드를 사용할 때 발생 하는 상황을 고려 합니다. 예를 들어 동기 인 이라는 메서드가 있다고 가정 `SyndicationClient.RetrieveFeed` 합니다. 이러한 메서드는 없지만이 있다고 가정 합니다. 앱이 대신 줄을 포함 하는 경우에는의 `SyndicationFeed feed = client.RetrieveFeed(feedUri)` `SyndicationFeed feed = await client.RetrieveFeedAsync(feedUri)` 반환 값을 사용할 수 있을 때까지 응용 프로그램 실행이 중지 됩니다 `RetrieveFeed` . 앱이 메서드가 완료 될 때까지 대기 하는 동안 다른 [**Click**](/uwp/api/windows.ui.xaml.controls.primitives.buttonbase.click) 이벤트와 같은 다른 이벤트에 응답할 수 없습니다. 즉,가 반환 될 때까지 앱이 차단 됩니다 `RetrieveFeed` .

하지만를 호출 하 `client.RetrieveFeedAsync` 는 경우 메서드는 검색을 시작 하 고 즉시를 반환 합니다. [**RetrieveFeedAsync**](/uwp/api/windows.web.syndication.syndicationclient.retrievefeedasync)와 함께 **wait** 를 사용 하는 경우 앱은 일시적으로 이벤트 처리기를 종료 합니다. 그런 다음 **RetrieveFeedAsync** 가 비동기적으로 실행 되는 동안 다른 이벤트를 처리할 수 있습니다. 따라서 앱이 사용자에게 응답 가능한 상태를 유지하게 됩니다. **RetrieveFeedAsync** 가 완료 되 고 [**SyndicationFeed**](/uwp/api/Windows.Web.Syndication.SyndicationFeed) 를 사용할 수 있게 되 면 응용 프로그램은 기본적으로 이벤트 처리기를 후 하 `SyndicationFeed feed = await client.RetrieveFeedAsync(feedUri)` 고 나머지 메서드는 종료 합니다.

**Wait** 연산자를 사용 하는 것은 코드가 허수 메서드를 사용 했을 때 코드의 모양과 크게 다르지 않는다는 점입니다 `RetrieveFeed` . **Wait** 연산자 없이 c # 또는 Visual Basic로 비동기 코드를 작성 하는 방법이 있지만 결과 코드는 비동기식으로 실행 되는 메커니즘을 강조 하는 경향이 있습니다. 이렇게 하면 비동기 코드를 작성 하 고 이해 하기 어렵고 유지 관리 하기 어려울 수 있습니다. **Wait** 연산자를 사용 하 여 코드를 복잡 하 게 만들지 않고 비동기 앱의 이점을 얻을 수 있습니다.

## <a name="return-types-and-results-of-asynchronous-apis"></a>비동기 Api의 반환 형식 및 결과


[**RetrieveFeedAsync**](/uwp/api/windows.web.syndication.syndicationclient.retrievefeedasync)에 대 한 링크를 팔 로우 하는 경우 **RetrieveFeedAsync** 의 반환 형식이 [**SyndicationFeed**](/uwp/api/Windows.Web.Syndication.SyndicationFeed)가 아닌 것을 알 수 있습니다. 대신 반환 형식은 `IAsyncOperationWithProgress<SyndicationFeed, RetrievalProgress>` 입니다. 비동기 API는 원시 구문에서 볼 때 그 안에 결과를 포함 하는 개체를 반환 합니다. 일반적이 고 때로는 비동기 메서드를 대기 가능 하는 것으로 생각 하는 것이 일반적 이지만 **wait** 연산자는 메서드가 아닌 메서드의 반환 값에 대해 실제로 작동 합니다. **Wait** 연산자를 적용 하면 메서드에서 반환 하는 개체에 대해 **getresult** 를 호출한 결과가 반환 됩니다. 이 예에서 **SyndicationFeed** 은 **RetrieveFeedAsync ()** 의 결과입니다.

비동기 메서드를 사용 하는 경우 시그니처를 검사 하 여 메서드에서 반환 된 값을 대기 한 후에 다시 가져올 항목을 확인할 수 있습니다. UWP의 모든 비동기 Api는 다음 형식 중 하나를 반환 합니다.

-   [**Iasyncoperation<tresult> &lt; TResult&gt;**](/uwp/api/Windows.Foundation.IAsyncOperation_TResult_)
-   [**IAsyncOperationWithProgress &lt; TResult, TProgress&gt;**](/uwp/api/Windows.Foundation.IAsyncOperationWithProgress_TResult_TProgress_)
-   [**IAsyncAction**](/uwp/api/windows.foundation.iasyncaction)
-   [**IAsyncActionWithProgress &lt; tprogress&gt;**](/uwp/api/Windows.Foundation.IAsyncActionWithProgress_TProgress_)

비동기 메서드의 결과 형식은 `      TResult` 형식 매개 변수와 같습니다. 가 없는 형식에는 `TResult` 결과가 없습니다. 결과를 **void**로 간주할 수 있습니다. Visual Basic에서 [Sub](/dotnet/articles/visual-basic/programming-guide/language-features/procedures/sub-procedures) 프로시저는 **void** 반환 형식을 사용 하는 메서드와 동일 합니다.

다음 표에서는 비동기 메서드의 예제를 제공 하 고 각각의 반환 형식 및 결과 형식을 나열 합니다.

| 비동기 메서드                                                                           | 반환 형식                                                                                                                                        | 결과 형식                                       |
|-----------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------|---------------------------------------------------|
| [**SyndicationClient. RetrieveFeedAsync**](/uwp/api/windows.web.syndication.syndicationclient.retrievefeedasync)     | [**IAsyncOperationWithProgress &lt; SyndicationFeed, RetrievalProgress&gt;**](/uwp/api/Windows.Foundation.IAsyncOperationWithProgress_TResult_TProgress_)                                 | [**SyndicationFeed**](/uwp/api/Windows.Web.Syndication.SyndicationFeed) |
| [**FileOpenPicker. PickSingleFileAsync**](/uwp/api/windows.storage.pickers.fileopenpicker.picksinglefileasync) | [**Iasyncoperation<tresult> &lt; StorageFile&gt;**](/uwp/api/Windows.Foundation.IAsyncOperation_TResult_)                                                                                | [**StorageFile**](/uwp/api/Windows.Storage.StorageFile)          |
| [**XmlDocument SaveToFileAsync**](/uwp/api/windows.data.xml.dom.xmldocument.savetofileasync)                 | [**IAsyncAction**](/uwp/api/windows.foundation.iasyncaction)                                                                                                           | **void**                                          |
| [**InkStrokeContainer. LoadAsync**](/uwp/api/windows.ui.input.inking.inkstrokecontainer.loadasync)               | [**IAsyncActionWithProgress &lt; UInt64&gt;**](/uwp/api/Windows.Foundation.IAsyncActionWithProgress_TProgress_)                                                                   | **void**                                          |
| [**DataReader. LoadAsync**](/uwp/api/windows.storage.streams.datareader.loadasync)                            | **Iasyncoperation<tresult> &lt; UInt32 &gt; ** 를 구현 하는 사용자 지정 결과 클래스인 [**datareaderloadoperation**](/uwp/api/Windows.Storage.Streams.DataReaderLoadOperation) | [**UInt32**](/uwp/api/Windows.Foundation.IAsyncOperation_TResult_)                     |

 

[**UWP 앱 용 .net**](https://dotnet.microsoft.com/apps/desktop) 에 정의 된 비동기 메서드에는 반환 형식 [**작업**](/dotnet/api/system.threading.tasks.task) 또는 [**작업 &lt; TResult &gt; **](/dotnet/api/system.threading.tasks.task-1)가 있습니다. **작업** 을 반환 하는 메서드는 [**iasyncaction**](/uwp/api/windows.foundation.iasyncaction)을 반환 하는 UWP의 비동기 메서드와 유사 합니다. 각 경우에서 비동기 메서드의 결과는 **void**입니다. 반환 형식 **작업 &lt; Tresult &gt; ** 은 [**iasyncoperation<tresult> &lt; TResult &gt; **](/uwp/api/Windows.Foundation.IAsyncOperation_TResult_) 와 비슷하며, 작업을 실행 하는 경우 비동기 메서드의 결과가 `TResult` 형식 매개 변수와 동일한 형식입니다. **UWP 앱 및 작업에 .net** 을 사용 하는 방법에 대 한 자세한 내용은 [Windows 런타임 앱에 대 한 .net 개요](/previous-versions/windows/apps/br230302(v=vs.140))를 참조 하세요.

## <a name="handling-errors"></a>오류 처리


**Wait** 연산자를 사용 하 여 비동기 메서드에서 결과를 검색 하는 경우에는 동기 메서드와 마찬가지로 **try/catch** 블록을 사용 하 여 비동기 메서드에서 발생 하는 오류를 처리할 수 있습니다. 이전 예제에서는 **RetrieveFeedAsync** **메서드를 래핑하고** **try/catch** 블록에서 작업을 대기 하 여 예외가 throw 될 때 오류를 처리 합니다.

비동기 메서드가 다른 비동기 메서드를 호출할 때 예외가 발생 하는 비동기 메서드는 외부 메서드에 전파 됩니다. 즉, 가장 바깥쪽 메서드에 **try/catch** 블록을 추가 하 여 중첩 된 비동기 메서드에 대 한 오류를 catch 할 수 있습니다. 이는 동기 메서드에 대 한 예외를 catch 하는 방법과 비슷합니다. 그러나 **catch** 블록에는 **wait** 를 사용할 수 없습니다.

**팁**    Microsoft Visual Studio 2005의 c # 부터는 **catch** 블록에서 **wait** 를 사용할 수 있습니다.

## <a name="summary-and-next-steps"></a>요약 및 다음 단계

여기에 표시 된 비동기 메서드를 호출 하는 패턴은 이벤트 처리기에서 비동기 Api를 호출할 때 사용할 수 있는 가장 간단한 방법입니다. **Void** 를 반환 하는 재정의 된 메서드 또는 Visual Basic의 **Sub** 에서 비동기 메서드를 호출 하는 경우에도이 패턴을 사용할 수 있습니다.

UWP에서 비동기 메서드를 사용할 때는 다음을 기억해 야 합니다.

-   규칙에 따라 비동기 메서드에는 "Async"로 끝나는 이름이 지정 됩니다.
-   **Wait** 연산자를 사용 하는 모든 메서드에는 **async** 키워드로 표시 된 선언이 있어야 합니다.
-   앱이 **wait** 연산자를 찾으면 비동기 메서드가 실행 되는 동안 앱은 사용자 상호 작용에 응답 하는 상태로 유지 됩니다.
-   비동기 메서드에서 반환 된 값을 대기 하면 결과가 포함 된 개체를 반환 합니다. 대부분의 경우 반환 값에 포함 된 결과는 반환 값 자체가 아니라 유용 합니다. 비동기 메서드의 반환 형식을 살펴보면 결과 내에 포함 된 값의 형식을 찾을 수 있습니다.
-   비동기 Api와 **비동기** 패턴을 사용 하는 것은 종종 응용 프로그램의 응답성을 향상 시키는 방법입니다.

이 항목의 예제에서는 다음과 같은 텍스트를 출력 합니다.

``` syntax
Windows Experience Blog
PC Snapshot: Sony VAIO Y, 8/9/2011 10:26:56 AM -07:00
Tech Tuesday Live Twitter #Chat: Too Much Tech #win7tech, 8/8/2011 12:48:26 PM -07:00
Windows 7 themes: what’s new and what’s popular!, 8/4/2011 11:56:28 AM -07:00
PC Snapshot: Toshiba Satellite A665 3D, 8/2/2011 8:59:15 AM -07:00
Time for new school supplies? Find back-to-school deals on Windows 7 PCs and Office 2010, 8/1/2011 2:14:40 PM -07:00
Best PCs for blogging (or working) on the go, 8/1/2011 10:08:14 AM -07:00
Tech Tuesday – Blogging Tips and Tricks–#win7tech, 8/1/2011 9:35:54 AM -07:00
PC Snapshot: Lenovo IdeaPad U460, 7/29/2011 9:23:05 AM -07:00
GIVEAWAY: Survive BlogHer with a Sony VAIO SA and a Samsung Focus, 7/28/2011 7:27:14 AM -07:00
3 Ways to Stay Cool This Summer, 7/26/2011 4:58:23 PM -07:00
Getting RAW support in Photo Gallery & Windows 7 (…and a contest!), 7/26/2011 10:40:51 AM -07:00
Tech Tuesdays Live Twitter Chats: Photography Tips, Tricks and Essentials, 7/25/2011 12:33:06 PM -07:00
3 Tips to Go Green With Your PC, 7/22/2011 9:19:43 AM -07:00
How to: Buy a Green PC, 7/22/2011 9:13:22 AM -07:00
Windows 7 themes: the distinctive artwork of Cheng Ling, 7/20/2011 9:53:07 AM -07:00
```