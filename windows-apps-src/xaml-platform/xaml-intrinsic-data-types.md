---
author: jwmsft
description: CLR(공용 언어 런타임)과 C++ 등 다른 프로그래밍 언어의 특정 데이터 형식에 제공되는 Windows 런타임에 대한 XAML의 언어 수준 지원이 나열됩니다.
title: XAML 기본 데이터 형식
ms.assetid: D50E6127-395D-4E27-BAA2-2FE627F4B711
ms.author: jimwalk
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 18cf7a63dea7a1913293e5cd174b8f6c69b5baf6
ms.sourcegitcommit: d0e836dfc937ebf7dfa9c424620f93f3c8e0a7e8
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/26/2018
ms.locfileid: "5654391"
---
# <a name="xaml-intrinsic-data-types"></a>XAML 기본 데이터 형식


Windows 런타임에 대한 XAML은 CLR(공용 언어 런타임)과 C++ 등의 다른 프로그래밍 언어에서 자주 사용되는 primitive인 여러 데이터 형식에 언어 수준 지원을 제공합니다.

XAML 내부 데이터 형식 사용을 볼 수 있는 가장 일반적인 경우는 리소스가 XAML 리소스 사전에서 정의되는 경우입니다. 여러 값에 사용하는 수와 같은 상수를 정의할 수 있습니다. 또는 문자열이나 부울 값을 사용하여 애니메이션 효과를 주는 스토리보드 애니메이션을 사용할 수 있으며, 이 경우 [**ObjectAnimationUsingKeyFrames**](https://msdn.microsoft.com/library/windows/apps/br210320) 정의의 키 프레임을 채울 문자열이나 부울을 나타내는 XAML 개체 요소가 필요합니다. Windows 런타임 기본 XAML 템플릿은 이러한 방법을 둘 다 사용합니다.

Windows 런타임용 XAML이 언어 수준 지원을 제공하는 형식은 다음과 같습니다.

| XAML primitive | 설명 |
|-------|-------------|
| **x:Boolean**  | CLR 지원의 경우 [**Boolean**](https://msdn.microsoft.com/library/windows/apps/xaml/system.boolean.aspx)에 해당합니다. XAML은 **x:Boolean** 값을 대/소문자 구분 없이 구문 분석합니다. "x:Bool"로 대체할 수 없습니다. |
| **x:String**   | CLR 지원의 경우 [**String**](https://msdn.microsoft.com/library/windows/apps/xaml/system.string.aspx)에 해당합니다. 문자열의 인코딩 기본값은 주위의 XML 인코딩입니다. |
| **x:Double**   | CLR 지원의 경우 [**Double**](https://msdn.microsoft.com/library/windows/apps/xaml/system.double.aspx)에 해당합니다. **x:Double**의 텍스트 구문에는 숫자 값뿐만 아니라 토큰 "NaN"도 허용됩니다. 이 토큰을 사용하면 레이아웃 동작에 대해 "Auto"를 리소스 값으로 저장할 수 있습니다. 토큰은 대/소문자를 구분하여 처리됩니다. 과학적 표기법을 사용할 수 있습니다(예: `1,000,000`에 대해 "1+E06"). |
| **x:Int32**    | CLR 지원의 경우 [**Int32**](https://msdn.microsoft.com/library/windows/apps/xaml/system.int32.aspx)에 해당합니다. **x:Int32**가 기호 지정 값으로 처리되며 음수에 빼기("-") 기호를 포함할 수 있습니다. XAML에서 텍스트 구문에 기호가 없으면 양수 값을 의미합니다. |

이러한 XAML 언어 primitive는 대개 사용자 XAML에서 **x:** 접두사가 사용되는 개체 요소를 정의하는 경우에만 사용됩니다. 다른 모든 XAML 언어 기능은 일반적으로 특성 폼에 사용되거나 태그 확장으로 사용됩니다.

**참고**통상적으로 XAML 및 다른 모든 XAML 언어 요소에 대 한 언어 primitive "x:" 접두사와 함께 표시 됩니다. 이는 XAML 언어 요소가 실제 태그에서 일반적으로 어떻게 사용되는지를 보여 줍니다. 이는 XAML용 설명서 및 XAML 사양에서도 준수됩니다.

## <a name="other-xaml-primitives"></a>기타 XAML primitive

XAML 2009 사양에서는 **x:Uri** 및 **x:Single** 같은 다른 XAML 언어 수준 primitive를 표시합니다. 이 항목의 표에 나열되어 있지 않은 기타 XAML 어휘 또는 XAML 2009 사양에서 정의된 기타 XAML 언어 기본 요소는 현재 Windows 런타임에 대한 XAML에서 지원되지 않습니다.

**참고**날짜 및 시간 ( [**DateTime**](https://msdn.microsoft.com/library/windows/apps/br206576) 또는 [**DateTimeOffset**](https://msdn.microsoft.com/library/windows/apps/xaml/system.datetimeoffset.aspx), [**TimeSpan**](https://msdn.microsoft.com/library/windows/apps/br225996) 또는 [**System.TimeSpan**](https://msdn.microsoft.com/library/windows/apps/xaml/system.timespan.aspx)를 사용 하는 속성)은 XAML 기본 되지 않습니다. Windows 런타임 XAML 파서에 날짜 및 시간에 대한 기본 from-string 변환 동작이 없기 때문에 일반적으로 이러한 속성은 XAML에서 설정할 수 없습니다. 날짜 및 시간 속성의 초기화 값에는 페이지 또는 요소가 로드될 때 실행되는 코드 숨김을 사용해야 합니다.

## <a name="related-topics"></a>관련 항목

* [XAML 개요](xaml-overview.md)
* [XAML 구문 가이드](xaml-syntax-guide.md)
* [스토리보드 애니메이션](https://msdn.microsoft.com/library/windows/apps/mt187354)
 

