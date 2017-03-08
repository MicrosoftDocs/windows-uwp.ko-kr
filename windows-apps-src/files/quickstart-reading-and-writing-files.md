---
author: laurenhughes
ms.assetid: 27914C0A-2A02-473F-BDD5-C931E3943AA0
title: "파일 만들기, 쓰기 및 읽기"
description: "StorageFile 개체를 사용하여 파일을 읽고 씁니다."
ms.author: lahugh
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: Windows 10, uwp
translationtype: Human Translation
ms.sourcegitcommit: c6b64cff1bbebc8ba69bc6e03d34b69f85e798fc
ms.openlocfilehash: ae2754f543a3bc799b3d5af4c5c3c46f654c1ed7
ms.lasthandoff: 02/07/2017

---

# <a name="create-write-and-read-a-file"></a>파일 만들기, 쓰기 및 읽기


\[ Windows 10의 UWP 앱에 맞게 업데이트되었습니다. Windows 8.x 문서는 [보관](http://go.microsoft.com/fwlink/p/?linkid=619132)을 참조하세요. \]


**중요 API**

-   [**StorageFolder 클래스**](https://msdn.microsoft.com/library/windows/apps/br227230)
-   [**StorageFile 클래스**](https://msdn.microsoft.com/library/windows/apps/br227171)
-   [**FileIO 클래스**](https://msdn.microsoft.com/library/windows/apps/hh701440)

[**StorageFile**](https://msdn.microsoft.com/library/windows/apps/br227171) 개체를 사용하여 파일을 읽고 씁니다.

> **참고**  [파일 액세스 샘플](http://go.microsoft.com/fwlink/p/?linkid=619995)도 참조하세요.

## <a name="prerequisites"></a>필수 조건

-   **UWP(유니버설 Windows 플랫폼) 앱에 대한 비동기 프로그래밍 이해**

    C# 또는 Visual Basic에서 비동기 앱을 작성하는 방법에 대한 자세한 내용은 [C# 또는 Visual Basic에서 비동기식 API 호출](https://msdn.microsoft.com/library/windows/apps/mt187337)을 참조하세요. C++에서 비동기 앱을 작성하는 방법은 [C++의 비동기 프로그래밍](https://msdn.microsoft.com/library/windows/apps/mt187334)을 참조하세요.

-   **읽거나, 쓰거나, 일고 쓸 파일을 가져오는 방법에 대해 알아봅니다.**

    파일 선택기를 사용하여 파일을 가져오는 방법은 [선택기를 사용하여 파일 및 폴더 열기](quickstart-using-file-and-folder-pickers.md)를 참조하세요.

## <a name="creating-a-file"></a>파일 만들기

앱의 로컬 폴더에 파일을 만드는 방법은 다음과 같습니다. 이미 있는 경우 바꿉니다.
> [!div class="tabbedCodeSnippets"]
```cs
// Create sample file; replace if exists.
Windows.Storage.StorageFolder storageFolder =
    Windows.Storage.ApplicationData.Current.LocalFolder;
Windows.Storage.StorageFile sampleFile =
    await storageFolder.CreateFileAsync("sample.txt",
        Windows.Storage.CreationCollisionOption.ReplaceExisting);
```
```vb
' Create sample file; replace if exists.
Dim storageFolder As StorageFolder = Windows.Storage.ApplicationData.Current.LocalFolder
Dim sampleFile As StorageFile = Await storageFolder.CreateFileAsync("sample.txt", CreationCollisionOption.ReplaceExisting)
```

## <a name="writing-to-a-file"></a>파일에 쓰기


[**StorageFile**](https://msdn.microsoft.com/library/windows/apps/br227171) 클래스를 사용하여 디스크의 쓰기 가능 파일에 쓰는 방법은 다음과 같습니다. 파일에 쓰는 각 방법의 공통적인 첫 번째 단계(파일을 만든 즉시 해당 파일에 쓰는 경우 제외)는 [**StorageFolder.GetFileAsync**](https://msdn.microsoft.com/library/windows/apps/br227272)를 사용하여 파일을 가져오는 것입니다.
> [!div class="tabbedCodeSnippets"]
```cs
Windows.Storage.StorageFolder storageFolder =
    Windows.Storage.ApplicationData.Current.LocalFolder;
Windows.Storage.StorageFile sampleFile =
    await storageFolder.GetFileAsync("sample.txt");
```
```vb
Dim storageFolder As StorageFolder = Windows.Storage.ApplicationData.Current.LocalFolder
Dim sampleFile As StorageFile = Await storageFolder.GetFileAsync("sample.txt")
```

**파일에 텍스트 쓰기**

[**FileIO**](https://msdn.microsoft.com/library/windows/apps/hh701505) 클래스의 [**WriteTextAsync**](https://msdn.microsoft.com/library/windows/apps/hh701440) 메서드를 호출하여 파일에 텍스트를 씁니다.
> [!div class="tabbedCodeSnippets"]
```cs
await Windows.Storage.FileIO.WriteTextAsync(sampleFile, "Swift as a shadow");
```
```vb
Await Windows.Storage.FileIO.WriteTextAsync(sampleFile, "Swift as a shadow")
```

**버퍼를 사용하여 파일에 바이트 쓰기(2단계)**

1.  먼저 [**ConvertStringToBinary**](https://msdn.microsoft.com/library/windows/apps/br241385)를 호출하여 파일에 쓰려는 바이트(임의 문자열 기반)의 버퍼를 가져옵니다.
> [!div class="tabbedCodeSnippets"]
```cs
var buffer = Windows.Security.Cryptography.CryptographicBuffer.ConvertStringToBinary(
        "What fools these mortals be", Windows.Security.Cryptography.BinaryStringEncoding.Utf8);
```
```vb
Dim buffer = Windows.Security.Cryptography.CryptographicBuffer.ConvertStringToBinary(
                    "What fools these mortals be",
                    Windows.Security.Cryptography.BinaryStringEncoding.Utf8)
```

2.  그런 다음 [**FileIO**](https://msdn.microsoft.com/library/windows/apps/hh701490) 클래스의 [**WriteBufferAsync**](https://msdn.microsoft.com/library/windows/apps/hh701440) 메서드를 호출하여 파일에 버퍼의 바이트를 씁니다.
> [!div class="tabbedCodeSnippets"]
```cs
await Windows.Storage.FileIO.WriteBufferAsync(sampleFile, buffer);
```
```vb
Await Windows.Storage.FileIO.WriteBufferAsync(sampleFile, buffer)
```

**스트림을 사용하여 파일에 텍스트 쓰기(4단계)**

1.  먼저 [**StorageFile.OpenAsync**](https://msdn.microsoft.com/library/windows/apps/dn889851) 메서드를 호출하여 파일을 엽니다. 이 메서드는 열기 작업이 완료되면 파일 내용의 스트림을 반환합니다.
> [!div class="tabbedCodeSnippets"]
```cs
var stream = await sampleFile.OpenAsync(Windows.Storage.FileAccessMode.ReadWrite);
```
```vb
Dim stream = Await sampleFile.OpenAsync(Windows.Storage.FileAccessMode.ReadWrite)
```

2.  다음으로, `stream`에서 [**GetOutputStreamAt**](https://msdn.microsoft.com/library/windows/apps/br241738) 메서드를 호출하여 출력 스트림을 가져옵니다. **using** 문에 이 스트림을 넣어 출력 스트림의 수명을 관리합니다.
> [!div class="tabbedCodeSnippets"]
```cs
using (var outputStream = stream.GetOutputStreamAt(0))
    {
        // We'll add more code here in the next step.
    }
    stream.Dispose(); // Or use the stream variable (see previous code snippet) with a using statement as well.
```
```vb
Using outputStream = stream.GetOutputStreamAt(0)
  ' We'll add more code here in the next step.
End Using
```

3.  이제 기존 **using** 문 내에 다음 코드를 추가해 새 [**DataWriter**](https://msdn.microsoft.com/library/windows/apps/br208154) 개체를 만들고 [**DataWriter.WriteString**](https://msdn.microsoft.com/library/windows/apps/br241642) 메서드를 호출하여 출력 스트림에 씁니다.
> [!div class="tabbedCodeSnippets"]
```cs
using (var dataWriter = new Windows.Storage.Streams.DataWriter(outputStream))
    {
        dataWriter.WriteString("DataWriter has methods to write to various types, such as DataTimeOffset.");
    }
```
```vb
    Dim dataWriter As New DataWriter(outputStream)
    dataWriter.WriteString("DataWriter has methods to write to various types, such as DataTimeOffset.")
```

4.  마지막으로, 내부 **using** 문 내에 다음 코드를 추가하여 [**StoreAsync**](https://msdn.microsoft.com/library/windows/apps/br208171)로 텍스트를 파일에 저장하고 [**FlushAsync**](https://msdn.microsoft.com/library/windows/apps/br241729)로 스트림을 닫습니다.
> [!div class="tabbedCodeSnippets"]
```cs
    await dataWriter.StoreAsync();
        await outputStream.FlushAsync();
```
```vb
    Await dataWriter.StoreAsync()
        Await outputStream.FlushAsync()
```

## <a name="reading-from-a-file"></a>파일에서 읽기


[**StorageFile**](https://msdn.microsoft.com/library/windows/apps/br227171) 클래스를 사용하여 디스크의 파일에서 읽는 방법은 다음과 같습니다. 파일에서 읽는 각 방법의 공통적인 첫 번째 단계는 [**StorageFolder.GetFileAsync**](https://msdn.microsoft.com/library/windows/apps/br227272)를 사용하여 파일을 가져오는 것입니다.
> [!div class="tabbedCodeSnippets"]
```cs
Windows.Storage.StorageFolder storageFolder =
    Windows.Storage.ApplicationData.Current.LocalFolder;
Windows.Storage.StorageFile sampleFile =
    await storageFolder.GetFileAsync("sample.txt");
```
```vb
Dim storageFolder As StorageFolder = Windows.Storage.ApplicationData.Current.LocalFolder
Dim sampleFile As StorageFile = Await storageFolder.GetFileAsync("sample.txt")
```

**파일에서 텍스트 읽기**

[**FileIO**](https://msdn.microsoft.com/library/windows/apps/hh701482) 클래스의 [**ReadTextAsync**](https://msdn.microsoft.com/library/windows/apps/hh701440) 메서드를 호출하여 파일에서 텍스트를 읽습니다.
> [!div class="tabbedCodeSnippets"]
```cs
string text = await Windows.Storage.FileIO.ReadTextAsync(sampleFile);
```
```vb
Dim text As String = Await Windows.Storage.FileIO.ReadTextAsync(sampleFile)
```

**버퍼를 사용하여 파일에서 텍스트 읽기(2단계)**

1.  먼저 [**FileIO**](https://msdn.microsoft.com/library/windows/apps/hh701468) 클래스의 [**ReadBufferAsync**](https://msdn.microsoft.com/library/windows/apps/hh701440) 메서드를 호출합니다.
> [!div class="tabbedCodeSnippets"]
```cs
var buffer = await Windows.Storage.FileIO.ReadBufferAsync(sampleFile);
```
```vb
Dim buffer = Await Windows.Storage.FileIO.ReadBufferAsync(sampleFile)
```

2.  그런 다음 [**DataReader**](https://msdn.microsoft.com/library/windows/apps/br208119) 개체를 사용하여 버퍼의 길이와 버퍼의 내용을 차례로 읽습니다.
> [!div class="tabbedCodeSnippets"]
```cs
using (var dataReader = Windows.Storage.Streams.DataReader.FromBuffer(buffer))
    {
        string text = dataReader.ReadString(buffer.Length);
    }
```
```vb
Dim dataReader As DataReader = Windows.Storage.Streams.DataReader.FromBuffer(buffer)
    Dim text As String = dataReader.ReadString(buffer.Length)
```

**스트림을 사용하여 파일에서 텍스트 읽기(4단계)**

1.  [**StorageFile.OpenAsync**](https://msdn.microsoft.com/library/windows/apps/dn889851) 메서드를 호출하여 파일의 스트림을 엽니다. 이 메서드는 작업이 완료되면 파일 내용의 스트림을 반환합니다.
> [!div class="tabbedCodeSnippets"]
```cs
var stream = await sampleFile.OpenAsync(Windows.Storage.FileAccessMode.ReadWrite);
```
```vb
Dim stream = Await sampleFile.OpenAsync(Windows.Storage.FileAccessMode.ReadWrite)
```

2.  나중에 사용할 스트림의 크기를 가져옵니다.
> [!div class="tabbedCodeSnippets"]
```cs
ulong size = stream.Size;
```
```vb
Dim size = stream.Size
```

3.  [**GetInputStreamAt**](https://msdn.microsoft.com/library/windows/apps/br241737) 메서드를 호출하여 입력 스트림을 가져옵니다. 이를 **using** 문에 배치하여 스트림의 수명을 관리합니다. **GetInputStreamAt**을 호출할 때 0을 지정하여 위치를 스트림의 시작 부분으로 설정합니다.
> [!div class="tabbedCodeSnippets"]
```cs
using (var inputStream = stream.GetInputStreamAt(0))
    {
        // We'll add more code here in the next step.
    }
```
```vb
Using inputStream = stream.GetInputStreamAt(0)
    ' We'll add more code here in the next step.
End Using
```

4.  마지막으로, 기존 **using** 문 내에 다음 코드를 추가하여 스트림에서 [**DataReader**](https://msdn.microsoft.com/library/windows/apps/br208119) 개체를 가져온 다음 [**DataReader.LoadAsync**](https://msdn.microsoft.com/library/windows/apps/br208135) 및 [**DataReader.ReadString**](https://msdn.microsoft.com/library/windows/apps/br208147)를 호출하여 텍스트를 읽습니다.
> [!div class="tabbedCodeSnippets"]
```cs
using (var dataReader = new Windows.Storage.Streams.DataReader(inputStream))
    {
        uint numBytesLoaded = await dataReader.LoadAsync((uint)size);
        string text = dataReader.ReadString(numBytesLoaded);
    }
```
```vb
Dim dataReader As New DataReader(inputStream)
    Dim numBytesLoaded As UInteger = Await dataReader.LoadAsync(CUInt(size))
    Dim text As String = dataReader.ReadString(numBytesLoaded)
```

 

 

