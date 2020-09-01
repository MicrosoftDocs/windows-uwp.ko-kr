---
ms.assetid: ''
description: 게임 비디오, 오디오 및 스크린샷을 캡처하는 방법 및 시스템이 캡처 및 브로드캐스트 미디어에 포함 하는 메타 데이터를 제출 하는 방법에 대해 알아봅니다.
title: 게임 오디오, 비디오, 스크린샷 및 메타데이터 캡처
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, 게임, 캡처, 오디오, 비디오, 메타 데이터
ms.localizationpriority: medium
ms.openlocfilehash: b8c7c285f95302d5aa78a9f4e2d5ab33a206ee62
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89173187"
---
# <a name="capture-game-audio-video-screenshots-and-metadata"></a>게임 오디오, 비디오, 스크린샷 및 메타데이터 캡처
이 문서에서는 게임 비디오, 오디오 및 스크린샷을 캡처하는 방법 및 시스템에서 캡처 및 브로드캐스트 미디어에 포함할 메타 데이터를 제출 하는 방법에 대해 설명 합니다. 그러면 앱 및 기타 사용자가 게임 플레이 이벤트와 동기화 되는 동적 환경을 만들 수 있습니다. 

UWP 앱에서 게임을 캡처할 수 있는 두 가지 방법이 있습니다. 사용자는 기본 제공 시스템 UI를 사용 하 여 캡처를 시작할 수 있습니다. 이 기술을 사용 하 여 캡처한 미디어는 Microsoft 게임 에코 시스템에 수집, Xbox 앱과 같은 자사 환경을 통해 보고 공유할 수 있으며, 앱 또는 사용자에 게 직접 사용할 수 않습니다. 이 문서의 첫 번째 섹션에서는 시스템 구현 앱 캡처를 사용 및 사용 하지 않도록 설정 하는 방법과 앱 캡처가 시작 되거나 중지 될 때 알림을 수신 하는 방법을 보여 줍니다.

미디어를 캡처하는 다른 방법은 **[Windows.](/uwp/api/windows.media.apprecording)** p a p. a p a. a p a의 api를 사용 하는 것입니다. 장치에서 캡처를 사용 하도록 설정한 경우 앱에서 게임 플레이 캡처를 시작한 다음, 시간이 지난 후에는 미디어를 파일에 기록 하는 시점에서 캡처를 중지할 수 있습니다. 사용자가 기록 캡처를 사용 하도록 설정한 경우 과거의 시작 시간 및 기록할 기간을 지정 하 여 이미 발생 한 게임을 기록할 수도 있습니다. 이러한 두 가지 방법 모두 앱에서 액세스할 수 있는 비디오 파일을 생성 하 고, 사용자가 파일을 저장 하도록 선택한 위치에 따라 생성 됩니다. 이 문서의 가운데 섹션은 이러한 시나리오를 구현 하는 과정을 안내 합니다.

**[Windows. Capture](/uwp/api/windows.media.capture)** 네임 스페이스는 캡처 또는 방송 플레이를 설명 하는 메타 데이터를 만들기 위한 api를 제공 합니다. 여기에는 텍스트 또는 숫자 값이 포함 될 수 있으며 각 데이터 항목을 식별 하는 텍스트 레이블이 포함 됩니다. 메타 데이터는 단일 순간에 발생 하는 "이벤트"를 나타낼 수 있습니다. 예를 들어, 사용자가 레이스 게임에서 랩을 완료 하거나 사용자가 게임 중인 현재 게임 맵과 같이 특정 기간 동안 지속 되는 "상태"를 나타낼 수 있습니다. 메타 데이터는 시스템에서 앱에 대해 할당 되 고 관리 되는 캐시에 기록 됩니다. 메타 데이터는 기본 제공 시스템 캡처 또는 사용자 지정 앱 캡처 기법을 포함 하 여 브로드캐스트 스트림과 캡처한 비디오 파일에 포함 됩니다. 이 문서의 마지막 섹션에서는 게임 플레이 메타 데이터를 작성 하는 방법을 보여 줍니다.

> [!NOTE] 
> 게임 플레이 메타 데이터는 네트워크를 통해 잠재적으로 공유 될 수 있는 미디어 파일에 포함할 수 있으므로 사용자의 제어를 받지 않으므로 개인 식별 정보 또는 기타 잠재적으로 중요 한 데이터를 메타 데이터에 포함 해서는 안 됩니다.


## <a name="enable-and-disable-system-app-capture"></a>시스템 앱 캡처 사용 및 사용 안 함
시스템 앱 캡처는 기본 제공 시스템 UI를 사용 하 여 사용자에 의해 시작 됩니다. 파일은 Windows 게임 에코 시스템에 의해 수집, Xbox 앱과 같은 자사 환경을 사용 하는 경우를 제외 하 고 앱 또는 사용자가 사용할 수 없습니다. 앱에서 시스템이 시작한 앱 캡처를 사용 하지 않도록 설정 하 고 사용 하도록 설정 하 여 사용자가 특정 콘텐츠나 게임을 캡처하지 못하도록 방지할 수 있습니다. 

시스템 앱 캡처를 사용 하거나 사용 하지 않도록 설정 하려면 정적 메서드 **[Appcapture. SetAllowedAsync](/uwp/api/windows.media.capture.appcapture.setallowedasync)** 를 호출 하 고 캡처를 사용 하지 않도록 설정 하려면 **false** 를 전달 하 여 **캡처를 사용** 하도록 설정 합니다.

[!code-cpp[SetAppCaptureAllowed](./code/AppRecordingExample/cpp/AppRecordingExample/App.cpp#SnippetSetAppCaptureAllowed)]


## <a name="receive-notifications-when-system-app-capture-starts-and-stops"></a>시스템 앱 캡처가 시작 및 중지 될 때 알림 받기
시스템 앱 캡처를 시작 하거나 종료할 때 알림을 받으려면 먼저 팩터리 메서드 **[GetForCurrentView](/uwp/api/windows.media.capture.appcapture.GetForCurrentView)** 를 호출 하 여 **[appcapture](/uwp/api/windows.media.capture.appcapture)** 클래스의 인스턴스를 가져옵니다. 다음으로 **[Capturingchanged](/uwp/api/windows.media.capture.appcapture.CapturingChanged)** 이벤트에 대 한 처리기를 등록 합니다.

[!code-cpp[RegisterCapturingChanged](./code/AppRecordingExample/cpp/AppRecordingExample/App.cpp#SnippetRegisterCapturingChanged)]

**Capturingchanged** 이벤트에 대 한 처리기에서 **[IsCapturingAudio](/uwp/api/windows.media.capture.appcapture.IsCapturingAudio)** 및 **[IsCapturingVideo](/uwp/api/windows.media.capture.appcapture.IsCapturingVideo)** 속성을 확인 하 여 오디오나 비디오가 각각 캡처되고 있는지 확인할 수 있습니다. 현재 캡처 상태를 나타내기 위해 응용 프로그램의 UI를 업데이트할 수 있습니다.

[!code-cpp[OnCapturingChanged](./code/AppRecordingExample/cpp/AppRecordingExample/App.cpp#SnippetOnCapturingChanged)]

## <a name="add-the-windows-desktop-extensions-for-the-uwp-to-your-app"></a>앱에 UWP 용 Windows 데스크톱 확장 추가
오디오 및 비디오 기록을 위한 Api와 앱에서 직접 스크린샷 캡처에 대 한 Api는 유니버설 API 계약에 포함 되지 **[않습니다.](/uwp/api/windows.media.apprecording)** Api에 액세스 하려면 다음 단계를 사용 하 여 UWP에 대 한 Windows 데스크톱 확장에 대 한 참조를 앱에 추가 해야 합니다.

1. Visual Studio의 **솔루션 탐색기**에서 UWP 프로젝트를 확장 하 고 **참조** 를 마우스 오른쪽 단추로 클릭 한 다음 **참조 추가**...를 선택 합니다. 
2. **유니버설 Windows** 노드를 확장 하 고 **확장**을 선택 합니다.
3. 확장 목록에서 프로젝트에 대 한 대상 빌드와 일치 하는 **UWP 항목에 대 한 Windows 데스크톱 확장** 옆의 확인란을 선택 합니다. 앱 브로드캐스트 기능의 경우 버전은 1709 이상 이어야 합니다.
4. **확인**을 클릭합니다.

## <a name="get-an-instance-of-apprecordingmanager"></a>AppRecordingManager 인스턴스 가져오기
**[Apprecordingmanager](/uwp/api/windows.media.apprecording.apprecordingmanager)** 클래스는 앱 기록을 관리 하는 데 사용할 중앙 API입니다. 팩터리 메서드 **[Getdefault](/uwp/api/windows.media.apprecording.apprecordingmanager.GetDefault)** 를 호출 하 여이 클래스의 인스턴스를 가져옵니다. **Windows.** p r a n a m e. p a r a c. Windows 10, 버전 1709 이전 버전의 OS를 실행 하는 장치에서는 Api를 사용할 수 없습니다. 특정 OS 버전을 확인 하는 대신 IsApiContractPresent 메서드를 사용 하 여 *Windows.* **[ApiInformation.IsApiContractPresent](/uwp/api/windows.foundation.metadata.apiinformation.isapicontractpresent)** 메서드를 사용 하 여 해당 버전 1.0을 쿼리 합니다. 이 계약이 있는 경우 장치에서 기록 Api를 사용할 수 있습니다. 이 문서의 예제 코드는 Api를 한 번 확인 한 다음 후속 작업 전에 **Apprecordingmanager** 가 null 인지 확인 합니다.

[!code-cpp[GetAppRecordingManager](./code/AppRecordingExample/cpp/AppRecordingExample/App.cpp#SnippetGetAppRecordingManager)]

## <a name="determine-if-your-app-can-currently-record"></a>앱이 현재 녹음할 수 있는지 확인
현재 장치가 기록을 위한 하드웨어 요구 사항을 충족 하지 않거나 다른 앱이 현재 브로드캐스팅 중인 경우를 포함 하 여 앱에서 현재 오디오 또는 비디오를 캡처할 수 없는 몇 가지 이유가 있습니다. 기록을 시작 하기 전에 앱이 현재 녹음할 수 있는지 확인할 수 있습니다. **Apprecordingmanager** 개체의 **[GetStatus](/uwp/api/windows.media.apprecording.apprecordingmanager.GetStatus)** 메서드를 호출한 다음 반환 된 **[Apprecordingmanager](/uwp/api/windows.media.apprecording.apprecordingstatus)** 개체의 **[canrecord](/uwp/api/windows.media.apprecording.apprecordingstatus.CanRecord)** 속성을 선택 합니다. **Canrecord** 에서 **false**를 반환 하는 경우 앱이 현재 녹음할 수 없음을 의미 하는 **[Details](/uwp/api/windows.media.apprecording.apprecordingstatus.Details)** 속성을 확인 하 여 이유를 확인할 수 있습니다. 이유에 따라 사용자에 게 상태를 표시 하거나 앱 기록을 사용 하도록 설정 하는 지침을 표시할 수 있습니다.



[!code-cpp[CanRecord](./code/AppRecordingExample/cpp/AppRecordingExample/App.cpp#SnippetCanRecord)]

## <a name="manually-start-and-stop-recording-your-app-to-a-file"></a>수동으로 응용 프로그램을 시작 하 고 파일에 기록 중지

앱이 녹음할 수 있는지 확인 한 후 **Apprecordingmanager** 개체의 **[StartRecordingToFileAsync](/uwp/api/windows.media.apprecording.apprecordingmanager.startrecordingtofileasync)** 메서드를 호출 하 여 새 기록을 시작할 수 있습니다.

다음 예제에서 첫 번째 **then** 블록은 비동기 태스크가 실패할 때 실행 됩니다. **Then** 블록은 작업 결과에 대 한 액세스를 시도 하 고 결과가 null 이면 태스크가 완료 된 것입니다. 두 경우 모두 아래에 표시 된 **Onrecordingcomplete** 도우미 메서드를 호출 하 여 결과를 처리 합니다. 

[!code-cpp[StartRecordToFile](./code/AppRecordingExample/cpp/AppRecordingExample/App.cpp#SnippetStartRecordToFile)]

기록 작업이 완료 되 면 반환 된 **[Apprecordingresult](/uwp/api/windows.media.apprecording.apprecordingresult)** 개체의 **[Succeeded](/uwp/api/windows.media.apprecording.apprecordingresult.Succeeded)** 속성을 확인 하 여 레코드 작업이 성공 했는지 확인 합니다. 그렇다면 **[IsFileTruncated](/uwp/api/windows.media.apprecording.apprecordingresult.IsFileTruncated)** 속성을 확인 하 여 시스템에서 캡처된 파일을 강제로 잘라내야 하는지 여부를 확인할 수 있습니다. **[Duration](/uwp/api/windows.media.apprecording.apprecordingresult.Duration)** 속성을 확인 하 여 기록 된 파일의 실제 기간을 검색할 수 있습니다. 즉, 파일이 잘린 경우 기록 작업 기간 보다 짧을 수 있습니다.

[!code-cpp[OnRecordingComplete](./code/AppRecordingExample/cpp/AppRecordingExample/App.cpp#SnippetOnRecordingComplete)]

다음 예제에서는 이전 예제에 표시 된 기록 작업을 시작 및 중지 하기 위한 몇 가지 기본 코드를 보여 줍니다.

[!code-cpp[CallStartRecordToFile](./code/AppRecordingExample/cpp/AppRecordingExample/App.cpp#SnippetCallStartRecordToFile)]

[!code-cpp[FinishRecordToFile](./code/AppRecordingExample/cpp/AppRecordingExample/App.cpp#SnippetFinishRecordToFile)]

## <a name="record-a-historical-time-span-to-a-file"></a>기록 시간 범위를 파일에 기록
사용자가 시스템 설정에서 앱에 대 한 기록 기록을 사용 하도록 설정 하는 경우 이전에 의심할 된 게임의 시간 범위를 기록할 수 있습니다. 이 문서의 이전 예제에서는 앱이 현재 게임을 녹음할 수 있는지 확인 하는 방법을 살펴보았습니다. 기록 캡처가 사용 되는지 여부를 확인 하는 추가 검사가 있습니다. 다시 한 번, **[GetStatus](/uwp/api/windows.media.apprecording.apprecordingmanager.GetStatus)** 를 호출 하 고 반환 된 **Apprecordingstatus** 개체의 **[canrecordtimespan](/uwp/api/windows.media.apprecording.apprecordingstatus.CanRecordTimeSpan)** 속성을 확인 합니다. 또한이 예제에서는 기록 작업의 유효한 시작 시간을 결정 하는 데 사용 되는 **Apprecordingstatus** 의 **[HistoricalBufferDuration](/uwp/api/windows.media.apprecording.apprecordingstatus.HistoricalBufferDuration)** 속성을 반환 합니다.

[!code-cpp[CanRecordTimeSpan](./code/AppRecordingExample/cpp/AppRecordingExample/App.cpp#SnippetCanRecordTimeSpan)]

기록 timespan을 캡처하려면 기록 및 기간의 시작 시간을 지정 해야 합니다. 시작 시간은 **[DateTime](/uwp/api/windows.foundation.datetime)** 구조체로 제공 됩니다. 시작 시간은 기록 기록 버퍼의 길이 내에서 현재 시간 이전의 시간 이어야 합니다. 이 예에서는 이전 코드 예제에 표시 된 기록 기록이 사용 되는지 확인 하기 위한 검사의 일부로 버퍼 길이를 검색 합니다. 기록 기록의 기간은  **[TimeSpan](/uwp/api/windows.foundation.timespan)** 구조체로 제공 되며이는 기록 버퍼 기간과 같거나 작아야 합니다. 원하는 시작 시간과 기간을 결정 했으면 **[RecordTimeSpanToFileAsync](/uwp/api/windows.media.apprecording.apprecordingmanager.recordtimespantofileasync)** 를 호출 하 여 기록 작업을 시작 합니다.

수동 시작 및 중지와 같이 기록 기록이 완료 되 면 반환 된 **[Apprecordingresult](/uwp/api/windows.media.apprecording.apprecordingresult)** 개체의 **[Succeeded](/uwp/api/windows.media.apprecording.apprecordingresult.Succeeded)** 속성을 확인 하 여 기록 작업이 성공 했는지 확인 하 고, **[IsFileTruncated](/uwp/api/windows.media.apprecording.apprecordingresult.IsFileTruncated)** 및 **[Duration](/uwp/api/windows.media.apprecording.apprecordingresult.Duration)** 속성을 선택 하 여 기록 된 파일의 실제 기간을 검색할 수 있습니다 .이 경우 파일이 잘린 경우 요청 된 기간 보다 짧을 수 있습니다.

[!code-cpp[RecordTimeSpanToFile](./code/AppRecordingExample/cpp/AppRecordingExample/App.cpp#SnippetRecordTimeSpanToFile)]

다음 예제에서는 이전 예제에 표시 된 기록 레코드 작업을 시작 하기 위한 몇 가지 기본 코드를 보여 줍니다.

[!code-cpp[CallRecordTimeSpanToFile](./code/AppRecordingExample/cpp/AppRecordingExample/App.cpp#SnippetCallRecordTimeSpanToFile)]

## <a name="save-screenshot-images-to-files"></a>스크린샷 이미지를 파일에 저장
앱은 응용 프로그램 창의 현재 콘텐츠를 하나의 이미지 파일 또는 다른 이미지 인코딩을 사용 하는 여러 이미지 파일에 저장 하는 스크린샷 캡처를 시작할 수 있습니다. 사용할 이미지 인코딩을 지정 하려면 각에서 이미지 형식을 나타내는 문자열 목록을 만듭니다. **[ImageEncodingSubtypes](/uwp/api/windows.media.mediaproperties.mediaencodingsubtypes)** 의 속성은 지원 되는 각 이미지 형식에 대 한 올바른 문자열을 제공 합니다 (예: **MediaEncodingSubtypes.Png** 또는 **MediaEncodingSubtypes JpegXr**).

**Apprecordingmanager** 개체의 **[Savescreenshottofilesasync](/uwp/api/windows.media.apprecording.apprecordingmanager.savescreenshottofilesasync)** 메서드를 호출 하 여 화면 캡처를 시작 합니다. 이 메서드의 첫 번째 매개 변수는 이미지 파일이 저장 되는 **Storagefolder** 입니다. 두 번째 매개 변수는 ".png"와 같이 저장 된 각 이미지 형식에 대 한 확장명을 시스템이 추가 하는 파일 이름 접두사입니다.

**Savescreenshottofilesasync** 에 대 한 세 번째 매개 변수는 시스템에서 캡처할 현재 창이 HDR 콘텐츠를 표시 하는 경우 적절 한 colorspace 변환을 수행할 수 있도록 하는 데 필요 합니다. HDR 콘텐츠가 있는 경우이 매개 변수는 **Apprecordingsavescreenshotoption**로 설정 해야 합니다. 그렇지 않은 경우 **Apprecordingsavescreenshotcretcretnetcretcretcretne** 메서드의 마지막 매개 변수는 화면을 캡처할 이미지 형식의 목록입니다.

**Savescreenshottofilesasync** 에 대 한 비동기 호출이 완료 되 면 저장 된 각 이미지에 대 한 이미지 형식을 나타내는 **StorageFile** 및 연결 된 **MediaEncodingSubtypes** 값을 제공 하는 **[AppRecordingSavedScreenshotInfo](/uwp/api/windows.media.apprecording.apprecordingsavedscreenshotinfo)** 개체를 반환 합니다.

[!code-cpp[SaveScreenShotToFiles](./code/AppRecordingExample/cpp/AppRecordingExample/App.cpp#SnippetSaveScreenShotToFiles)]

다음 예제에서는 이전 예제에 표시 된 스크린샷 작업을 시작 하기 위한 몇 가지 기본 코드를 보여 줍니다.

[!code-cpp[CallSaveScreenShotToFiles](./code/AppRecordingExample/cpp/AppRecordingExample/App.cpp#SnippetCallSaveScreenShotToFiles)]

## <a name="add-game-metadata-for-system-and-app-initiated-capture"></a>시스템 및 앱에서 시작한 캡처에 대 한 게임 메타 데이터 추가
이 문서의 다음 섹션에서는 시스템이 캡처 또는 브로드캐스트 게임의 MP4 스트림에 포함 하는 메타 데이터를 제공 하는 방법을 설명 합니다. 메타 데이터는 기본 제공 시스템 UI와 **Apprecordingmanager**를 사용 하 여 앱에서 캡처한 미디어를 사용 하 여 캡처되는 미디어에 포함 될 수 있습니다. 이 메타 데이터는 미디어를 재생 하는 동안 앱 및 기타 앱에서 추출할 수 있으며,이를 통해 컨텍스트 인식 환경을 제공 하 여 캡처 또는 방송 플레이와 동기화 할 수 있습니다.

### <a name="get-an-instance-of-appcapturemetadatawriter"></a>AppCaptureMetadataWriter의 인스턴스 가져오기
앱 캡처 메타 데이터를 관리 하기 위한 기본 클래스는 **[AppCaptureMetadataWriter](/uwp/api/windows.media.capture.appcapturemetadatawriter)** 입니다. 이 클래스의 인스턴스를 초기화 하기 전에 **[IsApiContractPresent](/uwp/api/windows.foundation.metadata.apiinformation.isapicontractpresent)** 메서드를 사용 하 여 *AppCaptureMetadataContract* 버전 1.0을 쿼리하여 API가 현재 장치에서 사용 가능한 지 확인 합니다.

[!code-cpp[GetMetadataWriter](./code/AppRecordingExample/cpp/AppRecordingExample/App.cpp#SnippetGetMetadataWriter)]

### <a name="write-metadata-to-the-system-cache-for-your-app"></a>앱의 시스템 캐시에 메타 데이터를 씁니다.
각 메타 데이터 항목은 메타 데이터 항목을 식별 하는 문자열 레이블, 문자열, 정수 또는 double 값이 될 수 있는 연결 된 데이터 값, 데이터 항목의 상대적 우선 순위를 나타내는 **[AppCaptureMetadataPriority](/uwp/api/windows.media.capture.appcapturemetadatapriority)** 열거의 값을 포함 합니다. 메타 데이터 항목은 단일 시점에 발생 하는 "이벤트" 또는 시간 창에서 값을 유지 하는 "상태"로 간주할 수 있습니다. 메타 데이터는 시스템에서 앱에 대해 할당 되 고 관리 되는 메모리 캐시에 기록 됩니다. 시스템은 메타 데이터 메모리 캐시에 크기 제한을 적용 하며, 한도에 도달 하면 각 메타 데이터 항목이 작성 된 우선 순위에 따라 데이터를 제거 합니다. 이 문서의 다음 섹션에서는 앱의 메타 데이터 메모리 할당을 관리 하는 방법을 보여 줍니다.

일반적인 앱은 캡처 세션의 시작 부분에 일부 메타 데이터를 작성 하 여 후속 데이터에 대 한 일부 컨텍스트를 제공할 수 있습니다. 이 시나리오에서는 즉시 "이벤트" 데이터를 사용 하는 것이 좋습니다. 이 예에서는 **[Addstringevent](/uwp/api/windows.media.capture.appcapturemetadatawriter.addstringevent)**, **[AddDoubleEvent](/uwp/api/windows.media.capture.appcapturemetadatawriter.adddoubleevent)** 및 **[AddInt32Event](/uwp/api/windows.media.capture.appcapturemetadatawriter.addint32event)** 를 호출 하 여 각 데이터 형식에 대 한 순간 값을 설정 합니다.

[!code-cpp[StartSession](./code/AppRecordingExample/cpp/AppRecordingExample/App.cpp#SnippetStartSession)]

시간이 지남에 따라 유지 되는 "상태" 데이터를 사용 하는 일반적인 시나리오는 플레이어가 현재 내에 있는 게임 지도의 추적 하는 것입니다. 이 예에서는 **[Startstringstate](/uwp/api/windows.media.capture.appcapturemetadatawriter.startstringstate)** 를 호출 하 여 상태 값을 설정 합니다. 

[!code-cpp[StartMap](./code/AppRecordingExample/cpp/AppRecordingExample/App.cpp#SnippetStartMap)]

**[Stopstate](/uwp/api/windows.media.capture.appcapturemetadatawriter.stopstate)** 를 호출 하 여 특정 상태가 종료 되었음을 기록 합니다.

[!code-cpp[EndMap](./code/AppRecordingExample/cpp/AppRecordingExample/App.cpp#SnippetEndMap)]

기존 상태 레이블로 새 값을 설정 하 여 상태를 덮어쓸 수 있습니다.

[!code-cpp[LevelUp](./code/AppRecordingExample/cpp/AppRecordingExample/App.cpp#SnippetLevelUp)]

**[Stopallstates](/uwp/api/windows.media.capture.appcapturemetadatawriter.StopAllStates)** 를 호출 하 여 현재 열려 있는 상태를 모두 종료할 수 있습니다.

[!code-cpp[RaceComplete](./code/AppRecordingExample/cpp/AppRecordingExample/App.cpp#SnippetRaceComplete)]

### <a name="manage-metadata-cache-storage-limit"></a>메타 데이터 캐시 저장소 제한 관리
**AppCaptureMetadataWriter** 를 사용 하 여 작성 하는 메타 데이터는 연결 된 미디어 스트림에 기록 될 때까지 시스템에 의해 캐시 됩니다. 시스템은 각 앱의 메타 데이터 캐시에 대 한 크기 제한을 정의 합니다. 캐시 크기 제한에 도달 하면 시스템에서 캐시 된 메타 데이터 제거를 시작 합니다. 시스템에서는 AppCaptureMetadataPriority 우선 순위를 사용 하 여 메타 데이터를 삭제 하기 전에 **[AppCaptureMetadataPriority](/uwp/api/windows.media.capture.appcapturemetadatapriority)** 우선 순위 값으로 작성 된 메타 데이터를 삭제 **[합니다.](/uwp/api/windows.media.capture.appcapturemetadatapriority)**

언제 든 지 **[RemainingStorageBytesAvailable](/uwp/api/windows.media.capture.appcapturemetadatawriter.RemainingStorageBytesAvailable)** 를 호출 하 여 앱의 메타 데이터 캐시에서 사용할 수 있는 바이트 수를 확인할 수 있습니다. 사용자 고유의 앱 정의 임계값을 설정 하도록 선택할 수 있으며, 그 후에는 캐시에 작성 하는 메타 데이터의 양을 줄일 수 있습니다. 다음 예제에서는이 패턴의 간단한 구현을 보여 줍니다.

[!code-cpp[CheckMetadataStorage](./code/AppRecordingExample/cpp/AppRecordingExample/App.cpp#SnippetCheckMetadataStorage)]

[!code-cpp[ComboExecuted](./code/AppRecordingExample/cpp/AppRecordingExample/App.cpp#SnippetComboExecuted)]

### <a name="receive-notifications-when-the-system-purges-metadata"></a>시스템에서 메타 데이터를 제거 하는 경우 알림 받기
시스템에서 **[MetadataPurged](/uwp/api/windows.media.capture.appcapturemetadatawriter.MetadataPurged)** 이벤트에 대 한 처리기를 등록 하 여 앱에 대 한 메타 데이터 제거를 시작할 때 알림을 받도록 등록할 수 있습니다.

[!code-cpp[RegisterMetadataPurged](./code/AppRecordingExample/cpp/AppRecordingExample/App.cpp#SnippetRegisterMetadataPurged)]

**MetadataPurged** 이벤트에 대 한 처리기에서 낮은 우선 순위 상태를 종료 하 여 메타 데이터 캐시의 공간을 지우거 나, 캐시에 작성 하는 메타 데이터의 양을 줄이기 위해 앱 정의 논리를 구현할 수 있습니다. 또는 아무 작업도 수행 하지 않고 시스템이 작성 된 우선 순위에 따라 캐시를 계속 제거 하도록 할 수 있습니다.

[!code-cpp[OnMetadataPurged](./code/AppRecordingExample/cpp/AppRecordingExample/App.cpp#SnippetOnMetadataPurged)]

## <a name="related-topics"></a>관련 항목

* [게임](index.md)
 

 