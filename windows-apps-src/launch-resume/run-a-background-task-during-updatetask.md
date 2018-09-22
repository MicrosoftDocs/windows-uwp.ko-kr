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
ms.sourcegitcommit: a160b91a554f8352de963d9fa37f7df89f8a0e23
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 09/21/2018
ms.locfileid: "4124248"
---
# <a name="run-a-background-task-when-your-uwp-app-is-updated"></a>UWP 앱 업데이트 시 백그라운드 작업을 실행합니다.

유니버설 Windows 플랫폼 (UWP) 스토어 앱 업데이트 후 실행 되는 백그라운드 작업을 작성 하는 방법을 알아봅니다.

업데이트 작업 백그라운드 작업은 사용자 장치에 설치 하는 앱에 대 한 업데이트를 설치한 후 운영 체제에 의해 호출 됩니다. 이 앱을 업데이트 데이터베이스 스키마 및 등 사용자가 업데이트 된 앱을 실행 하기 전에 새 푸시 알림 채널, 초기화 등의 초기화 작업을 수행할 수 있습니다.

이 경우 앱 실행 해야 하므로 한 번 이상 합니다 **활성화 되는 백그라운드 작업을 등록 하기 위해 업데이트 되기 전에 [ServicingComplete](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Background.SystemTriggerType) 트리거를 사용 하 여 백그라운드 작업을 시작에서 업데이트 작업의 차이점 ServicingComplete** 트리거 합니다.  업데이트 작업 등록 되지 않은 및 앱을 실행, 하지만 업그레이드 하는 해당 업데이트 작업이 트리거될 계속 수 있으므로 합니다.

## <a name="step-1-create-the-background-task-class"></a>1 단계: 백그라운드 작업 클래스 만들기

으로 다른 유형의 백그라운드 작업으로 구현 하 업데이트 작업 백그라운드 작업 Windows 런타임 구성 요소입니다. 이 구성 요소를 만들려면 [out of process 백그라운드 작업 만들기 및 등록의](https://docs.microsoft.com/windows/uwp/launch-resume/create-and-register-a-background-task) **백그라운드 작업 클래스 만들기** 섹션의 단계를 따릅니다. 단계는 다음과 같습니다.

- Windows 런타임 구성 요소 프로젝트를 솔루션에 추가 합니다.
- 앱에서 구성 요소에 대 한 참조를 만듭니다.
- 구성 요소에 공용, 봉인 클래스를 만들고 [**IBackgroundTask**](https://msdn.microsoft.com/library/windows/apps/br224794)를 구현 합니다.
- [**Run**](https://msdn.microsoft.com/library/windows/apps/br224811) 메서드를 구현 하는 경우 업데이트 작업이 실행 될 때 호출 되는 필수 진입점 백그라운드 작업에서 비동기 호출 하려는 [out of process 백그라운드 작업 만들기 및 등록](https://docs.microsoft.com/windows/uwp/launch-resume/create-and-register-a-background-task) **실행** 메서드에서 deferral을 사용 하는 방법을 설명 합니다.

업데이트 작업을 사용 하 여이 백그라운드 작업 ( **out of process 백그라운드 작업 만들기 및 등록** 항목의 "백그라운드 작업이 실행 되도록 등록" 섹션)를 등록할 필요가 없습니다. 작업을 등록 하 여 앱 코드를 추가할 필요가 없습니다 및 앱을 업데이트 하 게 백그라운드 작업을 등록 하기 전에 한 번 이상 실행 필요가 때문에 업데이트 작업을 사용 하는 주된 이유입니다.

다음 샘플 코드 C#에서 업데이트 작업 백그라운드 작업 클래스는 기본 시작 지점을 보여 줍니다. 백그라운드 작업 클래스 자체와 백그라운드 작업 프로젝트의 다른 모든 클래스가- **public** 및 **sealed**되도록 해야 합니다. 백그라운드 작업 클래스 **IBackgroundTask** 에서 파생 하 고 아래 표시 된 서명이 포함 된 공개 **run ()** 메서드가 있어야 해야:

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

위의 XML에서의 `EntryPoint` 특성이 업데이트 작업 클래스의 namespace.class 이름으로 설정 합니다. 이름은 대/소문자 구분 합니다.

## <a name="step-3-debugtest-your-update-task"></a>3 단계: 디버그/업데이트 작업 테스트

업데이트를 위해 되도록 시스템에 앱 배포 확인 합니다.

백그라운드 작업의 run () 메서드에 중단점을 설정 합니다.

![설정 중단점](images/run-func-breakpoint.png)

다음으로 솔루션 탐색기에서 앱의 프로젝트 (백그라운드 작업 프로젝트가 아닌)를 마우스 오른쪽 단추로 클릭 하 고 **속성**을 차례로 클릭 합니다. 응용 프로그램 속성 창의 왼쪽에 **디버그** 를 클릭 한 다음 **실행 하지 않지만 시작 되 면 내 코드 디버그**선택:

![디버그 설정 지정](images/do-not-launch-but-debug.png)

다음으로 UpdateTask 트리거될 되도록 패키지의 버전 번호를 높입니다. 솔루션 탐색기에서 패키지 디자이너를 열려면 앱의 **Package.appxmanifest** 파일을 두 번 클릭 하 고 **빌드** 번호를 업데이트 합니다.

![버전 업데이트](images/bump-version.png)

이제 Visual Studio 2017에서 F5 키를 누를 때 앱 업데이트 되 고 시스템에서 백그라운드에서 UpdateTask 구성 요소를 정품 인증 됩니다. 디버거는 백그라운드 프로세스를 자동으로 연결 됩니다. 중단점이 적중 가져오기지 것입니다 및 업데이트 코드 논리를 단계별로 실행할 수 있습니다.

백그라운드 작업이 완료 되 면 동일한 디버그 세션 내에서 Windows 시작 메뉴에서 포그라운드 앱을 시작할 수 있습니다. 디버거는 자동으로 다시 연결 포그라운드 프로세스이 이번 하 고, 앱의 논리를 단계별로 실행할 수 있습니다.

> [!NOTE]
> Visual Studio 2015 사용자: Visual Studio 2017에 위의 단계를 적용 합니다. Visual Studio 2015를 사용 하는 경우 트리거를 테스트 하 여 Visual Studio를 제외 하 고 UpdateTask를 연결 되지 것입니다 동일한 기술을 사용할 수 있습니다. VS 2015에는 다른 절차는 UpdateTask의 진입점으로 설정 하는 [ApplicationTrigger](https://docs.microsoft.com/windows/uwp/launch-resume/trigger-background-task-from-app) 를 설치 하 고 포그라운드 앱에서 직접 실행을 시작 하는 것입니다.

## <a name="see-also"></a>참고 항목

[Out-of-process 백그라운드 작업 만들기 및 등록](https://docs.microsoft.com/windows/uwp/launch-resume/create-and-register-a-background-task)
