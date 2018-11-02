---
author: drewbatgit
ms.assetid: B5E3A66D-0453-4D95-A3DB-8E650540A300
description: 이 문서에서는 MediaProcessingTrigger 및 백그라운드 작업을 사용하여 백그라운드에서 미디어 파일을 처리하는 방법을 보여 줍니다.
title: 백그라운드에서 미디어 파일 처리
ms.author: drewbat
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 866fedf35aa6f1f585825195b18cdd1fed4bad11
ms.sourcegitcommit: 70ab58b88d248de2332096b20dbd6a4643d137a4
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/02/2018
ms.locfileid: "5947382"
---
# <a name="process-media-files-in-the-background"></a>백그라운드에서 미디어 파일 처리



이 문서에서는 [**MediaProcessingTrigger**](https://msdn.microsoft.com/library/windows/apps/dn806005) 및 백그라운드 작업을 사용하여 백그라운드에서 미디어 파일을 처리하는 방법을 보여 줍니다.

이 문서에 설명된 예제 앱을 사용하여 코드 변환할 입력 미디어 파일을 선택하고 코드 변환 결과를 기록할 출력 파일을 지정할 수 있습니다. 그런 다음 코드 변환 작업을 수행하도록 백그라운드 작업을 시작합니다. [**MediaProcessingTrigger**](https://msdn.microsoft.com/library/windows/apps/dn806005)에서는 코드 변환 이외의 여러 다른 미디어 처리 시나리오(예: 디스크로 미디어 컴포지션 렌더링 및 처리를 완료한 후 처리된 미디어 파일 업로드)를 지원합니다.

이 샘플에서 활용하는 여러 다른 유니버설 Windows 앱 기능에 대한 자세한 내용은 다음을 참조하세요.

-   [미디어 파일 코드 변환](transcode-media-files.md)
-   [다시 시작 및 백그라운드 작업 시작](https://msdn.microsoft.com/library/windows/apps/mt227652)
-   [타일, 배지 및 알림](https://msdn.microsoft.com/library/windows/apps/mt185606)

## <a name="create-a-media-processing-background-task"></a>미디어 처리 백그라운드 작업 만들기

Microsoft Visual Studio에서 기존 솔루션에 백그라운드 작업을 추가하려면 구성 요소의 이름을 입력합니다.

1.  **파일** 메뉴에서 **추가**를 선택한 다음 **새 프로젝트...** 를 선택합니다.
2.  **Windows 런타임 구성 요소(유니버설 Windows)** 프로젝트 유형을 선택합니다.
3.  새 구성 요소 프로젝트의 이름을 입력합니다. 이 예제에서는 **MediaProcessingBackgroundTask** 프로젝트 이름을 사용합니다.
4.  확인을 클릭합니다.

**솔루션 탐색기**에서 기본적으로 생성되는 "Class1.cs" 파일의 아이콘을 마우스 오른쪽 단추로 클릭하고 **이름 바꾸기**를 선택합니다. 파일의 이름을 "MediaProcessingTask.cs"로 바꿉니다. Visual Studio에서 이 클래스의 모든 참조 이름을 바꾸려는지 물으면 **예**를 클릭합니다.

이름이 바뀐 클래스 파일에서 다음 **using** 지시문을 추가하여 프로젝트에 네임스페이스를 포함시킵니다.
                                  
[!code-cs[BackgroundUsing](./code/MediaProcessingTriggerWin10/cs/MediaProcessingBackgroundTask/MediaProcessingTask.cs#SnippetBackgroundUsing)]

[**IBackgroundTask**](https://msdn.microsoft.com/library/windows/apps/br224794)에서 클래스를 상속하도록 클래스 선언을 업데이트합니다.

[!code-cs[BackgroundClass](./code/MediaProcessingTriggerWin10/cs/MediaProcessingBackgroundTask/MediaProcessingTask.cs#SnippetBackgroundClass)]

클래스에 다음 멤버 변수를 추가합니다.

-   백그라운드 작업의 진행 상황에 따라 포그라운드 앱을 업데이트하는 데 사용하는 [**IBackgroundTaskInstance**](https://msdn.microsoft.com/library/windows/apps/br224797)입니다.
-   미디어 코드 변환을 비동기적으로 수행하는 동안 시스템에서 백그라운드 작업이 종료되지 않게 유지하는 [**BackgroundTaskDeferral**](https://msdn.microsoft.com/library/windows/apps/hh700499)입니다.
-   비동기 코드 변환 작업을 취소하는 데 사용할 수 있는 **CancellationTokenSource** 개체입니다.
-   미디어 파일의 코드를 변환하는 데 사용할 [**MediaTranscoder**](https://msdn.microsoft.com/library/windows/apps/br207080) 개체입니다.

[!code-cs[BackgroundMembers](./code/MediaProcessingTriggerWin10/cs/MediaProcessingBackgroundTask/MediaProcessingTask.cs#SnippetBackgroundMembers)]

작업이 시작되면 시스템에서 백그라운드 작업의 [**Run**](https://msdn.microsoft.com/library/windows/apps/br224811) 메서드를 호출합니다. 메서드에 전달된 [**IBackgroundTask**](https://msdn.microsoft.com/library/windows/apps/br224794) 개체를 해당 멤버 변수로 설정합니다. 시스템에서 백그라운드 작업을 종료해야 하는 경우 실행될 [**Canceled**](https://msdn.microsoft.com/library/windows/apps/br224798) 이벤트의 처리기를 등록합니다. 그런 다음 [**Progress**](https://msdn.microsoft.com/library/windows/apps/br224800) 속성을 0으로 설정합니다.

다음으로 백그라운드 작업 개체의 [**GetDeferral**](https://msdn.microsoft.com/library/windows/apps/hh700507) 메서드를 호출하여 지연을 확보합니다. 이를 통해 현재 비동기 조작을 수행 중이므로 작업을 종료하지 않아야 한다고 시스템에 지시합니다.

이제 다음 섹션에 정의되어 있는 도우미 메서드 **TranscodeFileAsync**를 호출합니다. 호출에 성공하면 사용자에게 코드 변환이 완료되었음을 알리기 위해 도우미 메서드를 호출하여 알림 메시지를 실행합니다.

**Run** 메서드 종료 시 지연 개체에서 [**Complete**](https://msdn.microsoft.com/library/windows/apps/hh700504)를 호출하여 백그라운드 작업이 완료되었으로 종료할 수 있음을 시스템에 알립니다.

[!code-cs[Run](./code/MediaProcessingTriggerWin10/cs/MediaProcessingBackgroundTask/MediaProcessingTask.cs#SnippetRun)]

**TranscodeFileAsync** 도우미 메서드에서 코드 변환 조작의 입력 및 출력 파일의 파일 이름을 앱의 [**LocalSettings**](https://msdn.microsoft.com/library/windows/apps/br241622)에서 검색합니다. 이러한 값은 포그라운드 앱에서 설정합니다. 입력 및 출력 파일의 [**StorageFile**](https://msdn.microsoft.com/library/windows/apps/br227171) 개체를 만든 다음 코드 변환에 사용할 인코딩 프로필을 만듭니다.

[**PrepareFileTranscodeAsync**](https://msdn.microsoft.com/library/windows/apps/hh700936)를 호출하여 입력 파일, 출력 파일 및 인코딩 프로필을 전달합니다. 이 호출을 통해 반환된 [**PrepareTranscodeResult**](https://msdn.microsoft.com/library/windows/apps/hh700941) 개체를 사용하여 코드 교환을 수행할 수 있는지 알립니다. [**CanTranscode**](https://msdn.microsoft.com/library/windows/apps/hh700942) 속성이 true이면 [**TranscodeAsync**](https://msdn.microsoft.com/library/windows/apps/hh700946)를 호출하여 코드 교환 조작을 수행합니다.

**AsTask** 메서드를 사용하여 비동기 조작의 진행 상태를 추적하거나 조작을 취소할 수 있습니다. 원하는 진행 단위를 지정하고 작업의 현재 진행 상태를 알리기 위해 호출할 메서드를 지정하여 새 **Progress** 개체를 만듭니다. 작업을 취소하는 데 사용할 수 있는 취소 토큰과 함께 **Progress** 개체를 **AsTask** 메서드에 전달합니다.

[!code-cs[TranscodeFileAsync](./code/MediaProcessingTriggerWin10/cs/MediaProcessingBackgroundTask/MediaProcessingTask.cs#SnippetTranscodeFileAsync)]

이전 단계에서, **Progress**에서 Progress 개체를 만드는 데 사용한 메서드에서 백그라운드 작업 인스턴스의 진행률을 설정합니다. 그러면 포그라운드 앱에 진행률을 전달합니다(실행 중인 경우).

[!code-cs[Progress](./code/MediaProcessingTriggerWin10/cs/MediaProcessingBackgroundTask/MediaProcessingTask.cs#SnippetProgress)]

**SendToastNotification** 도우미 메서드에서 텍스트 콘텐츠만 있는 알림 메시지의 템플릿 XML 문서를 가져와 새로운 알림 메시지를 만듭니다. 알림 XML의 텍스트 요소가 설정되고 나면 XML 문서에서 새 [**ToastNotification**](https://msdn.microsoft.com/library/windows/apps/br208641) 개체가 생성됩니다. 마지막으로 [**ToastNotifier.Show**](https://msdn.microsoft.com/library/windows/apps/br208659)를 호출하여 사용자에게 알림이 표시됩니다.

[!code-cs[SendToastNotification](./code/MediaProcessingTriggerWin10/cs/MediaProcessingBackgroundTask/MediaProcessingTask.cs#SnippetSendToastNotification)]

시스템이 백그라운드 작업을 취소할 때 호출되는 [**Canceled**](https://msdn.microsoft.com/library/windows/apps/Windows.ApplicationModel.Background.IBackgroundTaskInstance.Canceled) 이벤트에 대한 처리기에서 원격 분석을 위해 오류를 로깅할 수 있습니다.

[!code-cs[OnCanceled](./code/MediaProcessingTriggerWin10/cs/MediaProcessingBackgroundTask/MediaProcessingTask.cs#SnippetOnCanceled)]

## <a name="register-and-launch-the-background-task"></a>백그라운드 작업 등록 및 시작

앱에서 백그라운드 작업을 사용함을 시스템에 알릴 수 있도록 포그라운드 앱의 Package.appmanifest 파일을 업데이트해야 포그라운드 앱에서 백그라운드 작업을 시작할 수 있습니다.

1.  **솔루션 탐색기**에서 Package.appmanifest 파일 아이콘을 두 번 클릭하여 매니페스트 편집기를 엽니다.
2.  **선언** 탭을 선택합니다.
3.  **사용 가능한 선언**에서 **백그라운드 작업**을 선택하고 **추가**를 클릭합니다.
4.  **지원되는 선언**에서 **백그라운드 작업** 항목이 선택되었는지 확인합니다. **속성**에서 **미디어 처리**의 확인란을 선택합니다.
5.  **진입점** 입력란에서 백그라운드 테스트의 네임스페이스와 클래스 이름을 마침표로 구분하여 지정합니다. 이 예제에서는 다음과 같이 입력합니다.
   ```csharp
   MediaProcessingBackgroundTask.MediaProcessingTask
   ```
다음으로 백그라운드 작업에 대한 참조를 포그라운드 앱에 추가해야 합니다.
1.  **솔루션 탐색기**의 포그라운드 앱 프로젝트에서 **참조** 폴더를 마우스 오른쪽 단추로 클릭하고 **참조 추가...** 를 선택합니다.
2.  **프로젝트** 노드를 확장하고 **솔루션**을 선택합니다.
3.  백그라운드 작업 프로젝트 옆의 상자를 선택하고 **확인**을 클릭합니다.

이 예의 나머지 코드는 포그라운드 앱에 추가해야 합니다. 먼저 다음 네임스페이스를 프로젝트에 추가해야 합니다.

[!code-cs[ForegroundUsing](./code/MediaProcessingTriggerWin10/cs/MediaProcessingTriggerWin10/MainPage.xaml.cs#SnippetForegroundUsing)]

다음으로 백그라운드 작업을 등록하는 데 필요한 다음 멤버 변수를 추가합니다.

[!code-cs[ForegroundMembers](./code/MediaProcessingTriggerWin10/cs/MediaProcessingTriggerWin10/MainPage.xaml.cs#SnippetForegroundMembers)]

**PickFilesToTranscode** 도우미 메서드에서는 [**FileOpenPicker**](https://msdn.microsoft.com/library/windows/apps/br207847) 및 [**FileSavePicker**](https://msdn.microsoft.com/library/windows/apps/br207871)를 사용하여 코드 변환에 사용할 입력 및 출력 파일을 엽니다. 앱에서 액세스할 수 없는 위치에 있는 파일을 선택했을 가능성이 있습니다. 백그라운드 작업에서 파일을 열 수 있도록 앱의 [**FutureAccessList**](https://msdn.microsoft.com/library/windows/apps/br207457)에 파일을 추가합니다.

마지막으로 앱의 [**LocalSettings**](https://msdn.microsoft.com/library/windows/apps/br241622)에 입력 및 출력 파일 이름의 항목을 설정합니다. 백그라운드 작업이 이 위치에서 파일 이름을 검색합니다.

[!code-cs[PickFilesToTranscode](./code/MediaProcessingTriggerWin10/cs/MediaProcessingTriggerWin10/MainPage.xaml.cs#SnippetPickFilesToTranscode)]

백그라운드 작업을 등록하기 위해 새 [**MediaProcessingTrigger**](https://msdn.microsoft.com/library/windows/apps/dn806005) 및 새 [**BackgroundTaskBuilder**](https://msdn.microsoft.com/library/windows/apps/br224768)를 만듭니다. 나중에 식별할 수 있도록 백그라운드 작업 작성기의 이름을 설정합니다. 매니페스트 파일에서 사용하는 네임스페이스 및 클래스 이름 문자열과 동일하게 [**TaskEntryPoint**](https://msdn.microsoft.com/library/windows/apps/br224774)를 설정합니다. [**Trigger**](https://msdn.microsoft.com/library/windows/apps/dn641725) 속성을 **MediaProcessingTrigger** 인스턴스로 설정합니다.

작업을 등록하기 전에 [**AllTasks**](https://msdn.microsoft.com/library/windows/apps/br224787) 컬렉션에서 루프 실행하고 [**BackgroundTaskBuilder.Name**](https://msdn.microsoft.com/library/windows/apps/br224771) 속성에 지정한 이름의 작업에서 [**Unregister**](https://msdn.microsoft.com/library/windows/apps/br229870)를 호출하여 이전에 등록한 작업의 등록을 취소하세요.

[**Register**](https://msdn.microsoft.com/library/windows/apps/br224772)를 호출하여 백그라운드 작업을 등록합니다. [**Completed**](https://msdn.microsoft.com/library/windows/apps/br224788) 및 [**Progress**](https://msdn.microsoft.com/library/windows/apps/br224808) 이벤트에 대한 처리기를 등록합니다.

[!code-cs[RegisterBackgroundTask](./code/MediaProcessingTriggerWin10/cs/MediaProcessingTriggerWin10/MainPage.xaml.cs#SnippetRegisterBackgroundTask)]

앱이 처음 시작, **OnNavigatedTo** 이벤트 등 일반적인 앱은 백그라운드 작업을 등록 합니다.

**MediaProcessingTrigger** 개체의 [**RequestAsync**](https://msdn.microsoft.com/library/windows/apps/dn765071) 메서드를 호출하여 백그라운드 작업을 시작합니다. 이 메서드에서 반환한 [**MediaProcessingTriggerResult**](https://msdn.microsoft.com/library/windows/apps/dn806007) 개체를 통해 백그라운드 작업이 성공적으로 시작되었는지 알 수 있으며, 제대로 시작되지 않은 경우에는 백그라운드 작업을 시작하지 못한 이유를 알 수 있습니다. 

[!code-cs[LaunchBackgroundTask](./code/MediaProcessingTriggerWin10/cs/MediaProcessingTriggerWin10/MainPage.xaml.cs#SnippetLaunchBackgroundTask)]

일반적인 앱 UI 컨트롤의 **클릭** 이벤트에서와 같은 사용자 상호 작용에 대 한 응답으로 백그라운드 작업을 시작 됩니다.

백그라운드 작업에서 조작의 진행률을 업데이트할 때 **OnProgress** 이벤트 처리기가 호출됩니다. 이때 진행률 정보로 UI를 업데이트할 수 있습니다.

[!code-cs[OnProgress](./code/MediaProcessingTriggerWin10/cs/MediaProcessingTriggerWin10/MainPage.xaml.cs#SnippetOnProgress)]

백그라운드 작업에서 실행을 완료하면 **OnCompleted** 이벤트 처리기가 호출됩니다. 이때에도 사용자에게 상태 정보를 제공하도록 UI를 업데이트할 수 있습니다.

[!code-cs[OnCompleted](./code/MediaProcessingTriggerWin10/cs/MediaProcessingTriggerWin10/MainPage.xaml.cs#SnippetOnCompleted)]


 

 




