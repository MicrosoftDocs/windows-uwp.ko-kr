---
Search.SourceType: Video
title: iOS에서 UWP로 이동
description: IOS 개발자가 Windows 10 및 UWP (유니버설 Windows 플랫폼)를 포함 하도록 사용자 기반을 확장 하는 데 사용할 수 있는 도구에 대해 알아봅니다.
ms.assetid: 7a05751d-02df-4240-9ba5-d95f65a7a9c5
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 2a420ae0d34548e4e0e1e14f820d191b4d27f54a
ms.sourcegitcommit: afc4ff2c89f148d32073ab1cc42063ccdc573a8c
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 01/11/2021
ms.locfileid: "98104494"
---
# <a name="move-from-ios-to-uwp"></a>iOS에서 UWP로 이동

Windows 10 및 유니버설 Windows 플랫폼 (UWP)를 포함 하도록 사용자 기반을 확장 하는 방법에 대 한 자세한 내용은 iOS 개발자에 게 도움이 되는 다양 한 도구가 있습니다. 수행할 수 있는 방법은 작업 중인 앱의 유형 (게임, 라이프 스타일, 엔터프라이즈 등) 및 개발 프로세스에 있는 정도에 따라 달라 집니다. 예를 들어 OpenGL 또는 Cocos2D에 크게 의존 하는 완료 된 게임 또는 거의 완료 된 게임은 [iOS 용 Windows 브리지](https://github.com/microsoft/WinObjC)를 사용 하는 것이 적합 한 반면, 소규모 비즈니스에 대 한 플랫폼 간 앱을 계획 하는 경우 [xamarin.ios](/xamarin/xamarin-forms/)를 사용 하는 것을 고려해 야 합니다. Unity와 같은 플랫폼 간 도구에서 앱을 작성 한 경우 [Windows에 게시](https://blogs.unity3d.com/2015/09/09/windows-10-universal-apps-in-unity-5-2/) 하는 것은 매우 간단 합니다.

## <a name="why-windows"></a>Windows?

현재 Windows는 상당한 수의 장치에서 실행 되 고 있습니다. UWP는 개발자에 게 데스크톱 컴퓨터, 게임 콘솔 및 holographic 표시와 같이 다양 한 장치에서 원활 응답성 사용자 인터페이스를 만들도록 설계 된 최신 Api 집합을 제공 합니다. 단일 Visual Studio 솔루션을 사용 하 고 여러 플랫폼에서 자동으로 최적화 하기에 충분 한 스마트 인 사용자 인터페이스 컨트롤을 사용 하 여 더 작은 코드를 작성 하 고 더 많은 하드웨어에서 실행 하는 경우가 많습니다.

> [!VIDEO https://channel9.msdn.com/Blogs/One-Dev-Minute/How-to-Convert-your-iOS-app-to-Windows/player]

| 항목 | 설명 |
|-------|-------------|
| [iOS 및 UWP 앱 개발 방법 선택](selecting-an-approach-to-ios-and-uwp-app-development.md) | 플랫폼 간 앱을 개발할 때 선택할 수 있는 항목은 무엇 인가요? |
| [iOS 개발자용 UWP 시작](getting-started-with-uwp-for-ios-developers.md) | Windows 10 용 개발을 고려 하는 iOS 개발자 라면 이러한 문서를 시작 하는 것이 좋습니다. |
| [Windows 10에서 Mac 설정](setting-up-your-mac-with-windows-10.md) | 현재 Mac 컴퓨터를 사용 하 여 Windows 용 앱을 개발할 수 있습니다. |

## <a name="related-topics"></a>관련 항목

**디자이너 및 개발자 용**
* [모든 Windows 장치용 유니버설 Windows 앱 빌드](../get-started/universal-application-platform-guide.md)
* [UWP 앱에 대 한 디자인 자산 다운로드](../design/downloads/index.md)