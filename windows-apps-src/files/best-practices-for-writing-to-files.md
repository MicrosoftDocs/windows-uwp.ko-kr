---
title: 파일을 작성 하기 위한 모범 사례
description: FileIO 및 PathIO 클래스의 메서드를 작성 하는 다양 한 파일을 사용 하기 위한 모범 사례에 알아봅니다.
ms.date: 02/06/2019
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: f8bed97e060015f92ff95c9f7d797bbcb83db431
ms.sourcegitcommit: 079801609165bc7eb69670d771a05bffe236d483
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 02/27/2019
ms.locfileid: "9115714"
---
# <a name="best-practices-for-writing-to-files"></a>파일을 작성 하기 위한 모범 사례

**중요 API**

* [**FileIO 클래스**](https://docs.microsoft.com/uwp/api/Windows.Storage.FileIO)
* [**PathIO 클래스**](https://docs.microsoft.com/uwp/api/windows.storage.pathio)

때로는 개발자가 **작성** [**FileIO**](https://docs.microsoft.com/uwp/api/Windows.Storage.FileIO) 및 [**PathIO**](https://docs.microsoft.com/uwp/api/windows.storage.pathio) 클래스의 메서드를 사용 하 여 파일 시스템 I/O 작업을 수행 하는 일련의 일반적인 문제에 실행 합니다. 예를 들어, 일반적인 문제 다음과 같습니다.

• 파일은 부분적으로 • 앱 예외가 발생 하는 방법 중 하나를 호출할 때 기록 됩니다. • 작업 뒤에 둡니다. 대상 파일 이름에 유사한 파일 이름 가진 TMP 파일입니다.

[**FileIO**](https://docs.microsoft.com/uwp/api/Windows.Storage.FileIO) 및 [**PathIO**](https://docs.microsoft.com/uwp/api/windows.storage.pathio) 클래스 **를 작성** 하는 방법은 다음과 같습니다.

* **WriteBufferAsync**
* **WriteBytesAsync**
* **WriteLinesAsync**
* **WriteTextAsync**

 이 문서에서는 이러한 메서드의 하므로 개발자가 작동 하는 방식에 대해 자세히 설명 시기 및 그 사용 방법을 더 잘 이해를 제공 합니다. 이 문서에서는 지침을 제공 하며 모든 가능한 파일 I/O 문제에 대 한 솔루션을 제공 하려고 시도 하지 않습니다. 

> [!NOTE]
> 이 문서에서는 예제와 토론에서 **FileIO** 방법에 설명 합니다. **하지만 PathIO** 메서드 비슷한 패턴 따릅니다 대부분의이 문서의 지침 너무 메서드가에 적용 합니다. 

## <a name="conveience-vs-control"></a>컨트롤 및 Conveience

[**StorageFile**](https://docs.microsoft.com/uwp/api/windows.storage.storagefile) 개체는 기본 Win32 프로그래밍 모델 같은 파일 핸들을 아닙니다. 대신, [**StorageFile**](https://docs.microsoft.com/uwp/api/windows.storage.storagefile) 해당 콘텐츠를 조작할 수 있는 메서드를 사용 하 여 파일 표현입니다.

이 개념을 이해 **StorageFile**을 사용 하 여 I/O를 수행 하는 경우 유용 합니다. 예를 들어 [파일에 쓰](quickstart-reading-and-writing-files.md#writing-to-a-file) 는 섹션 파일에 작성 하는 세 가지 방법을 제공 합니다.

* [**FileIO.WriteTextAsync**](https://docs.microsoft.com/uwp/api/windows.storage.fileio.writetextasync) 메서드를 사용합니다.
* 버퍼를 만들고 [**FileIO.WriteBufferAsync**](https://docs.microsoft.com/en-us/uwp/api/windows.storage.fileio.writebufferasync) 메서드를 호출 합니다.
* 스트림을 사용 하는 4 단계 모델:
  1. [열린](https://docs.microsoft.com/uwp/api/windows.storage.storagefile.openasync) 파일 스트림을 가져옵니다.
  2. [가져오기](https://docs.microsoft.com/uwp/api/windows.storage.streams.irandomaccessstream.getoutputstreamat) 출력 스트림을 합니다.
  3. [**DataWriter**](https://docs.microsoft.com/uwp/api/windows.storage.streams.datawriter) 개체를 만들고 해당 **작성** 메서드를 호출 합니다.
  4. [커밋](https://docs.microsoft.com/uwp/api/windows.storage.streams.datawriter.storeasync) 의 데이터 데이터 작성기와 출력 스트림 [을 플러시합니다](https://docs.microsoft.com/uwp/api/windows.storage.streams.ioutputstream.flushasync) .

처음 두 시나리오는 앱에서 가장 일반적으로 사용 되는 메시지입니다. 단일 작업에서 파일에 쓰는 코드 및 유지 관리 하기가 쉽습니다. 및 파일 I/O의 복잡성을 크게 줄일 다루는에서 한 앱의 책임을 제거 합니다. 이 편리는 비용이 수반 되는 반면: 전체 작업 및 특정 지점에서 오류를 검색 하는 기능에 대 한 제어 손실 됩니다.

## <a name="the-transactional-model"></a>트랜잭션 모델

[**FileIO**](https://docs.microsoft.com/uwp/api/Windows.Storage.FileIO) 및 [**PathIO**](https://docs.microsoft.com/uwp/api/windows.storage.pathio) 클래스의 **쓰기** 메서드 추가 계층을 사용 하 여 위에서 설명한 세 번째 쓰기 모델에서 단계를 래핑합니다. 이 계층은 저장소 거래에서 캡슐화 됩니다.

데이터를 기록 하는 동안 문제가 발생 하는 경우 원본 파일의 무결성을 보호, **Write** 메서드 [**OpenTransactedWriteAsync**](https://docs.microsoft.com/uwp/api/windows.storage.storagefile.opentransactedwriteasync)를 사용 하 여 파일을 열어 트랜잭션 모델을 사용 합니다. 이 프로세스는 [**StorageStreamTransaction**](https://docs.microsoft.com/uwp/api/windows.storage.storagestreamtransaction) 개체를 만듭니다. 이 거래 개체를 만든 후 Api 유사한 방식으로 [파일 액세스](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/FileAccess) 샘플 또는 [**StorageStreamTransaction**](https://docs.microsoft.com/uwp/api/windows.storage.storagestreamtransaction) 문서의 코드 예제에서는 다음 데이터를 작성 합니다.

다음 다이어그램은 하 여 수행 하는 기본 작업의 성공적인 쓰기 작업에서 **WriteTextAsync** 메서드. 이 그림 작업의 단순화 된 보기를 제공합니다. 예를 들어, 서로 다른 스레드에서 텍스트 인코딩 및 비동기 완료와 같은 단계를 건너뜁니다.

![파일에 쓰기에 대 한 UWP API 호출 시퀀스 다이어그램](images/file-write-call-sequence.svg)

스트림을 사용 하 여 더 복잡 한 4 단계로 모델 대신 **쓰기** [**FileIO**](https://docs.microsoft.com/uwp/api/Windows.Storage.FileIO) 및 [**PathIO**](https://docs.microsoft.com/uwp/api/windows.storage.pathio) 클래스의 메서드를 사용 하는 이점은 다음과 같습니다.

* 오류를 포함 하 여 모든 중간 단계를 처리를 한 번 API 호출 합니다.
* 오류가 발생 한 경우 원본 파일 유지 됩니다.
* 시스템 상태는 가능한 한 깔끔하게 정리 유지 하려고 합니다.

그러나 많은 가능한 중간 지점을 사용 실패는 오류가 발생할 가능성이 합니다. 오류 발생 시 프로세스에 실패 한 위치를 이해 하기 어려울 수 있습니다. 다음 섹션에서는 **Write** 메서드를 사용 하 여 할 때 발생 하 고 가능한 솔루션을 제공 하는 오류 몇 가지 제공 합니다.

## <a name="common-error-codes-for-write-methods-of-the-fileio-and-pathio-classes"></a>FileIO 및 PathIO 클래스의 쓰기 메서드에 대 한 일반적인 오류 코드

이 표는 앱 개발자가 **작성** 방법을 사용 하 여 때 발생 하는 일반적인 오류 코드를 제공 합니다. 위 다이어그램의 단계에 해당 하는 테이블의 단계.

|  오류 이름 (값)  |  단계  |  원인  |  해결 방법  |
|----------------------|---------|----------|-------------|
|  ERROR_ACCESS_DENIED (0X80070005)  |  5  |  원본 파일 이전 작업에서 가능한 경우 삭제 표시 될 수 있습니다.  |  작업을 다시 시도 합니다.</br>파일에 대 한 동기화 되는지 확인 합니다.  |
|  ERROR_SHARING_VIOLATION (0X80070020)  |  5  |  원본 파일은 다른 단독 쓰기 열립니다.   |  작업을 다시 시도 합니다.</br>파일에 대 한 동기화 되는지 확인 합니다.  |
|  ERROR_UNABLE_TO_REMOVE_REPLACED (0X80070497)  |  19-20  |  사용 중이기 때문에 원본 파일 (file.txt)를 대체할 수 있습니다. 다른 프로세스 또는 작업 파일에 대 한 액세스를 확보 전에 대체할 수 있습니다.  |  작업을 다시 시도 합니다.</br>파일에 대 한 동기화 되는지 확인 합니다.  |
|  ERROR_DISK_FULL (0X80070070)  |  7, 14, 16, 20  |  트랜잭션된 모델 추가 파일을 만들고이 추가 저장소를 사용 합니다.  |    |
|  ERROR_OUTOFMEMORY (0X8007000E)  |  14, 16  |  여러 처리 중인 I/O 작업 또는 파일 크기가 커질이 발생할 수 있습니다.  |  스트림을 제어 하 여 보다 세밀 하 게 접근 오류를 해결할 수 있습니다.  |
|  E_FAIL (0X80004005) |  임의  |  기타  |  작업을 다시 시도 합니다. 문제가 계속 발생 하는 경우 플랫폼 오류 수 및 불일치 상태인 이기 때문에 앱을 종료 해야 합니다. |

## <a name="other-considerations-for-file-states-that-might-lead-to-errors"></a>오류를 유발할 수 있는 파일 상태에 대 한 기타 고려 사항

**쓰기** 메서드에 의해 반환 되는 오류를 제외한 일부의 지침은 앱 파일에 쓸 때 기대할 수에 있습니다.

### <a name="data-was-written-to-the-file-if-and-only-if-operation-completed"></a>작업이 완료 하는 경우에 데이터 파일에 기록 된

쓰기 작업이 진행 중인 동안 앱 파일의 데이터에 대 한 모든 가정을 확인 하지 해야 합니다. 작업이 완료 되기 전에 파일에 액세스 하려고 하면 데이터 발생할 수 있습니다. 앱의 뛰어난 I/o를 추적 해야 합니다.

### <a name="readers"></a>판독기

파일에 기록 되 고 이기도 정중 리더에서 사용 중인 경우 (즉, [**FileAccessMode.Read**](https://docs.microsoft.com/uwp/api/Windows.Storage.FileAccessMode)으로 열고, 읽기가 실패 ERROR_OPLOCK_HANDLE_CLOSED (0x80070323) 오류가 발생 합니다. 경우에 따라 앱 파일을 열고 읽기에 대 한 다시 **쓰기** 작업이 진행 중인 동안 다시 시도 합니다. 이 경합 상태는 **작성** 궁극적으로 실패 대체할 수 없는 때문에 원본 파일을 덮어쓸지 하려고 할 때 발생할 수 있습니다.

### <a name="files-from-knownfolders"></a>KnownFolders의 파일

앱은 [**KnownFolders**](https://docs.microsoft.com/uwp/api/Windows.Storage.KnownFolders)중 하나에 있는 파일에 액세스 하려고 하는 유일한 앱 아닐 수도 있습니다. 다음에 파일을 읽는 시도 작업이 성공적 이면 파일에 작성 앱 콘텐츠 일정 하 게 유지 됩니다 아닙니다. 또한 공유 또는 액세스 거부 오류가이 시나리오에서 더 많이 있습니다.

### <a name="conflicting-io"></a>충돌 하는 I/O

앱의 로컬 데이터를 파일에 대 한 **쓰기** 메서드를 사용 하지만 일부 주의 경우의 동시성 오류 가능성을 낮출 수 있습니다. 파일에 **쓰기** 작업을 여러 개를 동시에 전송 되는, 경우에 어떤 데이터는 파일에서 결국에 대 한 아닙니다. 이러한 문제를 해결 하려면 앱 파일에 **쓰기** 작업 바이너리로 직렬화 하는 것이 좋습니다.

### <a name="tmp-files"></a>~ TMP 파일

경우에 따라 작업 (예를 들어 경우 앱 일시 중단 되거나 OS가 종료) 강제로 취소, 거래 하지 커밋 또는 적절 하 게 종료 합니다. 이 파일을 두고 수는 (. ~ TMP) 확장 합니다. (앱의 로컬 데이터에 있는) 하는 경우 이러한 임시 파일을 삭제 하는 것이 좋습니다. 앱 활성화를 처리 하는 경우.

## <a name="considerations-based-on-file-types"></a>파일 형식에 따라 고려 사항

일부 오류는 파일, 액세스 하는 빈도 및 파일 크기의 유형에 따라 많이 될 수 있습니다. 일반적으로 가지 앱이 액세스할 수 있는의 세 가지 범주가 있습니다.

* 파일 생성 및 앱의 로컬 데이터 폴더에 사용자가 편집 합니다. 만들고 앱을 사용 하는 동안에 편집 하 고 앱 내에 존재 합니다.
* 앱 메타 데이터입니다. 앱 자체의 상태를 추적 하기 위해 이러한 파일을 사용 합니다.
* 앱에 액세스 하는 기능을 선언 하는 위치에서 파일 시스템 위치에 있는 다른 파일. [**KnownFolders**](https://docs.microsoft.com/uwp/api/Windows.Storage.KnownFolders)중 하나에 있는 가장 일반적으로 이러한 합니다.

앱의 패키지 파일의 일부가 앱 단독으로 액세스 하기 때문에 앱이 파일의 처음 두 개의 범주에 전체 제어 합니다. 파일의 마지막 항목에 대 한 앱은 다른 앱 및 OS 서비스를 액세스 하는 파일 동시 알고 있어야 합니다.

앱에 따라 파일에 대 한 액세스 주파수 다를 수 있습니다.

* 매우 낮은 합니다. 이들은 일반적으로 앱이 일시 중단 된 앱 실행 및가 저장 하는 경우 열려 있는 파일입니다.
* 부족 합니다. 이들은 사용자 (예: 저장 하거나 로드)에서 작업을 수행한 구체적으로 파일입니다.
* 중간 또는 높은 합니다. 이들은 파일 앱 데이터 (예를 들어 자동 기능 또는 상수 메타 데이터 추적) 업데이트 지속적으로 해야 합니다.

파일 크기에 대 한 성능 데이터 **WriteBytesAsync** 메서드에 대 한 다음 차트를 고려 합니다. 이 차트는 평균 성능 제어 된 환경에서 파일 크기 당 10000 작업을 통해 작업 vs 파일 크기를 완료 하는 데 시간을 비교 합니다.

![WriteBytesAsync 성능](images/writebytesasync-performance.png)

Y 축에 시간 값은 다양 한 하드웨어 및 구성을 다른 절대 시간 값 얻을 수 있으므로이 차트에서 의도적으로 생략 됩니다. 그러나이 테스트에서 이러한 추세 관찰 일관 되 게가:

* 매우 작은 파일에 대 한 (< 1MB =): 작업을 완료 하는 일관 되 게 합니다.
* 더 큰 파일에 대 한 (gt_ 1MB): 작업을 완료 하는 데 시간을 기하급수적으로 증가 시작 합니다.

## <a name="io-during-app-suspension"></a>앱 일시 중단 된 동안 I/O

앱이 세션에서 나중에 사용 하기 위해 메타 데이터 또는 상태 정보를 유지 하려는 경우 일시 중단을 처리 하도록 설계 해야 합니다. 앱 일시 중단에 대 한 배경 정보를 [응용 프로그램 수명 주기](../launch-resume/app-lifecycle.md) 및 [이 블로그 게시물](https://blogs.windows.com/buildingapps/2016/04/28/the-lifecycle-of-a-uwp-app/#qLwdmV5zfkAPMEco.97)을 참조 하세요.

OS 확장된 실행을 앱에 부여 하지 않는 한 앱이 일시 중단 된 경우 5 초가 필요 하는 모든 리소스를 해제 하 고 데이터를 저장 합니다. 최상의 안정성 및 사용자 환경, 항상 일시 중단 작업을 처리할 수 있는 시간이 제한 되어 있다고 가정 합니다. 5 초 일시 중단 작업을 처리 하기 위한 기간 동안 다음 지침을 염두에 유지:

* I/O 플러시 및 릴리스 작업에서 발생 하는 경합 상태를 방지 하는 최소한으로 유지 하려고 합니다.
* 수백 쓰려고 밀리초 이상 필요한 파일을 작성 하지 마세요.
* 앱 **Write** 메서드를 사용 하는 경우 염두에 이러한 메서드를 필요로 하는 중간 단계입니다.

앱 일시 중단 된 동안 소량의 상태 데이터에서 작동 하는 경우를 플러시합니다 데이터 **쓰기** 메서드를 사용 하 대부분의 경우 수 있습니다. 그러나 앱 많은 양의 상태 데이터를 사용 하는 경우 스트림을 사용 하 여 직접 데이터를 저장 하는 것이 좋습니다. **쓰기** 메서드의 트랜잭션 모델에 의해 정의 된 지연 단축 하는 데 도움이 됩니다. 

예를 들어 [BasicSuspension](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/BasicSuspension) 샘플을 참조 하세요.

## <a name="other-examples-and-resources"></a>다른 예제 및 리소스

다음은 몇 가지 예제 및 특정 시나리오에 대 한 다른 리소스입니다.

### <a name="code-example-for-retrying-file-io-example"></a>파일 I/O 예제를 다시 시도 대 한 코드 예제

다음은 쓰기 후 사용자가 나중에 파일을 선택 하는 것을 가정 하는 쓰기 (C#)를 다시 시도 하는 방법에 의사 코드 예제입니다.

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

### <a name="synchronize-access-to-the-file"></a>파일에 대 한 액세스를 동기화

[.NET 블로그를 사용 하 여 병렬 프로그래밍](https://blogs.msdn.microsoft.com/pfxteam/) 병렬 프로그래밍에 대 한 지침에 대 한 좋은 리소스입니다. 특히, [AsyncReaderWriterLock에 대해 게시할](https://blogs.msdn.microsoft.com/pfxteam/2012/02/12/building-async-coordination-primitives-part-7-asyncreaderwriterlock/) 동시에 읽기 액세스를 허용 하면서 쓰기에 대 한 파일에 대 한 단독 액세스를 관리 하는 방법을 설명 합니다. 염두에 해당 직렬화 I/O 영향을 미칩니다 성능.

## <a name="see-also"></a>참고 항목

* [파일 만들기, 쓰기 및 읽기](quickstart-reading-and-writing-files.md)
