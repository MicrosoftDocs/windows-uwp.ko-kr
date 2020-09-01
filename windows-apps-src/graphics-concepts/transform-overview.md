---
title: 변형 개요
description: 행렬 변환은 3D 그래픽의 낮은 수준 수치를 많이 처리 합니다.
ms.assetid: B5220EE8-2533-4B55-BF58-A3F9F612B977
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 0f8efd1984ae8a726870bd8e7aaa3960baf91218
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89156217"
---
# <a name="transform-overview"></a>변형 개요

행렬 변환은 3D 그래픽의 낮은 수준 수치를 많이 처리 합니다.

Geometry 파이프라인은 버텍스를 입력으로 사용 합니다. 변환 엔진은 세계, 뷰 및 프로젝션 변환을 꼭 짓 점에 적용 하 고, 결과를 클리핑 하 고, 모든 것을 래스터 라이저에 전달 합니다.

| 변환 및 공간                           | Description                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                       |
|-----------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| 모델 공간의 모델 좌표              | 파이프라인의 헤드에서 모델의 꼭 짓 점은 로컬 좌표계를 기준으로 하 여 선언 됩니다. 이는 로컬 원점과 방향입니다. 이러한 좌표 방향은 종종 *모델 공간*이라고 합니다. 개별 좌표를 *모델 좌표*라고 합니다.                                                                                                                                                                                                                                                                                                                                                                                                                                                                      |
| 세계 공간으로 변형              | Geometry 파이프라인의 첫 번째 단계는 모델의 꼭 짓 점을 로컬 좌표계에서 장면의 모든 개체에 사용 되는 좌표계로 변환 합니다. 꼭지점을 reorienting 하는 프로세스를 [세계 변환](world-transform.md)이라고 하며,이를 통해 모델 공간에서 *세계 공간*이라는 새로운 방향으로 변환 합니다. 세계 공간의 각 꼭 짓 점은 *세계 좌표*를 사용 하 여 선언 됩니다.                                                                                                                                                                                                                                                                                                                           |
| 보기 공간으로 변환 보기 (카메라 공간) | 다음 단계에서 3D 세계를 설명 하는 꼭 짓 점은 카메라와 관련 하 여 지향 됩니다. 즉, 응용 프로그램은 장면의 관점을 선택 하 고, 세계 공간 좌표는 카메라 보기를 기준으로 재배치 되 고 회전 하며, 세계 공간을 *보기 공간* ( *카메라 공간이*라고도 함)으로 전환 합니다. 이는 세계 공간에서 공간을 볼 수 있도록 변환 하는 [뷰 변환](view-transform.md)입니다.                                                                                                                                                                                                                                                                                                                        |
| 프로젝션 공간으로 변환 프로젝션    | 다음 단계는 뷰 공간에서 프로젝션 공간으로 변환 하는 [프로젝션 변환](projection-transform.md)입니다. 파이프라인의이 부분에서 개체는 일반적으로 뷰어와의 거리를 기준으로 크기를 조정 하 여 장면에 깊이의 효과를 제공 합니다. 개체 닫기가 먼 개체 보다 크게 나타나도록 합니다. 간단히 하기 위해이 설명서에서는 프로젝션 변환 후 꼭 짓 점이 *프로젝션 공간*으로 있는 공간을 나타냅니다. 일부 그래픽 책은 *사후 큐브 뷰 같은 공간*으로 프로젝션 공간을 참조할 수 있습니다. 일부 프로젝션 변환은 장면에서 개체의 크기를 조정 하지 않습니다. 이와 같은 프로젝션을 *관계* 또는 *직교 프로젝션*이 라고도 합니다. |
| 화면 공간에서 클리핑                      | 파이프라인의 마지막 부분에서 화면에 표시 되지 않는 꼭 짓 점이 제거 되므로 래스터 라이저는 볼 수 없는 항목에 대 한 색 및 음영을 계산 하는 데 시간이 걸리지 않습니다. 이 프로세스를 *클리핑*이라고 합니다. 클리핑 후에는 나머지 꼭지점이 뷰포트 매개 변수에 따라 크기가 조정 되 고 화면 좌표로 변환 됩니다. 장면이 래스터화 될 때 화면에 표시 되는 결과 꼭지점이 *화면 공간*에 있습니다.                                                                                                                                                                                                                                                    |

 

변환은 한 좌표 공간에서 다른 좌표 공간으로 개체 기 하 도형을 변환 하는 데 사용 됩니다. Direct3D는 행렬을 사용 하 여 3D 변환을 수행 합니다. 행렬 3D 변환 만들기 행렬을 결합 하 여 여러 변환을 포함 하는 단일 행렬을 생성할 수 있습니다.

모델 공간, 세계 공간 및 보기 공간 간에 좌표를 변형할 수 있습니다.

-   [세계 변형](world-transform.md) -모델 공간에서 세계 공간으로 변환 합니다.
-   [변환 보기](view-transform.md) -공간을 보기 위해 세계 공간에서 변환 합니다.
-   [프로젝션 변환](projection-transform.md) -뷰 공간에서 프로젝션 공간으로 변환 합니다.

## <a name="span-idmatrix_transformsspanspan-idmatrix_transformsspanspan-idmatrix_transformsspanmatrix-transforms"></a><span id="Matrix_Transforms"></span><span id="matrix_transforms"></span><span id="MATRIX_TRANSFORMS"></span>행렬 변환


3D 그래픽을 사용 하는 응용 프로그램에서는 기하학적 변형을 사용 하 여 다음 작업을 수행할 수 있습니다.

-   다른 개체를 기준으로 개체의 위치를 표현 합니다.
-   개체를 회전 하 고 크기를 조정 합니다.
-   보기 위치, 방향 및 큐브 뷰를 변경 합니다.

다음 수식에 나와 있는 것 처럼 4x4 행렬을 사용 하 여 점 (x, y, z)을 다른 점 (x, y, z)으로 변환할 수 있습니다.

![점을 다른 점으로 변형 하는 수식](images/matmult.png)

(X, y, z)에서 다음 수식을 수행 하 고 행렬에서 (x ', y ', z ') 점을 생성 합니다.

![새 점의 수식](images/matexpnd.png)

가장 일반적인 변환은 변환, 회전 및 크기 조정입니다. 이러한 효과를 생성 하는 행렬을 단일 행렬로 결합 하 여 여러 변환을 한 번에 계산할 수 있습니다. 예를 들어 일련의 요소를 변환 하 고 회전 하는 단일 행렬을 만들 수 있습니다.

행렬은 행 열 순서로 작성 됩니다. 균일 한 배율 이라고 하는 각 축을 따라 꼭 짓 점을 고르게 확장 하는 행렬은 수학적 표기법을 사용 하 여 다음 행렬로 표시 됩니다.

![균일 한 크기 조정을 위한 행렬의 수식](images/matrix.png)

C + +에서 Direct3D는 행렬 구조체를 사용 하 여 매트릭스를 2 차원 배열로 선언 합니다. 다음 예에서는 균일 한 크기 조정 매트릭스 (배율 인수 "s")로 작동 하도록 [**D3DMATRIX**](/windows/desktop/direct3d9/d3dmatrix) 구조체를 초기화 하는 방법을 보여 줍니다.

```cpp
D3DMATRIX scale = {
    5.0f,            0.0f,            0.0f,            0.0f,
    0.0f,            5.0f,            0.0f,            0.0f,
    0.0f,            0.0f,            5.0f,            0.0f,
    0.0f,            0.0f,            0.0f,            1.0f
};
```

## <a name="span-idtranslatespanspan-idtranslatespanspan-idtranslatespantranslate"></a><span id="Translate"></span><span id="translate"></span><span id="TRANSLATE"></span>번역하기


다음 수식은 point (x, y, z)를 새 점 (x ', y ', z ')으로 변환 합니다.

![새 점에 대 한 번역 행렬의 수식](images/transl8.png)

C + +에서 번역 행렬을 수동으로 만들 수 있습니다. 다음 예제는 꼭 짓 점을 변환 하는 행렬을 만드는 함수의 소스 코드를 보여 줍니다.

```cpp
D3DXMATRIX Translate(const float dx, const float dy, const float dz) {
    D3DXMATRIX ret;

    D3DXMatrixIdentity(&ret);
    ret(3, 0) = dx;
    ret(3, 1) = dy;
    ret(3, 2) = dz;
    return ret;
}    // End of Translate
```

## <a name="span-idscalespanspan-idscalespanspan-idscalespanscale"></a><span id="Scale"></span><span id="scale"></span><span id="SCALE"></span>배율을


다음 수식은 x, y, z (x, y, z)를 새 점 (x ', y ', z ')에 대 한 임의 값으로 크기를 조정 합니다.

![새 점의 크기 조정 행렬 수식](images/matscale.png)

## <a name="span-idrotatespanspan-idrotatespanspan-idrotatespanrotate"></a><span id="Rotate"></span><span id="rotate"></span><span id="ROTATE"></span>시키거나


여기에서 설명 하는 변환은 왼손 좌표계에 대 한 것 이므로 다른 곳에서 확인 한 변환과 다를 수 있습니다.

다음 수식은 x 축 주위에 점 (x, y, z)을 회전 하 여 새 점 (x ', y ', z ')을 생성 합니다.

![새 점의 x 회전 행렬 수식](images/matxrot.png)

다음 수식은 y 축을 중심으로 점을 회전 합니다.

![새 점의 y 회전 행렬 수식](images/matyrot.png)

다음 수식은 z 축을 중심으로 점을 회전 합니다.

![새 점의 z 회전 행렬 수식](images/matzrot.png)

이 예제 매트릭스에서 그리스 문자 테타는 회전 각도 (라디안)를 나타냅니다. 각도는 회전 축을 따라 원점 쪽을 볼 때 시계 방향으로 측정 됩니다.

다음 코드에서는 X 축에 대 한 회전을 처리 하는 함수를 보여 줍니다.

```cpp
    // Inputs are a pointer to a matrix (pOut) and an angle in radians.
    float sin, cos;
    sincosf(angle, &sin, &cos);  // Determine sin and cos of angle

    pOut->_11 = 1.0f; pOut->_12 =  0.0f;   pOut->_13 = 0.0f; pOut->_14 = 0.0f;
    pOut->_21 = 0.0f; pOut->_22 =  cos;    pOut->_23 = sin;  pOut->_24 = 0.0f;
    pOut->_31 = 0.0f; pOut->_32 = -sin;    pOut->_33 = cos;  pOut->_34 = 0.0f;
    pOut->_41 = 0.0f; pOut->_42 =  0.0f;   pOut->_43 = 0.0f; pOut->_44 = 1.0f;

    return pOut;
}
```

## <a name="span-idconcatenating_matricesspanspan-idconcatenating_matricesspanspan-idconcatenating_matricesspanconcatenating-matrices"></a><span id="Concatenating_Matrices"></span><span id="concatenating_matrices"></span><span id="CONCATENATING_MATRICES"></span>행렬 연결


행렬을 사용 하는 경우의 이점 중 하나는 두 개 이상의 행렬을 곱하여 효과를 결합할 수 있다는 점입니다. 즉, 모델을 회전 한 다음 특정 위치로 변환 하려면 두 행렬을 적용 하지 않아도 됩니다. 대신 회전 및 변환 매트릭스를 곱하여 모든 효과를 포함 하는 복합 행렬을 생성 합니다. 행렬 연결 이라고 하는이 프로세스는 다음 수식을 사용 하 여 작성할 수 있습니다.

![행렬 연결 수식](images/matrxcat.png)

이 수식에서 C는 만들어지는 복합 행렬 이며, M ₁ ~ Mn는 개별 행렬입니다. 대부분의 경우에는 두 개 또는 세 개의 행렬만 연결 되지만 제한이 없습니다.

행렬 곱셈을 수행 하는 순서는 중요 합니다. 위의 수식은 행렬 연결의 왼쪽에서 오른쪽으로의 규칙을 반영 합니다. 즉, 복합 행렬을 만드는 데 사용 하는 행렬의 표시 효과는 왼쪽에서 오른쪽 순서로 발생 합니다. 일반적인 세계 행렬은 다음 예제에 나와 있습니다. Stereotypical 비행 saucer의 세계 행렬을 만드는 중 이라고 가정 합니다. 모델 공간의 y 축 인 중심 주위에 비행 saucer를 회전 하 고 장면의 다른 위치로 변환 하는 것이 좋습니다. 이 효과를 달성 하려면 먼저 회전 행렬을 만든 다음, 다음 수식에 나와 있는 것 처럼 변환 매트릭스에 곱합니다.

![회전 행렬 및 변환 행렬을 기준으로 회전 하는 수식](images/wrldexpl.png)

이 수식에서 R<sub>y</sub> 는 y 축에 대 한 회전에 대 한 행렬 이며 T<sub>w</sub> 는 세계 좌표에서 특정 위치로의 변환입니다.

두 스칼라 값을 곱하는 것과 달리 행렬 곱셈은 비가 환 되지 않으므로 행렬을 곱하는 순서는 중요 합니다. 행렬을 반대 방향으로 곱하여 비행 saucer를 전 세계 공간 위치로 변환한 다음 전 세계를 중심으로 회전 하는 것이 시각적 효과가 있습니다.

어떤 종류의 행렬을 만들고 있는지에 관계 없이, 예상 되는 효과를 얻기 위해 왼쪽에서 오른쪽으로 규칙을 기억할 수 있습니다.

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>관련 항목


[변환](transforms.md)

 

 