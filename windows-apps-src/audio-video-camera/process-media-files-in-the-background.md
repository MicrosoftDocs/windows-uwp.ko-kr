---
ms.assetid: B5E3A66D-0453-4D95-A3DB-8E650540A300
description: 이 문서에서는 MediaProcessingTrigger 및 백그라운드 작업을 사용 하 여 백그라운드에서 미디어 파일을 처리 하는 방법을 보여 줍니다.
title: 백그라운드에서 미디어 파일 처리
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 38261e8f6a03e17ba94a064b8e475503414e91d6
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89174637"
---
# <a name="process-media-files-in-the-background"></a>백그라운드에서 미디어 파일 처리



이 문서에서는 [**MediaProcessingTrigger**](/uwp/api/Windows.ApplicationModel.Background.MediaProcessingTrigger) 및 백그라운드 작업을 사용 하 여 백그라운드에서 미디어 파일을 처리 하는 방법을 보여 줍니다.

이 문서에서 설명 하는 예제 앱을 사용 하면 사용자는 트랜스 코딩을 위해 입력 미디어 파일을 선택 하 고 코드 변환 결과에 대 한 출력 파일을 지정할 수 있습니다. 그런 다음, 코드 변환 작업을 수행 하기 위해 백그라운드 작업이 시작 됩니다. [**MediaProcessingTrigger**](/uwp/api/Windows.ApplicationModel.Background.MediaProcessingTrigger) 는 디스크에 미디어 컴포지션을 렌더링 하 고 처리가 완료 된 후 처리 된 미디어 파일을 업로드 하는 등 트랜스 코딩 외에 다양 한 미디어 처리 시나리오를 지원 하기 위한 것입니다.

이 샘플에서 사용 되는 다양 한 유니버설 Windows 앱 기능에 대 한 자세한 내용은 다음을 참조 하세요.

-   [미디어 파일 코드 변환](transcode-media-files.md)
-   [다시 시작 및 백그라운드 작업 시작](../launch-resume/index.md)
-   [타일 배지 및 알림](../design/shell/tiles-and-notifications/index.md)

## <a name="create-a-media-processing-background-task"></a>미디어 처리 백그라운드 작업 만들기

Microsoft Visual Studio에서 기존 솔루션에 백그라운드 작업을 추가 하려면 사용자의 구성 요소에 대 한 이름을 입력 합니다.

1.  **파일** 메뉴에서 **추가** 를 선택한 다음 **새 프로젝트**...를 선택 합니다.
2.  프로젝트 형식 **Windows 런타임 구성 요소 (유니버설 Windows)** 를 선택 합니다.
3.  새 구성 요소 프로젝트의 이름을 입력 합니다. 이 예제에서는 프로젝트 이름 **MediaProcessingBackgroundTask**을 사용 합니다.
4.  확인을 클릭합니다.

**솔루션 탐색기**에서 기본적으로 생성 되는 "Class1.cs" 파일의 아이콘을 마우스 오른쪽 단추로 클릭 하 고 **이름 바꾸기**를 선택 합니다. 파일 이름을 "MediaProcessingTask.cs"로 바꿉니다. Visual Studio가이 클래스에 대 한 모든 참조의 이름을 바꿀지 묻는 메시지를 표시 하는 경우 **예**를 클릭 합니다.

이름이 바뀐 클래스 파일에서 다음 **using** 지시문을 추가 하 여이 네임 스페이스를 프로젝트에 포함 합니다.
                                  
[!code-cs[BackgroundUsing](./code/MediaProcessingTriggerWin10/cs/MediaProcessingBackgroundTask/MediaProcessingTask.cs#SnippetBackgroundUsing)]

클래스가 [**IBackgroundTask**](/uwp/api/Windows.ApplicationModel.Background.IBackgroundTask)에서 상속 하도록 클래스 선언을 업데이트 합니다.

[!code-cs[BackgroundClass](./code/MediaProcessingTriggerWin10/cs/MediaProcessingBackgroundTask/MediaProcessingTask.cs#SnippetBackgroundClass)]

클래스에 다음 멤버 변수를 추가 합니다.

-   백그라운드 작업의 진행 상태를 사용 하 여 포그라운드 앱을 업데이트 하는 데 사용할 [**IBackgroundTaskInstance**](/uwp/api/Windows.ApplicationModel.Background.IBackgroundTaskInstance) 입니다.
-   미디어 트랜스 코딩을 비동기식으로 수행 하는 동안 시스템에서 백그라운드 작업을 종료 하는 것을 유지 하는 [**BackgroundTaskDeferral**](/uwp/api/Windows.ApplicationModel.Background.BackgroundTaskDeferral) 입니다.
-   비동기 코드 변환 작업을 취소 하는 데 사용할 수 있는 **CancellationTokenSource** 개체입니다.
-   미디어 파일을 트랜스 코딩 하는 데 사용 되는 [**MediaTranscoder**](/uwp/api/Windows.Media.Transcoding.MediaTranscoder) 개체입니다.

[!code-cs[BackgroundMembers](./code/MediaProcessingTriggerWin10/cs/MediaProcessingBackgroundTask/MediaProcessingTask.cs#SnippetBackgroundMembers)]

작업을 시작할 때 시스템에서 백그라운드 작업의 [**Run**](/uwp/api/windows.applicationmodel.background.ibackgroundtask.run) 메서드를 호출 합니다. 메서드에 전달 된 [**IBackgroundTask**](/uwp/api/Windows.ApplicationModel.Background.IBackgroundTask) 개체를 해당 멤버 변수로 설정 합니다. 시스템이 백그라운드 작업을 종료 해야 하는 경우 발생 하는 [**취소**](/uwp/api/windows.applicationmodel.background.ibackgroundtaskinstance.canceled) 된 이벤트에 대 한 처리기를 등록 합니다. 그런 다음 [**Progress**](/uwp/api/windows.applicationmodel.background.ibackgroundtaskinstance.progress) 속성을 0으로 설정 합니다.

그런 다음 백그라운드 작업 개체의 [**GetDeferral**](/uwp/api/windows.applicationmodel.background.ibackgroundtaskinstance.getdeferral) 메서드를 호출 하 여 지연을 가져옵니다. 이렇게 하면 비동기 작업을 수행 하기 때문에 작업을 종료 하지 않도록 시스템에 지시할 수 있습니다.

다음으로 다음 섹션에 정의 된 **TranscodeFileAsync**도우미 메서드를 호출 합니다. 성공적으로 완료 되 면 코드 변환이 완료 되었음을 사용자에 게 알리는 알림 메시지를 시작 하기 위해 도우미 메서드가 호출 됩니다.

**Run** 메서드가 끝날 때 지연 개체에서 [**complete**](/uwp/api/windows.applicationmodel.background.backgroundtaskdeferral.complete) 를 호출 하 여 백그라운드 작업이 완료 되 고 종료 될 수 있음을 시스템에 알립니다.

[!code-cs[Run](./code/MediaProcessingTriggerWin10/cs/MediaProcessingBackgroundTask/MediaProcessingTask.cs#SnippetRun)]

**TranscodeFileAsync** helper 메서드에서, 코드 변환 작업의 입력 및 출력 파일에 대 한 파일 이름은 앱에 대 한 [**LocalSettings**](/uwp/api/windows.storage.applicationdata.localsettings) 에서 검색 됩니다. 이러한 값은 포그라운드 앱에 의해 설정 됩니다. 입력 및 출력 파일에 대 한 [**StorageFile**](/uwp/api/Windows.Storage.StorageFile) 개체를 만든 다음 트랜스 코딩에 사용할 인코딩 프로필을 만듭니다.

[**PrepareFileTranscodeAsync**](/uwp/api/windows.media.transcoding.mediatranscoder.preparefiletranscodeasync)를 호출 하 고 입력 파일, 출력 파일 및 인코딩 프로필을 전달 합니다. 이 호출에서 반환 된 [**PrepareTranscodeResult**](/uwp/api/Windows.Media.Transcoding.PrepareTranscodeResult) 개체를 사용 하면 코드 변환을 수행할 수 있는지 알 수 있습니다. [**Cantranscode 코딩**](/uwp/api/windows.media.transcoding.preparetranscoderesult.cantranscode) 속성이 True 이면 [**TranscodeAsync**](/uwp/api/windows.media.transcoding.preparetranscoderesult.transcodeasync) 를 호출 하 여 코드 변환 작업을 수행 합니다.

**Ask ask** 메서드를 사용 하 여 비동기 작업의 진행률을 추적 하거나 취소할 수 있습니다. 원하는 진행률 단위와 작업의 현재 진행 상태를 알리기 위해 호출 되는 메서드 이름을 지정 하 여 새 **진행률** 개체를 만듭니다. 작업을 취소할 수 있도록 하는 취소 토큰과 함께 **진행률** 개체를 **표시 합니다.**

[!code-cs[TranscodeFileAsync](./code/MediaProcessingTriggerWin10/cs/MediaProcessingBackgroundTask/MediaProcessingTask.cs#SnippetTranscodeFileAsync)]

이전 단계에서 progress 개체를 만드는 데 사용한 방법의 진행률에서 백그라운드 작업 인스턴스의 **진행률을 설정**합니다. 이렇게 하면 실행 중인 포그라운드 앱에 진행률이 전달 됩니다.

[!code-cs[Progress](./code/MediaProcessingTriggerWin10/cs/MediaProcessingBackgroundTask/MediaProcessingTask.cs#SnippetProgress)]

**Sendto notification** 도우미 메서드는 텍스트 콘텐츠만 있는 알림 메시지에 대 한 템플릿 XML 문서를 가져와 새 알림 메시지를 만듭니다. Toast XML의 text 요소가 설정 된 다음 XML 문서에서 새 [**To notification**](/uwp/api/Windows.UI.Notifications.ToastNotification) 개체가 만들어집니다. 마지막으로, 알림은 사용자에 게 표시 됩니다. [**표시**](/uwp/api/windows.ui.notifications.toastnotifier.show)를 호출 합니다.

[!code-cs[SendToastNotification](./code/MediaProcessingTriggerWin10/cs/MediaProcessingBackgroundTask/MediaProcessingTask.cs#SnippetSendToastNotification)]

시스템에서 백그라운드 작업을 취소할 때 호출 되는 [**취소**](/uwp/api/windows.applicationmodel.background.ibackgroundtaskinstance.canceled) 된 이벤트에 대 한 처리기에서 원격 분석을 위해 오류를 기록할 수 있습니다.

[!code-cs[OnCanceled](./code/MediaProcessingTriggerWin10/cs/MediaProcessingBackgroundTask/MediaProcessingTask.cs#SnippetOnCanceled)]

## <a name="register-and-launch-the-background-task"></a>백그라운드 작업 등록 및 시작

포그라운드 앱에서 백그라운드 작업을 시작 하려면 먼저 응용 프로그램에서 백그라운드 작업을 사용 하 고 있음을 시스템에서 알 수 있도록 포그라운드 앱의 appmanifest 파일을 업데이트 해야 합니다.

1.  **솔루션 탐색기**에서 appmanifest 파일 아이콘을 두 번 클릭 하 여 매니페스트 편집기를 엽니다.
2.  **선언** 탭을 선택 합니다.
3.  **사용 가능한 선언**에서 **백그라운드 작업** 을 선택 하 고 **추가**를 클릭 합니다.
4.  **지원 되는 선언** 에서 **백그라운드 작업** 항목이 선택 되어 있는지 확인 합니다. **속성**에서 **미디어 처리**확인란을 선택 합니다.
5.  **진입점** 텍스트 상자에서 백그라운드 테스트에 대 한 네임 스페이스 및 클래스 이름을 마침표로 구분 하 여 지정 합니다. 이 예의 경우 항목은 다음과 같습니다.
   ```csharp
   MediaProcessingBackgroundTask.MediaProcessingTask
   ```
다음으로 백그라운드 작업에 대 한 참조를 전경 앱에 추가 해야 합니다.
1.  **솔루션 탐색기**의 포그라운드 앱 프로젝트에서 **참조** 폴더를 마우스 오른쪽 단추로 클릭 하 고 **참조 추가**...를 선택 합니다.
2.  **프로젝트** 노드를 확장 하 고 **솔루션**을 선택 합니다.
3.  백그라운드 작업 프로젝트 옆의 확인란을 선택 하 고 **확인**을 클릭 합니다.

이 예제의 나머지 코드는 전경 앱에 추가 해야 합니다. 먼저 프로젝트에 다음 네임 스페이스를 추가 해야 합니다.

[!code-cs[ForegroundUsing](./code/MediaProcessingTriggerWin10/cs/MediaProcessingTriggerWin10/MainPage.xaml.cs#SnippetForegroundUsing)]

그런 다음 백그라운드 작업을 등록 하는 데 필요한 다음 멤버 변수를 추가 합니다.

[!code-cs[ForegroundMembers](./code/MediaProcessingTriggerWin10/cs/MediaProcessingTriggerWin10/MainPage.xaml.cs#SnippetForegroundMembers)]

**PickFilesToTranscode** 도우미 메서드는 [**fileopenpicker**](/uwp/api/Windows.Storage.Pickers.FileOpenPicker) 와 [**FileSavePicker**](/uwp/api/Windows.Storage.Pickers.FileSavePicker) 를 사용 하 여 트랜스 코딩을 위한 입력 및 출력 파일을 엽니다. 사용자가 앱에 액세스할 수 없는 위치에서 파일을 선택할 수 있습니다. 백그라운드 작업이 파일을 열 수 있도록 하려면 앱에 대 한 [**FutureAccessList**](/uwp/api/windows.storage.accesscache.storageapplicationpermissions.futureaccesslist) 에 추가 합니다.

마지막으로 앱에 대 한 [**LocalSettings**](/uwp/api/windows.storage.applicationdata.localsettings) 의 입력 및 출력 파일 이름에 대 한 항목을 설정 합니다. 백그라운드 작업은이 위치에서 파일 이름을 검색 합니다.

[!code-cs[PickFilesToTranscode](./code/MediaProcessingTriggerWin10/cs/MediaProcessingTriggerWin10/MainPage.xaml.cs#SnippetPickFilesToTranscode)]

백그라운드 작업을 등록 하려면 새 [**MediaProcessingTrigger**](/uwp/api/Windows.ApplicationModel.Background.MediaProcessingTrigger) 및 새 [**BackgroundTaskBuilder**](/uwp/api/Windows.ApplicationModel.Background.BackgroundTaskBuilder)를 만듭니다. 나중에 식별할 수 있도록 백그라운드 작업 빌더의 이름을 설정 합니다. [**Taskentrypoint**](/uwp/api/windows.applicationmodel.background.backgroundtaskbuilder.taskentrypoint) 를 매니페스트 파일에서 사용한 것과 같은 네임 스페이스 및 클래스 이름 문자열로 설정 합니다. [**트리거**](/uwp/api/windows.applicationmodel.background.backgroundtaskregistration.trigger) 속성을 **MediaProcessingTrigger** 인스턴스로 설정 합니다.

작업을 등록 하기 전에 [**alltasks**](/uwp/api/windows.applicationmodel.background.backgroundtaskregistration.alltasks) 컬렉션을 반복 하 고 [**BackgroundTaskBuilder.Name**](/uwp/api/windows.applicationmodel.background.backgroundtaskbuilder.name) 속성에 지정한 이름을 가진 작업에 대해 [**등록 취소**](/uwp/api/windows.applicationmodel.background.ibackgroundtaskregistration.unregister) 를 호출 하 여 이전에 등록 된 작업의 등록을 취소 해야 합니다.

[**Register**](/uwp/api/windows.applicationmodel.background.backgroundtaskbuilder.register)를 호출 하 여 백그라운드 작업을 등록 합니다. [**완료**](/uwp/api/windows.applicationmodel.background.backgroundtaskregistration.completed) 된 이벤트와 [**진행 중인**](/uwp/api/windows.applicationmodel.background.ibackgroundtaskregistration.progress) 이벤트에 대 한 처리기를 등록 합니다.

[!code-cs[RegisterBackgroundTask](./code/MediaProcessingTriggerWin10/cs/MediaProcessingTriggerWin10/MainPage.xaml.cs#SnippetRegisterBackgroundTask)]

일반적인 앱은 **OnNavigatedTo** 이벤트와 같이 앱이 처음 시작 될 때 백그라운드 작업을 등록 합니다.

**MediaProcessingTrigger** 개체의 [**requestasync**](/uwp/api/windows.applicationmodel.background.mediaprocessingtrigger.requestasync) 메서드를 호출 하 여 백그라운드 작업을 시작 합니다. 이 메서드에서 반환 된 [**MediaProcessingTriggerResult**](/uwp/api/Windows.ApplicationModel.Background.MediaProcessingTriggerResult) 개체를 사용 하면 백그라운드 작업이 성공적으로 시작 되었는지 여부를 알 수 있으며, 그렇지 않으면 백그라운드 작업이 시작 되지 않은 이유를 알 수 있습니다. 

[!code-cs[LaunchBackgroundTask](./code/MediaProcessingTriggerWin10/cs/MediaProcessingTriggerWin10/MainPage.xaml.cs#SnippetLaunchBackgroundTask)]

일반적인 앱은 UI 컨트롤의 **Click** 이벤트와 같이 사용자 상호 작용에 대 한 응답으로 백그라운드 작업을 시작 합니다.

**OnProgress** 이벤트 처리기는 백그라운드 태스크가 작업 진행률을 업데이트할 때 호출 됩니다. 이 기회를 사용 하 여 진행률 정보로 UI를 업데이트할 수 있습니다.

[!code-cs[OnProgress](./code/MediaProcessingTriggerWin10/cs/MediaProcessingTriggerWin10/MainPage.xaml.cs#SnippetOnProgress)]

**Oncompleted** 이벤트 처리기는 백그라운드 작업의 실행이 완료 될 때 호출 됩니다. 이는 UI를 업데이트 하 여 사용자에 게 상태 정보를 제공 하는 또 다른 기회입니다.

[!code-cs[OnCompleted](./code/MediaProcessingTriggerWin10/cs/MediaProcessingTriggerWin10/MainPage.xaml.cs#SnippetOnCompleted)]


 

 