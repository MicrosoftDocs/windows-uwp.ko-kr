---
author: laurenhughes
ms.assetid: 1abcbb13-80f0-4bf1-a812-649ee8bd1915
title: "앱 패키징"
description: "이 섹션에는 UWP(유니버설 Windows 플랫폼) 앱 패키지에 대한 문서의 링크가 포함되어 있습니다."
translationtype: Human Translation
ms.sourcegitcommit: 28cd2b2a922a20e0b9ffc4d1ca65f6a55e92aa8f
ms.openlocfilehash: 8aa4bfc8004b4d0739fe09e6e49e66de372569c4

---
# <a name="packaging-apps"></a>앱 패키징

\[ Windows 10의 UWP 앱에 맞게 업데이트되었습니다. Windows 8.x 문서는 [보관](http://go.microsoft.com/fwlink/p/?linkid=619132)을 참조하세요. \]

## <a name="purpose"></a>목적

이 섹션에는 UWP(유니버설 Windows 플랫폼) 앱 패키지에 대한 문서의 링크가 포함되어 있습니다.

| 항목 | 설명 |
|-------|-------------|
| [UWP 앱 패키징](packaging-uwp-apps.md) | UWP 앱을 판매하거나 다른 사용자에게 배포하려면 앱용 appxupload 패키지를 만들어야 합니다. appxupload를 만들면 테스트 및 테스트용으로 로드하는 데 사용할 다른 appx 패키지도 생성됩니다. 이 appx 패키지를 장치에 테스트용으로 로드하여 앱을 직접 배포할 수 있습니다. 이 문서에서는 UWP 앱 패키지 구성, 생성, 테스트 프로세스에 대해 설명합니다. 테스트용 로드에 대한 자세한 내용은 [DISM을 사용하여 앱을 테스트용으로 로드](http://go.microsoft.com/fwlink/?LinkID=231020)를 참조하세요. |
| [MakeAppx.exe 도구를 사용하여 앱 패키지 만들기](create-app-package-with-makeappx-tool.md) | MakeAppx.exe는 앱 패키지와 번들을 만들고, 암호화 및 암호 해독하고, 파일을 추출합니다. |
| [WinAppDeployCmd.exe 도구를 사용하여 앱 설치](install-universal-windows-apps-with-the-winappdeploycmd-tool.md) | Windows 응용 프로그램 배포(WinAppDeployCmd.exe)는 Windows 10 컴퓨터에서 Windows 10 Mobile 디바이스로 UWP 앱을 배포하는 데 사용할 수 있는 명령줄 도구입니다. Windows 10 Mobile 디바이스가 Microsoft Visual Studio나 해당 앱에 대한 솔루션 없이 동일한 서브넷에서 사용 가능하거나 USB로 연결되면 이 도구를 사용하여 .appx 패키지를 배포할 수 있습니다. 이 문서는 이 도구를 사용하여 UWP 앱을 설치하는 방법을 설명합니다. |
| [UWP 앱에 대한 자동화된 빌드 설정](auto-build-package-uwp-apps.md) | 자동화된 빌드 프로세스의 일부로 앱을 패키징하려는 경우 이 항목에서 VSTS(Visual Studio Team Services)를 사용하여 수행하는 방법을 보여 줍니다. |
| [앱 접근 권한 값 선언](app-capability-declarations.md) | 사진, 음악 또는 디바이스(예: 카메라 또는 마이크)와 같은 특정 리소스 및 API에 액세스하려면 UWP 앱의 [패키지 매니페스트](https://msdn.microsoft.com/library/windows/apps/BR211474)에서 접근 권한 값을 선언해야 합니다. |
| [앱에 대한 패키지 업데이트 다운로드 및 설치](self-install-package-updates.md) | UWP 앱은 프로그래밍 방식으로 패키지 업데이트를 확인하고 업데이트를 설치할 수 있습니다. 앱은 Windows 개발자 센터 대시보드에 필수로 표시된 패키지를 쿼리하고 필수 업데이트가 설치될 때까지 기능을 사용하지 않도록 할 수 있습니다. 이 문서에서는 이러한 작업을 수행하는 방법을 설명합니다. |
 



<!--HONumber=Dec16_HO1-->


