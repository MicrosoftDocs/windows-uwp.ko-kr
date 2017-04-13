---
author: Mtoepke
title: "Xbox One의 UWP"
description: "Xbox One에서 UWP(유니버설 Windows 플랫폼)용 앱을 빌드하는 방법"
ms.author: mtoepke
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
ms.assetid: 2d935f53-84db-4108-86dc-cb6a0749782f
ms.openlocfilehash: 82c8fd0945ed49f8accf5e101acfbea151caa3a1
ms.sourcegitcommit: 909d859a0f11981a8d1beac0da35f779786a6889
translationtype: HT
---
# <a name="uwp-on-xbox-one"></a>Xbox One의 UWP

Xbox One에서 UWP(유니버설 Windows 플랫폼)용 앱의 빌드를 시작합니다.

Xbox One의 UWP는 앱과 게임 개발을 둘 다 지원합니다. Xbox에서 게임이나 앱을 실험, 제작, 테스트하기 위해 개발자 프로그램에 참여할 필요는 없습니다. Xbox One에 게임을 게시하고 판매할 준비가 되었거나 Windows 10의 Xbox Live이 제공하는 장점을 누리려면 [Xbox Live Creators 프로그램](https://developer.microsoft.com/games/xbox/xboxlive/creator)에 참여하거나 [ID@Xbox](http://www.xbox.com/Developers/id) 개발자가 되어야 합니다. 자세한 내용은 [개발자 프로그램 개요](https://developer.microsoft.com/games/xbox/docs/xboxlive/get-started/developer-program-overview.html)를 참조하세요.

이 섹션에는 설정 단계, 인증 프로세스 안내, 필요한 버전의 Visual Studio 및 Windows 10 도구 설치 정보, 간단한 첫 번째 응용 프로그램을 빌드, 실행, 디버그하는 단계 등이 포함되어 있습니다. 

| 항목      | 설명 |
|------------|-------------|
|[시작하기](getting-started.md)| Xbox One의 UWP 개발 시작 가이드입니다. |
|[새로운 기능](whats-new.md)| Xbox One의 UWP에 대한 새로운 기능을 강조하여 설명합니다. |
|[Xbox 모범 사례](tailoring-for-xbox.md)| 마우스 모드를 끄고, 화면 가장자리까지 그리고, 크기 조정을 사용하지 않도록 설정하는 방법입니다. |
|[알려진 문제](known-issues.md)| Xbox One의 UWP에 대해 알려진 문제입니다. |
|[FAQ](frequently-asked-questions.md)| Xbox One의 UWP와 관련된 질문과 대답입니다. |
|[Xbox One 개발자 모드 활성화](devkit-activation.md)| Xbox One에서 개발자 모드를 사용하도록 설정하는 방법을 설명합니다. |
|[도구](introduction-to-xbox-tools.md)| Xbox One 관련 도구 _개발자 홈_, Windows Device Portal 사용 방법 및 개발을 위한 Visual Studio 설정 방법에 대해 설명합니다. 또한 이 섹션에서는 새로운 개발자에게 첫 번째 Xbox UWP 응용 프로그램을 안내하고 Fiddler 도구를 사용하여 네트워크 트래픽을 보는 방법도 설명합니다. |
|[Xbox 개발 환경에서의 UWP 설정](development-environment-setup.md)| Xbox One 개발 환경을 설정하고 테스트하는 단계를 설명합니다. |
|[Xbox One의 UWP 앱 및 게임에 대한 시스템 리소스](system-resource-allocation.md)| Xbox One에서 실행할 때 응용 프로그램에서 사용할 수 있는 리소스를 설명합니다. | 
|[Xbox 및 TV용 디자인](..\input-and-devices\designing-for-tv.md)| TV에 표시되고 컨트롤러를 입력에 사용하는 앱을 디자인하는 모범 사례를 설명합니다. |  
|[다중 사용자 응용 프로그램 소개](multi-user-applications.md)| Xbox One의 MUA(다중 사용자 응용 프로그램)에 대해 설명합니다. |
|[샘플](samples.md)| Xbox용 개발을 시작하는 데 유용한 XAML 및 JavaScript 샘플이 있는 github 위치(TVHelpers)에 대한 포인터입니다. 샘플에는 전체 XAML 미디어 앱 템플릿, 자동 컨트롤러 탐색, 리치 미디어 재생 및 웹 기반 기술 검색이 포함됩니다. |
|[기존 게임을 Xbox로 가져오기](development-lanes-landing.md)|게임이 제작된 기반 기술을 바탕으로 UWP를 사용하여 게임을 Xbox로 가져오는 프로세스를 촉진할 수 있는 단계별 지침을 안내할 수 있습니다.|
|[Xbox One에서 개발자 모드 사용 안 함](devkit-deactivation.md)| Xbox One에서 개발자 모드를 사용하지 않도록 설정하는 방법을 설명합니다. |
|[Xbox One에서 아직 지원되지 않는 UWP 기능](http://go.microsoft.com/fwlink/p/?LinkId=760755)|  Xbox One에서 아직 완전히 작동하지 않는 UWP 기능 영역을 설명합니다.|  

## <a name="see-also"></a>참고 항목
- [Xbox One의 UWP 앱 개요](http://go.microsoft.com/fwlink/p/?LinkId=780786) 
- [Windows 10 UWP 앱 시작 자동화](automate-launching-uwp-apps.md)
- [게임 개발용 CPUSets](cpusets-games.md)
  
