---
title: 파일 쓰기 모범 사례
description: FileIO 및 PathIO 클래스의 다양한 파일 쓰기 메서드를 사용하는 모범 사례에 대해 알아봅니다.
ms.date: 02/06/2019
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: dcbeffc7e3db8f3df9c197e8c388f30faf7ad03d
ms.sourcegitcommit: 26bb75084b9d2d2b4a76d4aa131066e8da716679
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 01/06/2020
ms.locfileid: "75685245"
---
# <a name="best-practices-for-writing-to-files"></a>파일 쓰기 모범 사례

**중요 API**

* [**FileIO 클래스**](https://docs.microsoft.com/uwp/api/Windows.Storage.FileIO)
* [**PathIO 클래스**](https://docs.microsoft.com/uwp/api/windows.storage.pathio)

경우에 따라 개발자는 [**FileIO**](https://docs.microsoft.com/uwp/api/Windows.Storage.FileIO) 및 [**PathIO**](https://docs.microsoft.com/uwp/api/windows.storage.pathio) 클래스의 **Write** 메서드를 사용하여 파일 시스템 I/O 작업을 수행할 때 몇 가지 일반적인 문제가 발생합니다. 예를 들어, 일반적인 문제는 다음과 같습니다.

* 파일이 부분적으로 기록됩니다.
* 앱이 메서드 중 하나를 호출할 때 예외가 발생합니다.
* 작업을 수행할 때 대상 파일 이름과 비슷한 이름의 .TMP 파일이 남아 있습니다.

[**FileIO**](https://docs.microsoft.com/uwp/api/Windows.Storage.FileIO) 및 [**PathIO**](https://docs.microsoft.com/uwp/api/windows.storage.pathio) 클래스의 **Write** 메서드에는 다음이 포함됩니다.

* **WriteBufferAsync**
* **WriteBytesAsync**
* **WriteLinesAsync**
* **WriteTextAsync**

 이 문서에서는 이러한 메서드가 작동하는 방식을 자세히 설명하므로 개발자는 메서드를 사용해야 하는 경우 및 방법을 보다 잘 이해할 수 있습니다. 이 문서는 지침을 제공하며, 가능한 모든 파일 I/O 문제에 대한 해결 방법을 제공하지는 않습니다. 

> [!NOTE]
> 이 문서는 예제에 포함되며 논의 중인 **FileIO** 메서드를 중점적으로 다룹니다. 그러나 **PathIO** 메서드는 비슷한 패턴을 따르며 이 문서의 지침 대부분이 해당 메서드에도 적용됩니다. 

## <a name="convenience-vs-control"></a>편의성 및 제어

[**StorageFile**](https://docs.microsoft.com/uwp/api/windows.storage.storagefile) 개체는 네이티브 Win32 프로그래밍 모델과 같은 파일 핸들이 아닙니다. 대신, [**StorageFile**](https://docs.microsoft.com/uwp/api/windows.storage.storagefile)은 해당 콘텐츠를 조작하는 메서드를 포함하는 파일을 나타냅니다.

이 개념을 이해하면 **StorageFile**에서 I/O를 수행할 때 도움이 됩니다. 예를 들어 [파일에 쓰기](quickstart-reading-and-writing-files.md#writing-to-a-file) 섹션에서는 파일에 쓰는 다음과 같은 세 가지 방법을 제공합니다.

* [**FileIO.WriteTextAsync**](https://docs.microsoft.com/uwp/api/windows.storage.fileio.writetextasync) 메서드 사용
* 버퍼를 만든 후 [**FileIO.WriteBufferAsync**](https://docs.microsoft.com/uwp/api/windows.storage.fileio.writebufferasync) 메서드 호출
* 스트림을 사용하는 4단계 모델:
  1. 파일을 [열어](https://docs.microsoft.com/uwp/api/windows.storage.storagefile.openasync) 스트림을 가져옵니다.
  2. 출력 스트림을 [가져옵니다](https://docs.microsoft.com/uwp/api/windows.storage.streams.irandomaccessstream.getoutputstreamat).
  3. [**DataWriter**](https://docs.microsoft.com/uwp/api/windows.storage.streams.datawriter) 개체를 만들고 해당 **Write** 메서드를 호출합니다.
  4. 데이터 기록기에서 데이터를 [커밋](https://docs.microsoft.com/uwp/api/windows.storage.streams.datawriter.storeasync)하고 출력 스트림을 [플러시](https://docs.microsoft.com/uwp/api/windows.storage.streams.ioutputstream.flushasync)합니다.

처음 두 시나리오는 앱에서 가장 일반적으로 사용하는 시나리오입니다. 단일 작업으로 파일을 쓰면 코딩 및 유지 관리가 더 쉬우며 앱이 파일 I/O의 많은 복잡성을 처리할 필요도 없습니다. 그러나 이러한 편리성에는 문제점이 따릅니다. 전체 작업을 제어할 수 없게 되고 특정 지점의 오류도 잡아낼 수 없습니다.

## <a name="the-transactional-model"></a>트랜잭션 모델

[**FileIO**](https://docs.microsoft.com/uwp/api/Windows.Storage.FileIO) 및 [**PathIO**](https://docs.microsoft.com/uwp/api/windows.storage.pathio) 클래스의 **Write** 메서드는 추가 계층을 사용하여 위에서 설명한 세 번째 쓰기 모델의 단계를 래핑합니다. 이 계층은 스토리지 트랜잭션에 캡슐화됩니다.

데이터를 작성하는 동안 문제가 발생하는 경우 원본 파일의 무결성을 보호하기 위해 **Write** 메서드는 [**OpenTransactedWriteAsync**](https://docs.microsoft.com/uwp/api/windows.storage.storagefile.opentransactedwriteasync)를 통해 파일을 열어 트랜잭션 모델을 사용합니다. 이 프로세스에서는 [**StorageStreamTransaction**](https://docs.microsoft.com/uwp/api/windows.storage.storagestreamtransaction) 개체를 만듭니다. 이 트랜잭션 개체가 만들어지면 API는 [**StorageStreamTransaction**](https://docs.microsoft.com/uwp/api/windows.storage.storagestreamtransaction) 문서의 [파일 액세스](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/FileAccess) 샘플 또는 코드 예제와 비슷한 방식에 따라 데이터를 씁니다.

다음 다이어그램에서는 성공적인 쓰기 작업에서 **WriteTextAsync** 메서드가 수행하는 기본 작업을 보여줍니다. 이 그림에서는 작업의 단순화된 보기를 제공합니다. 예를 들어, 텍스트 인코딩 및 다른 스레드의 비동기 완성과 같은 단계를 건너뜁니다.

![파일에 쓰기 위한 UWP API 호출 시퀀스 다이어그램](images/file-write-call-sequence.svg)

스트림을 사용하는 좀 더 복잡한 4단계 모델 대신, [**FileIO**](https://docs.microsoft.com/uwp/api/Windows.Storage.FileIO) 및 [**PathIO**](https://docs.microsoft.com/uwp/api/windows.storage.pathio) 클래스의 **Write** 메서드를 사용할 때의 이점은 다음과 같습니다.

* 오류를 비롯한 모든 중간 단계를 한 번의 API 호출로 처리합니다.
* 문제가 있는 경우 원본 파일은 그대로 유지됩니다.
* 시스템 상태가 가능한 한 깔끔하게 유지됩니다.

그러나 중간에 실패 지점이 많을 수 있으므로 오류 발생 가능성이 높아집니다. 오류가 발생하면 프로세스가 실패한 위치를 이해하기 어려울 수 있습니다. 다음 섹션에서는 **Write** 메서드를 사용할 때 발생할 수 있는 몇 가지 오류와 가능한 해결 방법을 제공합니다.

## <a name="common-error-codes-for-write-methods-of-the-fileio-and-pathio-classes"></a>FileIO 및 PathIO 클래스, Write 메서드의 일반적인 오류 코드

이 표에서는 앱 개발자가 **Write** 메서드를 사용할 때 발생하는 일반적인 오류 코드를 나타냅니다. 이 표의 단계는 이전 다이어그램에 나와 있는 단계에 해당합니다.

|  오류 이름(값)  |  단계  |  원인  |  솔루션  |
|----------------------|---------|----------|-------------|
|  ERROR_ACCESS_DENIED(0X80070005)  |  5  |  원본 파일은 이전 작업에서 삭제용으로 표시될 수 있습니다.  |  작업을 다시 시도합니다.</br>파일에 대한 액세스 권한이 동기화되었는지 확인합니다.  |
|  ERROR_SHARING_VIOLATION(0x80070020)  |  5  |  원본 파일을 다른 배타적 쓰기에서 열려 있습니다.   |  작업을 다시 시도합니다.</br>파일에 대한 액세스 권한이 동기화되었는지 확인합니다.  |
|  ERROR_UNABLE_TO_REMOVE_REPLACED(0x80070497)  |  19-20  |  원본 파일(file.txt)이 사용 중이므로 바꿀 수 없습니다. 파일을 바꾸기 전에 다른 프로세스 또는 작업에서 해당 파일에 대한 액세스 권한을 얻었습니다.  |  작업을 다시 시도합니다.</br>파일에 대한 액세스 권한이 동기화되었는지 확인합니다.  |
|  ERROR_DISK_FULL(0x80070070)  |  7, 14, 16, 20  |  트랜잭션 모델은 추가 파일을 만들며, 이로 인해 추가 스토리지가 사용됩니다.  |    |
|  ERROR_OUTOFMEMORY(0x8007000E)  |  14, 16  |  이 오류는 여러 개의 미해결 I/O 작업 또는 대형 파일 크기로 인해 발생할 수 있습니다.  |  보다 세분화된 방법으로 스트림을 제어하여 이 오류를 해결할 수 있습니다.  |
|  E_FAIL(0x80004005) |  임의  |  기타  |  작업을 다시 시도합니다. 여전히 실패할 경우 플랫폼 오류이며 앱이 일관되지 않은 상태이므로 종료해야 합니다. |

## <a name="other-considerations-for-file-states-that-might-lead-to-errors"></a>오류가 발생할 수 있는 파일 상태에 대한 기타 고려 사항

**Write** 메서드에서 반환되는 오류와 별도로, 앱이 파일에 쓸 때 예상할 수 있는 오류에 대한 몇 가지 지침은 다음과 같습니다.

### <a name="data-was-written-to-the-file-if-and-only-if-operation-completed"></a>작업이 완료된 경우에만 데이터가 파일에 기록됨

앱은 쓰기 작업이 진행 중인 동안 파일의 데이터에 대해 어떤 가정도 수행하지 않아야 합니다. 작업이 완료되기 전에 파일에 액세스하려고 하면 일관되지 않은 데이터가 나타날 수 있습니다. 앱은 미해결 I/O를 추적해야 합니다.

### <a name="readers"></a>Readers

쓰고 있는 파일이 처리 완료 후 판독기에서도 사용 중인 경우(즉, [**FileAccessMode.Read**](https://docs.microsoft.com/uwp/api/Windows.Storage.FileAccessMode)를 사용하여 연 경우) 후속 읽기는 ERROR_OPLOCK_HANDLE_CLOSED(0x80070323) 오류를 나타내며 실패합니다. 경우에 따라 앱은 **쓰기** 작업이 진행 중인 동안 다시 읽기 위해 파일을 열려고 합니다. 이 경우 파일을 바꿀 수 없기 때문에 원본 파일을 덮어쓰려고 할 때 **쓰기**가 궁극적으로 실패하는 경합 상태가 발생할 수 있습니다.

### <a name="files-from-knownfolders"></a>KnownFolders의 파일

사용자의 앱 외에 [**KnownFolders**](https://docs.microsoft.com/uwp/api/Windows.Storage.KnownFolders) 중 하나에 있는 파일에 액세스하려고 하는 앱이 또 있을 수 있습니다. 해당 작업이 성공할 경우 다음번에 파일을 읽으려고 할 때 앱이 파일에 쓴 콘텐츠가 동일하게 유지된다는 보장은 없습니다. 또한 이러한 시나리오에서는 공유 또는 액세스 거부 오류가 좀 더 일반적으로 나타날 수 있습니다.

### <a name="conflicting-io"></a>충돌하는 I/O

앱에서 로컬 데이터의 파일에 대해 **Write** 메서드를 사용하는 경우 동시성 오류의 발생 가능성을 낮출 수 있지만 여전히 주의가 필요합니다. 여러 **쓰기** 작업이 파일에 동시에 전송되는 경우 파일에 최종적으로 어떤 데이터가 포함될지 보장할 수 없습니다. 이 문제를 완화하려면 앱이 파일에 대한 **쓰기** 작업을 직렬화하는 것이 좋습니다.

### <a name="tmp-files"></a>~TMP 파일

경우에 따라 작업이 강제로 취소되면(예를 들어, 앱이 OS에 의해 일시 중단되거나 종료되는 경우) 트랜잭션은 적절히 커밋되거나 닫힙니다. 이 경우 (.~TMP) 확장명의 파일이 남을 수 있습니다. 앱 활성화를 처리하는 경우 이러한 임시 파일(앱의 로컬 데이터에 있는 경우)을 삭제하는 것이 좋습니다.

## <a name="considerations-based-on-file-types"></a>파일 형식에 따른 고려 사항

일부 오류는 파일 형식, 액세스 빈도 및 해당 파일 크기에 따라 좀 더 많이 발생할 수 있습니다. 일반적으로 앱에서 액세스할 수 있는 파일에는 다음 세가지 범주가 있습니다.

* 앱의 로컬 데이터 폴더에서 사용자가 만들어 편집한 파일. 이러한 파일은 앱을 사용하는 동안에만 생성 및 편집되며, 앱 내에서만 존재합니다.
* 앱 메타데이터. 앱은 이러한 파일을 사용하여 고유한 상태를 추적합니다.
* 앱이 액세스하기 위한 접근 권한 값을 선언한 파일 시스템 위치에 있는 기타 파일 이러한 파일은 가장 일반적으로 [**KnownFolders**](https://docs.microsoft.com/uwp/api/Windows.Storage.KnownFolders) 중 하나에 있습니다.

처음 두 범주의 파일은 앱 패키지 파일에 속하고 앱에서만 액세스할 수 있으므로 앱에 모든 권한이 있습니다. 마지막 범주에 속하는 파일의 경우, 다른 앱 및 OS 서비스가 파일에 동시에 액세스할 수 있다는 사실을 앱에서 인식해야 합니다.

앱에 따라, 파일에 대한 액세스는 다음과 같은 빈도에 따라 달라질 수 있습니다.

* 매우 낮음. 일반적으로 앱이 시작될 때 한 번 열리고, 앱이 일시 중단될 때 저장되는 파일입니다.
* 낮음. 사용자가 특수하게 작업(예: 저장 또는 로드)을 수행하는 파일입니다.
* 보통 또는 높음. 앱이 즉시 데이터를 업데이트해야 하는 파일입니다(예: 자동 저장 기능 또는 지속적인 메타데이터 추적).

파일 크기의 경우 **WriteBytesAsync** 메서드에 대한 다음 차트에서 성능 데이터를 고려하세요. 이 차트는 제어되는 환경에서 파일 크기당 평균 10,000개 작업을 기준으로, 작업을 완료하는 데 소요되는 시간과 파일 크기를 비교해서 보여 줍니다.

![WriteBytesAsync 성능](images/writebytesasync-performance.png)

y 축의 시간 값은 다른 하드웨어 및 구성이 다른 절대 시간 값을 생성하므로 이차트에서 의도적으로 생략됩니다. 그러나 테스트에서 다음과 같은 추세가 일관되게 확인되었습니다.

* 매우 작은 파일(<=1MB): 작업을 완료하기 위한 시간이 일관되게 빠릅니다.
* 더 큰 파일(>1MB): 작업을 완료하기 위한 시간이 기하급수적으로 증가하기 시작합니다.

## <a name="io-during-app-suspension"></a>앱 일시 중단 동안의 I/O

앱은 이후 세션에서 사용하기 위해 상태 정보나 메타데이터를 유지하려는 경우 일시 중단을 처리하도록 디자인해야 합니다. 앱 일시 중단에 대한 배경 정보는 [앱 수명 주기](../launch-resume/app-lifecycle.md) 및 [이 블로그 게시물](https://blogs.windows.com/buildingapps/2016/04/28/the-lifecycle-of-a-uwp-app/#qLwdmV5zfkAPMEco.97)을 참조하세요.

OS가 앱에 확장된 실행 권한을 부여하지 않는 한, 앱이 일시 중단되면 5초 안에 모든 리소스를 해제하고 해당 데이터를 저장합니다. 더 나은 안정성 및 사용자 환경을 위해서는 항상 일시 중단 작업을 처리해야 하는 시간이 제한된다고 가정합니다. 일시 중단 작업을 처리하기 위해 5초 동안 다음 지침에 유의하세요.

* 플러시 및 해제 작업으로 인해 야기되는 경합 상태를 방지하기 위해 I/O를 최소로 유지합니다.
* 쓰는 데 수백 밀리초가 필요한 파일은 쓰지 않도록 합니다.
* 앱에서 **Write** 메서드를 사용하는 경우 이러한 메서드에 필요한 모든 중간 단계에 유의합니다.

앱이 일시 중단 중에 소량의 상태 데이터에 작동하는 경우 대부분이 **Write** 메서드를 사용하여 데이터를 플러시할 수 있습니다. 그러나 앱이 대량의 상태 데이터를 사용하는 경우에는 스트림을 사용하여 데이터를 직접 저장하는 것이 좋습니다. 이렇게 하면 **Write** 메서드의 트랜잭션 모델로 인한 지연을 줄일 수 있습니다. 

예를 들어, [BasicSuspension](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/BasicSuspension) 샘플을 참조하세요.

## <a name="other-examples-and-resources"></a>기타 예제 및 리소스

다음은 특정 시나리오에 대한 몇 가지 예제 및 기타 리소스입니다.

### <a name="code-example-for-retrying-file-io-example"></a>파일 I/O 예제를 다시 시도하기 위한 코드 예제

다음은 쓰기를 다시 시도하는 방법에 대한 의사 코드 예제(C#)로, 사용자가 저장할 파일을 선택한 후에 쓰기가 완료되어야 한다고 가정합니다.

```csharp
Windows.Storage.Pickers.FileSavePicker savePicker = new Windows.Storage.Pickers.FileSavePicker();
savePicker.FileTypeChoices.Add("Plain Text", new List<string>() { ".txt" });
Windows.Storage.StorageFile file = await savePicker.PickSaveFileAsync();

Int32 retryAttempts = 5;

const Int32 ERROR_ACCESS_DENIED = unchecked((Int32)0x80070005);
const Int32 ERROR_SHARING_VIOLATION = unchecked((Int32)0x80070020);

if (file != null)
{
    // Application now has read/write access to the picked file.
    while (retryAttempts > 0)
    {
        try
        {
            retryAttempts--;
            await Windows.Storage.FileIO.WriteTextAsync(file, "Text to write to file");
            break;
        }
        catch (Exception ex) when ((ex.HResult == ERROR_ACCESS_DENIED) ||
                                   (ex.HResult == ERROR_SHARING_VIOLATION))
        {
            // This might be recovered by retrying, otherwise let the exception be raised.
            // The app can decide to wait before retrying.
        }
    }
}
else
{
    // The operation was cancelled in the picker dialog.
}
```

### <a name="synchronize-access-to-the-file"></a>파일에 대한 액세스 동기화

[.NET를 사용한 병렬 프로그래밍 블로그](https://devblogs.microsoft.com/pfxteam/)는 병렬 프로그래밍에 대한 지침을 제공하는 유용한 리소스입니다. 특히 [AsyncReaderWriterLock에 대한 게시물](https://devblogs.microsoft.com/pfxteam/building-async-coordination-primitives-part-7-asyncreaderwriterlock/)은 동시 읽기 액세스를 허용하면서 쓰기 위해 파일에 대한 배타적 액세스를 유지하는 방법을 설명합니다. I/O를 직렬화하면 성능에 영향을 줄 수 있다는 사실에 유의합니다.

## <a name="see-also"></a>참고 항목

* [파일 만들기, 쓰기 및 읽기](quickstart-reading-and-writing-files.md)
