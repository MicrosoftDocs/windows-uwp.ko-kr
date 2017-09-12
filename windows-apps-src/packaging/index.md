---
author: laurenhughes
ms.assetid: 1abcbb13-80f0-4bf1-a812-649ee8bd1915
title: "앱 패키징"
description: "이 섹션에는 UWP(유니버설 Windows 플랫폼) 앱 패키지에 대한 문서의 링크가 포함되어 있습니다."
ms.author: lahugh
ms.date: 08/09/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: "windows 10, uwp, 패키징"
ms.openlocfilehash: 0b4f8cf2e90a125539c3dade51f6b3db78219e3f
ms.sourcegitcommit: 63c815f8c6665872987b5410cabf324f2b7e3c7c
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/10/2017
---
# <a name="packaging-apps"></a>앱 패키징

\[ Windows 10의 UWP 앱에 맞게 업데이트되었습니다. Windows8.x 문서는 [보관](http://go.microsoft.com/fwlink/p/?linkid=619132)을 참조하세요. \]

## <a name="purpose"></a>목적

이 섹션에는 UWP(유니버설 Windows 플랫폼) 앱 패키지에 대한 문서의 링크가 포함되어 있습니다.

| 항목 | 설명 |
|-------|-------------|
| [Visual Studio를 사용하여 UWP 앱 패키징](packaging-uwp-apps.md) | UWP 앱을 배포 또는 판매하려면, 여기에 대한 앱 패키지를 만들어야 합니다. |
| [수동 앱 패키징](manual-packaging-root.md) | 앱 패키지를 만들어 서명하려 하지만 앱 개발에 Visual Studio를 사용하지 않은 경우, 수동 앱 패키징 도구를 사용해야 합니다. |
| [앱 패키지 아키텍처](device-architecture.md) | UWP 앱 패키지를 빌드할 때 사용해야 할 프로세스 아키텍처에 대해 알아봅니다. | 
| [UWP 앱 스트리밍 설치](streaming-install.md) | 유니버설 Windows 플랫폼(UWP) 앱 스트리밍 설치는 Windows 스토어에서 먼저 다운로드하고 싶은 앱의 부분을 지정할 수 있도록 해줍니다. 앱의 기본 파일이 먼저 다운로드되면, 백그라운드에서 나머지 부분이 완전히 다운로드되는 동안 사용자는 앱을 실행하고 상호 작용할 수 있습니다. |
| [선택적 패키지 및 관련 집합 제작](optional-packages.md) | 옵션 패키지에는 주 패키지에 통합할 수 있는 콘텐츠가 포함되어 있습니다. 다운로드 가능한 콘텐츠(DLC)에서 크기 제한을 위해 대형 앱을 분할하거나, 원래 앱과의 분리를 위해 추가 콘텐츠를 배송하는 경우에 유용합니다. | 
| [WinAppDeployCmd.exe 도구를 사용하여 앱 설치](install-universal-windows-apps-with-the-winappdeploycmd-tool.md) | Windows 응용 프로그램 배포(WinAppDeployCmd.exe)는 Windows10 컴퓨터에서 Windows10 Mobile 디바이스로 UWP 앱을 배포하는 데 사용할 수 있는 명령줄 도구입니다. Windows10 Mobile 디바이스가 Microsoft Visual Studio나 해당 앱에 대한 솔루션 없이 동일한 서브넷에서 사용 가능하거나 USB로 연결되면 이 도구를 사용하여 .appx 패키지를 배포할 수 있습니다. 이 문서는 이 도구를 사용하여 UWP 앱을 설치하는 방법을 설명합니다. |
| [UWP 앱에 대한 자동화된 빌드 설정](auto-build-package-uwp-apps.md) | 자동화된 빌드 프로세스의 일부로 앱을 패키징하려는 경우 이 항목에서 VSTS(Visual Studio Team Services)를 사용하여 수행하는 방법을 보여 줍니다. |
| [앱 접근 권한 값 선언](app-capability-declarations.md) | 사진, 음악 또는 디바이스(예: 카메라 또는 마이크)와 같은 특정 리소스 및 API에 액세스하려면 UWP 앱의 [패키지 매니페스트](https://msdn.microsoft.com/library/windows/apps/BR211474)에서 접근 권한 값을 선언해야 합니다. |
| [앱에 대한 패키지 업데이트 다운로드 및 설치](self-install-package-updates.md) | UWP 앱은 프로그래밍 방식으로 패키지 업데이트를 확인하고 업데이트를 설치할 수 있습니다. 앱은 Windows 개발자 센터 대시보드에 필수로 표시된 패키지를 쿼리하고 필수 업데이트가 설치될 때까지 기능을 사용하지 않도록 할 수 있습니다. 이 문서에서는 이러한 작업을 수행하는 방법을 설명합니다. |