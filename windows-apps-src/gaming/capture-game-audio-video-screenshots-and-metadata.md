---
author: drewbatgit
ms.assetid: ''
description: UWP 앱의 게임 오디오, 비디오 및 메타데이터를 기록하는 방법을 보여 줍니다.
title: 게임 오디오, 비디오, 스크린샷 및 메타데이터 캡처
ms.author: drewbat
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, 게임, 캡처, 오디오, 비디오, 메타데이터
ms.localizationpriority: medium
ms.openlocfilehash: 6c1641cb4c50b86d7f678bf18fa85ad0215b4b15
ms.sourcegitcommit: 086001cffaf436e6e4324761d59bcc5e598c15ea
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/26/2018
ms.locfileid: "5694215"
---
# <a name="capture-game-audio-video-screenshots-and-metadata"></a>게임 오디오, 비디오, 스크린샷 및 메타데이터 캡처
이 문서는 앱 등에서 게임 플레이 이벤트가 동기화되는 동적 환경을 만들 수 있도록 게임 비디오, 오디오, 스크린샷을 캡처하는 방법과 시스템이 캡처된 미디어 및 브로드캐스트 미디어에 포함할 메타데이터를 전송하는 방법에 대해 설명합니다. 

UWP 앱에서 게임 플레이를 캡처하는 방법은 두 가지가 있습니다. 사용자는 기본 제공된 시스템 UI를 사용하여 캡처를 시작할 수 있습니다. 이 기술을 사용하여 캡처한 미디어는 Microsoft 게임 에코시스템으로 수집되며 XBox 앱 같은 자사 환경을 통해 보고 공유할 수 있으며 앱 또는 사용자에서 직접 사용할 수 없습니다. 이 문서의 첫 번째 섹션에서는 시스템 구현 앱 캡처를 사용하거나 사용하지 않도록 설정하는 방법 및 앱 캡처를 시작하거나 중지 하는 경우 알림을 받는 방법을 보여 줍니다.

미디어를 캡처하는 다른 방법은 **[Windows.Media.AppRecording](https://docs.microsoft.com/uwp/api/windows.media.apprecording)** 네임스페이스의 API를 사용하는 것입니다. 디바이스에서 캡처를 사용할 수 있는 경우 앱은 게임 플레이 캡처를 시작하고 약간의 시간이 경과한 후 캡처를 중지할 수 있으며 이때 미디어가 파일에 기록됩니다. 사용자가 기록 캡처를 활성화한 경우 과거의 시작 시간 및 녹화 기간을 지정하여 이미 발생한 게임 플레이를 녹화할 수도 있습니다. 이러한 방법은 모두 앱에서 액세스할 수 있으며 파일을 저장하도록 선택하는 위치에 따라 사용자가 액세스할 수 있는 비디오 파일을 생성합니다. 이 문서의 가운데 섹션은 이러한 시나리오 구현을 설명합니다.

**[Windows.Media.Capture](https://docs.microsoft.com/uwp/api/windows.media.capture)** 네임스페이스는 캡처되는 또는 브로드캐스트되는 게임 플레이를 설명하는 메타데이터를 만드는 API를 제공합니다. 이에는 각 데이터 항목을 식별하는 텍스트 레이블이 있는 텍스트 또는 숫자 값이 포함될 수 있습니다. 메타데이터는 사용자가 레이싱 게임에서 한 바퀴를 돌았거나 하는 등의 단일 모멘트에서 발생하는 "이벤트"를 나타내거나 사용자가 플레이하는 현재 게임 지도와 같이 일정 시간 동안 지속되는 "상태"를 나타낼 수 있습니다. 메타데이터는 시스템에 의해 사용자의 앱에 할당되고 관리되는 캐시에 기록됩니다. 메타데이터는 기본 제공 시스템 캡처 또는 사용자 지정 앱 캡처 기술을 각각 포함하는 브로드캐스트 스트림 및 캡처한 비디오 파일에 포함됩니다. 이 문서의 최종 섹션은 게임 플레이 메타데이터를 쓰는 방법을 보여 줍니다.

> [!NOTE] 
> 게임 플레이 메타데이터는 사용자의 제어를 벗어난 네트워크를 통해 잠재적으로 공유될 수 있는 미디어 파일에 포함될 수 있으므로 메타데이터에 식별할 수 있는 개인 정보 또는 잠재적으로 중요한 기타 데이터를 포함해서는 안됩니다.


## <a name="enable-and-disable-system-app-capture"></a>시스템 앱 캡처 사용 및 사용 안 함
시스템 앱 캡처는 사용자에 의해 기본 제공 시스템 UI로 시작됩니다. 파일은 Windows 게임 에코시스템에 의해 수집되며 XBox 앱 등의 자사 환경을 통해서를 제외하고 앱 또는 사용자에게 제공되지 않습니다. 앱에서 시스템 시작 앱 캡처를 사용하거나 사용하지 않도록 설정하여 사용자가 특정 콘텐츠나 게임 플레이를 캡처하지 못하게 할 수 있습니다. 

시스템 앱 캡처를 사용하거나 사용하지 않도록 설정하려면 정적 메서드를 **[AppCapture.SetAllowedAsync](https://docs.microsoft.com/uwp/api/windows.media.capture.appcapture.setallowedasync)** 를 호출하고 **false**를 전달하여 캡처를 사용하지 않도록 설정하거나 **true**를 전달하여 캡처를 사용하도록 할 수 있습니다.

[!code-cpp[SetAppCaptureAllowed](./code/AppRecordingExample/cpp/AppRecordingExample/App.cpp#SnippetSetAppCaptureAllowed)]


## <a name="receive-notifications-when-system-app-capture-starts-and-stops"></a>시스템 앱 캡처를 시작하거나 중지하는 경우 알림 수신
시스템 앱 캡처를 시작하거나 종료하는 경우 알림을 받으려면 먼저 **[GetForCurrentView](https://docs.microsoft.com/uwp/api/windows.media.capture.appcapture.GetForCurrentView)** 팩터리 메서드를 호출하여 **[AppCapture](https://docs.microsoft.com/uwp/api/windows.media.capture.appcapture)** 클래스를 인스턴스를 가져옵니다. 다음으로 **[CapturingChanged](https://docs.microsoft.com/uwp/api/windows.media.capture.appcapture.CapturingChanged)** 이벤트에 대한 처리기를 등록합니다.

[!code-cpp[RegisterCapturingChanged](./code/AppRecordingExample/cpp/AppRecordingExample/App.cpp#SnippetRegisterCapturingChanged)]

오디오 또는 비디오가 각각 캡처되는지 확인하기 위해 **CapturingChanged** 이벤트에 대한 처리기에서 **[IsCapturingAudio](https://docs.microsoft.com/uwp/api/windows.media.capture.appcapture.IsCapturingAudio)** 및 **[IsCapturingVideo](https://docs.microsoft.com/uwp/api/windows.media.capture.appcapture.IsCapturingVideo)** 속성을 확인할 수 있습니다. 현재 캡처 상태를 나타내도록 앱의 UI를 업데이트하고자 할 수 있습니다.

[!code-cpp[OnCapturingChanged](./code/AppRecordingExample/cpp/AppRecordingExample/App.cpp#SnippetOnCapturingChanged)]

## <a name="add-the-windows-desktop-extensions-for-the-uwp-to-your-app"></a>앱에 UWP용 Windows 데스크톱 확장 추가
**[Windows.Media.AppRecording](https://docs.microsoft.com/uwp/api/windows.media.apprecording)** 네임스페이스에서 찾을 수 있는 오디오 및 비디오를 녹화/녹음하고, 앱에서 직접 스크린샷을 캡처하는 API는 유니버설 API 계약에 포함되지 않습니다. API에 액세스하려면 다음 단계를 따라 앱에 UWP용 Windows 데스크톱 확장에 대한 참조를 앱 추가해야 합니다.

1. Visual Studio에서 **솔루션 탐색기**의 UWP 프로젝트를 확장하고 **참조**를 마우스 오른쪽 단추로 클릭하고 **참조 추가...** 를 선택합니다. 
2. **유니버설 Windows** 노드를 확장하고 **확장**을 선택합니다.
3. 확장 목록에서 프로젝트의 대상 빌드와 일치하는 **UWP용 Windows 데스크톱 확장** 항목 옆에 있는 확인란을 선택합니다. 앱 브로드캐스트 기능의 경우 버전이 1709 이상이어야 합니다.
4. **확인**을 클릭합니다.

## <a name="get-an-instance-of-apprecordingmanager"></a>AppRecordingManager 인스턴스 가져오기
**[AppRecordingManager](https://docs.microsoft.com/uwp/api/windows.media.apprecording.apprecordingmanager)** 클래스는 앱 녹음/녹화 관리에 사용할 중앙 API입니다. 팩터리 메서드 **[GetDefault](https://docs.microsoft.com/uwp/api/windows.media.apprecording.apprecordingmanager.GetDefault)** 를 호출하여 이 클래스의 인스턴스를 가져옵니다. **Windows.Media.AppRecording** 네임스페이스에서 API를 사용하기 전에 현재 장치에서 해당 API가 존재하는지 확인해야 합니다. API는 Windows 10 버전 1709 이전의 운영 체제 버전을 실행하는 디바이스에서는 사용할 수 없습니다. 특정 OS 버전을 확인하는 것보다 **[ApiInformation.IsApiContractPresent](https://docs.microsoft.com/uwp/api/windows.foundation.metadata.apiinformation.isapicontractpresent)** 메서드를 사용하여 *Windows.Media.AppBroadcasting.AppRecordingContract* 버전 1.0을 쿼리하는 것이 좋습니다. 이 계약이 있는 경우 녹음/녹화 API는 디바이스에서 사용할 수 있습니다. 이 문서의 예 코드는 한 번 API를 확인하고 이후 작업 이전에 **AppRecordingManager**가 null인지 확인합니다.

[!code-cpp[GetAppRecordingManager](./code/AppRecordingExample/cpp/AppRecordingExample/App.cpp#SnippetGetAppRecordingManager)]

## <a name="determine-if-your-app-can-currently-record"></a>현재 앱이 기록할 수 있는지 여부 확인
현재 기기가 녹음/녹화의 하드웨어 요구 사항을 충족하지 않거나 다른 앱이 현재 브로드캐스팅 중인 경우를 포함하여 현재 사용자의 앱이 오디오 또는 비디오를 캡처하지 못하는 이유가 여러 가지 있을 수 있습니다. 녹음/녹화를 시작하기 전에 현재 앱이 녹음/녹화를 지원하는지 확인할 수 있습니다. **AppRecordingManager** 개체의 **[GetStatus](https://docs.microsoft.com/uwp/api/windows.media.apprecording.apprecordingmanager.GetStatus)** 메서드를 호출한 다음 반환된 **[AppRecordingStatus](https://docs.microsoft.com/uwp/api/windows.media.apprecording.apprecordingstatus)** 개체의 **[CanRecord](https://docs.microsoft.com/uwp/api/windows.media.apprecording.apprecordingstatus.CanRecord)** 속성을 확인합니다. **CanRecord**가 **false**를 반환하는 경우 현재 앱에서 녹음/녹화할 수 없다는 것을 의미하며 그 이유는 **[세부 정보](https://docs.microsoft.com/uwp/api/windows.media.apprecording.apprecordingstatus.Details)** 속성에서 확인할 수 있습니다. 이유에 따라 사용자에게 상태를 표시하거나 앱 녹음/녹화 사용에 대한 지침을 표시할 수 있습니다.



[!code-cpp[CanRecord](./code/AppRecordingExample/cpp/AppRecordingExample/App.cpp#SnippetCanRecord)]

## <a name="manually-start-and-stop-recording-your-app-to-a-file"></a>수동으로 파일에 앱 녹화 시작 및 중지

앱이 녹화/녹음할 수 있음을 확인한 후 **AppRecordingManager** 개체의 **[StartRecordingToFileAsync](https://docs.microsoft.com/uwp/api/windows.media.apprecording.apprecordingmanager.startrecordingtofileasync)** 메서드를 호출하여 새 녹음/녹화를 시작할 수 있습니다.

다음 예제에서 비동기 작업이 실패하는 경우 첫 번째 **then** 블록이 실행됩니다. 두 번째 **then** 블록은 작업 결과에 액세스하려고 시도하고 결과가 null인 경우 작업이 완료됩니다. 두 경우 모두 아래와 같이 **OnRecordingComplete** 도우미 메서드가 결과를 처리하기 위해 호출됩니다. 

[!code-cpp[StartRecordToFile](./code/AppRecordingExample/cpp/AppRecordingExample/App.cpp#SnippetStartRecordToFile)]

녹음/녹화 작업이 완료되면 반환된 **[AppRecordingResult](https://docs.microsoft.com/uwp/api/windows.media.apprecording.apprecordingresult)** 개체의 **[Succeeded](https://docs.microsoft.com/uwp/api/windows.media.apprecording.apprecordingresult.Succeeded)** 속성을 확인하여 녹음/녹화 작업이 성공했는지 확인합니다. 성공했다면 **[IsFileTruncated](https://docs.microsoft.com/uwp/api/windows.media.apprecording.apprecordingresult.IsFileTruncated)** 속성을 확인할 수 있으며 저장소 이유로 시스템에서 캡처한 파일을 잘랐을 수도 있습니다. **[Duration](https://docs.microsoft.com/uwp/api/windows.media.apprecording.apprecordingresult.Duration)** 속성을 확인하여 실제 녹음/녹화된 파일의 길이를 파악할 수 있으며 파일이 잘린 경우 녹음/녹화 작업의 길이보다 짧을 수 있습니다.

[!code-cpp[OnRecordingComplete](./code/AppRecordingExample/cpp/AppRecordingExample/App.cpp#SnippetOnRecordingComplete)]

다음 예제는 이전 예에서 살펴본 녹음/녹화 작업을 시작 및 중지하는 몇 가지 기본 코드를 보여 줍니다.

[!code-cpp[CallStartRecordToFile](./code/AppRecordingExample/cpp/AppRecordingExample/App.cpp#SnippetCallStartRecordToFile)]

[!code-cpp[FinishRecordToFile](./code/AppRecordingExample/cpp/AppRecordingExample/App.cpp#SnippetFinishRecordToFile)]

## <a name="record-a-historical-time-span-to-a-file"></a>파일에 기록 시간 범위 녹음/녹화
사용자가 시스템 설정에서 앱에 대한 기록 녹음/녹화를 활성화한 경우 이전의 게임 플레이의 시간 범위를 녹음/녹화할 수 있습니다. 이 문서의 이전 예제는 현재 앱에서 게임 플레이를 녹음/녹화할 수 있는지 확인하는 방법을 설명했습니다. 기록 캡처가 활성화되었는지 파악하기 위한 추가 확인이 있습니다. 다시 한 번 **[GetStatus](https://docs.microsoft.com/uwp/api/windows.media.apprecording.apprecordingmanager.GetStatus)** 를 호출하고 반환된 **AppRecordingStatus** 개체의 **[CanRecordTimeSpan](https://docs.microsoft.com/uwp/api/windows.media.apprecording.apprecordingstatus.CanRecordTimeSpan)** 속성을 확인합니다. 이 예제는 녹음/녹화 작업에 대한 올바를 시작 시간을 확인하는 데 사용될 **AppRecordingStatus**의 **[HistoricalBufferDuration](https://docs.microsoft.com/uwp/api/windows.media.apprecording.apprecordingstatus.HistoricalBufferDuration)** 속성 또한 반환합니다.

[!code-cpp[CanRecordTimeSpan](./code/AppRecordingExample/cpp/AppRecordingExample/App.cpp#SnippetCanRecordTimeSpan)]

기록 시간 범위를 캡처하려면 녹음/녹화 및 기간에 대한 시작 시간을 지정해야 합니다. 시작 시간은 **[DateTime](https://docs.microsoft.com/uwp/api/windows.foundation.datetime)** 구조로 제공됩니다. 시작 시간은 현재 시간 이전 시간이며 기록 녹음/녹화 버퍼 길이보다 작아야 합니다. 이 예의 경우 버퍼 길이는 이전 코드 예제에서 살펴본 기록 녹음/녹화가 활성화되었는지 확인하는 일환으로 검색됩니다. 기록 녹음/녹화의 기간은 **[TimeSpan](https://docs.microsoft.com/uwp/api/windows.foundation.timespan)** 구조로 제공되며 이 역시 기록 버퍼의 기간과 동일하거나 보다 작아야 합니다. 원하는 시작 시간과 기간을 확인한 후 **[RecordTimeSpanToFileAsync](https://docs.microsoft.com/uwp/api/windows.media.apprecording.apprecordingmanager.recordtimespantofileasync)** 를 호출하여 녹음/녹화 작업을 시작합니다.

수동 시작 및 중지 녹음/녹화와 같이 기록 녹음/녹화가 완료되면 반환된 **[AppRecordingResult](https://docs.microsoft.com/uwp/api/windows.media.apprecording.apprecordingresult)** 개체의 **[Succeeded](https://docs.microsoft.com/uwp/api/windows.media.apprecording.apprecordingresult.Succeeded)** 속성을 확인하여 녹음/녹화 작업이 성공적인지 파악하고 **[IsFileTruncated](https://docs.microsoft.com/uwp/api/windows.media.apprecording.apprecordingresult.IsFileTruncated)** 및 **[Duration](https://docs.microsoft.com/uwp/api/windows.media.apprecording.apprecordingresult.Duration)** 속성을 확인하여 녹음/녹화된 파일의 실제 길이를 파악할 수 있으며, 파일이 잘린 경우 요청한 기간보다 짧을 수 있습니다.

[!code-cpp[RecordTimeSpanToFile](./code/AppRecordingExample/cpp/AppRecordingExample/App.cpp#SnippetRecordTimeSpanToFile)]

다음 예제는 이전 예에서 살펴본 기록 녹음/녹화 작업을 시작하는 몇 가지 기본 코드를 보여 줍니다.

[!code-cpp[CallRecordTimeSpanToFile](./code/AppRecordingExample/cpp/AppRecordingExample/App.cpp#SnippetCallRecordTimeSpanToFile)]

## <a name="save-screenshot-images-to-files"></a>파일에 스크린샷 이미지 저장
앱은 하나의 이미지 파일이나 다른 이미지 인코딩의 여러 이미지 파일에 앱의 창 현재 내용을 저장하는 스크린샷 캡처를 시작할 수 있습니다. 사용하려는 이미지 인코딩을 지정하려면 각각 이미지 종류를 나타내는 문자열 목록을 만듭니다. **[ImageEncodingSubtypes](https://docs.microsoft.com/uwp/api/windows.media.mediaproperties.mediaencodingsubtypes)** 의 속성은 **MediaEncodingSubtypes.Png** 또는 **MediaEncodingSubtypes.JpegXr** 같은 각 지원되는 이미지 유형에 대한 올바른 문자열을 제공합니다.

**AppRecordingManager** 개체의 **[SaveScreenshotToFilesAsync](https://docs.microsoft.com/uwp/api/windows.media.apprecording.apprecordingmanager.savescreenshottofilesasync)** 메서드를 호출하여 화면 캡처를 시작합니다. 이 메서드에 대한 첫 번째 매개 변수는 이미지 파일이 저장되는 **StorageFolder**입니다. 두 번째 매개 변수는 시스템이 ".png"와 같은 각 이미지 종류별로 확장을 추가하는 파일 이름 접두사입니다.

**SaveScreenshotToFilesAsync**에 대한 세 번째 매개 변수는 캡처할 현재 창이 HDR 콘텐츠를 표시하는 경우 시스템이 적절한 색 공간 전환을 수행하기 위해 필요합니다. HDR 콘텐츠가 있는 경우 이 매개 변수를 **AppRecordingSaveScreenshotOption.HdrContentVisible**로 설정해야 합니다. 그렇지 않으면 **AppRecordingSaveScreenshotOption.None**을 사용합니다. 메서드에 대한 마지막 매개 변수는 화면에서 캡처해야 할 이미지 형식의 목록입니다.

**SaveScreenshotToFilesAsync**에 대한 비동기 호출이 완료되면 저장된 각 이미지에 대한 이미지 유형을 나타내는 **StorageFile** 및 **MediaEncodingSubtypes** 값을 제공하는 **[AppRecordingSavedScreenshotInfo](https://docs.microsoft.com/uwp/api/windows.media.apprecording.apprecordingsavedscreenshotinfo)** 개체가 반환됩니다.

[!code-cpp[SaveScreenShotToFiles](./code/AppRecordingExample/cpp/AppRecordingExample/App.cpp#SnippetSaveScreenShotToFiles)]

다음 예제는 이전 예에서 살펴본 스크린샷 작업을 시작하는 몇 가지 기본 코드를 보여 줍니다.

[!code-cpp[CallSaveScreenShotToFiles](./code/AppRecordingExample/cpp/AppRecordingExample/App.cpp#SnippetCallSaveScreenShotToFiles)]

## <a name="add-game-metadata-for-system-and-app-initiated-capture"></a>시스템 및 앱 시작 캡처에 대한 게임 메타데이터 추가
이 문서의 다음 섹션은 시스템이 캡처한 또는 브로드캐스트 게임 플레이의 MP4 스트림에 포함할 메타데이터를 제공하는 방법을 설명합니다. 메타데이터는 기본 제공 시스템 UI를 사용하여 캡처된 미디어와 **AppRecordingManager**로 앱에서 캡처된 미디어에 포함됩니다. 이 메타데이터는 캡처한 또는 브로드캐스트 게임 플레이와 동기화되는 컨텍스트 인식 환경을 제공하기 위해 앱에서 미디어를 재생하는 동안 사용자의 앱 및 다른 앱에서 추출할 수 있습니다.

### <a name="get-an-instance-of-appcapturemetadatawriter"></a>AppCaptureMetadataWriter 인스턴스 가져오기
앱 캡처 메타데이터를 관리하기 위한 기본 클래스는 **[AppCaptureMetadataWriter](https://docs.microsoft.com/uwp/api/windows.media.capture.appcapturemetadatawriter)** 입니다. 이 클래스의 인스턴스를 초기화하기 전에 **[ApiInformation.IsApiContractPresent](https://docs.microsoft.com/uwp/api/windows.foundation.metadata.apiinformation.isapicontractpresent)** 메서드를 사용하여 *Windows.Media.Capture.AppCaptureMetadataContract* 버전 1.0을 쿼리하여 현재 디바이스에서 API를 사용할 수 있는지 확인할 수 있습니다.

[!code-cpp[GetMetadataWriter](./code/AppRecordingExample/cpp/AppRecordingExample/App.cpp#SnippetGetMetadataWriter)]

### <a name="write-metadata-to-the-system-cache-for-your-app"></a>앱에 대한 시스템 캐시에 대한 메타데이터 쓰기
각 메타데이터 항목에는 메타데이터 항목을 식별하는 문자열 레이블, 문자열이 될 수 있는 관련 데이터 값, 정수, 또는 이중 값, 그리고 데이터 항목의 관련 우선 순위를 나타내는 **[AppCaptureMetadataPriority](https://docs.microsoft.com/uwp/api/windows.media.capture.appcapturemetadatapriority)** 열거가 있습니다. 메타데이터 항목은 단일 시점에서 발생하는 "이벤트" 또는 일정 기간 동안 값을 유지하는 "상태"로 간주할 수 있습니다. 메타데이터는 시스템에 의해 사용자의 앱에 할당되고 관리되는 메모리 캐시에 기록됩니다. 시스템은 메타데이터 메모리 캐시의 크기 제한을 적용하며 제한에 도달하면 각 메타데이터 항목이 작성된 우선 순위를 기준으로 데이터를 제거합니다. 이 문서의 다음 섹션에서는 앱의 메타데이터 메모리 할당을 관리하는 방법을 보여 줍니다.

일반적인 앱은 이후 데이터에 대한 몇 가지 컨텍스트를 제공하기 위해 캡처 세션 시작 시 몇 가지 메타데이터를 쓰도록 선택할 수 있습니다. 이 시나리오의 경우 일시적인 "이벤트" 데이터를 사용하는 것이 좋습니다. 이 예에서는 **[AddStringEvent](https://docs.microsoft.com/uwp/api/windows.media.capture.appcapturemetadatawriter.addstringevent)**, **[AddDoubleEvent](https://docs.microsoft.com/uwp/api/windows.media.capture.appcapturemetadatawriter.adddoubleevent)**, 및 **[AddInt32Event](https://docs.microsoft.com/uwp/api/windows.media.capture.appcapturemetadatawriter.addint32event)** 를 호출하여 각 데이터 유형에 대한 일시적인 값을 설정할 수 있습니다.

[!code-cpp[StartSession](./code/AppRecordingExample/cpp/AppRecordingExample/App.cpp#SnippetStartSession)]

시간이 지남에 따라 지속되는 "상태" 데이터 사용에 대한 일반적인 시나리오는 플레이어가 현재 위치한 게임 지도를 추적하는 것입니다. 이 예제는 **[StartStringState](https://docs.microsoft.com/uwp/api/windows.media.capture.appcapturemetadatawriter.startstringstate)** 를 호출하여 상태 값을 설정합니다. 

[!code-cpp[StartMap](./code/AppRecordingExample/cpp/AppRecordingExample/App.cpp#SnippetStartMap)]

특정 상태의 중단 레코드에 대한 **[StopState](https://docs.microsoft.com/uwp/api/windows.media.capture.appcapturemetadatawriter.stopstate)** 를 호출합니다.

[!code-cpp[EndMap](./code/AppRecordingExample/cpp/AppRecordingExample/App.cpp#SnippetEndMap)]

기존 상태 레이블로 새로운 값을 설정하여 상태를 덮어쓸 수 있습니다.

[!code-cpp[LevelUp](./code/AppRecordingExample/cpp/AppRecordingExample/App.cpp#SnippetLevelUp)]

**[StopAllStates](https://docs.microsoft.com/uwp/api/windows.media.capture.appcapturemetadatawriter.StopAllStates)** 를 호출하여 현재 열려 있는 모든 상태를 끝낼 수 있습니다.

[!code-cpp[RaceComplete](./code/AppRecordingExample/cpp/AppRecordingExample/App.cpp#SnippetRaceComplete)]

### <a name="manage-metadata-cache-storage-limit"></a>메타데이터 캐시 저장 공간 제한 관리
**AppCaptureMetadataWriter**로 쓴 메타데이터는 관련 미디어 스트림에 쓰여질 때까지 시스템에 의해 캐시됩니다. 시스템은 각 앱의 메타데이터 캐시에 대한 크기 제한을 정의합니다. 캐시 크기 제한에 도달하며 시스템은 캐시된 메타데이터 지우기를 시작합니다. 시스템은 **[AppCaptureMetadataPriority.Important](https://docs.microsoft.com/uwp/api/windows.media.capture.appcapturemetadatapriority)** 우선 순위의 메타데이터를 삭제하기 전에 **[AppCaptureMetadataPriority.Informational](https://docs.microsoft.com/uwp/api/windows.media.capture.appcapturemetadatapriority)** 우선 순위 값으로 작성된 메타데이터를 삭제합니다.

언제든지 **[RemainingStorageBytesAvailable](https://docs.microsoft.com/uwp/api/windows.media.capture.appcapturemetadatawriter.RemainingStorageBytesAvailable)** 을 호출하여 앱의 메타데이터 캐시의 바이트 수를 확인할 수 있습니다. 직접 앱 정의 임계값을 설정하고 이후에 캐시에 쓰는 메타데이터의 양을 줄이도록 선택할 수 있습니다. 다음 예는 이러한 패턴의 간단한 구현을 나타냅니다.

[!code-cpp[CheckMetadataStorage](./code/AppRecordingExample/cpp/AppRecordingExample/App.cpp#SnippetCheckMetadataStorage)]

[!code-cpp[ComboExecuted](./code/AppRecordingExample/cpp/AppRecordingExample/App.cpp#SnippetComboExecuted)]

### <a name="receive-notifications-when-the-system-purges-metadata"></a>시스템이 메타데이터를 제거하는 경우 알림 수신
시스템이 **[MetadataPurged](https://docs.microsoft.com/uwp/api/windows.media.capture.appcapturemetadatawriter.MetadataPurged)** 이벤트에 대한 처리기를 등록하여 앱에 대한 메타데이터 제거를 시작할 때 알림을 받도록 등록할 수 있습니다.

[!code-cpp[RegisterMetadataPurged](./code/AppRecordingExample/cpp/AppRecordingExample/App.cpp#SnippetRegisterMetadataPurged)]

**MetadataPurged** 이벤트에 대한 처리기에서 낮은 우선 순위 상태로 종료하여 메타데이터 캐시에 일부 공간을 확보하거나 캐시에 쓴 메타데이터의 양을 줄이기 위한 앱 정의 논리를 구현할 수 있습니다. 또는 아무것도 하지 않고 시스템에서 작성된 우선 순위에 따라 캐시를 제거하도록 할 수 있습니다.

[!code-cpp[OnMetadataPurged](./code/AppRecordingExample/cpp/AppRecordingExample/App.cpp#SnippetOnMetadataPurged)]

## <a name="related-topics"></a>관련 항목

* [게임](index.md)
 

 




