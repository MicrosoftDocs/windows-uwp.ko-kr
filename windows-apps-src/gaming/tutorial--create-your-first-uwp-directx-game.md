---
title: DirectX UWP(유니버설 Windows 플랫폼) 게임 만들기
description: 이 자습서 집합에서는 DirectX 및 [c + +/Winrt](../cpp-and-winrt-apis/index.md) 를 사용 하 여 **Simple3DGameDX**이라는 UWP (기본 유니버설 Windows 플랫폼) 샘플 게임을 만드는 방법에 대해 알아봅니다.
ms.assetid: 9edc5868-38cf-58cc-1fb3-8fb85a7ab2c9
keywords: DirectX 샘플 게임, 샘플 게임, 유니버설 Windows 플랫폼 (UWP), Direct3D 11 game
ms.date: 06/24/2020
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 284aa821cc58a49f45bed3b0d7e28c20f9d19ba1
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89163027"
---
# <a name="create-a-simple-universal-windows-platform-uwp-game-with-directx"></a>DirectX를 사용 하 여 단순 유니버설 Windows 플랫폼 (UWP) 게임 만들기

이 자습서 집합에서는 DirectX 및 [c + +/Winrt](../cpp-and-winrt-apis/index.md) 를 사용 하 여 **Simple3DGameDX**이라는 UWP (기본 유니버설 Windows 플랫폼) 샘플 게임을 만드는 방법에 대해 알아봅니다. 게임 플레이는 간단한 첫 번째 사용자 3D 촬영 갤러리에서 발생 합니다.

> [!NOTE]
> **Simple3DGameDX** 샘플 게임 자체를 다운로드할 수 있는 링크는 [Direct3D 샘플 게임](/samples/microsoft/windows-universal-samples/simple3dgamedx/)입니다. C + +/WinRT 소스 코드는 라는 폴더에 `cppwinrt` 있습니다. 다른 UWP 샘플 앱에 대 한 정보는 [uwp 앱 샘플 가져오기](../get-started/get-app-samples.md)를 참조 하세요.

이러한 자습서는 미술 및 메시와 같은 자산을 로드 하 고, 기본 게임 루프를 만들고, 간단한 렌더링 파이프라인을 구현 하 고, 사운드 및 컨트롤을 추가 하는 프로세스를 포함 하 여 게임의 주요 부분을 모두 포함 합니다.

UWP 게임 개발 기술 및 고려 사항도 확인할 수 있습니다. 핵심 UWP DirectX 게임 개발 개념에 초점을 맞춘 다음 이러한 개념에 대 한 Windows 런타임 관련 고려 사항을 호출 합니다.

## <a name="objective"></a>Objective

UWP DirectX 게임의 기본 개념 및 구성 요소에 대해 알아보고 DirectX를 사용 하 여 UWP 게임을 디자인 하는 것이 더 편리할 수 있습니다.

## <a name="what-you-need-to-know"></a>기억해야 하는 사항

이 자습서에서는 이러한 주제에 대해 잘 알고 있어야 합니다.

- [C + +/Winrt](../cpp-and-winrt-apis/index.md). C + +/WinRT는 표준 최신 c + + 17 언어 Windows 런타임 프로젝션 (WinRT) Api로, 헤더 파일 기반 라이브러리로 구현 되며 최신 Windows Api에 대 한 최고 수준의 액세스를 제공 하도록 설계 되었습니다.
- 기본 선형 대 수 및 Newtonian 물리학 개념.
- 기본 그래픽 프로그래밍 용어.
- 기본 Windows 프로그래밍 개념.
- [Direct2D](/windows/desktop/Direct2D/direct2d-portal) 및 [Direct3D 11](/windows/desktop/direct3d11/how-to-use-direct3d-11) api에 대 한 기본 지식.

##  <a name="direct3d-uwp-shooting-gallery-sample"></a>Direct3D UWP 해결 갤러리 샘플

**Simple3DGameDX** 샘플 게임은 최고 수준의 첫 번째 사용자 3d 촬영 갤러리를 구현 합니다. 여기서 플레이어는 이동 대상에서 구슬을 발생 시킵니다. 각 대상에 게는 집합의 점수를 표시 하 고, 플레이어는 6 가지 수준의 늘어난 챌린지를 진행할 수 있습니다. 수준 끝에서 점수가 계산 되 고 플레이어는 최종 점수를 획득 합니다.

이 샘플에서는 이러한 게임 개념을 보여 줍니다.

- DirectX 11.1와 Windows 런타임 간의 상호 운용
- 첫 번째 사용자 3D 큐브 뷰 및 카메라
- Stereoscopic 3D 효과
- 3D의 개체 간 충돌 감지
- 마우스, 터치 및 Xbox 컨트롤러 컨트롤에 대 한 플레이어 입력 처리
- 오디오 믹싱 및 재생
- 기본 게임 상태-컴퓨터

![작동 중인 샘플 게임](images/simple-dx-game-overview.png)

|항목|Description|
|-------|-------------|
|[게임 프로젝트 설정](tutorial--setting-up-the-games-infrastructure.md)|게임을 개발 하는 첫 번째 단계는 Microsoft Visual Studio에서 프로젝트를 설정 하는 것입니다. 게임 개발을 위해 특별히 프로젝트를 구성한 후 나중에 해당 프로젝트를 일종의 템플릿으로 다시 사용할 수 있습니다.|
|[게임의 UWP 앱 프레임워크 정의](tutorial--building-the-games-uwp-app-framework.md)|UWP (유니버설 Windows 플랫폼) 게임을 코딩 하는 첫 번째 단계는 앱 개체가 Windows와 상호 작용할 수 있도록 하는 프레임 워크를 구축 하는 것입니다.|
|[게임 흐름 관리](tutorial-game-flow-management.md)|플레이어 및 시스템 상호 작용을 가능 하 게 하는 상위 수준 상태 시스템을 정의 합니다. UI가 전체 게임의 상태 시스템과 상호 작용 하는 방법 및 UWP 게임에 대 한 이벤트 처리기를 만드는 방법에 대해 알아봅니다.|
|[주 게임 개체 정의](tutorial--defining-the-main-game-loop.md)|이제 샘플 게임의 주 개체에 대 한 세부 정보 및 구현 하는 규칙이 게임 세계와의 상호 작용으로 변환 되는 방식을 살펴보겠습니다.|
|[렌더링 프레임 워크 I: 렌더링 소개](tutorial--assembling-the-rendering-pipeline.md)|렌더링 파이프라인을 개발 하 여 그래픽을 표시 하는 방법을 알아봅니다. 렌더링을 소개 합니다.|
|[렌더링 프레임 워크 II: 게임 렌더링](tutorial-game-rendering.md)|렌더링 파이프라인을 조합 하 여 그래픽을 표시 하는 방법을 알아봅니다. 게임 렌더링, 데이터 설정 및 준비|
|[사용자 인터페이스 추가](tutorial--adding-a-user-interface.md)|DirectX UWP 게임에 2D 사용자 인터페이스 오버레이를 추가 하는 방법에 대해 알아봅니다.|
|[컨트롤 추가](tutorial--adding-controls.md)|이제 샘플 게임에서 3 차원 게임의 이동 모양 컨트롤을 구현 하는 방법과 기본적인 터치, 마우스 및 게임 컨트롤러 컨트롤을 개발 하는 방법을 살펴보겠습니다.|
|[소리 추가](tutorial--adding-sound.md)|XAudio2 Api를 사용 하 여 게임 음악과 음향 효과를 재생 하는 간단한 사운드 엔진을 개발 합니다.|
|[샘플 게임 확장](tutorial-resources.md)|UWP DirectX 게임에 대해 XAML 오버레이를 구현 하는 방법에 대해 알아봅니다.|