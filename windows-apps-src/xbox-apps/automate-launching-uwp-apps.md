---
title: Windows 10 UWP(유니버설 Windows 플랫폼) 앱 시작 자동화
description: 개발자는 프로토콜 활성화 및 시작 활성화를 사용 하 여 자동화 된 테스트를 위해 UWP 앱 또는 게임의 시작을 자동화할 수 있습니다.
ms.topic: article
ms.localizationpriority: medium
ms.date: 02/08/2017
ms.openlocfilehash: 661576ede9a940f7f8aa71715900306d2c2b28a9
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89154877"
---
# <a name="automate-launching-windows-10-uwp-apps"></a>Windows 10 UWP 앱 시작 자동화

## <a name="introduction"></a>소개

개발자는 UWP (유니버설 Windows 플랫폼 자동 시작) 앱을 얻기 위한 몇 가지 옵션을 사용할 수 있습니다. 이 문서에서는 프로토콜 활성화 및 시작 활성화를 사용 하 여 앱을 시작 하는 방법을 알아봅니다.

*프로토콜 활성화* 를 사용 하면 앱이 자신을 지정 된 프로토콜에 대 한 처리기로 등록할 수 있습니다. 

앱 타일에서 시작 하는 것과 같이 앱을 정상적 *으로 시작 하는 것입니다.*

각 활성화 방법을 사용 하는 경우 명령줄 또는 시작 관리자 응용 프로그램을 사용 하는 옵션이 있습니다. 모든 활성화 방법에 대해 앱이 현재 실행 중인 경우 활성화는 응용 프로그램을 포그라운드로 가져오고 (다시 활성화 됨) 새 활성화 인수를 제공 합니다. 이렇게 하면 유연 하 게 활성화 명령을 사용 하 여 앱에 새 메시지를 제공할 수 있습니다. 새로 업데이트 된 앱을 실행 하려면 활성화 방법에 대해 프로젝트를 컴파일하고 배포 해야 합니다. 

## <a name="protocol-activation"></a>프로토콜 활성화

앱에 대 한 프로토콜 활성화를 설정 하려면 다음 단계를 수행 합니다. 

1. Visual Studio에서 **appxmanifest.xml** 파일을 엽니다.
2. **선언** 탭을 선택 합니다.
3. **사용 가능한 선언** 드롭다운 아래에서 **프로토콜**을 선택 하 고 **추가**를 선택 합니다.
4. **속성**의 **이름** 필드에 앱을 시작 하는 고유한 이름을 입력 합니다. 

    ![프로토콜 활성화](images/automate-uwp-apps-1.png)

5. 파일을 저장 하 고 프로젝트를 배포 합니다. 
6. 프로젝트를 배포한 후에는 프로토콜 활성화를 설정 해야 합니다. 
7. **Control Panel\All Control Panel Items\Default** program로 이동 하 여 **파일 형식 또는 프로토콜을 특정 프로그램과 연결**을 선택 합니다. **프로토콜 섹션으로** 스크롤하여 프로토콜이 나열 되는지 확인 합니다. 

이제 프로토콜 활성화가 설정 되었으므로 프로토콜을 사용 하 여 앱을 활성화 하는 두 가지 옵션 (명령줄 또는 시작 관리자 응용 프로그램)이 있습니다. 

### <a name="command-line"></a>명령줄

명령줄을 사용 하 여 명령을 시작한 다음 이전에 설정한 프로토콜 이름, 콜론 (":") 및 매개 변수를 사용 하 여 앱을 프로토콜을 활성화할 수 있습니다. 매개 변수는 임의의 임의의 문자열일 수 있습니다. 그러나 URI (Uniform Resource Identifier) 기능을 활용 하려면 표준 URI 형식을 따르는 것이 좋습니다. 

  ```
  scheme://username:password@host:port/path.extension?query#fragment
  ```

Uri 개체에는 URI 문자열을이 형식으로 구문 분석 하는 메서드가 있습니다. 자세한 내용은 [Uri 클래스 (MSDN)](/uwp/api/windows.foundation.uri)를 참조 하세요. 

예:

  ```
  >start bingnews:
  >start myapplication:protocol-parameter
  >start myapplication://single-player/level3?godmode=1&ammo=200
  ```

프로토콜 명령줄 활성화는 원시 URI에서 2038 자까지 유니코드 문자 제한을 지원 합니다. 

### <a name="launcher-application"></a>시작 관리자 응용 프로그램

시작 하려면 WinRT API를 지 원하는 별도의 응용 프로그램을 만듭니다. 시작 프로그램에서 프로토콜 활성화를 사용 하 여 시작 하기 위한 c + + 코드는 다음 샘플에 나와 있습니다. 여기서 **Packageuri** 는 인수를 사용 하는 응용 프로그램의 uri입니다. 예를 들면 `myapplication:` 또는 `myapplication:protocol activation arguments` 입니다.

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
시작 관리자 응용 프로그램을 사용 하는 프로토콜 활성화는 명령줄에서 프로토콜 활성화와 같은 인수에 대 한 제한 사항이 동일 합니다. 는 모두 원시 URI에서 2038 자까지 유니코드 문자를 지원 합니다. 

## <a name="launch-activation"></a>활성화 시작

시작 활성화를 사용 하 여 앱을 시작할 수도 있습니다. 설치는 필요 하지 않지만 UWP 앱의 응용 프로그램 사용자 모델 ID (AUMID)가 필요 합니다. AUMID는 패키지 제품군 이름 뒤에 느낌표와 응용 프로그램 ID가 나옵니다. 

패키지 제품군 이름을 가져오는 가장 좋은 방법은 다음 단계를 완료 하는 것입니다.

1. **Appxmanifest.xml** 파일을 엽니다.
2. **패키징** 탭에서 **패키지 이름을**입력 합니다.

    ![활성화 시작](images/automate-uwp-apps-2.png)

3. **패키지 제품군 이름이** 나열 되지 않은 경우 PowerShell을 열고를 실행 `>get-appxpackage MyPackageName` 하 여 **PackageFamilyName**를 찾습니다.

응용 프로그램 ID는 요소 아래에 있는 **appxmanifest.xml** 파일 (XML 뷰로 열림)에서 찾을 수 있습니다. `<Applications>`

### <a name="command-line"></a>명령줄

UWP 앱의 시작 활성화를 수행 하는 도구는 Windows 10 SDK와 함께 설치 됩니다. 이 파일은 명령줄에서 실행할 수 있으며 응용 프로그램의 AUMID 인수로 시작 됩니다.

```
C:\Program Files (x86)\Windows Kits\10\App Certification Kit\microsoft.windows.softwarelogo.appxlauncher.exe <AUMID>
```

다음과 같이 표시 됩니다.

```
"C:\Program Files (x86)\Windows Kits\10\App Certification Kit\microsoft.windows.softwarelogo.appxlauncher.exe" MyPackageName_ph1m9x8skttmg!AppId
```

이 옵션은 명령줄 인수를 지원 하지 않습니다. 

### <a name="launcher-application"></a>시작 관리자 응용 프로그램

를 시작 하는 데 사용 하는 COM 사용을 지 원하는 별도의 응용 프로그램을 만들 수 있습니다. 다음 예제에서는 시작 관리자 프로그램에서 시작 활성화를 사용 하 여 시작 하는 c + + 코드를 보여 줍니다. 이 코드를 사용 하 여 **ApplicationActivationManager** 개체를 만들고 이전에 찾은 AUMID 인수를 전달 하는 **응용 프로그램** 을 호출할 수 있습니다. 다른 매개 변수에 대 한 자세한 내용은 [IApplicationActivationManager:: 응용 프로그램 메서드 (MSDN)](/windows/desktop/api/shobjidl_core/nf-shobjidl_core-iapplicationactivationmanager-activateapplication)를 참조 하세요.

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

이전 메서드 (즉, 명령줄 사용)와는 달리이 메서드는 전달 되는 인수를 지원 합니다.

## <a name="accepting-arguments"></a>인수 허용

UWP 앱 활성화 시 전달 된 인수를 허용 하려면 앱에 일부 코드를 추가 해야 합니다. 프로토콜 활성화 또는 활성화 시작이 발생 했는지 확인 하려면 **onactivated** 이벤트를 재정의 하 고 인수 형식을 확인 한 다음 원시 문자열 또는 Uri 개체의 미리 구문 분석 된 값을 가져옵니다. 

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

## <a name="summary"></a>요약
요약 하자면, 다양 한 방법을 사용 하 여 UWP 앱을 시작할 수 있습니다. 요구 사항 및 사용 사례에 따라 다른 방법이 다른 방법 보다 더 적합할 수 있습니다. 

## <a name="see-also"></a>참고 항목
- [Xbox One의 UWP](index.md)