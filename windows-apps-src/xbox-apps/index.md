---
title: Xbox One의 UWP
description: Xbox One에서 UWP(유니버설 Windows 플랫폼)용 앱을 빌드하는 방법
ms.date: 10/25/2017
ms.topic: article
keywords: windows 10, uwp
ms.assetid: 2d935f53-84db-4108-86dc-cb6a0749782f
ms.localizationpriority: medium
ms.openlocfilehash: aecb0a6e6a7d9dfbe05a43b760b044d032fff4e1
ms.sourcegitcommit: f727b68e86a86c94eff00f67ed79a1c12666e7bc
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/29/2020
ms.locfileid: "75685028"
---
# <a name="uwp-on-xbox-one"></a>Xbox One의 UWP

Xbox One에서 UWP(유니버설 Windows 플랫폼)용 앱 빌드를 시작합니다.

Xbox One의 UWP는 앱과 게임 개발을 둘 다 지원합니다. Xbox에서 게임이나 앱을 실험, 제작, 테스트하기 위해 개발자 프로그램에 참여할 필요는 없습니다. [파트너 센터](https://partner.microsoft.com/dashboard)의 [개발자 계정](https://developer.microsoft.com/store/register)만 있으면 됩니다. Xbox One에 게임을 게시하고 판매할 준비가 되었거나 Windows 10의 Xbox Live가 제공하는 장점을 누리려면 [Xbox Live 크리에이터스 프로그램](https://developer.microsoft.com/games/xbox/xboxlive/creator)에 참여하거나 [ID@Xbox](https://www.xbox.com/Developers/id) 개발자가 되어야 합니다. ID@Xbox 개발자가 될 생각이라면 개발자 계정에 등록하기 전에 프로그램 참가 신청부터 하는 것이 좋습니다. 자세한 내용은 [개발자 프로그램 개요](https://docs.microsoft.com/gaming/xbox-live/developer-program-overview)를 참조하세요.

이 섹션에는 설정 단계, 인증 프로세스 안내, 필요한 버전의 Visual Studio 및 Windows 10 도구 설치 정보, 간단한 첫 번째 애플리케이션을 빌드, 실행, 디버그하는 단계 등이 포함되어 있습니다. 

| 항목      | 설명 |
|------------|-------------|
|[시작](getting-started.md)| Xbox One의 UWP 개발 시작 가이드입니다. |
|[새로운 기능](whats-new.md)| Xbox One의 UWP에 대한 새로운 기능을 강조하여 설명합니다. |
|[Xbox One 개발자 모드 활성화](devkit-activation.md)| Xbox One에서 개발자 모드를 사용하도록 설정하는 방법을 설명합니다. |
|[Xbox One에서 개발자 모드 사용 안 함](devkit-deactivation.md)| Xbox One에서 개발자 모드를 사용하지 않도록 설정하는 방법을 설명합니다. |
|[Xbox 개발 환경에서 UWP 설정](development-environment-setup.md)| Xbox One 개발 환경을 설정하고 테스트하는 단계를 설명합니다. |
|[샘플](samples.md)| Xbox용 제품 개발을 시작하는 데 유용한 XAML 및 JavaScript 샘플이 있는 GitHub 위치(TVHelpers)를 안내하는 포인터입니다. 샘플에는 전체 XAML 미디어 앱 템플릿, 자동 컨트롤러 탐색, 리치 미디어 재생 및 웹 기반 기술 검색이 포함됩니다. |
|[알려진 문제](known-issues.md)| Xbox One의 UWP에 대해 알려진 문제입니다. |
|[FAQ](frequently-asked-questions.md)| Xbox One의 UWP와 관련된 질문과 대답입니다. |
|[도구](introduction-to-xbox-tools.md)| Xbox One 관련 도구 _개발자 홈_, Windows Device Portal 사용 방법 및 개발을 위한 Visual Studio 설정 방법에 대해 설명합니다. 또한 이 섹션에서는 새로운 개발자에게 첫 번째 Xbox UWP 애플리케이션을 안내하고 Fiddler 도구를 사용하여 네트워크 트래픽을 보는 방법도 설명합니다. |
| [Xbox에서 앱 개발 이벤트](https://developer.microsoft.com/windows/projects/campaigns/app-dev-on-xbox-event) | Xbox에서 앱 개발 이벤트는 처음으로 Xbox에서 앱을 빌드하는 개발자를 위한 멋진 출발점입니다. 녹화된 세션을 시청하고 이벤트의 블로그 게시물을 읽어보세요. |
|[Xbox 및 TV용 디자인](../design/devices/designing-for-tv.md)| TV에 표시되고 컨트롤러를 입력에 사용하는 앱을 디자인하는 모범 사례를 설명합니다. |
|[Xbox 모범 사례](tailoring-for-xbox.md)| 마우스 모드를 끄고, 화면 가장자리까지 그리고, 크기 조정을 사용하지 않도록 설정하는 방법입니다. |
|[음성을 사용하여 UI 요소 호출](ves-on-xbox.md)| Xbox의 UWP 앱에서 음성 지원 셸을 지원하는 모범 사례를 설명합니다. |
|[Xbox One의 UWP 앱 및 게임에 대한 시스템 리소스](system-resource-allocation.md)| Xbox One에서 실행할 때 애플리케이션에서 사용할 수 있는 리소스를 설명합니다. |
|[다중 사용자 애플리케이션 소개](multi-user-applications.md)| Xbox One의 MUA(다중 사용자 애플리케이션)에 대해 설명합니다. |
| [Xbox One 개발 작업 자동화](https://github.com/Microsoft/WindowsDevicePortalWrapper/tree/v0.9.4) | GitHub의 WindowsDevicePortalWrapper 프로젝트는 앱을 배포하거나 시작하는 등의 일반적인 개발 작업을 자동화할 수 있는 라이브러리를 제공합니다. 이 프로젝트에는 일반적인 작업에 API를 사용하는 방법을 보여주는 XboxWdpDriver.exe 샘플이 포함되어 있습니다. |
|[기존 게임을 Xbox로 가져오기](development-lanes-landing.md)|게임이 제작된 기반 기술을 바탕으로 UWP를 사용하여 게임을 Xbox로 가져오는 프로세스를 촉진할 수 있는 단계별 지침을 안내할 수 있습니다.|
|[Xbox One에서 아직 지원되지 않는 UWP 기능](https://docs.microsoft.com/uwp/extension-sdks/uwp-limitations-on-xbox?redirectedfrom=MSDN)|  Xbox One에서 아직 완전히 작동하지 않는 UWP 기능 영역을 설명합니다.|

## <a name="videos"></a>동영상

Channel 9에 대한 다음 영상에서도 멋진 Xbox용 앱을 빌드하는 방법에 대한 유용한 정보를 제공합니다.

* [Building Great Universal Windows Platform (UWP) Apps for Xbox(멋진 Xbox용 UWP(유니버설 Windows 플랫폼) 앱 빌드)](https://channel9.msdn.com/Events/Build/2016/B883)
* [Adapt Your App for Xbox One and TV(Xbox One 및 TV에 맞게 앱 조정)](https://channel9.msdn.com/Events/Build/2016/T651-R1)
* [UWP 개발 1: 적응형 UI 빌드](https://channel9.msdn.com/Events/Build/2016/L724-R1)
* [Web Apps Beyond the Browser: Cross-Platform Meets Cross Device(브라우저를 뛰어넘는 웹앱: 교차 플랫폼과 교차 디바이스의 만남)](https://channel9.msdn.com/Events/Build/2016/B888)

## <a name="see-also"></a>참고 항목

- [Windows 10 UWP 앱 시작 자동화](automate-launching-uwp-apps.md)
- [게임 개발용 CPUSets](cpusets-games.md)
- [Xbox One용 점진적 웹앱](https://docs.microsoft.com/microsoft-edge/progressive-web-apps/xbox-considerations)