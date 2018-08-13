---
title: 텍스처 뷰
description: Direct3D에서 텍스처 리소스는 메모리 리소스에 대한 하드웨어 해석 메커니즘인 뷰를 통해 액세스됩니다.
ms.assetid: 18DABFCE-8A36-4C4E-B08E-10428B05D701
keywords:
- 텍스처 뷰
author: michaelfromredmond
ms.author: mithom
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
ms.localizationpriority: medium
ms.openlocfilehash: 78263272c0a150bf7ccf06aa29cc66f13770ad19
ms.sourcegitcommit: 897a111e8fc5d38d483800288ad01c523e924ef4
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/13/2018
ms.locfileid: "1043632"
---
# <a name="texture-views"></a>텍스처 뷰


Direct3D에서 텍스처 리소스는 메모리 내 리소스의 하드웨어 해석 메커니즘인 뷰를 사용하여 액세스됩니다. 뷰에서는 특정 파이프라인 단계가 응용 프로그램에서 원하는 표현으로 필요한 [하위 리소스](resource-types.md)에만 액세스할 수 있습니다.

뷰는 무형식(typeless) 리소스라는 개념을 지원합니다. 무형식 리소스란 특정 데이터 형식이 아닌 특정 크기로 생성된 리소스를 말합니다. 데이터는 파이프라인에 바인딩될 때 동적으로 해석됩니다.

다음 그림은 셰이더 리소스 뷰를 생성하여 텍스처가 6개인 2D 텍스처 배열을 셰이더 리소스로 바인딩하는 예입니다. 그러면 리소스 주소가 텍스처 배열로 지정됩니다. (참고: 하위 리소스는 파이프라인에 입력 및 출력으로 동시에 바인딩할 수 없습니다)

![텍스처가 6개인 텍스처 배열 그림](images/d3d10-cube-texture-faces.png)

2D 텍스처 배열을 렌더링 대상으로 사용할 때는 Mipmap 수준(이번 예의 3번)에서 2D 텍스처 배열(이번 예의 6번)로 리소스를 볼 수 있습니다.

CreateRenderTargetView를 호출하여 렌더링 대상을 위한 뷰 객체를 생성합니다. 그런 다음 OMSetRenderTargets를 호출하여 렌더링 대상 뷰를 파이프라인으로 설정합니다. Draw를 호출한 후 RenderTargetArrayIndex를 사용하고 배열에서 적합한 텍스처를 인덱싱하여 렌더링 대상으로 렌더링합니다 하위 리소스(Mipmap 수준, 배열 인덱스 조합)를 사용하여 모든 하위 리소스 배열로 바인딩할 수 있습니다. 따라서 두 번째 Mipmap 수준으로 바인딩한 후 원한다면 다음 그림과 같이 이 특정 Mipmap 수준만 업데이트할 수 있습니다.

![텍스트 배열에서 두 번째 Mipmap 수준으로만 바인딩하는 그림](images/d3d10-cube-texture-faces-subresource.png)

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>관련 항목


[리소스](resources.md)

 

 




