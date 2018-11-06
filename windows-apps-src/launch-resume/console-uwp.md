---
author: TylerMSFT
title: 유니버설 Windows 플랫폼 콘솔 앱 만들기
description: 이 항목에서는 콘솔 창에서 실행되는 UWP 앱을 작성하는 방법을 설명합니다.
keywords: 콘솔 uwp
ms.author: twhitney
ms.date: 08/02/2018
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 7e5923176f5f9fff1a6e4f30a7ba2419e99c074b
ms.sourcegitcommit: e814a13978f33654d8e995584f4b047cb53e0aef
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/05/2018
ms.locfileid: "6041051"
---
# <a name="create-a-universal-windows-platform-console-app"></a>유니버설 Windows 플랫폼 콘솔 앱 만들기

이 항목을 만드는 방법을 설명에 [C + + WinRT](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt) 또는 C + + /CX 유니버설 Windows 플랫폼 (UWP) 콘솔 앱.

Windows 10, 버전 1803부터 다음을 작성할 수 있습니다 C + + /winrt 또는 C + + /CX UWP 콘솔 앱 DOS 또는 PowerShell 콘솔 창 등의 콘솔 창에서 실행 합니다. 콘솔 앱 콘솔 창을 사용 하 여 입력 및 출력 및 **printf** **getchar**등의 [유니버설 C 런타임](/cpp/c-runtime-library/reference/crt-alphabetical-function-reference) 기능을 사용할 수 있습니다. UWP 콘솔 앱을 Microsoft Store에 게시할 수 있습니다. UWP 콘솔 앱에는 앱 목록의 항목과 시작 메뉴에 고정할 수 있는 기본 타일이 있습니다. 명령줄에서 일반적으로 시작 됩니다 하지만 시작 메뉴에서 UWP 콘솔 앱을 실행할 수 있습니다.

작업에서 하나를 보려면 UWP 콘솔 앱 만들기에 대 한 비디오는 다음과 같습니다.

> [!VIDEO https://www.youtube.com/embed/bwvfrguY20s]

## <a name="use-a-uwp-console-app-template"></a>UWP 콘솔 앱 템플릿 사용 

UWP 콘솔 앱을 만들려면 먼저 [Visual Studio Marketplace](https://marketplace.visualstudio.com/items?itemName=AndrewWhitechapelMSFT.ConsoleAppUniversal)에 있는 **콘솔 앱(유니버설) 프로젝트 템플릿**을 설치하세요. **새 프로젝트**에서 사용할 수 있는 설치 된 템플릿 됩니다. > **설치 됨** > **기타 언어** > **Visual c + +** > **Windows 유니버설** 으로 **콘솔 앱 C + /winrt (유니버설 Windows) **및 **콘솔 앱 C + + /CX (유니버설 Windows)** 합니다.

## <a name="add-your-code-to-main"></a>main()에 코드 추가

템플릿은 `main()` 함수가 포함된 **Program.cpp**를 추가합니다. 이는 UWP 콘솔 앱에서 실행이 시작되는 위치입니다. `__argc` 및 `__argv` 매개 변수로 명령줄 인수에 액세스합니다. `main()`에서 제어가 반환되면 UWP 콘솔 앱이 종료됩니다.

다음 예제에서는 **Program.cpp** 의으로 추가 됩니다 합니다 **콘솔 앱 C + WinRT** 서식 파일:

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

UWP 콘솔 앱은 실행되는 디렉토리 및 하위 디렉토리에서 파일 시스템에 액세스할 수 있습니다. 이는 템플릿이 [AppExecutionAlias](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/element-uap5-appexecutionalias) 확장을 앱의 Package.appxmanifest 파일에 추가하기 때문에 가능합니다. 이 확장을 사용하면 사용자가 콘솔 창에서 앨리어스를 입력하여 앱을 시작할 수 있습니다. 앱이 시스템 경로에 있지 않아도 앱을 시작할 수 있습니다.

[파일 액세스 권한](https://docs.microsoft.com/windows/uwp/files/file-access-permissions)에서 설명된 대로 제한된 접근 권한 값 `broadFileSystemAccess`를 추가하여 파일 시스템에 대한 광범위한 액세스 권한을 UWP 콘솔 앱에 추가적으로 제공할 수 있습니다. 이 접근 권한 값은 [**Windows.Storage**](https://msdn.microsoft.com/library/windows/apps/BR227346) 네임스페이스의 API에 대해 작동합니다.

템플릿이 [SupportsMultipleInstances](multi-instance-uwp.md) 접근 권한 값을 앱의 Package.appxmanifest 파일에 추가하므로 여러 개의 UWP 콘솔 앱 인스턴스를 한 번에 실행할 수 있습니다.

또한 템플릿은 `Subsystem="console"` 접근 권한 값을 Package.appxmanifest 파일에 추가하여 이 UWP 앱이 콘솔 앱임을 표시합니다. `desktop4` 및 iot2 `namespace` 접두사를 참고하세요. UWP 콘솔 앱은 데스크톱 및 사물 인터넷(IoT) 프로젝트에서만 지원됩니다.

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

## <a name="additional-considerations-for-uwp-console-apps"></a>UWP 콘솔 앱을 위한 추가 고려 사항

- 만 C + + /winrt와 C + + /CX UWP 앱 콘솔 앱이 될 수 있습니다.
- UWP 콘솔 앱은 데스크톱 또는 IoT 프로젝트 유형을 대상으로 해야 합니다.
- UWP 콘솔 앱 창을 만들 수 없습니다. 사용자 동의 프롬프트 같은 만드는 messagebox (), 또는 Location(), 어떤 이유로 든 창을 만들 수 있는 다른 모든 API를 사용할 수 없습니다.
- UWP 콘솔 앱은 백그라운드 작업을 사용하지도 않고 백그라운드 작업으로 저장하지 않을 수도 있습니다.
- [명령줄 활성화](https://blogs.windows.com/buildingapps/2017/07/05/command-line-activation-universal-windows-apps/#5YJUzjBoXCL4MhAe.97)를 제외하고 UWP 콘솔 앱은 파일 연결, 프로토콜 연결 등의 활성화를 지원하지 않습니다.
- UWP 콘솔 앱은 다중 인스턴스를 지원하지만 [다중 인스턴스 리디렉션](multi-instance-uwp.md)은 지원하지 않습니다.
- UWP 앱에 사용할 수 있는 Win32 API 목록을 보려면 [UWP 앱용 Win32 및 COM API](https://docs.microsoft.com/uwp/win32-and-com/win32-and-com-for-uwp-apps)를 참조하세요.