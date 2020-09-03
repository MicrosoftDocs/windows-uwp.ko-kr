---
ms.assetid: F46306EC-DFF3-4FF0-91A8-826C1F8C4A52
title: 데이터 바인딩 및 MVVM
description: 데이터 바인딩은 MVVM(Model-View-ViewModel) UI 아키텍처 디자인 패턴의 핵심이며 UI 및 비 UI 코드 간의 느슨한 결합을 가능하게 합니다.
ms.date: 10/02/2018
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: ad0595fa070a1970e4890ce7e95627c06385ba6a
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89154217"
---
# <a name="data-binding-and-mvvm"></a>데이터 바인딩 및 MVVM

MVVM(Model-View-ViewModel)은 UI 및 비 UI 코드를 분리하기 위한 UI 아키텍처 디자인 패턴입니다. MVVM을 사용하여 UI를 XAML로 선언적으로 정의하고 데이터 바인딩 태그를 사용하여 데이터 및 명령을 포함하는 다른 계층에 연결합니다. 데이터 바인딩 인프라는 UI 및 연결된 데이터를 동기화된 상태로 유지하고, 사용자 입력을 적절한 명령으로 라우팅하는 느슨한 결합을 제공합니다. 

느슨한 결합을 제공하기 때문에, 데이터 바인딩을 사용하면 서로 다른 종류의 코드 간에 하드 종속성이 줄어듭니다. 이렇게 하면 다른 단원에서 의도치 않은 부작용을 발생시키지 않고도 개별 코드 단위(메서드, 클래스, 컨트롤 등)를 보다 쉽게 변경할 수 있습니다. 이러한 분리는 여러 디자인 패턴에서 중요한 개념인 *문제의 분리* 예입니다. 

## <a name="benefits-of-mvvm"></a>MVVM의 이점

코드를 분리하면 다음과 같은 많은 이점이 있습니다.

* 반복적인 예비 코딩 스타일 사용. 격리되는 변경 내용은 위험이 적고 실험하기가 더 쉽습니다.
* 단위 테스트 간소화. 서로 격리된 코드 단위는 프로덕션 환경 외부에서 개별적으로 테스트할 수 있습니다.
* 팀 공동 작업 지원. 잘 디자인된 인터페이스를 따르는 분리된 코드는 별도의 개인이나 팀에서 개발한 후 나중에 통합할 수 있습니다.
* 유지 관리 효율성 향상. 분리된 코드의 버그를 수정하면 다른 코드에서 재발할 가능성이 줄어듭니다.

MVVM과 달리, 기존의 "코드 숨김" 구조를 사용하는 앱은 일반적으로 표시 전용 데이터에 대해 데이터 바인딩을 사용하고, 컨트롤이 노출하는 이벤트를 직접 처리하여 사용자 입력에 응답합니다. 이벤트 처리기는 코드 숨김 파일(예: MainPage.xaml.cs)에서 구현되며 일반적으로 UI를 직접 조작하는 코드를 포함하는 컨트롤에 긴밀하게 결합됩니다. 이 경우 이벤트 처리 코드를 업데이트하지 않고 컨트롤을 대체하는 것이 어렵거나 불가능할 수 있습니다. 이 아키텍처를 사용하는 경우 코드 숨김 파일은 다른 페이지에서 사용하기 위해 복제 및 수정되는 데이터베이스 액세스 코드와 같이 UI와 직접 관련되지 않은 코드를 누적하기도 합니다.

## <a name="app-layers"></a>앱 계층

MVVM 패턴을 사용하는 경우 앱은 다음 계층으로 나뉩니다.

* **모델** 계층은 비즈니스 데이터를 나타내는 형식을 정의합니다. 여기에는 코어 앱 도메인을 모델링하는 데 필요한 모든 항목이 포함되며, 종종 코어 앱 논리가 포함됩니다. 이 계층은 뷰 및 뷰 모델 계층과는 완전히 별개이며, 클라우드에 부분적으로 상주하는 경우가 많습니다. 완전히 구현된 모델 계층이 있는 경우 동일한 기본 데이터를 사용하는 UWP 및 웹앱과 같이, 사용자의 선택에 따라 여러 다른 클라이언트 앱을 만들 수 있습니다.
* **뷰** 계층은 XAML 태그를 사용하여 UI를 정의합니다. 태그에는 특정 UI 구성 요소와 다양한 뷰 모델 및 모델 멤버 간의 연결을 정의하는 데이터 바인딩 식(예: [x:Bind](../xaml-platform/x-bind-markup-extension.md))이 포함되어 있습니다. 경우에 따라 코드 숨김 파일은 UI를 사용자 지정하거나 조작하는 데 필요한 추가 코드를 포함하거나, 작업을 수행하는 뷰 모델 메서드를 호출하기 전에 이벤트 처리기 인수에서 데이터를 추출하는 데 사용됩니다. 
* **뷰 모델** 계층은 뷰의 데이터 바인딩 대상을 제공합니다. 대부분의 경우 뷰 모델은 모델을 직접 노출하거나 특정 모델 멤버를 래핑하는 멤버를 제공합니다. 또한 뷰 모델은 항목 목록의 표시 순서와 같이 UI와는 관련이 있지만 모델과는 관련이 없는 데이터를 추적하는 멤버를 정의할 수 있습니다. 또한 뷰 모델은 데이터베이스 액세스 코드와 같이 다른 서비스와의 통합 지점 역할을 합니다. 간단한 프로젝트의 경우 별도의 모델 계층이 필요하지 않을 수 있으며 필요한 모든 데이터를 캡슐화하는 뷰 모델만 있으면 됩니다. 

## <a name="basic-and-advanced-mvvm"></a>기본 및 고급 MVVM

모든 디자인 패턴과 마찬가지로, MVVM를 구현하는 방법에는 여러 가지가 있으며 여러 다른 기술이 MVVM의 일부로 간주됩니다. 이러한 이유로 UWP를 비롯한 다양한 XAML 기반 플랫폼을 지원하는 여러 타사 MVVM 프레임워크가 있습니다. 그러나 이러한 프레임워크에는 일반적으로 분리된 아키텍처를 구현하기 위한 여러 서비스가 포함되어 있으므로 MVVM의 정확한 정의가 약간 모호해집니다. 

정교한 MVVM 프레임워크는 특히 엔터프라이즈급 프로젝트에 매우 유용할 수 있지만, 일반적으로 특정 패턴 또는 기술을 채택하는 것과 관련된 비용이 발생하며, 프로젝트의 규모와 크기에 따라 이점이 항상 명확한 것은 아닙니다. 다행히 명확하고 명백한 이점을 제공하는 기술만 채택하고, 필요할 때까지 다른 방법은 무시할 수 있습니다. 

특히, 데이터 바인딩의 전체 기능을 이해하고 적용하며, 앞에서 설명한 계층으로 앱 논리를 분리하여 많은 이점을 얻을 수 있습니다. 이러한 이점은 Windows SDK에서 제공하는 기능만 사용하고 외부 프레임워크를 사용하지 않고 구현할 수 있습니다. 특히 [{x:Bind} 태그 확장](../xaml-platform/x-bind-markup-extension.md)은 데이터 바인딩을 이전 XAML 플랫폼의 경우보다 더 쉽고 더 효율적으로 수행하도록 하므로 이전에 필요한 많은 상용구 코드가 필요하지 않습니다.

기본 MVVM을 사용하는 방법에 대한 추가 지침은 GitHub에서 [고객 주문 데이터베이스 샘플](https://github.com/Microsoft/Windows-appsample-customers-orders-database)을 참조하세요. 다른 [UWP 앱 샘플](https://github.com/Microsoft?q=windows-appsample
) 대부분은 기본 MVVM 아키텍처를 사용하며, [트래픽 앱 샘플](https://github.com/Microsoft/Windows-appsample-trafficapp)에는 코드 숨김 및 MVVM 버전과 [MVVM 변환](https://github.com/Microsoft/Windows-appsample-trafficapp/blob/MVVM/MVVM.md)을 설명하는 메모가 포함되어 있습니다. 

## <a name="see-also"></a>참고 항목

### <a name="topics"></a>항목

[데이터 바인딩 심층 분석](./data-binding-in-depth.md)  
[{x:Bind} 태그 확장](../xaml-platform/x-bind-markup-extension.md)  

### <a name="samples"></a>샘플

[고객 주문 데이터베이스 샘플](https://github.com/Microsoft/Windows-appsample-customers-orders-database)  
[VanArsdel 인벤토리 샘플](https://github.com/Microsoft/InventorySample)  
[트래픽 앱 샘플](https://github.com/Microsoft/Windows-appsample-trafficapp)