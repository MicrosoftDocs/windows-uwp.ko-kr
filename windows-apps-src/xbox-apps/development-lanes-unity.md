---
title: Xbox에서 UWP로 Unity 게임 가져오기
description: UWP (유니버설 Windows 플랫폼) Visual Studio 솔루션에서 Unity 게임을 빌드 및 배포 하는 방법을 알아봅니다.
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.assetid: fca3267a-0c0f-4872-8017-90384fb34215
ms.localizationpriority: medium
ms.openlocfilehash: 5b985372c52711dd7b6b4f865ab5164e25c88e0c
ms.sourcegitcommit: e273e5901bfa6596dfef4cc741bb1c42614c25ab
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 09/01/2020
ms.locfileid: "89238328"
---
# <a name="bringing-unity-games-to-uwp-on-xbox"></a>Xbox에서 UWP로 Unity 게임 가져오기


이 단계별 자습서에서는 이미 Unity에서 게임을 빌드하고 배포할 준비가 되었다고 가정 합니다.

[이 자습서의 비디오 버전](https://www.youtube.com/watch?v=f0Ptvw7k-CE)도 참조 하세요.

Unity UWP 프로젝트 버전을 찾으십니까? [UWP 프로젝트 버전 제어를](development-lanes-unity-versioning.md)참조 하세요.

## <a name="step-0-ensure-unity-is-installed-correctly"></a>0 단계: Unity가 올바르게 설치 되었는지 확인

Unity를 설치 하는 경우 다음 구성 요소를 선택 해야 합니다.

![Unity 설치 구성 요소](images/unity-install-components.png)

## <a name="step-1-building-the-uwp-solution"></a>1 단계: UWP 솔루션 빌드

Unity 게임 프로젝트에서 **파일 > 빌드 설정**에 있는 **빌드 설정** 창을 열고 Microsoft Store 옵션 메뉴로 이동 합니다.

![빌드 설정 창](images/build-settings.png)

**SDK** 설정이 **유니버설 10**으로 설정 되어 있는지 확인 한 다음, **빌드** 단추를 선택 하면 대상 폴더를 요청 하는 파일 탐색기 창이 시작 됩니다. 프로젝트의 **자산** 디렉터리 옆에 **UWP** 라는 폴더를 만들고,이 폴더를 빌드의 대상 폴더로 선택 합니다.

![빌드 대상 폴더](images/build-destination.png)

이제 Unity는에서 UWP 게임을 배포 하는 데 사용할 새 Visual Studio 솔루션을 만들었습니다.

![UWP VS 솔루션](images/uwp-vs-solution.png)

## <a name="step-2-deploying-your-game"></a>2 단계: 게임 배포

**UWP** 폴더에서 새로 생성 된 솔루션을 열고 대상 플랫폼을 x **64**로 변경 합니다.

![x64 빌드 플랫폼](images/x64-build-platform.png)

이제 게임을 위한 UWP Visual Studio 솔루션이 있으므로 [이러한 단계를 수행](getting-started.md) 하면 소매 Xbox one에 게임을 성공적으로 배포할 수 있습니다.

## <a name="step-3-modify-and-rebuild"></a>3 단계: 수정 및 다시 빌드

스크립트가 아닌 모든 항목을 변경 하는 경우 이러한 변경 내용이 게임의 UWP 빌드에 표시 되려면 __1 단계__에서 설명한 대로 편집기 내에서 프로젝트를 다시 빌드해야 합니다.

## <a name="versioning-your-uwp-project"></a>UWP 프로젝트 버전 관리

새로 생성 된 UWP 디렉터리의 일부를 버전 제어에 추가 하는 일반적인 상황이 있습니다. 예를 들어 UWP 프로젝트에 새 종속성을 추가 하는 경우 (예: Xbox Live SDK)  이 예제에서는 [UWP 프로젝트 버전 제어](development-lanes-unity-versioning.md)에 대해 자세히 설명 합니다.

## <a name="see-also"></a>참고 항목
- [기존 게임을 Xbox로 가져오기](development-lanes-landing.md)
- [Xbox One의 UWP](index.md)
