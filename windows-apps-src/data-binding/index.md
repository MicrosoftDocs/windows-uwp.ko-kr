---
ms.assetid: 83b4be37-6613-4d00-a48a-0451a24a30fb
title: 데이터 바인딩
description: 데이터 바인딩은 앱의 UI에서 데이터를 표시하고 선택적으로 해당 데이터와 동기화된 상태를 유지하는 하나의 방법입니다.
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 9f4f83711c30aab94005e90e7de2db4e4b48bfc8
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89157197"
---
# <a name="data-binding"></a>데이터 바인딩

데이터 바인딩은 앱의 UI에서 데이터를 표시하고 선택적으로 해당 데이터와 동기화된 상태를 유지하는 하나의 방법입니다. 데이터 바인딩은 데이터 문제를 UI 문제와 분리하여 개념 모델을 간소화하고 앱의 가독성, 테스트 용이성 및 유지 관리성을 향상시킬 수 있도록 해줍니다. 태그에서 [{x:Bind} 태그 확장](../xaml-platform/x-bind-markup-extension.md) 또는 [{Binding} 태그 확장](../xaml-platform/binding-markup-extension.md)을 사용하도록 선택할 수 있습니다. 동일한 앱에서 이 둘을 혼합하여 사용할 수도 있으며, 동일한 UI 요소에도 마찬가지입니다. {x:Bind}는 Windows 10의 새로운 기능으로, 향상된 성능을 제공합니다.

| 항목 | 설명 |
|-------|-------------|
| [데이터 바인딩 개요](data-binding-quickstart.md) | 이 항목에서는 UWP(유니버설 Windows 플랫폼) 앱에서 컨트롤(또는 다른 UI 요소)을 단일 항목에 바인딩하거나 항목 컨트롤을 항목 컬렉션에 바인딩하는 방법을 보여 줍니다. 또한 항목의 렌더링을 제어하고 선택 항목을 기반으로 세부 정보 보기를 구현하고, 표시할 데이터를 변환하는 방법을 보여 줍니다. 자세한 내용은 [데이터 바인딩 심층 분석](data-binding-in-depth.md)을 참조하세요. | 
| [데이터 바인딩 심층 분석](data-binding-in-depth.md) | 이 항목에서는 데이터 바인딩 기능에 대해 자세히 설명합니다. |
| [디자인 화면의 샘플 데이터 및 프로토타입 생성용 샘플 데이터](displaying-data-in-the-designer.md) | Visual Studio 디자이너에서 컨트롤에 데이터를 채워 앱의 레이아웃, 템플릿 및 기타 시각적 속성을 작업할 수 있도록 하기 위해 다양한 방법으로 디자인 타임 샘플 데이터를 사용할 수 있습니다. 스케치(또는 프로토타입) 앱을 빌드하는 경우에도 샘플 데이터를 사용하면 정말 유용하고 시간을 절약할 수 있습니다. 스케치 또는 프로토타입에서 런타임에 샘플 데이터를 사용하면 실제 라이브 데이터에 연결하지 않더라도 아이디어를 설명할 수 있습니다. |
| [계층적 데이터에 바인딩하고 마스터/자세히 보기 만들기](how-to-bind-to-hierarchical-data-and-create-a-master-details-view.md) | 체인으로 함께 바인딩된 [<strong>CollectionViewSource</strong>](/uwp/api/Windows.UI.Xaml.Data.CollectionViewSource) 인스턴스에 항목 컨트롤을 바인딩하여 계층적 데이터에 대한 여러 수준 마스터/세부 정보(목록-세부 정보라고도 함) 보기를 만들 수 있습니다. |
| [데이터 바인딩 및 MVVM](data-binding-and-mvvm.md) | 이 토픽에서는 MVVM(Model-View-ViewModel) UI 아키텍처 디자인 패턴을 설명합니다. 데이터 바인딩은 MVVM의 핵심이며, UI와 UI가 아닌 코드 간의 느슨한 결합을 지원합니다. |