---
title: DirectX 포트 계획
description: DirectX 9에서 DirectX 11 및 유니버설 Windows 플랫폼 (UWP)로 게임 포팅 프로젝트를 계획 합니다. 그래픽 코드를 업그레이드 하 고 Windows 런타임 환경에 게임을 배치할 수 있습니다.
ms.assetid: 3c0c33ca-5d15-ae12-33f8-9b5d8da08155
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp, directx, 포트
ms.localizationpriority: medium
ms.openlocfilehash: 784d46d3f1a0c023d8c597c99e7a6cf9a0979d83
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89175227"
---
# <a name="plan-your-directx-port"></a>DirectX 포트 계획



**요약**

-   DirectX 포트 계획
-   [Direct3D 9에서 Direct3D 11로 중요 한 변경 내용](understand-direct3d-11-1-concepts.md)
-   [기능 매핑](feature-mapping.md)


DirectX 9에서 DirectX 11 및 유니버설 Windows 플랫폼 (UWP)로 게임 포팅 프로젝트 계획: 그래픽 코드를 업그레이드 하 고 Windows 런타임 환경에 게임을 배치 합니다.

## <a name="plan-to-port-graphics-code"></a>그래픽 코드를 이식할 계획


게임을 UWP로 포팅 하려면 먼저 게임에 Direct3D 8의 holdovers 있는지 확인 하는 것이 중요 합니다. 게임에 고정 함수 파이프라인의 남아가 없는지 확인 합니다. 고정 파이프라인 기능을 포함 하 여 더 이상 사용 되지 않는 기능의 전체 목록은 [더 이상 사용 되지 않는 기능](/windows/desktop/direct3d10/d3d10-graphics-programming-guide-api-features-deprecated)을 참조 하세요.

Direct3D 9에서 Direct3D 11로 업그레이드 하는 것은 검색 및 바꾸기 변경 보다 더 다양 합니다. Direct3D 장치, 장치 컨텍스트 및 그래픽 인프라 간의 차이점을 알고 있어야 하 고 Direct3D 9 이후의 다른 중요 한 변경 내용에 대해 알아보세요. 이 섹션의 다른 항목을 읽어이 프로세스를 시작할 수 있습니다.

D3DX 및 DXUT 도우미 라이브러리를 고유한 도우미 라이브러리나 커뮤니티 도구로 바꾸어야 합니다. 자세한 내용은 [기능 매핑](feature-mapping.md) 섹션을 참조 하세요.

> **참고**    [DirectX Tool Kit](https://github.com/Microsoft/DirectXTK) 또는 [DirectXTex](https://github.com/Microsoft/DirectXTex) 를 사용 하 여 이전에 D3DX 및 DXUT에서 제공 했던 일부 기능을 대체할 수 있습니다.

 

어셈블리 언어로 작성 된 셰이더는 셰이더 모델 4 수준 9 1 또는 9 3 기능을 사용 하 여 HLSL로 업그레이드 하 \_ \_ 고 효과 라이브러리에 대해 작성 된 셰이더를 최신 버전의 HLSL 구문으로 업데이트 해야 합니다. 자세한 내용은 [기능 매핑](feature-mapping.md) 섹션을 참조 하세요.

다양 한 [Direct3D 기능 수준](/windows/desktop/direct3d11/overviews-direct3d-11-devices-downlevel-intro)에 대해 알아봅니다. 기능 수준은 알려진 기능 집합을 정의 하 여 광범위 한 비디오 하드웨어를 분류 합니다. 각 집합은 9.1에서 11.2 까지의 Direct3D 버전에 해당 합니다. 모든 기능 수준은 DirectX 11 API를 사용 합니다.

## <a name="plan-to-port-win32-ui-code-to-corewindow"></a>CoreWindow에 Win32 UI 코드를 이식할 계획


UWP 앱은 [**CoreWindow**](/uwp/api/Windows.UI.Core.CoreWindow)이라는 앱 컨테이너에 대해 만들어진 창에서 실행 됩니다. 게임은 [**IFrameworkView**](/uwp/api/Windows.ApplicationModel.Core.IFrameworkView)에서 상속 하 여 창을 제어 하며,이는 데스크톱 창 보다 구현 세부 정보가 더 적습니다. 게임의 기본 루프는 [**IFrameworkView:: Run**](/uwp/api/windows.applicationmodel.core.iframeworkview.run) 메서드에 있습니다.

UWP 앱의 수명 주기는 데스크톱 앱과 매우 다릅니다. 일시 중단 이벤트가 발생 하는 경우 응용 프로그램에서 코드 실행을 중지 하는 데 제한 된 시간만 사용 하 고 앱이 다시 시작 될 때 바로 그 곳으로 이동할 수 있는지 확인 하려는 경우 게임을 자주 저장 해야 합니다. 게임은 계속 해 서 연속 게임 플레이 환경을 유지 관리할 수 있을 만큼 자주 저장 해야 하지만 게임에서 영향을 주는 프레임 속도를 절감 하거나 게임을 끊길 수 있는 경우가 많습니다. 게임이 종료 됨 상태에서 다시 시작 되 면 게임 상태를 로드 해야 할 수 있습니다.

[Directxmath](/windows/desktop/dxmath/ovw-xnamath-progguide) 는 D3DXMath 및 XNAMath에 대 한 대체 항목으로 사용 될 수 있으며 수학 라이브러리가 필요한 경우에 유용할 수 있습니다. DirectXMath에는 셰이더와 함께 사용 하기 위해 맞추고 압축 된 빠르고 이식 가능한 데이터 형식 및 형식이 있습니다.

[연동 API](/windows/desktop/Sync/what-s-new-in-synchronization) 와 같은 네이티브 라이브러리는 ARM 내장 함수를 지원 하도록 확장 되었습니다. 게임에서 연동 Api를 사용 하는 경우 DirectX 11 및 UWP에서 계속 사용할 수 있습니다.

템플릿 및 코드 샘플은 아직 익숙하지 않을 수 있는 새로운 c + + 기능을 사용 합니다. 예를 들어 비동기 메서드는 UI 스레드를 차단 하지 않고 Direct3D 리소스를 로드 하는 [**람다 식**](/cpp/cpp/lambda-expressions-in-cpp) 과 함께 사용 됩니다.

자주 사용 하는 두 가지 개념이 있습니다.

-   관리 되는 참조 ([**^ operator**](/cpp/windows/handle-to-object-operator-hat-cpp-component-extensions)) 및 [**관리 되는 클래스**](/cpp/windows/classes-and-structs-cpp-component-extensions) (ref 클래스)는 Windows 런타임의 기본적인 부분입니다. Windows 런타임 구성 요소와 상호 작용 하는 데 관리 되는 ref 클래스를 사용 해야 합니다 (예: [**IFrameworkView**](/uwp/api/Windows.ApplicationModel.Core.IFrameworkView) ).
-   Direct3D 11 COM 인터페이스를 사용 하 여 작업할 때 COM 포인터를 더 쉽게 사용할 수 있도록 [**Microsoft:: WRL:: ComPtr**](/cpp/windows/comptr-class) 템플릿 형식을 사용 합니다.

 

 