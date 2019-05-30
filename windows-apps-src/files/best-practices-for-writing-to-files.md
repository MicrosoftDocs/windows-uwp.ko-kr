---
title: 파일 쓰기 모범 사례
description: FileIO 및 PathIO 클래스의 메서드를 작성 하는 다양 한 파일을 사용 하 여에 대 한 모범 사례에 알아봅니다.
ms.date: 02/06/2019
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: a6a1d93b1deaad084ff25db946199b678b35703c
ms.sourcegitcommit: ac7f3422f8d83618f9b6b5615a37f8e5c115b3c4
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 05/29/2019
ms.locfileid: "66369517"
---
# <a name="best-practices-for-writing-to-files"></a>파일 쓰기 모범 사례

**중요 한 Api**

* [**FileIO 클래스**](https://docs.microsoft.com/uwp/api/Windows.Storage.FileIO)
* [**PathIO 클래스**](https://docs.microsoft.com/uwp/api/windows.storage.pathio)

경우에 따라 사용 하는 경우 일반적인 문제의 집합으로 실행 하는 개발자는 **작성** 의 메서드는 [ **FileIO** ](https://docs.microsoft.com/uwp/api/Windows.Storage.FileIO) 고 [ **PathIO** ](https://docs.microsoft.com/uwp/api/windows.storage.pathio) 파일 시스템 I/O 작업을 수행 하는 클래스입니다. 예를 들어, 일반적인 문제 다음과 같습니다.

* 파일을 부분적으로 기록 됩니다.
* 메서드 중 하나를 호출 하는 경우 앱에서 예외가 발생 합니다.
* 작업에 둡니다. 대상 파일 이름에 비슷한 파일 이름의 TMP 파일입니다.

**작성** 의 메서드는 [ **FileIO** ](https://docs.microsoft.com/uwp/api/Windows.Storage.FileIO) 및 [ **PathIO** ](https://docs.microsoft.com/uwp/api/windows.storage.pathio) 클래스는 다음과 같습니다.

* **WriteBufferAsync**
* **WriteBytesAsync**
* **WriteLinesAsync**
* **WriteTextAsync**

 이 문서에서는 이러한 메서드의 하므로 개발자가 작동 하는 방식을 자세히 시기 및 사용 하는 방법을 더 잘 이해를 제공 합니다. 이 문서는 지침을 제공 하 고 모든 가능한 파일 I/O 문제에 대 한 솔루션을 제공 하지 않습니다. 

> [!NOTE]
> 이 문서에 중점을 둡니다 합니다 **FileIO** 메서드 예제 및 토론입니다. 그러나 합니다 **PathIO** 메서드는 유사한 패턴을 따르며이 문서의 지침은 대부분 너무 해당 메서드에 적용 됩니다. 

## <a name="convenience-vs-control"></a>편의 컨트롤 비교

A [ **StorageFile** ](https://docs.microsoft.com/uwp/api/windows.storage.storagefile) 개체가 네이티브 Win32 프로그래밍 모델과 같이 파일 핸들을 아닙니다. 대신에 [ **StorageFile** ](https://docs.microsoft.com/uwp/api/windows.storage.storagefile) 해당 콘텐츠를 조작 하는 메서드를 사용 하 여 파일의 표현입니다.

이 개념을 이해 유용 사용 하 여 I/O를 수행 하는 경우는 **StorageFile**합니다. 예를 들어를 [파일에 쓸](quickstart-reading-and-writing-files.md#writing-to-a-file) 섹션에서는 파일에 쓸 수 있는 세 가지 방법으로 제공 합니다.

* 사용 하 여 [ **FileIO.WriteTextAsync** ](https://docs.microsoft.com/uwp/api/windows.storage.fileio.writetextasync) 메서드.
* 버퍼를 만들고 호출 하 여 합니다 [ **FileIO.WriteBufferAsync** ](https://docs.microsoft.com/en-us/uwp/api/windows.storage.fileio.writebufferasync) 메서드.
* 스트림을 사용 하 여 4 단계로 모델:
  1. [열기](https://docs.microsoft.com/uwp/api/windows.storage.storagefile.openasync) 스트림을 가져오기 위해 합니다.
  2. [가져올](https://docs.microsoft.com/uwp/api/windows.storage.streams.irandomaccessstream.getoutputstreamat) 는 출력 스트림입니다.
  3. 만들기는 [ **DataWriter** ](https://docs.microsoft.com/uwp/api/windows.storage.streams.datawriter) 개체와 해당 호출 **작성** 메서드.
  4. [커밋](https://docs.microsoft.com/uwp/api/windows.storage.streams.datawriter.storeasync) 의 데이터 기록기에서 데이터 및 [플러시](https://docs.microsoft.com/uwp/api/windows.storage.streams.ioutputstream.flushasync) 출력 스트림에 합니다.

처음 두 시나리오는 앱에서 가장 일반적으로 사용 하는 메시지입니다. 단일 작업에서 파일에 쓰지 코딩 및 유지 관리 하는 작업을 쉽게 이며 다양 한 파일 I/O의 복잡성 처리에서 한 앱의 책임을 제거 합니다. 그러나 이러한 편리 함 비용: 전체 작업 및 특정 지점에서 오류를 포착 하는 기능에 대 한 제어 손실 합니다.

## <a name="the-transactional-model"></a>트랜잭션 모델

**작성** 의 메서드는 [ **FileIO** ](https://docs.microsoft.com/uwp/api/Windows.Storage.FileIO) 및 [ **PathIO** ](https://docs.microsoft.com/uwp/api/windows.storage.pathio) 클래스 세 번째 쓰기 단계 래핑 추가 계층을 사용 하 여 위에 설명 된 모델입니다. 이 계층은 저장소 트랜잭션이에 캡슐화 됩니다.

데이터를 작성 하는 동안 문제가 발생 하는 경우 원본 파일의 무결성을 보호 하는 **작성할** 사용 하 여 파일을 열어 트랜잭션 모델을 사용 하는 방법 [ **OpenTransactedWriteAsync** ](https://docs.microsoft.com/uwp/api/windows.storage.storagefile.opentransactedwriteasync). 이 프로세스에서는 [ **StorageStreamTransaction** ](https://docs.microsoft.com/uwp/api/windows.storage.storagestreamtransaction) 개체입니다. Api를 비슷한 방식으로 다음 데이터를 쓰려면이 트랜잭션 개체를 만든 후 합니다 [파일 액세스](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/FileAccess) 샘플 또는 코드 예제는 [ **StorageStreamTransaction** ](https://docs.microsoft.com/uwp/api/windows.storage.storagestreamtransaction) 문서.

다음 다이어그램에서 수행 하는 기본 작업을 합니다 **WriteTextAsync** 성공적인 쓰기 작업에서 메서드. 이 그림에서는 작업의 단순화 된 보기를 제공합니다. 예를 들어 다른 스레드에서 텍스트 인코딩 및 비동기 완성과 같은 단계를 건너뜁니다.

![파일에 쓰기 위한 UWP API 호출 시퀀스 다이어그램](images/file-write-call-sequence.svg)

사용 하는 이점은 합니다 **작성** 의 메서드는 [ **FileIO** ](https://docs.microsoft.com/uwp/api/Windows.Storage.FileIO) 및 [ **PathIO** ](https://docs.microsoft.com/uwp/api/windows.storage.pathio) 대신 클래스 스트림을 사용 하 여 더 복잡 한 4 단계로 모델의 다음과 같습니다.

* 오류를 포함 하 여, 모든 중간 단계 처리를 하나의 API 호출입니다.
* 원본 파일에 문제가 있는 경우 유지 됩니다.
* 시스템 상태는 가능한 한 깔끔하게 정리 유지 하려고 합니다.

그러나 많은 가능한 중간 지점의 오류를 사용 하 여 방법이 오류의 가능성이 높아집니다. 오류가 발생 하면 프로세스가 실패 한 위치를 이해 하기 어려울 수 있습니다. 다음 섹션에서는 사용 하 여 때 발생할 수 있는 오류 중 일부를 제공 합니다 **쓰기** 메서드 가능한 솔루션을 제공 합니다.

## <a name="common-error-codes-for-write-methods-of-the-fileio-and-pathio-classes"></a>FileIO 및 PathIO 클래스의 쓰기 방법에 대 한 일반적인 오류 코드

이 테이블에서는 앱 개발자 들이 사용 하는 경우 일반적인 오류 코드를 표시 합니다 **쓰기** 메서드. 테이블의 단계는 이전 다이어그램에 나와 있는 단계에 해당 합니다.

|  오류 이름 (값)  |  단계  |  원인  |  해결 방법  |
|----------------------|---------|----------|-------------|
|  ERROR_ACCESS_DENIED (0X80070005)  |  5  |  원본 파일은 이전 작업에서 삭제 표시 될 수 있습니다.  |  작업을 다시 시도합니다.</br>파일에 대 한 액세스가 동기화를 확인 합니다.  |
|  ERROR_SHARING_VIOLATION (0X80070020)  |  5  |  원본 파일을 다른 배타적 쓰기 열려 있습니다.   |  작업을 다시 시도합니다.</br>파일에 대 한 액세스가 동기화를 확인 합니다.  |
|  ERROR_UNABLE_TO_REMOVE_REPLACED (0X80070497)  |  19-20  |  사용 중이기 때문에 원본 파일 (file.txt)을 대체할 수 있습니다. 다른 프로세스 또는 작업 바꿀 수 전에 파일에 대 한 액세스를 얻은 합니다.  |  작업을 다시 시도합니다.</br>파일에 대 한 액세스가 동기화를 확인 합니다.  |
|  ERROR_DISK_FULL (0x80070070)  |  7, 14, 16, 20  |  트랜잭션된 모델 추가 파일을 만들고이 추가 저장소를 사용 합니다.  |    |
|  ERROR_OUTOFMEMORY (0x8007000E)  |  14, 16  |  이 여러 개의 미해결 I/O 작업 또는 파일 크기가 커질으로 인해 발생할 수 있습니다.  |  스트림을 제어 하 여 보다 세분화 된 방법은 오류를 해결할 수 있습니다.  |
|  E_FAIL (0X80004005) |  임의의 값  |  기타  |  작업을 다시 시도합니다. 그래도 실패할 경우 플랫폼 오류를 수 있습니다 하 고 일관성이 없는 상태에 있기 때문에 앱을 종료 해야 합니다. |

## <a name="other-considerations-for-file-states-that-might-lead-to-errors"></a>오류가 발생할 수 있는 파일 상태에 대 한 기타 고려 사항

반환한 오류 외에도 합니다 **쓰기** 메서드 앱 파일에 쓸 때 기대할 수 있는 한 몇 가지 지침은 다음과 같습니다.

### <a name="data-was-written-to-the-file-if-and-only-if-operation-completed"></a>작업을 완료 하는 경우에 데이터를 파일에 기록 된

앱은 가정을 하지 데이터에 대 한 파일에 쓰기 작업이 진행 중인 동안. 일관 되지 않은 데이터는 작업이 완료 되기 전에 파일에 액세스 하는 동안 발생할 수 있습니다. 앱 책임 스레드당 미해결 I/o를 추적 해야 합니다.

### <a name="readers"></a>Readers

파일에 기록 되 고도 처리 완료 후 판독기에서 사용 중인 경우 (즉, 사용 하 여 연 [ **FileAccessMode.Read**](https://docs.microsoft.com/uwp/api/Windows.Storage.FileAccessMode), 후속 읽기 ERROR_OPLOCK_HANDLE_CLOSED (0x80070323) 오류와 함께 실패 합니다. 경우에 따라 앱을 다시 시도 하세요 다시 하는 동안 읽기에 대 한 파일을 여는 **쓰기** 작업이 진행 중입니다. 이 발생할 수 있는 경합 합니다 **쓰기** 궁극적으로 바꿀 수 없습니다 때문에 원래 파일을 덮어쓸 하려고 할 때 실패 합니다.

### <a name="files-from-knownfolders"></a>KnownFolders 파일

앱 중 하나에 있는 파일에 액세스 하는 앱만을 사용 하지 않을 합니다 [ **KnownFolders**](https://docs.microsoft.com/uwp/api/Windows.Storage.KnownFolders)합니다. 다음에 파일을 읽으려고은 작업이 성공 하면 앱이 파일에 쓴 내용을 상수 남아 아닙니다. 또한 공유 또는 액세스가이 시나리오에서 더 보편화 하는 오류를 거부 합니다.

### <a name="conflicting-io"></a>충돌 하는 I/O

앱에서 사용 하는 경우 동시성 오류 가능성을 낮출 수 있습니다 합니다 **쓰기** 때는 주의 해야 하지만 해당 로컬 데이터 파일에 대 한 메서드는 여전히 필요 합니다. 여러 **쓰기** operations에 전송 된 동시에 파일을 데이터 파일에서 종료 하는 방법에 대 한 보장이 합니다. 이 문제를 완화 하는 앱 serialize 권장 **쓰기** 파일로 작업 합니다.

### <a name="tmp-files"></a>~ TMP 파일

경우에 따라 (예를 들어, 앱 일시 중단 되었거나 경우 OS가 종료)에 강제로 작업이 취소 됩니다, 경우 트랜잭션이 커밋되거나 적절 하 게 닫힙니다. 이 사용 하 여 파일을 남을 수는 (. ~ TMP) 확장 합니다. (앱의 로컬 데이터에 있는) 하는 경우 이러한 임시 파일을 삭제 하는 것이 좋습니다. 앱 활성화를 처리 하는 경우.

## <a name="considerations-based-on-file-types"></a>파일 형식을 기반으로 하는 고려 사항

일부 오류는 파일, 액세스 하는 빈도 및 해당 파일 크기의 형식에 따라 더 많이 사용 될 수 있습니다. 일반적으로 범주 파일을 앱에 액세스할 수에 세 가지가 있습니다.

* 파일 생성 및 앱의 로컬 데이터 폴더에서 사용자가 편집 합니다. 이러한 생성 되 고 앱을 사용 하는 동안에 편집 및 앱 내 에서만 존재 합니다.
* 앱 메타 데이터입니다. 앱 자체의 상태를 추적 하기 위해 이러한 파일을 사용 합니다.
* 앱이 액세스 하는 기능을 선언 하는 위치는 파일 시스템의 위치에서 다른 파일입니다. 중 하나에 있는 가장 일반적으로 이러한 합니다 [ **KnownFolders**](https://docs.microsoft.com/uwp/api/Windows.Storage.KnownFolders)합니다.

앱에 앱의 패키지 파일의 일부가 앱에서 단독으로 액세스할 수 있기 때문에 파일의 처음 두 가지 범주에 대 한 모든 권한이 있습니다. 마지막 범주에서 파일에 대 한 앱에는 다른 앱 및 OS 서비스를 액세스 하는 파일 동시에 인식 해야 합니다.

앱에 따라 파일에 대 한 액세스 빈도에 달라질 수 있습니다.

* 매우 부족 합니다. 이들은 일반적으로 앱 시작 되며 앱 일시 중단 되 면 저장 하는 경우 열려 있는 파일입니다.
* 부족 합니다. 이들은 사용자 (예: 저장 또는 로드)에서 액션을 구체적으로 수행 되는 파일입니다.
* 보통 또는 높음. 이 앱 (예를 들어, 자동 저장 기능 또는 상수 메타 데이터를 추적) 데이터를 지속적으로 업데이트 해야 합니다는 파일입니다.

파일 크기에 대 한 다음 차트에 대 한 성능 데이터를 고려 합니다 **WriteBytesAsync** 메서드. 이 차트는 평균 성능 통제 된 환경에서 파일 크기당 10000 작업에 비해 작업 및 파일 크기를 완료 시간은 경우에 비교 합니다.

![WriteBytesAsync 성능](images/writebytesasync-performance.png)

다른 하드웨어 구성과 다른 절대 시간 값을 생성 하는 y 축에 시간 값이이 차트에서 의도적으로 생략 됩니다. 그러나 테스트에서 이러한 추세 관찰 일관 되 게 했습니다.

* 매우 작은 파일 (< = 1MB). 작업을 완료 하려면 시간이 일관 되 게 빠릅니다.
* 더 큰 파일 (> 1 MB): 작업을 완료 하려면 시간이 기하급수적으로 증가 하기 시작 합니다.

## <a name="io-during-app-suspension"></a>응용 프로그램 일시 중단 하는 동안 I/O

앱 세션에서 나중에 사용할 상태 정보나 메타 데이터를 유지 하려는 경우 일시 중단을 처리 하도록 디자인 해야 합니다. 응용 프로그램 일시 중단 하는 방법에 대 한 배경 정보를 참조 하세요 [앱 수명 주기](../launch-resume/app-lifecycle.md) 하 고 [이 블로그 게시물](https://blogs.windows.com/buildingapps/2016/04/28/the-lifecycle-of-a-uwp-app/#qLwdmV5zfkAPMEco.97)합니다.

OS 확장된 실행을 앱에 부여 하지 않는 한 모든 리소스를 해제 하 고 해당 데이터를 저장 하는 데 5 초에 앱이 일시 중지 합니다. 안정성 및 사용자에 대 한 환경, 항상 일시 중단 작업을 처리 해야 하는 시간이 제한 된 것으로 가정 합니다. 다음 지침에서 염두 일시 중단 작업을 처리 하기 위한 기간을 5 초로:

* 플러시 및 릴리스 작업에서 발생 한 경합을 방지 하려면 최소 I/O를 유지 하려고 합니다.
* 수백 개의 작성 하는 데 시간 (밀리초) 이상 필요한 파일을 작성 하지 마세요.
* 앱에서 사용 하는 경우는 **쓰기** 메서드를 염두에 이러한 메서드를 필요로 하는 모든 중간 단계입니다.

앱 일시 중단 하는 동안 약간의 상태 데이터에 작동 하는 경우 대부분의 경우에서 사용할 수는 **쓰기** 플러시하는 방법입니다. 그러나 앱이 많은 양의 상태 데이터를 사용 하는 경우에 스트림을 사용 하 여 직접 데이터를 저장 하는 것이 좋습니다. 트랜잭션 모델을 여 도입 된 지연 시간을 줄일 수 있습니다 합니다 **쓰기** 메서드. 

예를 들어 참조 된 [BasicSuspension](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/BasicSuspension) 샘플입니다.

## <a name="other-examples-and-resources"></a>다른 예제 및 리소스

다음은 몇 가지 예제 및 특정 시나리오에 대 한 기타 리소스입니다.

### <a name="code-example-for-retrying-file-io-example"></a>파일 I/O 예제를 다시 시도 하는 데 대 한 코드 예제

다음은 쓰기를 다시 시도 하는 방법에 의사 (pseudo) 코드 예제 (C#), 사용자 선택 파일을 저장 한 후 수행 해야 하는 쓰기를 가정 합니다.

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

### <a name="synchronize-access-to-the-file"></a>파일에 대 한 액세스를 동기화 합니다.

합니다 [.NET 블로그에서 병렬 프로그래밍에](https://devblogs.microsoft.com/pfxteam/) 병렬 프로그래밍에 대 한 지침에 대 한 중요 한 리소스입니다. 특히 합니다 [AsyncReaderWriterLock에 대 한 게시](https://devblogs.microsoft.com/pfxteam/building-async-coordination-primitives-part-7-asyncreaderwriterlock/) 동시 읽기 액세스를 허용 하는 동시 쓰기에 대 한 파일에 대 한 단독 액세스를 유지 하는 방법에 설명 합니다. 염두 I/O 영향을 줍니다 해당 직렬화 성능.

## <a name="see-also"></a>참조

* [파일 만들기, 쓰기 및 읽기](quickstart-reading-and-writing-files.md)
