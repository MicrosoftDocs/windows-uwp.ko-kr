---
ms.assetid: 066711E0-D5C4-467E-8683-3CC64EDBCC83
title: C# 또는 Visual Basic에서 비동기식 API 호출
description: UWP(유니버설 Windows 플랫폼)에는 여러 비동기 API가 포함되어 있으므로 앱이 장시간 작업을 수행하는 동안에도 응답 가능한 상태를 유지할 수 있습니다.
---
# C# 또는 Visual Basic에서 비동기식 API 호출

\[ Windows 10의 UWP 앱에 맞게 업데이트되었습니다. Windows 8.x 문서는 [보관](http://go.microsoft.com/fwlink/p/?linkid=619132)을 참조하세요. \]


UWP(유니버설 Windows 플랫폼)에는 여러 비동기 API가 포함되어 있으므로 앱이 장시간 작업을 수행하는 동안에도 응답 가능한 상태를 유지할 수 있습니다. 이 항목에서는 C# 또는 Microsoft Visual Basic에서 UWP의 비동기 메서드를 사용하는 방법을 설명합니다.

비동기 API를 사용하면 앱에서 큰 작업이 완료될 때까지 기다릴 필요 없이 계속 실행될 수 있습니다. 예를 들어 인터넷에서 정보를 다운로드하는 앱은 정보가 도착할 때까지 몇 초간 기다려야 할 수 있습니다. 동기 메서드를 사용하여 정보를 검색할 경우 메서드가 반환할 때까지 앱이 차단됩니다. 앱은 사용자 조작에 반응하지 않으며, 사용자는 앱이 반응하지 않는 것처럼 보이므로 불만을 느낄 수 있습니다. UWP는 비동기 API를 제공하여 앱이 오랜 시간이 걸리는 작업을 수행할 경우 사용자에게 응답 가능한 상태를 유지하도록 해 줍니다.

UWP에서 제공하는 대부분의 비동기 API는 그에 상응하는 동기 API가 없습니다. 그러므로 UWP(유니버설 Windows 플랫폼) 앱에서 C# 또는 Visual Basic의 비동기 API를 사용하는 방법을 잘 알고 있어야 합니다. 여기에서는 UWP의 비동기 API를 호출하는 방법을 보여 줍니다.

## 비동기 API 사용


규칙에 따라 비동기 메서드에는 "Async"로 끝나는 이름이 지정됩니다. 일반적으로 비동기 API는 사용자가 단추를 클릭할 때 등의 사용자 작업에 대한 응답으로 호출합니다. 비동기 API를 사용하는 가장 간단한 방법 중 하나는 이벤트 처리기에서 비동기 메서드를 호출하는 것입니다. 여기에서는 예로 **await** 연산자를 사용합니다.

특정 위치에 있는 블로그 게시물의 제목을 나열하는 앱이 있다고 가정해 보겠습니다. 앱에는 사용자가 제목을 표시하기 위해 클릭하는 [**Button**](https://msdn.microsoft.com/library/windows/apps/BR209265)이 있습니다. 제목은 [**TextBlock**](https://msdn.microsoft.com/library/windows/apps/BR209652)에 표시됩니다. 사용자가 단추를 클릭할 때 앱은 블로그의 웹 사이트에서 정보를 가져올 때까지 기다리는 동안 응답 가능한 상태를 유지하는 것이 중요합니다. 이 응답성을 위해 UWP는 피드를 다운로드할 때 비동기 메서드인 [**SyndicationClient.RetrieveFeedAsync**](https://msdn.microsoft.com/library/windows/apps/BR243460)를 제공합니다.

다음 예제에서는 비동기 메서드인 [**SyndicationClient.RetrieveFeedAsync**](https://msdn.microsoft.com/library/windows/apps/BR243460)를 호출하고 결과를 기다려 블로그에서 블로그 게시물 목록을 가져옵니다.

> [!div class="tabbedCodeSnippets" data-resources="OutlookServices.Calendar"]
[!code-csharp[Main](./AsyncSnippets/csharp/MainPage.xaml.cs#SnippetDownloadRSS)]
[!code-vb[Main](./AsyncSnippets/vbnet/MainPage.xaml.vb#SnippetDownloadRSS)]

이 예제에는 눈여겨볼 두 가지 중요한 사항이 있습니다. 먼저 `SyndicationFeed feed = await client.RetrieveFeedAsync(feedUri)` 줄에서는 비동기 메서드 [**RetrieveFeedAsync**](https://msdn.microsoft.com/library/windows/apps/BR243460)를 호출할 때 **await** 연산자를 사용합니다. **await** 연산자는 컴파일러에게 비동기 메서드를 호출한다는 것을 알려 컴파일러가 개발자 대신 일부 추가 작업을 수행하도록 합니다. 다음으로 이벤트 처리기 선언에 키워드 **async**가 포함됩니다. **await** 연산자를 사용하는 메서드의 메서드 선언에 이 키워드를 포함해야 합니다.

이 항목에서는 컴파일러가 **await** 연산자와 함께 수행하는 작업을 자세히 다루지는 않으며, 대신 앱이 비동기 및 응답 상태를 유지하기 위해 수행하는 작업을 살펴보겠습니다. 동기 코드를 사용할 경우에는 어떻게 되는지 살펴보겠습니다. 예를 들어 `SyndicationClient.RetrieveFeed`라는 동기 메서드가 있다고 가정해 보겠습니다. (이런 방법은 없지만 있다고 가정함) 앱에 `SyndicationFeed feed = await client.RetrieveFeedAsync(feedUri)` 대신 `SyndicationFeed feed = client.RetrieveFeed(feedUri)` 줄이 포함되어 있으면 `RetrieveFeed`의 반환 값을 사용할 수 있을 때까지 앱 실행이 중지됩니다. 또한 앱이 메서드가 완료될 때까지 기다리는 동안에는 [**Click**](https://msdn.microsoft.com/library/windows/apps/BR227737) 이벤트 등의 다른 이벤트에 응답할 수 없습니다. 즉, `RetrieveFeed`가 반환될 때까지 앱이 차단됩니다.

반면 `client.RetrieveFeedAsync`를 호출하면 메서드가 검색을 시작한 후 바로 반환됩니다. [
            **RetrieveFeedAsync**](https://msdn.microsoft.com/library/windows/apps/BR243460)와 함께 **await**를 사용하면 앱은 일시적으로 이벤트 처리기를 종료합니다. 그런 다음 앱은 **RetrieveFeedAsync**가 비동기적으로 실행되는 동안 다른 이벤트를 처리할 수 있습니다. 따라서 앱이 사용자에게 응답 가능한 상태를 유지하게 됩니다. **RetrieveFeedAsync**가 완료되고 [**SyndicationFeed**](https://msdn.microsoft.com/library/windows/apps/BR243485)를 사용할 수 있게 되면 앱은 이벤트 처리기를 종료한 지점인 `SyndicationFeed feed = await client.RetrieveFeedAsync(feedUri)` 다음부터 이벤트 처리기를 다시 시작한 후 메서드의 나머지 부분을 완료합니다.

**await** 연산자를 사용하면 코드가 가상의 `RetrieveFeed` 메서드를 사용했을 때의 코드와 많은 차이가 없다는 이점이 있습니다. C# 또는 Visual Basic에서 **await** 연산자를 사용하지 않고 비동기 코드를 작성하는 방법에는 여러 가지가 있지만 결과 코드는 비동기적으로 실행되는 구조 위주로 작성되기 쉽습니다. 이 경우 비동기 코드를 작성하기 어려울 뿐 아니라 이해하거나 유지 관리하기도 어렵습니다. **await** 연산자를 사용하면 코드를 복잡하게 만들지 않고 비동기 앱의 이점을 얻을 수 있습니다.

## 비동기 API의 반환 형식 및 결과


[
            **RetrieveFeedAsync**](https://msdn.microsoft.com/library/windows/apps/BR243460) 링크를 따라가면 **RetrieveFeedAsync**의 반환 형식이 [**SyndicationFeed**](https://msdn.microsoft.com/library/windows/apps/BR243485)가 아니라는 것을 알 수 있습니다. 대신 반환 형식은 `IAsyncOperationWithProgress<SyndicationFeed, RetrievalProgress>`입니다. 원시 구문에서 보면 비동기 API는 내부에 결과가 포함된 개체를 반환합니다. 일반적으로 비동기 메서드는 awaitable 형식으로 여기며 이러한 가정이 때로는 유용하기도 하지만, 실제로 **await** 연산자는 메서드가 아닌 메서드의 반환 값에서 작동합니다. **await** 연산자를 적용하면 메서드에서 반환된 개체에 대해 **GetResult**를 호출하는 결과를 얻게 됩니다. 위 예제에서 **SyndicationFeed**는 **RetrieveFeedAsync.GetResult()**의 결과입니다.

비동기 메서드를 사용하면 메서드에서 값이 반환되는 것을 기다린 후 그 결과를 보기 위해 서명을 검사할 수 있습니다. UWP의 모든 비동기 API는 다음 형식 중 하나를 반환합니다.

-   [**IAsyncOperation&lt;TResult&gt;**](https://msdn.microsoft.com/library/windows/apps/BR206598)
-   [**IAsyncOperationWithProgress&lt;TResult, TProgress&gt;**](https://msdn.microsoft.com/library/windows/apps/BR206594)
-   [**IAsyncAction**](https://msdn.microsoft.com/library/windows/apps/BR206580)
-   [**IAsyncActionWithProgress&lt;TProgress&gt;**](https://msdn.microsoft.com/library/windows/apps/BR206580withprogress_1)

비동기 메서드의 결과 형식은 `      TResult` 형식 매개 변수와 같습니다. `TResult`가 없는 형식에는 결과가 없습니다. 이 경우 결과를 **void**로 간주할 수 있습니다. Visual Basic에서 [Sub](https://msdn.microsoft.com/en-us/library/windows/apps/xaml/831f9wka.aspx) 프로시저는 반환 형식이 **void**인 메서드와 같습니다.

아래 표에는 비동기 메서드의 예와 각각의 반환 형식 및 결과 형식이 나와 있습니다.

| 비동기 메서드                                                                           | 반환 형식                                                                                                                                        | 결과 형식                                       |
|-----------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------|---------------------------------------------------|
| [**SyndicationClient.RetrieveFeedAsync**](https://msdn.microsoft.com/library/windows/apps/BR243460)     | [**IAsyncOperationWithProgress&lt;SyndicationFeed, RetrievalProgress&gt;**](https://msdn.microsoft.com/library/windows/apps/BR206594)                                 | [**SyndicationFeed**](https://msdn.microsoft.com/library/windows/apps/BR243485) |
| [**FileOpenPicker.PickSingleFileAsync**](https://msdn.microsoft.com/library/windows/apps/JJ635275) | [**IAsyncOperation&lt;StorageFile&gt;**](https://msdn.microsoft.com/library/windows/apps/BR206598)                                                                                | [**StorageFile**](https://msdn.microsoft.com/library/windows/apps/BR227171)          |
| [**XmlDocument.SaveToFileAsync**](https://msdn.microsoft.com/library/windows/apps/BR206284)                 | [**IAsyncAction**](https://msdn.microsoft.com/library/windows/apps/BR206580)                                                                                                           | **void**                                          |
| [**InkStrokeContainer.LoadAsync**](https://msdn.microsoft.com/library/windows/apps/Hh701757)               | [**IAsyncActionWithProgress&lt;UInt64&gt;**](https://msdn.microsoft.com/library/windows/apps/BR206580withprogress_1)                                                                   | **void**                                          |
| [**DataReader.LoadAsync**](https://msdn.microsoft.com/library/windows/apps/BR208135)                            | [
            **DataReaderLoadOperation**](https://msdn.microsoft.com/library/windows/apps/BR208120)(**IAsyncOperation&lt;UInt32&gt;**를 구현하는 사용자 지정 결과 클래스) | [**UInt32**](T:System.UInt32)                     |

 

[
            **UWP 앱용 .NET**](https://msdn.microsoft.com/en-us/library/windows/apps/xaml/br230232.aspx)에 정의된 비동기 메서드의 반환 형식은 [**Task**](https://msdn.microsoft.com/en-us/library/windows/apps/xaml/system.threading.tasks.task.aspx) 또는 [**Task&lt;TResult&gt;**](https://msdn.microsoft.com/en-us/library/windows/apps/xaml/dd321424.aspx)입니다. **Task**를 반환하는 메서드는 [**IAsyncAction**](https://msdn.microsoft.com/library/windows/apps/BR206580)을 반환하는 UWP의 비동기 메서드와 비슷합니다. 각각의 경우 비동기 메서드의 결과는 **void**입니다. 반환 형식 **Task&lt;TResult&gt;**는 작업을 실행할 때 비동기 메서드의 결과가 `TResult` 형식 매개 변수와 동일한 형식이라는 점에서 [**IAsyncOperation&lt;TResult&gt;**](https://msdn.microsoft.com/library/windows/apps/BR206598)와 비슷합니다. **UWP 앱용 .NET** 및 작업 사용 방법에 대한 자세한 내용은 [Windows 런타임 앱용 .NET 개요](https://msdn.microsoft.com/en-us/library/windows/apps/xaml/br230302.aspx)를 참조하세요.

## 오류 처리


**await** 연산자를 사용하여 비동기 메서드에서 결과를 검색할 경우 **try/catch** 블록을 사용하여 동기 메서드에서 오류를 처리할 때처럼 비동기 메서드에서 발생하는 오류를 처리할 수 있습니다. 위 예제에서는 **RetrieveFeedAsync** 메서드와 **await** 작업을 **try/catch** 블록으로 래핑하여 예외가 발생할 때 오류를 처리합니다.

비동기 메서드가 다른 비동기 메서드를 호출할 경우 예외가 발생한 모든 비동기 메서드는 외부 메서드로 전파됩니다. 이는 가장 바깥쪽 메서드에 **try/catch** 블록을 넣어 중첩된 비동기 메서드에 대한 오류를 catch할 수 있음을 의미합니다. 즉, 동기 메서드에 대한 예외를 catch하는 방법과 유사합니다. 그러나 **catch** 블록에는 **await**를 사용할 수 없습니다.

**팁** Microsoft Visual Studio 2005의 C#부터는 **catch** 블록에서 **await**를 사용할 수 있습니다.

## 요약 및 다음 단계

여기에서 살펴본 비동기 메서드를 호출하는 패턴은 이벤트 처리기에서 비동기 API를 호출할 때 사용할 수 있는 가장 간단한 패턴입니다. 또한 Visual Basic에서 **void** 또는 **Sub**를 반환하는 재정의된 메서드에서 비동기 메서드를 호출할 경우에도 이 패턴을 사용할 수 있습니다.

UWP에서 비동기 메서드가 발생할 때는 다음 사항을 기억해야 합니다.

-   규칙에 따라 비동기 메서드에는 "Async"로 끝나는 이름이 지정됩니다.
-   **await** 연산자를 사용하는 메서드는 **async** 키워드로 표시된 메서드 선언이 있어야 합니다.
-   앱이 **await** 연산자를 찾으면 비동기 메서드가 실행되는 동안 사용자 조작에 응답 가능한 상태를 유지합니다.
-   비동기 메서드에서 값이 반환되는 것을 기다리면 결과가 포함된 개체를 반환합니다. 대부분의 경우 반환 값 자체가 아닌 반환 값에 포함된 결과가 유용합니다. 비동기 메서드의 반환 형식을 보면 결과에 포함된 값의 형식을 찾을 수 있습니다.
-   앱의 응답성을 향상시키기 위해 비동기 API 및 **async** 패턴을 사용하는 경우가 자주 있습니다.

이 항목의 예제는 다음과 같은 텍스트를 출력합니다.

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



<!--HONumber=Mar16_HO1-->


