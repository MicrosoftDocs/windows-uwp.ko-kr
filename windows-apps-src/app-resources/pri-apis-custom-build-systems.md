---
Description: With the package resource indexing (PRI) APIs, you can develop a custom build system for your UWP app's resources. The build system will be able to create, version, and dump PRI files to whatever level of complexity your UWP app needs.
title: 패키지 리소스 인덱싱(PRI) API 및 사용자 지정 빌드 시스템
template: detail.hbs
ms.date: 05/07/2018
ms.topic: article
keywords: Windows 10, uwp, 리소스, 이미지, 자산, MRT, 한정자
ms.localizationpriority: medium
ms.openlocfilehash: 617812415d3dcd00ec24d5f55971ae311265b61d
ms.sourcegitcommit: 8921a9cc0dd3e5665345ae8eca7ab7aeb83ccc6f
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 12/10/2018
ms.locfileid: "8874856"
---
# <a name="package-resource-indexing-pri-apis-and-custom-build-systems"></a>패키지 리소스 인덱싱(PRI) API 및 사용자 지정 빌드 시스템
[패키지 리소스 인덱싱(PRI) API](https://msdn.microsoft.com/library/windows/desktop/mt845690)를 사용하여 UWP 앱 리소스에 대한 사용자 지정 빌드 시스템을 개발할 수 있습니다. 빌드 시스템은 UWP 앱에 필요한 복잡도 수준에 상관없이 PRI(패키지 리소스 인덱스) 파일을 만들고 버전화하고 덤프할 수 있습니다(XML로). 현재 MakePri.exe 명령줄 도구를 사용하는 사용자 지정 빌드 시스템이 있는 경우([MakePri.exe를 사용하여 수동으로 리소스 컴파일](makepri-exe-command-options.md)), 성능을 높이고 제어권을 강화하기 위해 MakePri.exe를 호출하는 대신 PRI API를 호출하는 것이 좋습니다.

PRI API는 Windows 10 버전 1803용 Windows SDK에서 도입되었습니다. API는 Win32 Windows API의 형식을 취합니다. 이는 호출 방법이 여러 가지가 있음을 의미합니다. Win32 앱에서 바로 호출할 수도 있고, .NET 앱이나 UWP 앱으로부터도 [플랫폼 호출](/dotnet/framework/interop/consuming-unmanaged-dll-functions?branch=live)을 통해 호출할 수도 있습니다.

이 항목의 시나리오에서는 Win32 Visual C++ Windows 콘솔 응용 프로그램 프로젝트에서 PRI API를 호출하는 방법을 보여줍니다. 배경 정보에 대한 내용은 [리소스 관리 시스템](resource-management-system.md)을 참조하세요.

PRI 파일의 크기 제한은 64킬로바이트입니다.

> [!NOTE]
> 보통 사용자 지정 빌드 시스템 앱을 Microsoft Store에 제출하지는 않기 때문에 이 주의 사항은 크게 문제가 되지 않습니다. 하지만 사용자 지정 빌드 시스템을 UWP 앱 형식으로 개발하는 옵션을 선택한 경우, 이를 Microsoft Store에 제출할 수 없다는 점에서 흔치 않은 UWP 앱이 됩니다. 플랫폼 호출을 사용하는 UWP 앱이 Microsoft Store 인증에 실패하기 때문입니다. 이 경우 플랫폼 호출이*사용자 지정 빌드 시스템에만 존재*하며, 배송 UWP 앱(PRI 파일을 빌드하고 있는 대상)에는 존재하지 *않습니다*.

## <a name="scenario-walkthroughs"></a>시나리오 연습
|항목|설명|
|-|-|
|[시나리오 1: 문자열 리소스와 자산 파일에서 PRI 파일 생성](pri-apis-scenario-1.md)|이 시나리오에서는 사용자 지정 빌드 시스템을 표시하는 새로운 앱을 만듭니다. 리소스 인덱서를 만들고 문자열 및 다른 종류의 리소스를 여기에 추가합니다. 그런 다음 PRI 파일을 생성하고 덤프합니다.|

## <a name="important-apis"></a>중요 API
* [패키지 리소스 인덱싱(PRI) 참조](https://msdn.microsoft.com/library/windows/desktop/mt845690)

## <a name="related-topics"></a>관련 항목
* [MakePri.exe를 사용하여 수동으로 리소스 컴파일](makepri-exe-command-options.md)
* [관리되지 않는 DLL 기능 사용](/dotnet/framework/interop/consuming-unmanaged-dll-functions?branch=live)
* [리소스 관리 시스템](resource-management-system.md)
