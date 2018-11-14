---
author: eliotcowley
title: C++ 및 DirectX로 UWP 게임 Marble Maze 개발
description: 이 설명서 섹션에서는 DirectX 및 Visual C++를 사용하여 3D UWP(유니버설 Windows 플랫폼) 게임을 만드는 방법을 설명합니다.
ms.assetid: 43f1977a-7e1d-614c-696e-7669dd8a9cc7
ms.author: elcowle
ms.date: 08/10/2017
ms.topic: article
keywords: windows 10, uwp, 게임, 샘플, directx, 3d
ms.localizationpriority: medium
ms.openlocfilehash: 7a808c36ab319d76f16c653c5812ebe4b269ec59
ms.sourcegitcommit: f2c9a050a9137a473f28b613968d5782866142c6
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/09/2018
ms.locfileid: "6249261"
---
# <a name="developing-marble-maze-a-uwp-game-in-c-and-directx"></a>C++ 및 DirectX로 UWP 게임 Marble Maze 개발




이 항목에서는 DirectX 및 Visual C++를 사용하여 3D UWP(유니버설 Windows 플랫폼) 게임을 만드는 방법을 설명합니다. Marble Maze라는 게임은 태블릿뿐 아니라 전통적인 데스크톱과 노트북 PC 같은 여러 폼 팩터를 수용합니다.

> [!NOTE]
> Marble Maze 소스 코드를 다운로드하려면 [GitHub 샘플](http://go.microsoft.com/fwlink/?LinkId=624011)을 참조하세요.

> [!IMPORTANT]
> Marble Maze는 UWP 게임을 만드는 모범 사례인 디자인 패턴을 보여 줍니다. 개발 중인 게임의 고유한 요구 사항과 자신의 사례에 맞게 구현 정보를 대부분 조정할 수 있습니다. 요구에 더 적합한 경우 다른 기술이나 라이브러리를 사용해도 됩니다. (그러나 코드가 [Windows 앱 인증 키트](https://docs.microsoft.com/windows/uwp/debug-test-perf/windows-app-certification-kit)를 전달하는지 항상 확인해야 합니다.) 여기에 사용된 구현 방법이 성공적인 게임 개발에 필수적이라고 판단될 경우 이 설명서에서 강조해 두었습니다.

 

## <a name="introducing-marble-maze"></a>Marble Maze 소개


Marble Maze가 선택된 이유는 비교적 기본적이면서도 대부분의 게임에 있는 다양한 기능을 보여 주기 때문입니다. Marble Maze는 그래픽, 입력 처리 및 오디오를 사용하는 방법을 보여 줍니다. 또한 규칙 및 목표와 같은 게임 방법을 보여 줍니다.

Marble Maze는 일반적으로 구멍이 포함된 상자와 강철 또는 유리 구슬로 구성된 테이블 미로 게임과 비슷합니다. Marble Maze의 목표는 테이블 버전과 마찬가지로, 미로를 기울여 구슬이 구멍에 빠지지 않도록 하면서 최대한 짧은 시간 내에 구슬을 미로 시작부터 끝까지 보내는 것입니다. Marble Maze는 검사점 개념을 추가합니다. 구슬이 구멍에 빠지면 구슬이 마지막으로 통과한 검사점 위치에서 게임이 다시 시작됩니다.

Marble Maze는 사용자가 게임판을 조작하는 여러 가지 방법을 제공합니다. 터치 사용 또는 가속도계 사용 장치가 있는 경우 해당 장치를 사용하여 게임판을 이동할 수 있습니다. Xbox One 컨트롤러 또는 마우스를 사용하여 게임 플레이를 제어할 수도 있습니다.

![Marble Maze 게임의 스크린샷](images/marblemaze-2.png)

## <a name="prerequisites"></a>필수 조건


-   Windows10 크리에이터 스 업데이트
-   [Microsoft Visual Studio2017](https://www.visualstudio.com/downloads/)
-   C++ 프로그래밍 지식
-   DirectX 및 DirectX 용어 숙지
-   COM에 대한 기본 지식

## <a name="who-should-read-this"></a>대상 사용자


3D 게임 또는 Windows10에 대 한 다른 그래픽 집약적인 응용 프로그램을 만들려는 경우이 적합 합니다. 이 설명서에서 제공하는 원칙과 사례를 사용하여 고유한 UWP 게임을 만들어 보세요. C++ 및 DirectX 프로그래밍에 대한 배경 지식이나 관심이 있으면 이 설명서를 활용하는 데 도움이 됩니다. DirectX 사용 경험이 없는 경우 유사한 3D 그래픽 프로그래밍 환경의 사용 경험이 있어도 도움이 될 수 있습니다.

[연습: DirectX를 사용하여 간단한 UWP 게임 만들기](tutorial--create-your-first-uwp-directx-game.md)에서는 DirectX 및 C++를 사용하여 기본적인 3D 슈팅 게임을 구현하는 다른 샘플에 대해 설명합니다.

## <a name="what-this-documentation-covers"></a>이 설명서에서 다루는 내용


이 설명서에서는 다음을 수행하는 방법을 설명합니다.

-   Windows 런타임 API와 DirectX를 사용하여 UWP 게임을 만듭니다.
-   [Direct3D](https://msdn.microsoft.com/library/windows/desktop/ff476080) 및 [Direct2D](https://msdn.microsoft.com/library/windows/desktop/dd370990)를 사용하여 모델, 텍스처, 꼭짓점 및 픽셀 셰이더, 2D 오버레이 등의 시각적 콘텐츠 작업을 합니다.
-   터치, 가속도계, Xbox One 컨트롤러 등의 입력 메커니즘을 통합합니다.
-   [XAudio2](https://msdn.microsoft.com/library/windows/desktop/hh405049)를 사용하여 음악 및 소리 효과를 통합합니다.

## <a name="what-this-documentation-does-not-cover"></a>이 설명서에서 다루지 않는 내용


이 설명서에서는 다음과 같은 게임 개발 측면을 다루지 않습니다. 이러한 측면 뒤에는 해당 측면을 설명하는 추가 리소스가 제공됩니다.

-   3D 게임 디자인 원칙
-   C++ 또는 DirectX 프로그래밍 기본 사항
-   텍스처, 모델, 오디오 등의 리소스를 디자인하는 방법
-   게임에서 동작 또는 성능 문제를 해결하는 방법
-   다른 세계 지역에서 사용하기 위해 게임을 준비하는 방법
-   게임을 인증하고 Microsoft Store에 게시하는 방법

또한 Marble Maze는 [DirectXMath](https://msdn.microsoft.com/library/windows/desktop/hh437833) 라이브러리를 사용하여 3D 기하 도형 작업을 하고 충돌 등의 물리학 계산을 수행합니다. DirectXMath는 이 섹션에서 자세히 다루지 않습니다. Marble Maze에서 DirectXMath를 사용하는 방법에 대한 자세한 내용은 소스 코드를 참조하세요.

Marble Maze는 많은 재사용 가능 구성 요소를 제공하지만 전체 게임 개발 프레임워크는 아닙니다. Marble Maze 구성 요소가 게임에 재사용 가능한 경우 설명서에서 강조됩니다.

## <a name="next-steps"></a>다음 단계


[Marble Maze 샘플 기본 사항](marble-maze-sample-fundamentals.md)부터 시작하여 Marble Maze 소스 코드가 따르는 코딩 및 스타일 지침과 Marble Maze 구조에 대해 알아볼 것을 권장합니다. 다음 표에서는 더 쉽게 참조할 수 있도록 이 섹션의 문서에 대해 간단히 설명합니다.

## <a name="in-this-section"></a>섹션 내용


| 제목                                                                                                                    | 설명                                                                                                                                                                                                                                        |
|--------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| [Marble Maze 샘플 기본 사항](marble-maze-sample-fundamentals.md)                                                   | 소스 코드가 따르는 코딩 및 스타일 지침과 게임 구조의 개요를 제공합니다.                                                                                                                                 |
| [Marble Maze 응용 프로그램 구조](marble-maze-application-structure.md)                                               | Marble Maze 응용 프로그램 코드가 구성된 방식 및 DirectX UWP 앱의 구조와 일반적인 데스크톱 응용 프로그램 구조 간의 차이점에 대해 설명합니다.                                                                                    |
| [Marble Maze 샘플에 시각적 콘텐츠 추가](adding-visual-content-to-the-marble-maze-sample.md)                   | Direct3D 및 Direct2D 작업을 할 때 고려할 몇 가지 주요 사항에 대해 설명합니다. 또한 Marble Maze가 시각적 콘텐츠에 대해 이러한 사례를 적용하는 방법을 설명합니다.                                                                           |
| [Marble Maze 샘플에 입력 및 대화형 작업 추가](adding-input-and-interactivity-to-the-marble-maze-sample.md) | 사용자가 메뉴를 탐색하고 게임판을 조작할 수 있도록 Marble Maze가 가속도계, 터치 및 Xbox One 컨트롤러 입력을 작업하는 방식에 대해 설명합니다. 또한 입력 작업을 할 때 고려할 몇 가지 모범 사례에 대해 설명합니다. |
| [Marble Maze 샘플에 오디오 추가](adding-audio-to-the-marble-maze-sample.md)                                     | 게임 환경에 음악과 소리 효과를 추가하기 위해 Marble Maze가 오디오를 사용하는 방식에 대해 설명합니다.                                                                                                                                                  |

 

 

 




