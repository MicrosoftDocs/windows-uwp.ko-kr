---
description: 플랫폼 간 앱을 개발할 때 선택 하는 항목은 무엇 인가요?
title: IOS 및 UWP 앱 개발에 대 한 접근 방식 선택
ms.assetid: 5CDAB313-07B7-4A32-A49B-026361DCC853
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 538fbc33d161f5f91033427af76438f49d3d3b68
ms.sourcegitcommit: 28bd367ab8acc64d4b6f3f73adca12100cbd359f
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/24/2020
ms.locfileid: "82148581"
---
# <a name="selecting-an-approach-to-ios-and-uwp-app-development"></a>IOS 및 UWP 앱 개발에 대 한 접근 방식 선택


플랫폼 간 앱을 개발할 때 선택할 수 있는 항목은 무엇 인가요?

## <a name="whats-the-best-way-to-support-both-ios-and-windows"></a>IOS와 Windows를 모두 지 원하는 가장 좋은 방법은 무엇 인가요?

Windows 및 iOS는 매우 다른 beasts 것 처럼 보일 수 있지만, 두 플랫폼과 Android를 모두 지 원하는 앱을 작성 해야 하는 경우에는 많은 도구와 기술을 통해 많은 도움이 될 수 있습니다. 가장 적합 한 솔루션은 작성 하는 앱의 유형 및 처음부터 시작 하거나 기존 프로젝트를 이식 하는지에 따라 달라 집니다.

## <a name="writing-a-new-app"></a>새 앱 작성

깨끗 한 슬레이트를 사용 하면 다음과 같은 다양 한 옵션을 사용할 수 있습니다.

-   [Xamarin](https://xamarin.com/)

    Xamarin을 사용 하면 c #으로 앱을 작성 하 고, Windows에서 실행 하 고, 네이티브 iOS 앱을 만들 수 있습니다. Xamarin에 대 한 지원은 Visual Studio에 기본 제공 됩니다. 올바른 프로젝트 형식을 선택 하면 됩니다.

-   [Apache Cordova](https://www.microsoft.com/?ref=go)

    Javascript 및 HTML이 더 많은 경우 Apache Cordova (즉, PhoneGap)를 통해 iOS, Windows 및 Android 용 플랫폼 간 앱을 만들 수 있습니다. 이 프로젝트 형식은 Visual Studio에도 기본적으로 제공 됩니다.

-   게임-엔진

    [Unity3D](https://www.unity3d.com/) 및 [unreal Engine](https://www.unrealengine.com/en-US/) 과 같은 도구를 사용 하 여 Windows 및 iOS를 비롯 한 다양 한 플랫폼에 대 한 AAA 품질 게임을 작성할 수 있습니다. Unity는 c # 스크립팅을 지원 합니다. Unreal은 c + +를 사용 합니다.

-   [MonoGame](http://www.monogame.net/)

    정신적 후속 작업은 XNA입니다. 이제는 오픈 소스 플랫폼 간 프레임 워크입니다. 즉, 물리 엔진을 지 원하는 많은 플랫폼에 대해 c #으로 앱을 작성 하 고 2D 및 3D 그래픽을 만들 수 있습니다.

## <a name="adapting-an-existing-app"></a>기존 앱 적응

기존 iOS 앱을 사용 하면 옵션이 약간 제한적입니다. 그러나 모두 손실 되지는 않습니다.

-   [IOS 용 Windows 브리지](https://github.com/Microsoft/WinObjC)

    Project Islandwood 라고도 하는이 도구는 Visual Studio로 직접 Xcode 프로젝트를 가져올 수 있는 개발이 가능한 개발 도구입니다. Visual Studio 내에서 목표-C 코드를 작성 하 고 디버그할 수 있습니다. 프로젝트에서 그래픽의 코코스와 같은 라이브러리를 사용 하는 경우 앱을 신속 하 게 이식 하는 데 유용한 방법을 찾을 수 있습니다.

-   C + + 코드의 용도를 변경 합니다.

    핵심 비즈니스 논리를 c + +로 작성 하는 경우에는 목표 C 또는 Swift이 아니라 프로젝트의 사소한 변경 내용 으로만이 코드를 사용할 수 있습니다. 그런 다음 다른 Windows 앱과 마찬가지로 XAML을 사용 하 여 UI를 정의 하 고 필요한 경우 c + + 코드를 호출할 수 있습니다.

-   [각도를 사용 하 여 Windows에서 OpenGL ES 실행](https://github.com/microsoft/angle/wiki)

    OpenGL ES 2.0 프로젝트를 이식 하는 중간 단계는 각도를 사용 하는 것입니다. 각도를 사용 하면 OpenGL ES API 호출을 DirectX 11 API 호출로 변환 하 여 Windows에서 OpenGL ES 콘텐츠를 실행할 수 있습니다.

## <a name="other-cross-platform-authoring-tools"></a>기타 플랫폼 간 제작 도구

-   [GameSalad](https://gamesalad.com/)

    게임 제작 환경.

-   [생성 2]( https://www.scirra.com/)

    게임 제작 환경.

-   [티타늄 Studio](https://www.appcelerator.com/platform/titanium-studio/)

    플랫폼 간 제작 환경.

-   [Cocos2D-x](https://www.cocos2d-x.org/)

    스프라이트 처리 및 물리학 모델링을 위한 플랫폼 간 코드 라이브러리입니다.

-   [영향 .js](https://impactjs.com/)

    HTML 기반 게임 라이브러리.

-   [마말레이](http://madewithmarmalade.com/)

    플랫폼 간 SDK입니다.

-   [OpenFL](https://www.openfl.org/)

    플랫폼 간 개발 도구입니다.

-   [GameMaker](https://www.yoyogames.com/gamemaker/studio)

    게임 전용 제작 환경

-   [PlayCanvas](https://playcanvas.com/)

    HTML 기반 게임 개발 도구입니다.

