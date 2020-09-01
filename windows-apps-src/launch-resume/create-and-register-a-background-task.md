---
title: Out-of-process 백그라운드 작업 만들기 및 등록
description: Out-of-process 백그라운드 작업 클래스를 만들고 앱이 전경에 있지 않을 때 실행 되도록 등록 합니다.
ms.assetid: 4F98F6A3-0D3D-4EFB-BA8E-30ED37AE098B
ms.date: 02/27/2019
ms.topic: article
keywords: windows 10, uwp, 백그라운드 작업
ms.localizationpriority: medium
dev_langs:
- csharp
- cppwinrt
- cpp
ms.openlocfilehash: 638d4e8b4d4bc074566c8b0a6c24d733a5769b32
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89164897"
---
# <a name="create-and-register-an-out-of-process-background-task"></a>Out-of-process 백그라운드 작업 만들기 및 등록

**중요 API**

-   [**IBackgroundTask**](/uwp/api/Windows.ApplicationModel.Background.IBackgroundTask)
-   [**BackgroundTaskBuilder**](/uwp/api/Windows.ApplicationModel.Background.BackgroundTaskBuilder)
-   [**BackgroundTaskCompletedEventHandler**](/uwp/api/windows.applicationmodel.background.backgroundtaskcompletedeventhandler)

백그라운드 작업 클래스를 만들고 응용 프로그램이 전경에 있지 않을 때 실행 되도록 등록 합니다. 이 항목에서는 앱 프로세스와는 다른 별도의 프로세스에서 실행 되는 백그라운드 작업을 만들고 등록 하는 방법을 보여 줍니다. 포그라운드 응용 프로그램에서 직접 백그라운드 작업을 수행 하려면 [in-process 백그라운드 작업 만들기 및 등록](create-and-register-an-inproc-background-task.md)을 참조 하세요.

> [!NOTE]
> 백그라운드에서 미디어를 재생 하는 데 백그라운드 작업을 사용 하는 경우 Windows 10 버전 1607의 향상 된 기능에 대 한 자세한 내용을 보려면 [백그라운드에서 미디어 재생](../audio-video-camera/background-audio.md) 을 참조 하세요.

## <a name="create-the-background-task-class"></a>백그라운드 작업 클래스 만들기

[**IBackgroundTask**](/uwp/api/Windows.ApplicationModel.Background.IBackgroundTask) 인터페이스를 구현 하는 클래스를 작성 하 여 백그라운드에서 코드를 실행할 수 있습니다. 이 코드는 (예: [**Systemtrigger**](/uwp/api/Windows.ApplicationModel.Background.SystemTriggerType) 또는 [**MaintenanceTrigger**](/uwp/api/Windows.ApplicationModel.Background.MaintenanceTrigger))를 사용 하 여 특정 이벤트를 트리거할 때 실행 됩니다.

다음 단계에서는 [**IBackgroundTask**](/uwp/api/Windows.ApplicationModel.Background.IBackgroundTask) 인터페이스를 구현 하는 새 클래스를 작성 하는 방법을 보여 줍니다.

1.  백그라운드 작업에 대 한 새 프로젝트를 만들어 솔루션에 추가 합니다. 이렇게 하려면 **솔루션 탐색기** 에서 솔루션 노드를 마우스 오른쪽 단추로 클릭 하 고 **Add** \> **새 프로젝트**추가를 선택 합니다. 그런 다음 **Windows 런타임 구성 요소** 프로젝트 형식을 선택 하 고 프로젝트 이름을 지정한 다음 확인을 클릭 합니다.
2.  UWP (유니버설 Windows 플랫폼) 앱 프로젝트에서 백그라운드 작업 프로젝트를 참조 합니다. C # 또는 c + + 앱의 경우 앱 프로젝트에서 **참조** 를 마우스 오른쪽 단추로 클릭 하 고 **새 참조 추가**를 선택 합니다. **솔루션**에서 **프로젝트** 를 선택한 다음 백그라운드 작업 프로젝트의 이름을 선택 하 고 **확인**을 클릭 합니다.
3.  백그라운드 작업 프로젝트에 [**IBackgroundTask**](/uwp/api/Windows.ApplicationModel.Background.IBackgroundTask) 인터페이스를 구현 하는 새 클래스를 추가 합니다. [**IBackgroundTask**](/uwp/api/windows.applicationmodel.background.ibackgroundtask.run) 메서드는 지정 된 이벤트가 트리거될 때 호출 되는 필수 진입점입니다. 이 메서드는 모든 백그라운드 작업에 필요 합니다.

> [!NOTE]
> 백그라운드 작업 프로젝트의 백그라운드 작업 클래스 자체 &mdash; 와 기타 모든 클래스는 &mdash; **sealed** 또는 **final**인 **공용** 클래스 여야 합니다.

다음 샘플 코드는 백그라운드 작업 클래스의 매우 기본적인 시작 지점을 보여 줍니다.

```csharp
// ExampleBackgroundTask.cs
using Windows.ApplicationModel.Background;

namespace Tasks
{
    public sealed class ExampleBackgroundTask : IBackgroundTask
    {
        public void Run(IBackgroundTaskInstance taskInstance)
        {
            
        }        
    }
}
```

```cppwinrt
// First, add ExampleBackgroundTask.idl, and then build.
// ExampleBackgroundTask.idl
namespace Tasks
{
    [default_interface]
    runtimeclass ExampleBackgroundTask : Windows.ApplicationModel.Background.IBackgroundTask
    {
        ExampleBackgroundTask();
    }
}

// ExampleBackgroundTask.h
#pragma once

#include "ExampleBackgroundTask.g.h"

namespace winrt::Tasks::implementation
{
    struct ExampleBackgroundTask : ExampleBackgroundTaskT<ExampleBackgroundTask>
    {
        ExampleBackgroundTask() = default;

        void Run(Windows::ApplicationModel::Background::IBackgroundTaskInstance const& taskInstance);
    };
}

namespace winrt::Tasks::factory_implementation
{
    struct ExampleBackgroundTask : ExampleBackgroundTaskT<ExampleBackgroundTask, implementation::ExampleBackgroundTask>
    {
    };
}

// ExampleBackgroundTask.cpp
#include "pch.h"
#include "ExampleBackgroundTask.h"

namespace winrt::Tasks::implementation
{
    void ExampleBackgroundTask::Run(Windows::ApplicationModel::Background::IBackgroundTaskInstance const& taskInstance)
    {
        throw hresult_not_implemented();
    }
}
```

```cpp
// ExampleBackgroundTask.h
#pragma once

using namespace Windows::ApplicationModel::Background;

namespace Tasks
{
    public ref class ExampleBackgroundTask sealed : public IBackgroundTask
    {

    public:
        ExampleBackgroundTask();

        virtual void Run(IBackgroundTaskInstance^ taskInstance);
        void OnCompleted(
            BackgroundTaskRegistration^ task,
            BackgroundTaskCompletedEventArgs^ args
        );
    };
}

// ExampleBackgroundTask.cpp
#include "ExampleBackgroundTask.h"

using namespace Tasks;

void ExampleBackgroundTask::Run(IBackgroundTaskInstance^ taskInstance)
{
}
```

4.  백그라운드 작업에서 비동기 코드를 실행 하는 경우 백그라운드 작업에서 지연을 사용 해야 합니다. 지연을 사용 하지 않는 경우 비동기 작업이 완료 되기 전에 **실행** 메서드가 반환 되 면 백그라운드 작업 프로세스가 예기치 않게 종료 될 수 있습니다.

비동기 메서드를 호출 하기 전에 **Run** 메서드에서 지연 된 요청을 요청 합니다. 비동기 메서드에서 액세스할 수 있도록 지연을 클래스 데이터 멤버에 저장 합니다. 비동기 코드가 완료 된 후 지연 완료를 선언 합니다.

다음 샘플 코드는 지연 시간을 가져오고, 저장 하 고, 비동기 코드가 완료 되 면이를 해제 합니다.

```csharp
BackgroundTaskDeferral _deferral; // Note: defined at class scope so that we can mark it complete inside the OnCancel() callback if we choose to support cancellation
public async void Run(IBackgroundTaskInstance taskInstance)
{
    _deferral = taskInstance.GetDeferral();
    //
    // TODO: Insert code to start one or more asynchronous methods using the
    //       await keyword, for example:
    //
    // await ExampleMethodAsync();
    //

    _deferral.Complete();
}
```

```cppwinrt
// ExampleBackgroundTask.h
...
private:
    Windows::ApplicationModel::Background::BackgroundTaskDeferral m_deferral{ nullptr };

// ExampleBackgroundTask.cpp
...
Windows::Foundation::IAsyncAction ExampleBackgroundTask::Run(
    Windows::ApplicationModel::Background::IBackgroundTaskInstance const& taskInstance)
{
    m_deferral = taskInstance.GetDeferral(); // Note: defined at class scope so that we can mark it complete inside the OnCancel() callback if we choose to support cancellation.
    // TODO: Modify the following line of code to call a real async function.
    co_await ExampleCoroutineAsync(); // Run returns at this point, and resumes when ExampleCoroutineAsync completes.
    m_deferral.Complete();
}
```

```cpp
void ExampleBackgroundTask::Run(IBackgroundTaskInstance^ taskInstance)
{
    m_deferral = taskInstance->GetDeferral(); // Note: defined at class scope so that we can mark it complete inside the OnCancel() callback if we choose to support cancellation.

    //
    // TODO: Modify the following line of code to call a real async function.
    //       Note that the task<void> return type applies only to async
    //       actions. If you need to call an async operation instead, replace
    //       task<void> with the correct return type.
    //
    task<void> myTask(ExampleFunctionAsync());

    myTask.then([=]() {
        m_deferral->Complete();
    });
}
```

> [!NOTE]
> C #에서는 **async/wait** 키워드를 사용 하 여 백그라운드 작업의 비동기 메서드를 호출할 수 있습니다. C + +/CX에서는 작업 체인을 사용 하 여 비슷한 결과를 얻을 수 있습니다.

비동기 패턴에 대 한 자세한 내용은 [비동기 프로그래밍](../threading-async/asynchronous-programming-universal-windows-platform-apps.md)을 참조 하세요. 지연과를 사용 하 여 백그라운드 작업을 일찍 중지 하지 않도록 하는 방법에 대 한 추가 예제는 [백그라운드 작업 샘플](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/BackgroundTask)을 참조 하세요.

다음 단계는 앱 클래스 중 하나에서 완료 됩니다 (예: MainPage.xaml.cs).

> [!NOTE]
> 백그라운드 작업 등록을 위한 전용 함수를 만들 수도 있습니다 &mdash; . [백그라운드 작업 등록](register-a-background-task.md)을 참조 하세요. 이 경우 다음 세 단계를 사용 하는 대신 트리거를 생성 하 고 작업 이름, 작업 진입점 및 (선택 사항) 조건에 따라 등록 함수에 제공할 수 있습니다.

## <a name="register-the-background-task-to-run"></a>실행할 백그라운드 작업 등록

1.  [**BackgroundTaskRegistration**](/uwp/api/windows.applicationmodel.background.backgroundtaskregistration.alltasks) 속성을 반복 하 여 백그라운드 작업을 이미 등록 했는지 여부를 확인 합니다. 이 단계는 중요 합니다. 앱에서 기존 백그라운드 작업 등록을 확인 하지 않는 경우 작업을 여러 번 등록 하 여 성능에 문제가 발생 하 고 작업을 완료할 수 있을 때까지 작업의 사용 가능한 CPU 시간을 제한 합니다.

다음 예에서는 **Alltasks** 속성을 반복 하 고 태스크가 이미 등록 된 경우 플래그 변수를 true로 설정 합니다.

```csharp
var taskRegistered = false;
var exampleTaskName = "ExampleBackgroundTask";

foreach (var task in BackgroundTaskRegistration.AllTasks)
{
    if (task.Value.Name == exampleTaskName)
    {
        taskRegistered = true;
        break;
    }
}
```

```cppwinrt
std::wstring exampleTaskName{ L"ExampleBackgroundTask" };

auto allTasks{ Windows::ApplicationModel::Background::BackgroundTaskRegistration::AllTasks() };

bool taskRegistered{ false };
for (auto const& task : allTasks)
{
    if (task.Value().Name() == exampleTaskName)
    {
        taskRegistered = true;
        break;
    }
}

// The code in the next step goes here.
```

```cpp
boolean taskRegistered = false;
Platform::String^ exampleTaskName = "ExampleBackgroundTask";

auto iter = BackgroundTaskRegistration::AllTasks->First();
auto hascur = iter->HasCurrent;

while (hascur)
{
    auto cur = iter->Current->Value;

    if(cur->Name == exampleTaskName)
    {
        taskRegistered = true;
        break;
    }

    hascur = iter->MoveNext();
}
```

2.  백그라운드 태스크가 아직 등록 되지 않은 경우 [**BackgroundTaskBuilder**](/uwp/api/Windows.ApplicationModel.Background.BackgroundTaskBuilder) 를 사용 하 여 백그라운드 작업의 인스턴스를 만듭니다. 작업 진입점은 네임 스페이스가 접두사로 지정 된 백그라운드 작업 클래스의 이름 이어야 합니다.

백그라운드 작업은 백그라운드 작업이 실행 될 때 컨트롤을 트리거합니다. 가능한 트리거의 목록은 [**Systemtrigger**](/uwp/api/Windows.ApplicationModel.Background.SystemTriggerType)를 참조 하세요.

예를 들어 다음 코드는 새 백그라운드 작업을 만들고 **TimeZoneChanged** 트리거가 발생할 때 실행 되도록 설정 합니다.

```csharp
var builder = new BackgroundTaskBuilder();

builder.Name = exampleTaskName;
builder.TaskEntryPoint = "Tasks.ExampleBackgroundTask";
builder.SetTrigger(new SystemTrigger(SystemTriggerType.TimeZoneChange, false));
```

```cppwinrt
if (!taskRegistered)
{
    Windows::ApplicationModel::Background::BackgroundTaskBuilder builder;
    builder.Name(exampleTaskName);
    builder.TaskEntryPoint(L"Tasks.ExampleBackgroundTask");
    builder.SetTrigger(Windows::ApplicationModel::Background::SystemTrigger{
        Windows::ApplicationModel::Background::SystemTriggerType::TimeZoneChange, false });
    // The code in the next step goes here.
}
```

```cpp
auto builder = ref new BackgroundTaskBuilder();

builder->Name = exampleTaskName;
builder->TaskEntryPoint = "Tasks.ExampleBackgroundTask";
builder->SetTrigger(ref new SystemTrigger(SystemTriggerType::TimeZoneChange, false));
```

3.  트리거 이벤트가 발생 한 후 태스크가 실행 되는 시기를 제어 하는 조건을 추가할 수 있습니다 (옵션). 예를 들어 사용자가 있을 때까지 작업을 실행 하지 않으려면 **Userpresent**조건을 사용 합니다. 가능한 조건 목록은 [**SystemConditionType**](/uwp/api/Windows.ApplicationModel.Background.SystemConditionType)를 참조 하세요.

다음 샘플 코드에서는 사용자를 표시 해야 하는 조건을 할당 합니다.

```csharp
builder.AddCondition(new SystemCondition(SystemConditionType.UserPresent));
```

```cppwinrt
builder.AddCondition(Windows::ApplicationModel::Background::SystemCondition{ Windows::ApplicationModel::Background::SystemConditionType::UserPresent });
// The code in the next step goes here.
```

```cpp
builder->AddCondition(ref new SystemCondition(SystemConditionType::UserPresent));
```

4.  [**BackgroundTaskBuilder**](/uwp/api/Windows.ApplicationModel.Background.BackgroundTaskBuilder) 개체에 대해 register 메서드를 호출 하 여 백그라운드 작업을 등록 합니다. [**BackgroundTaskRegistration**](/uwp/api/Windows.ApplicationModel.Background.BackgroundTaskRegistration) 결과를 저장 하 여 다음 단계에서 사용할 수 있도록 합니다.

다음 코드는 백그라운드 작업을 등록 하 고 결과를 저장 합니다.

```csharp
BackgroundTaskRegistration task = builder.Register();
```

```cppwinrt
Windows::ApplicationModel::Background::BackgroundTaskRegistration task{ builder.Register() };
```

```cpp
BackgroundTaskRegistration^ task = builder->Register();
```

> [!NOTE]
> 유니버설 Windows 앱은 백그라운드 트리거 형식을 등록 하기 전에 [**Requestaccessasync**](/uwp/api/windows.applicationmodel.background.backgroundexecutionmanager.requestaccessasync) 를 호출 해야 합니다.

업데이트를 릴리스된 후 유니버설 Windows 앱이 계속 실행 되도록 하려면 **ServicingComplete** ( [systemtriggertype](/uwp/api/Windows.ApplicationModel.Background.SystemTriggerType)참조) 트리거를 사용 하 여 앱 데이터베이스 마이그레이션 및 백그라운드 작업 등록과 같은 업데이트 후 구성 변경 작업을 수행 합니다. 지금은 이전 버전의 앱과 연결 된 백그라운드 작업 ( [**Removeaccess**](/uwp/api/windows.applicationmodel.background.backgroundexecutionmanager.removeaccess)참조) 및 새 버전의 앱에 대 한 백그라운드 작업 등록을 취소 하는 것이 가장 좋습니다 ( [**requestaccessasync**](/uwp/api/windows.applicationmodel.background.backgroundexecutionmanager.requestaccessasync)참조).

자세한 내용은 [백그라운드 작업에 대 한 지침](guidelines-for-background-tasks.md)을 참조 하세요.

## <a name="handle-background-task-completion-using-event-handlers"></a>이벤트 처리기를 사용 하 여 백그라운드 작업 완료 처리

앱이 백그라운드 작업에서 결과를 가져올 수 있도록 [**BackgroundTaskCompletedEventHandler**](/uwp/api/windows.applicationmodel.background.backgroundtaskcompletedeventhandler)에 메서드를 등록 해야 합니다. 앱이 시작 되거나 다시 시작 되 면 응용 프로그램이 포그라운드에서 마지막으로 있었던 이후 백그라운드 작업이 완료 되 면 표시 된 메서드가 호출 됩니다. 응용 프로그램이 현재 포그라운드로 있는 동안 백그라운드 작업이 완료 되 면 OnCompleted 메서드가 즉시 호출 됩니다.

1.  백그라운드 작업 완료를 처리 하는 OnCompleted 메서드를 작성 합니다. 예를 들어 백그라운드 작업 결과가 UI 업데이트를 발생 시킬 수 있습니다. 여기에 표시 된 메서드 공간은이 예제가 *args* 매개 변수를 사용 하지 않더라도 oncompleted 이벤트 처리기 메서드에 필요 합니다.

다음 샘플 코드는 백그라운드 작업 완료를 인식 하 고 메시지 문자열을 사용 하는 예제 UI 업데이트 메서드를 호출 합니다.

```csharp
private void OnCompleted(IBackgroundTaskRegistration task, BackgroundTaskCompletedEventArgs args)
{
    var settings = Windows.Storage.ApplicationData.Current.LocalSettings;
    var key = task.TaskId.ToString();
    var message = settings.Values[key].ToString();
    UpdateUI(message);
}
```

```cppwinrt
void UpdateUI(winrt::hstring const& message)
{
    MyTextBlock().Dispatcher().RunAsync(Windows::UI::Core::CoreDispatcherPriority::Normal, [=]()
    {
        MyTextBlock().Text(message);
    });
}

void OnCompleted(
    Windows::ApplicationModel::Background::BackgroundTaskRegistration const& sender,
    Windows::ApplicationModel::Background::BackgroundTaskCompletedEventArgs const& /* args */)
{
    // You'll previously have inserted this key into local settings.
    auto settings{ Windows::Storage::ApplicationData::Current().LocalSettings().Values() };
    auto key{ winrt::to_hstring(sender.TaskId()) };
    auto message{ winrt::unbox_value<winrt::hstring>(settings.Lookup(key)) };

    UpdateUI(message);
}
```

```cpp
void MainPage::OnCompleted(BackgroundTaskRegistration^ task, BackgroundTaskCompletedEventArgs^ args)
{
    auto settings = ApplicationData::Current->LocalSettings->Values;
    auto key = task->TaskId.ToString();
    auto message = dynamic_cast<String^>(settings->Lookup(key));
    UpdateUI(message);
}
```

> [!NOTE]
> Ui 스레드를 유지 하지 않으려면 UI 업데이트를 비동기적으로 수행 해야 합니다. 예제는 [백그라운드 작업 샘플](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/BackgroundTask)에서 updateui 메서드를 참조 하세요.

2.  백그라운드 작업을 등록 한 위치로 돌아갑니다. 해당 코드 줄 뒤에 새 [**BackgroundTaskCompletedEventHandler**](/uwp/api/windows.applicationmodel.background.backgroundtaskcompletedeventhandler) 개체를 추가 합니다. OnCompleted 메서드를 **BackgroundTaskCompletedEventHandler** 생성자에 대 한 매개 변수로 제공 합니다.

다음 샘플 코드는 [**BackgroundTaskRegistration**](/uwp/api/Windows.ApplicationModel.Background.BackgroundTaskRegistration)에 [**BackgroundTaskCompletedEventHandler**](/uwp/api/windows.applicationmodel.background.backgroundtaskcompletedeventhandler) 를 추가 합니다.

```csharp
task.Completed += new BackgroundTaskCompletedEventHandler(OnCompleted);
```

```cppwinrt
task.Completed({ this, &MainPage::OnCompleted });
```

```cpp
task->Completed += ref new BackgroundTaskCompletedEventHandler(this, &MainPage::OnCompleted);
```

## <a name="declare-in-the-app-manifest-that-your-app-uses-background-tasks"></a>앱이 백그라운드 작업을 사용 하는 앱 매니페스트에서 선언

앱이 백그라운드 작업을 실행 하기 전에 응용 프로그램 매니페스트에서 각 백그라운드 작업을 선언 해야 합니다. 앱이 매니페스트에 나열 되지 않은 트리거를 사용 하 여 백그라운드 작업을 등록 하려고 하면 백그라운드 작업 등록이 실패 하 고 "런타임 클래스가 등록 되지 않았습니다." 오류가 표시 됩니다.

1.  Appxmanifest.xml 라는 파일을 열어 패키지 매니페스트 디자이너를 엽니다.
2.  **선언** 탭을 엽니다.
3.  **사용 가능한 선언** 드롭다운에서 **백그라운드 작업** 을 선택 하 고 **추가**를 클릭 합니다.
4.  **시스템 이벤트** 확인란을 선택 합니다.
5.  **진입점:** 텍스트 상자에이 예제에 사용할 배경 클래스의 네임 스페이스와 이름을 입력 합니다 (ExampleBackgroundTask).
6.  수동 디자이너를 닫습니다.

다음 확장명 요소는 appxmanifest.xml 파일에 추가 되어 백그라운드 작업을 등록 합니다.

```xml
<Extensions>
  <Extension Category="windows.backgroundTasks" EntryPoint="Tasks.ExampleBackgroundTask">
    <BackgroundTasks>
      <Task Type="systemEvent" />
    </BackgroundTasks>
  </Extension>
</Extensions>
```

## <a name="summary-and-next-steps"></a>요약 및 다음 단계

이제 백그라운드 작업 클래스를 작성 하는 방법, 앱 내에서 백그라운드 작업을 등록 하는 방법 및 백그라운드 작업이 완료 될 때 앱을 인식 하도록 설정 하는 방법에 대 한 기본 사항을 이해 해야 합니다. 앱이 백그라운드 작업을 성공적으로 등록할 수 있도록 응용 프로그램 매니페스트를 업데이트 하는 방법도 알아야 합니다.

> [!NOTE]
> 백그라운드 작업을 사용 하는 완전 하 고 강력한 UWP 앱의 컨텍스트에서 유사한 코드 예제를 보려면 [백그라운드 작업 샘플](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/BackgroundTask) 을 다운로드 합니다.

백그라운드 작업을 사용 하는 앱을 작성 하는 방법에 대 한 API 참조, 백그라운드 작업 개념 지침 및 자세한 지침은 다음 관련 항목을 참조 하세요.

## <a name="related-topics"></a>관련 항목

**자세한 백그라운드 작업 지침 항목**

* [백그라운드 작업으로 시스템 이벤트에 응답](respond-to-system-events-with-background-tasks.md)
* [백그라운드 작업 등록](register-a-background-task.md)
* [백그라운드 작업 실행 조건 설정](set-conditions-for-running-a-background-task.md)
* [유지 관리 트리거 사용](use-a-maintenance-trigger.md)
* [취소된 백그라운드 작업 처리](handle-a-cancelled-background-task.md)
* [백그라운드 작업 진행 및 완료 모니터링](monitor-background-task-progress-and-completion.md)
* [타이머에 따라 백그라운드 작업 실행](run-a-background-task-on-a-timer-.md)
* [In-process 백그라운드 작업을 만들고 등록](create-and-register-an-inproc-background-task.md)합니다.
* [Out-of-process 백그라운드 작업을 in-process 백그라운드 작업으로 변환](convert-out-of-process-background-task.md)  

**백그라운드 작업 지침**

* [백그라운드 작업 지침](guidelines-for-background-tasks.md)
* [백그라운드 작업 디버그](debug-a-background-task.md)
* [UWP 앱에서 일시 중단, 다시 시작 및 백그라운드 이벤트를 트리거하는 방법 (디버깅 시)](/previous-versions/hh974425(v=vs.110))

**백그라운드 작업 API 참조**

* [**Windows.ApplicationModel.Background**](/uwp/api/Windows.ApplicationModel.Background)