---
title: "스트리밍 리소스에 사용할 수 있는 주소 공간"
description: "이 섹션에서는 스트리밍 리소스에 사용할 수 있는 가상 주소 공간을 지정합니다."
ms.assetid: 145EB4A3-3910-4126-BC7E-A4CF53E2A098
keywords: "스트리밍 리소스에 사용할 수 있는 주소 공간"
author: PeterTurcan
ms.author: pettur
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
ms.openlocfilehash: a63136f04570c4bf964c461f7296c930f5e168b5
ms.sourcegitcommit: 909d859a0f11981a8d1beac0da35f779786a6889
translationtype: HT
---
# <a name="address-space-available-for-streaming-resources"></a>스트리밍 리소스에 사용할 수 있는 주소 공간


이 섹션에서는 스트리밍 리소스에 사용할 수 있는 가상 주소 공간을 지정합니다.

64비트 운영 체제에서는 40비트 이상의 가상 주소 공간(1테라바이트)를 사용할 수 있습니다.

32비트 운영 체제의 경우 주소 공간은 32비트(4GB)입니다. 32비트 ARM 시스템의 경우 할당에 27비트 이상의 주소 공간(128MB)이 사용될 경우 개별 스트리밍 리소스를 만들지 못할 수 있습니다. 여기에는 하드웨어가 MIP 맵, 압축된 타일 패딩 및 패딩 표면 차원에 사용할 수 있는 주소 공간에 숨겨진 패딩이 2의 제곱까지 포함됩니다.

GPU(그래픽 처리 장치)를 위한 별도의 페이지 테이블이 있는 그래픽 시스템에서 이 주소 공간은 응용 프로그램에서 만드는 GPU 리소스에 대부분 사용할 수 있습니다. 그러나 디스플레이 드라이버에서 만드는 GPU 할당이 동일한 공간에 맞습니다.

CPU와 GPU가 페이지 테이블을 공유하는 향후 시스템에서는 프로세스의 모든 CPU 할당과 GPU 할당이 사용 가능한 주소 공간을 공유합니다.

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>관련 항목


[스트리밍 리소스 생성 매개 변수](streaming-resource-creation-parameters.md)

 

 




