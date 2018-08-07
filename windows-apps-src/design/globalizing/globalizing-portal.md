---
author: stevewhims
Description: Learn about the benefits of globalizing and localizing your app, and exactly what these terms mean.
Search.SourceType: Video
title: 세계화 및 지역화
ms.assetid: c0791eec-5bb8-4a13-8977-61d7d98e35ce
label: Intro
template: detail.hbs
ms.author: stwhi
ms.date: 10/18/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp, 세계화, 현지화, 지역화
ms.localizationpriority: medium
ms.openlocfilehash: 06931c53ba2670fd27fbf94a204ffd62a97bb184
ms.sourcegitcommit: f9a4854b6aecfda472fb3f8b4a2d3b271b327800
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 12/12/2017
ms.locfileid: "1394782"
---
# <a name="globalization-and-localization"></a>세계화 및 지역화

Windows는 전 세계에서 언어, 지역, 문화가 다양한 사용자가 사용합니다. 사용자는 다양한 국가와 지역에서 다양한 언어를 사용합니다. 일부 사용자는 2개국어 이상을 사용합니다. 따라서 앱은 언어, 지역 및 문화 시스템 설정이 다양하게 조합된 구성에서 실행됩니다. *세계화* 및 *지역화*를 사용하여 앱을 쉽게 조정할 수 있도록 디자인하면 앱의 잠재 시장을 늘릴 수 있습니다.

이 동영상에는 세계에 판매하기 위해 앱을 준비하는 방법을 간략히 소개하는 [세계화 및 지역화 소개](https://channel9.msdn.com/Blogs/One-Dev-Minute/Introduction-to-globalization-and-localization)가 포함되어 있습니다.

**세계화**는 문화에 따라 변경 또는 사용자 지정할 필요 없이 서로 다른 세계 시장(다른 언어 및 문화로 구성된 시스템)에서 적절하게 작동하는 방식으로 앱을 디자인하고 개발하는 과정입니다.

- 문자열을 조정할 때는 문화를 고려하십시오. 예를 들어 문자열을 비교하기 전에 변경하지 않습니다.
- 현재 문화에 적합한 달력을 사용합니다.
- 숫자, 날짜, 시간, 통화 등 국가 또는 지역에 올바른 형식의 데이터를 표시하도록 세계화 API를 사용합니다.
- 문화에 따라 텍스트 및 기타 데이터를 정렬하기 위한 다른 규칙이 존재한다는 점을 고려합니다.

앱을 지원하도록 결정한 모든 문화에서 코드가 동등하게 잘 작동해야 합니다. *모든* 언어, 지역 또는 문화에서 코드가 동등하게 잘 작동하는 것이 이상적입니다. 앱의 기능을 세계화하기 위한 가장 효율적인 방법은 문화/로캘 개념을 사용하는 것입니다. 문화/로캘은 특정 언어 및 지역과 관련된 규칙 및 데이터 모음입니다. 이러한 규칙 및 데이터에는 다음과 같은 정보가 포함됩니다.

- 문자 분류
- 쓰기 시스템
- 날짜 및 시간 형식
- 숫자, 통화, 무게, 측정 규칙
- 정렬 규칙

**지역화**는 세계화된 앱을 현지화하기 위해 준비하고 앱이 현지화되기 위한 준비가 되었는지 확인하는 과정입니다. 앱이 올바르게 현지화되도록 하면 나중에 현지화 과정에서 앱의 작동 문제를 다루지 않아도 됩니다. 지역화할 앱의 가장 중요한 속성은 실행 코드가 앱의 지역화 리소스와 깔끔하게 분리된 것입니다.

- 다른 언어로 번역된 문자열은 길이가 상당히 다를 수 있습니다. 따라서 레이블 및 텍스트 입력 컨트롤에 다른 텍스트 길이 및 글꼴 크기를 사용할 수 있도록 UI를 디자인합니다.
- 이미지에 텍스트 및/또는 문화에 민감한 자료를 포함하지 않도록 하십시오.
- 앱 코드 및 마크업에 문자열 및 문화 종속 이미지를 하드 코딩하지 않습니다. 대신 앱이 작성된 이진 코드와 개별적으로 다른 현지 시장에 적용될 수 있도록 문자열과 이미지 리소스로 저장합니다.
- 앱을 의사 지역화하여 현지화 문제를 공개합니다.

**지역화**는 앱을 지원하려고 하는 특정 현지 시장의 언어, 문화, 정치적 요구 사항에 적합하도록 앱의 현지화 리소스를 적용하고 변환하는 과정입니다. 앱의 현지화 과정에 들어설 때쯤 앱을 현지화할 수 있는 경우 이 과정에서 어떠한 논리도 수정할 필요가 없습니다.

- 앱의 문자열 리소스 및 기타 자산을 신흥 시장에 맞게 변환합니다.
- 문화 종속 이미지를 필요에 따라 수정합니다.
- 언어와는 별도로 사용자의 지역에 따라 파일이 다를 수도 있습니다. 예를 들어, 사용자의 지역에 따라 지도에 여러 경계가 있을 수 있지만 레이블은 사용자가 원하는 언어를 따라야 합니다.

대부분의 지역화 팀은 이 과정을 돕기 위해 특별한 도구를 사용합니다. 예를 들어 반복되는 텍스트를 위해 번역을 재사용합니다.

| 문서 | 설명 |
|---------|-------------|
| [세계화 지침](guidelines-and-checklist-for-globalizing-your-app.md) | 다른 언어 및 문화 구성을 가진 시스템에서 올바르게 작동하는 방식으로 앱을 디자인하고 개발합니다. |
| [사용자 프로필 언어와 앱 매니페스트 언어 이해](manage-language-and-region.md) | 이 항목에서는 "사용자 프로필 언어 목록", "앱 매니페스트 언어 목록" 및 "앱 런타임 언어 목록"이라는 용어를 정의합니다. 이 항목 및 이 기능 영역의 다른 항목에서 이러한 용어를 사용할 예정이므로 이 용어의 의미를 알아 두는 것이 중요합니다. |
| [날짜/시간 숫자 형식 세계화](use-global-ready-formats.md) | 날짜, 시간, 숫자, 전화 번호 및 통화의 형식을 적절하게 지정하여 세계화를 대비한 앱을 디자인합니다. 그러면 나중에 전 세계 지역/국가의 다른 문화, 지역 및 언어에 맞춰 앱을 조정할 수 있습니다. |
| [날짜 및 시간 형식 지정 템플릿 및 패턴 사용](use-patterns-to-format-dates-and-times.md) | 날짜 및 시간을 원하는 형식으로 정확히 표시하려면 [**Windows.Globalization.DateTimeFormatting**](/uwp/api/windows.globalization.datetimeformatting?branch=live) 네임스페이스와 사용자 지정 템플릿에 클래스를 사용하세요. |
| [레이아웃 및 글꼴 조정, RTL 지원](adjust-layout-and-fonts--and-support-rtl.md) | RTL(오른쪽에서 왼쪽) 방향으로 읽는 것을 포함하여 여러 언어의 레이아웃과 글꼴을 지원하기 위해 앱을 디자인합니다. |
| [NumeralSystem 값](glob-numeralsystem-values.md) | 이 항목에서는 [**Windows.Globalization**](/uwp/api/windows.globalization?branch=live) 네임스페이스의 다양한 **NumeralSystem** 속성 클래스에 대해 사용할 수 있는 값을 나열합니다. |
| [자신의 앱을 현지화 가능하도록 만들 수 있습니다.](prepare-your-app-for-localization.md) | 현지화된 앱은 앱에 기능적인 결함은 다루지 않고 다른 시장, 언어 또는 지역에 맞게 현지화할 수 있습니다. 지역화할 앱의 가장 중요한 속성은 실행 코드가 앱의 지역화 리소스와 깔끔하게 분리된 것입니다. |
| [국가별 글꼴](loc-international-fonts.md) | 이 항목에서는 미국 영어 이외의 언어로 지역화되어 UWP 앱에 사용할 수 있는 글꼴을 나열합니다. |
| [양방향 텍스트를 위한 앱 디자인](design-for-bidi-text.md) | 왼쪽에서 오른쪽 및 오른쪽에서 왼쪽 쓰기 시스템으로 구성된 스크립트를 결합할 수 있도록 양방향 텍스트 지원을 제공하도록 앱을 디자인합니다. |
| [다국어 앱 도구 키트 4.0 사용](use-mat.md) | MAT(다국어 앱 도구 키트) 4.0은 UWP 앱에 번역 지원, 번역 파일 관리 및 편집기 도구를 제공하도록 Microsoft Visual Studio 2017과 통합되어 있습니다. |
| [다국어 앱 도구 키트 4.0 FAQ 및 문제 해결](mat-faq-troubleshooting.md) | 이 항목에는 자주 묻는 질문과 대답, 다국어 앱 도구 키트(MAT) 4.0과 관련된 문제가 나와 있습니다. |