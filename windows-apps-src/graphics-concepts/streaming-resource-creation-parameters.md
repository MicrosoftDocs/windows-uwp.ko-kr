---
title: 스트리밍 리소스 생성 매개 변수
description: 스트리밍 리소스로 만들 수 있는 Direct3D 리소스의 유형에는 몇 가지 제약이 있습니다.
ms.assetid: 6FC5AD93-6F47-479E-947C-895C99B427BC
keywords:
- 스트리밍 리소스 생성 매개 변수
author: michaelfromredmond
ms.author: mithom
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
ms.localizationpriority: medium
ms.openlocfilehash: f2a17f69ae0353bb7682a1dbfa48d5909f48d4aa
ms.sourcegitcommit: 0ab8f6fac53a6811f977ddc24de039c46c9db0ad
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/15/2018
ms.locfileid: "1652992"
---
# <a name="streaming-resource-creation-parameters"></a>스트리밍 리소스 생성 매개 변수


스트리밍 리소스로 생성할 수 있는 Direct3D 리소스의 유형에는 몇 가지 제약이 있습니다.

<span id="Supported-Resource-Type"></span><span id="supported-resource-type"></span><span id="SUPPORTED-RESOURCE-TYPE"></span>**지원되는 리소스 유형**  
Texture2D\[Array\](Texture2D\[Array\]의 변형인 TextureCube\[Array\] 포함) 또는 버퍼.

**지원되지 않음:  **Texture1D\ [Array\].

<span id="Supported-Resource-Usage"></span><span id="supported-resource-usage"></span><span id="SUPPORTED-RESOURCE-USAGE"></span>**지원되는 리소스 사용**  
기본 사용.

**지원되지 않음:  **동적, 준비 또는 변경 불가능.

<span id="Supported-Resource-Misc-Flags"></span><span id="supported-resource-misc-flags"></span><span id="SUPPORTED-RESOURCE-MISC-FLAGS"></span>**지원되는 리소스 기타 플래그**  
타일식, 즉 스트리밍(정의상), 텍스처 큐브, 간접 인수 그리기, 버퍼에서 원시 뷰 허용, 구조화된 버퍼, 리소스 클램프 또는 밉 생성.

**지원되지 않음:  **공유, 공유 키 입력 뮤텍스, GDI 호환, 공유 NT 핸들, 제한된 콘텐츠, 공유 리소스 제한, 공유 리소스 드라이버 제한, 보호 또는 타일 풀.

<span id="Supported-Bind-Flags"></span><span id="supported-bind-flags"></span><span id="SUPPORTED-BIND-FLAGS"></span>**지원되는 바인딩 플래그**  
셰이더 리소스로 바인딩, 렌더링 대상, 깊이 스텐실 또는 순서가 지정되지 않은 액세스.

**지원되지 않음:  **상수 버퍼로 바인딩, 꼭짓점 버퍼(타일식 버퍼를 SRV/UAV/RTV로 바인딩은 지원됨), 인덱스 버퍼, 출력 스트리밍, 디코더 또는 비디오 인코더.

<span id="Supported-Formats"></span><span id="supported-formats"></span><span id="SUPPORTED-FORMATS"></span>**지원되는 형식**  
타일링 여부와 상관없이 지정된 구성에 사용 가능한 모든 형식(일부 예외).

<span id="Supported-Sample-Description--Multisample-count--quality-"></span><span id="supported-sample-description--multisample-count--quality-"></span><span id="SUPPORTED-SAMPLE-DESCRIPTION--MULTISAMPLE-COUNT--QUALITY-"></span>**지원되는 샘플 설명(다중 샘플 수, 품질)**  
타일링 여부와 상관없이 지정된 구성에 대해 지원되는 모든 설명(일부 예외).

<span id="Supported-Width-Height-MipLevels-ArraySize"></span><span id="supported-width-height-miplevels-arraysize"></span><span id="SUPPORTED-WIDTH-HEIGHT-MIPLEVELS-ARRAYSIZE"></span>**지원되는 너비/높이/MipLevel/ArraySize**  
Direct3D에 의해 지원되는 전체 범위. 스트리밍 리소스는 비 스트리밍 리소스에 적용되는 총 메모리 크기에 대한 제한이 없습니다. 스트리밍 리소스는 전체 가상 주소 공간 제한에 의해서만 제한됩니다. [스트리밍 리소스에 사용할 수 있는 주소 공간](address-space-available-for-streaming-resources.md)을 참조하세요.

타일 풀 메모리의 초기 내용은 정의되지 않습니다.

## <a name="span-idin-this-sectionspanin-this-section"></a><span id="in-this-section"></span>이 섹션의 내용


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
<td align="left"><p><a href="address-space-available-for-streaming-resources.md">스트리밍 리소스에 사용할 수 있는 주소 공간</a></p></td>
<td align="left"><p>이 섹션에서는 스트리밍 리소스에 사용할 수 있는 가상 주소 공간을 지정합니다.</p></td>
</tr>
</tbody>
</table>

 

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>관련 항목


[스트리밍 리소스 만들기](creating-streaming-resources.md)

 

 




