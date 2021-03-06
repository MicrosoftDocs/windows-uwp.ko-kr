---
title: 그래픽 파이프라인
description: Direct3D 그래픽 파이프라인은 실시간 게임 애플리케이션용 그래픽을 생성하도록 설계되었습니다. 데이터는 각각의 구성 가능하거나 프로그래밍 가능한 각 단계를 통해 입력에서 출력으로 흐릅니다.
ms.assetid: C9519AD0-5425-48BD-9FF4-AED8959CA4AD
keywords:
- 그래픽 파이프라인
- 파이프라인 단계
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 2032a57b598f08b24c1d52cecfa4f92b90591ec0
ms.sourcegitcommit: 48702934676ae366fd46b7d952396c5e2fb2cbbe
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 01/06/2021
ms.locfileid: "97927796"
---
# <a name="graphics-pipeline"></a>그래픽 파이프라인

Direct3D 그래픽 파이프라인은 실시간 게임 애플리케이션용 그래픽을 생성하도록 설계되었습니다. 데이터는 각각의 구성 가능하거나 프로그래밍 가능한 각 단계를 통해 입력에서 출력으로 흐릅니다.

모든 단계는 Direct3D API를 사용 하 여 구성할 수 있습니다. [HLSL](/windows/desktop/direct3dhlsl/dx-graphics-hlsl) 프로그래밍 언어를 사용 하 여 일반적인 셰이더 코어 (모퉁이가 둥근 사각형 블록)를 기능 하는 단계를 프로그래밍할 수 있습니다. 이렇게 하면 파이프라인을 매우 유연 하 고 유연 하 게 만들 수 있습니다.

가장 일반적으로 사용 되는는 꼭 짓 점 셰이더 (VS) 단계와 픽셀 셰이더 (PS) 단계입니다. 이러한 셰이더 단계를 제공 하지 않는 경우에는 기본 no op, 통과 꼭 짓 점 및 픽셀 셰이더가 사용 됩니다.

![direct3d 11 프로그래밍 가능 파이프라인의 데이터 흐름 다이어그램](images/d3d11-pipeline-stages.jpg)

## <a name="input-assembler-stage"></a>입력 어셈블러 단계

[입력 어셈블러 (IA) 단계](input-assembler-stage--ia-.md) 는 아직 처리 되지 않은 기본 형식으로 처리를 줄임으로써 셰이더를 보다 효율적으로 만드는 데 도움이 되는 의미 체계 id를 포함 하 여 파이프라인에 기본 및 인접 데이터를 제공 합니다.

- 입력

   메모리에서 사용자가 채운 버퍼의 기본 데이터 (삼각형, 선 및/또는 위치)입니다. 인접 한 데이터입니다. 삼각형은 각 삼각형에 대해 3 개의 꼭지점이 고 삼각형 당 인접 하지 않은 데이터의 경우 3 개의 꼭지점이 될 수 있습니다.

- 출력

   연결 된 시스템 생성 값 (예: 기본 ID, 인스턴스 ID 또는 꼭 짓 점 ID)을 사용 하는 기본 형식입니다.

## <a name="vertex-shader-stage"></a>꼭 짓 점 셰이더 단계

[꼭 짓 점 셰이더 (VS) 단계](vertex-shader-stage--vs-.md) 에서는 일반적으로 변환, 스킨, 조명 등의 작업을 수행 하는 꼭 짓 점을 처리 합니다. 꼭 짓 점 셰이더는 단일 입력 버텍스를 사용 하 고 단일 출력 꼭 짓 점을 생성 합니다. 변환, 스키닝, 모핑, 꼭 짓 점 별 조명 등의 개별 꼭 짓 점 별 작업

- 입력

   VertexID 및 InstanceID 시스템 생성 값을 포함 하는 단일 꼭 짓 점입니다. 각 꼭 짓 점 셰이더 입력 꼭 짓 점은 최대 16 32 비트 벡터 (각각 최대 4 개의 구성 요소)로 구성 될 수 있습니다.

- 출력

   단일 꼭 짓 점입니다. 각 출력 꼭 짓 점은 16 32 비트 4 구성 요소 벡터로 구성 될 수 있습니다.

## <a name="hull-shader-stage"></a>선체 셰이더 단계

[선체 셰이더 (HS) 단계](hull-shader-stage--hs-.md) 는 공간 분할 (tessellation) 단계 중 하나로, 모델의 단일 표면을 여러 삼각형으로 효율적으로 분할 합니다. 선체 셰이더는 patch 당 한 번 호출 되며, 낮은 순서 화면을 정의 하는 입력 제어 점을 패치를 구성 하는 제어 점으로 변환 합니다. 또한 Tessellator (TS) 단계 및 도메인 셰이더 (DS) 단계에 대 한 데이터를 제공 하기 위해 몇 가지 패치를 계산 합니다.

- 입력

   1에서 32 사이의 입력 제어 지점과 함께 낮은 순서 표면을 정의 합니다.

- 출력

   1-32 출력 제어 지점과 함께 패치를 구성 합니다. 선체 셰이더는 제어 지점의 수, 패치 표면의 유형 및 개체당 때 사용할 분할 유형을 포함 하 여 Tessellator (TS) 단계의 상태를 선언 합니다.

## <a name="tessellator-stage"></a>Tessellator 단계

[Tessellator (TS) 단계](tessellator-stage--ts-.md) 는 기 하 도형 패치를 나타내는 도메인의 샘플링 패턴을 만들고 이러한 예제를 연결 하는 작은 개체 (삼각형, 요소 또는 선) 집합을 생성 합니다.

- 입력

   Tessellator는 공간 분할 요소 (도메인을 공간 분할 하는 정도를 지정 하는 공간 분할)와 단계에서 전달 된 분할 유형 (패치를 조각화 하는 데 사용 되는 알고리즘을 지정 하는)을 사용 하 여 패치 당 한 번만 작동 합니다.

- 출력

   Tessellator는 uv (및 선택적으로) 좌표와 표면 토폴로지를 도메인 셰이더 단계로 출력 합니다.

## <a name="domain-shader-stage"></a>도메인 셰이더 단계

[DS (도메인 셰이더) 단계](domain-shader-stage--ds-.md) 는 출력 patch에서 분리 지점의 꼭 짓 점 위치를 계산 합니다. 각 도메인 샘플에 해당 하는 꼭 짓 점 위치를 계산 합니다. 도메인 셰이더는 tessellator 스테이지 출력 지점 마다 한 번 실행 되며, 선체 셰이더 출력 패치 및 출력 패치 상수에 대 한 읽기 전용 액세스 권한 및 tessellator 스테이지 출력 UV 좌표를 가집니다.

- 입력

   도메인 셰이더는 [선체 셰이더 (HS) 단계](hull-shader-stage--hs-.md)에서 출력 제어 점을 사용 합니다. 선체 셰이더 출력에는 제어 요소, 패치 상수 데이터 및 공간 분할 (tessellation) 요소가 포함 됩니다. 공간 분할 (tessellation) 요인은 tessellator (예: geomorphing를 용이 하 게 하는 정수 공간 분할으로 반올림 하기 전에)에 의해 사용 되는 값을 포함할 수 있습니다. 도메인 셰이더는 [Tessellator (TS) 단계](tessellator-stage--ts-.md)에서 출력 좌표 마다 한 번씩 호출 됩니다.

- 출력

   DS (도메인 셰이더) 단계는 출력 패치에서 분리 지점의 꼭 짓 점 위치를 출력 합니다.

## <a name="geometry-shader-stage"></a>기 하 도형 셰이더 단계

[기 하 도형 셰이더 (GS) 스테이지](geometry-shader-stage--gs-.md) 는 인접 한 꼭지점과 함께 삼각형, 선 및 점의 전체 기본 형식을 처리 합니다. 기 하 도형 증폭 및 비 증폭을 지원 합니다. 이는 점 스프라이트 확장, 동적 파티클 시스템, 이유/Fin 생성, 섀도 볼륨 생성, 단일 패스 렌더링 큐브 맵, Per-Primitive 재질 스와핑 및 Per-Primitive 재질 설정 (픽셀 셰이더가 사용자 지정 특성 보간을 수행할 수 있도록 기본 데이터로 barycentric 좌표 생성 포함)을 포함 하는 알고리즘에 유용 합니다.

- 입력

   단일 꼭 짓 점에 대해 작동 하는 꼭 짓 점 셰이더와 달리 기 하 도형 셰이더의 입력은 전체 기본 형식 (삼각형의 경우 3 개의 꼭 짓 점, 선에는 두 개의 꼭 짓 점)에 대 한 꼭 짓 점입니다.

- 출력

   기 하 도형 셰이더 (GS) 단계는 단일 선택 토폴로지를 형성 하는 여러 꼭 짓 점을 출력할 수 있습니다. 사용 가능한 기 하 도형 셰이더 출력 토폴로지는 **tristrip**, **linestrip** 및 **pointlist** 입니다. 내보낼 수 있는 최대 꼭 짓 점 수는 정적으로 선언 되어야 하지만 내보내는 기본 형식의 수는 기 하 도형 셰이더 호출 내에서 자유롭게 달라질 수 있습니다. 기 하 도형 셰이더 호출에서 내보낸 스트립 길이는 임의로 지정할 수 있으며 [RestartStrip](/windows/desktop/direct3dhlsl/dx-graphics-hlsl-so-restartstrip) HLSL 함수를 통해 새 스트립을 만들 수 있습니다.

## <a name="stream-output-stage"></a>스트림 출력 단계

[스트림 출력 ()](stream-output-stage--so-.md) 은 이전 활성 단계에서 메모리의 하나 이상의 버퍼로 버텍스 데이터를 지속적으로 출력 (또는 스트림) 합니다. 메모리로 스트리밍되는 데이터는 입력 데이터로 다시 파이프라인으로 재순환 되거나 CPU에서 다시 읽을 수 있습니다.

- 입력

   이전 파이프라인 단계의 꼭 짓 점 데이터입니다.

- 출력

   스트림 출력 ()은 이전 활성 단계 (예: 기 하 도형 셰이더 (GS) 단계)에서 메모리의 하나 이상의 버퍼로 연속 된 데이터 (또는 스트림)를 출력 합니다. 기 하 도형 셰이더 (GS) 단계가 비활성 상태이 고 스트림 출력 () 단계가 활성 상태 이면 DS (도메인 셰이더) 단계의 꼭 짓 점 데이터를 지속적으로 메모리의 버퍼에 출력 합니다. 즉, DS가 비활성 상태인 경우에는 셰이더 (VS) 단계에서도 해당 합니다.

## <a name="rasterizer-stage"></a>래스터 라이저 단계

[래스터 라이저 (RS) 스테이지](rasterizer-stage--rs-.md) 는 표시 되지 않고, 픽셀 셰이더 (PS) 단계에 대 한 기본 형식을 준비 하 고, 픽셀 셰이더를 호출 하는 방법을 결정 하는 기본 형식을 클립 합니다. 셰이프 또는 기본 형식으로 구성 된 벡터 정보를 표준 3D 그래픽을 표시 하기 위한 래스터 이미지 (픽셀로 구성)로 변환 합니다.

- 입력

   래스터 라이저 단계에 들어오는 꼭 짓 점 (x, y, z, w)은 동일한 클립 공간에 있는 것으로 간주 됩니다. 이 좌표 공간에서 X 축은 오른쪽을 가리키고 Y는 카메라에서 떨어진 위치를 가리킵니다.

- 출력

   렌더링 해야 하는 실제 픽셀입니다. 보간에 픽셀 셰이더에 사용할 일부 꼭 짓 점 특성을 포함 합니다.

## <a name="pixel-shader-stage"></a>픽셀 셰이더 단계

[PS (픽셀 셰이더) 단계](pixel-shader-stage--ps-.md) 는 기본 형식에 대 한 보간된 데이터를 받고 색과 같은 픽셀 별 데이터를 생성 합니다.

- 입력

   기 하 도형 셰이더 없이 파이프라인이 구성 된 경우 픽셀 셰이더는 16 개, 32 비트, 4 개의 구성 요소 입력으로 제한 됩니다. 그렇지 않으면 픽셀 셰이더는 최대 32, 32 비트, 4 개의 구성 요소 입력을 사용할 수 있습니다. 픽셀 셰이더 입력 데이터에는 꼭 짓 점 수정을 사용 하거나 사용 하지 않고 보간 될 수 있는 꼭 짓 점 특성이 포함 되거나 기본 상수로 처리 될 수 있습니다. 픽셀 셰이더 입력은 선언 된 보간 모드에 따라 래스터화된 기본 형식의 꼭 짓 점 특성에서 보간됩니다. 기본 형식이 래스터화 전에 잘린 경우에는 클리핑 프로세스 중에도 보간 모드가 적용 됩니다.

- 출력

   픽셀 셰이더는 픽셀을 삭제 하는 경우 최대 8, 32 비트, 4 개의 구성 요소 색 또는 색을 출력할 수 있습니다. 픽셀 셰이더 출력 레지스터 구성 요소를 사용 하려면 먼저 선언 해야 합니다. 각 레지스터에는 고유한 출력 쓰기 마스크가 허용 됩니다.

## <a name="output-merger-stage"></a>출력 병합기 단계

[OM (출력 병합기) 단계](output-merger-stage--om-.md) 에서는 다양 한 유형의 출력 데이터 (픽셀 셰이더 값, 깊이 및 스텐실 정보)를 렌더링 대상의 내용과 깊이/스텐실 버퍼의 내용과 결합 하 여 최종 파이프라인 결과를 생성 합니다.

- 입력

   출력 병합기 입력은 파이프라인 상태, 픽셀 셰이더에 의해 생성 된 픽셀 데이터, 렌더링 대상의 내용 및 깊이/스텐실 버퍼의 내용입니다.

- 출력

   렌더링 된 마지막 픽셀 색입니다.

## <a name="related-topics"></a>관련 항목

- [Direct3D 그래픽 학습 가이드](index.md)

- [계산 파이프라인](compute-pipeline.md)