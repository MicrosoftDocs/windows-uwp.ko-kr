---
author: jwmsft
description: XAML에서 사용되는 공백 처리 규칙에 대해 알아봅니다.
title: XAML 및 공백
ms.assetid: 025F4A8E-9479-4668-8AFD-E20E7262DC24
ms.author: jimwalk
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
ms.openlocfilehash: 88d4b155acb38a3ab11cc180d112fb3434af87a0
ms.sourcegitcommit: 909d859a0f11981a8d1beac0da35f779786a6889
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.locfileid: "220162"
---
# <a name="xaml-and-whitespace"></a>XAML 및 공백

\[ Windows 10의 UWP 앱에 맞게 업데이트되었습니다. Windows 8.x 문서는 [보관](http://go.microsoft.com/fwlink/p/?linkid=619132)을 참조하세요. \]

XAML에서 사용되는 공백 처리 규칙에 대해 알아봅니다.

## <a name="whitespace-processing"></a>공백 처리

XML에서와 마찬가지로 XAML의 공백 문자는 공백, 줄 바꿈 및 탭입니다. 이러한 값은 각각 유니코드 값 0020, 000A 및 0009에 해당합니다. XAML 프로세서가 XAML 파일의 요소 사이에서 내부 텍스트를 발견하면 기본적으로 공백이 다음과 같이 정규화됩니다.

-   동아시아 문자 사이의 줄 바꿈 문자가 제거됩니다.
-   모든 공백 문자(공백, 줄 바꿈, 탭)가 공백으로 변환됩니다.
-   연속되는 모든 공백이 삭제되고 단일 공백으로 바뀝니다.
-   시작 태그 바로 다음에 오는 공백이 삭제됩니다.
-   끝 태그 바로 앞에 오는 공백이 삭제됩니다.
-   *동아시아 문자*는 U+20000 - U+2FFFD 및 U+30000 - U+3FFFD 범위의 유니코드 문자 집합으로 정의됩니다. 이 하위 집합을 *한중일 한자*라고도 합니다. 자세한 내용은 http://www.unicode.org(영문)를 참조하세요.

여기서 "기본"이란 의미는 **xml:space** 특성의 기본값이 나타내는 상태에 해당합니다.

### <a name="whitespace-in-inner-text-and-string-primitives"></a>내부 텍스트의 공백 및 문자열 기본 형식

위의 정규화 규칙은 XAML 요소 내에 있는 내부 텍스트에 적용됩니다. 정규화를 수행한 이후 XAML 프로세서는 다음과 같이 모든 내부 텍스트를 적절한 형식으로 변환합니다.

-   속성이 컬렉션 형식이 아니고 직접적인 **Object** 형식이 아니면 XAML 프로세서는 형식 변환기를 사용하여 해당 형식으로 변환하려고 시도합니다. 변환에 실패하면 XAML 구문 분석 오류가 발생합니다.
-   속성이 컬렉션 형식이고 내부 텍스트가 연속적(중간에 요소 태그가 없음)이면 내부 텍스트가 단일 **String**으로 구문 분석됩니다. 컬렉션 형식에 **String**이 허용되지 않는 경우에도 XAML 구문 분석 오류가 발생합니다.
-   속성이 **Object** 형식이면 내부 텍스트가 단일 **String**으로 구문 분석됩니다. **Object** 형식은 **String** 등의 단일 개체를 나타내므로 중간에 요소 태그가 있으면 XAML 구문 분석 오류가 발생합니다.
-   속성이 컬렉션 형식이고 내부 텍스트가 연속적이지 않으면 첫 번째 하위 문자열이 **String**으로 변환되어 컬렉션 항목으로 추가되고, 중간 요소가 컬렉션 항목으로 추가된 다음, 마지막 하위 문자열(있는 경우)이 컬렉션에 세 번째 **String** 항목으로 추가됩니다.

### <a name="whitespace-and-text-content-models"></a>공백 및 텍스트 콘텐츠 모델

실제로 공백 유지는 사용할 수 있는 모든 콘텐츠 모델의 하위 집합하고만 관련이 있습니다. 이 하위 집합은 특정 폼에서 단일 **String** 형식을 사용하거나, 전용 **String** 컬렉션을 사용하거나, 목록, 컬렉션 또는 사전에서 **String**과 다른 형식을 혼합하여 사용할 수 있는 콘텐츠 모델로 구성되어 있습니다.

이러한 콘텐츠 모델은 문자열을 사용할 수 있는 경우에도 콘텐츠 모델 내에 기본적으로 남아 있는 공백을 유효한 문자로 처리하지 않습니다.

### <a name="preserving-whitespace"></a>공백 유지

XAML 프로세서 공백 정규화의 영향을 받지 않고 최종 표시되는 소스 XAML에 공백을 유지하는 방법은 여러 가지가 있습니다.

`xml:space="preserve"`: 이 특성은 공백을 유지해야 하는 요소의 수준에서 지정합니다. 이 방법은 코드 편집기 또는 디자인 화면에서 태그 요소를 시각적인 면에서 직관적인 중첩으로 정렬하기 위해 추가할 수 있는 공백을 포함하여 모든 공백을 유지합니다. 이러한 공백의 렌더링 여부는 포함 요소의 콘텐츠 모델에 따라 다릅니다. 대부분의 개체 모델은 공백을 중요하게 간주하지 않으므로 `xml:space="preserve"`를 루트 레벨에서 지정하지 않는 것이 좋습니다. 이 특성은 문자열 내의 공백을 렌더링하는 요소 또는 중요한 공백 컬렉션인 요소의 수준에서만 설정하는 것이 좋습니다.

엔터티 및 줄 바꿈하지 않는 공백: XAML에서는 텍스트 개체 모델 내에 유니코드 엔터티를 삽입할 수 있습니다. 줄 바꿈하지 않는 공백(UTF-8 인코딩의 경우)과 같은 전용 엔터티를 사용하거나, 줄 바꿈하지 않는 공백 문자를 지원하는 서식 있는 텍스트 컨트롤을 사용할 수도 있습니다. 엔터티의 런타임 출력은 패널 및 여백을 올바르게 사용할 때처럼 일반적인 레이아웃 기능에서 사용할 때보다 더 많은 요인의 영향을 받을 수 있기 때문에 엔터티를 사용하여 들여쓰기 같은 레이아웃 특성을 시뮬레이트할 때는 주의해야 합니다.
