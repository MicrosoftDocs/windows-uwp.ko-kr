---
description: CLR (공용 언어 런타임) 및 c + +와 같은 기타 프로그래밍 언어의 특정 데이터 형식에 대 한 Windows 런타임 XAML에서 언어 수준 지원을 나열 합니다.
title: XAML 기본 데이터 형식
ms.assetid: D50E6127-395D-4E27-BAA2-2FE627F4B711
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 683c41cba0c002f1482851b3cd3d00a2df352721
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89173767"
---
# <a name="xaml-intrinsic-data-types"></a>XAML 기본 데이터 형식


Windows 런타임 XAML은 CLR (공용 언어 런타임) 및 c + + 등의 다른 프로그래밍 언어에서 자주 사용 되는 여러 데이터 형식에 대 한 언어 수준 지원을 제공 합니다.

Xaml 내장 데이터 형식이 사용 되는 가장 일반적인 장소는 리소스가 XAML 리소스 사전에 정의 된 경우입니다. 여러 값에 사용 하는 숫자와 같이 상수를 정의할 수 있습니다. 또는 문자열 또는 부울 값을 사용 하 여 애니메이션 효과를 주는 storyboarded 애니메이션을 사용할 수 있습니다. 그런 다음 [**ObjectAnimationUsingKeyFrames**](/uwp/api/Windows.UI.Xaml.Media.Animation.ObjectAnimationUsingKeyFrames) 정의의 키 프레임을 채우기 위해 문자열 또는 부울을 나타내는 XAML 개체 요소가 필요 합니다. Windows 런타임 기본 XAML 템플릿에서는 이러한 기술을 모두 사용 합니다.

Windows 런타임 XAML은 이러한 형식에 대 한 언어 수준 지원을 제공 합니다.

| XAML 기본 형식 | Description |
|-------|-------------|
| **x:Boolean**  | CLR 지원의 경우은 [**부울**](/dotnet/api/system.boolean)에 해당 합니다. XAML은 **X:boolean** 의 값을 대/소문자를 구분 하지 않고 구문 분석 합니다. "X:Bool"은 허용 되는 대안이 아닙니다. |
| **x:String**   | CLR 지원의 경우는 [**문자열**](/dotnet/api/system.string)에 해당 합니다. 문자열에 대 한 인코딩은 기본적으로 주변 XML 인코딩으로 설정 됩니다. |
| **x:Double**   | CLR 지원의 경우는 [**Double**](/dotnet/api/system.double)에 해당 합니다. 숫자 값 외에도 **X:double** 의 텍스트 구문은 "NaN" 토큰을 허용 합니다 .이는 레이아웃 동작에 대 한 "자동"을 리소스 값으로 저장할 수 있는 방법입니다. 토큰은 대/소문자를 구분 하 여 처리 됩니다. 의 과학적 표기법 (예: "1 + E06")을 사용할 수 있습니다 `1,000,000` . |
| **x:Int32**    | CLR 지원의 경우은 [**Int32**](/dotnet/api/system.int32)에 해당 합니다. **X:int32** 는 signed로 처리 되며 음의 정수에 빼기 ("-") 기호를 포함할 수 있습니다. XAML에서 로그인 텍스트 구문이 없으면 부호 있는 양수 값을 의미 합니다. |

이러한 XAML 언어 기본 형식은 일반적으로 XAML에서 **x:** 접두사를 사용 하는 개체 요소를 정의 하는 경우에만 해당 됩니다. 다른 모든 XAML 언어 기능은 일반적으로 특성 형식 또는 태그 확장으로 사용 됩니다.

**참고**    규칙에 따라 XAML 및 다른 모든 XAML 언어 요소에 대 한 언어 기본 형식이 "x:" 접두사로 표시 됩니다. 이 규칙은 XAML 언어 요소가 실제 태그에서 일반적으로 사용되는 방식입니다. 이 규칙은 XAML 및 xaml 사양에 대 한 설명서에 있습니다.

## <a name="other-xaml-primitives"></a>다른 XAML 기본 형식

XAML 2009 사양에는 **X:uri** 및 **x:uri**과 같은 다른 XAML 언어 수준 기본 형식에 대 한 설명이 있습니다. 이 항목의 표에 나열 되지 않은 경우 다른 xaml 어휘 또는 XAML 2009 사양에서 정의한 다른 XAML 언어 기본 형식은 현재 Windows 런타임 XAML에서 지원 되지 않습니다.

**참고**    날짜 및 시간 ( [**DateTime**](/uwp/api/Windows.Foundation.DateTime) 또는 [**DateTimeOffset**](/dotnet/api/system.datetimeoffset), [**timespan**](/uwp/api/Windows.Foundation.TimeSpan) 또는 [**SYSTEM.OBJECT**](/dotnet/api/system.timespan)를 사용 하는 속성은 XAML 기본 형식으로 설정 되지 않음) 날짜 및 시간에 대 한 Windows 런타임 XAML 파서에 기본 문자열 변환 동작이 없기 때문에 이러한 속성은 일반적으로 XAML에서 설정 되지 않습니다. 모든 날짜 및 시간 속성의 초기화 값에 대해 페이지 또는 요소가 로드 될 때 실행 되는 코드 숨김과를 사용 해야 합니다.

## <a name="related-topics"></a>관련 항목

* [XAML 개요](xaml-overview.md)
* [XAML 구문 가이드](xaml-syntax-guide.md)
* [스토리보드 애니메이션](../design/motion/storyboarded-animations.md)
 