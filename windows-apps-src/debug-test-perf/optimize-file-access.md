---
author: jwmsft
ms.assetid: 40122343-1FE3-4160-BABE-6A2DD9AF1E8E
title: 파일 액세스 최적화
description: 파일 시스템에 효율적으로 액세스하여 디스크 대기 시간 및 메모리/CPU 주기로 인한 성능 문제를 방지하는 UWP(유니버설 Windows 플랫폼) 앱을 만듭니다.
ms.author: jimwalk
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 1b0b1a45bc967dd69d38f2e85609a5e13ffd61b8
ms.sourcegitcommit: e814a13978f33654d8e995584f4b047cb53e0aef
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/05/2018
ms.locfileid: "6040755"
---
# <a name="optimize-file-access"></a>파일 액세스 최적화


파일 시스템에 효율적으로 액세스하여 디스크 대기 시간 및 메모리/CPU 주기로 인한 성능 문제를 방지하는 UWP(유니버설 Windows 플랫폼) 앱을 만듭니다.

대규모 파일 모음에 액세스할 때 일반적인 Name, FileType 및 Path 속성이 아닌 속성 값에 액세스하려는 경우 [**QueryOptions**](https://msdn.microsoft.com/library/windows/apps/BR207995)를 만들고 [**SetPropertyPrefetch**](https://msdn.microsoft.com/library/windows/apps/hh973319)를 호출하여 액세스합니다. **SetPropertyPrefetch** 메서드를 사용하면 파일 시스템에서 가져온 항목의 모음(예: 이미지의 모음)을 표시하는 앱의 성능을 크게 개선할 수 있습니다. 다음 예제 집합은 여러 파일에 액세스하는 몇 가지 방법을 보여줍니다.

첫 번째 예제는 [**Windows.Storage.StorageFolder.GetFilesAsync**](https://msdn.microsoft.com/library/windows/apps/BR227273)를 사용하여 파일 집합의 이름 정보를 검색합니다. 그러면 name 속성만 액세스하므로 우수한 성능을 얻을 수 있습니다.

> [!div class="tabbedCodeSnippets"]
> ```csharp
> StorageFolder library = Windows.Storage.KnownFolders.PicturesLibrary;
> IReadOnlyList<StorageFile> files = await library.GetFilesAsync(Windows.Storage.Search.CommonFileQuery.OrderByDate);
> 
> for (int i = 0; i < files.Count; i++)
> {
>     // do something with the name of each file
>     string fileName = files[i].Name;
> }
> ```
> ```vb
> Dim library As StorageFolder = Windows.Storage.KnownFolders.PicturesLibrary
> Dim files As IReadOnlyList(Of StorageFile) =
>     Await library.GetFilesAsync(Windows.Storage.Search.CommonFileQuery.OrderByDate)
> 
> For i As Integer = 0 To files.Count - 1
>     ' do something with the name of each file
>     Dim fileName As String = files(i).Name
> Next i
> ```

두 번째 예제는 [**Windows.Storage.StorageFolder.GetFilesAsync**](https://msdn.microsoft.com/library/windows/apps/BR227273)를 사용하고 각 파일의 이미지 속성을 검색합니다. 이 방법에서는 성능이 저하됩니다.

> [!div class="tabbedCodeSnippets"]
> ```csharp
> StorageFolder library = Windows.Storage.KnownFolders.PicturesLibrary;
> IReadOnlyList<StorageFile> files = await library.GetFilesAsync(Windows.Storage.Search.CommonFileQuery.OrderByDate);
> for (int i = 0; i < files.Count; i++)
> {
>     ImageProperties imgProps = await files[i].Properties.GetImagePropertiesAsync();
> 
>     // do something with the date the image was taken
>     DateTimeOffset date = imgProps.DateTaken;
> }
> ```
> ```vb
> Dim library As StorageFolder = Windows.Storage.KnownFolders.PicturesLibrary
> Dim files As IReadOnlyList(Of StorageFile) = Await library.GetFilesAsync(Windows.Storage.Search.CommonFileQuery.OrderByDate)
> For i As Integer = 0 To files.Count - 1
>     Dim imgProps As ImageProperties =
>         Await files(i).Properties.GetImagePropertiesAsync()
> 
>     ' do something with the date the image was taken
>     Dim dateTaken As DateTimeOffset = imgProps.DateTaken
> Next i
> ```

세 번째 예제는 [**QueryOptions**](https://msdn.microsoft.com/library/windows/apps/BR207995)를 사용하여 파일 집합에 대한 정보를 얻습니다. 이전의 예제보다 훨씬 우수한 성능을 얻을 수 있습니다.

> [!div class="tabbedCodeSnippets"]
> ```csharp
> // Set QueryOptions to prefetch our specific properties
> var queryOptions = new Windows.Storage.Search.QueryOptions(CommonFileQuery.OrderByDate, null);
> queryOptions.SetThumbnailPrefetch(ThumbnailMode.PicturesView, 100,
>         ThumbnailOptions.ReturnOnlyIfCached);
> queryOptions.SetPropertyPrefetch(PropertyPrefetchOptions.ImageProperties, 
>        new string[] {"System.Size"});
> 
> StorageFileQueryResult queryResults = KnownFolders.PicturesLibrary.CreateFileQueryWithOptions(queryOptions);
> IReadOnlyList<StorageFile> files = await queryResults.GetFilesAsync();
> 
> foreach (var file in files)
> {
>     ImageProperties imageProperties = await file.Properties.GetImagePropertiesAsync();
> 
>     // Do something with the date the image was taken.
>     DateTimeOffset dateTaken = imageProperties.DateTaken;
> 
>     // Performance gains increase with the number of properties that are accessed.
>     IDictionary<String, object> propertyResults =
>         await file.Properties.RetrievePropertiesAsync(
>               new string[] {"System.Size" });
> 
>     // Get/Set extra properties here
>     var systemSize = propertyResults["System.Size"];
> }
> ```
> ```vb
> ' Set QueryOptions to prefetch our specific properties
> Dim queryOptions = New Windows.Storage.Search.QueryOptions(CommonFileQuery.OrderByDate, Nothing)
> queryOptions.SetThumbnailPrefetch(ThumbnailMode.PicturesView,
>             100, Windows.Storage.FileProperties.ThumbnailOptions.ReturnOnlyIfCached)
> queryOptions.SetPropertyPrefetch(PropertyPrefetchOptions.ImageProperties,
>                                  New String() {"System.Size"})
> 
> Dim queryResults As StorageFileQueryResult = KnownFolders.PicturesLibrary.CreateFileQueryWithOptions(queryOptions)
> Dim files As IReadOnlyList(Of StorageFile) = Await queryResults.GetFilesAsync()
> 
> 
> For Each file In files
>     Dim imageProperties As ImageProperties = Await file.Properties.GetImagePropertiesAsync()
> 
>     ' Do something with the date the image was taken.
>     Dim dateTaken As DateTimeOffset = imageProperties.DateTaken
> 
>     ' Performance gains increase with the number of properties that are accessed.
>     Dim propertyResults As IDictionary(Of String, Object) =
>         Await file.Properties.RetrievePropertiesAsync(New String() {"System.Size"})
> 
>     ' Get/Set extra properties here
>     Dim systemSize = propertyResults("System.Size")
> 
> Next file
> ```
Windows.Storage 개체(예: `Windows.Storage.ApplicationData.Current.LocalFolder`)에 대해 여러 작업을 수행하는 경우, 액세스할 때마다 중간 개체를 만들지 않도록 저장소 원본을 가리키는 로컬 변수를 만듭니다.

## <a name="stream-performance-in-c-and-visual-basic"></a>C# 및 Visual Basic에서 스트림 성능

### <a name="buffering-between-uwp-and-net-streams"></a>UWP와 .NET 스트림 간의 버퍼링

UWP 스트림(예: [**Windows.Storage.Streams.IInputStream**](https://msdn.microsoft.com/library/windows/apps/BR241718) 또는 [**IOutputStream**](https://msdn.microsoft.com/library/windows/apps/BR241728))을 .NET 스트림([**System.IO.Stream**](https://msdn.microsoft.com/library/windows/apps/xaml/system.io.stream.aspx))으로 변환할 수 있는 여러 시나리오가 있습니다. 예를 들어 이 시나리오는 UWP 앱을 작성하고 UWP 파일 시스템의 스트림에서 작동되는 기존 .NET 코드를 사용하려는 경우에 유용합니다. 이 지원 하려면 UWP 앱 용.NET Api는.NET 및 UWP 스트림 유형 간을 변환할 수 있는 확장 메서드를 제공 합니다. 자세한 내용은 [**WindowsRuntimeStreamExtensions**](https://msdn.microsoft.com/library/windows/apps/xaml/system.io.windowsruntimestreamextensions.aspx)를 참조하세요.

UWP 스트림을 .NET 스트림으로 변환하면 결과적으로 기본 UWP 스트림에 대한 어댑터가 만들어집니다. 경우에 따라 UWP 스트림의 메서드 호출과 관련된 런타임 부담이 있을 수 있습니다. 이는 특히 여러 작은 읽기나 쓰기 작업을 빈번히 수행하는 시나리오에서 앱 속도에 영향을 줄 수 있습니다.

앱의 속도를 향상하기 위해 UWP 스트림 어댑터에는 데이터 버퍼가 포함되어 있습니다. 다음 코드 샘플에서는 기본 버퍼 크기의 UWP 스트림 어댑터를 사용한 작은 연속 읽기를 보여 줍니다.

> [!div class="tabbedCodeSnippets"]
> ```csharp
> StorageFile file = await Windows.Storage.ApplicationData.Current
>     .LocalFolder.GetFileAsync("example.txt");
> Windows.Storage.Streams.IInputStream windowsRuntimeStream = 
>     await file.OpenReadAsync();
> 
> byte[] destinationArray = new byte[8];
> 
> // Create an adapter with the default buffer size.
> using (var managedStream = windowsRuntimeStream.AsStreamForRead())
> {
> 
>     // Read 8 bytes into destinationArray.
>     // A larger block is actually read from the underlying 
>     // windowsRuntimeStream and buffered within the adapter.
>     await managedStream.ReadAsync(destinationArray, 0, 8);
> 
>     // Read 8 more bytes into destinationArray.
>     // This call may complete much faster than the first call
>     // because the data is buffered and no call to the 
>     // underlying windowsRuntimeStream needs to be made.
>     await managedStream.ReadAsync(destinationArray, 0, 8);
> }
> ```
> ```vb
> Dim file As StorageFile = Await Windows.Storage.ApplicationData.Current -
> .LocalFolder.GetFileAsync("example.txt")
> Dim windowsRuntimeStream As Windows.Storage.Streams.IInputStream =
>     Await file.OpenReadAsync()
> 
> Dim destinationArray() As Byte = New Byte(8) {}
> 
> ' Create an adapter with the default buffer size.
> Dim managedStream As Stream = windowsRuntimeStream.AsStreamForRead()
> Using (managedStream)
> 
>     ' Read 8 bytes into destinationArray.
>     ' A larger block is actually read from the underlying 
>     ' windowsRuntimeStream and buffered within the adapter.
>     Await managedStream.ReadAsync(destinationArray, 0, 8)
> 
>     ' Read 8 more bytes into destinationArray.
>     ' This call may complete much faster than the first call
>     ' because the data is buffered and no call to the 
>     ' underlying windowsRuntimeStream needs to be made.
>     Await managedStream.ReadAsync(destinationArray, 0, 8)
> 
> End Using
> ```

이 기본 버퍼링 동작은 UWP 스트림을 .NET 스트림으로 변환하는 대부분의 시나리오에서 적합합니다. 그러나 일부 시나리오에서는 성능을 늘리기 위해 버퍼링 동작을 조정할 수 있습니다.

### <a name="working-with-large-data-sets"></a>큰 데이터 집합 작업

큰 데이터 집합을 읽거나 쓸 경우에는 [**AsStreamForRead**](https://msdn.microsoft.com/library/windows/apps/xaml/system.io.windowsruntimestreamextensions.asstream.aspx), [**AsStreamForWrite**](https://msdn.microsoft.com/library/windows/apps/xaml/system.io.windowsruntimestreamextensions.asstreamforwrite.aspx) 및 [**AsStream**](https://msdn.microsoft.com/library/windows/apps/xaml/system.io.windowsruntimestreamextensions.asstream.aspx) 확장 메서드에 큰 버퍼 크기를 제공하여 읽기 또는 쓰기 처리량을 늘릴 수 있습니다. 이렇게 하면 스트림 어댑터에 더 큰 내부 버퍼 크기가 제공됩니다. 예를 들어 큰 파일의 스트림을 XML 파서에 전달할 경우 파서는 스트림에서 여러 작은 읽기를 순서대로 수행할 수 있습니다. 버퍼가 크면 기본 UWP 스트림의 호출 수가 줄고 성능이 향상될 수 있습니다.

> **참고**  을 설정할 때 버퍼 크기를 약 80KB 보다 크게 가비지 수집기 힙에서 조각화가 발생할 수이 주의 해야 ( [가비지 수집 성능 향상](improve-garbage-collection-performance.md)참조). 다음 코드 예제에서는 버퍼가 81,920바이트인 관리 스트림 어댑터를 만듭니다.

> [!div class="tabbedCodeSnippets"]
```csharp
// Create a stream adapter with an 80 KB buffer.
Stream managedStream = nativeStream.AsStreamForRead(bufferSize: 81920);
```
```vb
' Create a stream adapter with an 80 KB buffer.
Dim managedStream As Stream = nativeStream.AsStreamForRead(bufferSize:=81920)
```

[**Stream.CopyTo**](https://msdn.microsoft.com/library/windows/apps/xaml/system.io.stream.copyto.aspx) 및 [**CopyToAsync**](https://msdn.microsoft.com/library/windows/apps/xaml/system.io.stream.copytoasync.aspx) 메서드도 스트림 간에 복사하기 위한 로컬 버퍼를 할당합니다. [**AsStreamForRead**](https://msdn.microsoft.com/library/windows/apps/xaml/system.io.windowsruntimestreamextensions.asstreamforread.aspx) 확장 메서드와 마찬가지로 기본 버퍼 크기를 재정의하여 큰 스트림 복사본에 대한 성능을 향상할 수 있습니다. 다음 코드 예제에서는 **CopyToAsync** 호출의 기본 버퍼 크기를 변경하는 방법을 보여 줍니다.

> [!div class="tabbedCodeSnippets"]
> ```csharp
> MemoryStream destination = new MemoryStream();
> // copies the buffer into memory using the default copy buffer
> await managedStream.CopyToAsync(destination);
> 
> // Copy the buffer into memory using a 1 MB copy buffer.
> await managedStream.CopyToAsync(destination, bufferSize: 1024 * 1024);
> ```
> ```vb
> Dim destination As MemoryStream = New MemoryStream()
> ' copies the buffer into memory using the default copy buffer
> Await managedStream.CopyToAsync(destination)
> 
> ' Copy the buffer into memory using a 1 MB copy buffer.
> Await managedStream.CopyToAsync(destination, bufferSize:=1024 * 1024)
> ```

이 예제에서는 앞에서 권장한 80KB보다 큰 1MB의 버퍼 크기를 사용합니다. 이와 같이 큰 버퍼를 사용하면 매우 큰 데이터 집합(즉, 수백 메가바이트)에 대한 복사 작업의 처리량을 향상할 수 있습니다. 그러나 큰 개체 힙에서 이 버퍼를 할당하면 가비지 수집 성능이 저하될 수 있습니다. 앱의 성능이 크게 중요한 경우에만 큰 버퍼 크기를 사용해야 합니다.

많은 수의 스트림을 동시에 사용하는 경우에는 버퍼의 메모리 오버헤드를 줄이거나 제거할 수 있습니다. 더 작은 버퍼를 지정하거나 *bufferSize* 매개 변수를 0으로 설정하여 해당 스트림 어댑터에 대한 버퍼링을 완전히 끌 수 있습니다. 관리 스트림에 대한 많은 읽기 및 쓰기를 수행하는 경우 버퍼링 없이도 적합한 처리 성능을 얻을 수 있습니다.

### <a name="performing-latency-sensitive-operations"></a>대기 시간이 중요한 작업 수행

읽기 및 쓰기의 대기 시간이 짧아야 하고 기본 UWP 스트림에서 큰 블록을 읽지 않는 경우에도 버퍼링을 방지할 수 있습니다. 예를 들어 네트워크 통신을 위해 스트림을 사용하는 경우 읽기 및 쓰기의 대기 시간이 짧아야 합니다.

채팅 앱에서는 네트워크 인터페이스를 통해 스트림을 사용하여 메시지를 주고받을 수 있습니다. 이 경우 메시지를 준비되는 즉시 보내고 버퍼가 찰 때까지 기다리지 않아야 합니다. [**AsStreamForRead**](https://msdn.microsoft.com/library/windows/apps/xaml/system.io.windowsruntimestreamextensions.asstreamforread.aspx), [**AsStreamForWrite**](https://msdn.microsoft.com/library/windows/apps/xaml/system.io.windowsruntimestreamextensions.asstreamforwrite.aspx) 및 [**AsStream**](https://msdn.microsoft.com/library/windows/apps/xaml/system.io.windowsruntimestreamextensions.asstream.aspx) 확장 메서드를 호출할 때 버퍼 크기를 0으로 설정하면 결과 어댑터는 버퍼를 할당하지 않고 모든 호출에서 기본 UWP 스트림을 직접 조작합니다.


