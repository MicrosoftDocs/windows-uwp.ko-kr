---
author: Xansky
Description: Introduces accessibility concepts that relate to Universal Windows Platform (UWP) apps.
ms.assetid: C89D79C2-B830-493D-B020-F3FF8EB5FFDD
title: 접근성
label: Accessibility
template: detail.hbs
ms.author: mhopkins
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 5827bf0c9ddb7f5ebbab3f34e40e08d1620b6e0c
ms.sourcegitcommit: 086001cffaf436e6e4324761d59bcc5e598c15ea
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/27/2018
ms.locfileid: "5700800"
---
# <a name="accessibility"></a>접근성  



UWP(유니버설 Windows 플랫폼) 앱과 관련된 접근성 개념을 소개합니다.

접근성이란 다양한 환경에서 기술을 사용하는 사람들이 응용 프로그램을 사용할 수 있도록 하는 환경을 구축하는 것으로, 다양한 요구 사항 및 환경을 염두에 두고 사용자 인터페이스를 다룹니다. 일부 상황의 경우 접근성 요구 사항이 법으로 지정됩니다. 그러나 앱이 최대한 많은 사용자층을 확보할 수 있도록 하려면 법적 요구 사항에 관계없이 접근성 문제를 해결하는 것이 좋습니다. 앱의 접근성과 관련된 Microsoft Store 선언도 있습니다.

> [!NOTE]
> 앱을 접근성이 있는 앱으로 선언하는 것은 Microsoft Store와만 관련이 있습니다.

| 문서 | 설명 |
|---------|-------------|
| [접근성 개요](accessibility-overview.md) | 이 문서는 UWP 앱의 접근성 시나리오와 관련된 개념 및 기술에 대한 개요입니다. |
| [포괄 소프트웨어 디자인](designing-inclusive-software.md) | Windows 10용 UWP 앱을 사용하여 포괄 디자인을 개발하는 방법에 대해 알아보세요.  접근성을 염두에 둔 포괄 소프트웨어를 디자인하고 빌드하세요. |
| [포괄 Windows 앱 개발](developing-inclusive-windows-apps.md) | 이 문서는 접근성 있는 UWP 앱을 개발하기 위한 로드맵입니다. |
| [접근성 테스트](accessibility-testing.md) | UWP 앱에 접근성을 구현하기 위해 따라야 할 몇 가지 테스트 절차입니다. |
| [스토어의 접근성](accessibility-in-the-store.md) | UWP 앱을 Microsoft Store에서 접근 가능한 앱으로 선언하기 위한 요구 사항에 대해 설명합니다. |
| [접근성 검사 목록](accessibility-checklist.md) | UWP 앱이 접근 가능한지 확인하는 검사 목록을 제공합니다. |
| [기본적인 접근성 정보 표시](basic-accessibility-information.md) | 경우에 따라 기본 접근성 정보는 이름, 역할 및 값으로 분류됩니다. 이 항목에서는 보조 기술이 필요로 하는 기본 정보를 앱에 표시하는 데 도움이 되는 코드에 대해 설명합니다. |
| [키보드 접근성](keyboard-accessibility.md) | 따라서 앱의 키보드 접근성이 좋지 않을 경우 시각 장애나 이동성 문제가 있는 사용자가 앱을 사용하는 데 어려움을 겪거나 아예 사용하지 못할 수 있습니다. |
| [랜드마크 및 머리글](landmarks-and-headings.md) | 랜드마크 및 머리글은 화면 읽기 프로그램 등의 보조 기술의 사용자에 대한 효율적인 탐색에 도움이 되는 사용자 인터페이스의 섹션을 정의합니다. |
| [고대비 테마](high-contrast-themes.md) | 고대비 테마가 활성 상태일 때 UWP 앱을 사용할 수 있도록 하는 데 필요한 단계에 대해 설명합니다. |
| [접근성 있는 텍스트 요구 사항](accessible-text-requirements.md) | 이 항목에서는 색 및 배경이 필요한 명암비를 충족하도록 하여 앱 텍스트의 접근성에 대한 모범 사례를 설명합니다. 또한 이 항목에서는 UWP 앱의 텍스트 요소에 부여될 수 있는 Microsoft UI 자동화 역할 및 그래픽의 텍스트에 대한 모범 사례를 설명합니다. |
| [피해야 할 접근성 사례](practices-to-avoid.md) | UWP 앱에 접근성을 구현하려는 경우 피해야 할 사례에 대해 설명합니다. |
| [사용자 지정 자동화 피어](custom-automation-peers.md) | UI 자동화의 자동화 피어 개념과 고유한 사용자 지정 UI 클래스에 대해 자동화 지원을 제공하는 방법을 설명합니다. |
| [컨트롤 패턴 및 인터페이스](control-patterns-and-interfaces.md) | Microsoft UI 자동화 컨트롤 패턴, 클라이언트가 컨트롤 패턴에 액세스하는 데 사용하는 클래스 및 공급자가 컨트롤 패턴을 구현하는 데 사용하는 인터페이스를 나열합니다. |

## <a name="related-topics"></a>관련 항목  
* [**Windows.UI.Xaml.Automation**](https://msdn.microsoft.com/library/windows/apps/BR209179) 
* [내레이터 시작](https://support.microsoft.com/en-us/help/22798/windows-10-narrator-get-started)