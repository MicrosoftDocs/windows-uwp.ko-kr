---
author: QuinnRadich
title: 유니버설 Windows 플랫폼을 통해 앱 제작
description: 생각보다 쉽게 Windows 10용 UWP(유니버설 Windows 플랫폼) 앱을 만들 수 있습니다.
ms.author: quradic
ms.date: 08/24/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 66536a3059ea6d9b17709c836f4149b1ec583165
ms.sourcegitcommit: 1eabcf511c7c7803a19eb31f600c6ac4a0067786
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/28/2018
ms.locfileid: "1692714"
---
# <a name="create-apps-for-windows-10"></a>Windows 10용 앱 만들기

![앱 빌드](images/build-your-app.png)

[UWP 플랫폼](universal-application-platform-guide.md)을 시작합니다! 이 자습서는 첫 번째 UWP 앱을 만들려고 하거나 더 고급 기능을 사용하길 원하는 모든 사용자에게 올바른 방향을 제시합니다. 다음 항목에 대하 알아봅니다.

-   Microsoft Visual Studio에서 UWP 프로젝트 만들기.
-   프로젝트에 UI 요소 및 코드 추가.
-   XAML, 데이터 바인딩 및 기타 기본 UWP 요소 사용.
-   앱에 Ink 및 Dial과 같은 고유한 UWP 기능 통합.
-   타사 라이브러리를 사용하여 새 기능 추가.
-   로컬 컴퓨터에서 앱 빌드 및 디버그.

## <a name="ask-a-bot"></a>봇에게 물어보세요!

문제가 있거나 올바른 문서를 찾는 데 도움이 필요하면 아래의 실험용 채팅 봇에게 물어보세요. 예를 들어 "어디에서 Visual Studio를 다운로드할 수 있나요?" 또는 "Fluent 디자인에 대해 말해 주세요."라고 물어보세요. 유용한 답을 얻지 못하면 질문을 약간 수정해 보세요.

<iframe src='https://webchat.botframework.com/embed/DocBot4?s=T2nP6qZUXC8.cwA.lvc.AR-ZBwtULpaITu6_dAhMwrmg4R2GSLNzIoiMNFL8M7M' height="400" width="400"></iframe>

## <a name="write-your-first-uwp-app-in-your-favorite-programming-language"></a>즐겨 쓰는 프로그래밍 언어를 사용하여 첫 번째 UWP 앱 작성

새로운 개발자이거나 Windows 플랫폼에 익숙하고 UWP를 시작하려는 경우 다음 기본 자습서를 확인하세요.

* [C#, Visual C++ 또는 JavaScript 사용하여 첫 UWP 앱 만들기](your-first-app.md)

IOS 개발자인 경우

* [iOS용 Windows 브리지](https://developer.microsoft.com/windows/bridges/ios)를 사용하여 기존 코드를 UWP 앱으로 변환하고 Objective-C로 개발을 계속 진행하세요.

계속 배우고 있는 중이거나 기억을 되살리려면 다음 외부 리소스를 읽어 보세요.

* [Windows 10 개발자 가이드](https://go.microsoft.com/fwlink/?linkid=850804)
* [Microsoft Virtual Academy](http://www.microsoftvirtualacademy.com/)

## <a name="customize-your-apps-layout-and-appearance-with-xaml"></a>XAML을 사용하여 앱의 레이아웃 및 모양 사용자 지정

대부분의 UWP 앱은 XAML 생성 언어를 사용하여 UI를 만듭니다. 핵심 기능을 사용하여 앱의 시각적 표현을 사용자 지정할 수 있는 방법을 확인하고 앱의 고유한 모습을 만드는 방법을 이 지침에서 살펴보세요.

* [앱 UI 디자인 소개](../design/basics/design-and-ui-intro.md)
* [자습서: XAML에서 사용자 인터페이스 만들기](../design/basics/xaml-basics-ui.md)
* [UWP 앱의 레이아웃](../design/layout/index.md)
* [UWP 앱의 컨트롤 및 패턴](../design/controls-and-patterns/index.md)

## <a name="use-features-unique-to-windows-10"></a>Windows 10 고유의 기능 사용

Windows 10이 특별한 이유는 무엇인가요? 고유 기능 중 일부만 사용하는 방법을 알아보세요.

* [자습서: UWP 앱에서 잉크 지원](../design/input/ink-walkthrough.md)
* [자습서: Surface Dial 지원](../design/input/radialcontroller-walkthrough.md)
* [최신 Windows 버전의 새로운 기능 살펴보기](../whats-new/windows-10-version-latest.md)

Windows 10 개발에 대한 방법 문서와 자세한 설명서를 살펴보세요.

* [UWP 앱 개발에 대한 방법 문서](https://developer.microsoft.com/windows/apps/develop)
* [UWP 앱에 대한 참조 API](https://docs.microsoft.com/en-us/uwp/)

## <a name="develop-javascript-and-web-apps"></a>JavaScript 및 웹 앱 개발

UWP는 다양한 언어와 프레임워크를 지원하는 매우 유연한 플랫폼입니다. JavaScript를 사용하여 UWP 앱을 빌드하고, 자신만의 기술을 사용하여 Microsoft Store에서 추천될 수 있는 호스트된 웹 앱을 빌드하세요.

* [HTML5, CSS3, 및 JavaScript를 사용하여 웹 기술을 활용, 앱을 빌드합니다.](your-first-app.md#javascript-and-html)

웹 앱 빌드에 대한 추가 정보를 살펴보고 싶으세요?

* [Microsoft Edge 개발자 설명서](https://docs.microsoft.com/microsoft-edge/)

## <a name="cross-platform-and-mobile-development"></a>플랫폼 간 및 모바일 개발

* Android 및 iOS를 대상으로 해야 하나요? [Xamarin](https://www.xamarin.com)을 확인하세요.

## <a name="see-also"></a>참고 항목

* [UWP 앱 게시](https://developer.microsoft.com/store/publish-apps)
* [UWP 앱 개발에 대한 방법 문서](https://developer.microsoft.com/windows/apps/develop)
* [UWP 개발자를 위한 코드 샘플](https://developer.microsoft.com/windows/samples)
* [UWP 앱이란 무엇인가요?](universal-application-platform-guide.md)
* [설정](get-set-up.md)
* [Windows 계정 등록](sign-up.md)