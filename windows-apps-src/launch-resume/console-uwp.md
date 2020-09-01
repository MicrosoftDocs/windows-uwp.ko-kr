---
title: 유니버설 Windows 플랫폼 콘솔 앱 만들기
description: 이 항목에서는 콘솔 창에서 실행 되는 UWP 앱을 작성 하는 방법에 대해 설명 합니다.
keywords: 콘솔 uwp
ms.date: 08/02/2018
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: cab52a84e631404fc4cbd682d6cedef7e799d387
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89162667"
---
# <a name="create-a-universal-windows-platform-console-app"></a>유니버설 Windows 플랫폼 콘솔 앱 만들기

이 항목에서는 [c + +/Winrt](../cpp-and-winrt-apis/intro-to-using-cpp-with-winrt.md) 또는 UWP (c + +/cx 유니버설 Windows 플랫폼) 콘솔 앱을 만드는 방법에 대해 설명 합니다.

Windows 10 버전 1803부터 DOS 또는 PowerShell 콘솔 창과 같은 콘솔 창에서 실행 되는 c + +/WinRT 또는 c + +/CX UWP 콘솔 앱을 작성할 수 있습니다. 콘솔 앱은 입력 및 출력을 위해 콘솔 창을 사용 하며 **printf** 및 **Getchar**와 같은 [유니버설 C 런타임](/cpp/c-runtime-library/reference/crt-alphabetical-function-reference) 함수를 사용할 수 있습니다. UWP 콘솔 앱을 Microsoft Store에 게시할 수 있습니다. 앱 목록에 항목이 있고 시작 메뉴에 고정 될 수 있는 기본 타일이 있습니다. UWP 콘솔 앱은 시작 메뉴에서 시작할 수 있지만 일반적으로 명령줄에서 시작 합니다.

실행 중인 작업을 보려면 UWP 콘솔 앱을 만드는 방법에 대 한 비디오를 참조 하세요.

> [!VIDEO https://www.youtube.com/embed/bwvfrguY20s]

## <a name="use-a-uwp-console-app-template"></a>UWP 콘솔 앱 템플릿 사용 

UWP 콘솔 앱을 만들려면 먼저 [Visual Studio Marketplace](https://marketplace.visualstudio.com/items?itemName=AndrewWhitechapelMSFT.ConsoleAppUniversal)에서 사용할 수 있는 **콘솔 앱 (유니버설) 프로젝트 템플릿을**설치 합니다. 그러면 설치 된 템플릿이 **새 프로젝트**에  >  **설치**된  >  **다른 언어로**제공 됩니다  >  **Visual C++**  >  .**Windows 유니버설** 을 **콘솔 앱 c + +/winrt (유니버설 windows)** 및 **콘솔 앱 c + +/cx (유니버설 windows)** Visual C++ 합니다.

## <a name="add-your-code-to-main"></a>Main ()에 코드를 추가 합니다.

템플릿은 함수를 포함 하는 Program을 추가 합니다 **.** `main()` 이 경우 UWP 콘솔 앱에서 실행이 시작 됩니다. 및 매개 변수를 사용 하 여 명령줄 인수에 액세스 `__argc` `__argv` 합니다. 에서 제어가 반환 될 때 UWP 콘솔 앱이 종료 `main()` 됩니다.

다음 예제는 **콘솔 앱 c + +/WinRT** 템플릿에서 **추가 됩니다.**

```cppwinrt
#include "pch.h"

using namespace winrt;

// This example code shows how you could implement the required main function
// for a Console UWP Application. You can replace all the code inside main
// with your own custom code.

int __cdecl main()
{
    // You can get parsed command-line arguments from the CRT globals.
    wprintf(L"Parsed command-line arguments:\n");
    for (int i = 0; i < __argc; i++)
    {
        wprintf(L"__argv[%d] = %S\n", i, __argv[i]);
    }

    // Keep the console window alive in case you want to see console output when running from within Visual Studio
      wprintf(L"Press 'Enter' to continue: ");
    getchar();
}
```

## <a name="uwp-console-app-behavior"></a>UWP 콘솔 앱 동작

UWP 콘솔 앱은 실행 중인 디렉터리에서 파일 시스템에 액세스할 수 있습니다. 이는 템플릿이 앱의 appxmanifest.xml 파일에 [Appexecutionalias](/uwp/schemas/appxpackage/uapmanifestschema/element-uap5-appexecutionalias) 확장을 추가 하기 때문에 가능 합니다. 이 확장을 사용 하면 사용자가 콘솔 창에서 별칭을 입력 하 여 앱을 시작할 수도 있습니다. 앱이 시작 하기 위해 시스템 경로에 있을 필요는 없습니다.

또한 `broadFileSystemAccess` [파일 액세스 권한](../files/file-access-permissions.md)에 설명 된 대로 제한 된 기능을 추가 하 여 파일 시스템에 대 한 광범위 한 액세스를 UWP 콘솔 앱에 제공할 수 있습니다. 이 기능은 [**Windows. Storage**](/uwp/api/Windows.Storage) 네임 스페이스의 api와 함께 작동 합니다.

템플릿이 앱의 appxmanifest.xml 파일에 [SupportsMultipleInstances](multi-instance-uwp.md) 기능을 추가 하기 때문에 UWP 콘솔 앱의 인스턴스를 한 번에 하나만 실행할 수 있습니다.

또한 `Subsystem="console"` 이 템플릿은이 UWP 앱이 콘솔 앱 임을 나타내는 appxmanifest.xml 파일에 기능을 추가 합니다. `desktop4`및 iot2 접두사를 확인 `namespace` 합니다. UWP 콘솔 앱은 데스크톱 및 IoT (사물 인터넷) 프로젝트 에서만 지원 됩니다.

```xml
<Package
  ...
  xmlns:desktop4="http://schemas.microsoft.com/appx/manifest/desktop/windows10/4" 
  xmlns:iot2="http://schemas.microsoft.com/appx/manifest/iot/windows10/2" 
  IgnorableNamespaces="uap mp uap5 desktop4 iot2">
  ...
  <Applications>
    <Application Id="App"
      ...
      desktop4:Subsystem="console" 
      desktop4:SupportsMultipleInstances="true" 
      iot2:Subsystem="console" 
      iot2:SupportsMultipleInstances="true"  >
      ...
      <Extensions>
          <uap5:Extension 
            Category="windows.appExecutionAlias" 
            Executable="YourApp.exe" 
            EntryPoint="YourApp.App">
            <uap5:AppExecutionAlias desktop4:Subsystem="console">
              <uap5:ExecutionAlias Alias="YourApp.exe" />
            </uap5:AppExecutionAlias>
          </uap5:Extension>
      </Extensions>
    </Application>
  </Applications>
    ...
</Package>
```

## <a name="additional-considerations-for-uwp-console-apps"></a>UWP 콘솔 앱에 대 한 추가 고려 사항

- C + +/WinRT 및 c + +/CX UWP 앱만 콘솔 앱이 될 수 있습니다.
- UWP 콘솔 앱은 데스크톱 또는 IoT 프로젝트 유형을 대상으로 해야 합니다.
- UWP 콘솔 앱은 창을 만들지 못할 수 있습니다. MessageBox (), 위치 () 또는 사용자 동의 프롬프트와 같은 어떤 이유로 든 창을 만들 수 있는 다른 API는 사용할 수 없습니다.
- UWP 콘솔 앱은 백그라운드 작업을 사용 하거나 백그라운드 작업으로 사용할 수 없습니다.
- [명령줄 활성화](https://blogs.windows.com/buildingapps/2017/07/05/command-line-activation-universal-windows-apps/#5YJUzjBoXCL4MhAe.97)를 제외 하 고 UWP 콘솔 앱은 파일 연결, 프로토콜 연결 등의 활성화 계약을 지원 하지 않습니다.
- UWP 콘솔 앱은 다중 인스턴스를 지원 하지만 [다중 인스턴스 리디렉션을](multi-instance-uwp.md) 지원 하지 않습니다.
- UWP 앱에서 사용할 수 있는 Win32 Api 목록은 [uwp 앱 용 win32 및 COM api](/uwp/win32-and-com/win32-and-com-for-uwp-apps) 를 참조 하세요.