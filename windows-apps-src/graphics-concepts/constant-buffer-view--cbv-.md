---
title: "CBV(상수 버퍼 뷰)"
description: "상수 버퍼에는 셰이더 상수 데이터가 들어 있습니다. 이러한 데이터의 가치는 데이터 변경이 필요할 때까지 데이터가 유지되고 모든 GPU 셰이더에서 데이터에 액세스할 수 있다는 점입니다."
ms.assetid: 99AEC6B0-A43B-4B61-8C3A-ECC8DE1B69A7
keywords: "CBV(상수 버퍼 뷰)"
author: PeterTurcan
ms.author: pettur
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
ms.openlocfilehash: 755c80e895281b93e9e37144526ed1790c63199f
ms.sourcegitcommit: 909d859a0f11981a8d1beac0da35f779786a6889
translationtype: HT
---
# <a name="constant-buffer-view-cbv"></a>CBV(상수 버퍼 뷰)


상수 버퍼에는 셰이더 상수 데이터가 들어 있습니다. 이러한 데이터의 가치는 데이터 변경이 필요할 때까지 데이터가 유지되고 모든 GPU 셰이더에서 데이터에 액세스할 수 있다는 점입니다.

상수 버퍼의 일반적인 데이터는 한 프레임의 그리기 전체에서 일정하게 유지되는 세계 좌표, 프로젝션 및 뷰 매트릭스입니다.

상수 버퍼 레이아웃은 HLSL 레이아웃과 일치해야 합니다([Packing Rules for Constant Variables(상수 변수에 대한 압축 규칙)](https://msdn.microsoft.com/library/windows/desktop/bb509632.aspx) 참조).

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>관련 항목


[뷰](views.md)

 

 



