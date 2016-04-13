---
백그라운드 전송 API를 사용하여 네트워크를 통해 파일을 안정적으로 복사합니다.
백그라운드 전송
ms.assetid: 1207B089-BC16-4BF0-BBD4-FD99950C764B
---

# 백그라운드 전송

\[ Windows 10의 UWP 앱에 맞게 업데이트되었습니다. Windows 8.x 문서는 [보관](http://go.microsoft.com/fwlink/p/?linkid=619132)을 참조하세요. \]


**중요 API**

-   [**Windows.Networking.backgroundTransfer**](https://msdn.microsoft.com/library/windows/apps/br207242)
-   [**Windows.Foundation.Uri**](https://msdn.microsoft.com/library/windows/apps/br225998)
-   [**Windows.Networking.Sockets**](https://msdn.microsoft.com/library/windows/apps/br226960)

백그라운드 전송 API를 사용하여 네트워크를 통해 파일을 안정적으로 복사합니다. 백그라운드 전송 API는 앱이 일시 중단된 동안 백그라운드 실행되고 앱 종료 이후에도 유지되는 고급 업로드 및 다운로드 기능을 제공합니다. 이 API는 네트워크 상태를 모니터링하여 연결이 끊어진 경우 자동으로 전송을 일시 중단 및 다시 시작하며, 전송이 데이터 및 배터리를 인식합니다. 즉, 현재 연결 및 장치 배터리 상태에 따라 다운로드 작업이 조정됩니다. 이 API는 HTTP(S)를 사용한 대용량 파일 업로드 및 다운로드에 적합합니다. FTP도 지원되지만 다운로드에만 지원됩니다.

백그라운드 전송은 호출 앱과 별도로 실행되며, 주로 동영상, 음악, 큰 이미지와 같은 리소스에 대한 장기 전송 작업에 사용하도록 설계되었습니다. 이러한 시나리오에서는 앱이 일시 중단된 경우에도 다운로드가 계속 진행되기 때문에 백그라운드 전송을 사용해야 합니다.

빠르게 완료되는 작은 리소스를 다운로드하는 경우 백그라운드 전송 대신 [**HttpClient**](https://msdn.microsoft.com/library/windows/apps/dn298639) API를 사용해야 합니다.

## Windows.Networking.BackgroundTransfer 사용


### 백그라운드 전송 기능은 어떻게 작동하나요?

앱에서 백그라운드 전송을 사용하여 전송을 시작하면 [**BackgroundDownloader**](https://msdn.microsoft.com/library/windows/apps/br207126) 또는 [**BackgroundUploader**](https://msdn.microsoft.com/library/windows/apps/br207140) 클래스 개체를 사용하여 요청이 구성 및 초기화됩니다. 각 전송 작업은 호출 앱과는 별도로 시스템에서 개별적으로 처리됩니다. 앱의 UI를 통해 사용자에게 상태를 제공하려는 경우 진행률 정보를 사용할 수 있으며, 전송 중에도 앱이 일시 중지, 다시 시작 또는 취소되거나 데이터를 읽을 수 있습니다. 시스템에서 전송을 처리하는 방법에서는 스마트 전원 사용이 이루어지며, 연결된 앱에서 앱 일시 중단, 종료 또는 갑작스러운 네트워크 상태 변경 등의 이벤트 발생 시 생길 수 있는 문제가 방지됩니다.

### 백그라운드 전송을 사용하여 인증된 파일 요청 수행

백그라운드 전송은 기본 서버 및 프록시 자격 증명, 쿠키를 지원하는 방법을 제공하는 것은 물론, 각 전송 작업에 대한 사용자 지정 HTTP 헤더 사용([**SetRequestHeader**](https://msdn.microsoft.com/library/windows/apps/br207146)를 통해)도 지원합니다.

### 이 기능은 네트워크 상태 변경 또는 예기치 못한 종료 시 어떻게 대응하나요?

백그라운드 전송 기능은 네트워크 상태 변경 발생 시 각 전송 작업에 대해 일관된 환경을 유지 관리하여, [연결](https://msdn.microsoft.com/library/windows/apps/hh452990) 기능이 제공하는 연결 및 통신사 요금제 상태 정보를 지능적으로 활용합니다. 네트워크 시나리오별 동작을 정의하기 위해 앱은 [**BackgroundTransferCostPolicy**](https://msdn.microsoft.com/library/windows/apps/br207138)에 정의된 값을 사용하여 각 작업에 대한 비용 정책을 설정합니다.

예를 들어 작업에 대해 정의되는 비용 정책에서는 장치가 데이터 통신 연결 네트워크를 사용 중인 경우 작업을 자동으로 일시 중지하도록 나타낼 수 있습니다. 그런 다음 "무제한" 네트워크에 연결되면 전송이 자동으로 다시 시작됩니다. 비용을 기준으로 네트워크를 정의하는 방법에 대한 자세한 내용은 [**NetworkCostType**](https://msdn.microsoft.com/library/windows/apps/br207292)을 참조하세요.

백그라운드 전송 기능은 네트워크 상태 변경을 처리하는 자체 메커니즘을 가지고 있지만, 네트워크에 연결된 앱에 대한 다른 일반적인 연결 고려 사항도 있습니다. 자세한 내용은 [사용 가능한 네트워크 연결 정보 활용](https://msdn.microsoft.com/library/windows/apps/hh452983)을 읽어보세요.

> **참고** 모바일 디바이스에서 실행되는 앱에는 연결 형식, 로밍 상태 및 사용자의 데이터 요금제에 따라 전송되는 데이터 양을 사용자가 모니터링하고 제한할 수 있는 기능이 있습니다. 이 때문에 [**BackgroundTransferCostPolicy**](https://msdn.microsoft.com/library/windows/apps/br207138)에 전송이 진행 중으로 표시되는 경우에도 휴대폰에서 백그라운드 전송이 일시 중지될 수도 있습니다.

다음 표에서는 현재 휴대폰 상태에서 각 [**BackgroundTransferCostPolicy**](https://msdn.microsoft.com/library/windows/apps/br207138) 값에 대해 휴대폰에서 백그라운드 전송이 허용되는 경우를 보여 줍니다. [
            **ConnectionCost**](https://msdn.microsoft.com/library/windows/apps/br207244) 클래스를 사용하여 현재 휴대폰 상태를 확인할 수 있습니다.

| 장치 상태                                                                                                                      | UnrestrictedOnly | 기본값 | 항상 |
|-----------------------------------------------------------------------------------------------------------------------------------|------------------|---------|--------|
| WiFi에 연결됨                                                                                                                 | 허용            | 허용   | 허용  |
| 데이터 통신 연결, 로밍 안 함, 데이터 제한 적용, 제한 아래로 유지됨                                                   | 거부             | 허용   | 허용  |
| 데이터 통신 연결, 로밍 안 함, 데이터 제한 적용, 제한 초과                                                       | 거부             | 거부    | 허용  |
| 데이터 통신 연결, 로밍 중, 데이터 제한 아래                                                                                     | 거부             | 거부    | 허용  |
| 데이터 통신 연결, 데이터 제한 초과. 이 상태는 사용자가 Data Sense UI에서 "백그라운드 데이터 제한"을 사용하도록 설정한 경우에만 발생합니다. | 거부             | 거부    | 거부   |

 

## 파일 업로드


백그라운드 전송을 사용할 경우 업로드는 작업을 다시 시작하거나 취소하는 데 사용된 많은 컨트롤 메서드를 노출하는 [**UploadOperation**](https://msdn.microsoft.com/library/windows/apps/br207224)으로 존재합니다. 앱 이벤트(예: 일시 중단 또는 종료) 및 연결 변경은 **UploadOperation**별로 자동으로 처리되며 업로드는 앱 일시 중단 또는 일시 중지 중에도 계속되며 앱 종료 후에도 유지됩니다. 또한 [**CostPolicy**](https://msdn.microsoft.com/library/windows/apps/hh701018) 속성을 설정하여 데이터 통신 연결 네트워크로 인터넷에 연결한 동안 앱에서 업로드를 시작할지를 나타냅니다.

다음 예에서는 기본 업로드를 만들고 초기화하는 방법과 이전 앱 세션의 지속형 작업을 열거 및 다시 시도하는 방법을 안내합니다.

### 단일 파일 업로드

업로드 만들기는 [**BackgroundUploader**](https://msdn.microsoft.com/library/windows/apps/br207140)로 시작합니다. 이 클래스는 결과 [**UploadOperation**](https://msdn.microsoft.com/library/windows/apps/br207224) 개체를 만들기 전에 앱에서 업로드를 구성할 수 있는 방법을 제공합니다. 다음 예에서는 필요한 [**Uri**](https://msdn.microsoft.com/library/windows/apps/br225998) 및 [**StorageFile**](https://msdn.microsoft.com/library/windows/apps/br227171) 개체를 사용하여 이 작업을 수행하는 방법을 보여 줍니다.

**파일 및 업로드 대상 식별**

[
            **UploadOperation**](https://msdn.microsoft.com/library/windows/apps/br207224) 만들기를 시작하려면 먼저 업로드할 위치의 URI 및 업로드할 파일을 식별해야 합니다. 다음 예에서는 UI 입력의 문자열을 사용하여 *uriString* 값을 채우고 [**PickSingleFileAsync**](https://msdn.microsoft.com/library/windows/apps/jj635275) 작업에서 반환한 [**StorageFile**](https://msdn.microsoft.com/library/windows/apps/br227171) 개체를 사용하여 *file* 값을 채웁니다.

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

    The async method call is followed by a then statement which indicates methods, defined by the app, that are called when a result from the async method call is returned. For more information on this programming pattern, see [Asynchronous programming in JavaScript using promises](http://msdn.microsoft.com/library/windows/apps/hh464930.aspx).

### 여러 파일 업로드

**파일 및 업로드 대상 식별**

    In a scenario involving multiple files transferred with a single [**UploadOperation**](https://msdn.microsoft.com/library/windows/apps/br207224), the process begins as it usually does by first providing the required destination URI and local file information. Similar to the example in the previous section, the URI is provided as a string by the end-user and [**FileOpenPicker**](https://msdn.microsoft.com/library/windows/apps/br207847) can be used to provide the ability to indicate files through the user interface as well. However, in this scenario the app should instead call the [**PickMultipleFilesAsync**](https://msdn.microsoft.com/library/windows/apps/br207851) method to enable the selection of multiple files through the UI.

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

    The next two examples use code contained in a single example method, **startMultipart**, which was called at the end of the last step. For the purpose of instruction the code in the method that creates an array of [**BackgroundTransferContentPart**](https://msdn.microsoft.com/library/windows/apps/hh923029) objects has been split from the code that creates the resultant [**UploadOperation**](https://msdn.microsoft.com/library/windows/apps/br207224).

    First, the URI string provided by the user is initialized as a [**Uri**](https://msdn.microsoft.com/library/windows/apps/br225998). Next, the array of [**IStorageFile**](https://msdn.microsoft.com/library/windows/apps/br227102) objects (**files**) passed to this method is iterated through, each object is used to create a new [**BackgroundTransferContentPart**](https://msdn.microsoft.com/library/windows/apps/hh923029) object which is then placed in the **contentParts** array.

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

    With our contentParts array populated with all of the [**BackgroundTransferContentPart**](https://msdn.microsoft.com/library/windows/apps/hh923029) objects representing each [**IStorageFile**](https://msdn.microsoft.com/library/windows/apps/br227102) for upload, we are ready to call [**CreateUploadAsync**](https://msdn.microsoft.com/library/windows/apps/hh923973) using the [**Uri**](https://msdn.microsoft.com/library/windows/apps/br225998) to indicate where the request will be sent.

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

### 중단된 업로드 작업 다시 시작

[
            **UploadOperation**](https://msdn.microsoft.com/library/windows/apps/br207224)의 완료 또는 취소 시, 연결된 시스템 리소스가 해제됩니다. 그러나 완료 또는 취소가 발생하기 전에 앱이 종료되는 경우 활성 작업은 중단되지만 연결된 리소스는 그대로 유지됩니다. 작업이 열거되지 않아서 다음 앱 세션에 다시 사용되지 않을 경우 작업이 완료되지 않고 장치 리소스를 계속해서 차지하게 됩니다.

1.  지속형 작업을 열거하는 함수를 정의하기 전에 다음과 같이 반환될 [**UploadOperation**](https://msdn.microsoft.com/library/windows/apps/br207224) 개체를 포함할 배열을 만들어야 합니다.

[!code-js[uploadFile](./code/backgroundtransfer/upload_quickstart/js/main.js#Snippetupload_quickstart_C "Restart interrupted upload operation")]

2.  그런 다음 지속형 작업을 열거하고 배열에 저장하는 함수를 정의합니다. [
            **UploadOperation**](https://msdn.microsoft.com/library/windows/apps/br207224)에 콜백을 다시 할당하기 위해 호출된 **load** 메서드는 앱이 종료될 때까지 지속되는 경우 이 섹션의 뒷부분에서 정의하는 UploadOp 클래스에 있습니다.

[!code-js[uploadFile](./code/backgroundtransfer/upload_quickstart/js/main.js#Snippetupload_quickstart_D "Enumerate persisted operations")]

## 파일 다운로드

백그라운드 전송을 사용할 경우 각 다운로드는 작업을 일시 중지, 다시 시작 및 취소하는 데 사용된 많은 컨트롤 메서드를 노출하는 [**DownloadOperation**](https://msdn.microsoft.com/library/windows/apps/br207154)으로 존재합니다. 앱 이벤트(예: 일시 중단 또는 종료) 및 연결 변경은 **DownloadOperation**별로 자동으로 처리되며 다운로드는 앱 일시 중단 또는 일시 중지 중에도 계속되며 앱 종료 후에도 유지됩니다. 모바일 네트워크 시나리오의 경우 [**CostPolicy**](https://msdn.microsoft.com/library/windows/apps/hh701018) 속성을 설정하여 데이터 통신 연결 네트워크로 인터넷에 연결한 동안 앱에서 다운로드를 시작하거나 계속할지를 나타냅니다.

빠르게 완료되는 작은 리소스를 다운로드하는 경우 백그라운드 전송 대신 [**HttpClient**](https://msdn.microsoft.com/library/windows/apps/dn298639) API를 사용해야 합니다.

다음 예에서는 기본 다운로드를 만들고 초기화하는 방법과 이전 앱 세션의 지속형 작업을 열거 및 다시 시도하는 방법을 안내합니다.

### 백그라운드 전송 파일 다운로드 구성 및 시작

다음 예에서는 URI 및 파일 이름을 나타내는 문자열을 사용하여 [**Uri**](https://msdn.microsoft.com/library/windows/apps/br225998) 개체 및 요청한 파일을 포함할 [**StorageFile**](https://msdn.microsoft.com/library/windows/apps/br227171)을 만들 수 있는 방법을 보여 줍니다. 이 예에서 새 파일은 미리 정의된 위치에 자동으로 배치됩니다. 또는 [**FileSavePicker**](https://msdn.microsoft.com/library/windows/apps/br207871)를 사용하여 사용자가 장치에서 파일을 저장할 위치를 나타내도록 할 수 있습니다. [
            **DownloadOperation**](https://msdn.microsoft.com/library/windows/apps/br207154)에 콜백을 다시 할당하기 위해 호출되는 **load** 메서드는 앱 종료 기간 동안 지속될 경우 이 섹션의 뒷 부분에서 정의하는 DownloadOp 클래스에 있습니다.

[!code-js[uploadFile](./code/backgroundtransfer/download_quickstart/js/main.js#Snippetdownload_quickstart_A)]

JavaScript Promise를 사용하여 정의된 비동기 메서드 호출에 유의하세요. 이전 코드 예에서 17줄을 봅니다.

```javascript
promise = download.startAsync().then(complete, error, progress);
```

비동기 메서드 호출 뒤에 then 문이 오며, 비동기 메서드 호출에서 결과가 반환되면 호출되는, 앱에 의해 정의된 메서드를 나타냅니다. 이 프로그래밍 패턴에 대한 자세한 내용은 [Promises를 사용하는 JavaScript의 비동기 프로그래밍](http://msdn.microsoft.com/library/windows/apps/hh464930.aspx)을 참조하세요.

### 작업 컨트롤 메서드 추가

제어 수준은 추가 [**DownloadOperation**](https://msdn.microsoft.com/library/windows/apps/br207154) 메서드를 구현하여 높일 수 있습니다. 예를 들어 위의 예에 다음 코드를 추가하여 다운로드 취소 기능을 적용할 수 있습니다.

[!code-js[uploadFile](./code/backgroundtransfer/download_quickstart/js/main.js#Snippetdownload_quickstart_B)]

### 시작할 때 지속형 작업 열거

[
            **DownloadOperation**](https://msdn.microsoft.com/library/windows/apps/br207154)의 완료 또는 취소 시, 연결된 시스템 리소스가 해제됩니다. 이러한 이벤트가 발생하기 이전에 앱이 종료될 경우 다운로드는 일시 중지되고 백그라운드에서 지속됩니다. 다음 예에서는 지속형 다운로드를 새 앱 세션에서 다시 사용하는 방법을 보여 줍니다.

1.  지속형 작업을 열거하는 함수를 정의하기 전에 다음과 같이 반환될 [**DownloadOperation**](https://msdn.microsoft.com/library/windows/apps/br207154) 개체를 포함할 배열을 만들어야 합니다.

[!code-js[uploadFile](./code/backgroundtransfer/download_quickstart/js/main.js#Snippetdownload_quickstart_D)]

2.  그런 다음 지속형 작업을 열거하고 배열에 저장하는 함수를 정의합니다. 지속형 [**DownloadOperation**](https://msdn.microsoft.com/library/windows/apps/br207154)에 대한 콜백을 다시 할당하기 위해 호출되는 **load** 메서드는 이 섹션의 뒷 부분에서 정의하는 DownloadOp 예에 있습니다.

[!code-js[uploadFile](./code/backgroundtransfer/download_quickstart/js/main.js#Snippetdownload_quickstart_E)]

3.  이제 채워진 목록을 사용하여 보류 중인 작업을 다시 시작할 수 있습니다.

## 사후 처리

Windows 10의 한 가지 새로운 기능은 앱이 실행되지 않는 경우에도 백그라운드 전송 완료 시 응용 프로그램 코드를 실행하는 기능입니다. 예를 들어 앱을 시작할 때마다 새 동영상을 검색하는 대신 영화 다운로드가 완료된 후 앱에서 사용 가능한 동영상 목록을 업데이트할 수 있습니다. 또는 앱에서 다른 서버나 포트를 다시 사용하여 실패한 파일 전송을 처리할 수 있습니다. 사후 처리는 성공한 전송과 실패한 전송 모두에 대해 호출되므로 이를 사용하여 사용자 지정 오류 처리 및 다시 시도 논리를 구현할 수 있습니다.

사후 처리에서는 기존 백그라운드 작업 인프라를 사용합니다. 전송을 시작하기 전에 백그라운드 작업을 만들고 전송과 연결합니다. 그러면 백그라운드에서 전송이 실행되고, 완료되면 사후 처리를 수행하기 위해 백그라운드 작업이 호출됩니다.

사후 처리에서는 새 클래스 [**BackgroundTransferCompletionGroup**](https://msdn.microsoft.com/library/windows/apps/dn804209)을 사용합니다. 이 클래스는 백그라운드 전송 작업을 그룹화할 수 있는 기존 [**BackgroundTransferGroup**](https://msdn.microsoft.com/library/windows/apps/dn279030)과 유사하지만 **BackgroundTransferCompletionGroup**은 전송이 완료되면 실행할 백그라운드 작업을 지정할 수 있는 기능을 추가합니다.

다음과 같이 사후 처리를 사용하여 백그라운드 전송을 시작합니다.

1.  [
            **BackgroundTransferCompletionGroup**](https://msdn.microsoft.com/library/windows/apps/dn804209) 개체를 만듭니다. 그런 다음 [**BackgroundTaskBuilder**](https://msdn.microsoft.com/library/windows/apps/br224768) 개체를 만듭니다. 작성기 개체의 **Trigger** 속성을 완료 그룹 개체로 설정하고, 작성기의 **TaskEngtyPoint** 속성을 전송 완료 시 실행해야 하는 백그라운드 작업의 진입점으로 설정합니다. 마지막으로 [**BackgroundTaskBuilder.Register**](https://msdn.microsoft.com/library/windows/apps/br224772) 메서드를 호출하여 백그라운드 작업을 등록합니다. 여러 완료 작업이 하나의 백그라운드 작업 진입점을 공유할 수 있지만 백그라운드 작업 등록당 하나의 완료 그룹만 유지할 수 있습니다.

   ```csharp
    var completionGroup = new BackgroundTransferCompletionGroup();
    BackgroundTaskBuilder builder = new BackgroundTaskBuilder();

    builder.Name = "MyDownloadProcessingTask";
    builder.SetTrigger(completionGroup.Trigger);
    builder.TaskEntryPoint = "Tasks.BackgroundDownloadProcessingTask";

    BackgroundTaskRegistration downloadProcessingTask = builder.Register();
    ```

2.  Next you associate background transfers with the completion group. Once all transfers are created, enable the completion group.

   ```csharp
    BackgroundDownloader downloader = new BackgroundDownloader(completionGroup);
    DownloadOperation download = downloader.CreateDownload(uri, file);
    Task<DownloadOperation> startTask = download.StartAsync().AsTask();

    // App still sees the normal completion path
    startTask.ContinueWith(ForegroundCompletionHandler);

    // Do not enable the CompletionGroup until after all downloads are created.
    downloader.CompletinGroup.Enable();
    ```

3.  The code in the background task extracts the list of operations from the trigger details, and your code can then inspect the details for each operation and perform appropriate post-processing for each operation.

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

The post-processing task is a regular background task. It is part of the pool of all background tasks, and it is subject to the same resource management policy as all background tasks.

Also, note that post-processing does not replace foreground completion handlers. If your app defines a foreground completion handler, and your app is running when the file transfer completes, then both your foreground completion handler and your background completion handler will be called. The order in which foreground and background tasks are called is not guaranteed. If you define both, you should ensure that the two tasks will work properly and not interfere with each other if they are running concurrently.

## Request timeouts

There are two primary connection timeout scenarios to take into consideration:

-   When establishing a new connection for a transfer, the connection request is aborted if it is not established within five minutes.

-   After a connection has been established, an HTTP request message that has not received a response within two minutes is aborted.

> **Note**  In either scenario, assuming there is Internet connectivity, Background Transfer will retry a request up to three times automatically. In the event Internet connectivity is not detected, additional requests will wait until it is.

## Debugging guidance

Stopping a debugging session in Microsoft Visual Studio is comparable to closing your app; PUT uploads are paused and POST uploads are terminated. Even while debugging, your app should enumerate and then restart or cancel any persisted uploads. For example, you can have your app cancel enumerated persisted upload operations at app startup if there is no interest in previous operations for that debug session.

While enumerating downloads/uploads on app startup during a debug session, you can have your app cancel them if there is no interest in previous operations for that debug session. Note that if there are Visual Studio project updates, like changes to the app manifest, and the app is uninstalled and re-deployed, [**GetCurrentUploadsAsync**](https://msdn.microsoft.com/library/windows/apps/hh701149) cannot enumerate operations created using the previous app deployment.

When using Background Transfer during development, you may get into a situation where the internal caches of active and completed transfer operations can get out of sync. This may result in the inability to start new transfer operations or interact with existing operations and [**BackgroundTransferGroup**](https://msdn.microsoft.com/library/windows/apps/dn279030) objects. In some cases, attempting to interact with existing operations may trigger a crash. This result can occur if the [**TransferBehavior**](https://msdn.microsoft.com/library/windows/apps/dn279033) property is set to **Parallel**. This issue occurs only in certain scenarios during development and is not applicable to end users of your app.

Four scenarios using Visual Studio can cause this issue.

-   You create a new project with the same app name as an existing project, but a different language (from C++ to C#, for example).
-   You change the target architecture (from x86 to x64, for example) in an existing project.
-   You change the culture (from neutral to en-US, for example) in an existing project.
-   You add or remove a capability in the package manifest (adding **Enterprise Authentication**, for example) in an existing project.

Regular app servicing, including manifest updates which add or remove capabilities, do not trigger this issue on end user deployments of your app.
To work around this issue, completely uninstall all versions of the app and re-deploy with the new language, architecture, culture, or capability. This can be done via the **Start** screen or using PowerShell and the **Remove-AppxPackage** cmdlet.

## Exceptions in Windows.Networking.BackgroundTransfer

An exception is thrown when an invalid string for a the Uniform Resource Identifier (URI) is passed to the constructor for the [**Windows.Foundation.Uri**](https://msdn.microsoft.com/library/windows/apps/br225998) object.

**.NET:  **The [**Windows.Foundation.Uri**](https://msdn.microsoft.com/library/windows/apps/br225998) type appears as [**System.Uri**](https://msdn.microsoft.com/library/windows/apps/xaml/system.uri.aspx) in C# and VB.

In C# and Visual Basic, this error can be avoided by using the [**System.Uri**](https://msdn.microsoft.com/library/windows/apps/xaml/system.uri.aspx) class in the .NET 4.5 and one of the [**System.Uri.TryCreate**](https://msdn.microsoft.com/library/windows/apps/xaml/system.uri.trycreate.aspx) methods to test the string received from the app user before the URI is constructed.

In C++, there is no method to try and parse a string to a URI. If an app gets input from the user for the [**Windows.Foundation.Uri**](https://msdn.microsoft.com/library/windows/apps/br225998), the constructor should be in a try/catch block. If an exception is thrown, the app can notify the user and request a new hostname.

The [**Windows.Networking.backgroundTransfer**](https://msdn.microsoft.com/library/windows/apps/br207242) namespace has convenient helper methods and uses enumerations in the [**Windows.Networking.Sockets**](https://msdn.microsoft.com/library/windows/apps/br226960) namespace for handling errors. This can be useful for handling specific network exceptions differently in your app.

An error encountered on an asynchronous method in the [**Windows.Networking.backgroundTransfer**](https://msdn.microsoft.com/library/windows/apps/br207242) namespace is returned as an **HRESULT** value. The [**BackgroundTransferError.GetStatus**](https://msdn.microsoft.com/library/windows/apps/hh701093) method is used to convert a network error from a background transfer operation to a [**WebErrorStatus**](https://msdn.microsoft.com/library/windows/apps/hh747818) enumeration value. Most of the **WebErrorStatus** enumeration values correspond to an error returned by the native HTTP or FTP client operation. An app can filter on specific **WebErrorStatus** enumeration values to modify app behavior depending on the cause of the exception.

For parameter validation errors, an app can also use the **HRESULT** from the exception to learn more detailed information on the error that caused the exception. Possible **HRESULT** values are listed in the *Winerror.h* header file. For most parameter validation errors, the **HRESULT** returned is **E\_INVALIDARG**.



<!--HONumber=Mar16_HO1-->


