---
author: mtoepke
title: DirectX 포트 계획
description: DirectX 9에서 11 DirectX 및 UWP(유니버설 Windows 플랫폼)로 게임 포팅 프로젝트를 계획하세요. 그래픽 코드를 업그레이드 하 고 Windows 런타임 환경에서 게임을 저장합니다.
ms.assetid: 3c0c33ca-5d15-ae12-33f8-9b5d8da08155
ms.author: mtoepke
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, uwp, directx, 포트
ms.localizationpriority: medium
ms.openlocfilehash: dea6455b4e9aaef2a4239ef70d0919a4b8841bc5
ms.sourcegitcommit: e2fca6c79f31e521ba76f7ecf343cf8f278e6a15
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/15/2018
ms.locfileid: "6977633"
---
# <a name="plan-your-directx-port"></a>DirectX 포트 계획



**요약**

-   DirectX 포트 계획
-   [Direct3D 9에서 Direct3D 11로의 중요 변경 사항](understand-direct3d-11-1-concepts.md)
-   [기능 매핑](feature-mapping.md)


DirectX 9에서 11 DirectX 및 UWP(유니버설 Windows 플랫폼)로 게임 포팅 프로젝트를 계획하세요. 그래픽 코드를 업그레이드 하 고 Windows 런타임 환경에서 게임을 저장합니다.

## <a name="plan-to-port-graphics-code"></a>그래픽 코드 포팅 계획


UWP에 게임 포팅을 시작하기 전에 해당 게임에 Direct3D 8의 기존 내용이 없는지 확인하는 것이 중요합니다. 게임에 고정된 함수 파이프라인의 나머지 부분이 있지 않은지 확인합니다. 고정된 파이프라인 기능을 포함하여 사용되지 않는 기능의 전체 목록은 [사용되지 않는 기능](https://msdn.microsoft.com/library/windows/desktop/cc308047)을 참조하세요.

Direct3D 9에서 Direct3D 11로 업그레이드하면 검색 및 바꾸기 변경 사항 이상을 활용할 수 있습니다. Direct3D 장치, 장치 컨텍스트 및 그래픽 인프라 간의 차이 알아보고 Direct3D 9 이후 다른 중요한 변경 사항에 대해 학습해야 합니다. 이 섹션의 다른 항목을 참조하여 이 프로세스를 시작할 수 있습니다.

고유한 도우미 라이브러리 또는 커뮤니티 도구로 D3DX 및 DXUT 도우미 라이브러리를 교체해야 합니다. 자세한 내용은 [기능 매핑](feature-mapping.md) 섹션을 참조하세요.

> **참고**  이전의 D3DX 및 DXUT에서 제공한 일부 기능을 대체 [DirectX 도구 키트](http://go.microsoft.com/fwlink/p/?LinkID=248929) 또는 [DirectXTex](http://go.microsoft.com/fwlink/p/?LinkID=248926) 를 사용할 수 있습니다.

 

어셈블리 언어로 작성된 셰이더는 셰이더 모델 4 수준 9\_1 또는 9\_3 기능을 사용하는 HLSL로 업그레이드해야 하고 효과 라이브러리에 대해 작성된 셰이더는 최신 버전의 HLSL 구문으로 업데이트해야 합니다. 자세한 내용은 [기능 매핑](feature-mapping.md) 섹션을 참조하세요.

여러 [Direct3D 기능 수준](https://msdn.microsoft.com/library/windows/desktop/ff476876)에 익숙해지세요. 기능 수준은 알려진 기능 집합을 정의하여 다양한 범위의 비디오 하드웨어를 분류합니다. 각 집합은 대략 각각 9.1에서 11.2까지 Direct3D 버전에 해당합니다. 모든 기능 수준은 DirectX 11 API를 사용합니다.

## <a name="plan-to-port-win32-ui-code-to-corewindow"></a>Win32 UI 코드를 CoreWindow로 포팅하기 위한 계획


UWP 앱은 [**CoreWindow**](https://msdn.microsoft.com/library/windows/apps/br208225)라는 앱 컨테이너에 대해 작성된 창에서 실행됩니다. 게임은 바탕 화면 창보다 구현 세부 사항이 적게 필요한 [**IFrameworkView**](https://msdn.microsoft.com/library/windows/apps/hh700478)에서 상속하여 이 창을 제어합니다. 게임의 주 루프는 [**IFrameworkView::Run**](https://msdn.microsoft.com/library/windows/apps/hh700505) 메서드에 있습니다.

UWP 앱의 수명 주기는 데스크톱 앱과 전혀 다릅니다. 일시 중단 이벤트가 발생하는 경우 앱에는 코드 실행을 중지하기 위한 제한된 시간만 있기 때문에 게임을 자주 저장해야 하고, 앱이 다시 시작될 때 마쳤던 위치로 바로 플레이어가 돌아갈 수 있는지 확인할 수 있습니다. 게임은 다시 시작에서 지속적인 게임 플레이 환경을 유지 관리하기에 충분한 정도로 자주 저장해야 하지만 게임 저장이 프레임 속도에 영향을 미치거나 게임이 잠시 멈추게 하는 경우는 드뭅니다. 게임이 종료된 상태에서 다시 시작되면 잠재적으로 게임이 게임 상태를 다시 로드해야 합니다.

[DirectXMath](https://msdn.microsoft.com/library/windows/desktop/ee415571)는 D3DXMath 및 XNAMath에 대한 대체로 사용할 수 있으며 수학 라이브러리가 필요한 경우 유용할 수 있습니다. DirectXMath에는 빠른 포팅 가능한 데이터 형식 및 셰이더에서 사용할 수 있도록 정렬되고 압축되는 형식이 있습니다.

[연동된 API](https://msdn.microsoft.com/library/windows/desktop/dd405529)와 같은 네이티브 라이브러리는 ARM 내부 기능을 지원하도록 확장되어 있습니다. 게임이 연동된 API를 사용하는 경우 DirectX 11와 UWP에서 이 API를 계속 사용할 수 있습니다.

Microsoft 템플릿 및 코드 샘플은 아직 익숙하지 않을 수 있는 새로운 C++ 기능을 사용합니다. 예를 들어 비동기 메서드는 [**lambda expressions**](https://msdn.microsoft.com/library/windows/apps/dd293608.aspx)과 함께 사용되어 UI 스레드를 차단하지 않고 Direct3D 리소스를 로드합니다.

자주 사용하게 되는 두 가지 개념이 있습니다.

-   관리되는 참조([**^ 연산자**](https://msdn.microsoft.com/library/windows/apps/yk97tc08.aspx)) 및 [**관리되는 클래스**](https://msdn.microsoft.com/library/windows/apps/6w96b5h7.aspx)(ref 클래스)는 Windows 런타임의 기본적인 부분입니다. 예를 들어 [**IFrameworkView**](https://msdn.microsoft.com/library/windows/apps/hh700478)(연습의 추가 정보)와 같은 Windows 런타임 구성 요소와 함께 인터페이스에 대한 관리되는 ref 클래스를 사용해야 합니다.
-   Direct3D 11 COM 인터페이스 작업 시 [**Microsoft::WRL::ComPtr**](https://msdn.microsoft.com/library/windows/apps/br244983.aspx) 템플릿 종류 사용하여 COM 포인터를 보다 쉽게 사용할 수 있게 합니다.

 

 




