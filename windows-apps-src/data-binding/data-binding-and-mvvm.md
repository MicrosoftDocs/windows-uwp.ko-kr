---
author: KarlErickson
ms.assetid: F46306EC-DFF3-4FF0-91A8-826C1F8C4A52
title: 데이터 바인딩 및 MVVM
description: 데이터 바인딩 모델-보기-ViewModel (MVVM) UI 아키텍처 디자인 패턴의 핵심 이며 UI 및 비 UI 코드 간의 느슨한 결합을 통해 수 있습니다.
ms.author: karler
ms.date: 10/02/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: eda370db8b68232066052cca3d0abfa6e3876167
ms.sourcegitcommit: fbdc9372dea898a01c7686be54bea47125bab6c0
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/08/2018
ms.locfileid: "4431222"
---
# <a name="data-binding-and-mvvm"></a>데이터 바인딩 및 MVVM

모델-보기-ViewModel (MVVM)는 UI 및 비 UI 코드를 분리 하는 것에 대 한 UI 아키텍처 디자인 패턴입니다. Mvvm을 XAML에서 선언적으로 UI를 정의 하 고 데이터 바인딩 태그를 사용 하 여 데이터 및 명령을 포함 하는 다른 계층에 연결 합니다. 데이터 바인딩 인프라 느슨한 결합 하면 UI를 제공 하 고 연결 된 데이터를 동기화 하 고 적절 한 명령에 사용자 입력을 라우팅합니다. 

느슨한 결합을 제공 하기 때문에 데이터 바인딩 사용 하는 코드의 여러 종류 간의 하드 종속성 줄어듭니다. 따라서 쉽게 다른 단위로 의도 하지 않은 부작용 일으키지 않고 개별 코드 단위 (메서드, 클래스, 컨트롤, 등)를 변경할 수 있습니다. 이러한 분리은 *관심사의 분리*의 예 인 많은 디자인 패턴에 중요 한 개념입니다. 

## <a name="benefits-of-mvvm"></a>MVVM의 이점

코드를 분리 포함 하 여 여러 가지 이점이 있습니다.

* 반복, 예비 코딩 스타일을 사용 하도록 설정 합니다. 격리 된 변경은 위험한 덜 시험해 쉽습니다.
* 간소화 된 단위 테스트 되었습니다. 코드 단위 서로 격리를 개별적으로 및 프로덕션 환경 외부에서 테스트할 수 있습니다.
* 팀 공동 작업을 지원합니다. 잘 디자인 된 인터페이스를 따르는 분리 된 코드를 별도 개인 이나 팀에서 개발 하 고 나중에 통합 합니다.
* 관리 용이성을 향상 합니다. 분리 된 코드에서 버그를 해결 하는 다른 코드에서 릴리스하기 일으킬 가능성이입니다.

MVVM, 달리 더 많은 기본 "코드 숨김" 구조를 사용 하 여 앱은 일반적으로 데이터를 표시 전용 데이터에 대 한 바인딩을 사용 하 고 직접 컨트롤에 의해 노출 되는 이벤트를 처리 하 여 사용자 입력에 응답 합니다. 이벤트 처리기 코드 숨김 파일 (예: MainPage.xaml.cs)에서 구현 되 고 일반적으로 UI를 직접 조작 하는 코드를 포함 하는 컨트롤에 종종 긴밀 하 게 결합 합니다. 따라서 이벤트 처리 코드를 업데이트 하지 않고 컨트롤을 대체 하기 어렵거나 있습니다. 이 아키텍처를 사용 하 여 코드 숨김 파일은 중복 되 고 다른 페이지를 사용 하 여 사용 하기 위해 수정 갔 데이터베이스 액세스 코드 등의 UI에 직접 관련 없는 코드를 종종 누적 됩니다.

## <a name="app-layers"></a>응용 프로그램 계층

MVVM 패턴을 사용 하는 경우 앱은 다음 계층으로 나뉩니다.

* **모델** 계층 비즈니스 데이터를 표시 하는 형식을 정의 합니다. 이 핵심 앱 도메인 모델링 하는 데 필요한 모든 것을 포함 하며 종종 핵심 앱 논리를 포함 합니다. 이 계층 보기 및 보기 모델 계층으로 완전히 별개 이며 종종 클라우드에서 부분적으로 상주 합니다. 완전히 구현 된 모델 계층을 지정 만들 수 있습니다 여러 다른 클라이언트 앱 동일한 기본 데이터와 함께 작동 하는 UWP와 웹 앱 등의 하므로 선택 하는 경우.
* **보기** 계층은 XAML 태그를 사용 하 여 UI를 정의 합니다. 태그는 특정 UI 구성 요소 및 보기 모델 및 모델에 대 한 다양 한 멤버 간의 연결을 정의 하는 (예: [X:bind](https://docs.microsoft.com/windows/uwp/xaml-platform/x-bind-markup-extension)) 데이터 바인딩 식에 포함 됩니다. 코드 숨김 파일 경우에 따라 사용자 지정 하거나 UI를 조작 하거나 작업을 수행 하는 뷰 모델 메서드를 호출 하기 전에 이벤트 처리기 인수에서 데이터를 추출 하는 데 필요한 추가 코드를 포함 하도록 보기 계층의 일부로 사용 됩니다. 
* **보기 모델** 계층 데이터 바인딩 대상 보기를 제공합니다. 대부분의 경우에서 보기 모델, 직접 모델을 노출 하거나 특정 모델 멤버를 래핑하는 멤버를 제공 합니다. 보기 모델 추적 하기 위해 관련 데이터 멤버 정의할 수도 항목 목록 표시 순서와 같은 모델 하지 않고 UI 하도록 합니다. 보기 모델 데이터베이스 액세스 코드와 같은 다른 서비스와의 통합으로도 사용 됩니다. 하지 간단한 프로젝트에 대 한 별도 모델 계층을 하지만 뷰-모델 필요한 모든 데이터를 캡슐화 할 수도 있습니다. 

## <a name="basic-and-advanced-mvvm"></a>기본 및 고급 MVVM

모든 디자인 패턴에서와 마찬가지로 MVVM을 구현 하는 여러 가지 방법 이며 다양 한 기술을 MVVM의 일부로 간주 됩니다. 이러한 이유로 UWP를 비롯 한 다양 한 XAML 기반 플랫폼을 지 원하는 몇 가지 다른 타사 MVVM 프레임 있습니다. 그러나 이러한 프레임 워크는 일반적으로 MVVM의 정확한 정의 다소 모호 하 분리 된 아키텍처를 구현 하는 것에 대 한 여러 서비스를 포함 합니다. 

MVVM 프레임 워크를 정교한 매우 유용할 수 있지만 특히 엔터프라이즈급 프로젝트는 일반적으로 모든 특정 패턴 또는 기술을 채택와 관련 된 비용 및 혜택에 항상 배율 및 크기에 따라 지우기 프로젝트입니다. 다행히 명확 하 고 실질적인 이점을 제공 하는 기술을 채택할 수 있으며 필요할 때까지 다른 무시할 수 있습니다. 

특히 많은 이해 하 고 데이터 바인딩, 앞에서 설명한 계층에 구분 하는 앱 논리의 모든 기능을 적용 하 여 혜택을 얻을 수 있습니다. 외부 모든 프레임 워크를 사용 하지 않고 Windows SDK에서 제공 되는 기능만 사용 하 여 얻을 수 있습니다. 특히, [{x: Bind} 태그 확장](https://docs.microsoft.com/windows/uwp/xaml-platform/x-bind-markup-extension) 쉽게 데이터 바인딩 및 수많은 상용구 코드에 대 한 필요성을 제거 하는 이전 XAML 플랫폼에서 수행 보다 높은 필요한 이전.

추가에 대 한 기본, 제공 하지 않는 MVVM을 사용 하 여 GitHub에서 [고객 주문 데이터베이스 샘플](https://github.com/Microsoft/Windows-appsample-customers-orders-database) 확인 합니다. 많은 다른 [UWP 앱 샘플](https://github.com/Microsoft?q=windows-appsample
) 도 사용 하 여 기본 MVVM 아키텍처 및 [MVVM 변환](https://github.com/Microsoft/Windows-appsample-trafficapp/blob/MVVM/MVVM.md)에 대 한 설명을 사용 하 여 코드 숨김 및 MVVM 버전 모두를 포함 하는 [트래픽 앱 샘플](https://github.com/Microsoft/Windows-appsample-trafficapp) 입니다. 

## <a name="see-also"></a>참고 항목

### <a name="topics"></a>항목

[데이터 바인딩 심층 분석](https://docs.microsoft.com/windows/uwp/data-binding/data-binding-in-depth)  
[{x:Bind} 태그 확장](https://docs.microsoft.com/windows/uwp/xaml-platform/x-bind-markup-extension)  

### <a name="samples"></a>샘플

[고객 주문 데이터베이스 샘플](https://github.com/Microsoft/Windows-appsample-customers-orders-database)  
[VanArsdel 인벤토리 샘플](https://github.com/Microsoft/InventorySample)  
[교통 앱 샘플](https://github.com/Microsoft/Windows-appsample-trafficapp)  
