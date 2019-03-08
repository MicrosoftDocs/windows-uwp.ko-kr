---
title: Xbox Live Api 이진 UWP 프로젝트에 추가
description: Xbox Live Api 이진 패키지 UWP 프로젝트를 추가 하려면 NuGet을 사용 하는 방법에 알아봅니다.
ms.assetid: 1e77ce9f-8a0e-402c-9f46-e37f9cda90ed
ms.date: 11/28/2017
ms.topic: article
keywords: xbox live, xbox, 게임, uwp, windows 10, 하나는 xbox, nuget
ms.localizationpriority: medium
ms.openlocfilehash: 20b4d4ae27282ccf71964d31da4d1f577c280de8
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57593948"
---
# <a name="add-xbox-live-apis-binary-package-to-your-uwp-project"></a>Xbox Live Api 이진 패키지를 UWP 프로젝트에 추가

## <a name="requirements"></a>요구 사항

2. **[Windows 10](https://microsoft.com/windows)**.
3. **[Visual Studio](https://www.visualstudio.com/)**. Visual Studio 2015 업데이트 3 이상에 UWP 앱을 빌드할 수 있습니다. 개발자와 보안 업데이트에 대 한 Visual Studio의 최신 릴리스를 사용 하는 것이 좋습니다.
4. **[Windows 10 SDK](https://developer.microsoft.com/windows/downloads/windows-10-sdk) v10.0.10586.0** 이상.

## <a name="add-the-binary-package-via-nuget"></a>NuGet 통해 이진 패키지 추가

프로젝트에서 Xbox Live API를 사용 하려면 NuGet 패키지를 사용 하거나 API 소스를 추가 하거나 이진 파일에 대 한 참조를 추가할 수 있습니다. NuGet 패키지를 추가하면 쉽게 컴파일하고 소스를 추가하면 쉽게 디버깅할 수 있습니다. 이 문서에서는 NuGet 패키지를 사용 하 여 안내 합니다. 원본을 사용 하려는 경우을 참조 하세요 [Xbox Live Api 소스에서 Your UWP 프로젝트를 컴파일](add-xbox-live-apis-source-to-a-uwp-project.md)합니다.

UWP와 XDK, 및 c + + 및 WinRT Xbox 서비스 API는 버전으로 제공 하 고 해당 네임 스페이스도 구조화 **Microsoft.Xbox.Live.SDK.* 합니다. UWP** 고 **Microsoft.Xbox.Live.SDK.* 합니다. XboxOneXDK**합니다.

1. **UWP** PC, Xbox One 또는 Windows Phone 실행할 수 있는 UWP 게임을 작성 중인 개발자입니다.
2. **XboxOneXDK** 용인지 ID@Xbox 및 Xbox One XDK 사용 하는 개발자를 관리 합니다.
3. WinRT SDK로 작업 하는 경우 c + + 게임 엔진에 대 한 c + + SDK를 사용할 수 있습니다는 c + +로 작성 된 게임 엔진에 대 한 C#, 또는 JavaScript입니다.
4. WinRT는 c + + 엔진을 사용 하 여 사용 하는 경우 사용 해야 C + + /cli CX hats (^)를 사용 합니다. C + +는 c + + 게임 엔진을 사용 하도록 권장 되는 API입니다.  

> [!TIP]
> 자세한 내용은 UWP에서 Xbox One에서 실행 하는 방법에 대 한 [Xbox One에서 UWP 앱 개발을 시작 하기](https://docs.microsoft.com/windows/uwp/xbox-apps/getting-started).

Xbox Live SDK NuGet 패키지에서 추가할 수 있습니다.

1. Visual Studio로 이동 **도구가** > **NuGet 패키지 관리자** > **솔루션용 NuGet 패키지 관리...** .
2. NuGet 패키지 관리자에서 클릭 **찾아보기** enter **Xbox.Live.SDK** 검색 상자에 있습니다.
3. 왼쪽 목록에서 사용 하려는 Xbox Live SDK의 버전을 선택 합니다.
3. 창의 오른쪽에서 프로젝트 옆의 확인란을 클릭 **설치**합니다.

> [!NOTE]
> Xbox Live 크리에이터 스 프로그램 개발자는 XDK 지원 되지 Xbox Live SDK는 UWP 버전 중 하나 사용 해야 합니다.

![NuGet 통해 XBL 추가](../images/getting_started/vs-add-nuget-xbl.gif)

> [!IMPORTANT]
> 에 대 한 `Microsoft.Xbox.Live.SDK.Cpp.*` 기반된 프로젝트 헤더를 포함 해야 `#include <xsapi\services.h>` 프로젝트의 원본에 있습니다.
