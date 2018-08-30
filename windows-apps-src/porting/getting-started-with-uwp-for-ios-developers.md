---
author: mcleblanc
description: iOS 개발자용 UWP 시작
title: iOS 개발자용 UWP 시작
ms.assetid: 9F67068B-E578-4C70-B3E0-DFF150FA9BDD
ms.author: markl
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
ms.openlocfilehash: 8b1d7259d16ba963d19c7656ff2572fa659a1710
ms.sourcegitcommit: 909d859a0f11981a8d1beac0da35f779786a6889
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.locfileid: "219557"
---
# <a name="getting-started-with-uwp-for-ios-developers"></a>iOS 개발자용 UWP 시작

\[ Windows 10의 UWP 앱에 맞게 업데이트되었습니다. Windows 8.x 문서는 [보관](http://go.microsoft.com/fwlink/p/?linkid=619132)을 참조하세요. \]

Windows 10용 개발을 고려하는 iOS 개발자인 경우 이러한 문서는 시작하는 데 도움이 됩니다. 또한 앱 작성을 시작할 때 알고 있어야 하는 몇 가지 개념을 소개하고 Windows 스토어에 작업을 게시하는 방법을 설명합니다.

이 섹션에서는 점진적으로 시작하면서 Microsoft Visual Studio 및 C# 프로그래밍 언어를 사용하여 간단한 앱을 만드는 방법을 살펴보고 특히 현재 사용하는 도구에 따라 프로세스가 어떻게 달라지는지에 대해 설명합니다. C#을 잘 몰라도 됩니다. 다른 프로그래밍 언어와 도구도 사용할 수 있으며, 이 내용은 [시작: 프로그래밍 언어 선택](getting-started-choosing-a-programming-language.md)에서 살펴보겠습니다.

Windows 10에서는 데스크톱, 랩톱, 태블릿 및 휴대폰 장치 등에서 흥미로운 앱을 만들기 위한 새로운 플랫폼을 소개합니다. UWP(유니버설 Windows 플랫폼) 앱은 많은 고유한 기능을 제공하므로 iOS 앱을 직접 포팅하면 이러한 기능을 사용하지 못할 수 있습니다. 따라서 새 컨트롤 및 기능을 사용하여 개발자로서 생활을 더 편리하게 하고 새 앱을 가능하게 만드는 방법을 확인하는 것이 좋습니다.

앱을 포팅하지만 말고 **reimagine**하여 새로운 기능 및 새 장치를 활용하는 것이 좋습니다. 비슷한 기능에 안주하지 말고 라이브 타일, 알림, Cortana와의 상호 작용 등 고유한 Windows 10 기능을 사용하는 풍부한 환경을 만들어 보세요.

연습을 시작하려면 컴퓨터에 Windows 10과 Microsoft Visual Studio가 둘 다 설치되어 있어야 합니다. 이러한 프로그램은 [Windows 스토어 앱 프로그래밍을 위한 개발자 다운로드](https://developer.microsoft.com/en-us/windows/downloads)에서 다운로드할 수 있습니다. PC가 없는 경우 걱정하지 마세요. Mac을 사용할 수 있습니다. [Mac에 Windows 및 개발 도구 설치](setting-up-your-mac-with-windows-10.md)를 참조하세요.

| 항목 | 설명 |
|-------|-------------|
| [시작: 프로젝트 만들기](getting-started-creating-a-project.md) | Visual Studio는 Windows용이고 Xcode는 iOS 및 Mac OS용입니다. 이 연습에서는 Visual Studio를 익숙하게 사용할 수 있습니다. |
| [시작: 프로그래밍 언어 선택](getting-started-choosing-a-programming-language.md) | 이제 UWP 앱을 개발할 때 선택할 수 있는 프로그래밍 언어에 대해 알아야 합니다. |
| [시작: Visual Studio 둘러보기](getting-started-getting-around-in-visual-studio.md) | 이제 앞에서 만든 프로젝트로 돌아가서 Visual Studio IDE(통합 개발 환경)를 탐색하는 방법에 대해 알아보겠습니다. |
| [시작: 공용 컨트롤](getting-started-common-controls.md) | 앱에서 사용하는 몇 가지 일반적인 컨트롤과 이와 동일한 iOS 컨트롤은 다음과 같습니다. |
| [시작: 탐색](getting-started-navigation.md) | Windows 10 앱에서 이 탐색을 관리하는 방법 중 하나는 [Frame](https://msdn.microsoft.com/library/windows/apps/br242682) 클래스를 사용하는 것입니다. 다음 연습에서는 이 작업을 수행하는 방법을 보여 줍니다. |
| [시작: 애니메이션](getting-started-animation.md) | Windows 앱에서는 애니메이션을 프로그래밍 방식으로 만들 수 있지만 XAML(Extensible Application Markup Language)을 사용하여 선언적으로 정의할 수도 있습니다. |
| [시작: 다음에 할 일](getting-started-what-next.md) | 이제 이 기본 정보를 사용하여 보다 흥미로운 UWP(유니버설 Windows 플랫폼) 앱을 작성할 수 있습니다. 다음 단계를 위해 아래 항목을 읽고 Visual Studio를 실행하여 코드 작성을 시작하세요. |
| [Windows 앱 개념 매핑](https://msdn.microsoft.com//windows/uwp/porting/android-ios-uwp-map) | Windows(및 Android) 기능 측면에서 iOS 개념을 고려하는 방법 |

 

 

 