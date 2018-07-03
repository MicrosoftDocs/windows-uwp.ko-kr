---
author: joannaleecy
title: DirectX UWP(유니버설 Windows 플랫폼) 게임 만들기
description: 이 자습서 집합에서는 DirectX 및 C++를 사용하여 기본 UWP(유니버설 Windows 플랫폼) 게임을 만드는 방법을 알아봅니다.
ms.assetid: 9edc5868-38cf-58cc-1fb3-8fb85a7ab2c9
keywords: DirectX 게임 샘플, 게임 샘플, UWP(유니버설 Windows 플랫폼), Direct3D 11 게임
ms.author: joanlee
ms.date: 12/01/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
ms.localizationpriority: medium
ms.openlocfilehash: c043b20cb00873bf115ff2d65306bc727d23a02a
ms.sourcegitcommit: 4b6c197e1567d86e19af3ab5da516c022f1b6dfb
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 05/11/2018
ms.locfileid: "1877225"
---
# <a name="create-a-simple-universal-windows-platform-uwp-game-with-directx"></a>DirectX로 작성된 간단한 UWP(유니버설 Windows 플랫폼) 게임 만들기

이 자습서 집합에서는 DirectX 및 C++를 사용하여 기본 UWP(유니버설 Windows 플랫폼) 게임을 만드는 방법을 알아봅니다. 아트 및 메시 같은 자산 로드, 주 게임 루프 만들기, 간단한 렌더링 파이프라인 구현, 사운드 및 컨트롤 추가 등에 대한 프로세스를 포함하여 게임의 모든 주요 부분에 대해 다루고,

UWP 게임 개발 기술 및 고려 사항에 대해서도 설명합니다. 단, 완벽한 게임을 제공하지는 않습니다. 대신 주요 UWP DirectX 게임 개발 개념에 중점을 두고 이 개념과 관련하여 Windows 런타임에 대한 고려 사항을 찾아냅니다.

## <a name="objective"></a>목표

UWP DirectX 게임의 기본 개념 및 구성 요소를 사용하고 DirectX로 작성된 UWP 게임 디자인에 보다 익숙해집니다.

## <a name="what-you-need-to-know-before-starting"></a>시작하기 전에 알아야 할 사항


이 자습서를 시작하기 전에 다음 주제에 대해 잘 알고 있어야 합니다.

-   Windows 런타임 언어 확장(C++/CX)을 사용하는 Microsoft C++입니다. 자동 참조 카운트를 통합하는 Microsoft C++의 업데이트이며 DirectX 11.1 이상 버전을 사용하여 UWP 게임을 개발하기 위한 언어입니다.
-   기본 선형 대수 및 뉴턴 물리학 개념
-   기본 그래픽 프로그래밍 용어
-   기본 Windows 프로그래밍 개념
-   [Direct2D](https://msdn.microsoft.com/library/windows/apps/dd370990.aspx) 및 [Direct3D 11](https://msdn.microsoft.com/library/windows/desktop/hh404569) API에 대한 기본적인 지식.

##  <a name="direct3d-uwp-shooting-game-sample"></a>Direct3D UWP 슈팅 게임 샘플


이 샘플은 플레이어가 움직이는 대상을 향해 공을 발사하는 간단한 1인칭 슈팅 갤러리를 구현합니다. 각 대상을 맞추면 설정된 점수가 부여되고 플레이어는 목표를 높이면서 6개 레벨을 진행할 수 있습니다. 레벨이 완료되면 점수가 기록되고 플레이어에게는 최종 점수가 부여됩니다.

이 샘플에서는 다음과 같은 게임 개념을 보여 줍니다.

-   DirectX 11.1과 Windows 런타임 간 상호 운용
-   1인칭 3D 원근 및 카메라
-   입체 3D 효과
-   3D의 개체 간 충돌 감지
-   마우스, 터치 및 Xbox 컨트롤러 컨트롤로 플레이어 입력 처리
-   오디오 믹싱 및 재생
-   기본 게임 상태 시스템

![실행 중인 게임 샘플](images/simple-dx-game-overview.png)

| 항목 | 설명 |
|-------|-------------|
|[게임 프로젝트 설정](tutorial--setting-up-the-games-infrastructure.md) | 게임을 어셈블하는 첫 번째 단계는 Microsoft Visual Studio에서 수행해야 하는 코드 인프라 작업의 양을 최소화하는 방식으로 프로젝트를 설정하는 것입니다. 적합한 템플릿을 사용하고 게임 개발에 맞게 특별히 프로젝트를 구성하면 시간을 절약하고 혼동을 줄일 수 있습니다. 여기서는 간단한 게임 프로젝트의 설정 및 구성 과정을 안내합니다. |
| [게임의 UWP 앱 프레임워크 정의](tutorial--building-the-games-uwp-app-framework.md) | UWP DirectX 게임 개체가 Windows와 상호 작용하는 수 있도록 해주는 프레임워크를 빌드합니다. 일시 중단/재개 이벤트 처리, 창 포커스, 끌기 같은 Windows 런타임 속성이 포함됩니다.  |
| [게임 흐름 관리](tutorial-game-flow-management.md) | 플레이어와 시스템 상호 작용이 가능하도록 고급 상태 시스템을 정의합니다. UI가 전체 게임의 상태 시스템과 상호 작용하는 방법과 UWP 게임에 대한 처리기 이벤트를 생성하는 방법을 자세히 알아보세요. |
| [주 게임 개체 정의](tutorial--defining-the-main-game-loop.md) | 규칙을 만들어 게임을 실행하는 방법을 정의합니다. |
| [렌더링 프레임워크 I: 렌더링 소개](tutorial--assembling-the-rendering-pipeline.md) | 그래픽을 표시할 렌더링 프레임워크를 어셈블합니다. 이 항목은 두 부분으로 나누어져 있습니다. 렌더링 소개 화면에서는 장면 개체를 화면 상의 디스플레이에 표시하는 방법을 설명합니다. |
| [렌더링 프레임워크 II: 게임 렌더링](tutorial-game-rendering.md) | 렌더링 항목의 두 번째 파트에서는 렌더링 수행 전에 필요한 데이터를 준비하는 방법을 알아봅니다. |
| [사용자 인터페이스 추가](tutorial--adding-a-user-interface.md) | 간단한 메뉴 옵션과 HUD(주의 표시) 구성 요소를 추가하여 플레이어에게 피드백을 제공합니다. |
| [컨트롤 추가](tutorial--adding-controls.md) | 게임&mdash; 기본 터치, 마우스 및 게임 컨트롤러 컨트롤에 이동 보기 컨트롤을 추가합니다. |
| [소리 추가](tutorial--adding-sound.md) | [XAudio2](https://msdn.microsoft.com/library/windows/desktop/ee415813) API를 사용하여 게임에 대한 소리를 만드는 방법을 자세히 알아보세요. |
| [게임 샘플 확장](tutorial-resources.md) | XAML을 사용한 오버레이 생성을 포함해 DirectX 게임 개발에 대한 정보를 넓힐 수 있는 리소스입니다. |