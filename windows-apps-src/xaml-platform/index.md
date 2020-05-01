---
ms.assetid: b632a6cc-3503-4ab8-bfd1-dde731bd89ab
description: 이 섹션에는 UWP(유니버설 Windows 플랫폼) 앱을 위한 XAML 프레임워크를 설명하는 토픽이 포함되어 있습니다.
title: XAML 플랫폼
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 84427f0cec6aa0921f903fff9b5e74b4da5a58a9
ms.sourcegitcommit: f727b68e86a86c94eff00f67ed79a1c12666e7bc
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/29/2020
ms.locfileid: "68623388"
---
# <a name="xaml-platform"></a>XAML 플랫폼

이 섹션에는 C#, Visual Basic 또는 Visual C++ 구성 요소 확장(C++/CX)과 UI 정의용 XAML을 사용하여 작성하는 앱에 일반적으로 적용할 수 있는 프로그래밍 개념에 대해 설명하는 토픽이 포함되어 있습니다. 프로그래밍 개념으로는 속성 및 이벤트의 사용 방법 및 UWP(유니버설 Windows 플랫폼) 앱 프로그래밍에 적용되는 방식이 포함됩니다. 유니버설 Windows 플랫폼은 종속성 속성 시스템을 추가하여 속성의 C#, Visual Basic 또는 C++/CX 개념과 해당 값을 확장합니다. 또한 이 섹션의 토픽에서는 UWP에 사용되는 XAML 언어를 문서화하고 XAML을 사용하여 UWP 앱의 UI를 정의하는 방법에 대한 고급 시나리오를 다룹니다.

| 항목 | 설명 |
|-------|-------------|
| [XAML 개요](xaml-overview.md) | Windows 런타임 앱 개발자에게 XAML 언어와 개념을 소개하고, XAML에서 Windows 런타임 앱을 만드는 데 사용되는 개체를 선언하고 특성을 설정하는 다양한 방법을 설명합니다. |
| [종속성 속성 개요](dependency-properties-overview.md) | UI의 XAML 정의와 함께 C++, C# 또는 Visual Basic을 사용하여 Windows 런타임 앱을 작성할 때 사용할 수 있는 종속성 속성 시스템에 대해 설명합니다. |
| [사용자 지정 종속성 속성](custom-dependency-properties.md) | C++, C# 또는 Visual Basic으로 작성한 Windows 런타임 앱의 사용자 지정 종속성 속성을 정의하고 구현하는 방법에 대해 설명합니다. |
| [연결된 속성 개요](attached-properties-overview.md) | XAML의 연결된 속성에 대한 개념을 설명하고 몇 가지 예를 제공합니다. |
| [사용자 지정 연결된 속성](custom-attached-properties.md) | XAML 연결된 속성을 종속성 속성으로 구현하는 방법 및 연결된 속성을 XAML에서 사용 가능하게 하는 데 필요한 접근자 규칙을 정의하는 방법에 대해 설명합니다. |
| [이벤트 및 라우트된 이벤트 개요](events-and-routed-events-overview.md) | 프로그래밍 언어로 C#, Visual Basic 또는 C++/CX를 사용하고 UI 정의에 XAML을 사용할 때 Windows 런타임 앱의 이벤트 프로그래밍 개념에 대해 설명합니다. 이벤트 처리기를 XAML에서 UI 요소 선언의 일부로 할당하거나 코드에서 처리기를 추가할 수 있습니다. Windows 런타임은 *라우트된 이벤트*를 지원합니다. 이 기능을 통해 특정 입력 이벤트와 데이터 이벤트가 이벤트를 발생시킨 개체가 아닌 다른 개체에 의해 처리될 수 있습니다. 라우트된 이벤트는 컨트롤 템플릿을 정의하거나 페이지 또는 레이아웃 컨테이너를 사용할 때 유용합니다. |
|[데스크톱 앱의 UWP 컨트롤(XAML Islands)](/windows/apps/desktop/modernize/xaml-islands)| UWP XAML 컨트롤을 사용하여 Windows Forms, WPF 또는 Win 32 데스크톱 애플리케이션의 UI를 개선하는 방법을 설명합니다.|
