---
author: jwmsft
ms.assetid: b632a6cc-3503-4ab8-bfd1-dde731bd89ab
description: 이 섹션에는 UWP(유니버설 Windows 플랫폼) 앱을 위한 XAML 프레임워크를 설명하는 항목이 포함되어 있습니다.
title: XAML 플랫폼
ms.author: jimwalk
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: fe9bad5afabca3ef0b9c782446e581aea61fe4dc
ms.sourcegitcommit: 086001cffaf436e6e4324761d59bcc5e598c15ea
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/26/2018
ms.locfileid: "5694169"
---
# <a name="xaml-platform"></a>XAML 플랫폼


이 섹션에는 C#, Microsoft Visual Basic 또는 VisualC + + 구성 요소 확장을 사용 하는 경우 작성 하는 모든 앱에 일반적으로 적용할 수 있는 프로그래밍 개념을 설명 하는 항목이 포함 됩니다 (C + + CX) 프로그래밍 언어와 XAML UI에 대 한 정의 합니다. 여기에는 속성과 이벤트 사용과 같은 기본적인 프로그래밍 개념 및 이 개념이 UWP(유니버설 Windows 플랫폼) 앱 프로그래밍에 적용되는 방식이 포함됩니다. UWP(유니버설 Windows 플랫폼)는 종속성 속성 시스템을 추가하여 속성의 C#, Visual Basic 또는 C++/CX 개념 및 해당 값을 확장합니다. 또한 이 섹션의 항목에서는 UWP에서 사용되는 XAML 언어를 문서화하고 기본 시나리오뿐만 아니라 XAML을 사용하여 UWP 앱의 UI를 정의하는 방법을 설명하는 고급 항목도 다룹니다.

| 항목 | 설명 |
|-------|-------------|
| [XAML 개요](xaml-overview.md) | Windows 런타임 앱 개발자에게 XAML 언어와 XAML 개념을 소개하고 XAML에서 Windows 런타임 앱을 만드는 데 사용되는 개체를 선언하고 특성을 설정하는 다양한 방법을 설명합니다. |
| [종속성 속성 개요](dependency-properties-overview.md) | 이 항목에서는 UI의 XAML 정의와 함께 C++, C# 또는 Visual Basic을 사용하여 Windows 런타임 앱을 작성할 때 사용할 수 있는 종속성 속성 시스템에 대해 설명합니다. |
| [사용자 지정 종속성 속성](custom-dependency-properties.md) | C++, C# 또는 Visual Basic으로 작성한 Windows 런타임 앱의 사용자 지정 종속성 속성을 정의하고 구현하는 방법에 대해 설명합니다. |
| [연결된 속성 개요](attached-properties-overview.md) | XAML의 연결된 속성에 대한 개념을 설명하고 몇 가지 예를 제공합니다. |
| [사용자 지정 연결된 속성](custom-attached-properties.md) | XAML 연결된 속성을 종속성 속성으로 구현하는 방법 및 연결된 속성을 XAML에서 사용 가능하게 하는 데 필요한 접근자 규칙을 정의하는 방법에 대해 설명합니다. |
| [이벤트 및 라우트된 이벤트 개요](events-and-routed-events-overview.md) | 프로그래밍 언어로 C#, Visual Basic 또는 C++/CX를 사용하고 UI 정의에 XAML을 사용하는 경우 Windows 런타임 앱의 이벤트 프로그래밍 개념에 대해 설명합니다. 이벤트 처리기를 XAML에서 UI 요소 선언의 일부로 할당하거나 코드에서 처리기를 추가할 수 있습니다. Windows 런타임은 **라우트된 이벤트**를 지원합니다. 이 기능을 통해 특정 입력 이벤트와 데이터 이벤트가 이벤트를 발생시킨 개체가 아닌 다른 개체에 의해 처리될 수 있습니다. 라우트된 이벤트는 컨트롤 템플릿을 정의하거나 페이지 또는 레이아웃 컨테이너를 사용하는 경우 유용합니다. |
|[WPF 및 Windows Forms 응용 프로그램의 호스트 UWP 컨트롤](xaml-host-controls.md)| UWP XAML 컨트롤을 사용하여 Windows Forms 또는 WPF 데스크톱 응용 프로그램의 UI를 개선하는 방법을 설명합니다.|
