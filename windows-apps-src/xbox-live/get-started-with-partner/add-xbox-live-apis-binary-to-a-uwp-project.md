---
title: Xbox Live Api 이진 UWP 프로젝트 추가
description: NuGet을 사용 하 여 UWP 프로젝트에 Xbox Live Api 이진 패키지를 추가 하는 방법을 알아봅니다.
ms.assetid: 1e77ce9f-8a0e-402c-9f46-e37f9cda90ed
ms.date: 11/28/2017
ms.topic: article
keywords: xbox live, xbox, 게임, uwp, windows 10, 하나는 xbox, nuget
ms.localizationpriority: medium
ms.openlocfilehash: 20b4d4ae27282ccf71964d31da4d1f577c280de8
ms.sourcegitcommit: 8921a9cc0dd3e5665345ae8eca7ab7aeb83ccc6f
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 12/11/2018
ms.locfileid: "8892024"
---
# <a name="add-xbox-live-apis-binary-package-to-your-uwp-project"></a>UWP 프로젝트에 Xbox Live Api 이진 패키지 추가

## <a name="requirements"></a>요구 사항

2. **[Windows 10](https://microsoft.com/windows)** 입니다.
3. **[Visual Studio](https://www.visualstudio.com/)** 합니다. Visual Studio 2015 업데이트 3 이상 UWP 앱을 빌드할 수 있습니다. 개발자 및 보안 업데이트에 대 한 Visual Studio의 최신 버전을 사용 하는 것이 좋습니다.
4. ** [Windows 10 SDK](https://developer.microsoft.com/windows/downloads/windows-10-sdk) v10.0.10586.0** 이상.

## <a name="add-the-binary-package-via-nuget"></a>NuGet 통해 이진 패키지 추가

프로젝트에서 Xbox Live API를 사용 하려면 NuGet 패키지를 사용 하거나 API 소스 추가 하거나 이진 파일에 대 한 참조를 추가할 수 있습니다. NuGet 패키지를 추가하면 쉽게 컴파일하고 소스를 추가하면 쉽게 디버깅할 수 있습니다. 이 문서에서는 NuGet 패키지를 사용 하 여 안내 합니다. 소스를 사용 하려는 경우 다음 참조 [는 Xbox Live Api 소스 UWP 프로젝트의 컴파일](add-xbox-live-apis-source-to-a-uwp-project.md)합니다.

Xbox 서비스 API c + + 및 WinRT 및 UWP 및 XDK, 모두에 대 한 특성은 있고 해당 네임 스페이스 **Microsoft.Xbox.Live.SDK.*로 구성 됩니다. UWP** 및 **Microsoft.Xbox.Live.SDK.* 합니다. XboxOneXDK**.

1. **UWP** 는 PC, Xbox One 또는 Windows Phone 실행할 수 있는 UWP 게임을 빌드하는 개발자를 위한.
2. **XboxOneXDK** 는 ID@Xbox Xbox One XDK를 사용 하는 개발자를 관리 합니다.
3. WinRT SDK로 작업 하는 경우 c + + 게임 엔진에 사용할 수는 c + + SDK c + +, C# 또는 JavaScript로 작성 된 게임 엔진입니다.
4. C + + 엔진을 사용 하 여 WinRT을 사용할 때 사용 해야 C + + CX hat (^)을 사용 합니다. C + +는 c + + 게임 엔진에 사용 하도록 권장된 API입니다.  

> [!TIP]
> 자세한 내용은 [Xbox One에서 UWP 앱 개발 시작](https://docs.microsoft.com/windows/uwp/xbox-apps/getting-started)에서 Xbox One의 UWP를 실행 하는 방법에 대 한 합니다.

하 여 Xbox Live SDK NuGet 패키지를 추가할 수 있습니다.

1. Visual Studio에서 **도구**를 이동 > **NuGet 패키지 관리자** > **... 솔루션용 NuGet 패키지 관리**합니다.
2. NuGet 패키지 관리자에서 **찾아** 클릭 하 고 **Xbox.Live.SDK** 검색 상자에 입력 합니다.
3. 왼쪽의 목록에서 사용할 수 있는 Xbox Live sdk 버전을 선택 합니다.
3. 창의 오른쪽에서 프로젝트 옆의 확인란을 선택 하 고 **설치**를 클릭 합니다.

> [!NOTE]
> Xbox Live 크리에이터 스 프로그램 개발자 XDK는 지원 되지 Xbox Live SDK의 UWP 버전 중 하나를 사용 해야 합니다.

![NuGet 통해 XBL 추가](../images/getting_started/vs-add-nuget-xbl.gif)

> [!IMPORTANT]
> 에 대 한 `Microsoft.Xbox.Live.SDK.Cpp.*` 를 기반으로 프로젝트를 헤더를 포함 해야 `#include <xsapi\services.h>` 에서 프로젝트의 소스입니다.
