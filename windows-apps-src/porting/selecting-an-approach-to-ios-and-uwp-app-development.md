---
description: 플랫폼 간 앱을 개발할 때 어떤 방법을 선택할 수 있나요?
title: iOS 및 UWP 앱 개발 방법 선택
ms.assetid: 5CDAB313-07B7-4A32-A49B-026361DCC853
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 6b87ee76481492de0dfb23394e0aef7f017f3305
ms.sourcegitcommit: b11f305dbf7649c4b68550b666487c77ea30d98f
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/27/2018
ms.locfileid: "7850063"
---
# <a name="selecting-an-approach-to-ios-and-uwp-app-development"></a>iOS 및 UWP 앱 개발 방법 선택


플랫폼 간 앱을 개발할 때 어떤 방법을 선택할 수 있나요?

## <a name="whats-the-best-way-to-support-both-ios-and-windows"></a>iOS와 Windows를 모두 지원하는 가장 좋은 방법은 무엇인가요?

Windows 및 iOS는 양상이 매우 다른 거대한 제품으로 보일 수 있지만 두 플랫폼(및 Android)을 모두 지원하는 앱을 작성해야 하는 경우에 도움이 되는 도구와 기술이 많이 있습니다. 최선의 솔루션은 작성하는 앱의 유형과 처음부터 새로 작성할지 아니면 기존 프로젝트를 포팅할지에 따라 좌우됩니다.

## <a name="writing-a-new-app"></a>새 앱 작성

새로 시작하는 경우 다음을 비롯한 다양한 옵션을 사용할 수 있습니다.

-   [Xamarin](http://go.microsoft.com/fwlink/p/?LinkID=320484)

    Xamarin을 사용하면 C#으로 앱을 작성하고, Windows에서 실행할 수 있으며 네이티브 iOS 앱도 만들 수 있습니다. Xamarin 지원은 Visual Studio에 기본적으로 제공되므로 올바른 프로젝트 형식만 선택하면 됩니다.

-   [Apache Cordova](http://go.microsoft.com/fwlink/p/?LinkID=400439)

    Javascript 및 HTML이 더 편리한 경우 Apache Cordova(일명 PhoneGap)가 iOS, Android 및 Windows용 플랫폼 간 앱을 만드는 데 도움이 됩니다. 이 프로젝트 형식 또한 Visual Studio에 기본적으로 제공됩니다.

-   게임 엔진

    [Unity3D](http://go.microsoft.com/fwlink/p/?LinkID=320479) 및 [Unreal Engine](http://go.microsoft.com/fwlink/p/?LinkID=394062)과 같은 도구를 재량껏 사용해서 Windows 및 iOS를 비롯한 다른 많은 플랫폼용 AAA급 품질의 게임을 작성할 수 있습니다. Unity는 C# 스크립팅을 지원하고 Unreal은 C++를 사용합니다.

-   [MonoGame](http://go.microsoft.com/fwlink/p/?LinkID=320483)

    XNA의 개념을 이어 받은 후속 제품입니다. 현재는 오픈 소스 플랫폼 간 프레임워크로 사용됩니다. 즉, 물리학 엔진 및 2D/3D 그래픽이 지원되는 많은 플랫폼에서 C#으로 앱을 작성할 수 있습니다.

## <a name="adapting-an-existing-app"></a>기존 앱 조정

기존 iOS 앱을 사용하는 경우에는 사용 가능한 옵션이 약간 더 제한됩니다. 그러나 기능을 구현하는 것이 불가능하지는 않습니다.

-   [iOS용 Windows 브리지](https://go.microsoft.com/fwlink/p/?LinkId=619014)

    Project Islandwood라고도 하는 이 도구는 Visual Studio로 직접 Xcode 프로젝트를 가져올 수 있는 개발 중인 도구입니다. Objective-C 코드는 Visual Studio 내에서 빌드 및 디버깅할 수 있습니다. 프로젝트에서 그래픽용 Cocos와 같은 라이브러리를 사용하면 이것이 앱을 빠르게 포팅하는 유용한 방법이라는 것을 알게 됩니다.

-   C++ 코드 용도를 다시 설정합니다.

    핵심 비즈니스 논리를 Objective-C 또는 Swift가 아닌 C++로 작성하는 경우 프로젝트를 사소하게 변경할 때만 이 코드를 사용하는 것이 좋습니다. 그런 후 다른 Windows 앱처럼 XAML을 사용하여 UI를 정의하고, 필요할 때 C++ 코드로 불러올 수 있습니다.

-   [ANGLE을 사용하여 Windows에서 OpenGL ES를 실행합니다.](http://go.microsoft.com/fwlink/p/?linkid=618387)

    OpenGL ES 2.0 프로젝트를 포팅하는 중간 단계에서 ANGLE을 사용합니다. ANGLE을 사용하면 OpenGL ES API 호출을 DirectX 11 API 호출로 변환하여 Windows에서 OpenGL ES 콘텐츠를 실행할 수 있습니다.

## <a name="other-cross-platform-authoring-tools"></a>기타 플랫폼 간 제작 도구

-   [GameSalad](http://go.microsoft.com/fwlink/p/?LinkID=320480)

    게임 제작 환경입니다.

-   [Construct 2]( http://go.microsoft.com/fwlink/p/?LinkID=320481)

    게임 제작 환경입니다.

-   [Titanium Studio](http://go.microsoft.com/fwlink/p/?LinkID=320482)

    플랫폼 간 제작 환경입니다.

-   [Cocos2D-x](http://go.microsoft.com/fwlink/p/?LinkID=320485)

    스프라이트 처리 및 물리학 모델링을 위한 플랫폼 간 코드 라이브러리입니다.

-   [Impact.js](http://go.microsoft.com/fwlink/p/?LinkID=320486)

    HTML 기반 게임 라이브러리입니다.

-   [Marmalade](http://go.microsoft.com/fwlink/p/?LinkID=320487)

    플랫폼 간 SDK입니다.

-   [OpenFL](http://go.microsoft.com/fwlink/p/?LinkID=320488)

    플랫폼 간 개발 도구입니다.

-   [GameMaker](http://go.microsoft.com/fwlink/p/?LinkID=320490)

    게임용 제작 환경입니다.

-   [PlayCanvas](http://go.microsoft.com/fwlink/p/?LinkID=394061)

    HTML 기반 게임 개발 도구입니다.

