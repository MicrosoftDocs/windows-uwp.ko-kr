---
title: UWP 앱 업데이트 시 백그라운드 작업 실행
description: 유니버설 Windows 플랫폼 (UWP) 스토어 앱이 업데이트 될 때 실행 되는 백그라운드 작업을 만드는 방법에 대해 알아봅니다.
ms.date: 04/21/2017
ms.topic: article
keywords: windows 10, uwp, 업데이트, 백그라운드 작업, updatetask 창, 백그라운드 작업
ms.localizationpriority: medium
ms.openlocfilehash: ff4dd5487357728b6a79f4c4d31a437075fcd006
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89164807"
---
# <a name="run-a-background-task-when-your-uwp-app-is-updated"></a>UWP 앱 업데이트 시 백그라운드 작업 실행

유니버설 Windows 플랫폼 (UWP) 스토어 앱이 업데이트 된 후 실행 되는 백그라운드 작업을 작성 하는 방법에 대해 알아봅니다.

업데이트 작업 백그라운드 작업은 사용자가 장치에 설치 된 앱에 대 한 업데이트를 설치 하 고 나면 운영 체제에서 호출 됩니다. 이렇게 하면 사용자가 업데이트 된 앱을 시작 하기 전에 앱이 새 푸시 알림 채널을 초기화 하거나 데이터베이스 스키마를 업데이트 하는 등의 초기화 작업을 수행할 수 있습니다.

업데이트 작업은 [ServicingComplete](/uwp/api/Windows.ApplicationModel.Background.SystemTriggerType) 트리거를 사용 하 여 백그라운드 작업을 시작 하는 것과는 다릅니다 .이 경우 **ServicingComplete** 트리거에서 활성화 될 백그라운드 작업을 등록 하기 위해 앱을 업데이트 하기 전에 한 번 이상 실행 해야 합니다.  업데이트 태스크가 등록 되지 않았기 때문에 실행 되지 않았지만 업그레이드 된 앱은 여전히 업데이트 작업이 트리거됩니다.

## <a name="step-1-create-the-background-task-class"></a>1 단계: 백그라운드 작업 클래스 만들기

다른 유형의 백그라운드 작업과 마찬가지로 Windows 런타임 구성 요소로 업데이트 작업 백그라운드 작업을 구현 합니다. 이 구성 요소를 만들려면 [out-of-process 백그라운드 작업 만들기 및 등록](./create-and-register-a-background-task.md)의 **백그라운드 작업 클래스 만들기** 섹션에 있는 단계를 따르세요. 단계는 다음과 같습니다.

- Windows 런타임 구성 요소 프로젝트를 솔루션에 추가 합니다.
- 응용 프로그램에서 구성 요소로 참조를 만듭니다.
- [**IBackgroundTask**](/uwp/api/Windows.ApplicationModel.Background.IBackgroundTask)를 구현 하는 구성 요소에서 봉인 된 public 클래스를 만듭니다.
- Update 작업이 실행 될 때 호출 되는 필수 진입점 인 [**run**](/uwp/api/windows.applicationmodel.background.ibackgroundtask.run) 메서드를 구현 합니다. 백그라운드 작업에서 비동기 호출을 수행 하려는 경우 [process out 백그라운드 작업을 만들고 등록](./create-and-register-a-background-task.md) 하면 **Run** 메서드에서 지연을 사용 하는 방법을 설명 합니다.

업데이트 작업을 사용 하려면이 백그라운드 작업을 등록 하지 않아도 됩니다. ( **out-of-process 백그라운드 작업을 만들고 등록** 하는 작업 항목에서 "실행할 백그라운드 작업 등록" 섹션). 작업을 등록 하기 위해 앱에 코드를 추가할 필요가 없고 앱이 백그라운드 작업을 등록 하기 위해 업데이트 하기 전에 한 번만 실행 될 필요가 없기 때문에이는 업데이트 작업을 사용 하는 주된 이유입니다.

다음 샘플 코드에서는 c #의 업데이트 작업 백그라운드 작업 클래스에 대 한 기본 시작 지점을 보여 줍니다. 백그라운드 작업 클래스 자체 및 백그라운드 작업 프로젝트의 다른 모든 클래스는 **public** 이 고 **sealed**여야 합니다. 백그라운드 작업 클래스는 **IBackgroundTask** 에서 파생 되 고 다음과 같은 서명을 사용 하 여 공용 **Run ()** 메서드를 사용 해야 합니다.

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

## <a name="step-2-declare-your-background-task-in-the-package-manifest"></a>2 단계: 패키지 매니페스트에서 백그라운드 작업 선언

Visual Studio 솔루션 탐색기에서 **appxmanifest.xml** 를 마우스 오른쪽 단추로 클릭 하 고 **코드 보기** 를 클릭 하 여 패키지 매니페스트를 확인 합니다. 다음 XML을 추가 `<Extensions>` 하 여 업데이트 작업을 선언 합니다.

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

위의 XML에서 `EntryPoint` 특성이 업데이트 작업 클래스의 namespace. 클래스 이름으로 설정 되어 있는지 확인 합니다. 이름은 대/소문자를 구분합니다.

## <a name="step-3-debugtest-your-update-task"></a>3 단계: 업데이트 작업 디버그/테스트

업데이트할 항목이 있도록 컴퓨터에 앱을 배포 했는지 확인 합니다.

백그라운드 작업의 Run () 메서드에 중단점을 설정 합니다.

![중단점 설정](images/run-func-breakpoint.png)

그런 다음 솔루션 탐색기에서 백그라운드 작업 프로젝트가 아닌 앱 프로젝트를 마우스 오른쪽 단추로 클릭 한 다음 **속성**을 클릭 합니다. 응용 프로그램 속성 창에서 왼쪽의 **디버그** 를 클릭 한 다음 시작 **하지 않음 (시작 시 코드 디버그**)을 선택 합니다.

![디버그 설정 설정](images/do-not-launch-but-debug.png)

그런 다음, UpdateTask 버전 번호를 늘립니다. 솔루션 탐색기에서 응용 프로그램의 **appxmanifest.xml** 파일을 두 번 클릭 하 여 패키지 디자이너를 연 다음 **빌드** 번호를 업데이트 합니다.

![버전 업데이트](images/bump-version.png)

이제 Visual Studio 2019에서 F5 키를 누르면 앱이 업데이트 되 고 시스템은 백그라운드에서 Updatdatcomponent를 활성화 합니다. 디버거는 백그라운드 프로세스에 자동으로 연결 됩니다. 중단점에 도달 하면 업데이트 코드 논리를 단계별로 실행 할 수 있습니다.

백그라운드 작업이 완료 되 면 동일한 디버그 세션 내의 Windows 시작 메뉴에서 포그라운드 앱을 시작할 수 있습니다. 그러면 디버거가 다시 자동으로 연결 되 고, 이번에는 포그라운드 프로세스에 자동으로 연결 되며, 앱의 논리를 단계별로 진행할 수 있습니다.

> [!NOTE]
> Visual Studio 2015 사용자: 위의 단계는 Visual Studio 2017 또는 Visual Studio 2019에 적용 됩니다. Visual Studio 2015을 사용 하는 경우 Visual Studio가 연결 되지 않는다는 점을 제외 하 고 동일한 기법을 사용 하 여 UpdateTask 트리거하고 테스트할 수 있습니다. VS 2015의 다른 절차는 UpdateTask을 진입점으로 설정 하는 [Applicationtrigger](./trigger-background-task-from-app.md) 를 설정 하 고 포그라운드 앱에서 직접 실행을 트리거하는 것입니다.

## <a name="see-also"></a>참고 항목

[Out-of-process 백그라운드 작업 만들기 및 등록](./create-and-register-a-background-task.md)