---
Description: Windows 앱에 액세스할 수 있는지 확인 하는 데 도움이 되는 검사 목록을 제공 합니다.
ms.assetid: BB8399E2-7013-4F77-AF2C-C1A0E5412856
title: 접근성 검사 목록
label: Accessibility checklist
template: detail.hbs
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 44864a0743443d976456f73a3bae5041fd63770e
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89163467"
---
# <a name="accessibility-checklist"></a>접근성 검사 목록

Windows 앱에 액세스할 수 있는지 확인 하는 데 도움이 되는 검사 목록을 제공 합니다.

여기서는 앱에 액세스할 수 있도록 하는 데 사용할 수 있는 검사 목록을 제공 합니다.

1. 앱의 콘텐츠 및 대화형 UI 요소에 대 한 액세스 가능 이름 (필수) 및 설명 (선택 사항)을 설정 합니다.

    액세스 가능한 이름은 화면 읽기 프로그램에서 UI 요소를 알리기 위해 사용 하는 간단한 설명 텍스트 문자열입니다. [**TextBlock**](/uwp/api/Windows.UI.Xaml.Controls.TextBlock) 및 [**TextBox**](/uwp/api/Windows.UI.Xaml.Controls.TextBox) 와 같은 일부 UI 요소는 텍스트 콘텐츠를 기본 액세스 가능 이름으로 승격 합니다. [기본 접근성 정보](basic-accessibility-information.md#name_from_inner_text)를 참조 하세요.

    내부 텍스트 콘텐츠를 암시적으로 액세스할 수 있는 이름으로 승격 하지 않는 이미지 또는 기타 컨트롤에 대해 액세스 가능한 이름을 명시적으로 설정 해야 합니다. 레이블 텍스트를 레이블 및 입력의 상관 관계를 지정 하기 위해 Microsoft UI 자동화 모델에서 [**있으면 labeledby**](/previous-versions/windows/silverlight/dotnet-windows-silverlight/ms591292(v=vs.95)) 대상으로 사용할 수 있도록 폼 요소에 레이블을 사용 해야 합니다. 일반적으로 액세스 가능한 이름에 포함 된 것 보다 더 많은 UI 지침을 사용자에 게 제공 하려는 경우 액세스 가능한 설명 및 도구 설명을 통해 사용자가 UI를 이해 하는 데 도움을 줍니다.

    자세한 내용은 [액세스 가능한 이름](basic-accessibility-information.md#accessible_name) 및 [액세스 가능한 설명](basic-accessibility-information.md)을 참조 하세요.

2. 키보드 접근성 구현:

    * UI에 대 한 기본 탭 인덱스 순서를 테스트 합니다. 필요한 경우 탭 인덱스 순서를 조정 합니다 .이 경우 특정 컨트롤을 사용 하거나 사용 하지 않도록 설정 하거나 일부 UI 요소에서 [**TabIndex**](/uwp/api/windows.ui.xaml.controls.control.tabindex) 의 기본값을 변경 해야 할 수 있습니다.
    * 복합 요소에 대 한 화살표 키 탐색을 지 원하는 컨트롤을 사용 합니다. 기본 컨트롤의 경우 화살표 키 탐색은 일반적으로 이미 구현 되어 있습니다.
    * 키보드 활성화를 지 원하는 컨트롤을 사용 합니다. 기본 컨트롤의 경우, 특히 UI 자동화 [**호출**](/uwp/api/Windows.UI.Xaml.Automation.Provider.IInvokeProvider) 패턴을 지 원하는 컨트롤의 경우 키보드 활성화를 일반적으로 사용할 수 있습니다. 해당 컨트롤에 대 한 설명서를 확인 합니다.
    * 상호 작용을 지 원하는 UI의 특정 부분에 대 한 액세스 키를 설정 하거나 액셀러레이터 키를 구현 합니다.
    * UI에서 사용 하는 사용자 지정 컨트롤의 경우 활성화를 위한 올바른 [**Automationpeer**](/uwp/api/Windows.UI.Xaml.Automation.Peers.AutomationPeer) 지원 및 활성화, 트래버스 및 액세스 또는 액셀러레이터 키를 지 원하는 데 필요한 키 처리에 대해 정의 된 재정의를 사용 하 여 이러한 컨트롤을 구현 했는지 확인 합니다.

    자세한 내용은 [키보드 상호 작용](../input/keyboard-interactions.md)을 참조 하세요.

3. 텍스트의 크기를 읽을 수 있는지 확인

    * Windows에는 사용자가 사용할 수 있는 다양 한 내게 필요한 옵션 도구 및 설정이 포함 되며,이를 통해 사용자가 원하는 대로 텍스트를 읽을 수 있습니다. 여기에는 다음이 포함됩니다.
        * UI의 선택한 영역을 확대 하는 돋보기 도구입니다. 앱에서 텍스트의 레이아웃을 사용 하 여 편집용으로 돋보기를 사용 하는 것이 어려울 수 있도록 해야 합니다.
        * **설정->시스템->디스플레이 >크기 조정 및 레이아웃에 대**한 전역 크기 조정 및 해상도 설정 사용 가능한 크기 옵션은 표시 장치의 기능에 따라 달라질 수 있습니다.
        * 설정의 텍스트 크기 설정 **-접근성 >표시를 >** 합니다. **텍스트 크게 만들기** 설정을 조정 하 여 모든 응용 프로그램 및 화면에서 지원 컨트롤의 텍스트 크기만 지정 합니다. 모든 UWP 텍스트 컨트롤은 사용자 지정 또는 템플릿 없이 텍스트 크기 조정 환경을 지원 합니다.
        > [!NOTE]
        > **모든 항목을 크게** 설정 하면 사용자가 기본 화면 에서만 일반 텍스트 및 앱에 대 한 기본 설정 크기를 지정할 수 있습니다.

4. UI를 시각적으로 확인 하 여 텍스트 대비가 충분 한지 확인 하 고, 요소가 고대비 테마에서 올바르게 렌더링 되며, 색이 올바르게 사용 되는지 확인 합니다.

    * 색 분석기 도구를 사용 하 여 시각적 텍스트 대비 비율이 4.5:1 이상 인지 확인 합니다.
    * 고대비 테마로 전환 하 고 앱에 대 한 UI를 읽고 사용할 수 있는지 확인 합니다.
    * UI에서 정보를 전달 하는 유일한 방법으로 색을 사용 하지 않는지 확인 합니다.

    자세한 내용은 고대비 [테마](high-contrast-themes.md) 및 [액세스 가능한 텍스트 요구 사항](accessible-text-requirements.md)을 참조 하세요.

5. 내게 필요한 옵션 도구를 실행 하 고 보고 된 문제를 해결 하며 화면 읽기 환경을 확인 합니다.

    [**검사**](/windows/desktop/WinAuto/inspect-objects) 와 같은 도구를 사용 하 여 프로그래밍 방식 액세스를 확인 하 고, [**accchecker**](/windows/desktop/WinAuto/ui-accessibility-checker) 와 같은 진단 도구를 실행 하 여 일반적인 오류를 검색 하 고, 내레이터에서 화면 읽기 환경을 확인 합니다.

    자세한 내용은 [내게 필요한 옵션 테스트](accessibility-testing.md)를 참조 하십시오.

6. 응용 프로그램 매니페스트 설정이 내게 필요한 옵션 지침을 따르는지 확인 합니다.

7. Microsoft Store에서 액세스할 수 있는 앱을 선언 합니다.

    기본 내게 필요한 옵션 지원 기능을 구현한 경우 Microsoft Store에서 액세스할 수 있는 앱을 선언 하면 더 많은 고객에 게 연결 하 고 몇 가지 추가 좋은 등급을 얻을 수 있습니다.

    자세한 내용은 [스토어의 접근성](accessibility-in-the-store.md)을 참조 하세요.

## <a name="related-topics"></a>관련 항목  

* [접근성 있는 텍스트 요구 사항](accessible-text-requirements.md)
* [텍스트 크기 조정](../input/text-scaling.md)
* [접근성](accessibility.md)
* [접근성을 위한 디자인](./accessibility-overview.md)
* [예방 방법](practices-to-avoid.md)