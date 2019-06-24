---
Description: UWP(유니버설 Windows 플랫폼) 앱에 접근성이 있는지 확인하는 검사 목록을 제공합니다.
ms.assetid: BB8399E2-7013-4F77-AF2C-C1A0E5412856
title: 접근성 검사 목록
label: Accessibility checklist
template: detail.hbs
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 9b789102b90e63e5456c030f6eb90df372354d7a
ms.sourcegitcommit: 6f32604876ed480e8238c86101366a8d106c7d4e
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 06/21/2019
ms.locfileid: "67319005"
---
# <a name="accessibility-checklist"></a>접근성 검사 목록

UWP(유니버설 Windows 플랫폼) 앱이 접근 가능한지 확인하는 검사 목록을 제공합니다.

여기서는 앱에 접근성이 있는지 확인하는 데 사용할 수 있는 검사 목록을 제공합니다.

1. 앱에 있는 콘텐츠 및 대화형 UI 요소의 접근성 있는 이름(필수) 및 설명(선택)을 설정합니다.

    접근성 있는 이름은 화면 읽기 프로그램이 UI 요소를 읽기 위해 사용하는 짧은 설명 텍스트 문자열입니다. [  **TextBlock**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.TextBlock) 및 [**TextBox**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.TextBox) 같은 일부 UI 요소는 해당 텍스트 콘텐츠를 기본 접근성 있는 이름으로 승격시킵니다. [기본 접근성 정보](basic-accessibility-information.md#name_from_inner_text)를 참조하세요.

    내부 텍스트 콘텐츠를 명시적인 접근성 있는 이름으로 승격시키지 않는 이미지 또는 기타 컨트롤에 대해서는 명시적으로 접근성 있는 이름을 설정해야 합니다. 레이블과 입력을 상호 연결하는 Microsoft UI 자동화 모델에서 레이블 텍스트를 [**LabeledBy**](https://docs.microsoft.com/previous-versions/windows/silverlight/dotnet-windows-silverlight/ms591292(v=vs.95)) 대상으로 사용할 수 있도록 양식 요소에는 레이블을 사용해야 합니다. 일반적으로 접근성 있는 이름에 포함된 것보다 더 많은 UI 지침을 사용자에게 제공하려는 경우 접근성 있는 설명 및 도구 설명을 사용하면 사용자가 UI를 이해하는 데 도움이 됩니다.

    자세한 내용은 [접근성 있는 이름](basic-accessibility-information.md#accessible_name) 및 [접근성 있는 설명](basic-accessibility-information.md)을 참조하세요.

2. 키보드 접근성 구현

    * UI의 기본 탭 인덱스 순서를 테스트합니다. 필요한 경우 탭 인덱스 순서를 조정하여 특정 컨트롤을 사용하거나 사용하지 않도록 설정하거나 일부 UI 요소에서 [**TabIndex**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.control.tabindex)의 기본값을 변경해야 할 수 있습니다.
    * 복합 요소에 화살표 키 탐색을 지원하는 컨트롤을 사용합니다. 기본 컨트롤의 경우 화살표 키 탐색이 일반적으로 이미 구현되어 있습니다.
    * 키보드 활성화를 지원하는 컨트롤을 사용합니다. 기본 컨트롤, 특히 UI 자동화 [**Invoke**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Automation.Provider.IInvokeProvider) 패턴을 지원하는 컨트롤의 경우 일반적으로 키보드를 활성화할 수 있습니다. 해당 컨트롤의 설명서를 확인하세요.
    * 조작을 지원하는 특정 UI 부분에 대해 선택키를 설정하거나 바로 가기 키를 구현합니다.
    * UI에서 사용하는 모든 사용자 지정 컨트롤의 경우 활성화를 위해 올바른 [**AutomationPeer**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Automation.Peers.AutomationPeer) 지원으로 이러한 컨트롤을 구현했으며 활성화, 통과 및 선택키 또는 바로 가기 키를 지원하기 위해 필요에 따라 키 처리의 재정의를 정의했는지 확인합니다.

    자세한 내용은 [키보드 조작](https://docs.microsoft.com/windows/uwp/input-and-devices/keyboard-interactions)을 참조하세요.

3. 텍스트는 읽을 수 있는 크기를 확인 합니다.

    * Windows에는 다양 한 내게 필요한 옵션 도구 및가 활용 하기 위해 자신의 요구 사항 및 텍스트 읽기 기본 설정을 조정할 수 있는 설정이 포함 됩니다. 이러한 개체는 다음과 같습니다.
        * UI의 선택된 영역 확대는 돋보기 도구입니다. 앱에서 텍스트의 레이아웃 하지 하기가 어려울 읽기용 돋보기를 사용 해야 합니다.
        * 전역 크기 및 해상도 설정을 **설정-> 시스템 표시->-> 확장 및 레이아웃**합니다. 정확 하 게 크기 조정을 사용할 수 있는 옵션으로 디스플레이 장치의 기능에 따라 달라 집니다. 달라질 수 있습니다.
        * 텍스트 크기 설정을 **설정에 대 한 액세스 편의성-> 표시->** 합니다. 조정 합니다 **텍스트를 더 크게 만들** 지원 컨트롤은 모든 응용 프로그램 및 (모든 UWP 텍스트 컨트롤 확장 환경 사용자 지정 또는 템플릿 없이 텍스트를 지원 하는 데 사용) 화면에서 텍스트의 크기를 지정 하는 설정입니다.
        > [!NOTE]
        > 합니다 **모든 것을 더 큰** 일반적만 해당 주 화면에서 텍스트 및 앱에 대 한 기본 크기를 지정 하는 사용자 수 있습니다.

4. UI를 시각적으로 검증하여 텍스트 대비가 적절한지, 요소가 고대비 테마에서 올바르게 렌더링되는지, 색이 제대로 사용되는지 확인합니다.

    * 색 분석기 도구를 사용하여 시각적 텍스트 대비가 4.5:1 이상인지 확인
    * 고대비 테마로 전환하고 앱의 UI를 읽고 사용할 수 있는지 검증
    * UI가 정보를 전달하는 유일한 수단으로 색을 사용하고 있지 않은지 확인

    자세한 내용은 [고대비 테마](high-contrast-themes.md) 및 [접근성 있는 텍스트에 대한 요구 사항](accessible-text-requirements.md)을 참조하세요.

5. 접근성 도구를 실행하고, 보고된 문제를 해결하고, 화면 읽기 환경을 확인합니다.

    [  **Inspect**](https://docs.microsoft.com/windows/desktop/WinAuto/inspect-objects) 같은 도구를 사용하여 프로그래밍 액세스를 확인하고 [**AccChecker**](https://docs.microsoft.com/windows/desktop/WinAuto/ui-accessibility-checker) 같은 진단 도구를 실행하여 일반 오류를 검색하며, 내레이터로 화면 읽기 환경을 확인합니다.

    자세한 내용은 [접근성 테스트](accessibility-testing.md)를 참조하세요.

6. 앱 매니페스트 설정이 접근성 지침을 따르는지 확인합니다.

7. Microsoft Store에서 접근할 수 있는 앱으로 선언하세요.

    기준 접근성 지원을 구현한 경우 Microsoft Store에서 접근할 수 있는 앱으로 선언하면 더 많은 고객에게 도달하고 좋은 평가를 받을 수 있습니다.

    자세한 내용은 [스토어의 접근성](accessibility-in-the-store.md)을 참조하세요.

## <a name="related-topics"></a>관련 항목  

* [접근성 있는 텍스트 요구 사항](accessible-text-requirements.md)
* [텍스트 크기 조정](../input/text-scaling.md)
* [액세스 가능성](accessibility.md)
* [내게 필요한 옵션에 대 한 디자인](https://docs.microsoft.com/windows/uwp/accessibility/accessibility-overview)
* [피해야 할 사례](practices-to-avoid.md)
