---
title: "Mipmap을 사용하는 텍스처 필터링"
description: "Mipmap이란 순차적으로 이어지는 텍스처로서 각각 동일한 이미지의 해상도가 점차 낮아지면서 표현됩니다. Mipmap에서 각 이미지 또는 수준의 높이와 너비는 이전 수준보다 2의 제곱 더 작습니다."
ms.assetid: 28E863A2-C776-40E4-8551-9851DF7EC93E
keywords: "Mipmap을 사용하는 텍스처 필터링"
author: PeterTurcan
ms.author: pettur
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
ms.openlocfilehash: 65c775a265f7c5a0b15f76d867a9403308fc7128
ms.sourcegitcommit: 909d859a0f11981a8d1beac0da35f779786a6889
translationtype: HT
---
# <a name="texture-filtering-with-mipmaps"></a>Mipmap을 사용하는 텍스처 필터링


*Mipmap*이란 순차적으로 이어지는 텍스처로서 각각 동일한 이미지의 해상도가 점차 낮아지면서 표현됩니다. Mipmap에서 각 이미지 또는 수준의 높이와 너비는 이전 수준보다 2의 제곱 더 작습니다. Mipmap이 사각형일 필요는 없습니다.

고해상도의 Mipmap 이미지는 사용자와 가까운 객체에 사용됩니다. 저해상도의 이미지는 멀리 보이는 객체에 사용됩니다. Mipmap을 사용하면 메모리 사용량이 늘어나지만 렌더링되는 텍스처의 품질이 향상됩니다.

Direct3D는 연쇄적으로 연결된 표면 사슬로 Mipmap을 표현합니다. 가장 높은 해상도의 텍스처가 사슬의 전면에 위치하고 연쇄적으로 다음 수준의 Mipmap이 연결됩니다. 이후에도 다음 수준의 텍스처가 연쇄적으로 연결되어 가장 낮은 해상도의 Mipmap까지 이어집니다.

다음 그림은 이러한 수준의 예를 나타냅니다. 비트맵 텍스처가 3D 1인칭 게임에서 컨테이너의 기호를 표현합니다. 이것을 Minimap으로 생성하면 가장 높은 해상도의 텍스처가 제일 먼저 나타납니다. 이후 Mipmap 집합에서 텍스처가 이어질 때마다 높이와 너비가 2의 제곱씩 작아집니다. 이 경우 최대 해상도의 Mipmap은 256x256 픽셀입니다. 다음은 128x128입니다. 연쇄 사슬에서 마지막 텍스처는 64x64입니다.

이 기호는 최대 가시 거리일 때의 모습입니다. 사용자가 기호에서 멀어지기 시작하면 게임은 Mipmap 사슬에서 가장 작은 텍스처를 표시하며, 여기에서는 64x64 텍스처입니다.

![64x64 텍스처의 위험 기호 그림](images/mip1.jpg)

이제 사용자가 기호에 가깝게 시점을 이동하면 Mipmap에서 더욱 높은 해상도의 텍스처가 점진적으로 사용됩니다. 다음 그림의 해상도는 128x128입니다.

![128x128 텍스처의 위험 기호 그림](images/mip2.jpg)

사용자의 시점이 기호까지 초소 허용 거리에 도달하면 다음 그림과 같이 가장 높은 해상도의 텍스처가 사용됩니다.

![256x256 텍스처의 위험 기호 그림](images/mip3.jpg)

이는 텍스처의 원근감을 더욱 효율적으로 시뮬레이션할 수 있는 방법입니다. 단일 텍스처를 여러 해상도로 렌더링하기 보다는 각기 다른 해상도에서 다수의 텍스처를 사용하는 것이 더욱 빠릅니다.

Direct3D는 Mipmap 집합에서 원하는 출력에 가장 가까운 해상도인 텍스처를 평가한 후 픽셀을 해당하는 텍셀 공간으로 매핑합니다. 최종 이미지의 해상도가 Mipmap 집합의 두 텍스처 해상도 사이라면 Direct3D가 두 Mipmap의 텍셀을 검사하여 색상 값을 혼합합니다.

Mipmap을 사용하려면 응용 프로그램이 Mipmap 세트를 생성해야 합니다. 응용 프로그램은 현재 텍스처 집합에서 Mipmap 집합을 첫 번째 텍스처로 선택하여 Mipmap을 적용합니다. [텍스처 혼합](texture-blending.md)을 참조하세요.

그런 다음 응용 프로그램이 Direct3D가 텍셀 샘플링에 사용할 필터링 방법을 설정해야 합니다. 가장 빠른 Mipmap 필터링 방법은 Direct3D가 가장 가까운 텍셀을 선택하도록 하는 것입니다. D3DTEXF\_POINT 열거형 값을 사용하여 이 방법을 선택하세요. 응용 프로그램이 D3DTEXF\_LINEAR 열거형 값을 사용하는 경우에는 Direct3D의 필터링 결과가 더 나아질 수 있습니다. 가장 가까운 Mipmap을 선택한 후 텍스처에서 현재 픽셀이 매핑되는 위치를 중심으로 텍셀의 가중 평균을 계산하기 때문입니다.

Mipmap 텍스처는 렌더링 시간을 줄일 목적으로 3D 장면에 사용됩니다. 또한 장면의 현실감을 높이기도 합니다. 단, 종종 많은 양의 메모리가 필요합니다.

**참고** Mipmap 사슬에서 각 표면의 크기는 선행 표면의 크기보다 50% 작습니다. 예를 들어 최상위 Mipmap의 크기가 256x128이라면, 두 번째 Mipmap의 크기가 128x64이고, 세 번째가 64x32인 식으로 이어져 1x1까지 계속 됩니다. 수준에서 다수의 Mipmap 수준을 요청할 수는 없습니다. 사슬의 Mipmap 너비 또는 높이가 1보다 작아질 수도 있기 때문입니다. 쉽게 말해서 최상위 Mipmap 표면의 크기가 4x2라면 수준에서 허용되는 최대 값은 3입니다. 최상위 크기가 4x2이면, 두 번째는 2x1이고, 세 번째는 1x1이 됩니다. 수준에서 값을 3보다 크게 설정하면 두 번째 Mipmap의 높이가 분수 값이 되어 사용할 수 없습니다.

 

Direct3D는 Mipmap 텍스처 필터링을 자동 실행할 수 있습니다. 응용 프로그램은 Mipmap 사슬을 수동으로 횡단하여 사슬의 각 표면에 비트맵 데이터를 로드할 수 있습니다. 종종 사슬을 횡단하는 이유는 단 하나입니다. 텍스처 생성 시 Mipmap을 자동으로 생성하게 하면 Mipmap이 비디오 메모리에 상주하여 하드웨어 필터링을 이용하기 때문입니다.

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>관련 항목


[텍스처 필터링](texture-filtering.md)

 

 



