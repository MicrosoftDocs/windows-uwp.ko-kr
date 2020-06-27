---
title: '*Marble Maze* &mdash; C + +를 사용 하 여 c + +를 사용 하 여 만든 대리석 x a 유니버설 Windows 플랫폼 (UWP) 게임'
description: 설명서의이 섹션에서는 DirectX 및 c + +를 사용 하 여 UWP (3D 유니버설 Windows 플랫폼) 게임을 만드는 방법을 설명 합니다.
ms.assetid: 43f1977a-7e1d-614c-696e-7669dd8a9cc7
ms.date: 08/10/2017
ms.topic: article
keywords: windows 10, uwp, 게임, 샘플, directx, 3d
ms.localizationpriority: medium
ms.openlocfilehash: d2c0c630090c178a54a0452ab3cc430ffee4a176
ms.sourcegitcommit: 20969781aca50738792631f4b68326f9171a3980
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 06/26/2020
ms.locfileid: "85409502"
---
# <a name="developing-marble-mazemdasha-universal-windows-platform-uwp-game-built-with-c-for-directx"></a>*Marble Maze* &mdash; C + +를 사용 하 여 c + +를 사용 하 여 만든 대리석 x a 유니버설 Windows 플랫폼 (UWP) 게임

이 항목에서는 DirectX 및 c + +를 사용 하 여 UWP (3D 유니버설 Windows 플랫폼) 게임을 만드는 방법에 대해 설명 합니다. *대리석*이라는 게임은 태블릿, 기존 데스크톱 pc 및 노트북 pc와 같은 여러 폼 팩터를 수용 합니다.

> [!NOTE]
> *대리석* 의 소스 코드를 다운로드 하려면 [GitHub의 샘플](https://github.com/microsoft/Windows-appsample-marble-maze)을 참조 하세요.

> [!IMPORTANT]
> *대리석 미로* 는 UWP 게임을 만들기 위한 모범 사례를 고려 하는 디자인 패턴을 보여 줍니다. 사용자 고유의 사례와 개발 중인 게임의 고유한 요구 사항에 맞게 많은 구현 세부 정보를 적용할 수 있습니다. 사용자의 요구 사항에 더 적합 한 다른 기술 또는 라이브러리를 자유롭게 사용할 수 있습니다. 그러나 코드에서 항상 [Windows 앱 인증 키트](https://docs.microsoft.com/windows/uwp/debug-test-perf/windows-app-certification-kit)를 전달 하는지 확인 합니다. 성공적인 게임 개발을 위해 여기에 사용 된 구현을 고려해 야 하는 경우이 문서에서이를 강조 합니다.

## <a name="introducing-marble-maze"></a>*대리석 무늬 미로* 소개

일반적으로는 기본으로 제공 되지만 대부분의 게임에서 발견 되는 다양 한 기능을 보여 주므로 *대리석* 을 선택 했습니다. 그래픽, 입력 처리 및 오디오를 사용 하는 방법을 보여 줍니다. 또한 규칙 및 목표와 같은 게임 메커니즘을 보여 줍니다.

*대리석* 은 일반적으로 구멍 및 강철 또는 유리 대리석을 포함 하는 상자에서 생성 되는 표-상위 labyrinth 게임과 비슷합니다. *대리석 자 메 이즈* 의 목표는 테이블-상위 버전과 동일 합니다. 미로로 이동 하 여 대리석이 구멍 중 하나에 포함 되지 않도록 하는 것이 좋습니다. *대리석* 은 검사점의 개념을 추가 합니다. 대리석이 구멍이 면 대리석이 전달 된 마지막 검사점 위치에서 게임이 다시 시작 됩니다.

*대리석* 은 사용자가 게임 보드와 상호 작용할 수 있는 여러 가지 방법을 제공 합니다. 터치 사용 또는가 속도계 사용 장치를 사용 하는 경우 해당 장치를 사용 하 여 게임 보드를 이동할 수 있습니다. Xbox One 컨트롤러 또는 마우스를 사용 하 여 게임 플레이를 제어할 수도 있습니다.

![대리석 무늬 메 이즈 게임의 스크린샷.](images/marblemaze-2.png)

## <a name="prerequisites"></a>전제 조건

-   Windows 10 크리에이터스 업데이트
-   [Microsoft Visual Studio 2017](https://visualstudio.microsoft.com/downloads/)
-   C + + 프로그래밍 기술 자료
-   DirectX 및 DirectX 용어에 대 한 지식
-   COM에 대 한 기본 지식

## <a name="who-should-read-this"></a>이 문서의 대상

3D 게임 또는 Windows 10 용 그래픽 집약적 응용 프로그램을 만드는 데 관심이 있는 경우에는이 작업이 필요 합니다. 이 문서에서 설명 하는 원칙과 관행을 사용 하 여 사용자 고유의 UWP 게임을 만드는 것이 좋습니다. C + + 및 DirectX 프로그래밍에 대 한 배경 지식이 나 강한 관심은이 설명서를 최대한 활용 하는 데 도움이 됩니다. DirectX를 사용 하는 경험이 없는 경우에도 유사한 3D 그래픽 프로그래밍 환경을 사용 하는 경우 혜택을 받을 수 있습니다.

문서 [연습: directx를 사용 하 여 간단한 UWP 게임 만들기](tutorial--create-your-first-uwp-directx-game.md) 는 Directx 및 c + +를 사용 하 여 기본 3d 촬영 게임을 구현 하는 또 다른 샘플을 설명 합니다.

## <a name="what-this-documentation-covers"></a>이 설명서에 포함된 내용

이 설명서에서는 다음 방법에 대해 설명 합니다.

-   Windows 런타임 API 및 DirectX를 사용 하 여 UWP 게임을 만듭니다.
-   [Direct3D](https://docs.microsoft.com/windows/desktop/direct3d11/atoc-dx-graphics-direct3d-11) 및 [Direct2D](https://docs.microsoft.com/windows/desktop/Direct2D/direct2d-portal) 를 사용 하 여 모델, 질감, 꼭 짓 점 및 픽셀 셰이더, 2d 오버레이 등의 시각적 콘텐츠로 작업할 수 있습니다.
-   Touch,가 속도계 및 Xbox One controller와 같은 입력 메커니즘을 통합 합니다.
-   [XAudio2](https://docs.microsoft.com/windows/desktop/xaudio2/xaudio2-apis-portal) 를 사용 하 여 음악과 음향 효과를 통합 합니다.

## <a name="what-this-documentation-does-not-cover"></a>이 설명서에서 다루지 않는 내용

이 설명서에서는 게임 개발의 다음과 같은 측면에 대해 다루지 않습니다. 이러한 측면에는 이러한 측면을 포함 하는 추가 리소스가 있습니다.

-   3D 게임 디자인 원칙.
-   C + + 또는 DirectX 프로그래밍 기본 사항입니다.
-   질감, 모델 또는 오디오와 같은 리소스를 디자인 하는 방법입니다.
-   게임의 동작 또는 성능 문제를 해결 하는 방법입니다.
-   전 세계의 다른 부분에서 사용 하기 위해 게임을 준비 하는 방법을 설명 합니다.
-   게임을 인증 하 고 Microsoft Store에 게시 하는 방법입니다.

또한 *대리석 메 이즈* 는 [directxmath](https://docs.microsoft.com/windows/desktop/dxmath/directxmath-portal) 라이브러리를 사용 하 여 3d 기 하 도형으로 작업 하 고 충돌과 같은 물리학 계산을 수행 합니다. DirectXMath는이 섹션에서 자세히 다루지 않습니다. *대리석* 에서 DirectXMath를 사용 하는 방법에 대 한 자세한 내용은 소스 코드를 참조 하세요.

*대리석* 은 재사용 가능한 많은 구성 요소를 제공 하지만 완전 한 게임 개발 프레임 워크는 아닙니다. 게임에서 대리석을 다시 사용할 수 있도록 하는 *대리석* 을 고려 하면 설명서에서이를 강조 합니다.

## <a name="next-steps"></a>다음 단계

대리석으로 시작 하는 것은 대리석의 [샘플](marble-maze-sample-fundamentals.md) 로 시작 하는 것이 좋습니다. 여기서는 대리석의 *대리석 구조와* *대리석* 의 일부 코딩 및 스타일 지침에 대해 알아봅니다. 다음 표에서는이 섹션의 문서를 간략하게 설명 하므로 쉽게 참조할 수 있습니다.

## <a name="in-this-section"></a>단원 내용

| 제목                                                                                                                    | 설명                                                                                                                                                                                                                                        |
|--------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| [Marble Maze 샘플 기본 사항](marble-maze-sample-fundamentals.md)                                                   | 게임 구조와 소스 코드가 따르는 코드 및 스타일 지침의 개요를 제공 합니다.                                                                                                                                 |
| [Marble Maze 응용 프로그램 구조](marble-maze-application-structure.md)                                               | *대리석* 의 응용 프로그램 코드를 구성 하는 방법 및 DirectX UWP 앱의 구조가 기존 데스크톱 응용 프로그램의 구조와 어떻게 다른 지 설명 합니다.                                                                                    |
| [Marble Maze 샘플에 시각적 콘텐츠 추가](adding-visual-content-to-the-marble-maze-sample.md)                   | Direct3D 및 Direct2D로 작업할 때 염두에 두어야 하는 몇 가지 주요 사례에 대해 설명 합니다. 또한 *대리석* 이 시각적 콘텐츠에 대해 이러한 사례를 적용 하는 방법을 설명 합니다.                                                                           |
| [Marble Maze 샘플에 입력 및 대화형 작업 추가](adding-input-and-interactivity-to-the-marble-maze-sample.md) | 사용자가 메뉴를 탐색 하 고 게임 보드와 상호 작용할 수 있도록가 속도계, touch 및 Xbox One 컨트롤러 입력에 대해 *대리석* 을 작동 하는 방법을 설명 합니다. 또한 입력으로 작업할 때 염두에 두어야 하는 몇 가지 모범 사례에 대해 설명 합니다. |
| [Marble Maze 샘플에 오디오 추가](adding-audio-to-the-marble-maze-sample.md)                                     | *대리석* 에서 오디오를 사용 하 여 음악 및 음향 효과를 게임 환경에 추가 하는 방법을 설명 합니다.                                                                                                                                                  |
