---
ms.assetid: 08C8F359-E8B6-4A45-8F4B-8A1962F0CE38
description: Xcode는 iOS 및 Mac OS에 대 한 것 이므로 Windows를 Microsoft Visual Studio 합니다. 이 연습에서는 Visual Studio를 사용 하는 데 도움이 됩니다.
title: Visual Studio에서 프로젝트 만들기
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 6be772573321e7f1cbf5e5861d5f7b15a1ba5183
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89174897"
---
# <a name="getting-started-creating-a-project"></a>시작: 프로젝트 만들기

## <a name="creating-a-project"></a>프로젝트 만들기

Xcode는 iOS 및 Mac OS에 대 한 것 이므로 Windows를 Microsoft Visual Studio 합니다. 이 연습에서는 Visual Studio를 사용 하는 데 도움이 됩니다. 시작 하기 위해 알아야 할 절대 기본 사항을 보여 줍니다. 앱을 만들 때마다 이와 유사한 단계를 수행 합니다.

다음 비디오는 Xcode와 Visual Studio를 비교 합니다.

> [!VIDEO https://channel9.msdn.com/Blogs/One-Dev-Minute/Comparing-Xcode-to-Visual-Studio/player]

또한 [Windows 용 앱 빌드 블로그 게시가](https://blogs.windows.com/buildingapps/2016/01/27/visual-studio-walkthrough-for-ios-developers/) 매우 유용 합니다.

Windows 유니버설 Windows 플랫폼 10 용 응용 프로그램을 만드는 것은 Storyboard를 사용 하 여 iOS 앱을 만드는 것입니다. Windows 10 앱은 종종 웹 사이트와 같이 사용자 인터페이스의 다른 부분을 포함 하는 여러 페이지에 걸쳐 생성 됩니다. 각 페이지에는 일반적으로 두 개의 연결 된 소스 파일이 있습니다. 하나는 [XAML 개요](../xaml-platform/xaml-overview.md) 형식으로 사용자 인터페이스를 저장 하 고 다른 하나는 c #을 포함 하는 소스 코드입니다. 사용자가 앱과 상호 작용 하면 이러한 페이지 간에 이동 합니다. 이 연습에서는 두 페이지를 사용 하 여 앱을 만듭니다.

**참고**    Windows 10 앱의 중요 한 기능은 플랫폼에 관계 없이 동일한 소스 코드와 동일한 API 집합을 사용할 수 있다는 사실입니다. 아시다시피 iPhone 및 iPad 용 유니버설 iOS 앱을 작성할 때 앱이 실행 되는 플랫폼을 런타임에 확인 하 고 적절 한 조치를 취할 수 있습니다. 이와 비슷한 방식으로 Windows 10 앱은 런타임에 실행 중인 장치를 런타임에 알려줄 수 있습니다. UWP 앱을 사용 하면 \# 소스 코드에서 ifdef를 사용 하 여 휴대폰 및 데스크톱 빌드를 만들 필요가 없습니다. 또한 Windows 10 앱은 장치에 따라 사용자 인터페이스 컨트롤을 지능적으로 사용 합니다. 예를 들어 앱에서 날짜 선택 컨트롤을 참조할 수 있으며, 컨트롤이 데스크톱 또는 휴대폰 화면에서 실행 되는지에 따라 자동으로 다르게 표시 되 고 작동 합니다. 그러나 소스 코드는 동일 하 게 유지 됩니다.

Windows 10 앱을 만들 수 있는 방법을 알아보겠습니다. Visual Studio를 실행 하 여 시작 합니다. 처음 실행 하는 경우 Visual Studio는 개발자 라이선스를 받을 것을 요청 합니다. 개발자 라이선스를 사용 하면 Microsoft Store에 제출 하기 전에 로컬 컴퓨터에서 UWP 앱을 설치 하 고 테스트할 수 있습니다. 라이선스를 얻으려면 화면의 지시에 따라 Microsoft 계정를 사용 하 여 로그인 합니다. 없는 경우 **개발자 라이선스** 대화 상자에서 **등록** 링크를 클릭 하 고 화면의 지시를 따릅니다.

비교를 위해 Xcode를 시작 하면 다음 그림과 비슷한 **Xcode 시작** 화면이 표시 됩니다.

![xcode 시작 화면](images/ios-to-uwp/ios-to-uwp-xcode-welcome.png)

Visual Studio는 매우 유사 합니다. 다음 그림에 표시 된 것 처럼 **시작 페이지가**표시 됩니다.

![visual studio 시작 화면](images/ios-to-uwp/ios-to-uwp-vs-welcome.png)

새 앱을 만들려면 먼저 다음 중 하나를 수행 하 여 프로젝트를 만듭니다.

-   **시작** 영역에서 **새 프로젝트**를 누릅니다.
-   **파일** 메뉴를 탭 한 다음 **새 프로젝트**를 누릅니다.

비교를 위해 Xcode에서 새 프로젝트를 만들 때 다음 그림에 표시 된 것과 같은 프로젝트 템플릿 목록이 표시 됩니다.

![xcode 새 프로젝트 대화 상자](images/ios-to-uwp/ios-to-uwp-xcode-choose-template.png)

Visual Studio에는 다음 그림에 표시 된 것 처럼 몇 가지 프로젝트 템플릿도 선택할 수 있습니다.

![visual studio 새 프로젝트 대화 상자 ](images/ios-to-uwp/ios-to-uwp-vs-choose-template.png) 이 연습에서는 **visual c #** 을 탭 한 다음 **windows**, **windows 유니버설** 및 **비어 있는 앱 (windows 유니버설)** 을 탭 합니다. **이름** 상자에 "MyApp"를 입력 한 다음 **확인**을 탭 합니다. Visual Studio에서 첫 번째 프로젝트를 만든 다음 표시 합니다. 이제 앱 디자인을 시작 하 고 여기에 코드를 추가할 수 있습니다.

## <a name="next-step"></a>다음 단계

[시작: 프로그래밍 언어 선택](getting-started-choosing-a-programming-language.md)