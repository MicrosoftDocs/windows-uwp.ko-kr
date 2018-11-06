---
author: stevewhims
Description: When a resource is requested, there may be several candidates that match the current resource context to some degree. The Resource Management System will analyze all of the candidates and determine the best candidate to return. This topic describes that process in detail and gives examples.
title: 리소스 관리 시스템이 리소스를 일치시키고 선택하는 방법
template: detail.hbs
ms.author: stwhi
ms.date: 10/23/2017
ms.topic: article
keywords: Windows 10, uwp, 리소스, 이미지, 자산, MRT, 한정자
ms.localizationpriority: medium
ms.openlocfilehash: c7576f98045bce3bcfcee093aa8d61059354d45a
ms.sourcegitcommit: e814a13978f33654d8e995584f4b047cb53e0aef
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/05/2018
ms.locfileid: "6045280"
---
# <a name="how-the-resource-management-system-matches-and-chooses-resources"></a>리소스 관리 시스템이 리소스를 일치시키고 선택하는 방법
리소스를 요청하는 경우 현재 리소스 컨텍스트와 어느 정도 일치하는 몇 가지 후보가 있을 수 있습니다. 리소스 관리 시스템은 모든 후보를 분석하고 반환할 최적의 후보를 결정합니다. 이 작업은 모든 후보의 순위를 지정할 모든 한정자를 고려하여 수행됩니다.

이 순위 지정 프로세스에서 다른 한정자에 다른 우선 순위가 부여됩니다. 언어가 전반적인 순위에 가장 큰 영향을 미치며 그 다음으로 대비, 배율 등의 순서입니다. 각 한정자에 대해 후보 한정자는 일치 정도를 확인하기 위해 컨텍스트 한정자 값과 비교됩니다. 비교 방식은 한정자에 따라 다릅니다.

언어 태그 일치 방법에 대한 자세한 내용은 [리소스 관리 시스템이 언어 태그를 일치하는 방법](how-rms-matches-lang-tags.md)을 참조하세요.

배율 및 고대비 등의 일부 한정자의 경우 항상 최소한의 어느 정도의 일치가 있습니다. 예를 들어, "scale-100"와 일치 "scale-400" 컨텍스트와 작은 어느 정도 대 한 정규화 된 후보와 하지 하지만"scale-200"또는 (완벽 한 일치)에 대해"scale-400"에 대 한 정규화 된 후보입니다.

하지만 언어 또는 홈 지역과 같은 다른 한정자에 대해 일치 정도뿐만 아니라 비일치 비교가 있을 수 있습니다. 예를 들어, "en-US"로 언어에 대해 정규화된 후보는 "en-GB" 컨텍스트에 대한 부분 일치이지만 "fr"로 정규화된 후보는 전혀 일치하지 않습니다. 마찬가지로 홈 지역에 대해 "155"(서유럽)로 정규화된 후보는 "FR"로 홈 지역이 설정된 사용자에 대한 컨텍스트와 일정 부분 일치하지만 "US"로 정규화된 후보는 전혀 일치하지 않습니다.

후보를 평가할 때 어떤 한정자에도 비일치 비교가 있는 경우 해당 후보는 전반적으로 비일치 순위를 받게 되며 선택되지 않습니다. 이런 방법으로 높은 우선 순위의 한정자는 최적의 일치를 선택할 때 중요도가 가장 크지만 이러한 비일치 때문에 우선 순위가 낮은 한정자도 후보를 제거할 수 있습니다.

후보는 한정자에 대해 표시되지 않은 경우 해당 한정자에 대해 중립입니다. 모든 한정자에 대해 중립 후보는 항상 컨텍스트 한정자 값에 대한 일치이지만 해당 한정자에 대해 표시된 다른 모든 후보에 비해 낮은 우선 순위의 일치이며 어느 정도의 일치를 가집니다(정확한 또는 부분 일치). 예를 들어 후보 "en-US", "en", "fr"에 대해 정규화된 후보와 언어 중립 후보가 있는 경우 언어 한정자 값이 "en-GB"인 컨텍스트에 대해 후보는 "en", "en-US", 중립, "fr" 순서대로 순위가 매겨집니다. 이 경우 "fr"은 전혀 일치하지 않으며 다른 후보들은 어느 정도 일치합니다.

전체 순위 선정 프로세스는 가장 우선 순위가 높은 한정자인 언어와 관련된 후보를 평가하여 시작합니다. 비일치는 제거됩니다. 나머지 후보는 언어에 대한 일치 수준을 기준으로 순위가 매겨집니다. 동등한 수준의 후보가 있는 경우 다음으로 우선 순위가 높은 한정자인 대비가 고려되며, 대비에 대한 일치 정도를 사용하여 동등한 후보를 구별합니다. 대비 이후에 배율 한정자가 나머지 동등한 후보를 구분하는 데 사용되며 잘 정렬된 순위에 도착할 때까지 필요한 만큼의 한정자로 같은 프로세스를 수행합니다.

한정자가 컨텍스트와 일치하지 않아 모든 후보가 고려 대상에서 제거된 경우 리소스 로더는 표시할 수 있는 기본 후보를 찾는 두 번째 단계를 거칩니다. 기본 후보는 PRI 파일을 생성하는 동안 결정되며 모든 런타임 컨텍스트에 대해 선택할 후보를 유지하기 위해 필요합니다. 후보에 일치 항목이나 기본값이 없는 경우 해당 리소스 후보는 영구적으로 고려 대상에서 제외됩니다.

여전히 고려 대상인 모든 리소스 후보에 대해 리소스 로더는 가장 우선 순위가 높은 컨텍스트 한정자 값을 찾으며 최적의 일치 또는 최고의 기본값 점수를 가진 후보를 선택합니다. 모든 실제 일치는 기본 점수보다 더 나은 것으로 간주됩니다.

동률이 있는 경우 다음 높은 우선 순위의 컨텍스트 한정자 값이 검사되며 최적의 일치를 찾을 때까지 프로세스가 계속됩니다.

## <a name="example-of-choosing-a-resource-candidate"></a>리소스 후보 선택 예제
이러한 파일을 고려해 보세요.

```console
en/images/logo.scale-400.jpg
en/images/logo.scale-200.jpg
en/images/logo.scale-100.jpg  
fr/images/logo.scale-100.jpg
fr/images/contrast-high/logo.scale-400.jpg
fr/images/contrast-high/logo.scale-100.jpg
de/images/logo.jpg
```

그리고 이 파일이 현재 컨텍스트의 설정이라고 생각해 보세요.

```console
Application language: en-US; fr-FR;
Scale: 400
Contrast: Standard
```

고대비 및 독일어가 설정에 의해 정의된 컨텍스트와 일치하지 않으므로 리소스 관리 시스템은 세 가지 파일을 제거합니다. 그 결과로 다음 후보가 남았습니다.

```console
en/images/logo.scale-400.jpg
en/images/logo.scale-200.jpg
en/images/logo.scale-100.jpg  
fr/images/logo.scale-100.jpg
```

이들 남은 후보에 대해 리소스 관리 시스템은 가장 우선 순위가 높은 컨텍스트 한정자인 언어를 사용합니다. 영어 리소스는 프랑스 리소스보다 더 나은 일치이므로 설정에서 영어가 프랑스어 이전에 나열됩니다.

```console
en/images/logo.scale-400.jpg
en/images/logo.scale-200.jpg
en/images/logo.scale-100.jpg  
```

그 다음 리소스 관리 시스템은 다음으로 우선 순위가 높은 컨텍스트 한정자인 배율을 사용합니다. 다음과 같은 리소스가 반환됩니다.

```console
en/images/logo.scale-400.jpg
```

고급 [**NamedResource.ResolveAll**](/uwp/api/windows.applicationmodel.resources.core.namedresource.resolveall?branch=live) 메서드를 사용하여 모든 후보를 컨텍스트 설정과 일치하는 순서대로 검색할 수 있습니다. 방금 언급한 예제에서 **ResolveAll**은 이 순서대로 후보를 반환합니다.

```console
en/images/logo.scale-400.jpg
en/images/logo.scale-200.jpg
en/images/logo.scale-100.jpg  
fr/images/logo.scale-100.jpg
```

## <a name="example-of-producing-a-fallback-choice"></a>대체 선택을 생성하는 예
이러한 파일을 고려해 보세요.

```console
en/images/logo.scale-400.jpg
en/images/logo.scale-200.jpg
en/images/logo.scale-100.jpg  
fr/images/contrast-standard/logo.scale-400.jpg
fr/images/contrast-standard/logo.scale-100.jpg
de/images/contrast-standard/logo.jpg
```

그리고 이 파일이 현재 컨텍스트의 설정이라고 생각해 보세요.

```console
User language: de-DE;
Scale: 400
Contrast: High
```

컨텍스트와 일치하는 파일이 없으므로 모든 파일이 제거됩니다. 따라서 PRI를 만드는 동안 기본값([MakePri.exe를 사용하여 수동으로 리소스 컴파일](compile-resources-manually-with-makepri.md) 참조)이 이와 같은 기본 전달을 입력합니다.

```console
Language: fr-FR;
Scale: 400
Contrast: Standard
```

이렇게 하면 현재 사용자 또는 기본값과 일치하는 모든 리소스가 남게 됩니다.

```console
fr/images/contrast-standard/logo.scale-400.jpg
fr/images/contrast-standard/logo.scale-100.jpg
de/images/contrast-standard/logo.jpg
```

리소스 관리 시스템은 가장 우선 순위가 높은 컨텍스트 한정자인 언어를 사용하여 가장 점수가 높은 명명된 리소스를 반환합니다.

```console
de/images/contrast-standard/logo.jpg
```

## <a name="important-apis"></a>중요 API
* [NamedResource.ResolveAll](/uwp/api/windows.applicationmodel.resources.core.namedresource.resolveall?branch=live)

## <a name="related-topics"></a>관련 항목
* [MakePri.exe를 사용하여 수동으로 리소스 컴파일](compile-resources-manually-with-makepri.md)