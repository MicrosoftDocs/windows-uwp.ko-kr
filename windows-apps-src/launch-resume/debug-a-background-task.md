---
author: TylerMSFT
title: 백그라운드 작업 디버그
description: Windows 이벤트 로그에서 백그라운드 작업 활성화 및 디버그 추적을 비롯한 백그라운드 작업을 디버그하는 방법을 알아봅니다.
ms.assetid: 24E5AC88-1FD3-46ED-9811-C7E102E01E9C
ms.author: twhitney
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp, 백그라운드 작업
ms.localizationpriority: medium
ms.openlocfilehash: f68c20a545e09d81912b8ef9a97a0ab0237ed0e0
ms.sourcegitcommit: 72710baeee8c898b5ab77ceb66d884eaa9db4cb8
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 09/11/2018
ms.locfileid: "3848767"
---
# <a name="debug-a-background-task"></a>백그라운드 작업 디버그


**중요 API**
-   [Windows.ApplicationModel.Background](https://msdn.microsoft.com/library/windows/apps/br224847)

Windows 이벤트 로그에서 백그라운드 작업 활성화 및 디버그 추적을 비롯한 백그라운드 작업을 디버그하는 방법을 알아봅니다.

## <a name="debugging-out-of-process-vs-in-process-background-tasks"></a>Out-of-process 및 In-process 백그라운드 작업 디버그
이 항목에서는 호스트 앱이 아닌 별도 프로세스에서 실행되는 백그라운드 작업을 주로 설명합니다. In-process 백그라운드 작업을 디버그하는 경우 별도 백그라운드 작업 프로젝트가 없으며 **OnBackgroundActivated()**(In-process 프로세스 백그라운드 코드가 실행되는 위치)에 중단점을 설정할 수 있습니다. 실행할 백그라운드 코드를 트리거하는 방법에 대한 자세한 내용은 아래의 [백그라운드 작업을 수동으로 트리거하여 백그라운드 작업 코드 디버그](#trigger-background-tasks-manually-to-debug-background-task-code)에서 2단계를 참조하세요.

## <a name="make-sure-the-background-task-project-is-set-up-correctly"></a>백그라운드 작업 프로젝트가 올바르게 설정되어 있는지 확인합니다.

이 항목에서는 기존 앱에 디버그할 백그라운드 작업이 이미 있다고 가정합니다. 다음은 Out-of-process에서 실행되는 특정 백그라운드 작업과 관련이 있으며 In-process 백그라운드 작업에는 적용되지 않습니다.

-   C# 및 C++에서 주 프로젝트가 백그라운드 작업 프로젝트를 참조하는지 확인합니다. 이 참조가 제대로 유지되지 않으면 백그라운드 작업이 앱 패키지에 포함되지 않습니다.
-   C# 및 C++에서 백그라운드 작업 프로젝트의 **출력 유형**이 "Windows 런타임 구성 요소"인지 확인합니다.
-   백그라운드 클래스 패키지 매니페스트의 진입점 특성에서 선언 해야 합니다.

## <a name="trigger-background-tasks-manually-to-debug-background-task-code"></a>백그라운드 작업을 수동으로 트리거하여 백그라운드 작업 코드 디버그

Microsoft Visual Studio를 통해 백그라운드 작업을 수동으로 트리거할 수 있습니다. 그런 다음 코드를 단계별로 실행하여 디버그할 수 있습니다.

1.  C#에서는 백그라운드 클래스의 Run 메서드에 중단점을 삽입(In-process 프로세스 백그라운드 작업의 경우 App.OnBackgroundActivated()에 중단점 삽입)하거나 [**System.Diagnostics**](https://msdn.microsoft.com/library/windows/apps/xaml/hh441592.aspx)를 사용하여 디버깅 출력을 씁니다.

    C++에서는 백그라운드 클래스의 Run 함수에 중단점을 삽입(In-process 백그라운드 작업의 경우 App.OnBackgroundActivated()에 중단점 삽입)하거나 [**OutputDebugString**](https://msdn.microsoft.com/library/windows/desktop/aa363362)를 사용하여 디버깅 출력을 씁니다.

2.  디버거에서 응용 프로그램을 실행한 다음 **수명 주기 이벤트** 도구 모음을 사용하여 백그라운드 작업을 트리거합니다. 이 드롭다운에는 Visual Studio에서 활성화할 수 있는 백그라운드 작업의 이름이 표시됩니다.

    이를 위해서는 백그라운드 작업이 이미 등록되어 있고 여전히 트리거를 대기하고 있어야 합니다. 예를 들어 백그라운드 작업이 원샷 TimeTrigger로 등록되었고 Visual Studio를 통해 작업을 실행해도 아무런 효과가 없습니다.

> [!Note]
> [**Application 트리거**](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.background.applicationtrigger.aspx), [**MediaProcessing 트리거**](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.background.mediaprocessingtrigger.aspx), [**ControlChannelTrigger**](https://msdn.microsoft.com/library/windows/apps/hh701032), [**PushNotificationTrigger**](https://msdn.microsoft.com/library/windows/apps/hh700543) 등의 트리거를 사용하는 백그라운드 작업과 [**SystemTrigger**](https://msdn.microsoft.com/library/windows/apps/br224838)를 [**SmsReceived**](https://msdn.microsoft.com/library/windows/apps/br224839) 트리거 유형과 함께 사용하는 백그라운드 작업은 이런 방식으로 활성화할 수 없습니다.  
> 코드에서 `trigger.RequestAsync()`를 사용하여 **Application 트리거** 및 **MediaProcessingTrigger**에 수동으로 신호를 보낼 수 있습니다.

![백그라운드 작업을 디버깅](images/debugging-activation.png)

3.  백그라운드 작업이 활성화되면 디버거가 작업에 연결하고 VS에 디버그 출력을 표시합니다.

## <a name="debug-background-task-activation"></a>백그라운드 작업 활성화 디버그

> [!NOTE]
> 이 섹션은 Out-of-process에서 실행되는 특정 백그라운드 작업과 관련이 있으며 In-process 백그라운드 작업에는 적용되지 않습니다.

백그라운드 작업 활성화는 다음 세 가지 항목에 따라 달라집니다.

-   백그라운드 작업 클래스의 이름 및 네임스페이스
-   패키지 매니페스트에 지정된 진입점 특성
-   백그라운드 작업을 등록할 때 앱에서 지정한 진입점

1.  Visual Studio를 사용하여 백그라운드 작업의 진입점을 기록합니다.

    -   C# 및 C++에서는 백그라운드 작업 프로젝트에 지정된 백그라운드 작업 클래스의 이름 및 네임스페이스를 기록합니다.

2.  매니페스트 디자이너를 사용하여 백그라운드 작업이 패키지 매니페스트에서 올바르게 선언되었는지 확인합니다.

    -   C# 및 C++에서는 진입점 특성이 백그라운드 작업 이름과 클래스 이름과 차례로 일치해야 합니다. 예를 들면 RuntimeComponent1.MyBackgroundTask와 같습니다.
    -   작업에 사용된 모든 트리거 유형도 지정해야 합니다.
    -   [**ControlChannelTrigger**](https://msdn.microsoft.com/library/windows/apps/hh701032) 또는 [**PushNotificationTrigger**](https://msdn.microsoft.com/library/windows/apps/hh700543)를 사용하지 않는 경우에는 실행 파일을 지정하면 안 됩니다.

3.  Windows에만 해당합니다. Windows에서 백그라운드 작업을 활성화하는 데 사용한 진입점을 확인하려면 디버그 추적을 사용하도록 설정하고 Windows 이벤트 로그를 사용합니다.

    이 절차를 수행할 때 이벤트 로그에 백그라운드 작업의 잘못된 진입점이나 트리거가 표시되면 앱에서 백그라운드 작업을 올바르게 등록할 수 없습니다. 이 작업에 대한 도움말은 [백그라운드 작업 등록](register-a-background-task.md)을 참조하세요.

    1.  시작 화면으로 이동하고 eventvwr.exe를 검색하여 이벤트 뷰어를 엽니다.
    2.  **응용 프로그램 및 서비스 로그로** 이동한 다음 - &gt; **Microsoft**  - &gt; **Windows**  - &gt; 이벤트 뷰어에서 **BackgroundTaskInfrastructure** 합니다.
    3.  작업 창에서 **보기** 를 선택 합니다. - &gt; **분석 및 디버그 로그 표시** 진단 로깅을 사용 하도록 설정 합니다.
    4.  **진단 로그**를 선택하고 **로그 사용**을 클릭합니다.
    5.  이제 앱을 사용하여 다시 백그라운드 작업을 등록하고 활성화합니다.
    6.  진단 로그에서 자세한 오류 정보를 봅니다. 여기에는 백그라운드 작업에 대해 등록된 진입점이 포함됩니다.

![이벤트 뷰어의 백그라운드 작업 디버그 정보](images/event-viewer.png)

## <a name="background-tasks-and-visual-studio-package-deployment"></a>백그라운드 작업 및 Visual Studio 패키지 배포

백그라운드 작업을 사용하는 앱을 Visual Studio를 사용하여 배포하고 매니페스트 디자이너에서 지정한 버전(주 및/또는 부)을 업데이트하는 경우 이후에 Visual Studio를 사용하여 앱을 다시 배포하면 앱의 백그라운드 작업이 정지할 수 있습니다. 이 문제는 다음과 같이 해결할 수 있습니다.

-   Visual Studio 대신 Windows PowerShell을 통해 패키지와 함께 생성된 스크립트를 실행하여 업데이트된 앱을 배포합니다.
-   이미 Visual Studio를 사용하여 앱을 배포했고 앱의 백그라운드 작업이 지금 정지된 경우 다시 부팅하거나 로그오프한 다음 로그인하여 앱의 백그라운드 작업이 다시 작동하게 합니다.
-   C# 프로젝트에서 "Always re-install my package(내 패키지 항상 다시 설치)" 디버깅 옵션을 선택하여 이 문제를 방지할 수 있습니다.
-   앱이 최종 배포되어 패키지 버전을 증가시킬 수 있을 때까지 기다립니다(디버그하는 중에 변경하지 않음).

## <a name="remarks"></a>설명

-   앱이 백그라운드 작업을 다시 등록하기 전에 기존 백그라운드 작업 등록을 확인하도록 하세요. 동일한 백그라운드 작업을 여러 번 등록하면 백그라운드 작업이 트리거될 때마다 두 번 이상 실행되어 예기치 않은 결과가 발생할 수 있습니다.
-   백그라운드 작업에서 잠금 화면에 액세스해야 하는 경우 백그라운드 작업을 디버그하기 전에 앱을 잠금 화면에 배치해야 합니다. 잠금 화면 지원 앱에 대한 매니페스트 옵션을 지정하는 방법은 [응용 프로그램 매니페스트에서 백그라운드 작업 선언](declare-background-tasks-in-the-application-manifest.md)을 참조하세요.
-   백그라운드 작업 등록 매개 변수는 등록 시 유효성이 검사됩니다. 등록 매개 변수가 하나라도 유효하지 않으면 오류가 반환됩니다. 백그라운드 작업 등록이 실패할 경우 앱이 시나리오를 적절하게 처리하도록 해야 합니다. 대신 앱이 작업 등록을 시도한 후 유효한 등록 개체를 사용하면 충돌할 수 있습니다.

VS를 사용 하 여 백그라운드 작업 디버그에 대 한 자세한 내용은 참조 [트리거하는 방법 일시 중단, 다시 시작 및 백그라운드 이벤트 UWP 앱에서](https://msdn.microsoft.com/library/windows/apps/xaml/hh974425.aspx).

## <a name="related-topics"></a>관련 항목

* [Out-of-process 백그라운드 작업 만들기 및 등록](create-and-register-a-background-task.md)
* [In-process 백그라운드 작업 만들기 및 등록](create-and-register-an-inproc-background-task.md)
* [백그라운드 작업 등록](register-a-background-task.md)
* [응용 프로그램 매니페스트에서 백그라운드 작업 선언](declare-background-tasks-in-the-application-manifest.md)
* [백그라운드 작업 지침](guidelines-for-background-tasks.md)
* [트리거하는 방법 일시 중단, 다시 시작 및 백그라운드 UWP 앱의 이벤트](https://msdn.microsoft.com/library/windows/apps/xaml/hh974425.aspx)
* [Visual Studio 코드 분석을 사용 하 여 UWP 앱의 코드 품질 분석](https://msdn.microsoft.com/library/windows/apps/xaml/hh441471.aspx)

 

 
