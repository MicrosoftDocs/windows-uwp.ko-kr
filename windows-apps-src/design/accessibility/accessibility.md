---
description: Windows 앱과 관련 된 접근성 개념을 소개 합니다.
ms.assetid: C89D79C2-B830-493D-B020-F3FF8EB5FFDD
title: 액세스 가능성
label: Accessibility
template: detail.hbs
ms.date: 09/24/2020
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: f49c784dcd6ed8872753bf4aedd9e79718ccfd99
ms.sourcegitcommit: a3bbd3dd13be5d2f8a2793717adf4276840ee17d
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/30/2020
ms.locfileid: "93032607"
---
# <a name="accessibility"></a>액세스 가능성  

내게 필요한 옵션은 다양 한 환경에서 기술을 사용 하는 사용자가 Windows 응용 프로그램을 사용 하 고 다양 한 요구 사항과 경험을 통해 UI에 접근 하는 환경에 대 한 것입니다. 일부 상황의 경우 접근성 요구 사항이 법으로 지정됩니다. 그러나 앱이 최대한 많은 사용자층을 확보할 수 있도록 하려면 법적 요구 사항에 관계없이 접근성 문제를 해결하는 것이 좋습니다.

> 앱에 대 한 액세스 가능성과 관련 된 Microsoft Store 선언도 있습니다.

| 아티클 | 설명 |
|---------|-------------|
| [접근성 개요](accessibility-overview.md) | 이 문서는 Windows 앱에 대 한 내게 필요한 옵션 시나리오와 관련 된 개념과 기술에 대 한 개요입니다. |
| [포괄 소프트웨어 디자인](designing-inclusive-software.md) | Windows 10 용 Windows 앱으로 진화 하는 포괄 디자인에 대해 알아봅니다.  접근성을 염두에 둔 포괄 소프트웨어를 디자인하고 빌드하세요. |
| [포괄 Windows 앱 개발](developing-inclusive-windows-apps.md) | 이 문서는 액세스 가능한 Windows 앱 개발을 위한 로드맵입니다. |
| [접근성 테스트](accessibility-testing.md) | Windows 앱에 액세스할 수 있도록 절차를 테스트 합니다. |
| [스토어의 접근성](accessibility-in-the-store.md) | Microsoft Store에서 액세스할 수 있는 Windows 앱을 선언 하기 위한 요구 사항을 설명 합니다. |
| [접근성 검사 목록](accessibility-checklist.md) | Windows 앱에 액세스할 수 있는지 확인 하는 데 도움이 되는 검사 목록을 제공 합니다. |
| [기본적인 접근성 정보 표시](basic-accessibility-information.md) | 경우에 따라 기본 접근성 정보는 이름, 역할 및 값으로 분류됩니다. 이 항목에서는 보조 기술이 필요로 하는 기본 정보를 앱에 표시하는 데 도움이 되는 코드에 대해 설명합니다. |
| [키보드 접근성](keyboard-accessibility.md) | 따라서 앱의 키보드 접근성이 좋지 않을 경우 시각 장애나 이동성 문제가 있는 사용자가 앱을 사용하는 데 어려움을 겪거나 아예 사용하지 못할 수 있습니다. |
| [화면 판독기 및 하드웨어 시스템 단추](system-button-narration.md) | [내레이터](https://support.microsoft.com/en-us/help/22798/windows-10-complete-guide-to-narrator)와 같은 화면 판독기는 하드웨어 시스템 단추 이벤트를 인식 하 고 처리 하 고 사용자에 게 상태를 전달할 수 있어야 합니다. 경우에 따라 화면 판독기에서 단추 이벤트를 배타적으로 처리 해야 하 고 다른 처리기로 버블링 하지 않을 수 있습니다. |
| [랜드마크 및 머리글](landmarks-and-headings.md) | 랜드마크 및 머리글은 화면 읽기 프로그램 등의 보조 기술의 사용자에 대한 효율적인 탐색에 도움이 되는 사용자 인터페이스의 섹션을 정의합니다. |
| [고대비 테마](high-contrast-themes.md) | 고대비 테마가 활성화 되어 있을 때 Windows 앱을 사용할 수 있는지 확인 하는 데 필요한 단계를 설명 합니다. |
| [접근성 있는 텍스트 요구 사항](accessible-text-requirements.md) | 이 항목에서는 색 및 배경이 필요한 명암비를 충족하도록 하여 앱 텍스트의 접근성에 대한 모범 사례를 설명합니다. 이 항목에서는 Windows 앱의 텍스트 요소에 포함 될 수 있는 Microsoft UI Automation 역할 및 그래픽의 텍스트에 대 한 모범 사례에 대해서도 설명 합니다. |
| [피해야 할 접근성 사례](practices-to-avoid.md) | 액세스 가능한 Windows 앱을 만들려는 경우 방지할 수 있는 방법을 나열 합니다. |
| [사용자 지정 자동화 피어](custom-automation-peers.md) | UI 자동화에 대 한 자동화 피어의 개념 및 사용자 지정 UI 클래스에 대 한 자동화 지원을 제공 하는 방법을 설명 합니다. |
| [컨트롤 패턴 및 인터페이스](control-patterns-and-interfaces.md) | Microsoft UI 자동화 컨트롤 패턴, 클라이언트에서 액세스 하는 데 사용 하는 클래스 및 인터페이스 공급자를 사용 하 여 구현 합니다. |

## <a name="related-topics"></a>관련 항목  
* [**Windows. .Xaml.**](/uwp/api/Windows.UI.Xaml.Automation) 
* [내레이터 시작](https://support.microsoft.com/help/22798/windows-10-complete-guide-to-narrator)
