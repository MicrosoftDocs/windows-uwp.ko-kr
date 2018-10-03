---
author: stevewhims
description: 백그라운드 전송 API를 사용하여 네트워크를 통해 파일을 안정적으로 복사합니다.
title: 백그라운드 전송
ms.assetid: 1207B089-BC16-4BF0-BBD4-FD99950C764B
ms.author: stwhi
ms.date: 03/23/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: fb273b6a37cb2f6322b0c9e3842b69676f82c616
ms.sourcegitcommit: 1938851dc132c60348f9722daf994b86f2ead09e
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/02/2018
ms.locfileid: "4261092"
---
# <a name="background-transfers"></a>백그라운드 전송
백그라운드 전송 API를 사용하여 네트워크를 통해 파일을 안정적으로 복사합니다. 백그라운드 전송 API는 앱이 일시 중단된 동안 백그라운드 실행되고 앱 종료 이후에도 유지되는 고급 업로드 및 다운로드 기능을 제공합니다. 이 API는 네트워크 상태를 모니터링하여 연결이 끊어진 경우 자동으로 전송을 일시 중단 및 다시 시작하며, 전송이 데이터 및 배터리를 인식합니다. 즉, 현재 연결 및 장치 배터리 상태에 따라 다운로드 작업이 조정됩니다. 이 API는 HTTP(S)를 사용한 대용량 파일 업로드 및 다운로드에 적합합니다. FTP도 지원되지만 다운로드에만 지원됩니다.

백그라운드 전송은 호출 앱과 별도로 실행되며, 주로 동영상, 음악, 큰 이미지와 같은 리소스에 대한 장기 전송 작업에 사용하도록 설계되었습니다. 이러한 시나리오에서는 앱이 일시 중단된 경우에도 다운로드가 계속 진행되기 때문에 백그라운드 전송을 사용해야 합니다.

빠르게 완료되는 작은 리소스를 다운로드하는 경우 백그라운드 전송 대신 [**HttpClient**](https://msdn.microsoft.com/library/windows/apps/dn298639) API를 사용해야 합니다.

## <a name="using-windowsnetworkingbackgroundtransfer"></a>Windows.Networking.BackgroundTransfer 사용

### <a name="how-does-the-background-transfer-feature-work"></a>백그라운드 전송 기능은 어떻게 작동하나요?
앱에서 백그라운드 전송을 사용하여 전송을 시작하면 [**BackgroundDownloader**](https://msdn.microsoft.com/library/windows/apps/br207126) 또는 [**BackgroundUploader**](https://msdn.microsoft.com/library/windows/apps/br207140) 클래스 개체를 사용하여 요청이 구성 및 초기화됩니다. 각 전송 작업은 호출 앱과는 별도로 시스템에서 개별적으로 처리됩니다. 앱의 UI를 통해 사용자에게 상태를 제공하려는 경우 진행률 정보를 사용할 수 있으며, 전송 중에도 앱이 일시 중지, 다시 시작 또는 취소되거나 데이터를 읽을 수 있습니다. 시스템에서 전송을 처리하는 방법에서는 스마트 전원 사용이 이루어지며, 연결된 앱에서 앱 일시 중단, 종료 또는 갑작스러운 네트워크 상태 변경 등의 이벤트 발생 시 생길 수 있는 문제가 방지됩니다.

> [!NOTE]
> 앱당 리소스 제한 때문에, 특정 주어진 시간 동안 앱의 전송이 200을 초과할 수 없습니다(DownloadOperations + UploadOperations). 이런 제한 기준을 초과하면 앱의 전송 큐가 복구할 수 없는 상태가 됩니다.

응용 프로그램을 시작할 때 [**AttachAsync**](/uwp/api/windows.networking.backgroundtransfer.downloadoperation.AttachAsync) 모든 기존 [**DownloadOperation**](/uwp/api/windows.networking.backgroundtransfer.downloadoperation?branch=live) 및 [**UploadOperation**](/uwp/api/windows.networking.backgroundtransfer.uploadperation?branch=live) 개체에서 호출 해야 합니다. 이렇게 하지 않으면 누수 이미 완료 된 전송 하면 하 고 결국 렌더링 백그라운드 전송 기능은 사용 사용할 수 없습니다.

### <a name="performing-authenticated-file-requests-with-background-transfer"></a>백그라운드 전송을 사용하여 인증된 파일 요청 수행
백그라운드 전송은 기본 서버 및 프록시 자격 증명, 쿠키를 지원하는 방법을 제공하는 것은 물론, 각 전송 작업에 대한 사용자 지정 HTTP 헤더 사용([**SetRequestHeader**](https://msdn.microsoft.com/library/windows/apps/br207146)를 통해)도 지원합니다.

### <a name="how-does-this-feature-adapt-to-network-status-changes-or-unexpected-shutdowns"></a>이 기능은 네트워크 상태 변경 또는 예기치 못한 종료 시 어떻게 대응하나요?
백그라운드 전송 기능은 네트워크 상태 변경 발생 시 각 전송 작업에 대해 일관된 환경을 유지 관리하여, [연결](https://msdn.microsoft.com/library/windows/apps/hh452990) 기능이 제공하는 연결 및 통신사 요금제 상태 정보를 지능적으로 활용합니다. 네트워크 시나리오별 동작을 정의하기 위해 앱은 [**BackgroundTransferCostPolicy**](https://msdn.microsoft.com/library/windows/apps/br207138)에 정의된 값을 사용하여 각 작업에 대한 비용 정책을 설정합니다.

예를 들어 작업에 대해 정의되는 비용 정책에서는 장치가 데이터 통신 연결 네트워크를 사용 중인 경우 작업을 자동으로 일시 중지하도록 나타낼 수 있습니다. 그런 다음 "무제한" 네트워크에 연결되면 전송이 자동으로 다시 시작됩니다. 비용을 기준으로 네트워크를 정의하는 방법에 대한 자세한 내용은 [**NetworkCostType**](https://msdn.microsoft.com/library/windows/apps/br207292)을 참조하세요.

백그라운드 전송 기능은 네트워크 상태 변경을 처리하는 자체 메커니즘을 가지고 있지만, 네트워크에 연결된 앱에 대한 다른 일반적인 연결 고려 사항도 있습니다. 자세한 내용은 [사용 가능한 네트워크 연결 정보 활용](https://msdn.microsoft.com/library/windows/apps/hh452983)을 읽어보세요.

> **참고**  모바일 장치에서 실행되는 앱에는 연결 형식, 로밍 상태 및 사용자의 데이터 요금제에 따라 전송되는 데이터 양을 사용자가 모니터링하고 제한할 수 있는 기능이 있습니다. 이 때문에 [**BackgroundTransferCostPolicy**](https://msdn.microsoft.com/library/windows/apps/br207138)에 전송이 진행 중으로 표시되는 경우에도 휴대폰에서 백그라운드 전송이 일시 중지될 수도 있습니다.

다음 표에서는 현재 휴대폰 상태에서 각 [**BackgroundTransferCostPolicy**](https://msdn.microsoft.com/library/windows/apps/br207138) 값에 대해 휴대폰에서 백그라운드 전송이 허용되는 경우를 보여 줍니다. [**ConnectionCost**](https://msdn.microsoft.com/library/windows/apps/br207244) 클래스를 사용하여 현재 휴대폰 상태를 확인할 수 있습니다.

| 장치 상태                                                                                                                      | UnrestrictedOnly | 기본값 | 항상 |
|-----------------------------------------------------------------------------------------------------------------------------------|------------------|---------|--------|
| WiFi에 연결됨                                                                                                                 | 허용            | 허용   | 허용  |
| 데이터 통신 연결, 로밍 안 함, 데이터 제한 적용, 제한 아래로 유지됨                                                   | 거부             | 허용   | 허용  |
| 데이터 통신 연결, 로밍 안 함, 데이터 제한 적용, 제한 초과                                                       | 거부             | 거부    | 허용  |
| 데이터 통신 연결, 로밍 중, 데이터 제한 아래                                                                                     | 거부             | 거부    | 허용  |
| 데이터 통신 연결, 데이터 제한 초과. 이 상태는 사용자가 Data Sense UI에서 "백그라운드 데이터 제한"을 사용하도록 설정한 경우에만 발생합니다. | 거부             | 거부    | 거부   |

## <a name="uploading-files"></a>파일 업로드
백그라운드 전송을 사용할 경우 업로드는 작업을 다시 시작하거나 취소하는 데 사용된 많은 컨트롤 메서드를 노출하는 [**UploadOperation**](https://msdn.microsoft.com/library/windows/apps/br207224)으로 존재합니다. 앱 이벤트(예: 일시 중단 또는 종료) 및 연결 변경은 **UploadOperation**별로 자동으로 처리되며 업로드는 앱 일시 중단 또는 일시 중지 중에도 계속되며 앱 종료 후에도 유지됩니다. 또한 [**CostPolicy**](https://msdn.microsoft.com/library/windows/apps/hh701018) 속성을 설정하여 데이터 통신 연결 네트워크로 인터넷에 연결한 동안 앱에서 업로드를 시작할지를 나타냅니다.

다음 예에서는 기본 업로드를 만들고 초기화하는 방법과 이전 앱 세션의 지속형 작업을 열거 및 다시 시도하는 방법을 안내합니다.

### <a name="uploading-a-single-file"></a>단일 파일 업로드
업로드 만들기는 [**BackgroundUploader**](https://msdn.microsoft.com/library/windows/apps/br207140)로 시작합니다. 이 클래스는 결과 [**UploadOperation**](https://msdn.microsoft.com/library/windows/apps/br207224) 개체를 만들기 전에 앱에서 업로드를 구성할 수 있는 방법을 제공합니다. 다음 예에서는 필요한 [**Uri**](https://msdn.microsoft.com/library/windows/apps/br225998) 및 [**StorageFile**](https://msdn.microsoft.com/library/windows/apps/br227171) 개체를 사용하여 이 작업을 수행하는 방법을 보여 줍니다.

**파일 및 업로드 대상 식별**

[**UploadOperation**](https://msdn.microsoft.com/library/windows/apps/br207224) 만들기를 시작하려면 먼저 업로드할 위치의 URI 및 업로드할 파일을 식별해야 합니다. 다음 예에서는 UI 입력의 문자열을 사용하여 *uriString* 값을 채우고 [**PickSingleFileAsync**](https://msdn.microsoft.com/library/windows/apps/jj635275) 작업에서 반환한 [**StorageFile**](https://msdn.microsoft.com/library/windows/apps/br227171) 개체를 사용하여 *file* 값을 채웁니다.

[!code-js[uploadFile](./code/backgroundtransfer/upload_quickstart/js/main.js#Snippetupload_quickstart_B "Identify the file and destination for the upload")]

**업로드 작업 만들기 및 초기화**

이전 단계에서는 *uriString* 및 *file* 값을 다음 예 UploadOp의 인스턴스에 전달하여 새로운 업로드 작업을 구성하고 시작하는 데 사용합니다. 먼저 *uriString*을 구문 분석하여 필요한 [**Uri**](https://msdn.microsoft.com/library/windows/apps/br225998) 개체를 만듭니다.

그런 다음 제공된 [**StorageFile**](https://msdn.microsoft.com/library/windows/apps/br227171)(*file*)의 속성을 [**BackgroundUploader**](https://msdn.microsoft.com/library/windows/apps/br207140)에서 사용하여 요청 헤더를 채우고 *SourceFile* 속성을 **StorageFile** 개체로 설정합니다. 그런 다음 문자열로 제공된 파일 이름 정보 및 [**StorageFile.Name**](https://msdn.microsoft.com/library/windows/apps/br227220) 속성을 삽입하도록 [**SetRequestHeader**](https://msdn.microsoft.com/library/windows/apps/br207146) 메서드가 호출됩니다.

마지막으로 [**BackgroundUploader**](https://msdn.microsoft.com/library/windows/apps/br207140)에서 [**UploadOperation**](https://msdn.microsoft.com/library/windows/apps/br207224)(*upload*)을 만듭니다.

[!code-js[uploadFile](./code/backgroundtransfer/upload_quickstart/js/main.js#Snippetupload_quickstart_A "Create and initialize the upload operation")]

JavaScript Promise를 사용하여 정의된 비동기 메서드 호출에 유의하세요. 마지막 예제에서 다음 줄을 봅니다.

```javascript
promise = upload.startAsync().then(complete, error, progress);
```

비동기 메서드 호출 뒤에 then 문이 오며, 비동기 메서드 호출에서 결과가 반환되면 호출되는, 앱에 의해 정의된 메서드를 나타냅니다. 이 프로그래밍 패턴에 대한 자세한 내용은 [Promises를 사용하는 JavaScript의 비동기 프로그래밍](http://msdn.microsoft.com/library/windows/apps/hh464930.aspx)을 참조하세요.

### <a name="uploading-multiple-files"></a>여러 파일 업로드
**파일 및 업로드 대상 식별**

단일 [**UploadOperation**](https://msdn.microsoft.com/library/windows/apps/br207224)으로 여러 파일을 전송하는 시나리오에서, 프로세스는 주로 요청된 대상 URI 및 로컬 파일 정보를 처음 제공하여 수행할 때 시작됩니다. 이전 섹션의 예제에서처럼, URI는 최종 사용자에 의해 문자열로 제공되며 [**FileOpenPicker**](https://msdn.microsoft.com/library/windows/apps/br207847)는 사용자 인터페이스를 통해 파일을 지정하는 기능을 제공하는 데도 사용할 수 있습니다. 그러나 대신 이 시나리오에서는 UI를 통해 여러 파일을 선택할 수 있도록 앱이 [**PickMultipleFilesAsync**](https://msdn.microsoft.com/library/windows/apps/br207851) 메서드를 호출해야 합니다.

```javascript
function uploadFiles() {
       var filePicker = new Windows.Storage.Pickers.FileOpenPicker();
       filePicker.fileTypeFilter.replaceAll(["*"]);

       filePicker.pickMultipleFilesAsync().then(function (files) {
          if (files === 0) {
             printLog("No file selected");
                return;
          }

          var upload = new UploadOperation();
          var uriString = document.getElementById("serverAddressField").value;
          upload.startMultipart(uriString, files);

          // Persist the upload operation in the global array.
          uploadOperations.push(upload);
       });
    }
```

**제공된 매개 변수에 대한 개체 만들기**

다음 두 예제에서는 마지막 단계의 끝에 호출된 단일 예제 메서드 **startMultipart**에 포함된 코드를 사용합니다. 이해를 돕기 위해 [**BackgroundTransferContentPart**](https://msdn.microsoft.com/library/windows/apps/hh923029) 개체의 배열을 만드는 메서드의 코드는 결과 [**UploadOperation**](https://msdn.microsoft.com/library/windows/apps/br207224)을 만드는 코드에서 분리되었습니다.

먼저, 사용자가 제공한 URI가 [**Uri**](https://msdn.microsoft.com/library/windows/apps/br225998)로 초기화됩니다. 다음으로, 이 메서드에 전달된 [**IStorageFile**](https://msdn.microsoft.com/library/windows/apps/br227102) 개체의 배열(**files**)에서 반복 작업이 수행되며, 각 개체는 **contentParts** 배열에 포함될 새 [**BackgroundTransferContentPart**](https://msdn.microsoft.com/library/windows/apps/hh923029) 개체를 만드는 데 사용됩니다.

```javascript
    upload.startMultipart = function (uriString, files) {
        try {
            var uri = new Windows.Foundation.Uri(uriString);
            var uploader = new Windows.Networking.BackgroundTransfer.BackgroundUploader();

            var contentParts = [];
            files.forEach(function (file, index) {
                var part = new Windows.Networking.BackgroundTransfer.BackgroundTransferContentPart("File" + index, file.name);
                part.setFile(file);
                contentParts.push(part);
            });
```

**다중 파트 업로드 작업 만들기 및 초기화**

업로드할 각 [**IStorageFile**](https://msdn.microsoft.com/library/windows/apps/br227102)을 나타내는 모든 [**BackgroundTransferContentPart**](https://msdn.microsoft.com/library/windows/apps/hh923029) 개체로 채워진 contentParts 배열이 만들어지면 요청이 보내질 위치를 나타내는 [**Uri**](https://msdn.microsoft.com/library/windows/apps/br225998)를 사용하여 [**CreateUploadAsync**](https://msdn.microsoft.com/library/windows/apps/hh923973)를 호출할 준비가 된 것입니다.

```javascript
        // Create a new upload operation.
            uploader.createUploadAsync(uri, contentParts).then(function (uploadOperation) {

               // Start the upload and persist the promise to be able to cancel the upload.
               upload = uploadOperation;
               promise = uploadOperation.startAsync().then(complete, error, progress);
            });

         } catch (err) {
             displayError(err);
         }
     };
```

### <a name="restarting-interrupted-upload-operations"></a>중단된 업로드 작업 다시 시작
[**UploadOperation**](https://msdn.microsoft.com/library/windows/apps/br207224)의 완료 또는 취소 시, 연결된 시스템 리소스가 해제됩니다. 그러나 완료 또는 취소가 발생하기 전에 앱이 종료되는 경우 활성 작업은 중단되지만 연결된 리소스는 그대로 유지됩니다. 작업이 열거되지 않아서 다음 앱 세션에 다시 사용되지 않을 경우 작업이 완료되지 않고 장치 리소스를 계속해서 차지하게 됩니다.

1.  지속형 작업을 열거하는 함수를 정의하기 전에 다음과 같이 반환될 [**UploadOperation**](https://msdn.microsoft.com/library/windows/apps/br207224) 개체를 포함할 배열을 만들어야 합니다.

    [!code-js[uploadFile](./code/backgroundtransfer/upload_quickstart/js/main.js#Snippetupload_quickstart_C "Restart interrupted upload operation")]

1.  그런 다음 지속형 작업을 열거하고 배열에 저장하는 함수를 정의합니다. [**UploadOperation**](https://msdn.microsoft.com/library/windows/apps/br207224)에 콜백을 다시 할당하기 위해 호출된 **load** 메서드는 앱이 종료될 때까지 지속되는 경우 이 섹션의 뒷부분에서 정의하는 UploadOp 클래스에 있습니다.

    [!code-js[uploadFile](./code/backgroundtransfer/upload_quickstart/js/main.js#Snippetupload_quickstart_D "Enumerate persisted operations")]

## <a name="downloading-files"></a>파일 다운로드
백그라운드 전송을 사용할 경우 각 다운로드는 작업을 일시 중지, 다시 시작 및 취소하는 데 사용된 많은 컨트롤 메서드를 노출하는 [**DownloadOperation**](https://msdn.microsoft.com/library/windows/apps/br207154)으로 존재합니다. 앱 이벤트(예: 일시 중단 또는 종료) 및 연결 변경은 **DownloadOperation**별로 자동으로 처리되며 다운로드는 앱 일시 중단 또는 일시 중지 중에도 계속되며 앱 종료 후에도 유지됩니다. 모바일 네트워크 시나리오의 경우 [**CostPolicy**](https://msdn.microsoft.com/library/windows/apps/hh701018) 속성을 설정하여 데이터 통신 연결 네트워크로 인터넷에 연결한 동안 앱에서 다운로드를 시작하거나 계속할지를 나타냅니다.

빠르게 완료되는 작은 리소스를 다운로드하는 경우 백그라운드 전송 대신 [**HttpClient**](https://msdn.microsoft.com/library/windows/apps/dn298639) API를 사용해야 합니다.

다음 예에서는 기본 다운로드를 만들고 초기화하는 방법과 이전 앱 세션의 지속형 작업을 열거 및 다시 시도하는 방법을 안내합니다.

### <a name="configure-and-start-a-background-transfer-file-download"></a>백그라운드 전송 파일 다운로드 구성 및 시작
다음 예에서는 URI 및 파일 이름을 나타내는 문자열을 사용하여 [**Uri**](https://msdn.microsoft.com/library/windows/apps/br225998) 개체 및 요청한 파일을 포함할 [**StorageFile**](https://msdn.microsoft.com/library/windows/apps/br227171)을 만들 수 있는 방법을 보여 줍니다. 이 예에서 새 파일은 미리 정의된 위치에 자동으로 배치됩니다. 또는 [**FileSavePicker**](https://msdn.microsoft.com/library/windows/apps/br207871)를 사용하여 사용자가 장치에서 파일을 저장할 위치를 나타내도록 할 수 있습니다. [**DownloadOperation**](https://msdn.microsoft.com/library/windows/apps/br207154)에 콜백을 다시 할당하기 위해 호출되는 **load** 메서드는 앱 종료 기간 동안 지속될 경우 이 섹션의 뒷 부분에서 정의하는 DownloadOp 클래스에 있습니다.

[!code-js[uploadFile](./code/backgroundtransfer/download_quickstart/js/main.js#Snippetdownload_quickstart_A)]

JavaScript Promise를 사용하여 정의된 비동기 메서드 호출에 유의하세요. 이전 코드 예에서 17줄을 봅니다.

```javascript
promise = download.startAsync().then(complete, error, progress);
```

비동기 메서드 호출 뒤에 then 문이 오며, 비동기 메서드 호출에서 결과가 반환되면 호출되는, 앱에 의해 정의된 메서드를 나타냅니다. 이 프로그래밍 패턴에 대한 자세한 내용은 [Promises를 사용하는 JavaScript의 비동기 프로그래밍](http://msdn.microsoft.com/library/windows/apps/hh464930.aspx)을 참조하세요.

### <a name="adding-additional-operation-control-methods"></a>작업 컨트롤 메서드 추가
제어 수준은 추가 [**DownloadOperation**](https://msdn.microsoft.com/library/windows/apps/br207154) 메서드를 구현하여 높일 수 있습니다. 예를 들어 위의 예에 다음 코드를 추가하여 다운로드 취소 기능을 적용할 수 있습니다.

[!code-js[uploadFile](./code/backgroundtransfer/download_quickstart/js/main.js#Snippetdownload_quickstart_B)]

### <a name="enumerating-persisted-operations-at-start-up"></a>시작할 때 지속형 작업 열거
[**DownloadOperation**](https://msdn.microsoft.com/library/windows/apps/br207154)의 완료 또는 취소 시, 연결된 시스템 리소스가 해제됩니다. 이러한 이벤트가 발생하기 이전에 앱이 종료될 경우 다운로드는 일시 중지되고 백그라운드에서 지속됩니다. 다음 예에서는 지속형 다운로드를 새 앱 세션에서 다시 사용하는 방법을 보여 줍니다.

1.  지속형 작업을 열거하는 함수를 정의하기 전에 다음과 같이 반환될 [**DownloadOperation**](https://msdn.microsoft.com/library/windows/apps/br207154) 개체를 포함할 배열을 만들어야 합니다.

    [!code-js[uploadFile](./code/backgroundtransfer/download_quickstart/js/main.js#Snippetdownload_quickstart_D)]

1.  그런 다음 지속형 작업을 열거하고 배열에 저장하는 함수를 정의합니다. 지속형 [**DownloadOperation**](https://msdn.microsoft.com/library/windows/apps/br207154)에 대한 콜백을 다시 할당하기 위해 호출되는 **load** 메서드는 이 섹션의 뒷 부분에서 정의하는 DownloadOp 예에 있습니다.

    [!code-js[uploadFile](./code/backgroundtransfer/download_quickstart/js/main.js#Snippetdownload_quickstart_E)]

1.  이제 채워진 목록을 사용하여 보류 중인 작업을 다시 시작할 수 있습니다.

## <a name="post-processing"></a>사후 처리
Windows 10의 한 가지 새로운 기능은 앱이 실행되지 않는 경우에도 백그라운드 전송 완료 시 응용 프로그램 코드를 실행하는 기능입니다. 예를 들어 앱을 시작할 때마다 새 동영상을 검색하는 대신 영화 다운로드가 완료된 후 앱에서 사용 가능한 동영상 목록을 업데이트할 수 있습니다. 또는 앱에서 다른 서버나 포트를 다시 사용하여 실패한 파일 전송을 처리할 수 있습니다. 사후 처리는 성공한 전송과 실패한 전송 모두에 대해 호출되므로 이를 사용하여 사용자 지정 오류 처리 및 다시 시도 논리를 구현할 수 있습니다.

사후 처리에서는 기존 백그라운드 작업 인프라를 사용합니다. 전송을 시작하기 전에 백그라운드 작업을 만들고 전송과 연결합니다. 그러면 백그라운드에서 전송이 실행되고, 완료되면 사후 처리를 수행하기 위해 백그라운드 작업이 호출됩니다.

사후 처리에서는 새 클래스 [**BackgroundTransferCompletionGroup**](https://msdn.microsoft.com/library/windows/apps/dn804209)을 사용합니다. 이 클래스는 백그라운드 전송 작업을 그룹화할 수 있는 기존 [**BackgroundTransferGroup**](https://msdn.microsoft.com/library/windows/apps/dn279030)과 유사하지만 **BackgroundTransferCompletionGroup**은 전송이 완료되면 실행할 백그라운드 작업을 지정할 수 있는 기능을 추가합니다.

다음과 같이 사후 처리를 사용하여 백그라운드 전송을 시작합니다.

1.  [**BackgroundTransferCompletionGroup**](https://msdn.microsoft.com/library/windows/apps/dn804209) 개체를 만듭니다. 그런 다음 [**BackgroundTaskBuilder**](https://msdn.microsoft.com/library/windows/apps/br224768) 개체를 만듭니다. 작성기 개체의 **Trigger** 속성을 완료 그룹 개체로 설정하고, 작성기의 **TaskEntryPoint** 속성을 전송 완료 시 실행해야 하는 백그라운드 작업의 진입점으로 설정합니다. 마지막으로 [**BackgroundTaskBuilder.Register**](https://msdn.microsoft.com/library/windows/apps/br224772) 메서드를 호출하여 백그라운드 작업을 등록합니다. 여러 완료 작업이 하나의 백그라운드 작업 진입점을 공유할 수 있지만 백그라운드 작업 등록당 하나의 완료 그룹만 유지할 수 있습니다.

```csharp
var completionGroup = new BackgroundTransferCompletionGroup();
BackgroundTaskBuilder builder = new BackgroundTaskBuilder();

builder.Name = "MyDownloadProcessingTask";
builder.SetTrigger(completionGroup.Trigger);
builder.TaskEntryPoint = "Tasks.BackgroundDownloadProcessingTask";

BackgroundTaskRegistration downloadProcessingTask = builder.Register();
```

2.  이제 백그라운드 전송을 완료 그룹과 연결합니다. 모든 전송을 만든 후에는 완료 그룹을 사용하도록 설정합니다.

```csharp
BackgroundDownloader downloader = new BackgroundDownloader(completionGroup);
DownloadOperation download = downloader.CreateDownload(uri, file);
Task<DownloadOperation> startTask = download.StartAsync().AsTask();

// App still sees the normal completion path
startTask.ContinueWith(ForegroundCompletionHandler);

// Do not enable the CompletionGroup until after all downloads are created.
downloader.CompletinGroup.Enable();
```

3.  백그라운드 작업의 코드가 트리거 세부 정보에서 작업 목록을 추출하고 나면 사용자 코드가 각 작업에 대한 세부 정보를 검사하고 각 작업에 대한 적절한 사후 처리를 수행할 수 있습니다.

```csharp
public class BackgroundDownloadProcessingTask : IBackgroundTask
{
    public async void Run(IBackgroundTaskInstance taskInstance)
    {
    var details = (BackgroundTransferCompletionGroupTriggerDetails)taskInstance.TriggerDetails;
    IReadOnlyList<DownloadOperation> downloads = details.Downloads;

    // Do post-processing on each finished operation in the list of downloads
    }
}
```

사후 처리 작업은 일반적인 백그라운드 작업입니다. 모든 백그라운드 작업의 풀에 속하며, 모든 백그라운드 작업과 동일한 리소스 관리 정책이 적용됩니다.

사후 처리는 전경 완료 처리기를 대체하지 않습니다. 앱에서 전경 완료 처리기를 정의한 경우 앱이 실행 중일 때 파일 전송이 완료되면 전경 완료 처리기와 백그라운드 완료 처리기가 둘 다 호출됩니다. 전경 작업과 백그라운드 작업이 호출되는 순서는 보장되지 않습니다. 둘 다 정의한 경우 두 작업이 제대로 작동하고 동시에 실행될 때 서로 간섭하지 않는지 확인해야 합니다.

## <a name="request-timeouts"></a>요청 시간 제한
두 가지 주요 연결 시간 제한 시나리오를 고려해야 합니다.

-   전송을 위해 새 연결을 설정할 때 5분 이내에 연결이 설정되지 않을 경우 연결 요청이 중단됩니다.

-   연결이 설정되면 2분 이내에 응답을 받지 못한 모든 HTTP 요청 메시지가 중단됩니다.

> **참고**  두 경우 모두 인터넷 연결이 있고 백그라운드 전송에서 요청을 최대 세 번까지 자동으로 다시 시도한다고 가정합니다. 인터넷 연결이 검색되지 않는 경우 추가 요청은 인터넷에 연결될 때까지 대기합니다.

## <a name="debugging-guidance"></a>디버깅 지침
Microsoft Visual Studio에서 디버깅 세션을 중지하는 것은 앱을 닫는 것과 같습니다. PUT 업로드가 일시 중지되고 POST 업로드가 종료됩니다. 디버깅 중에도 앱이 지속형 업로드를 열거한 다음 다시 시작하거나 취소해야 합니다. 예를 들어 해당 디버그 세션의 이전 작업에 관심이 없는 경우 앱을 시작할 때 앱에서 열거된 지속형 업로드 작업을 취소하도록 할 수 있습니다.

디버그 세션의 이전 작업에 관심이 없는 경우 디버그 세션 중 앱을 시작할 때 다운로드/업로드를 열거하는 동안 앱에서 취소하도록 할 수 있습니다. 앱 매니페스트 변경 등의 Visual Studio 프로젝트 업데이트가 있으며 앱을 제거하고 다시 배포하는 경우에는 [**GetCurrentUploadsAsync**](https://msdn.microsoft.com/library/windows/apps/hh701149)에서 이전 앱 배포를 사용하여 만든 작업을 열거할 수 없습니다.

개발 중에 백그라운드 전송을 사용하면 완료된 활성 전송 작업의 내부 캐시가 동기화되지 않는 상황이 발생할 수 있습니다. 이로 인해 새 전송 작업을 시작하지 못하거나 기존 작업 및 [**BackgroundTransferGroup**](https://msdn.microsoft.com/library/windows/apps/dn279030) 개체와의 상호 작용이 불가능해질 수 있습니다. 기존 작업을 조작할 때 크래시가 트리거되는 경우도 있습니다. [**TransferBehavior**](https://msdn.microsoft.com/library/windows/apps/dn279033) 속성이 **Parallel**로 설정된 경우 이러한 결과가 발생할 수 있습니다. 이 문제는 개발 중 특정 시나리오에서만 발생하며 앱의 최종 사용자에게는 해당하지 않습니다.

Visual Studio를 사용한 네 가지 시나리오에서 이 문제가 발생할 수 있습니다.

-   기존 프로젝트와 앱 이름은 같지만 다른 언어(예: C++에서 C#으로 변경)로 새 프로젝트를 만듭니다.
-   기존 프로젝트에서 대상 아키텍처를 변경합니다(예: x86에서 x64로 변경).
-   기존 프로젝트에서 문화권을 변경합니다(예: 중립에서 en-US로 변경).
-   기존 프로젝트의 패키지 매니페스트에서 접근 권한 값을 추가하거나 제거합니다(예: **엔터프라이즈 인증** 추가).

접근 권한 값을 추가하거나 제거하는 매니페스트 업데이트를 비롯한 정기 앱 서비스는 앱의 최종 사용자 배포에서 이 문제를 트리거하지 않습니다.
이 문제를 해결하려면 앱의 모든 버전을 완전히 제거하고 새로운 언어, 아키텍처, 문화권 또는 접근 권한 값으로 다시 배포합니다. **시작** 화면이나 PowerShell 및 **Remove-AppxPackage** cmdlet을 사용하여 이 작업을 수행할 수 있습니다.

## <a name="exceptions-in-windowsnetworkingbackgroundtransfer"></a>Windows.Networking.BackgroundTransfer의 예외
잘못된 URI(Uniform Resource Identifier) 문자열이 [**Windows.Foundation.Uri**](https://msdn.microsoft.com/library/windows/apps/br225998) 개체에 대한 생성자에 전달되면 예외가 발생합니다.

**.NET:** [**Windows.Foundation.Uri**](https://msdn.microsoft.com/library/windows/apps/br225998) 형식은 C# 및 VB에서 [**System.Uri**](https://msdn.microsoft.com/library/windows/apps/xaml/system.uri.aspx)로 표시됩니다.

C# 및 Visual Basic에서는 .NET 4.5의 [**System.Uri**](https://msdn.microsoft.com/library/windows/apps/xaml/system.uri.aspx) 클래스와 [**System.Uri.TryCreate**](https://msdn.microsoft.com/library/windows/apps/xaml/system.uri.trycreate.aspx) 메서드 중 하나를 통해 URI가 생성되기 전에 앱 사용자로부터 받은 문자열을 테스트하여 이 오류를 방지할 수 있습니다.

C++에는 URI에 대한 문자열을 시도 및 구문 분석할 메서드가 없습니다. 앱이 [**Windows.Foundation.Uri**](https://msdn.microsoft.com/library/windows/apps/br225998)에 대해 사용자 입력을 받으면 생성자는 try/catch 블록에 있게 됩니다. 예외가 발생하면 앱에서 사용자에게 알리고 새 호스트 이름을 요청할 수 있습니다.

[**Windows.Networking.backgroundTransfer**](https://msdn.microsoft.com/library/windows/apps/br207242) 네임스페이스에는 편리한 도우미 메서드가 있으며 오류를 처리하는 데 [**Windows.Networking.Sockets**](https://msdn.microsoft.com/library/windows/apps/br226960) 네임스페이스의 열거형을 사용합니다. 특정 네트워크 예외를 앱에서 다르게 처리하는 데 유용합니다.

[**Windows.Networking.backgroundTransfer**](https://msdn.microsoft.com/library/windows/apps/br207242) 네임스페이스의 비동기 메서드에서 발생한 오류는 **HRESULT** 값으로 반환됩니다. [**BackgroundTransferError.GetStatus**](https://msdn.microsoft.com/library/windows/apps/hh701093) 메서드는 백그라운드 전송 작업의 네트워크 오류를 [**WebErrorStatus**](https://msdn.microsoft.com/library/windows/apps/hh747818) 열거형 값으로 변환하는 데 사용됩니다. 대부분의 **WebErrorStatus** 열거형 값은 기본 HTTP 또는 FTP 클라이언트 작업에서 반환한 오류에 해당합니다. 앱은 특정 **WebErrorStatus** 열거형 값을 필터링하여 예외의 원인에 따라 앱 동작을 수정할 수 있습니다.

매개 변수 유효성 검사 오류의 경우 앱은 또한 예외에서 **HRESULT**를 사용하여 예외의 원인이 된 오류에 대한 더 자세한 정보를 알 수 있습니다. 가능한 **HRESULT** 값은 *Winerror.h* 헤더 파일에 나열되어 있습니다. 대부분의 매개 변수 유효성 검사 오류에서 반환되는 **HRESULT**는 **E\_INVALIDARG**입니다.

## <a name="important-apis"></a>중요 API
* [**Windows.Networking.BackgroundTransfer**](/uwp/api/windows.networking.backgroundtransfer)
* [**Windows.Foundation.Uri**](/uwp/api/Windows.Foundation.Uri)
* [**Windows.Networking.Sockets**](/uwp/api/Windows.Networking.Sockets)
