---
title: UWP 앱 업데이트 시 백그라운드 작업을 실행합니다.
description: UWP(유니버설 Windows 플랫폼) 스토어 앱 업데이트 시 실행되는 백그라운드 작업을 만드는 방법을 알아봅니다.
ms.date: 04/21/2017
ms.topic: article
keywords: windows 10, uwp, 업데이트, 백그라운드 작업, updatetask, 백그라운드 작업
ms.localizationpriority: medium
ms.openlocfilehash: fa5420b14d3d73f370031eed917e0e7c367c41c7
ms.sourcegitcommit: 51d884c3646ba3595c016e95bbfedb7ecd668a88
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/11/2019
ms.locfileid: "67820952"
---
# <a name="run-a-background-task-when-your-uwp-app-is-updated"></a>UWP 앱 업데이트 시 백그라운드 작업을 실행합니다.

UWP 스토어 앱 업데이트 후 실행되는 백그라운드 작업을 작성하는 방법을 알아봅니다.

사용자가 장치에 설치된 앱에 대한 업데이트를 설치하면 운영 체제에서 업데이트 작업 배경 작업을 호출합니다. 이를 통해 사용자가 업데이트된 앱을 실행하기 전에 앱이 새 푸시 알림 채널 초기화, 데이터베이스 스키마 업데이트 등의 초기화 작업을 수행할 수 있습니다.

업데이트 작업은 [ServicingComplete](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Background.SystemTriggerType) 트리거를 사용하는 백그라운드 작업을 시작하는 것과는 다릅니다. 이 경우 **ServicingComplete** 트리거에 의해 활성화되는 백그라운드 작업을 등록하기 위해 업데이트하기 전에 앱이 한 번 이상 실행되어야 합니다.  업데이트 작업은 등록되지 않으므로 실행되지 않았지만 업그레이드된 앱의 업데이트 작업은 계속 트리거됩니다.

## <a name="step-1-create-the-background-task-class"></a>1단계: 백그라운드 작업 클래스 만들기

다른 유형의 백그라운드 작업과 마찬가지로 Windows 런타임 구성 요소로 업데이트 작업 백그라운드 작업을 구현합니다. 이 구성 요소를 만들려면 [Out-of-process 백그라운드 작업 만들기 및 등록](https://docs.microsoft.com/windows/uwp/launch-resume/create-and-register-a-background-task)의 **백그라운드 작업 클래스 만들기** 섹션의 단계를 따르세요. 이 단계는 다음과 같습니다.

- 솔루션에 Windows 런타임 구성 요소 개체 추가
- 엡에서 구성 요소로의 참조 만들기
- [  **IBackgroundTask**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Background.IBackgroundTask)를 구현하는 구성 요소에 public sealed 클래스 만들기
- 업데이트 작업 실행 시 호출되는 필수 진입점인 [**Run**](https://docs.microsoft.com/uwp/api/windows.applicationmodel.background.ibackgroundtask.run) 메서드 구현 백그라운드 작업에서 비동기 호출을 만들려는 경우 [Out-of-process 백그라운드 작업 만들기 및 등록](https://docs.microsoft.com/windows/uwp/launch-resume/create-and-register-a-background-task)에 **Run** 메서드에 지연을 사용하는 방법이 나와 있습니다.

업데이트 작업을 사용하기 위해 이 백그라운드 작업을 등록할 필요가 없습니다(**Out-of-process 백그라운드 작업 만들기 및 등록** 항목의 "실행할 백그라운드 작업 등록" 섹션 참조). 바로 이것이 업데이트 작업을 사용하는 주된 이유입니다. 백그라운드 작업을 등록하기 위해 업데이트 전에 앱을 한 번 이상 실행할 필요가 없으며 작업 등록을 위해 앱에 코드를 추가할 필요가 없기 때문입니다.

다음 샘플 코드는 C#에서의 업데이트 작업 백그라운드 작업 클래스의 기본 시작 지점을 보여 줍니다. 백그라운드 작업 클래스 자체와 백그라운드 작업 프로젝트의 다른 모든 클래스가 **public** 및 **sealed**여야 합니다. 백그라운드 작업 클래스는 **IBackgroundTask**에서 파생되어야 하며 아래 표시된 서명과 함께 공용 **Run()** 메서드가 있어야 합니다.

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

## <a name="step-2-declare-your-background-task-in-the-package-manifest"></a>2단계: 패키지 매니페스트에 백그라운드 작업 선언

Visual Studio 솔루션 탐색기에서 **Package.appxmanifest** 파일을 마우스 오른쪽 단추로 클릭하고 **코드 보기**를 클릭하여 패키지 매니페스트를 봅니다. 다음 `<Extensions>` XML을 추가하여 업데이트 작업을 선언합니다.

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

위의 XML에서 `EntryPoint` 특성이 업데이트 작업 클래스의 namespace.class 이름으로 설정되어 있는지 확인합니다. 이름은 대/소문자 구분입니다.

## <a name="step-3-debugtest-your-update-task"></a>3단계: 업데이트 작업을 디버그 테스트 합니다.

업데이트할 사항이 있도록 컴퓨터에 앱을 배치했는지 확인합니다.

백그라운드 작업의 Run() 메서드에 중단점을 설정합니다.

![중단점 설정](images/run-func-breakpoint.png)

그런 다음 솔루션 탐색기에서 백그라운드 작업 프로젝트가 아닌 앱의 프로젝트를 마우스 오른쪽 단추로 클릭하고 **속성**을 클릭합니다. 응용 프로그램 속성 창의 왼쪽에서 **디버그**를 클릭하고 **실행하지 않지만 시작되면 내 코드 디버그**를 선택합니다.

![디버그 설정 지정](images/do-not-launch-but-debug.png)

그런 다음 패키지의 버전 번호를 늘려서 UpdateTask가 트리거되는지 확인합니다. 솔루션 탐색기에서 앱의 **Package.appxmanifest** 파일을 두 번 클릭하여 패키지 디자이너를 열고 **빌드** 번호를 업데이트합니다.

![버전 업데이트](images/bump-version.png)

이제 Visual Studio 2019에서 f5 키를 누르면 앱은 업데이트 되 고 시스템을 백그라운드에서 UpdateTask 구성 요소를 활성화 합니다. 디버거가 자동으로 백그라운드 프로세스에 연결됩니다. 중단점이 실행되고 업데이트 코드 논리를 단계별로 실행할 수 있습니다.

백그라운드 작업이 완료되면 같은 디버그 세션 내의 Windows 시작 메뉴에서 포그라운드 앱을 시작할 수 있습니다. 디버거가 다시 자동으로 포그라운드 프로세스에 연결되고 앱의 논리를 단계별로 실행할 수 있습니다.

> [!NOTE]
> Visual Studio 2015 사용자: 위의 단계는 Visual Studio 2017 또는 Visual Studio 2019에 적용 됩니다. Visual Studio 2015를 사용하는 경우 동일할 기술을 사용하여 UpdateTask를 트리거하고 테스트할 수 있습니다. 단, Visual Studio가 연결되지 않습니다. VS 2015에서 대체 절차는 UpdateTask를 진입점으로 설정하는 [ApplicationTrigger](https://docs.microsoft.com/windows/uwp/launch-resume/trigger-background-task-from-app)를 설정하고 포그라운드 앱에서 바로 실행을 트리거하는 것입니다.

## <a name="see-also"></a>참조

[Out-of-process 백그라운드 작업 만들기 및 등록](https://docs.microsoft.com/windows/uwp/launch-resume/create-and-register-a-background-task)
