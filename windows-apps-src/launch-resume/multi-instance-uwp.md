---
title: 다중 인스턴스 유니버설 Windows 앱 만들기
description: 이 항목에서는 다중 인스턴스를 지원하는 UWP 앱을 작성하는 방법을 설명합니다.
keywords: 다중 인스턴스 uwp
ms.date: 09/21/2018
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 6cceac0cf4b9cc4c13c0e99ce5beffad70787256
ms.sourcegitcommit: 8921a9cc0dd3e5665345ae8eca7ab7aeb83ccc6f
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 12/11/2018
ms.locfileid: "8888414"
---
# <a name="create-a-multi-instance-universal-windows-app"></a>다중 인스턴스 유니버설 Windows 앱 만들기

이 항목에서는 다중 인스턴스 유니버설 Windows 플랫폼(UWP) 앱을 만드는 방법을 설명합니다.

Windows 10, 버전 1803 (10.0; 빌드 17134) 낮춘, UWP 앱에 여러 인스턴스를 지원 하도록에서 선택할 수 있습니다. 다중 인스턴스 UWP 앱의 인스턴스를 실행 중이며 후속 정품 인증을 요청하는 경우 플랫폼은 기존 인스턴스를 활성화하지 않습니다. 대신 다른 프로세스에서 실행 중인 인스턴스를 새로 만듭니다.

> [!IMPORTANT]
> 다중 인스턴스는 JavaScript 응용 프로그램에 대 한 지원 하지만 다중 인스턴스 리디렉션 되었습니다. 다중 인스턴스 리디렉션은 JavaScript 응용 프로그램에 대해 지원 되지 않으므로, [**AppInstance**](/uwp/api/windows.applicationmodel.appinstance) 클래스 이러한 응용 프로그램에 대 한 유용 하지 않습니다.

## <a name="opt-in-to-multi-instance-behavior"></a>다중 인스턴스 동작 옵트인

새로운 다중 인스턴스 응용 프로그램을 만드는 경우 [Visual Studio Marketplace](https://aka.ms/E2nzbv)에 있는 **Multi-Instance App Project Templates.VSIX**를 설치할 수 있습니다. 이 템플릿을 설치하면 **Visual C# > Windows 유니버설**(또는 **기타 언어 > Visual C++ > Windows 유니버설**)의 **새 프로젝트** 대화 상자에서 사용할 수 있게 됩니다.

두 템플릿이 설치됩니다. **다중 인스턴스 UWP 앱**은 다중 인스턴스 앱을 만드는 템플릿을 제공하고 **다중 인스턴스 리디렉션 UWP 앱**은 새 인스턴스를 시작하거나 이미 시작된 인스턴스를 선택적으로 활성화하기 위해 빌드할 수 있는 추가 논리를 제공합니다. 예를 들어 동일한 문서를 편집할 때 한 번에 한 인스턴스에서만 편집되게 하려는 경우 새 인스턴스를 시작하지 않고 해당 파일이 열려 있는 인스턴스를 포그라운드로 가져옵니다.

두 템플릿 추가 `SupportsMultipleInstances` 에 `package.appxmanifest` 파일입니다. 네임 스페이스 접두사를 참고 `desktop4` 및 `iot2`: 데스크톱을 대상으로 하는 유일한 프로젝트나 사물 인터넷 (IoT) 프로젝트 다중 인스턴스를 지원 합니다.

```xml
<Package
  ...
  xmlns:desktop4="http://schemas.microsoft.com/appx/manifest/desktop/windows10/4"
  xmlns:iot2="http://schemas.microsoft.com/appx/manifest/iot/windows10/2"  
  IgnorableNamespaces="uap mp desktop4 iot2">
  ...
  <Applications>
    <Application Id="App"
      ...
      desktop4:SupportsMultipleInstances="true"
      iot2:SupportsMultipleInstances="true">
      ...
    </Application>
  </Applications>
   ...
</Package>
```

## <a name="multi-instance-activation-redirection"></a>다중 인스턴스 활성화 리디렉션

 UWP 앱에 대한 다중 인스턴스는 단순히 앱의 여러 인스턴스를 시작할 수 있도록 하는 것 이상입니다. 앱의 새 인스턴스를 시작할지 또는 이미 실행 중인 인스턴스를 활성화할지 여부를 선택하려는 경우 사용자 지정할 수 있습니다. 예를 들어 다른 인스턴스에서 이미 편집하고 있는 파일을 편집하기 위해 앱이 시작된 경우 파일을 편집하고 있는 다른 인스턴스를 여는 대신 활성화를 해당 인스턴스로 리디렉션하려고 할 수 있습니다.

다중 인스턴스 UWP 앱 만들기에 대 한이 동영상을 시청 하 작업에 표시 합니다.

> [!VIDEO https://www.youtube.com/embed/clnnf4cigd0]

**다중 인스턴스 리디렉션 UWP 앱** 템플릿은 위에 표시된 대로 package.appxmanifest 파일에 `SupportsMultipleInstances`를 추가하고 `Main()` 함수가 포함된 프로젝트에 **Program.cs**(또는 C++ 버전의 템플릿을 사용하는 경우 **Program.cpp**)를 추가합니다. 활성화를 리디렉션하는 논리가 `Main` 함수로 들어 갑니다. **Program.cs** 템플릿은 다음과 같습니다.

[**AppInstance.RecommendedInstance**](/uwp/api/windows.applicationmodel.appinstance.recommendedinstance) 속성이 있을 경우이 정품 인증 요청에 대 한 셸 제공 기본 인스턴스를 나타냅니다 (또는 `null` 경우 없는). 셸에서 기본 설정을 제공 하는 경우 해당 인스턴스를 활성화 한 다음 리디렉션할 수 있습니다 또는 선택 하는 경우 무시할 수 있습니다.

``` csharp
public static class Program
{
    // This example code shows how you could implement the required Main method to
    // support multi-instance redirection. The minimum requirement is to call
    // Application.Start with a new App object. Beyond that, you may delete the
    // rest of the example code and replace it with your custom code if you wish.

    static void Main(string[] args)
    {
        // First, we'll get our activation event args, which are typically richer
        // than the incoming command-line args. We can use these in our app-defined
        // logic for generating the key for this instance.
        IActivatedEventArgs activatedArgs = AppInstance.GetActivatedEventArgs();

        // If the Windows shell indicates a recommended instance, then
        // the app can choose to redirect this activation to that instance instead.
        if (AppInstance.RecommendedInstance != null)
        {
            AppInstance.RecommendedInstance.RedirectActivationTo();
        }
        else
        {
            // Define a key for this instance, based on some app-specific logic.
            // If the key is always unique, then the app will never redirect.
            // If the key is always non-unique, then the app will always redirect
            // to the first instance. In practice, the app should produce a key
            // that is sometimes unique and sometimes not, depending on its own needs.
            string key = Guid.NewGuid().ToString(); // always unique.
                                                    //string key = "Some-App-Defined-Key"; // never unique.
            var instance = AppInstance.FindOrRegisterInstanceForKey(key);
            if (instance.IsCurrentInstance)
            {
                // If we successfully registered this instance, we can now just
                // go ahead and do normal XAML initialization.
                global::Windows.UI.Xaml.Application.Start((p) => new App());
            }
            else
            {
                // Some other instance has registered for this key, so we'll 
                // redirect this activation to that instance instead.
                instance.RedirectActivationTo();
            }
        }
    }
}
```

`Main()` 이 템플릿은 실행되는 첫 번째 항목입니다. [**OnLaunched**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.application#Windows_UI_Xaml_Application_OnLaunched_Windows_ApplicationModel_Activation_LaunchActivatedEventArgs_) 와 [**OnActivated**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.application#Windows_UI_Xaml_Application_OnActivated_Windows_ApplicationModel_Activation_IActivatedEventArgs_)하기 전에 실행 됩니다. 이를 사용하면 앱에서 다른 초기화 코드가 실행되기 전에 해당 인스턴스 또는 다른 인스턴스를 활성화할지 여부를 결정할 수 있습니다.

위의 코드는 응용 프로그램의 기존 인스턴스가 활성화되는지 또는 새 인스턴스가 활성화되는지 여부를 결정합니다. 키를 사용하여 활성화하려는 기존 인스턴스가 있는지 여부를 판별합니다. 예를 들어 [파일 활성화 처리](https://docs.microsoft.com/en-us/windows/uwp/launch-resume/handle-file-activation)를 위해 앱을 시작할 수 있는 경우 이 파일 이름을 키로 사용할 수 있습니다. 그런 다음 앱의 인스턴스가 해당 키로 이미 등록되어 있는지 확인하고 새 인스턴스를 여는 대신 활성화할 수 있습니다. 다음은 코드의 개념입니다. `var instance = AppInstance.FindOrRegisterInstanceForKey(key);`

키로 등록된 인스턴스를 찾으면 해당 인스턴스가 활성화됩니다. 키가 없는 경우 현재 인스턴스(현재 `Main`을 실행하고 있는 인스턴스)가 응용 프로그램 개체를 만들고 실행을 시작합니다.

## <a name="background-tasks-and-multi-instancing"></a>백그라운드 작업 및 다중 인스턴스

- Out-of-proc 백그라운드 작업은 다중 인스턴스를 지원합니다. 일반적으로 각각의 새 트리거는 백그라운드 작업의 새 인스턴스를 생성합니다(기술적으로 말하자면 동일한 호스트 프로세스에서 여러 백그라운드 작업이 실행될 수 있음). 그러나 백그라운드 작업에 대해 서로 다른 인스턴스가 만들어집니다.
- In-proc 백그라운드 작업은 다중 인스턴스를 지원하지 않습니다.
- 백그라운드 오디오 작업은 다중 인스턴스를 지원하지 않습니다.
- 앱이 백그라운드 작업을 등록할 때 일반적으로 해당 작업이 이미 등록되어 있는지를 먼저 확인한 후 작업을 삭제하고 다시 등록하거나 기존 등록을 유지하기 위해 어떤 작업도 하지 않습니다. 이는 다중 인스턴스 앱의 일반적인 동작입니다. 그러나 다중 인스턴스 앱은 인스턴스당 다른 백그라운드 작업 이름을 등록하도록 선택할 수 있습니다. 이렇게 하면 동일한 트리거에 대해 여러 개의 등록이 생성되고 트리거가 실행되면 여러 백그라운드 작업 인스턴스가 활성화됩니다.
- 앱 서비스는 모든 연결에 대해 별도의 앱 서비스 백그라운드 작업 인스턴스를 시작합니다. 다중 인스턴스 앱의 경우에는 변경되지 않습니다. 즉, 다중 인스턴스 앱의 각 인스턴스는 고유한 앱 서비스 백그라운드 작업 인스턴스를 가지게 됩니다. 

## <a name="additional-considerations"></a>추가 고려 사항

- 다중 인스턴스는 데스크톱 및 사물 인터넷(IoT) 프로젝트를 대상으로 하는 UWP 앱에서 지원됩니다.
- 경합 조건과 경합 문제가 발생하지 않도록 하려면 다중 인스턴스 앱이 여러 인스턴스 간에 공유할 수 있는 설정, 앱 로컬 저장소 및 기타 리소스(예: 사용자 파일, 데이터 저장소 등)에 대한 액세스를 파티션/동기화하는 단계를 수행해야 합니다. 뮤텍스, 세마포어, 이벤트 및 등에 등의 표준 동기화 메커니즘을 사용할 수 있습니다.
- 앱의 Package.appxmanifest 파일에 `SupportsMultipleInstances`가 있는 경우 확장에서 `SupportsMultipleInstances`를 선언할 필요가 없습니다. 
- 백그라운드 작업 또는 앱 서비스를 제외한 다른 확장에 `SupportsMultipleInstances`를 추가하고 확장을 호스트하는 앱이 Package.appxmanifest 파일에서 `SupportsMultipleInstances`를 선언하지 않는 경우 스키마 오류가 발생합니다.
- 앱은 여러 백그라운드 작업을 동일한 호스트로 그룹화 할 매니페스트에 [**ResourceGroup**](https://docs.microsoft.com/windows/uwp/launch-resume/declare-background-tasks-in-the-application-manifest) 선언을 사용할 수 있습니다. 이는 각 활성화가 별도의 호스트로 들어가는 다중 인스턴스와 충돌합니다. 따라서 앱은 매니페스트에서 `SupportsMultipleInstances`와 `ResourceGroup`을 둘 다 선언할 수 없습니다.

## <a name="sample"></a>샘플

다중 인스턴스 활성화 리디렉션의 예에 대 한 [다중 인스턴스 샘플](https://aka.ms/Kcrqst) 참조 하세요.

## <a name="see-also"></a>참고 항목

[AppInstance.FindOrRegisterInstanceForKey](https://docs.microsoft.com/uwp/api/windows.applicationmodel.appinstance#Windows_ApplicationModel_AppInstance_FindOrRegisterInstanceForKey_System_String_)
[AppInstance.GetActivatedEventArgs](https://docs.microsoft.com/uwp/api/windows.applicationmodel.appinstance#Windows_ApplicationModel_AppInstance_GetActivatedEventArgs)
[AppInstance.RedirectActivationTo](https://docs.microsoft.com/uwp/api/windows.applicationmodel.appinstance#Windows_ApplicationModel_AppInstance_RedirectActivationTo)
[앱 활성화 처리](https://docs.microsoft.com/windows/uwp/launch-resume/activate-an-app)