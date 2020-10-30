---
description: 리소스가 요청되면 현재 리소스 컨텍스트와 어느 정도 일치하는 몇 가지 후보가 있을 수 있습니다. 리소스 관리 시스템은 모든 후보를 분석하여 반환할 가장 적합한 후보를 결정합니다. 이 문서에서는 이 프로세스를 자세히 설명하고 예제를 제공합니다.
title: 리소스 관리 시스템에서 리소스를 일치시키고 선택하는 방법
template: detail.hbs
ms.date: 10/23/2017
ms.topic: article
keywords: Windows 10, UWP, 리소스, 이미지, 자산, MRT, 한정자
ms.localizationpriority: medium
ms.openlocfilehash: d430aae696b0f021e2412a73f137ea6db826937b
ms.sourcegitcommit: a3bbd3dd13be5d2f8a2793717adf4276840ee17d
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/30/2020
ms.locfileid: "93031856"
---
# <a name="how-the-resource-management-system-matches-and-chooses-resources"></a>리소스 관리 시스템에서 리소스를 일치시키고 선택하는 방법
리소스가 요청되면 현재 리소스 컨텍스트와 어느 정도 일치하는 몇 가지 후보가 있을 수 있습니다. 리소스 관리 시스템은 모든 후보를 분석하여 반환할 가장 적합한 후보를 결정합니다. 모든 한정자를 모든 후보에 순위를 고려 하 여이 작업을 수행 합니다.

이 순위 프로세스에서 다른 한정자에는 다른 우선 순위가 지정 됩니다. language는 전체 순위에 가장 큰 영향을 줄 수 있으며, 그 다음에는 그 다음으로 크기를 조정할 수 있습니다. 각 한정자에 대해 후보 한정자를 컨텍스트 한정자 값과 비교 하 여 일치 품질을 결정 합니다. 비교를 수행 하는 방법은 한정자에 따라 다릅니다.

언어 태그 일치를 수행 하는 방법에 대 한 자세한 내용은 [리소스 관리 시스템이 언어 태그와 일치 하는 방법](how-rms-matches-lang-tags.md)을 참조 하세요.

배율 및 대비와 같은 일부 한정자의 경우 항상 최소한의 일치 수준이 있습니다. 예를 들어 "scale-100"에 대해 정규화 된 후보는 "scale-400"의 컨텍스트와 "scale-200" 또는 (완벽 하 게 일치 하는 항목) "scale-400"에 한정 된 후보 뿐만 아니라 일부 작은 수준으로 일치 합니다.

그러나 언어 또는 홈 지역과 같은 다른 한정자의 경우 일치 하지 않는 비교와 일치 하는 각도를 가질 수 있습니다. 예를 들어 "en-us"로 언어에 한정 된 후보는 "en-us"의 컨텍스트에 대 한 부분 일치 이지만 "fr"으로 정규화 된 후보는 일치 항목이 아닙니다. 마찬가지로, 홈 지역에 대해 "155" (서유럽)로 한정 된 후보는 홈 지역 설정이 "FR" 인 사용자의 컨텍스트와 일치 하지만 "US"로 한정 된 후보는 전혀 일치 하지 않습니다.

후보가 평가 될 때 한정자에 대해 일치 하지 않는 비교가 있으면 해당 후보는 전체 일치 하지 않는 순위를 얻게 되며 선택 되지 않습니다. 이러한 방식으로 우선 순위가 높은 한정자는 가장 일치 하는 항목을 선택 하는 데 가장 큰 가중치를 가질 수 있지만 우선 순위가 낮은 한정자 라도 일치 하지 않는 후보를 제거할 수 있습니다.

한정자는 한정자를 기준으로 표시 되지 않은 경우 한정자와 관련 하 여 중립적인 것입니다. 한정자의 경우 중립 후보는 항상 컨텍스트 한정자 값과 일치 하지만, 해당 한정자에 대해 표시 된 후보 보다 낮은 일치 품질로만 일치 하 고 정확한 일치 항목 (정확한 값 또는 부분)이 있습니다. 예를 들어, "en-us", "en", "fr" 및 언어 중립적인 후보에 대해 한정 된 후보가 있는 경우 언어 한정자 값이 "en-us" 인 컨텍스트의 경우 후보는 "en", "en-us", 중립 및 "fr" 순서로 순위가 부여 됩니다. 이 경우 "fr"은 전혀 일치 하지 않고 다른 후보는 어느 정도 일치 합니다.

전반적인 순위 지정 프로세스는 가장 높은 우선 순위 한정자 (language)와 관련 된 후보를 평가 하 여 시작 합니다. 일치 하지 않는 항목이 제거 됩니다. 나머지 후보는 언어와 일치 하는 품질을 기준으로 순위가 매겨집니다. 동률이 있는 경우 일치 하는 후보를 구별 하기 위해 대비에 일치 품질을 사용 하 여 우선 순위가 가장 높은 다음 한정자를 고려 합니다. 이와 대조적으로 배율 한정자는 남은 동률을 구분 하는 데 사용 되며,이는 잘 정렬 된 순위에 도달 하는 데 필요한 만큼의 한정자를 통해 수행 됩니다.

컨텍스트와 일치 하지 않는 한정자로 인해 모든 후보가 고려 대상에서 제거 되는 경우 리소스 로더는 표시할 기본 후보를 찾는 두 번째 패스를 거칩니다. 기본 후보는 PRI 파일을 만드는 동안 결정 되며 항상 모든 런타임 컨텍스트에 대해 선택할 후보가 있는지 확인 하는 데 필요 합니다. 후보에 일치 하지 않고 기본값이 아닌 한정자가 있으면 해당 리소스 후보가 고려 하지 않고 영구적으로 throw 됩니다.

여전히 고려 중인 모든 리소스 후보에 대해 리소스 로더는 우선 순위가 가장 높은 컨텍스트 한정자 값을 확인 하 고 가장 일치 하거나 가장 적합 한 기본 점수를 가진 값을 선택 합니다. 실제 일치는 기본 점수 보다 더 나은 것으로 간주 됩니다.

연결 된 경우 가장 높은 우선 순위 컨텍스트 한정자 값이 검사 되 고 가장 일치 하는 항목이 발견 될 때까지 프로세스가 계속 됩니다.

## <a name="example-of-choosing-a-resource-candidate"></a>리소스 후보를 선택 하는 예
이러한 파일을 고려 합니다.

```console
en/images/logo.scale-400.jpg
en/images/logo.scale-200.jpg
en/images/logo.scale-100.jpg  
fr/images/logo.scale-100.jpg
fr/images/contrast-high/logo.scale-400.jpg
fr/images/contrast-high/logo.scale-100.jpg
de/images/logo.jpg
```

그리고 이것이 현재 컨텍스트의 설정 이라고 가정 합니다.

```console
Application language: en-US; fr-FR;
Scale: 400
Contrast: Standard
```

고대비 및 독일어 언어가 설정에 정의 된 컨텍스트와 일치 하지 않기 때문에 리소스 관리 시스템은 3 개의 파일을 제거 합니다. 이러한 후보는 그대로 유지 됩니다.

```console
en/images/logo.scale-400.jpg
en/images/logo.scale-200.jpg
en/images/logo.scale-100.jpg  
fr/images/logo.scale-100.jpg
```

이러한 나머지 후보에 대해 리소스 관리 시스템은 가장 높은 우선 순위 컨텍스트 한정자 인 language를 사용 합니다. 영어는 설정에서 프랑스어 이전에 표시 되기 때문에 영어 리소스는 프랑스어와 더 가깝게 일치 합니다.

```console
en/images/logo.scale-400.jpg
en/images/logo.scale-200.jpg
en/images/logo.scale-100.jpg  
```

다음으로 리소스 관리 시스템은 우선 순위가 가장 높은 컨텍스트 한정자 인 scale을 사용 합니다. 이는 반환 되는 리소스입니다.

```console
en/images/logo.scale-400.jpg
```

고급 [**Namedresource. resolveall**](/uwp/api/windows.applicationmodel.resources.core.namedresource.resolveall?branch=live) 메서드를 사용 하 여 컨텍스트 설정과 일치 하는 순서로 모든 후보를 검색할 수 있습니다. 앞서 살펴본 예의 경우 **Resolveall** 은이 순서로 후보를 반환 합니다.

```console
en/images/logo.scale-400.jpg
en/images/logo.scale-200.jpg
en/images/logo.scale-100.jpg  
fr/images/logo.scale-100.jpg
```

## <a name="example-of-producing-a-fallback-choice"></a>대체 선택 항목을 생성 하는 예
이러한 파일을 고려 합니다.

```console
en/images/logo.scale-400.jpg
en/images/logo.scale-200.jpg
en/images/logo.scale-100.jpg  
fr/images/contrast-standard/logo.scale-400.jpg
fr/images/contrast-standard/logo.scale-100.jpg
de/images/contrast-standard/logo.jpg
```

그리고 이것이 현재 컨텍스트의 설정 이라고 가정 합니다.

```console
User language: de-DE;
Scale: 400
Contrast: High
```

컨텍스트와 일치 하지 않으므로 모든 파일이 제거 됩니다. 따라서 기본 패스를 입력 합니다. 여기서 기본값은 PRI 파일을 생성 하는 동안 기본값 ( [MakePri.exe를 사용 하 여 리소스를 수동으로 컴파일 ](compile-resources-manually-with-makepri.md)참조)입니다.

```console
Language: fr-FR;
Scale: 400
Contrast: Standard
```

그러면 현재 사용자 또는 기본값과 일치 하는 모든 리소스가 남게 됩니다.

```console
fr/images/contrast-standard/logo.scale-400.jpg
fr/images/contrast-standard/logo.scale-100.jpg
de/images/contrast-standard/logo.jpg
```

리소스 관리 시스템은 우선 순위가 가장 높은 컨텍스트 한정자 인 language를 사용 하 여 점수가 가장 높은 명명 된 리소스를 반환 합니다.

```console
de/images/contrast-standard/logo.jpg
```

## <a name="important-apis"></a>중요 API
* [NamedResource. ResolveAll](/uwp/api/windows.applicationmodel.resources.core.namedresource.resolveall?branch=live)

## <a name="related-topics"></a>관련 항목
* [MakePri.exe를 사용하여 수동으로 리소스 컴파일](compile-resources-manually-with-makepri.md)
