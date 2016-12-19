---
author: mcleblanc
ms.assetid: 64F7FC51-E8AC-4098-9C5F-0172E4724B5C
title: "성능"
description: "사용자는 앱이 응답성을 유지하고 자연스러운 느낌을 주며 배터리를 소모하지 않기를 기대합니다."
translationtype: Human Translation
ms.sourcegitcommit: 73b19e54b863693aece045e5b653bc0583a676bb
ms.openlocfilehash: b9395e80bca7a46076e20e42fa7ceee5df7cc0d5

---
# <a name="performance"></a>성능

\[ Windows 10의 UWP 앱에 맞게 업데이트되었습니다. Windows 8.x 문서는 [보관](http://go.microsoft.com/fwlink/p/?linkid=619132)을 참조하세요. \]

사용자는 앱이 응답성을 유지하고 자연스러운 느낌을 주며 배터리를 소모하지 않기를 기대합니다. 기술적으로 성능은 기능적 요구 사항이 아니지만 성능을 기능으로 간주하는 것이 사용자의 기대를 충족하는 데 도움이 됩니다. 목표 지정 및 측정이 중요한 요소입니다. 성능에 중요한 시나리오가 무엇인지 결정하고 우수한 성능이란 무엇인지 정의하세요. 그런 다음 초기에 측정하고 프로젝트의 수명 주기 동안 목표를 달성할 수 있는지 확인합니다. 이 섹션에서는 성능 워크플로를 구성하고, 애니메이션 결함 및 프레임 속도 문제를 해결하고, 시작 시간과 페이지 탐색 시간 및 메모리 사용량을 조정하는 방법을 설명합니다.

지금까지 살펴본 중요한 성능 향상 단계는 Windows 10을 대상으로 앱을 이식하는 것입니다. 여러 XAML 최적화(예: [{x:Bind}](https://msdn.microsoft.com/library/windows/apps/Mt204783))는 Windows 10 앱에서만 사용할 수 있습니다. [Windows 10으로 앱 이식](https://msdn.microsoft.com/library/windows/apps/Mt238321) 및 //build/ 세션 [유니버설 Windows 플랫폼으로 전환](http://channel9.msdn.com/Events/Build/2015/3-741)을 참조하세요.

| 항목 | 설명 |
|-------|-------------|
| [성능 계획](planning-and-measuring-performance.md) | 사용자는 앱이 응답성을 유지하고 자연스러운 느낌을 주며 배터리를 소모하지 않기를 기대합니다. 기술적으로 성능은 기능적 요구 사항이 아니지만 성능을 기능으로 간주하는 것이 사용자의 기대를 충족하는 데 도움이 됩니다. 목표 지정 및 측정이 중요한 요소입니다. 성능에 중요한 시나리오가 무엇인지 결정하고 우수한 성능이란 무엇인지 정의하세요. 그런 다음 초기뿐 아니라 프로젝트 수명 주기 동안 목표 달성을 확신할 수 있을 만큼 자주 측정합니다. |
| [백그라운드 작업 최적화](optimize-background-activity.md) | 시스템과 연동하여 배터리에 효율적인 방식으로 백그라운드 작업을 사용하는 UWP 앱을 만듭니다. |
| [ListView 및 GridView UI 최적화](optimize-gridview-and-listview.md) | UI 가상화, 요소 감소, 항목에 대한 점진적 업데이트를 통해 [<strong>GridView</strong>](https://msdn.microsoft.com/library/windows/apps/BR242705)의 성능과 시작 시간을 개선합니다. |
| [ListView 및 GridView 데이터 가상화](listview-and-gridview-data-optimization.md) | 데이터 가상화를 통해 [<strong>GridView</strong>](https://msdn.microsoft.com/library/windows/apps/BR242705) 성능과 시작 시간이 향상됩니다. |
| [가비지 수집 성능 향상](improve-garbage-collection-performance.md) | C# 및 Visual Basic으로 작성한 UWP(유니버설 Windows 플랫폼) 앱은 .NET 가비지 수집기의 자동 메모리 관리를 사용합니다. 이 섹션에는 UWP 앱에서 .NET 가비지 수집기에 대한 동작 및 성능 모범 사례가 요약되어 있습니다. |
| [UI 스레드 응답 유지](keep-the-ui-thread-responsive.md) | 사용자는 컴퓨터 유형에 관계없이 계산하는 동안 앱이 계속 응답할 것으로 기대합니다. 이는 앱에 따라 다른 항목을 의미합니다. 일부는 이는 더 실제적인 물리학을 제공하거나, 디스크나 웹에서 더 빠르게 데이터를 로드하거나, 빠르게 복잡한 화면을 제공하고 페이지를 탐색하거나, 즉시 방향을 찾거나, 데이터를 빠르게 처리하도록 변환됩니다. 계산 유형에 관계없이 사용자는 앱이 입력한 대로 작동하고 &quot;연산&quot; 중에 응답하지 않는 것으로 보이지 않기를 원합니다. |
| [XAML 태그 최적화](optimize-xaml-loading.md) | 메모리에서 개체를 생성하기 위해 XAML 태그를 구문 분석하는 작업은 복잡한 UI의 경우 시간이 많이 걸립니다. 다음은 XAML 태그 구문 분석 및 로드 시간과 앱의 메모리 효율성을 개선하기 위해 수행할 수 있는 몇 가지 작업입니다. | 
| [XAML 레이아웃 최적화](optimize-your-xaml-layout.md) | 레이아웃은 CPU 사용과 메모리 오버헤드 모두에서 비용이 많이 드는 XAML 앱 요소입니다. 다음은 XAML 앱의 레이아웃 성능 향상을 위해 수행할 수 있는 몇 가지 간단한 절차입니다. | 
| [MVVM 및 언어 성능 팁](mvvm-performance-tips.md) | 이 항목에서는 선택한 소프트웨어 디자인 패턴 및 프로그래밍 언어와 관련된 몇 가지 성능 고려 사항을 설명합니다. |
| [앱 시작 성능 모범 사례](best-practices-for-your-app-s-startup-performance.md) | 시작 및 활성화 처리 방법을 개선하여 시작 시간이 최적화된 UWP 앱을 만듭니다. |
| [애니메이션, 미디어 및 이미지 최적화](optimize-animations-and-media.md) | 매끄러운 애니메이션, 높은 프레임 속도 및 고성능 미디어 캡처 및 재생을 지원하는 UWP(유니버설 Windows 플랫폼) 앱을 만듭니다. |
| [일시 중단/다시 시작 최적화](optimize-suspend-resume.md) | 프로세스 수명 시스템의 사용을 간소화하여 일시 중단 또는 종료 후 효율적으로 다시 시작하는 UWP 앱을 만듭니다. |
| [파일 액세스 최적화](optimize-file-access.md) | 파일 시스템에 효율적으로 액세스하여 디스크 대기 시간 및 메모리/CPU 주기로 인한 성능 문제를 방지하는 UWP 앱을 만듭니다. |
| [Windows 런타임 구성 요소 및 interop 최적화](windows-runtime-components-and-optimizing-interop.md) | Interop 성능 문제를 방지하면서 네이티브 형식과 관리되는 형식 간의 Interop 및 UWP 구성 요소를 사용하는 UWP 앱을 만듭니다. |
| [프로파일링 및 성능 도구](tools-for-profiling-and-performance.md) | Microsoft는 UWP 앱의 성능을 개선하는 데 도움이 되는 여러 도구를 제공합니다.|




<!--HONumber=Dec16_HO1-->


