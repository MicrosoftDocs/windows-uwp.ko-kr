---
title: 다중 인스턴스 유니버설 Windows 앱 만들기
description: 이 항목에서는 다중 인스턴스를 지 원하는 UWP 앱을 작성 하는 방법에 대해 설명 합니다.
keywords: 다중 인스턴스 uwp
ms.date: 09/21/2018
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 8e19425222459bb3bd56a5fe406ec76c0b7fb6b9
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89164777"
---
# <a name="create-a-multi-instance-universal-windows-app"></a>다중 인스턴스 유니버설 Windows 앱 만들기

이 항목에서는 UWP (다중 인스턴스 유니버설 Windows 플랫폼) 앱을 만드는 방법에 대해 설명 합니다.

Windows 10, 버전 1803 (10.0; 빌드 17134) 부터는 UWP 앱에서 여러 인스턴스를 지원 하도록 옵트인 (opt in) 할 수 있습니다. 다중 인스턴스 UWP 앱의 인스턴스를 실행 중이며 후속 정품 인증을 요청하는 경우 플랫폼은 기존 인스턴스를 활성화하지 않습니다. 대신 다른 프로세스에서 실행 중인 인스턴스를 새로 만듭니다.

> [!IMPORTANT]
> 다중 인스턴스는 JavaScript 응용 프로그램에 대해 지원 되지만 다중 인스턴스 리디렉션은 지원 하지 않습니다. JavaScript 응용 프로그램에는 다중 인스턴스 리디렉션이 지원 되지 않으므로 [**Appinstance**](/uwp/api/windows.applicationmodel.appinstance) 클래스는 이러한 응용 프로그램에는 유용 하지 않습니다.

## <a name="opt-in-to-multi-instance-behavior"></a>다중 인스턴스 동작 옵트인 (Opt in)

새 다중 인스턴스 응용 프로그램을 만드는 경우 [Visual Studio Marketplace](https://marketplace.visualstudio.com/)에서 사용할 수 있는 [다중 인스턴스 앱 프로젝트 템플릿. VSIX](https://marketplace.visualstudio.com/items?itemName=AndrewWhitechapelMSFT.MultiInstanceApps)를 설치할 수 있습니다. 템플릿을 설치한 후에는 Visual c #의 **새 프로젝트** 대화 상자에서 **Visual c # > windows 유니버설** (또는 **다른 언어 > Visual C++ > windows 유니버설**)에서 사용할 수 있습니다.

두 개의 템플릿이 설치 됩니다. 다중 인스턴스 **UWP**앱은 다중 인스턴스 앱을 만들기 위한 템플릿을 제공 하 고 다중 **인스턴스 리디렉션 uwp 앱**은 새 인스턴스를 시작 하거나 이미 시작 된 인스턴스를 선택적으로 활성화 하기 위해 작성할 수 있는 추가 논리를 제공 합니다. 예를 들어 한 번에 하나의 인스턴스만 동일한 문서를 편집 하므로 새 인스턴스를 시작 하는 대신 해당 파일을 포함 하는 인스턴스를 전경으로 전환 합니다.

두 템플릿 모두 `SupportsMultipleInstances` 파일에 추가 `package.appxmanifest` 됩니다. 네임 스페이스 접두사 `desktop4` 및 `iot2` : 데스크톱 또는 IoT (사물 인터넷) 프로젝트를 대상으로 하는 프로젝트만 다중 인스턴스를 지원 합니다.

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

 UWP 앱에 대 한 다중 인스턴스 지원은 단순히 앱의 여러 인스턴스를 시작할 수 있도록 하는 것 외에도 마찬가지입니다. 앱의 새 인스턴스를 시작할지 또는 이미 실행 중인 인스턴스를 활성화할지를 선택 하려는 경우 사용자 지정할 수 있습니다. 예를 들어 다른 인스턴스에서 이미 편집 중인 파일을 편집 하기 위해 앱을 시작 하는 경우 이미 파일을 편집 하 고 있는 다른 인스턴스를 열지 않고 활성화를 해당 인스턴스로 리디렉션할 수 있습니다.

작동 하는지 확인 하려면 다중 인스턴스 UWP 앱 만들기에 대 한이 비디오를 시청 하세요.

> [!VIDEO https://www.youtube.com/embed/clnnf4cigd0]

**다중 인스턴스 리디렉션 UWP 앱** 템플릿은 `SupportsMultipleInstances` 위와 같이 appxmanifest.xml 파일에 추가 하 고, 함수를 포함 하는 프로젝트에 **Program.cs** (또는 c + + 버전의 경우)를 추가 **합니다.** `Main()` 활성화 리디렉션 논리는 함수에 전달 `Main` 됩니다. **Program.cs** 에 대 한 템플릿은 다음과 같습니다.

[**RecommendedInstance**](/uwp/api/windows.applicationmodel.appinstance.recommendedinstance) 속성은이 활성화 요청에 대해 셸을 제공 하는 기본 설정 인스턴스 (있는 경우)를 나타냅니다 (있는 경우 `null` ). 셸에서 기본 설정을 제공 하는 경우 활성화를 해당 인스턴스로 리디렉션할 수 있으며, 선택 하는 경우 무시 해도 됩니다.

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

`Main()` 가장 먼저를 실행 합니다. [**Onlaunched**](/uwp/api/windows.ui.xaml.application#Windows_UI_Xaml_Application_OnLaunched_Windows_ApplicationModel_Activation_LaunchActivatedEventArgs_) 및 [**onlaunched**](/uwp/api/windows.ui.xaml.application#Windows_UI_Xaml_Application_OnActivated_Windows_ApplicationModel_Activation_IActivatedEventArgs_)되기 전에 실행 됩니다. 이를 통해 앱의 다른 초기화 코드를 실행 하기 전에이를 활성화할지, 아니면 다른 인스턴스를 활성화할지를 결정할 수 있습니다.

위의 코드는 응용 프로그램의 기존 인스턴스 또는 새 인스턴스가 활성화 되는지 여부를 확인 합니다. 키는 활성화 하려는 기존 인스턴스가 있는지 여부를 확인 하는 데 사용 됩니다. 예를 들어 [파일 활성화를 처리](./handle-file-activation.md)하기 위해 앱을 시작할 수 있는 경우 파일 이름을 키로 사용할 수 있습니다. 그런 다음 앱의 인스턴스가 해당 키에 이미 등록 되어 있는지 확인 하 고 새 인스턴스를 여는 대신 활성화할 수 있습니다. 다음은 코드의 개념입니다. `var instance = AppInstance.FindOrRegisterInstanceForKey(key);`

키를 사용 하 여 등록 된 인스턴스를 찾으면 해당 인스턴스가 활성화 됩니다. 키를 찾을 수 없는 경우 현재 인스턴스 (현재 실행 중인 인스턴스 `Main` )는 응용 프로그램 개체를 만들고 실행을 시작 합니다.

## <a name="background-tasks-and-multi-instancing"></a>백그라운드 작업 및 다중 인스턴스

- 프로시저 외부 백그라운드 작업에서 다중 인스턴스를 지원 합니다. 일반적으로 각 새 트리거는 백그라운드 작업의 새 인스턴스를 생성 합니다 (기술적으로 여러 백그라운드 작업을 동일한 호스트 프로세스에서 실행할 수 있는 경우에도). 그럼에도 불구 하 고 백그라운드 작업의 다른 인스턴스가 만들어집니다.
- In-proc 백그라운드 작업은 다중 인스턴스를 지원 하지 않습니다.
- 배경 오디오 작업은 다중 인스턴스를 지원 하지 않습니다.
- 앱이 백그라운드 작업을 등록 하는 경우 일반적으로 먼저 태스크가 이미 등록 되어 있는지 확인 한 다음 다시 등록 하거나 다시 등록 하거나 기존 등록을 유지 하기 위해 아무 것도 수행 하지 않습니다. 이는 여전히 다중 인스턴스 앱에서 일반적인 동작입니다. 그러나 다중 인스턴스 앱은 인스턴스당 다른 백그라운드 작업 이름을 등록 하도록 선택할 수 있습니다. 이렇게 하면 동일한 트리거에 대 한 여러 등록이 발생 하 고 트리거가 실행 될 때 여러 백그라운드 태스크 인스턴스가 활성화 됩니다.
- 앱 서비스는 모든 연결에 대해 별도의 app service 백그라운드 작업 인스턴스를 시작 합니다. 다중 인스턴스 앱에 대해서는 변경 되지 않은 상태로 유지 됩니다. 즉, 다중 인스턴스 앱의 각 인스턴스는 자신의 앱 서비스 백그라운드 작업 인스턴스를 가져옵니다. 

## <a name="additional-considerations"></a>기타 고려 사항

- 다중 인스턴스는 데스크톱 및 IoT (사물 인터넷) 프로젝트를 대상으로 하는 UWP 앱에서 지원 됩니다.
- 경합 상태 및 경합 문제를 방지 하기 위해 다중 인스턴스 앱은 여러 인스턴스 간에 공유할 수 있는 설정, 앱 로컬 저장소 및 기타 리소스 (예: 사용자 파일, 데이터 저장소 등)에 대 한 액세스를 분할/동기화 하는 단계를 수행 해야 합니다. 뮤텍스, 세마포, 이벤트 등의 표준 동기화 메커니즘을 사용할 수 있습니다.
- 앱이 `SupportsMultipleInstances` appxmanifest.xml 파일에 있으면 해당 확장에서를 선언할 필요가 없습니다 `SupportsMultipleInstances` . 
- `SupportsMultipleInstances`백그라운드 작업 또는 앱 서비스와는 별도로 다른 확장에를 추가 하는 경우 확장을 호스트 하는 앱이 appxmanifest.xml 파일에도 선언 하지 않으면 `SupportsMultipleInstances` 스키마 오류가 생성 됩니다.
- 앱은 매니페스트에서 [**ResourceGroup**](./declare-background-tasks-in-the-application-manifest.md) 선언을 사용 하 여 여러 백그라운드 작업을 동일한 호스트로 그룹화 할 수 있습니다. 이는 각 활성화가 별도의 호스트로 이동 하는 다중 인스턴스를 사용 하는 경우와 충돌 합니다. 따라서 앱은 매니페스트에서 및를 모두 선언할 수 없습니다 `SupportsMultipleInstances` `ResourceGroup` .

## <a name="sample"></a>예제

다중 인스턴스 활성화 리디렉션의 예는 [다중 인스턴스 샘플](https://github.com/Microsoft/AppModelSamples/tree/master/Samples/BananaEdit) 을 참조 하세요.

## <a name="see-also"></a>참고 항목

[Appinstance. FindOrRegisterInstanceForKey](/uwp/api/windows.applicationmodel.appinstance#Windows_ApplicationModel_AppInstance_FindOrRegisterInstanceForKey_System_String_) 
 [Appinstance. GetActivatedEventArgs](/uwp/api/windows.applicationmodel.appinstance#Windows_ApplicationModel_AppInstance_GetActivatedEventArgs) 
 [Appinstance. RedirectActivationTo](/uwp/api/windows.applicationmodel.appinstance#Windows_ApplicationModel_AppInstance_RedirectActivationTo) 
 [앱 활성화 처리](./activate-an-app.md)