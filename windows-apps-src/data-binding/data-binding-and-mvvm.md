---
ms.assetid: F46306EC-DFF3-4FF0-91A8-826C1F8C4A52
title: 데이터 바인딩 및 MVVM
description: 데이터 바인딩 모델-뷰-ViewModel (MVVM) UI 아키텍처 디자인 패턴의 핵심 이며 UI 및 UI가 아닌 코드 간의 느슨한 결합을 사용 하도록 설정 합니다.
ms.date: 10/02/2018
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 931f2fcbcdbf58b9dc2ca40403d7466b620a8991
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57616738"
---
# <a name="data-binding-and-mvvm"></a>데이터 바인딩 및 MVVM

모델-뷰-ViewModel (MVVM)은 UI 및 비 UI 코드를 분리 하는 것에 대 한 UI 아키텍처 디자인 패턴입니다. MVVM을 사용 하 여 XAML에서 선언적으로 UI를 정의 하 고 데이터 바인딩 태그를 사용 하 여 데이터와 명령을 포함 하는 다른 계층에 연결 합니다. UI를 보관 하는 느슨한 결합을 제공 하는 데이터 바인딩 인프라 및 연결 된 데이터를 동기화 하 고 적절 한 명령에 사용자 입력을 라우팅합니다. 

느슨한 결합을 제공 하기 때문에 데이터 바인딩 사용 하 여 다른 종류의 코드 간의 굳은 종속성을 줄입니다. 따라서 쉽게 다른 단위로 의도 하지 않은 부작용을 일으키지 않고 개별 코드 단위 (메서드, 클래스, 컨트롤 등)을 변경할 수 있습니다. 예는이 분리는 *중요 한 부분의 분리*는 다양 한 디자인 패턴에서 중요 한 개념입니다. 

## <a name="benefits-of-mvvm"></a>MVVM의 이점

코드 분리 포함 하 여 여러 가지 이점이 있습니다.

* 반복, 예비 코딩 스타일을 사용 하도록 설정 합니다. 격리 된 변경 되어 덜 위험한 쉽게 실험할 수 있습니다.
* 간소화 된 단위 테스트 되었습니다. 개별적으로 및 테스팅 환경에서 서로 격리 된 코드 단위를 테스트할 수 있습니다.
* 팀 공동 작업을 지원합니다. 잘 설계 된 인터페이스를 따르는 코드를 분리 된 별도 개인 이나 팀에서 개발 하 고 나중에 통합 합니다.
* 관리 용이성을 향상 합니다. 분리 된 코드에서 버그를 해결 하는 것은 다른 코드에 회귀가 발생 가능성이 줄어들어입니다.

MVVM과 달리 더 기본적인 "코드 숨김" 구조체를 사용 하 여 앱은 일반적으로 표시 전용 데이터에 대 한 바인딩 데이터를 사용 하 고 직접 컨트롤에 의해 노출 되는 이벤트를 처리 하 여 사용자 입력에 응답 합니다. 이벤트 처리기 코드 숨김 파일 (예: MainPage.xaml.cs)에서 구현 되 고 일반적으로 UI를 직접 조작 하는 코드가 포함 된 컨트롤을 종종 긴밀 하 게 결합 됩니다. 따라서, 이벤트 처리 코드를 업데이트 하지 않고도 컨트롤을 대체할 어렵거나 합니다. 이 아키텍처를 사용 하 여 코드 숨김 파일 최종적으로 복제 되 고 다른 페이지를 사용 하 여 사용할 수 있도록 수정 하는 데이터베이스 액세스 코드와 같은 UI와 직접 관련 되지 않은 코드를 자주 축적 됩니다.

## <a name="app-layers"></a>앱 계층

MVVM 패턴을 사용 하는 경우 앱은 다음 계층으로 나뉩니다.

* 합니다 **모델** 계층 비즈니스 데이터를 나타내는 형식을 정의 합니다. 이 핵심 응용 프로그램 도메인을 모델링 하는 데 필요한 모든 포함 하 고 종종 핵심 앱 논리를 포함 합니다. 이 계층 보기 및 보기 모델 계층에 완전히 독립적 이며 종종 클라우드에 부분적으로 상주 합니다. 완전히 구현 된 모델 계층을 들어 만들 수 있습니다 여러 다른 클라이언트 앱 이므로 원하는 경우 동일한 기본 데이터를 사용 하는 UWP 및 웹 앱 같은.
* 합니다 **보기** 계층 XAML 태그를 사용 하 여 UI를 정의 합니다. 데이터 바인딩 식을 포함 하는 태그 (같은 [X:bind](https://docs.microsoft.com/windows/uwp/xaml-platform/x-bind-markup-extension)) 특정 UI 구성 요소 및 다양 한 뷰 모델 및 모델 멤버 간의 연결을 정의 하는 합니다. 코드 숨김 파일 경우에 따라 사용자 지정 하거나 UI 조작 또는 이벤트 처리기 인수에서 작업을 수행 하는 보기 모델 메서드를 호출 하기 전에 데이터를 추출 하는 데 필요한 추가 코드를 포함 하도록 뷰 계층의 일부로 사용 됩니다. 
* 합니다 **뷰 모델** 계층 뷰에 대 한 데이터 바인딩 대상을 제공 합니다. 대부분의 경우에서 뷰 모델은 모델을 직접 노출 하거나 특정 모델 멤버를 래핑하는 멤버를 제공 합니다. 보기-모델을 추적 하기 위한 관련 된 데이터 멤버를 정의할 수도 있습니다 UI 아니라 항목의 목록 표시 순서와 같은 모델입니다. 뷰 모델은 데이터베이스 액세스 코드와 같은 다른 서비스와 통합 지점으로도 사용 됩니다. 간단한 프로젝트에 대 한 별도 모델 계층이 있지만 보기-모델 필요한 모든 데이터를 캡슐화 하는 하지 해야 할 수 있습니다. 

## <a name="basic-and-advanced-mvvm"></a>기본 및 고급 MVVM

모든 디자인 패턴에서와 마찬가지로 MVVM을 구현 하는 둘 이상의 방법 이며 다양 한 기술이 MVVM의 일부로 간주 됩니다. 따라서 여러 다양 한 타사 MVVM 프레임 워크 UWP를 비롯 한 다양 한 XAML 기반 플랫폼을 지 원하는 있습니다. 그러나 이러한 프레임 워크는 일반적으로 MVVM의 정확한 정의 다소 모호 하 분리 된 아키텍처를 구현 하는 것에 대 한 여러 서비스를 포함 합니다. 

정교한 MVVM 프레임 워크는 매우 유용 하지만 특히 엔터프라이즈급 프로젝트에 대 한 비용은 일반적으로 모든 특정 패턴 또는 기술을 도입 하는 데 관련 된 및 혜택은 항상 확장 및 크기에 따라 선택이 취소 프로젝트입니다. 다행 스럽게도 명확 하 고 실질적인 혜택을 제공 하는 기술만을 채택 하 고 필요할 때까지 다른 사용자가 무시할 수 있습니다. 

특히 많은 이해 하 고 데이터 바인딩 및 앞에서 설명한 레이어로 앱 논리를 분리 하의 모든 기능을 적용 하면 혜택을 얻을 수 있습니다. 모든 외부 프레임 워크를 사용 하지 않고 Windows SDK에서 제공 된 기능만 사용 하 여 수행할 수 있습니다. 특히 합니다 [{X:bind} 태그 확장](https://docs.microsoft.com/windows/uwp/xaml-platform/x-bind-markup-extension) 쉽고 이전 필요한 상용구 코드를 많이 필요 하지 않게 이전 XAML 플랫폼에서 보다 성능이 뛰어난 데이터 바인딩을 만듭니다.

Basic, 기본 제공 MVVM을 사용 하 여 추가 지침을 확인 합니다 [고객 주문 데이터베이스 샘플](https://github.com/Microsoft/Windows-appsample-customers-orders-database) GitHub에서. 다른 많은 [UWP 앱 샘플](https://github.com/Microsoft?q=windows-appsample
) 도 기본 MVVM 아키텍처를 사용 하며 [트래픽을 앱 샘플](https://github.com/Microsoft/Windows-appsample-trafficapp) 코드 숨김 및 MVVM 버전 모두에 대 한 설명을 포함는 [MVVM 변환 ](https://github.com/Microsoft/Windows-appsample-trafficapp/blob/MVVM/MVVM.md). 

## <a name="see-also"></a>참고 항목

### <a name="topics"></a>항목

[깊이에서 데이터 바인딩](https://docs.microsoft.com/windows/uwp/data-binding/data-binding-in-depth)  
[{X:bind} 태그 확장](https://docs.microsoft.com/windows/uwp/xaml-platform/x-bind-markup-extension)  

### <a name="samples"></a>샘플

[고객 주문 데이터베이스 샘플](https://github.com/Microsoft/Windows-appsample-customers-orders-database)  
[VanArsdel 인벤토리 샘플](https://github.com/Microsoft/InventorySample)  
[트래픽 앱 샘플](https://github.com/Microsoft/Windows-appsample-trafficapp)  
