---
ms.assetid: 159681E4-BF9E-4A57-9FEE-EC7ED0BEFFAD
title: MVVM 및 언어 성능 팁
description: 이 항목에서는 선택한 소프트웨어 디자인 패턴 및 프로그래밍 언어와 관련된 몇 가지 성능 고려 사항을 설명합니다.
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 9027362eccfb8130b181bee26a57f13ce1e1af66
ms.sourcegitcommit: 76e8b4fb3f76cc162aab80982a441bfc18507fb4
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/29/2020
ms.locfileid: "63785430"
---
# <a name="mvvm-and-language-performance-tips"></a>MVVM 및 언어 성능 팁


이 항목에서는 선택한 소프트웨어 디자인 패턴 및 프로그래밍 언어와 관련된 몇 가지 성능 고려 사항을 설명합니다.

## <a name="the-model-view-viewmodel-mvvm-pattern"></a>MVVM(Model-View-ViewModel) 패턴

MVVM(Model-View-ViewModel) 패턴은 다양한 XAML 앱에서 일반적으로 사용됩니다. (MVVM은 Model-View-Presenter 패턴에 대한 Fowler의 설명과 매우 유사하지만 XAML에 맞게 조정되었습니다.) MVVM 패턴의 문제는 의도와 달리 계층이 너무 많고 할당이 너무 많이 포함된 앱이 생길 수 있다는 점입니다. MVVM의 이점은 다음과 같습니다.

-   **관심사의 분리**. 문제를 작은 조각으로 분할하면 항상 유용하며, MVVM 또는 MVC와 같은 패턴은 앱(또는 단일 컨트롤)을 실제 보기, 보기의 논리 모델(보기 모델), 보기에 독립적인 앱 논리(모델) 등의 작은 조각으로 분할하는 방법입니다. 특히 많이 사용되는 워크플로는 디자이너가 하나의 도구를 사용하는 보기를 소유하고, 개발자는 다른 도구를 사용하는 모델을 소유하며, 디자인 통합자는 두 도구를 모두 사용하는 보기 모델을 소유하는 것입니다.
-   **단위 테스트**. 창 만들기, 입력 등에 의존하지 않고 보기에 독립적인 보기 모델(및 결과적으로 모델)에 대해 단위 테스트를 수행할 수 있습니다. 보기를 작게 유지하면 창을 만들지 않고도 앱의 많은 부분을 테스트할 수 있습니다.
-   **사용자 환경 변화에 따른 유연성**. 사용자 환경이 최종 사용자 의견에 따라 조정됨에 따라, 보기에는 가장 자주 발생하는 변경 및 가장 최근 변경이 표시되는 경향이 있습니다. 보기를 별도로 유지하면 앱에 미치는 변동을 줄이면서도 이러한 변경 사항을 더욱 신속하게 수용할 수 있습니다.

MVVM 패턴에 대한 여러 개의 구체적인 정의와 이를 구현하는 데 도움이 되는 타사 프레임워크가 있습니다. 그러나 특정 패턴 변형을 엄격하게 따르면 앱에 조정할 수 있는 것보다 훨씬 더 많은 오버헤드가 발생할 수 있습니다.

-   XAML 데이터 바인딩({Binding} 태그 확장)은 부분적으로 모델/보기 패턴을 사용하도록 설계되었습니다. 하지만 {Binding}을 사용하면 특수 작업 집합과 CPU 오버헤드가 발생합니다. {Binding}을 만들면 일련의 할당이 발생하고, 바인딩 대상을 업데이트하면 리플렉션 및 boxing이 발생할 수 있습니다. 이 문제는 빌드 시 바인딩을 컴파일하는 {x:Bind} 태그 확장을 통해 해결됩니다. **권장 사항:** {x:Bind}를 사용합니다.
-   MVVM에서는 일반적인 DelegateCommand 또는 RelayCommand 도우미와 같은 ICommand를 사용하여 Button.Click을 보기 모델에 연결하는 방법을 많이 사용합니다. 이러한 명령은 CanExecuteChanged 이벤트 수신기 포함, 작업 집합에 추가, 페이지의 시작/탐색 시간에 추가 등을 포함한 추가 할당입니다. **권장 사항:** 편리한 ICommand 인터페이스를 사용하는 대신, 이벤트 처리기를 코드 숨김에 배치하고 보기 이벤트에 추가한 다음, 이러한 이벤트 발생 시 보기 모델에 대해 명령을 호출합니다. 또한 명령을 사용할 수 없는 경우 단추를 비활성화하는 추가 코드를 추가해야 합니다.
-   MVVM에서는 가능한 모든 UI 구성을 사용하여 페이지를 만든 다음 VM의 속성에 표시 여부 속성을 바인딩하여 트리의 일부를 축소하는 방법을 많이 사용합니다. 이렇게 하면 시작 시간이 불필요하게 늘어나며 작업 집합도 많아질 수 있습니다(트리의 일부가 전혀 표시되지 않을 수 있기 때문). **권장 사항:** [x:Load attribute](../xaml-platform/x-load-attribute.md) 또는 [x:DeferLoadStrategy 특성](../xaml-platform/x-deferloadstrategy-attribute.md) 기능을 사용하여 트리의 불필요한 부분을 시작에서 지연되도록 만듭니다. 또한 페이지의 다양한 모드별로 별도의 사용자 컨트롤을 만들고 코드 숨김을 사용하여 필수 컨트롤만 로드된 상태로 유지합니다.

## <a name="ccx-recommendations"></a>C++/CX 권장 사항

-   **최신 버전을 사용합니다**. C++/CX 컴파일러의 성능이 지속적으로 향상되고 있습니다. 최신 도구 집합을 사용하여 앱을 빌드해야 합니다.
-   **RTTI를 사용하지 않도록 설정합니다(/GR-)** . 컴파일러에서 RTTI는 기본적으로 켜져 있으므로, 빌드 환경에서 해제하지 않는 한 RTTI를 사용하게 됩니다. RTTI에는 상당한 오버헤드가 따르므로, 코드에 싶은 종속성이 없다면 RTTI를 해제해야 합니다. XAML 프레임워크에서는 코드에 RTTI를 사용하도록 하는 요구 사항이 없습니다.
-   **ppltasks를 과도하게 사용하지 않습니다**. ppltasks는 비동기 WinRT API를 호출할 때 매우 편리하지만 코드 크기 면에서 상당한 오버헤드가 발생합니다. C++/CX 팀은 훨씬 더 나은 성능을 제공하는 언어 기능을 위해 노력하고 있습니다. 이런 기능이 나오기 전까지는 코드의 실행 부하 과다 경로에서 균형 있게 ppltasks를 사용하세요.
-   **앱의 "비즈니스 논리"에 C++/CX를 사용하지 않습니다**. C++/CX는 C++ 앱에서 WinRT API에 액세스하는 편리한 방법으로 설계했습니다. 여기서는 오버헤드가 있는 래퍼를 사용합니다. C++/CX를 클래스의 비즈니스 논리/모델 내부에 사용하지 말고 코드와 WinRT 사이의 경계에 사용하도록 보존해야 합니다.