---
title: 백그라운드 작업 디버그
description: Windows 이벤트 로그에서 백그라운드 작업 활성화 및 디버그 추적을 비롯한 백그라운드 작업을 디버그하는 방법을 알아봅니다.
ms.assetid: 24E5AC88-1FD3-46ED-9811-C7E102E01E9C
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp, 백그라운드 작업
ms.localizationpriority: medium
ms.openlocfilehash: e7d008a6956c3acd22dcb99e6bf4e1cda1442545
ms.sourcegitcommit: 39fb8c0dff1b98ededca2f12e8ea7977c2eddbce
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/06/2020
ms.locfileid: "91750169"
---
# <a name="debug-a-background-task"></a>백그라운드 작업 디버그


**중요 API**
-   [Windows.ApplicationModel.Background](/uwp/api/Windows.ApplicationModel.Background)

Windows 이벤트 로그에서 백그라운드 작업 활성화 및 디버그 추적을 비롯한 백그라운드 작업을 디버그하는 방법을 알아봅니다.

## <a name="debugging-out-of-process-vs-in-process-background-tasks"></a>Out-of-process 및 in-process 백그라운드 작업 디버깅
이 항목에서는 호스트 앱이 아닌 별도의 프로세스에서 실행 되는 백그라운드 작업을 주로 다룹니다. In-process 백그라운드 작업을 디버깅 하는 경우 별도의 백그라운드 작업 프로젝트가 없고, **OnBackgroundActivated ()** 에 중단점을 설정 하 고 (in process 백그라운드 코드가 실행 되는 경우), 백그라운드 [작업을 수동으로 트리거하](#trigger-background-tasks-manually-to-debug-background-task-code)는 방법에 대 한 지침을 참조 하 여 백그라운드 작업 코드를 디버깅할 수 있습니다.

## <a name="make-sure-the-background-task-project-is-set-up-correctly"></a>백그라운드 작업 프로젝트가 올바르게 설정 되어 있는지 확인 합니다.

이 항목에서는 디버그 하는 백그라운드 작업을 사용 하는 기존 앱이 이미 있다고 가정 합니다. 다음은 out-of-process에서 실행 되며 in-process 백그라운드 작업에는 적용 되지 않는 백그라운드 작업과 관련 된 것입니다.

-   C # 및 c + +에서 주 프로젝트가 백그라운드 작업 프로젝트를 참조 하는지 확인 합니다. 이 참조가 없으면 백그라운드 작업은 앱 패키지에 포함 되지 않습니다.
-   C # 및 c + +에서 백그라운드 작업 프로젝트의 **출력 형식이** "Windows 런타임 구성 요소" 인지 확인 합니다.
-   패키지 매니페스트의 진입점 특성에는 background 클래스를 선언 해야 합니다.

## <a name="trigger-background-tasks-manually-to-debug-background-task-code"></a>백그라운드 작업 코드를 디버그 하는 백그라운드 작업을 수동으로 트리거

백그라운드 작업은 Microsoft Visual Studio를 통해 수동으로 트리거할 수 있습니다. 그런 다음 코드를 단계별로 실행 하 고 디버그할 수 있습니다.

1.  C #에서 백그라운드 클래스의 Run 메서드에 중단점을 설정 합니다 (in-process 백그라운드 작업에서 OnBackgroundActivated ()에 중단점을 배치 하는 경우),/또는 [**시스템 진단을**](/dotnet/api/system.diagnostics)사용 하 여 디버깅 출력을 씁니다.

    C + +에서 백그라운드 클래스의 Run 함수에 중단점을 배치 합니다 (in-process 백그라운드 작업에서 OnBackgroundActivated ()에 중단점을 배치). 또는 [**OutputDebugString**](/windows/desktop/api/debugapi/nf-debugapi-outputdebugstringw)을 사용 하 여 디버깅 출력을 씁니다.

2.  디버거에서 응용 프로그램을 실행 한 다음 **수명 주기 이벤트** 도구 모음을 사용 하 여 백그라운드 작업을 트리거합니다. 이 드롭다운에는 Visual Studio에서 활성화할 수 있는 백그라운드 작업 이름이 표시 됩니다.

    > [!NOTE]
    > 수명 주기 이벤트 도구 모음 옵션은 Visual Studio에 기본적으로 표시 되지 않습니다. 이러한 옵션을 표시 하려면 Visual Studio의 현재 도구 모음을 마우스 오른쪽 단추로 클릭 하 고 **디버그 위치** 옵션을 사용 하도록 설정 했는지 확인 합니다.

    이 작업을 수행 하려면 백그라운드 작업을 이미 등록 해야 하며 트리거를 계속 대기 해야 합니다. 예를 들어, 한 번의 샷 TimeTrigger에 백그라운드 작업을 등록 하 고 해당 트리거가 이미 발생 한 경우 Visual Studio를 통해 작업을 시작 해도 아무런 효과가 없습니다.

    > [!Note]
    > 다음 트리거를 사용 하는 백그라운드 작업은 이러한 방식으로 활성화할 수 없습니다. [**응용 프로그램 트리거**](/uwp/api/windows.applicationmodel.background.applicationtrigger), [**mediaprocessing 트리거**](/uwp/api/windows.applicationmodel.background.mediaprocessingtrigger), [**ControlChannelTrigger**](/uwp/api/Windows.Networking.Sockets.ControlChannelTrigger), [**Pushnotificationtrigger**](/uwp/api/Windows.ApplicationModel.Background.PushNotificationTrigger)및 백그라운드 작업은 [**smsreceived**](/uwp/api/Windows.ApplicationModel.Background.SystemTriggerType) 트리거 유형과 함께 [**systemtrigger**](/uwp/api/Windows.ApplicationModel.Background.SystemTrigger) 를 사용 합니다.  
    > **응용 프로그램 트리거와** **MediaProcessingTrigger** 는를 사용 하 여 코드에서 수동으로 신호를 받을 수 있습니다 `trigger.RequestAsync()` .

    ![백그라운드 작업 디버깅](images/debugging-activation.png)

3.  백그라운드 작업이 활성화 되 면 디버거가 해당 작업에 연결 되 고 VS에서 디버그 출력을 표시 합니다.

## <a name="debug-background-task-activation"></a>백그라운드 작업 활성화 디버그

> [!NOTE]
> 이 섹션은 실행 중인 백그라운드 작업에만 적용 되며 in-process 백그라운드 작업에는 적용 되지 않습니다.

백그라운드 작업 활성화는 다음 세 가지 항목에 따라 달라 집니다.

-   백그라운드 작업 클래스의 이름 및 네임 스페이스
-   패키지 매니페스트에 지정 된 진입점 특성
-   백그라운드 작업을 등록할 때 앱에서 지정 하는 진입점입니다.

1.  Visual Studio를 사용 하 여 백그라운드 작업의 진입점을 확인 합니다.

    -   C # 및 c + +에서 백그라운드 작업 프로젝트에 지정 된 백그라운드 작업 클래스의 이름 및 네임 스페이스를 확인 합니다.

2.  매니페스트 디자이너를 사용 하 여 패키지 매니페스트에서 백그라운드 작업이 올바르게 선언 되었는지 확인 합니다.

    -   C # 및 c + +에서 진입점 특성은 백그라운드 작업 네임 스페이스와 클래스 이름이 일치 해야 합니다. 예: RuntimeComponent1. MyBackgroundTask.
    -   또한 작업과 함께 사용 되는 모든 트리거 형식을 지정 해야 합니다.
    -   [**ControlChannelTrigger**](/uwp/api/Windows.Networking.Sockets.ControlChannelTrigger) 또는 [**pushnotificationtrigger**](/uwp/api/Windows.ApplicationModel.Background.PushNotificationTrigger)를 사용 하지 않는 경우 실행 파일을 지정 하지 않아야 합니다.

3.  Windows만 해당됩니다. Windows에서 백그라운드 작업을 활성화하는 데 사용한 진입점을 확인하려면 디버그 추적을 사용하도록 설정하고 Windows 이벤트 로그를 사용합니다.

    이 절차를 수행 하 고 이벤트 로그에 백그라운드 작업에 대 한 잘못 된 진입점 또는 트리거가 표시 되는 경우 앱이 백그라운드 작업을 올바르게 등록 하지 않습니다. 이 작업에 대 한 도움말 [은 백그라운드 작업 등록을](register-a-background-task.md)참조 하세요.

    1.  시작 화면으로 이동 하 여 eventvwr.exe을 검색 하 여 이벤트 뷰어를 엽니다.
    2.  이벤트 뷰어에서 **응용 프로그램 및 서비스 로그**  - &gt; **Microsoft**  - &gt; **Windows**  - &gt; **BackgroundTaskInfrastructure** 로 이동 합니다.
    3.  작업 창에서 **보기**  - &gt; **분석 및 디버그 로그 표시** 를 선택 하 여 진단 로깅을 사용 하도록 설정 합니다.
    4.  **진단 로그** 를 선택 하 고 **로그 사용**을 클릭 합니다.
    5.  이제 앱을 사용 하 여 백그라운드 작업을 다시 등록 하 고 활성화 해 봅니다.
    6.  자세한 오류 정보는 진단 로그를 참조 하십시오. 여기에는 백그라운드 작업에 대해 등록 된 진입점이 포함 됩니다.

![백그라운드 작업 디버그 정보에 대 한 이벤트 뷰어](images/event-viewer.png)

## <a name="background-tasks-and-visual-studio-package-deployment"></a>백그라운드 작업 및 Visual Studio 패키지 배포

Visual Studio를 사용 하 여 백그라운드 작업을 사용 하는 응용 프로그램을 배포 하 고 매니페스트 디자이너에 지정 된 버전 (주 및/또는 부)을 업데이트 한 후에는 Visual Studio를 사용 하 여 앱을 다시 배포 하면 앱의 백그라운드 작업이 정지 될 수 있습니다. 이는 다음과 같이 해결할 수 있습니다.

-   Windows PowerShell을 사용 하 여 패키지와 함께 생성 된 스크립트를 실행 하 여 Visual Studio 대신 업데이트 된 앱을 배포 합니다.
-   이미 Visual Studio를 사용 하 여 앱을 배포 하 고 백그라운드 작업이 중단 된 경우 다시 부팅 하거나 로그 오프/로그인 하 여 앱의 백그라운드 작업을 다시 시작 합니다.
-   C # 프로젝트에서이 문제를 방지 하기 위해 "항상 내 패키지 다시 설치" 디버깅 옵션을 선택할 수 있습니다.
-   앱이 최종 배포에서 패키지 버전을 증가 시킬 준비가 될 때까지 기다립니다 (디버깅 하는 동안에는 변경 하지 않음).

## <a name="remarks"></a>설명

-   백그라운드 작업을 다시 등록 하기 전에 앱이 기존 백그라운드 작업 등록을 확인 하는지 확인 합니다. 동일한 백그라운드 작업을 여러 번 등록 하면 트리거될 때마다 백그라운드 작업을 두 번 이상 실행 하 여 예기치 않은 결과가 발생할 수 있습니다.
-   백그라운드 작업에 잠금 화면 액세스가 필요한 경우 백그라운드 작업을 디버깅 하기 전에 잠금 화면에 앱을 배치 해야 합니다. 화면 지원 앱 잠금에 대 한 매니페스트 옵션 지정에 대 한 정보는 [응용 프로그램 매니페스트에서 백그라운드 작업 선언](declare-background-tasks-in-the-application-manifest.md)을 참조 하세요.
-   등록 시 백그라운드 작업 등록 매개 변수의 유효성이 검사 됩니다. 등록 매개 변수가 잘못 된 경우 오류가 반환 됩니다. 앱이 백그라운드 작업 등록에 실패 하는 시나리오를 정상적으로 처리 하는지 확인 합니다. 대신 앱이 작업 등록을 시도한 후 유효한 등록 개체가 있는 경우 충돌이 발생할 수 있습니다.

VS를 사용 하 여 백그라운드 작업을 디버깅 하는 방법에 대 한 자세한 내용은 [UWP 앱에서 일시 중단, 다시 시작 및 백그라운드 이벤트를 트리거하는 방법](/visualstudio/debugger/how-to-trigger-suspend-resume-and-background-events-for-windows-store-apps-in-visual-studio?view=vs-2015)을 참조 하세요.

## <a name="related-topics"></a>관련 항목

* [Out-of-process 백그라운드 작업 만들기 및 등록](create-and-register-a-background-task.md)
* [In-process 백그라운드 작업 만들기 및 등록](create-and-register-an-inproc-background-task.md)
* [백그라운드 작업 등록](register-a-background-task.md)
* [애플리케이션 매니페스트에서 백그라운드 작업 선언](declare-background-tasks-in-the-application-manifest.md)
* [백그라운드 작업 지침](guidelines-for-background-tasks.md)
* [UWP 앱에서 일시 중단, 다시 시작 및 백그라운드 이벤트를 트리거하는 방법](/visualstudio/debugger/how-to-trigger-suspend-resume-and-background-events-for-windows-store-apps-in-visual-studio?view=vs-2015)
* [Visual Studio code 분석을 사용 하 여 UWP 앱의 코드 품질 분석](/visualstudio/test/analyze-the-code-quality-of-store-apps-using-visual-studio-static-code-analysis?view=vs-2015)

 

 