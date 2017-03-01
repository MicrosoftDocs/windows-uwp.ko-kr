---
author: mtoepke
title: "연습 - 간단한 Direct3D 9 앱을 DirectX 11 및 UWP(유니버설 Windows 플랫폼)로 포팅"
description: "이 포팅 연습에서는 간단한 렌더링 프레임워크를 Direct3D 9에서 Direct3D 11 및 UWP(유니버설 Windows 플랫폼)로 가져오는 방법을 보여 줍니다."
ms.assetid: d4467e1f-929b-a4b8-b233-e142a8714c96
ms.author: mtoepke
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: "windows 10, uwp, 게임, directx, 포트, direct3d 9, direct3d 11"
translationtype: Human Translation
ms.sourcegitcommit: c6b64cff1bbebc8ba69bc6e03d34b69f85e798fc
ms.openlocfilehash: 1e3f9b7fbfaa110661ebb111c60d0089757dc885
ms.lasthandoff: 02/07/2017

---

# <a name="walkthrough-port-a-simple-direct3d-9-app-to-directx-11-and-universal-windows-platform-uwp"></a>연습: 간단한 Direct3D 9 앱을 DirectX 11 및 UWP(유니버설 Windows 플랫폼)로 포팅


\[ Windows 10의 UWP 앱에 맞게 업데이트되었습니다. Windows 8.x 문서는 [보관](http://go.microsoft.com/fwlink/p/?linkid=619132)을 참조하세요. \]

이 포팅 연습에서는 간단한 렌더링 프레임워크를 Direct3D 9에서 Direct3D 11 및 UWP(유니버설 Windows 플랫폼)로 가져오는 방법을 보여 줍니다.
## 
<table>
<colgroup>
<col width="50%" />
<col width="50%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">항목</th>
<th align="left">설명</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left"><p>[Direct3D 11 초기화](simple-port-from-direct3d-9-to-11-1-part-1--initializing-direct3d.md)</p></td>
<td align="left"><p>Direct3D 디바이스와 디바이스 컨텍스트로 핸들을 가져오는 방법 및 DXGI를 사용하여 스왑 체인을 설정하는 방법을 포함하여 Direct3D 11로 Direct3D 9 초기화 코드를 변환하는 방법을 보여 줍니다.</p></td>
</tr>
<tr class="even">
<td align="left"><p>[렌더링 프레임워크 변환](simple-port-from-direct3d-9-to-11-1-part-2--rendering.md)</p></td>
<td align="left"><p>기하 도형 버퍼를 포팅하는 방법, HLSL 셰이더 프로그램을 컴파일하고 로드하는 방법 및 Direct3D 11에서 렌더링 체인을 구현하는 방법을 포함하여 간단한 렌더링 프레임 워크를 Direct3D 9에서 Direct3D 11로 변환하는 방법을 보여 줍니다.</p></td>
</tr>
<tr class="odd">
<td align="left"><p>[게임 루프 포팅](simple-port-from-direct3d-9-to-11-1-part-3--viewport-and-game-loop.md)</p></td>
<td align="left"><p>[<strong>IFrameworkView</strong>](https://msdn.microsoft.com/library/windows/apps/hh700478)를 작성하여 전체 화면 [<strong>CoreWindow</strong>](https://msdn.microsoft.com/library/windows/apps/br208225)를 제어하는 방법을 포함하여 UWP 게임의 창을 구현하는 방법 및 게임 루프로 가져오는 방법을 보여 줍니다.</p></td>
</tr>
</tbody>
</table>

 

이 항목에서는 동일한 기본 그래픽 작업(회전하는 꼭짓점 음영 큐브 표시)을 수행하는 두 가지 코드 경로를 살펴봅니다. 두 경우 모두 코드에 다음 프로세스가 포함됩니다.

1.  Direct3D 장치 및 스왑 체인 만들기
2.  다채로운 색상의 큐브 메시를 표현하기 위한 꼭짓점 버퍼 및 인덱스 버퍼 만들기
3.  꼭짓점을 화면 공간으로 전환하는 꼭짓점 셰이더와 색 값을 혼합하는 픽셀 셰이더 만들기, 셰이더 컴파일 및 셰이더를 Direct3D 리소스로 로드
4.  렌더링 체인 구현 및 그려진 큐브를 화면에 표시
5.  창 만들기, 기본 루프 시작, 창 메시지 처리

이 연습을 완료하면 Direct3D 9과 Direct3D 11 간의 다음과 같은 기본 차이점을 이해할 수 있습니다.

-   장치, 디바이스 컨텍스트 및 그래픽 인프라 구분
-   셰이더를 컴파일하고, 런타임에 셰이더 바이트코드를 로드하는 프로세스
-   IA(입력 어셈블러) 단계에 대해 꼭짓점별 데이터를 구성하는 방법.
-   [**IFrameworkView**](https://msdn.microsoft.com/library/windows/apps/hh700478)를 사용하여 CoreWindow 뷰를 만드는 방법

이 연습에서는 간소하게 하기 위해 XAML 상호 운용성이 지원되지 않는 [**CoreWindow**](https://msdn.microsoft.com/library/windows/apps/br208225)를 사용합니다.

## <a name="prerequisites"></a>필수 조건


[UWP DirectX 게임 개발을 위한 개발 환경을 준비](prepare-your-dev-environment-for-windows-store-directx-game-development.md)해야 합니다. 아직 템플릿은 필요하지 않지만 이 연습을 위한 코드 샘플을 로드하려면 Microsoft Visual Studio 2015가 필요합니다.

이 연습에 나온 DirectX 11 및 UWP 프로그래밍 개념을 더 잘 이해하려면 [포팅 개념 및 고려 사항](porting-considerations.md)을 참조하세요.

## <a name="related-topics"></a>관련 항목

**Direct3D**

* [Direct3D 9에서 HLSL 셰이더 작성](https://msdn.microsoft.com/library/windows/desktop/bb944006)
* [DirectX 게임 프로젝트 템플릿](user-interface.md)

**Windows 스토어**

* [**Microsoft::WRL::ComPtr**](https://msdn.microsoft.com/library/windows/apps/br244983.aspx)
* [**개체 운영자에 대한 핸들(^)**](https://msdn.microsoft.com/library/windows/apps/yk97tc08.aspx)


