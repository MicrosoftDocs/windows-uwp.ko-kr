---
title: 연습-DirectX 11 및 UWP로 Direct3D 9 포팅
description: 이 포팅 연습에서는 Direct3D 9에서 Direct3D 11 및 유니버설 Windows 플랫폼 (UWP)로 간단한 렌더링 프레임 워크를 가져오는 방법을 보여 줍니다.
ms.assetid: d4467e1f-929b-a4b8-b233-e142a8714c96
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp, 게임, directx, 포트, direct3d 9, direct3d 11
ms.localizationpriority: medium
ms.openlocfilehash: 2e194ab79b8ba0a5dc79d4ad24f808d3613a0c98
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89158997"
---
# <a name="walkthrough-port-a-simple-direct3d-9-app-to-directx-11-and-universal-windows-platform-uwp"></a>연습: 간단한 Direct3D 9 앱에서 DirectX 11 및 유니버설 Windows 플랫폼 (UWP)로 이식



이 포팅 연습에서는 Direct3D 9에서 Direct3D 11 및 유니버설 Windows 플랫폼 (UWP)로 간단한 렌더링 프레임 워크를 가져오는 방법을 보여 줍니다.
## 
<table>
<colgroup>
<col width="50%" />
<col width="50%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">항목</th>
<th align="left">Description</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left"><p><a href="simple-port-from-direct3d-9-to-11-1-part-1--initializing-direct3d.md">Direct3D 11 초기화</a></p></td>
<td align="left"><p>Direct3d 장치 및 장치 컨텍스트에 대 한 핸들을 가져오는 방법 및 DXGI를 사용 하 여 스왑 체인을 설정 하는 방법을 비롯 하 여 direct3d 9 초기화 코드를 Direct3D 11로 변환 하는 방법을 보여 줍니다.</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="simple-port-from-direct3d-9-to-11-1-part-2--rendering.md">렌더링 프레임 워크 변환</a></p></td>
<td align="left"><p>기 하 도형 버퍼를 이식 하는 방법, HLSL 셰이더 프로그램을 컴파일 및 로드 하는 방법, Direct3D 11에서 렌더링 체인을 구현 하는 방법을 비롯 하 여 간단한 렌더링 프레임 워크를 Direct3D 9에서 Direct3D 11로 변환 하는 방법을 보여 줍니다.</p></td>
</tr>
<tr class="odd">
<td align="left"><p><a href="simple-port-from-direct3d-9-to-11-1-part-3--viewport-and-game-loop.md">게임 루프를 이식 합니다.</a></p></td>
<td align="left"><p>UWP 게임을 위한 창을 구현 하는 방법 및 <a href="https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Core.IFrameworkView"><strong>IFrameworkView</strong></a> 를 빌드하여 전체 화면 <a href="https://docs.microsoft.com/uwp/api/Windows.UI.Core.CoreWindow"><strong>CoreWindow</strong></a>을 제어 하는 방법을 비롯 하 여 게임 루프를 가져오는 방법을 보여 줍니다.</p></td>
</tr>
</tbody>
</table>

 

이 항목에서는 동일한 기본 그래픽 태스크를 수행 하는 두 개의 코드 경로를 안내 합니다. 회전 하는 꼭 짓 점 음영 큐브를 표시 합니다. 두 경우 모두 코드는 다음 프로세스를 포함 합니다.

1.  Direct3D 장치 및 스왑 체인 만들기
2.  다채로운 색상의 큐브 메시를 표현하기 위한 꼭짓점 버퍼 및 인덱스 버퍼 만들기
3.  꼭 짓 점을 화면 공간으로 변환 하는 꼭 짓 점 셰이더, 색 값을 혼합 하 고 셰이더를 컴파일하고 셰이더를 Direct3D 리소스로 로드 하는 픽셀 셰이더를 만듭니다.
4.  렌더링 체인을 구현 하 고 그려지는 큐브를 화면에 표시 합니다.
5.  창 만들기, 주 루프 시작, 창 메시지 처리를 처리 합니다.

이 연습을 완료 한 후에는 Direct3D 9와 Direct3D 11의 다음과 같은 기본 차이점에 대해 잘 알고 있어야 합니다.

-   장치, 장치 컨텍스트 및 그래픽 인프라의 분리입니다.
-   셰이더를 컴파일하고 런타임에 셰이더 바이트 코드를 로드 하는 프로세스입니다.
-   입력 어셈블러 (IA) 단계에 대 한 꼭 짓 점 별 데이터를 구성 하는 방법입니다.
-   [**IFrameworkView**](/uwp/api/Windows.ApplicationModel.Core.IFrameworkView) 를 사용 하 여 CoreWindow view를 만드는 방법

이 연습에서는 간소화를 위해 [**CoreWindow**](/uwp/api/Windows.UI.Core.CoreWindow) 를 사용 하 고 XAML interop에 대해서는 설명 하지 않습니다.

## <a name="prerequisites"></a>필수 구성 요소


[UWP DirectX 게임 개발을 위한 개발 환경을 준비](prepare-your-dev-environment-for-windows-store-directx-game-development.md)해야 합니다. 아직 템플릿이 필요 하지 않지만이 연습에 대 한 코드 샘플을 로드 하려면 2015 Microsoft Visual Studio 필요 합니다.

이 연습에서 설명 하는 DirectX 11 및 UWP 프로그래밍 개념을 더 잘 이해 하려면 [포팅 개념 및 고려 사항](porting-considerations.md) 을 참조 하세요.

## <a name="related-topics"></a>관련 항목

**Direct3D**

* [Direct3D 9에서 HLSL 셰이더 작성](/windows/desktop/direct3dhlsl/dx-graphics-hlsl-writing-shaders-9)
* [DirectX 게임 프로젝트 템플릿](user-interface.md)

**Microsoft Store**

* [**Microsoft:: WRL:: ComPtr**](/cpp/windows/comptr-class)
* [**개체 연산자에 대 한 핸들 (^)**](/cpp/windows/handle-to-object-operator-hat-cpp-component-extensions)