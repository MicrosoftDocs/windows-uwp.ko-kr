---
ms.assetid: 08C8F359-E8B6-4A45-8F4B-8A1962F0CE38
description: Microsoft Visual Studio는 Windows용이고 Xcode는 iOS 및 Mac OS용입니다. 이 연습에서는 Visual Studio를 익숙하게 사용할 수 있습니다.
title: Visual Studio에서 프로젝트 만들기
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 1b6ea9fdf2e504e1ceee71658eab308751e1745c
ms.sourcegitcommit: 681c70f964210ab49ac5d06357ae96505bb78741
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/27/2018
ms.locfileid: "7716937"
---
# <a name="getting-started-creating-a-project"></a>시작: 프로젝트 만들기

## <a name="creating-a-project"></a>프로젝트 만들기

Microsoft Visual Studio는 Windows용이고 Xcode는 iOS 및 Mac OS용입니다. 이 연습에서는 Visual Studio를 익숙하게 사용할 수 있습니다. 또한 시작하기 위해 알고 있어야 하는 기본 사항을 보여 줍니다. 앱을 만들 때마다 다음과 같은 단계를 수행합니다.

다음 동영상에서는 Xcode와 Visual Studio를 비교합니다.

> [!VIDEO https://channel9.msdn.com/Blogs/One-Dev-Minute/Comparing-Xcode-to-Visual-Studio/player]

또한 이 [Windows용 앱 빌드 블로그 게시물](https://blogs.windows.com/buildingapps/2016/01/27/visual-studio-walkthrough-for-ios-developers/)은 매우 유용합니다.

Windows10 (공식적으로 유니버설 Windows 플랫폼 (UWP) 앱 이라고 함)에 대 한 앱을 만드는 것이 좋습니다 스토리 보드를 사용 하 여 iOS 앱을 작성 합니다. Windows10 앱은 종종 여러 페이지로 생성 되며, 각 페이지에는 웹 사이트와 같이 사용자 인터페이스의 다른 부분을 포함 합니다. 각 페이지에는 주로 두 개의 연결된 소스 파일이 있습니다. 하나는 [XAML 개요](https://msdn.microsoft.com/library/windows/apps/mt185595) 형식의 사용자 인터페이스를 저장하고, 다른 하나는 소스 코드(대개 C#)를 포함합니다. 사용자가 앱을 조작하면 이러한 페이지 간을 탐색합니다. 이 연습에서는 페이지가 두 개인 앱을 만듭니다.

**참고**Windows10 앱의 중요 한 기능은 있다는 동일한 소스 코드와 동일한 API 집합입니다 플랫폼에 관계 없이 사용할 수 있습니다. iPhone 및 iPad용 유니버설 iOS 앱을 작성할 때는 런타임에 앱이 실행되는 플랫폼을 확인하여 적절히 조치할 수 있습니다. 비슷한 방식 Windows10 앱 알 수, 실행 시에서 실행 되는 장치. UWP 앱을 사용하면 휴대폰 빌드와 데스크톱 빌드를 만들기 위해 소스 코드에 \#ifdef를 사용할 필요가 없습니다. Windows10 앱도 지능적으로 사용 장치에 따라 사용자 인터페이스 컨트롤: 예를 들어 앱 날짜 선택 컨트롤을 참조할 수 있으며 컨트롤은 자동으로 모양과 있는지 여부에 따라 다르게 기능 데스크톱 또는 휴대폰 화면에서 실행 합니다. 그러나 소스 코드는 동일합니다.

Windows10 앱을 만드는 방법을 살펴보겠습니다. 먼저 Visual Studio를 실행합니다. 처음 실행하면 Visual Studio에서 개발자 라이선스가 필요하다는 메시지를 표시합니다. UWP 앱을 Microsoft Store에 제출하기 전에 개발자 라이선스를 사용하여 로컬 컴퓨터에 설치한 후 테스트할 수 있습니다. 라이선스를 가져오려면 화면의 지시에 따라 Microsoft 계정으로 로그인합니다. Microsoft 계정이 없을 경우 **개발자 라이선스** 대화 상자에서 **등록** 링크를 클릭한 다음 화면의 지시를 따릅니다.

반면에 Xcode를 시작하면 처음에 다음과 같은 **Xcode 시작** 화면이 표시됩니다.

![Xcode 시작 화면](images/ios-to-uwp/ios-to-uwp-xcode-welcome.png)

Visual Studio도 매우 유사합니다. 다음 그림과 같은 **시작 페이지**가 표시됩니다.

![Visual Studio 시작 화면](images/ios-to-uwp/ios-to-uwp-vs-welcome.png)

새 앱을 만들려면 다음 중 하나를 수행하여 프로젝트를 만듭니다.

-   **시작** 영역에서 **새 프로젝트**를 탭합니다.
-   **파일** 메뉴를 탭한 다음 **새 프로젝트**를 탭합니다.

반면에 Xcode에서 새 프로젝트를 만들면 다음과 같은 프로젝트 템플릿 목록이 표시됩니다.

![Xcode 새 프로젝트 대화 상자](images/ios-to-uwp/ios-to-uwp-xcode-choose-template.png)

Visual Studio에는 다음 그림과 같은 몇 가지 프로젝트 템플릿이 있습니다.

![visual studio 새 프로젝트 대화 상자 ](images/ios-to-uwp/ios-to-uwp-vs-choose-template.png)이 연습에서는 **Visual C#** 을 탭한 후 **Windows**, **Windows 유니버설** 및 **비어 있는 앱(Windows 유니버설)** 을 차례로 탭합니다. **이름** 상자에 "MyApp"을 입력하고 **확인**을 탭합니다. Visual Studio에서 개발자의 첫 번째 프로젝트를 만든 다음 표시합니다. 이제 앱을 디자인하여 코드를 추가할 수 있습니다.

## <a name="next-step"></a>다음 단계

[시작: 프로그래밍 언어 선택](getting-started-choosing-a-programming-language.md)
