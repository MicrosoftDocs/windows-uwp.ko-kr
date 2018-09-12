---
author: TylerMSFT
title: UWP 앱 업데이트 시 백그라운드 작업을 실행합니다.
description: UWP(유니버설 Windows 플랫폼) 스토어 앱 업데이트 시 실행되는 백그라운드 작업을 만드는 방법을 알아봅니다.
ms.author: twhitney
ms.date: 04/21/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: 창 10, uwp, 업데이트, 백그라운드 작업, updatetask, 백그라운드 작업
ms.localizationpriority: medium
ms.openlocfilehash: fcba2cb736f86cebc6d2664e2ec3b557d47c86d7
ms.sourcegitcommit: 2a63ee6770413bc35ace09b14f56b60007be7433
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 09/12/2018
ms.locfileid: "3931285"
---
# <a name="run-a-background-task-when-your-uwp-app-is-updated"></a>UWP 앱 업데이트 시 백그라운드 작업을 실행합니다.

유니버설 Windows 플랫폼 (UWP) 저장소 응용 프로그램을 업데이트 한 후 실행 하는 백그라운드 작업을 작성 하는 방법에 알아봅니다.

업데이트 작업이 백그라운드 작업이 사용자를 장치에 설치 된 응용 프로그램 업데이트를 설치한 후 운영 체제에 의해 호출 됩니다. 이렇게 하면 사용자가 응용 프로그램 업데이트를 시작 하기 전에 데이터베이스 스키마 및 기타 등등을 업데이트 새로운 푸시 알림 채널을 초기화 하는 같은 초기화 작업을 수행 하는 응용 프로그램.

업데이트 작업에서 시작 하는 경우 응용 프로그램 실행 해야 하므로 적어도 한 번은 **에 의해 활성화 되는 백그라운드 작업을 등록 하기 위해 업데이트 되기 전에 [ServicingComplete](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Background.SystemTriggerType) 트리거를 사용 하 여 백그라운드 작업에서 다른 ServicingComplete** 트리거.  업데이트 작업에 등록 되지 않은 및 응용 프로그램을 실행, 하지만 업그레이드 된 트리거된 업데이트 작업은 계속 보유 하는 때문.

## <a name="step-1-create-the-background-task-class"></a>1 단계: 백그라운드 작업 클래스 만들기

로 다른 유형의 백그라운드 작업으로 구현 하는 업데이트 작업 백그라운드 작업 Windows 런타임 구성 요소를. 이 구성 요소를 만들려면 [만들기](https://docs.microsoft.com/windows/uwp/launch-resume/create-and-register-a-background-task)및 레지스터는 독립 프로세스 백그라운드 작업 **백그라운드 작업 클래스 만들기** 섹션의 단계를 따릅니다. 단계는 다음과 같습니다.

- Windows 런타임 구성 요소 프로젝트를 솔루션에 추가 합니다.
- 응용 프로그램에서 구성 요소에 대 한 참조 만들기
- 구성 요소의 public, sealed 클래스를 만들고 [**IBackgroundTask**](https://msdn.microsoft.com/library/windows/apps/br224794)를 구현 합니다.
- 업데이트 작업이 실행 될 때 호출 되는 필요한 진입점이 [**Run**](https://msdn.microsoft.com/library/windows/apps/br224811) 메서드를 구현 하는. 백그라운드 작업에서 비동기 호출을 수행 하려는 경우 [만들기 및 등록 작업 중이 아닌 백그라운드는](https://docs.microsoft.com/windows/uwp/launch-resume/create-and-register-a-background-task) 지연 **실행** 메서드에서 사용 하는 방법을 설명 합니다.

업데이트 작업을 사용 하 여 백그라운드 작업 ( **만들기 및 레지스터는 독립 프로세스 백그라운드 작업** 항목에서 "실행 하는 백그라운드 작업 등록" 구역)를 등록 하지 않아도 됩니다. 응용 프로그램 업데이트는 백그라운드 작업을 등록 하 게 하기 전에 한 번 이상 실행할 수 없는 작업을 등록 하는 응용 프로그램에 코드를 추가할 필요가 없습니다 때문에 프로그램 업데이트 작업을 사용 하는 주된 이유입니다.

다음 샘플 코드에서 C# 업데이트 작업 백그라운드 작업 클래스에 대 한 기본적인 사용법을 보여 줍니다. 백그라운드 작업 클래스 자체-와 백그라운드 작업 프로젝트의 다른 모든 클래스- **공개** 및 **봉인**수 있어야 합니다. 백그라운드 작업 클래스 **IBackgroundTask** 에서 파생 되를 아래에 표시 된 서명을 사용 하 여 공용 **Run()** 메서드를가지고 해야 합니다.

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

## <a name="step-2-declare-your-background-task-in-the-package-manifest"></a>2 단계: 백그라운드 작업 패키지 매니페스트에 선언

Visual Studio의 솔루션 탐색기에서 **Package.appxmanifest** 를 마우스 오른쪽 단추로 클릭 하 고 패키지 매니페스트를 보려면 **코드 보기** 클릭 합니다. 다음 코드를 추가 `<Extensions>` 업데이트 작업을 선언 하는 XML:

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

위의 XML에서의 `EntryPoint` 특성이 namespace.class 업데이트 작업 클래스 이름으로 설정 되어 있습니다. 이름은 대 소문자를 구분 합니다.

## <a name="step-3-debugtest-your-update-task"></a>3 단계: 디버그/업데이트 작업 테스트

뭔가를 업데이트 하 여 시스템 응용 프로그램 배포를 확인 합니다.

백그라운드 작업의 Run() 메서드에 중단점을 설정 합니다.

![중단점 설정](images/run-func-breakpoint.png)

다음으로 솔루션 탐색기에서 (백그라운드 작업 프로젝트가 아닌) 응용 프로그램 프로젝트를 마우스 오른쪽 단추로 클릭 한 다음 **속성**을 클릭 합니다. 응용 프로그램 속성 창 왼쪽에서 **디버그** 를 클릭 후 **시작 되지 않습니다 하지만 시작 될 때 코드를 디버그**를 선택 합니다.

![디버그 설정](images/do-not-launch-but-debug.png)

다음으로 UpdateTask 트리거되는 되도록 패키지의 버전 번호를 늘립니다. 솔루션 탐색기에서 패키지 디자이너를 열려면 응용 프로그램의 **Package.appxmanifest** 파일을 두 번 클릭 하 고 **빌드** 번호를 업데이트 합니다.

![버전 업데이트](images/bump-version.png)

이제 2017 Visual Studio에서 f5 키를 눌러 응용 프로그램 업데이트 되 고 시스템 UpdateTask 구성 요소는 백그라운드에서 실행 됩니다. 디버거는 자동으로 백그라운드 프로세스에 연결 됩니다. 중단점을 얻을 업데이트 논리의 코드를 단계별로 실행할 수 있습니다.

백그라운드 작업이 완료 되 면 같은 디버그 세션 내에서 Windows 시작 메뉴에서 포그라운드 응용 프로그램을 시작할 수 있습니다. 디버거는 자동으로 다시,이 이번 전경 프로세스에 꽂고 응용 프로그램 논리를 단계별로 실행할 수 있습니다.

> [!NOTE]
> Visual Studio 2015 사용자: 위의 단계는 2017 Visual Studio에 적용 합니다. 2015 Visual Studio를 사용 하는 경우 트리거를 테스트를 제외 하 고 Visual Studio UpdateTask에 연결 되지 것입니다 동일한 기술을 사용할 수 있습니다. VS 2015에는 다른 절차는 UpdateTask의 진입점으로 설정 하는 [ApplicationTrigger](https://docs.microsoft.com/windows/uwp/launch-resume/trigger-background-task-from-app) 설치 전경 응용 프로그램에서 직접 실행을 시작 하는 것입니다.

## <a name="see-also"></a>참고 항목

[Out-of-process 백그라운드 작업 만들기 및 등록](https://docs.microsoft.com/windows/uwp/launch-resume/create-and-register-a-background-task)
