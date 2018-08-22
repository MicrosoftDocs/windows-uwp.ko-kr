---
author: TylerMSFT
title: UWP 앱 업데이트 시 백그라운드 작업을 실행합니다.
description: UWP(유니버설 Windows 플랫폼) 스토어 앱 업데이트 시 실행되는 백그라운드 작업을 만드는 방법을 알아봅니다.
ms.author: twhitney
ms.date: 04/21/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp, 업데이트, 백그라운드 작업, updatetask, 백그라운드 작업
ms.localizationpriority: medium
ms.openlocfilehash: fcba2cb736f86cebc6d2664e2ec3b557d47c86d7
ms.sourcegitcommit: f2f4820dd2026f1b47a2b1bf2bc89d7220a79c1a
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/22/2018
ms.locfileid: "2787065"
---
# <a name="run-a-background-task-when-your-uwp-app-is-updated"></a>UWP 앱 업데이트 시 백그라운드 작업을 실행합니다.

범용 Windows 플랫폼 (UWP) 저장소 앱을 업데이트 한 후 실행 되는 백그라운드 작업을 작성 하는 방법에 알아봅니다.

작업 업데이트 백그라운드 작업은 사용자가 장치에 설치 된 응용 프로그램에 대 한 업데이트를 설치 후 운영 체제에 의해 호출 됩니다. 이 사용자가 업데이트 된 응용 프로그램을 시작 하기 전에 데이터베이스 스키마와 등 업데이트 하는 새 푸시 알림 채널을 초기화 하 고 등 초기화 작업을 수행 하 여 앱 수 있습니다.

이 경우 앱 실행 해야 하므로 최소한 한번는 **하 여 활성화 되는 백그라운드 작업을 등록 하기 위해 업데이트 되기 전에 [ServicingComplete](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Background.SystemTriggerType) 트리거를 사용 하 여 백그라운드 작업을 시작에서 다른 업데이트 작업 ServicingComplete** 트리거 합니다.  업데이트 작업에 등록 되지 않습니다 하 고 있으므로 실행 하지가, 하지만 업그레이드 하는 응용 프로그램 에서도 계속 해 서 업데이트 작업을 트리거한 합니다.

## <a name="step-1-create-the-background-task-class"></a>1 단계: 백그라운드 작업 클래스 만들기

으로 다른 종류의 백그라운드 작업으로 구현 하는 업데이트 작업 백그라운드 작업 Windows 런타임 구성 요소. 이 구성 요소를 만들려면 [만들기 및 등록 작업 중이 아닌 백그라운드 작업의](https://docs.microsoft.com/windows/uwp/launch-resume/create-and-register-a-background-task) **백그라운드 작업 클래스 만들기** 섹션의 단계를 수행 합니다. 단계는 다음과 같습니다.

- 솔루션에 Windows 런타임 구성 요소 프로젝트를 추가 합니다.
- 앱에서 구성 요소에 대 한 참조를 만듭니다.
- 구성 요소에서 공용, 봉인 클래스 만들기 [**IBackgroundTask**](https://msdn.microsoft.com/library/windows/apps/br224794)를 구현 합니다.
- 업데이트 작업이 실행 될 때 호출 되는 필수 진입점은 [**Run**](https://msdn.microsoft.com/library/windows/apps/br224811) 메서드를 구현 합니다. 백그라운드 작업에 대 한 비동기 호출을 확인 하려는 경우 [만들기 및 등록 작업 중이 아닌 백그라운드 작업을](https://docs.microsoft.com/windows/uwp/launch-resume/create-and-register-a-background-task) 지연 **Run** 메서드를 사용 하 여에서 사용 하는 방법을 설명 합니다.

업데이트 작업을 사용 하 여이 백그라운드 작업 ( **만들기 및 등록 작업 중이 아닌 백그라운드 작업** 항목의 "를 실행 하려면 백그라운드 작업 등록" 섹션)을 등록할 필요가 없습니다. 코드 작업을 등록 하 여 앱에 추가할 필요가 없습니다 하 고 응용 프로그램 업데이트는 백그라운드 작업을 등록 하 게 하기 전에 한번 실행 하는 최소 필요가 없으므로 때문에 업데이트 작업을 사용 하는 주요 이유입니다.

다음 코드 예제에서는 C#에서 업데이트 작업 백그라운드 작업 클래스에 대 한 기본 시작점을 보여줍니다. 백그라운드 작업 클래스 자체-와 백그라운드 작업 프로젝트의 다른 모든 클래스- **공용** 및 **봉인**수 있어야 합니다. 백그라운드 작업 클래스는 **IBackgroundTask** 에서 파생 하 고 아래에 표시 된 서명을 사용 하 여 공용 **Run()** 메서드를 포함 해야 합니다.

```cs
using Windows.ApplicationModel.Background;

namespace BackgroundTasks
{
    public sealed class UpdateTask : IBackgroundTask
    {
        public void Run(IBackgroundTaskInstance taskInstance)
        {
            // your app migration/update code here
        }
    }
}
```

## <a name="step-2-declare-your-background-task-in-the-package-manifest"></a>2 단계: 선언 패키지 매니페스트에 백그라운드 작업

Visual Studio 솔루션 탐색기에서 **Package.appxmanifest** 를 마우스 오른쪽 단추로 클릭 하 고 패키지 매니페스트를 보려면 **코드 보기** 클릭 합니다. 다음 코드를 추가 `<Extensions>` 업데이트 작업을 선언 하는 XML:

```XML
<Package ...>
    ...
  <Applications>  
    <Application ...>  
        ...
      <Extensions>  
        <Extension Category="windows.updateTask"  EntryPoint="BackgroundTasks.UpdateTask">  
        </Extension>  
      </Extensions>

    </Application>  
  </Applications>  
</Package>
```

위의 XML에서 확인 하는 `EntryPoint` 특성이 업데이트 작업 클래스의 namespace.class 이름으로 설정 됩니다. 이름은 대/소문자 구분 합니다.

## <a name="step-3-debugtest-your-update-task"></a>3 단계: 디버그/테스트 업데이트 작업

업데이트할 뭔가 되도록 사용자 컴퓨터에 응용 프로그램 배포를 확인 합니다.

백그라운드 작업의 Run() 메서드에서 중단점을 설정 합니다.

![집합 중단점](images/run-func-breakpoint.png)

다음으로, 솔루션 탐색기에서 앱의 프로젝트 (백그라운드 작업 프로젝트가 아닌)를 마우스 오른쪽 단추로 클릭 하 고 **속성**을 클릭 합니다. 응용 프로그램 속성 창에서 왼쪽에서 **디버그** 를 클릭 한 다음 **실행 하지 않음을 하지만 시작 시 내 코드 디버그**를 선택 합니다.

![디버그 설정](images/do-not-launch-but-debug.png)

다음으로 UpdateTask 트리거되는 되도록 패키지의 버전 번호를 늘립니다. 솔루션 탐색기에서 패키지 디자이너를 열려면 앱의 **Package.appxmanifest** 파일을 두번클릭 하 고 **빌드** 번호를 업데이트 합니다.

![버전 업데이트](images/bump-version.png)

이제, Visual Studio 2017에서 f5 키를 누를 때 앱 업데이트 되 고 시스템 백그라운드에서 UpdateTask 구성 요소를 활성화 합니다. 디버거를 백그라운드 프로세스를 자동으로 연결 됩니다. 사용자는 가져올 중단점에서 코드 하 고 업데이트 코드 논리를 단계별로 실행할 수 있습니다.

백그라운드 작업이 완료 되 면 동일한 디버그 세션 내에서 Windows 시작 메뉴에서 전경 응용 프로그램을 시작할 수 있습니다. 디버거는 자동으로 다시 연결,이 이번에 전경 프로세스에는 및 앱의 논리를 단계별로 실행할 수 있습니다.

> [!NOTE]
> Visual Studio 2015 사용자: 위의 단계는 Visual Studio 2017에 적용 합니다. Visual Studio 2015를 사용 하는 경우에 트리거 및 테스트 하 여 Visual Studio를 제외 하 고 UpdateTask, 연결 되지 것입니다 동일한 기법을 사용할 수 있습니다. VS 2015에는 다른 절차는 해당 진입점으로는 UpdateTask를 설정 하는 [ApplicationTrigger](https://docs.microsoft.com/windows/uwp/launch-resume/trigger-background-task-from-app) 를 설치 하 고 전경 응용 프로그램에서 직접 실행을 시작 하는 것입니다.

## <a name="see-also"></a>기타 참조

[Out-of-process 백그라운드 작업 만들기 및 등록](https://docs.microsoft.com/windows/uwp/launch-resume/create-and-register-a-background-task)
