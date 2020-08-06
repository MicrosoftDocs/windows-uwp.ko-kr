---
title: Winmain 백그라운드 작업 만들기 및 등록
description: 패키지 된 winmain 앱이 실행 되 고 있지 않을 수 있는 경우 기본 프로세스 또는 out-of-process에서 실행할 수 있는 COM 백그라운드 작업을 만듭니다.
ms.assetid: 8CBD4986-6E65-4374-BC7C-C38908E417E1
ms.date: 03/27/2020
ms.topic: article
keywords: windows 10, 데스크톱 브리지, 스파스 서명 된 패키지, winmain, 백그라운드 작업
ms.localizationpriority: medium
dev_langs:
- csharp
- cppwinrt
ms.openlocfilehash: 1e06a87ce771f603721c928b984d0f57d8e45013
ms.sourcegitcommit: 1d53d89bd3d044f4a2dc290b93c1ad15a088b361
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/04/2020
ms.locfileid: "87547315"
---
# <a name="create-and-register-a-winmain-com-background-task"></a>Winmain COM 백그라운드 작업 만들기 및 등록

> [!TIP]
> BackgroundTaskBuilder SetTaskEntryPointClsid 메서드는 Windows 10 버전 2004부터 사용할 수 있습니다.

> [!NOTE]
> 이 시나리오는 패키지 된 WinMain 앱에만 적용 됩니다. UWP 응용 프로그램에서이 시나리오를 구현 하는 동안 오류가 발생 합니다.

**중요 API**

-   [**IBackgroundTask**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Background.IBackgroundTask)
-   [**BackgroundTaskBuilder**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Background.BackgroundTaskBuilder)

COM 백그라운드 작업 클래스를 만들고 트리거에 대 한 응답으로 완전 신뢰 패키지 winmain 앱에서 실행 되도록 등록 합니다. 백그라운드 작업을 사용 하 여 앱이 일시 중단 되거나 실행 되지 않는 경우 기능을 제공할 수 있습니다. 이 항목에서는 포그라운드 앱 프로세스나 다른 프로세스에서 실행할 수 있는 백그라운드 작업을 만들고 등록 하는 방법을 보여 줍니다.

## <a name="create-the-background-task-class"></a>백그라운드 작업 클래스 만들기

[**IBackgroundTask**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Background.IBackgroundTask) 인터페이스를 구현 하는 클래스를 작성 하 여 백그라운드에서 코드를 실행할 수 있습니다. 이 코드는 (예: [**Systemtrigger**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Background.SystemTriggerType) 또는 [**TimeTrigger**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Background.TimeTrigger))를 사용 하 여 특정 이벤트를 트리거할 때 실행 됩니다.

다음 단계에서는 [**IBackgroundTask**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Background.IBackgroundTask) 인터페이스를 구현 하 고 기본 프로세스에 추가 하는 새 클래스를 작성 하는 방법을 보여 줍니다.

1.  패키지 된 WinMain 응용 프로그램 솔루션에서 WinRT Api를 참조 하려면 [**다음 지침을 참조**](https://docs.microsoft.com/windows/apps/desktop/modernize/desktop-to-uwp-enhance) 하세요. 이는 IBackgroundTask 및 관련 Api를 사용 하는 데 필요 합니다.
2.  새 클래스에서 [**IBackgroundTask**](/uwp/api/Windows.ApplicationModel.Background.IBackgroundTask) 인터페이스를 구현 합니다. [**IBackgroundTask**](/uwp/api/windows.applicationmodel.background.ibackgroundtask.run) 메서드는 지정 된 이벤트가 트리거될 때 호출 되는 필수 진입점입니다. 이 메서드는 모든 백그라운드 작업에 필요 합니다.

> [!NOTE]
> 백그라운드 작업 클래스 자체 &mdash; 와 백그라운드 작업 프로젝트의 다른 모든 클래스는 &mdash; **public**이어야 합니다.

다음 샘플 코드에서는 prime를 계산 하 고 취소가 요청 될 때까지 파일에 쓰는 기본적인 백그라운드 작업 클래스를 보여 줍니다.

C + +/WinRT 예제는 백그라운드 작업 클래스를 [**COM coclass**](https://docs.microsoft.com/windows/uwp/cpp-and-winrt-apis/author-coclasses#implement-the-coclass-and-class-factory)로 구현 합니다.


<details>
<summary>백그라운드 작업 코드 샘플</summary>
<p>

```csharp

using System;
using System.IO; // Path
using System.Threading; // EventWaitHandle
using System.Collections.Generic; // Queue
using System.Runtime.InteropServices; // Guid, RegistrationServices
using Windows.ApplicationModel.Background; // IBackgroundTask

namespace PackagedWinMainBackgroundTaskSample
{
    // {14C5882B-35D3-41BE-86B2-5106269B97E6} is GUID to register this task with BackgroundTaskBuilder. Generate a random GUID before implementing.
    [ComVisible(true)]
    [ClassInterface(ClassInterfaceType.None)]
    [Guid("14C5882B-35D3-41BE-86B2-5106269B97E6")]
    [ComSourceInterfaces(typeof(IBackgroundTask))]
    public class SampleTask : IBackgroundTask
    {
        private volatile int cleanupTask; // flag used to indicate to Run method that it should exit
        private Queue<int> numbersQueue; // the data structure holding the set of primes in memory

        private const int maxPrimeNumber = 1000000000; // the number up to which task will attempt to calculate primes
        private const int queueDepthToWrite = 10; // how frequently this task should flush its queue of primes
        private const string numbersQueueFile = "numbersQueue.log"; // the file to write to relative to AppData

        public SampleTask()
        {
            cleanupTask = 0;
            numbersQueue = new Queue<int>(queueDepthToWrite);
        }

        /// <summary>
        /// This method writes all the numbers in the current queue to the specified file.
        /// </summary>
        private void FlushNumbersToFile(Queue<int> queueToWrite)
        {
            string logPath = Path.Combine(ApplicationData.Current.LocalFolder.Path,
                                        System.Diagnostics.Process.GetCurrentProcess().ProcessName);

            if (!Directory.Exists(logPath))
            {
                Directory.CreateDirectory(logPath);
            }

            logPath = Path.Combine(logPath, numbersQueueFile);

            const string delimiter = ", ";
            UnicodeEncoding unicodeEncoding = new UnicodeEncoding();
            // convert the queue to a list of comma separated values.
            string stringToWrite = String.Join(delimiter, queueToWrite);
            // Add the comma at the end.
            stringToWrite += delimiter;

            File.AppendAllText(logPath, stringToWrite);
        }

        /// <summary>
        /// This method determines if the specified number is a prime number.
        /// </summary>
        private bool IsPrimeNumber(int dividend)
        {
            bool isPrime = true;
            for (int divisor = dividend - 1; divisor > 1; divisor -= 1)
            {
                if ((dividend % divisor) == 0)
                {
                    isPrime = false;
                    break;
                }
            }

            return isPrime;
        }

        /// <summary>
        /// Given the current number, this method calculates the next prime number (excluding the specified number).
        /// </summary>
        private int GetNextPrime(int previousNumber)
        {
            int currentNumber = previousNumber + 1;
            while (!IsPrimeNumber(currentNumber))
            {
                currentNumber += 1;
            }

            return currentNumber;
        }

        /// <summary>
        /// This method is the main entry point for the background task. The system will believe this background task
        /// is complete when this method returns.
        /// </summary>
        [MTAThread]
        public void Run(IBackgroundTaskInstance taskInstance)
        {
            // Start with the first applicable number.
            int currentNumber = 1;

            taskDeferral = taskInstance.GetDeferral();

            // Wire the cancellation handler.
            taskInstance.Canceled += this.OnCanceled;

            // Set the progress to indicate this task has started
            taskInstance.Progress = 10;

            // Calculate primes until a cancellation has been requested or until
            // the maximum number is reached.
            while ((cleanupTask == 0) && (currentNumber < maxPrimeNumber)) {
                // Compute the next prime number and add it to our queue.
                currentNumber = GetNextPrime(currentNumber);
                numbersQueue.Enqueue(currentNumber);
                // Once the queue is filled to its max size, flush the numbers to the file.
                if (numbersQueue.Count >= queueDepthToWrite)
                {
                    FlushNumbersToFile(numbersQueue);
                    numbersQueue.Clear();
                }
            }

            // Flush any remaining numbers to the file as part of cleanup.
            FlushNumbersToFile(numbersQueue);

            if (taskDeferral != null)
            {
                taskDeferral.Complete();
            }
        }

        /// <summary>
        /// This method is signaled when the system requests the background task be canceled. This method will signal
        /// to the Run method to clean up and return.
        /// </summary>
        [MTAThread]
        public void OnCanceled(IBackgroundTaskInstance taskInstance, BackgroundTaskCancellationReason cancellationReason)
        {
            cleanupTask = 1;
        }
    }
}

```

```cppwinrt

#include <unknwn.h>
#include <winrt/Windows.Foundation.h>
#include <winrt/Windows.Foundation.Collections.h>
#include <winrt/Windows.ApplicationModel.Background.h>

using namespace winrt;
using namespace winrt::Windows::Foundation;
using namespace winrt::Windows::Foundation::Collections;
using namespace winrt::Windows::ApplicationModel::Background;

namespace PackagedWinMainBackgroundTaskSample {

    // Note insert unique UUID.
    struct __declspec(uuid("14C5882B-35D3-41BE-86B2-5106269B97E6"))
    SampleTask : implements<SampleTask, IBackgroundTask>
    {
        const unsigned int MaximumPotentialPrime = 1000000000;
        volatile bool isCanceled = false;
        BackgroundTaskDeferral taskDeferral = nullptr;

        void __stdcall Run (_In_ IBackgroundTaskInstance taskInstance)
        {
            taskInstance.Canceled({ this, &SampleTask::OnCanceled });

            taskDeferral = taskInstance.GetDeferral();

            unsigned int currentPrimeNumber = 1;
            while (!isCanceled && (currentPrimeNumber < MaximumPotentialPrime))
            {
                currentPrimeNumber = GetNextPrime(currentPrimeNumber);
            }

            taskDeferral.Complete();
        }

        void __stdcall OnCanceled (_In_ IBackgroundTaskInstance, _In_ BackgroundTaskCancellationReason)
        {
            isCanceled = true;
        }
    };

    struct TaskFactory : implements<TaskFactory, IClassFactory>
    {
        HRESULT __stdcall CreateInstance (_In_opt_ IUnknown* aggregateInterface, _In_ REFIID interfaceId, _Outptr_ VOID** object) noexcept final
        {
            if (aggregateInterface != NULL) {
                return CLASS_E_NOAGGREGATION;
            }

            return make<SampleTask>().as(interfaceId, object);
        }

        HRESULT __stdcall LockServer (BOOL) noexcept final
        {
            return S_OK;
        }
    };
}

```

</p>
</details>


## <a name="add-the-support-code-to-instantiate-the-com-class"></a>지원 코드를 추가 하 여 COM 클래스 인스턴스화

백그라운드 작업을 완전 신뢰 winmain 응용 프로그램으로 활성화 하려면 백그라운드 작업 클래스에서 응용 프로그램 프로세스가 실행 되 고 있지 않은 경우 응용 프로그램 프로세스를 시작 하는 방법을 이해 하 고 해당 백그라운드 작업에 대 한 새 활성화를 처리 하기 위해 현재 서버에 해당 하는 프로세스의 인스턴스를 이해 하는 지원 코드를 포함 해야 합니다.

1.  COM은 앱 프로세스가 아직 실행 되 고 있지 않은 경우이를 시작 하는 방법을 이해 해야 합니다. 백그라운드 작업 코드를 호스트 하는 앱 프로세스를 패키지 매니페스트에 선언 해야 합니다. 다음 샘플 코드는 **SampleBackgroundApp.exe**내에서 **sampletask** 호스트 하는 방법을 보여 줍니다. 프로세스가 실행 되 고 있지 않을 때 백그라운드 작업이 시작 되 면 **"-startsampletaskserver"** 프로세스 인수를 사용 하 여 **SampleBackgroundApp.exe** 시작 됩니다.

```xml

<Extensions>
  <com:Extension Category="windows.comServer">
    <com:ComServer>
      <com:ExeServer Executable="SampleBackgroundApp\SampleBackgroundApp.exe" DisplayName="SampleBackgroundApp" Arguments="-StartSampleTaskServer">
        <com:Class Id="14C5882B-35D3-41BE-86B2-5106269B97E6" DisplayName="Sample Task" />
      </com:ExeServer>
    </com:ComServer>
  </com:Extension>
</Extensions>

```

2.  올바른 인수를 사용 하 여 프로세스를 시작한 후에는 COM에 SampleTask 새 인스턴스에 대 한 현재 COM 서버 임을 알려 주어 야 합니다. 다음 샘플 코드에서는 응용 프로그램 프로세스를 COM에 등록 하는 방법을 보여 줍니다. 참고이 샘플은 프로세스가 종료 되기 전에 하나 이상의 인스턴스에 대해 SampleTask COM 서버로 선언 하는 방법을 보여 줍니다. 이 옵션은 선택 사항이 며 백그라운드 작업을 처리 하면 기본 프로세스 함수를 시작할 수 있습니다.

```csharp

class SampleTaskServer
{
    SampleTaskServer()
    {
        comRegistrationToken = 0;
        waitHandle = new EventWaitHandle(false, EventResetMode.AutoReset);
    }

    ~SampleTaskServer()
    {
        Stop();
    }

    public void Start()
    {
        RegistrationServices registrationServices = new RegistrationServices();
        comRegistrationToken = registrationServices.RegisterTypeForComClients(typeof(SampleTask), RegistrationClassContext.LocalServer, RegistrationConnectionType.MultipleUse);

        // Either have the background task signal this handle when it completes, or never signal this handle to keep this
        // process as the COM server until the process is closed.
        waitHandle.WaitOne();
    }

    public void Stop()
    {
        if (comRegistrationToken != 0)
        {
            RegistrationServices registrationServices = new RegistrationServices();
            registrationServices.UnregisterTypeForComClients(registrationCookie);
        }

        waitHandle.Set();
    }

    private int comRegistrationToken;
    private EventWaitHandle waitHandle;
}

var sampleTaskServer = new SampleTaskServer();
sampleTaskServer.Start();

```

```cppwinrt

class SampleTaskServer
{
public:
    SampleTaskServer()
    {
        waitHandle = EventWaitHandle(false, EventResetMode::AutoResetEvent);
        comRegistrationToken = 0;
    }

    ~SampleTaskServer()
    {
        Stop();
    }

    void Start()
    {
        try
        {
            com_ptr<IClassFactory> taskFactory = make<TaskFactory>();

            winrt::check_hresult(CoRegisterClassObject(__uuidof(SampleTask),
                                                       taskFactory.get(),
                                                       CLSCTX_LOCAL_SERVER,
                                                       REGCLS_MULTIPLEUSE,
                                                       &comRegistrationToken));

            // Either have the background task signal this handle when it completes, or never signal this handle to
            // keep this process as the COM server until the process is closed.
            waitHandle.WaitOne();

        }
        catch (...)
        {
            // Indicate an error has been encountered.
        }
    }

    void Stop()
    {
        if (comRegistrationToken != 0)
        {
            CoRevokeClassObject(comRegistrationToken);
        }

        waitHandle.Set();
    }

private:
    DWORD comRegistrationToken;
    EventWaitHandle waitHandle;
};

SampleTaskServer sampleTaskServer;
sampleTaskServer.Start();

```


## <a name="register-the-background-task-to-run"></a>실행할 백그라운드 작업 등록

1.  [**BackgroundTaskRegistration**](https://docs.microsoft.com/uwp/api/windows.applicationmodel.background.backgroundtaskregistration.alltasks) 속성을 반복 하 여 백그라운드 작업을 이미 등록 했는지 여부를 확인 합니다. *이 단계는 중요*합니다. 앱에서 기존 백그라운드 작업 등록을 확인 하지 않는 경우 작업을 여러 번 등록 하 여 성능에 문제가 발생 하 고 작업을 완료할 수 있을 때까지 작업의 사용 가능한 CPU 시간을 제한 합니다. 응용 프로그램은 동일한 진입점을 사용 하 여 모든 백그라운드 작업을 처리 하 고 [**BackgroundTaskRegistration**](https://docs.microsoft.com/uwp/api/windows.applicationmodel.background.backgroundtaskregistration) 에 할당 된 [**이름**](https://docs.microsoft.com/uwp/api/windows.applicationmodel.background.backgroundtaskregistration.name#Windows_ApplicationModel_Background_BackgroundTaskRegistration_Name) 또는 [**TaskId**](https://docs.microsoft.com/uwp/api/windows.applicationmodel.background.backgroundtaskregistration.taskid#Windows_ApplicationModel_Background_BackgroundTaskRegistration_TaskId) 와 같은 기타 속성을 사용 하 여 수행할 작업을 결정 합니다.

다음 예에서는 **Alltasks** 속성을 반복 하 고 태스크가 이미 등록 된 경우 플래그 변수를 true로 설정 합니다.

```csharp

var taskRegistered = false;
var sampleTaskName = "SampleTask";

foreach (var task in BackgroundTaskRegistration.AllTasks)
{
    if (task.Value.Name == sampleTaskName)
    {
        taskRegistered = true;
        break;
    }
}

// The code in the next step goes here.

```

```cppwinrt

bool taskRegistered = false;
std::wstring sampleTaskName = L"SampleTask";
auto allTasks = BackgroundTaskRegistration::AllTasks();

for (auto const& task : allTasks)
{
    if (task.Value().Name() == sampleTaskName)
    {
        taskRegistered = true;
        break;
    }
}

// The code in the next step goes here.

```

1.  백그라운드 태스크가 아직 등록 되지 않은 경우 [**BackgroundTaskBuilder**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Background.BackgroundTaskBuilder) 를 사용 하 여 백그라운드 작업의 인스턴스를 만듭니다. 작업 진입점은 네임 스페이스가 접두사로 지정 된 백그라운드 작업 클래스의 이름 이어야 합니다.

백그라운드 작업은 백그라운드 작업이 실행 될 때 컨트롤을 트리거합니다. 가능한 트리거의 목록은 [**Windows. ApplicationModel. Background**](https://docs.microsoft.com/uwp/api/windows.applicationmodel.background) 네임 스페이스를 참조 하세요.

> [!NOTE]
> 패키지 된 winmain 백그라운드 작업에는 트리거의 하위 집합만 지원 됩니다.

예를 들어이 코드는 새 백그라운드 작업을 만들고 15 분 되풀이 [**TimeTrigger**]()실행 되도록 설정 합니다.

```csharp

if (!taskRegistered)
{
    var builder = new BackgroundTaskBuilder();

    builder.Name = sampleTaskName;
    builder.SetTaskEntryPointClsid(typeof(SampleTask).GUID);
    builder.SetTrigger(new TimeTrigger(15, false));
}

// The code in the next step goes here.

```

```cppwinrt

if (!taskRegistered)
{
    BackgroundTaskBuilder builder;

    builder.Name(sampleTaskName);
    builder.SetTaskEntryPointClsid(__uuidof(SampleTask));
    builder.SetTrigger(TimeTrigger(15, false));
}

// The code in the next step goes here.

```

1.  트리거 이벤트가 발생 한 후 태스크가 실행 되는 시기를 제어 하는 조건을 추가할 수 있습니다 (옵션). 예를 들어 인터넷을 사용할 수 있을 때까지 작업을 실행 하지 않으려면 **Internetavailable**상태를 사용 합니다. 가능한 조건 목록은 [**SystemConditionType**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Background.SystemConditionType)를 참조 하세요.

다음 샘플 코드에서는 사용자를 표시 해야 하는 조건을 할당 합니다.

```csharp
builder.AddCondition(new SystemCondition(SystemConditionType.InternetAvailable));
// The code in the next step goes here.
```

```cppwinrt
builder.AddCondition(SystemCondition{ SystemConditionType::InternetAvailable });
// The code in the next step goes here.
```

4.  [**BackgroundTaskBuilder**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Background.BackgroundTaskBuilder) 개체에 대해 register 메서드를 호출 하 여 백그라운드 작업을 등록 합니다. [**BackgroundTaskRegistration**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Background.BackgroundTaskRegistration) 결과를 저장 하 여 다음 단계에서 사용할 수 있도록 합니다. Register 함수는 예외 형식으로 오류를 반환할 수 있습니다. Try catch에서 Register를 호출 해야 합니다.

다음 코드는 백그라운드 작업을 등록 하 고 결과를 저장 합니다.

```csharp

try
{
    var task = builder.Register();
}
catch (...)
{
    // Indicate an error was encountered.
}

```

```cppwinrt

try
{
    auto task = builder.Register();
}
catch (...)
{
    // Indicate an error was encountered.
}

```

## <a name="bringing-it-all-together"></a>모두 함께 가져오기

다음 코드 샘플에서는 COM winmain 백그라운드 작업을 실행 하 고 등록 하는 데 필요한 전체 코드를 보여 줍니다.

<details>
<summary>Winmain 앱 패키지 매니페스트 완료</summary>
<p>

```xml

<?xml version="1.0" encoding="utf-8"?>
<Package
  xmlns="http://schemas.microsoft.com/appx/manifest/foundation/windows10"
  xmlns:uap="http://schemas.microsoft.com/appx/manifest/uap/windows10"
  xmlns:rescap="http://schemas.microsoft.com/appx/manifest/foundation/windows10/restrictedcapabilities"
  xmlns:com="http://schemas.microsoft.com/appx/manifest/com/windows10"
  IgnorableNamespaces="uap rescap com">

  <Identity
    Name="SamplePackagedWinMainBackgroundApp"
    Publisher="CN=Contoso"
    Version="1.0.0.0" />

  <Properties>
    <DisplayName>SamplePackagedWinMainBackgroundApp</DisplayName>
    <PublisherDisplayName>Contoso</PublisherDisplayName>
    <Logo>Images\StoreLogo.png</Logo>
  </Properties>

  <Dependencies>
    <TargetDeviceFamily Name="Windows.Desktop" MinVersion="10.0.19041.0" MaxVersionTested="10.0.19041.0" />
  </Dependencies>

  <Resources>
    <Resource Language="x-generate"/>
  </Resources>

  <Applications>
    <Application Id="App"
                 Executable="SampleBackgroundApp\$targetnametoken$.exe"
                 EntryPoint="$targetentrypoint$">

      <uap:VisualElements
        DisplayName="SampleBackgroundApp"
        Description="SampleBackgroundApp"
        BackgroundColor="transparent"
        Square150x150Logo="Images\Square150x150Logo.png"
        Square44x44Logo="Images\Square44x44Logo.png">
        <uap:DefaultTile Wide310x150Logo="Images\Wide310x150Logo.png" />
        <uap:SplashScreen Image="Images\SplashScreen.png" />
      </uap:VisualElements>

      <Extensions>
        <com:Extension Category="windows.comServer">
          <com:ComServer>
            <com:ExeServer Executable="SampleBackgroundApp\SampleBackgroundApp.exe" DisplayName="SampleBackgroundApp" Arguments="-StartSampleTaskServer">
              <com:Class Id="14C5882B-35D3-41BE-86B2-5106269B97E6" DisplayName="Sample Task" />
            </com:ExeServer>
          </com:ComServer>
        </com:Extension>
      </Extensions>
    </Application>
  </Applications>

  <Capabilities>
  <rescap:Capability Name="runFullTrust" />
  </Capabilities>
</Package>

```

</p>
</details>


<details>
<summary>백그라운드 완료 작업 코드 샘플</summary>
<p>

```csharp

using System;
using System.IO; // Path
using System.Threading; // EventWaitHandle
using System.Collections.Generic; // Queue
using System.Runtime.InteropServices; // Guid, RegistrationServices
using Windows.ApplicationModel.Background; // IBackgroundTask

namespace PackagedWinMainBackgroundTaskSample
{
    // Background task implementation.
    // {14C5882B-35D3-41BE-86B2-5106269B97E6} is GUID to register this task with BackgroundTaskBuilder. Generate a random GUID before implementing.
    [ComVisible(true)]
    [ClassInterface(ClassInterfaceType.None)]
    [Guid("14C5882B-35D3-41BE-86B2-5106269B97E6")]
    [ComSourceInterfaces(typeof(IBackgroundTask))]
    public class SampleTask : IBackgroundTask
    {
        private volatile int cleanupTask; // flag used to indicate to Run method that it should exit
        private Queue<int> numbersQueue; // the data structure holding the set of primes in memory

        private const int maxPrimeNumber = 1000000000; // the number up to which task will attempt to calculate primes
        private const int queueDepthToWrite = 10; // how frequently this task should flush its queue of primes
        private const string numbersQueueFile = "numbersQueue.log"; // the file to write to relative to AppData

        public SampleTask()
        {
            cleanupTask = 0;
            numbersQueue = new Queue<int>(queueDepthToWrite);
        }

        /// <summary>
        /// This method writes all the numbers in the current queue to the specified file.
        /// </summary>
        private void FlushNumbersToFile(Queue<int> queueToWrite)
        {
            string logPath = Path.Combine(ApplicationData.Current.LocalFolder.Path,
                                        System.Diagnostics.Process.GetCurrentProcess().ProcessName);

            if (!Directory.Exists(logPath))
            {
                Directory.CreateDirectory(logPath);
            }

            logPath = Path.Combine(logPath, numbersQueueFile);

            const string delimiter = ", ";
            UnicodeEncoding unicodeEncoding = new UnicodeEncoding();
            // convert the queue to a list of comma separated values.
            string stringToWrite = String.Join(delimiter, queueToWrite);
            // Add the comma at the end.
            stringToWrite += delimiter;

            File.AppendAllText(logPath, stringToWrite);
        }

        /// <summary>
        /// This method determines if the specified number is a prime number.
        /// </summary>
        private bool IsPrimeNumber(int dividend)
        {
            bool isPrime = true;
            for (int divisor = dividend - 1; divisor > 1; divisor -= 1)
            {
                if ((dividend % divisor) == 0)
                {
                    isPrime = false;
                    break;
                }
            }

            return isPrime;
        }

        /// <summary>
        /// Given the current number, this method calculates the next prime number (excluding the specified number).
        /// </summary>
        private int GetNextPrime(int previousNumber)
        {
            int currentNumber = previousNumber + 1;
            while (!IsPrimeNumber(currentNumber))
            {
                currentNumber += 1;
            }

            return currentNumber;
        }

        /// <summary>
        /// This method is the main entry point for the background task. The system will believe this background task
        /// is complete when this method returns.
        /// </summary>
        [MTAThread]
        public void Run(IBackgroundTaskInstance taskInstance)
        {
            // Start with the first applicable number.
            int currentNumber = 1;

            taskDeferral = taskInstance.GetDeferral();

            // Wire the cancellation handler.
            taskInstance.Canceled += this.OnCanceled;

            // Set the progress to indicate this task has started
            taskInstance.Progress = 10;

            // Calculate primes until a cancellation has been requested or until
            // the maximum number is reached.
            while ((cleanupTask == 0) && (currentNumber < maxPrimeNumber)) {
                // Compute the next prime number and add it to our queue.
                currentNumber = GetNextPrime(currentNumber);
                numbersQueue.Enqueue(currentNumber);
                // Once the queue is filled to its max size, flush the numbers to the file.
                if (numbersQueue.Count >= queueDepthToWrite)
                {
                    FlushNumbersToFile(numbersQueue);
                    numbersQueue.Clear();
                }
            }

            // Flush any remaining numbers to the file as part of cleanup.
            FlushNumbersToFile(numbersQueue);

            if (taskDeferral != null)
            {
                taskDeferral.Complete();
            }
        }

        /// <summary>
        /// This method is signaled when the system requests the background task be canceled. This method will signal
        /// to the Run method to clean up and return.
        /// </summary>
        [MTAThread]
        public void OnCanceled(IBackgroundTaskInstance taskInstance, BackgroundTaskCancellationReason cancellationReason)
        {
            cleanupTask = 1;
        }
    }


    // COM server startup code.
    class SampleTaskServer
    {
        SampleTaskServer()
        {
            comRegistrationToken = 0;
            waitHandle = new EventWaitHandle(false, EventResetMode.AutoReset);
        }

        ~SampleTaskServer()
        {
            Stop();
        }

        public void Start()
        {
            RegistrationServices registrationServices = new RegistrationServices();
            comRegistrationToken = registrationServices.RegisterTypeForComClients(typeof(SampleTask), RegistrationClassContext.LocalServer, RegistrationConnectionType.MultipleUse);

            // Either have the background task signal this handle when it completes, or never signal this handle to keep this
            // process as the COM server until the process is closed.
            waitHandle.WaitOne();
        }

        public void Stop()
        {
            if (comRegistrationToken != 0)
            {
                RegistrationServices registrationServices = new RegistrationServices();
                registrationServices.UnregisterTypeForComClients(registrationCookie);
            }

            waitHandle.Set();
        }

        private int comRegistrationToken;
        private EventWaitHandle waitHandle;
    }


    // Background task registration code.
    class SampleTaskRegistrar
    {
        public static void Register()
        {
            var taskRegistered = false;
            var sampleTaskName = "SampleTask";

            foreach (var task in BackgroundTaskRegistration.AllTasks)
            {
                if (task.Value.Name == sampleTaskName)
                {
                    taskRegistered = true;
                    break;
                }
            }

            if (!taskRegistered)
            {
                var builder = new BackgroundTaskBuilder();

                builder.Name = sampleTaskName;
                builder.SetTaskEntryPointClsid(typeof(SampleTask).GUID);
                builder.SetTrigger(new TimeTrigger(15, false));
            }

            try
            {
                var task = builder.Register();
            }
            catch (...)
            {
                // Indicate an error was encountered.
            }
        }
    }


    // Application entry point.
    static class Program
    {
        [MTAThread]
        static void Main()
        {
            string[] commandLineArgs = Environment.GetCommandLineArgs();
            if (commandLineArgs.Length < 2)
            {
                // Open the WPF UI when no arguments are specified.
            }
            else
            {
                if (commandLineArgs.Contains("-RegisterSampleTask", StringComparer.InvariantCultureIgnoreCase))
                {
                    SampleTaskRegistrar.Register();
                }

                if (commandLineArgs.Contains("-StartSampleTaskServer", StringComparer.InvariantCultureIgnoreCase))
                {
                    var sampleTaskServer = new SampleTaskServer();
                    sampleTaskServer.Start();
                }
            }

            return;
        }
    }
}

```

```cppwinrt

#include <unknwn.h>
#include <winrt/Windows.Foundation.h>
#include <winrt/Windows.Foundation.Collections.h>
#include <winrt/Windows.ApplicationModel.Background.h>

using namespace winrt;
using namespace winrt::Windows::Foundation;
using namespace winrt::Windows::Foundation::Collections;
using namespace winrt::Windows::ApplicationModel::Background;

namespace PackagedWinMainBackgroundTaskSample
{
    // Background task implementation.
    // {14C5882B-35D3-41BE-86B2-5106269B97E6} is GUID to register this task with BackgroundTaskBuilder. Generate a random GUID before implementing.
    struct __declspec(uuid("14C5882B-35D3-41BE-86B2-5106269B97E6"))
    SampleTask : implements<SampleTask, IBackgroundTask>
    {
        const unsigned int maxPrimeNumber = 1000000000;
        volatile bool isCanceled = false;
        BackgroundTaskDeferral taskDeferral = nullptr;

        void __stdcall Run (_In_ IBackgroundTaskInstance taskInstance)
        {
            taskInstance.Canceled({ this, &SampleTask::OnCanceled });

            taskDeferral = taskInstance.GetDeferral();

            unsigned int currentPrimeNumber = 1;
            while (!isCanceled && (currentPrimeNumber < maxPrimeNumber))
            {
                currentPrimeNumber = GetNextPrime(currentPrimeNumber);
            }

            taskDeferral.Complete();
        }

        void __stdcall OnCanceled (_In_ IBackgroundTaskInstance, _In_ BackgroundTaskCancellationReason)
        {
            isCanceled = true;
        }
    };

    struct TaskFactory : implements<TaskFactory, IClassFactory>
    {
        HRESULT __stdcall CreateInstance (_In_opt_ IUnknown* aggregateInterface, _In_ REFIID interfaceId, _Outptr_ VOID** object) noexcept final
        {
            if (aggregateInterface != nullptr) {
                return CLASS_E_NOAGGREGATION;
            }

            return make<SampleTask>().as(interfaceId, object);
        }

        HRESULT __stdcall LockServer (BOOL) noexcept final
        {
            return S_OK;
        }
    };


    // COM server startup code.
    class SampleTaskServer
    {
    public:
        SampleTaskServer()
        {
            waitHandle = EventWaitHandle(false, EventResetMode::AutoResetEvent);
            comRegistrationToken = 0;
        }

        ~SampleTaskServer()
        {
            Stop();
        }

        void Start()
        {
            try
            {
                com_ptr<IClassFactory> taskFactory = make<TaskFactory>();

                winrt::check_hresult(CoRegisterClassObject(__uuidof(SampleTask),
                                                           taskFactory.get(),
                                                           CLSCTX_LOCAL_SERVER,
                                                           REGCLS_MULTIPLEUSE,
                                                           &comRegistrationToken));

                // Either have the background task signal this handle when it completes, or never signal this handle to
                // keep this process as the COM server until the process is closed.
                waitHandle.WaitOne();

            }
            catch (...)
            {
                // Indicate an error has been encountered.
            }
        }

        void Stop()
        {
            if (comRegistrationToken != 0)
            {
                CoRevokeClassObject(comRegistrationToken);
            }

            waitHandle.Set();
        }

    private:
        DWORD comRegistrationToken;
        EventWaitHandle waitHandle;
    };


    // Background task registration code.
    class SampleTaskRegistrar
    {
        public static void Register()
        {
            bool taskRegistered = false;
            std::wstring sampleTaskName = L"SampleTask";
            auto allTasks = BackgroundTaskRegistration::AllTasks();

            for (auto const& task : allTasks)
            {
                if (task.Value().Name() == sampleTaskName)
                {
                    taskRegistered = true;
                    break;
                }
            }

            if (!taskRegistered)
            {
                BackgroundTaskBuilder builder;

                builder.Name(sampleTaskName);
                builder.SetTaskEntryPointClsid(__uuidof(SampleTask));
                builder.SetTrigger(TimeTrigger(15, false));
            }

            try
            {
                auto task = builder.Register();
            }
            catch (...)
            {
                // Indicate an error was encountered.
            }
        }
    }

}

using namespace PackagedWinMainBackgroundTaskSample;

// Application entry point.
int wmain(_In_ int argc, _In_reads_(argc) const wchar** argv)
{
    unsigned int argumentIndex;

    winrt::init_apartment();

    if (argc <= 1)
    {
        return E_INVALIDARG;
    }

    for (argumentIndex = 0; argumentIndex < argc ; argumentIndex += 1)
    {
        if (_wcsnicmp(L"RegisterSampleTask",
                      argv[argumentIndex],
                      wcslen(L"RegisterSampleTask")) == 0)
        {
            SampleTaskRegistrar::Register();
        }

        if (_wcsnicmp(L"StartSampleTaskServer",
                      argv[argumentIndex],
                      wcslen(L"StartSampleTaskServer")) == 0)
        {
            SampleTaskServer sampleTaskServer;
            sampleTaskServer.Start();
        }
    }

    return S_OK;
}

```

</p>
</details>


## <a name="remarks"></a>설명

최신 대기에서 백그라운드 작업을 실행할 수 있는 UWP 앱과 달리, WinMain 앱은 최신 대기 상태의 낮은 전원 단계에서 코드를 실행할 수 없습니다. 자세히 알아보려면 [최신 대기](https://docs.microsoft.com/windows-hardware/design/device-experiences/modern-standby) 를 참조 하세요.

백그라운드 작업을 사용 하는 앱을 작성 하는 방법에 대 한 API 참조, 백그라운드 작업 개념 지침 및 자세한 지침은 다음 관련 항목을 참조 하세요.

## <a name="related-topics"></a>관련 항목

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
* [UWP 앱에서 일시 중단, 다시 시작 및 백그라운드 이벤트를 트리거하는 방법 (디버깅 시)](https://msdn.microsoft.com/library/windows/apps/hh974425(v=vs.110).aspx)

**백그라운드 작업 API 참조**

* [**Windows.ApplicationModel.Background**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Background)
