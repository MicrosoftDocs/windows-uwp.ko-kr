---
author: Mtoepke
title: Xbox One의 UWP
description: Xbox One에서 UWP(유니버설 Windows 플랫폼)용 앱을 빌드하는 방법
ms.author: mstahl
ms.date: 10/25/2017
ms.topic: article
keywords: windows 10, uwp
ms.assetid: 2d935f53-84db-4108-86dc-cb6a0749782f
ms.localizationpriority: medium
ms.openlocfilehash: e7c14c51c0b5b09b96859a8aba97172485a14c85
ms.sourcegitcommit: e2fca6c79f31e521ba76f7ecf343cf8f278e6a15
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/15/2018
ms.locfileid: "6983788"
---
# <a name="uwp-on-xbox-one"></a>Xbox One의 UWP

Xbox One에서 UWP(유니버설 Windows 플랫폼)용 앱의 빌드를 시작합니다.

Xbox One의 UWP는 앱과 게임 개발을 둘 다 지원합니다. Xbox에서 게임이나 앱을 실험, 제작, 테스트하기 위해 개발자 프로그램에 참여할 필요는 없습니다. 필요한 모든 [파트너 센터](https://partner.microsoft.com/dashboard)에서 [개발자 계정](https://developer.microsoft.com/en-us/store/register) 입니다. Xbox One에 게임을 게시하고 판매할 준비가 되었거나 Windows 10의 Xbox Live이 제공하는 장점을 누리려면 [Xbox Live Creators 프로그램](https://developer.microsoft.com/games/xbox/xboxlive/creator)에 참여하거나 [ID@Xbox](http://www.xbox.com/Developers/id) 개발자가 되어야 합니다. ID@Xbox 개발자가 되려면, 개발자 계정에 등록하기 전에 먼저 프로그램을 적용하는 것이 좋습니다. 자세한 내용은 [개발자 프로그램 개요](../xbox-live/developer-program-overview.md)를 참조하세요.

이 섹션에는 설정 단계, 인증 프로세스 안내, 필요한 버전의 Visual Studio 및 Windows 10 도구 설치 정보, 간단한 첫 번째 응용 프로그램을 빌드, 실행, 디버그하는 단계 등이 포함되어 있습니다. 

| 항목      | 설명 |
|------------|-------------|
|[시작하기](getting-started.md)| Xbox One의 UWP 개발 시작 가이드입니다. |
|[새로운 기능](whats-new.md)| Xbox One의 UWP에 대한 새로운 기능을 강조하여 설명합니다. |
|[Xbox One 개발자 모드 활성화](devkit-activation.md)| Xbox One에서 개발자 모드를 사용하도록 설정하는 방법을 설명합니다. |
|[Xbox One에서 개발자 모드 사용 안 함](devkit-deactivation.md)| Xbox One에서 개발자 모드를 사용하지 않도록 설정하는 방법을 설명합니다. |
|[Xbox 개발 환경에서의 UWP 설정](development-environment-setup.md)| Xbox One 개발 환경을 설정하고 테스트하는 단계를 설명합니다. |
|[샘플](samples.md)| Xbox용 개발을 시작하는 데 유용한 XAML 및 JavaScript 샘플이 있는 GitHub 위치(TVHelpers)에 대한 포인터입니다. 샘플에는 전체 XAML 미디어 앱 템플릿, 자동 컨트롤러 탐색, 리치 미디어 재생 및 웹 기반 기술 검색이 포함됩니다. |
|[알려진 문제](known-issues.md)| Xbox One의 UWP에 대해 알려진 문제입니다. |
|[FAQ](frequently-asked-questions.md)| Xbox One의 UWP와 관련된 질문과 대답입니다. |
|[도구](introduction-to-xbox-tools.md)| Xbox One 관련 도구 _개발자 홈_, Windows Device Portal 사용 방법 및 개발을 위한 Visual Studio 설정 방법에 대해 설명합니다. 또한 이 섹션에서는 새로운 개발자에게 첫 번째 Xbox UWP 응용 프로그램을 안내하고 Fiddler 도구를 사용하여 네트워크 트래픽을 보는 방법도 설명합니다. |
| [Xbox 이벤트에서 앱 개발](https://developer.microsoft.com/windows/projects/campaigns/app-dev-on-xbox-event) | Xbox의 앱 개발 이벤트는 Xbox에서 앱을 처음 빌드하는 개발자를 위한 멋진 출발점입니다. 기록된 세션을 보고 이벤트에서 블로그 게시물을 읽습니다. |
|[Xbox 및 TV용 디자인](../design/devices/designing-for-tv.md)| TV에 표시되고 컨트롤러를 입력에 사용하는 앱을 디자인하는 모범 사례를 설명합니다. |
|[Xbox 모범 사례](tailoring-for-xbox.md)| 마우스 모드를 끄고, 화면 가장자리까지 끌고, 크기 조정을 사용하지 않도록 설정하는 방법입니다. |
|[음성 명령을 사용하여 UI 요소 호출](ves-on-xbox.md)| Xbox의 UWP 앱에서 음성 지원 셸을 지원하는 모범 사례를 설명합니다. |
|[Xbox One의 UWP 앱 및 게임에 대한 시스템 리소스](system-resource-allocation.md)| Xbox One에서 실행할 때 응용 프로그램에서 사용할 수 있는 리소스를 설명합니다. |
|[다중 사용자 응용 프로그램 소개](multi-user-applications.md)| Xbox One의 MUA(다중 사용자 응용 프로그램)에 대해 설명합니다. |
| [Xbox One 개발 작업 자동화](https://github.com/Microsoft/WindowsDevicePortalWrapper/tree/v0.9.4) | GitHub에서 WindowsDevicePortalWrapper 프로젝트는 앱을 배포하거나 시작하는 등의 일반적인 개발 작업을 자동화 수 있도록 라이브러리를 제공합니다. 프로젝트에는 일반적인 작업에 API를 사용하는 방법을 보여 주는 XboxWdpDriver.exe 샘플이 포함되어 있습니다. |
|[기존 게임을 Xbox로 가져오기](development-lanes-landing.md)|게임이 제작된 기반 기술을 바탕으로 UWP를 사용하여 게임을 Xbox로 가져오는 프로세스를 촉진할 수 있는 단계별 지침을 안내할 수 있습니다.|
|[Xbox One에서 아직 지원되지 않는 UWP 기능](http://go.microsoft.com/fwlink/p/?LinkId=760755)|  Xbox One에서 아직 완전히 작동하지 않는 UWP 기능 영역을 설명합니다.|

## <a name="videos"></a>비디오

Channel 9에 대한 다음 문서에서도 Xbox에서 멋진 앱을 빌드하는 방법에 대한 유용한 정보를 제공합니다.

* [Building Great Universal Windows Platform (UWP) Apps for Xbox(뛰어난 Xbox용 UWP(유니버설 Windows 플랫폼) 앱 빌드)](https://channel9.msdn.com/Events/Build/2016/B883)
* [Adapt Your App for Xbox One and TV(Xbox One 및 TV에 맞게 앱 조정)](https://channel9.msdn.com/Events/Build/2016/T651-R1)
* [UWP Development 1: Building an Adaptive UI(UWP 개발 1: 적응 UI 빌드)](https://channel9.msdn.com/Events/Build/2016/L724-R1)
* [Web Apps Beyond the Browser: Cross-Platform Meets Cross Device(브라우저 이외의 웹앱: 플랫폼 간 및 디바이스 간)](https://channel9.msdn.com/Events/Build/2016/B888)

## <a name="see-also"></a>기타 참조

- [Windows 10 UWP 앱 시작 자동화](automate-launching-uwp-apps.md)
- [게임 개발용 CPUSets](cpusets-games.md)