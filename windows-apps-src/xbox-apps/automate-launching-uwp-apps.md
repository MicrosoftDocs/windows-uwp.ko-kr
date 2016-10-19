---
title: "Windows 10 UWP(유니버설 Windows 플랫폼) 앱 시작 자동화"
description: "개발자는 프로토콜 활성화 및 시작 활성화를 사용하여 자동화 테스트를 위해 해당 UWP 앱 또는 게임을 자동으로 시작할 수 있습니다."
author: listurm
translationtype: Human Translation
ms.sourcegitcommit: c5d0f685f4c733cbe4ba4c07aab565b888ddfe58
ms.openlocfilehash: 4b31ec06b1ded4882d26cffed029eb8179ff47c3

---

# Windows 10 UWP 앱 시작 자동화

## 소개

개발자는 UWP(유니버설 Windows 플랫폼) 앱의 자동 시작을 실현하기 위해 몇 가지 옵션을 사용할 수 있습니다. 이 문서에서는 프로토콜 활성화 및 시작 활성화를 사용하여 앱을 시작하는 방법을 알아봅니다.

*프로토콜 활성화*을 사용하면 앱이 자신을 지정된 프로토콜에 대한 처리기로 등록할 수 있습니다. 

*시작 활성화*는 앱 타일에서 시작하는 것과 같은 앱의 일반적인 시작 방법입니다.

각 활성화 방법에서 명령줄 또는 시작 관리자 응용 프로그램 중 하나를 선택하여 사용할 수 있습니다. 모든 활성화 방법에서, 앱이 현재 실행되고 있는 경우 활성화를 수행하면 앱이 포그라운드로 나오고(다시 활성화) 새로운 활성화 인수가 제공됩니다. 따라서 활성화 명령을 유연하게 사용하여 앱에 새 메시지를 제공할 수 있습니다. 활성화 방법에서 새로 업데이트된 앱을 실행하려면 프로젝트를 컴파일한 후 배포해야 합니다. 

## 프로토콜 활성화

다음 단계에 따라 앱에 대한 프로토콜 활성화를 설정합니다. 

1. Visual Studio에서 **Package.appxmanifest** 파일을 엽니다.
2. **선언** 탭을 선택합니다.
3. **사용 가능한 선언** 드롭다운 목록에서 **프로토콜**을 선택한 후 **추가**를 선택합니다.
4. **속성** 아래의 **이름** 필드에 앱을 실행하기 위한 고유한 이름을 입력합니다. 

    ![프로토콜 활성화](images/automate-uwp-apps-1.png)

5. 파일을 저장하고 프로젝트를 배포합니다. 
6. 프로젝트가 배포된 후에 프로토콜 활성화를 설정해야 합니다. 
7. **제어판\모든 제어판 항목\기본 프로그램**으로 이동한 후 **파일 형식 또는 프로토콜을 특정 프로그램과 연결**을 선택합니다. **프로토콜** 섹션으로 스크롤하여 해당 프로토콜이 목록에 표시되는지 확인합니다. 

프로토콜 활성화를 설정했으므로 이 프로토콜을 사용하여 앱을 활성화하기 위한 두 가지 옵션(명령줄 또는 시작 관리자 응용 프로그램) 중 하나를 사용할 수 있습니다. 

### 명령줄

앱은 명령줄에 start 명령, 이전에 설정한 프로토콜 이름, 콜론(“:”) 및 매개 변수를 차례로 사용하여 프로토콜을 통해 앱을 활성화할 수 있습니다. 매개 변수는 임의의 문자열일 수 있지만 URI(Uniform Resource Identifier) 기능을 활용하려면 표준 URI 형식에 따르는 것이 좋습니다. 

  ```
  scheme://username:password@host:port/path.extension?query#fragment
  ```

Uri 개체에는 다음 형식으로 URI 문자열을 구문 분석하는 메서드가 있습니다. 자세한 내용은 [URI 클래스(MSDN)](https://msdn.microsoft.com/library/windows/apps/windows.foundation.uri.aspx)를 참조하세요. 

예제:

  ```
  >start bingnews:
  >start myapplication:protocol-parameter
  >start myapplication://single-player/level3?godmode=1&ammo=200
  ```

프로토콜 명령줄 활성화에서는 유니코드 문자가 지원되며 원시 URI의 경우 2038자의 제한이 적용됩니다. 

### 시작 관리자 응용 프로그램

시작하는 경우 WinRT API를 지원하는 별도의 응용 프로그램을 만듭니다. 시작 관리자 프로그램에서 프로토콜 활성화로 시작하기 위한 C++ 코드는 다음 샘플에 나와 있습니다. 여기서 **PackageURI**는 인수가 있는 응용 프로그램의 URI(예: `myapplication:` 또는 `myapplication:protocol activation arguments`)입니다.

```
bool ProtocolLaunchURI(Platform::String^ URI)
{
       IAsyncOperation<bool>^ protocolLaunchAsyncOp;
       try
       {
              protocolLaunchAsyncOp = Windows::System::Launcher::LaunchUriAsync(ref new 
Uri(URI));
       }
       catch (Platform::Exception^ e)
       {
              Platform::String^ dbgStr = "ProtocolLaunchURI Exception Thrown: " 
+ e->ToString() + "\n";
              OutputDebugString(dbgStr->Data());
              return false;
       }

       concurrency::create_task(protocolLaunchAsyncOp).wait();

       if (protocolLaunchAsyncOp->Status == AsyncStatus::Completed)
       {
              bool LaunchResult = protocolLaunchAsyncOp->GetResults();
              Platform::String^ dbgStr = "ProtocolLaunchURI " + URI 
+ " completed. Launch result " + LaunchResult + "\n";
              OutputDebugString(dbgStr->Data());
              return LaunchResult;
       }
       else
       {
              Platform::String^ dbgStr = "ProtocolLaunchURI " + URI + " failed. Status:" 
+ protocolLaunchAsyncOp->Status.ToString() + " ErrorCode:" 
+ protocolLaunchAsyncOp->ErrorCode.ToString() + "\n";
              OutputDebugString(dbgStr->Data());
              return false;
       }
}
```
시작 관리자 응용 프로그램을 사용한 프로토콜 활성화는 인수에 대해 명령줄을 사용한 프로토콜 활성화의 경우와 동일한 제한이 적용됩니다. 둘 다 유니코드 문자를 지원하며, 원시 URI에 대해 2038자의 제한이 적용됩니다. 

## 시작 활성화

시작 활성화를 사용하여 앱을 시작할 수도 있습니다. 필요한 설정은 없지만 UWP 앱의 AUMID(응용 프로그램 사용자 모델 ID)가 필요합니다. AUMID는 패키지 제품군 이름, 느낌표 및 응용 프로그램 ID로 구성됩니다. 

패키지 패밀리 이름을 가져오는 가장 좋은 방법은 다음 단계를 완료하는 것입니다.

1. **Package.appxmanifest** 파일을 엽니다.
2. **패키징** 탭에서 **패키지 이름**을 입력합니다.

    ![시작 활성화](images/automate-uwp-apps-2.png)

3. **패키지 제품군 이름**이 나열되지 않으면 PowerShell을 열고 `>get-appxpackage MyPackageName`을(를) 실행하여 **PackageFamilyName**을 찾습니다.

응용 프로그램 ID는 XML 뷰에 열려 있는 **Package.appxmanifest** 파일의 `<Applications>` 요소 아래에서 볼 수 있습니다.

### 명령줄

UWP 앱의 시작 활성화를 수행하기 위한 도구는 Windows 10 SDK와 함께 설치됩니다. 이 도구를 명령줄에서 실행할 수도 있고 시작할 앱의 AUMID가 인수로 사용될 수도 있습니다.

```
C:\Program Files (x86)\Windows Kits\10\App Certification Kit\microsoft.windows.softwarelogo.appxlauncher.exe <AUMID>
```

이 도구는 다음과 같습니다.

```
"C:\Program Files (x86)\Windows Kits\10\App Certification Kit\microsoft.windows.softwarelogo.appxlauncher.exe" MyPackageName_ph1m9x8skttmg!AppId
```

이 옵션은 명령줄 인수를 지원하지 않습니다. 

### 시작 관리자 응용 프로그램

시작하는 데 COM을 사용하도록 지원하는 별도의 응용 프로그램을 만들 수 있습니다. 다음 예제에서는 시작 관리자 프로그램에서 시작 활성화로 시작하기 위한 C++ 코드를 보여 줍니다. 이 코드를 사용하여 **ApplicationActivationManager** 개체를 만들고, 앞에서 찾은 AUMID 및 인수를 전달하여 **ActivateApplication**을 호출합니다. 다른 매개 변수에 대한 자세한 내용은 [IApplicationActivationManager::ActivateApplication 메서드(MSDN)](https://msdn.microsoft.com/library/windows/desktop/hh706903(v=vs.85).aspx)를 참조하세요.

```
#include <ShObjIdl.h>
#include <atlbase.h>

HRESULT LaunchApp(LPCWSTR AUMID)
{
     HRESULT hr = CoInitializeEx(nullptr, COINIT_APARTMENTTHREADED);
     if (FAILED(hr))
     {
            wprintf(L"LaunchApp %s: Failed to init COM. hr = 0x%08lx \n", AUMID, hr);
     }
     {
            CComPtr<IApplicationActivationManager> AppActivationMgr = nullptr;
            if (SUCCEEDED(hr))
            {
                   hr = CoCreateInstance(CLSID_ApplicationActivationManager, nullptr,  
CLSCTX_LOCAL_SERVER, IID_PPV_ARGS(&AppActivationMgr));
                   if (FAILED(hr))
                   {
                         wprintf(L"LaunchApp %s: Failed to create Application Activation 
Manager. hr = 0x%08lx \n", AUMID, hr);
                   }
            }
            if (SUCCEEDED(hr))
            {
                   DWORD pid = 0;
                   hr = AppActivationMgr->ActivateApplication(AUMID, nullptr, AO_NONE, 
&pid);
                   if (FAILED(hr))
                   {
                         wprintf(L"LaunchApp %s: Failed to Activate App. hr = 0x%08lx 
\n", AUMID, hr);
                   }
            }
     }
     CoUninitialize();
     return hr;
}
```

앞에 나오는 시작 방법(명령줄 사용)과 달리, 이 메서드는 전달되는 인수를 지원합니다.

## 인수 수락

UWP 앱의 활성화 시 전달된 인수를 수락하려면 앱에 일부 코드를 추가해야 합니다. 프로토콜 활성화 또는 시작 활성화가 발생했는지 확인하려면 **OnActivated** 이벤트를 재정의하고 인수 형식을 확인한 후 원시 문자열 또는 URI 개체의 사전 분석된 값을 가져옵니다. 

이 예제에서는 원시 문자열을 가져오는 방법을 보여 줍니다.

```
void OnActivated(IActivatedEventArgs^ args)
{
        // Check for launch activation
        if (args->Kind == ActivationKind::Launch)
        {
            auto launchArgs = static_cast<LaunchActivatedEventArgs^>(args); 
Platform::String^ argval = launchArgs->Arguments;
            // Manipulate arguments …
        }

        // Check for protocol activation
        if (args->Kind == ActivationKind::Protocol)
        {
            auto protocolArgs = static_cast< ProtocolActivatedEventArgs^>(args);
            Platform::String^ argval = protocolArgs->Uri->ToString();
            // Manipulate arguments …
        }
    }
```

## 요약
다양한 방법으로 UWP 앱을 시작할 수 있습니다. 요구 사항 및 사용 사례에 따라 좀 더 적절한 방법을 사용하는 것이 좋습니다. 

## 참고 항목
- [Xbox One의 UWP](index.md)




<!--HONumber=Aug16_HO3-->


