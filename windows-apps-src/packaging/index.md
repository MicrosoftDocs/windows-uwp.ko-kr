---
ms.assetid: 1abcbb13-80f0-4bf1-a812-649ee8bd1915
title: 앱 패키징
description: 이 섹션에는 UWP(유니버설 Windows 플랫폼) 앱 패키지에 대한 문서의 링크가 포함되어 있습니다.
ms.date: 07/22/2019
ms.topic: article
keywords: windows 10, uwp, 패키징
ms.localizationpriority: medium
ms.openlocfilehash: 35adf8db66bbfaa1be11c0b389efea88b5c2437b
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89164366"
---
# <a name="packaging-apps"></a>앱 패키징

이 섹션에는 UWP(유니버설 Windows 플랫폼) 앱을 배포하고 설치할 수 있도록 MSIX 및 .appx 앱 패키지에 패키징하는 방법에 대한 문서 링크가 포함되어 있습니다. 일부 링크는 [MSIX 설명서](/windows/msix/)의 관련 문서로 이동합니다.

> [!NOTE]
> Windows 10에서 UWP 앱의 원래 앱 패키징 형식은 .appx였습니다. Windows 10 버전 1809부터 이 패키징 형식의 이름이 .msix로 바뀌었으며 .NET 및 C++/Win32 데스크톱 앱을 비롯한 모든 유형의 Windows 앱을 지원하도록 확장되었습니다. MSIX 지원도 Windows 이전 버전으로 확장 중입니다. 자세한 내용은 [MSIX 설명서](/windows/msix/)를 참조하세요.

| 항목 | 설명 |
|-------|-------------|
| [Visual Studio를 사용하여 UWP 앱 패키징](/windows/msix/package/packaging-uwp-apps) | UWP 앱을 배포 또는 판매하려면 그에 대한 앱 패키지를 만들어야 합니다. |
| [수동 앱 패키징](/windows/msix/package/manual-packaging-root) | 앱 패키지를 만들어 서명하려 하지만 앱 개발에 Visual Studio를 사용하지 않은 경우 수동 앱 패키징 도구를 사용해야 합니다. |
| [앱 패키지 아키텍처](/windows/msix/package/device-architecture) | 앱 패키지를 빌드할 때 사용해야 할 프로세스 아키텍처에 대해 자세히 알아봅니다. |
| [UWP 앱 스트리밍 설치](/windows/msix/package/streaming-install) | 앱 스트리밍을 설치하면 Microsoft Store에서 가장 먼저 다운로드하고 싶은 앱의 부분을 지정할 수 있습니다. 앱의 기본 파일이 먼저 다운로드되면, 백그라운드에서 나머지 부분이 완전히 다운로드되는 동안 사용자는 앱을 실행하고 상호 작용할 수 있습니다. |
| [선택형 패키지 및 관련 세트 제작](/windows/msix/package/optional-packages) | 선택형 패키지에는 주 패키지에 통합할 수 있는 콘텐츠가 포함되어 있습니다. 다운로드 가능한 콘텐츠(DLC)에서 크기 제한을 위해 대형 앱을 분할하거나, 원래 앱과의 분리를 위해 추가 콘텐츠를 배송하는 경우에 유용합니다. |
| [실행 가능한 코드가 있는 옵션 패키지](/windows/msix/package/optional-packages-with-executable-code) | Visual Studio를 사용하여 실행 코드로 선택적 패키지를 사용하는 방법을 알아보세요. |
| [앱 설치 관리자를 사용하여 Windows 10 앱 설치](/windows/msix/app-installer/app-installer-root) | 앱 설치 관리자를 사용하면 앱 패키지를 두 번 클릭하여 Windows 10 앱을 설치할 수 있습니다. |
| [WinAppDeployCmd.exe 도구를 사용하여 앱 설치](install-universal-windows-apps-with-the-winappdeploycmd-tool.md) | Windows 애플리케이션 배포(WinAppDeployCmd.exe)는 Windows 10 머신의 UWP 앱을 Windows 10 Mobile 디바이스에 배포하는 데 사용할 수 있는 명령줄 도구입니다. Windows 10 Mobile 디바이스가 Microsoft Visual Studio나 해당 앱에 대한 솔루션 없이 동일한 서브넷에서 사용 가능하거나 USB로 연결되면 이 도구를 사용하여 .appx 패키지를 배포할 수 있습니다. 이 문서는 이 도구를 사용하여 UWP 앱을 설치하는 방법을 설명합니다. |
| [UWP 앱에 대한 자동화된 빌드 설정](auto-build-package-uwp-apps.md) | 자동화된 빌드 프로세스의 일부로 앱을 패키징하려는 경우 이 항목에서 VSTS(Visual Studio Team Services)를 사용하여 수행하는 방법을 보여 줍니다. |
| [앱 기능 선언](app-capability-declarations.md) | 사진, 음악 또는 디바이스(예: 카메라 또는 마이크)와 같은 특정 API 또는 리소스에 액세스하려면 앱의 [패키지 매니페스트](/uwp/schemas/appxpackage/appx-package-manifest)에서 접근 권한 값을 선언해야 합니다. |
| [Microsoft Store에서 패키지 업데이트 다운로드 및 설치](self-install-package-updates.md) | UWP 앱은 프로그래밍 방식으로 패키지 업데이트를 확인하고 업데이트를 설치할 수 있습니다. 또한 앱은 파트너 센터에 필수로 표시된 패키지를 쿼리할 수 있으며, 필수 업데이트가 설치될 때까지 기능을 사용하지 않도록 설정할 수 있습니다.  |