---
description: PRI(패키지 리소스 인덱싱) API를 사용하여 UWP 앱의 리소스에 대한 사용자 지정 빌드 시스템을 개발할 수 있습니다. 빌드 시스템은 UWP 앱에 필요한 복잡도 수준에 상관없이 PRI 파일을 만들고 버전화하고 덤프할 수 있습니다.
title: 패키지 리소스 인덱싱 및 사용자 지정 빌드 시스템
template: detail.hbs
ms.date: 05/07/2018
ms.topic: article
keywords: Windows 10, UWP, 리소스, 이미지, 자산, MRT, 한정자
ms.localizationpriority: medium
ms.openlocfilehash: 84b6a05927e2106df136741a262c3af5ef08a575
ms.sourcegitcommit: a3bbd3dd13be5d2f8a2793717adf4276840ee17d
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/30/2020
ms.locfileid: "93031666"
---
# <a name="package-resource-indexing-pri-apis-and-custom-build-systems"></a>PRI(패키지 리소스 인덱싱) API 및 사용자 지정 빌드 시스템
[PRI(패키지 리소스 인덱싱) API](/windows/desktop/menurc/pri-indexing-reference)를 사용하여 UWP 앱의 리소스에 대한 사용자 지정 빌드 시스템을 개발할 수 있습니다. 빌드 시스템은 UWP 앱에 필요한 복잡도 수준에 관계없이 PRI(패키지 리소스 인덱스) 파일을 만들고, 버전 관리하고, 덤프할 수 있습니다(XML로). 현재 MakePri.exe 명령줄 도구를 사용 하는 사용자 지정 빌드 시스템이 있는 경우 ( [MakePri.exe를 사용 하 여 리소스를 수동으로 컴파일 ](makepri-exe-command-options.md)참조) 향상 된 성능 및 제어를 위해 MakePri.exe를 호출 하는 대신 PRI api 호출로 전환 하는 것이 좋습니다.

PRI Api는 Windows 10 버전 1803에 대 한 Windows SDK에서 도입 되었습니다. Api는 Win32 Windows Api 형식을 사용 합니다. 즉,이 api를 호출 하는 몇 가지 옵션이 있습니다. Win32 앱에서 직접 호출할 수도 있고 .NET 앱 또는 UWP 앱에서 [플랫폼 호출](/dotnet/framework/interop/consuming-unmanaged-dll-functions?branch=live) 을 통해 호출할 수도 있습니다.

이 항목의 시나리오에서는 Win32 Visual C++ Windows 콘솔 응용 프로그램 프로젝트에서 PRI Api를 호출 하는 방법을 보여 줍니다. 배경 정보는 [리소스 관리 시스템](resource-management-system.md)을 참조 하세요.

> [!NOTE]
> 사용자 지정 빌드 시스템 앱을 Microsoft Store에 제출 하지 않을 수 있기 때문에이 주의 사항은 문제가 될 가능성이 낮습니다. 그러나 UWP 앱 형식으로 사용자 지정 빌드 시스템을 개발 하는 옵션을 선택 하는 경우에는 Microsoft Store에 제출할 수 없다는 이상한 UWP 앱이 됩니다. 이는 플랫폼 호출을 사용 하는 UWP 앱이 인증 Microsoft Store 실패 하기 때문입니다. 이 경우 플랫폼 호출은 *사용자 지정 빌드 시스템에만 존재* 합니다. 배송 UWP 앱에 *있지 않습니다* (PRI 파일을 작성 하는 앱).

## <a name="scenario-walkthroughs"></a>시나리오 연습
|항목|설명|
|-|-|
|[시나리오 1: 문자열 리소스 및 자산 파일에서 PRI 파일 생성](pri-apis-scenario-1.md)|이 시나리오에서는 사용자 지정 빌드 시스템을 나타내는 새 앱을 만듭니다. 리소스 인덱서를 만들고 문자열 및 다른 종류의 리소스를 추가 합니다. 그런 다음, PRI 파일을 생성 하 고 덤프 합니다.|

## <a name="important-apis"></a>중요 API
* [PRI (패키지 리소스 인덱싱) 참조](/windows/desktop/menurc/pri-indexing-reference)

## <a name="related-topics"></a>관련 항목
* [MakePri.exe를 사용하여 수동으로 리소스 컴파일](makepri-exe-command-options.md)
* [관리되지 않는 DLL 함수 사용](/dotnet/framework/interop/consuming-unmanaged-dll-functions?branch=live)
* [리소스 관리 시스템](resource-management-system.md)
