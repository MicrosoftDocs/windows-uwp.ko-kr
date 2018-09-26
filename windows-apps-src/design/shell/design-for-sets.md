---
author: jwmsft
description: Sets UI에서 최상의 환경을 제공하기 위해 앱을 최적화하는 방법을 알아보세요.
title: Sets용 설계
template: detail.hbs
ms.author: jimwalk
ms.date: 05/07/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: Windows 10, uwp, 제목 표시줄
doc-status: Draft
ms.localizationpriority: medium
ms.openlocfilehash: 7c3e0e6ec7331e860c9153e2a2e29a51fb5848bd
ms.sourcegitcommit: e4f3e1b2d08a02b9920e78e802234e5b674e7223
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 09/26/2018
ms.locfileid: "4213333"
---
# <a name="designing-for-sets"></a>Sets용 설계

> [!IMPORTANT]
> 이 문서에서는 아직 출시되지 않아 상업적으로 출시하기 전에 크게 수정될 수 있는 기능에 대해 설명합니다. Microsoft는 여기에 제공된 정보에 대해 명시적 또는 묵시적 보증을 하지 않습니다.

Windows Insider Preview를 시작으로 앱의 사용자는 Sets 기능을 사용할 수 있습니다. Sets를 사용하면 앱이 다른 앱과 공유할 수 있는 창에 놓아지고 제목 표시줄에 각 앱의 자체 탭이 표시됩니다.

이 문서에서는 Sets UI에서 최상의 환경을 제공하기 위해 앱을 최적화하려는 영역 위로 이동합니다.

> [!TIP]
> 설정에 대한 자세한 내용은 [Sets 소개](https://insider.windows.com/en-us/articles/introducing-sets/) 블로그 게시물 및 Microsoft 빌드 2018의 [Sets용 위한 개발](https://developer.microsoft.com/events/build/content/developing-for-sets-on-windows-10) 대화를 참조하십시오.

## <a name="customizing-tab-visuals"></a>탭 화면 사용자 지정

기본적으로 앱의 탭이 활성화되면 시스템은 이에 적절한 텍스트 및 아이콘 색상을 선택하려고 시도합니다. 이렇게 하면 사용자가 최소한의 노력만 하거나 Sets에 대한 최적화를 수행하지 않는 경우에도 앱의 탭이 잘 보입니다.

그러나 앱에 대한 탭 색상을 사용자 지정하는 것이 좋은 경우도 있을 수 있습니다. 이 섹션에서는 기본 탭 동작을 설명하고 앱을 수정해야 하는 경우와 수정해서는 안 되는 경우에 대해 설명합니다.

### <a name="tab-states"></a>탭 상태

앱이 Set에 있으면 해당 탭은 선택 및 활성, 선택 및 비활성, 또는 미선택 및 비활성 중 하나의 상태일 수 있습니다.

- **선택-활성**: 그룹화된 창의 세트 내에서 선택되어 있고 활성 전경 창에 있는 탭입니다.

    (이 문서에서 _활성_ 탭 수정에 대한 언급 시 탭이 선택-활성임을 의미합니다.)
- **선택-비활성**: 그룹화된 창의 세트 내에서 선택되어 있지만 활성 전경 창에 있지 않은 탭입니다.
- **미선택-비활성**: 그룹화된 창 세트 내에서 선택하지 않은 탭입니다.

비활성 탭의 색상은 시스템에 의해 시스템 테마를 기반으로 업데이트 및 유지됩니다. 앱에서 이에 영향을 줄 방법은 없습니다.

기본적으로 선택하여 활성인 탭은 Windows 설정에서 사용자가 지정한 시스템 테마 색을 준수합니다. 탭이 활성화되어 있는 경우에만 앱에 대한 탭 색상을 사용자 지정할 수 있습니다.

![Sets의 탭 상태](images/sets-tab-states.jpg)

### <a name="coloring-of-active-tabs"></a>활성 탭의 색상

활성 탭의 색은 사용자가 앱에서 설정한 값 또는 시스템 설정에 따라 결정됩니다. 앱이 활성 상태일 때 사용되는 탭 색상은 다음과 같이 결정됩니다.

- 앱에서 탭 색상을 지정하면 이 색상이 가장 높은 우선 순위를 갖습니다. 앱에서 사용자가 지정한 탭 색상은 시스템 설정과 관계없이 앱이 활성화되면 사용됩니다.
- 그렇지 않은 경우, 사용자가 Windows 설정에서 옵션을 선택하여 제목 표시줄에 테마 컬러를 표시하는 경우 시스템 테마 컬러가 사용됩니다.
  - 이 설정은 Windows 설정 앱에서 개인 설정 > 색상 > 테마 컬러 표시 아래에서 제목 표시줄에서 찾을 수 있습니다.
- 마지막으로, 앱 또는 사용자 설정을 적용하지 않으면 탭은 현재 시스템 테마 색을 사용합니다.

### <a name="considerations-when-you-modify-tab-colors"></a>탭 색상 수정 시 고려 사항

여기에서는 앱에 대한 탭 색상을 수정해야 하는 경우와 이러한 경우 고려해야 할 사항을 설명합니다. 또한 탭 색상을 수정하지 않고 시스템이 관리하도록 하는 것이 좋은 경우를 알아봅니다.

#### <a name="match-your-brand-colors"></a>브랜드 색 일치

일반적으로 탭 색을 수정할지 여부를 결정하는 재정의 요소는 브랜드 색에 맞추는 것이 좋습니다. 브랜드 색에 맞게 앱 탭을 수정하는 경우 이 섹션에 설명된 앱 레이아웃 또는 다른 시스템 테마 색 등의 기타 고려 사항에서의 색 모양을 테스트해야 합니다.

#### <a name="horizontal-layout"></a>가로 레이아웃

앱 레이아웃이 위쪽에서 가로로 흐르는 단색(비-아크릴) 밴드를 포함하는 경우 일반적으로 일치하는 색을 사용하여 해당 탭과 앱을 연결하기 위해 제대로 작동합니다. 앱의 맨 위에 사용되는 색에 연속성을 제공하여 탭에 대해 앱과 일관적인 느낌을 주는 단색을 선택합니다.

#### <a name="horizontal-layout-with-acrylic"></a>아크릴 소재 가로 레이아웃

앱이 위쪽에서 가로로 흐르는 아크릴 소재의 밴드를 사용하는 경우 시스템이 탭 색을 결정하도록 설정하는 것이 좋습니다.

또한 이 밴드 뒤의 응용 프로그램 또는 데스크톱을 통해 표시하는 밴드 효과를 만들기보다 응용 프로그램 배경이 이 영역에 흐르도록 하기 위해 배경 아크릴을 사용하기 보다 여기에서 앱 내 아크릴을 사용하는 것이 좋습니다.

이 경우 사용자가 이러한 탭을 설정하여 테마 컬러를 사용하여 밝게/어둡게 테마 또는 테마 컬러를 표시할 수 있습니다.

#### <a name="vertical-layout"></a>세로 레이아웃

앱 레이아웃에 세로로 흐르는 단색의 세로 창이 포함된 경우, 탭 색상을 사용자 지정하지 않는 것이 좋습니다. 앱 위의 탭 위치는 사용자가 언제든지 변경할 수 있으므로 앱과 탭 상단 부분 간에 색 연속성을 기대할 수 없습니다. 시스템은 그림자와 같은 다른 시각 신호를 사용하여 탭을 앱과 연결합니다.

### <a name="how-to-modify-tab-colors"></a>탭 색을 수정하는 방법

활성 탭 색은 기존 제목 표시줄 사용자 지정 API를 사용합니다. 이미 앱에 대한 제목 표시줄 색을 사용자 지정한 경우 앱이 Set에 있을 때 변경 내용이 앱 탭에 적용됩니다.

탭 색을 수정하려면 [ApplicationViewTitleBar](https://docs.microsoft.com/uwp/api/windows.ui.viewmanagement.applicationviewtitlebar) 속성을 설정하여 다음을 지정합니다.

- 탭에 대한 단색 배경색.
- 탭 텍스트에 대한 단색 전경색.

이 예제에서는 ApplicationViewTitleBar의 인스턴스를 가져와서 색상 속성을 설정하는 방법을 보여 줍니다.

```csharp
// using Windows.UI.ViewManagement;
var titleBar = ApplicationView.GetForCurrentView().TitleBar;

// Set active window colors
titleBar.ForegroundColor = Windows.UI.Colors.White;
titleBar.BackgroundColor = Windows.UI.Colors.Green;
```

자세한 내용은 [제목 표시줄 사용자 지정](title-bar.md#simple-color-customization) 문서의 _간단한 색 사용자 지정_ 의 섹션과 [제목 표시줄 사용자 지정 샘플](http://go.microsoft.com/fwlink/p/?LinkId=620613)을 참조합니다.

### <a name="considerations-for-full-title-bar-customization"></a>전체 제목 표시줄 사용자 지정에 대한 고려 사항

[제목 표시줄 사용자 지정](title-bar.md#full-customization) 문서의 _전체 사용자 지정_ 섹션에 설명된 대로 완전히 앱의 제목 표시줄을 사용자 지정할 수 있습니다. 일반적으로 [아크릴을 제목 표시줄로 확장](../style/acrylic.md#extend-acrylic-into-the-title-bar)하거나 제목 표시줄에 사용자 지정 콘텐츠를 배치하기 위해 이를 수행합니다. 이 작업을 수행하는 경우 전체 화면 및 태블릿 모드에 대한 지침에 따르고 [CoreApplicationViewTitleBar.IsVisible](/uwp/api/windows.applicationmodel.core.coreapplicationviewtitlebar.isvisible)이 **true**일 때만 사용자 지정 제목 표시줄 콘텐츠를 표시해야 합니다.

앱이 Set에서 실행되면 CoreApplicationViewTitleBar.IsVisible은 **false**이며, 제목 표시줄 콘텐츠는 표시하지 않아야 합니다. 그러나 사용자 지정 제목 표시줄 콘텐츠를 숨기라는 이 지침을 따르지 않는 경우 제목 표시줄 영역이 아닌 앱의 탭 아래에 표시됩니다.

콘텐츠 또는 기능을 사용자 지정 제목 표시줄 UI에 배치한 경우 앱의 다른 UI 화면에서 사용할 수 있도록 만드는 것이 좋습니다.

### <a name="how-to-modify-the-tab-icon"></a>탭 아이콘을 수정하는 방법

Set에서 앱 아이콘이 최상의 모습인지 확인하려면 앱에 대한 판이 없는 대체 아이콘을 제공해야 합니다. (앱의 탭에 사용되는 앱 아이콘은 작업 표시줄에서 사용되는 것과 동일한 아이콘입니다.) 대체 아이콘의 목적은 배경색에 비교해 모습을 확인하는 것입니다. 사용 가능한 경우 대체 아이콘이 사용됩니다.

앱 매니페스트에서 일반 아이콘뿐 아니라 판이 없는 대체 양식 아이콘을 지정합니다. 자세한 내용은 [앱 아이콘 및 로고를](/windows/uwp/design/style/app-icons-and-logos)참조 하세요. 지정할 아이콘은 문서의 [앱 아이콘 자산에 대 한](/windows/uwp/design/style/app-icons-and-logos#more-about-app-icon-assets) 섹션에서 "판이 없는 대상 크기 목록 자산"으로 설명 됩니다.

앱 매니페스트에 대체 아이콘을 지정하지 않으면 시스템은 탭 색을 사용하여 타일 아이콘에 다시 판을 배치하고 이를 사용합니다.

![Sets에서 사용되는 아이콘](images/sets-icons.png)

> 동일한 아이콘이 작업 표시줄 및 앱 탭에서 사용됩니다.

## <a name="restore-previous-sets-with-user-activities"></a>사용자 활동으로 이전 Sets 복원

Sets의 혜택은 사용자가 앱을 시작하거나 문서를 열 때 앱 및 웹 콘텐츠에 대해 이전에 연 탭을 복원할 수 있다는 점입니다. (자세한 내용은 [Sets 소개](https://insider.windows.com/en-us/articles/introducing-sets/) 블로그 게시물의 동영상을 참조하십시오.) 이는 _사용자 활동_을 통해 활성화됩니다.

기본적으로 시스템이 앱을 대신해 사용자 활동을 만들며, 이를 통해 사용자가 앱을 시작하거나 문서를 열 때 앱이 탭으로 복원됩니다. 그러나 시스템에서 만든 기본 사용자 활동은 기본 상태에서 앱을 시작하기만 할 수 있습니다. Set의 일부로 이전 상태로 앱을 복원할 수 없습니다.

사용자 지정 사용자 활동을 제공하여 Sets에 대해 앱을 최적화할 수 있습니다. 사용자가 제공한 사용자 활동은 앱에 연결되어 복원되는 마지막으로 Set의 일부였던 상태로 이를 복원합니다.

사용자 지정 사용자 활동을 제공하려면:

- 운영 체제가 시작한 [UserActivityRequest](https://docs.microsoft.com/uwp/api/windows.applicationmodel.useractivities.useractivityrequest)에 적절한 [UserActivity](https://docs.microsoft.com/uwp/api/windows.applicationmodel.useractivities.useractivity)로 응답합니다.
- UserActivity는 시스템이 특정 컨텍스트로 앱을 시작하기 위해 사용하는 활성화 딥 링크 URI를 포함합니다.

자세한 내용은 [UserActivityRequestManager.UserActivityRequested 이벤트](https://docs.microsoft.com/uwp/api/windows.applicationmodel.useractivities.useractivityrequestmanager.useractivityrequested), [URI 활성화 처리](https://docs.microsoft.com/windows/uwp/launch-resume/handle-uri-activation) 및 [장치 간 사용자 활동 계속 수행](https://docs.microsoft.com/windows/uwp/launch-resume/useractivities)를 참조합니다.

## <a name="enable-multi-instance-for-uwp-apps"></a>UWP 앱에 대해 다중 인스턴스를 사용하도록 설정

Windows 10 버전 1803을 시작으로, UWP 앱은 다중 인스턴스를 지원합니다. UWP는 기본적으로 여전히 단일 인스턴스이며 다중 인스턴스를 적용하려는 각 앱을 명시적으로 선택해야 합니다.

앱 다중 인스턴스를 설정하면 한 번에 둘 이상의 Set에서 앱을 실행하는 사용자가 혜택을 얻을 수 있습니다. 단일 인스턴스 앱은 한 번에 하나의 Set에서만 실행할 수 있습니다.

UWP 앱에 대해 다중 인스턴스를 사용하는 방법에 대한 자세한 내용은 [다중 인스턴스 유니버설 Windows 앱 만들기](https://docs.microsoft.com/windows/uwp/launch-resume/multi-instance-uwp)를 참조합니다.

## <a name="use-an-in-app-back-button"></a>앱 내 뒤로 단추 사용

앱에서 뒤로 탐색을 구현하려면 [뒤로 단추 지침](../basics/navigation-history-and-backwards-navigation.md)에 의해 앱의 UI의 뒤로 단추를 배치하는 것이 좋습니다. 앱이 NavigationView 컨트롤을 사용하는 경우 NavigationView의 기본 뒤로 단추를 사용해야 합니다.

앱에서 시스템 뒤로 단추를 사용하는 경우 이를 앱 내 뒤로 단추로 교체해야 합니다. 이렇게 하면 사용자가 앱이 Set에서 실행되든 실행되지 않든 일관적인 뒤로 단추 환경을 유지할 수 있으며 뒤로 단추 시각 효과도 여러 앱에서 일관되게 유지됩니다.

앱 내 뒤로 단추 통합에 대한 자세한 내용은 [탐색 기록 및 뒤로 탐색](../basics/navigation-history-and-backwards-navigation.md)을 참조합니다.

### <a name="support-for-the-system-back-button-in-sets"></a>Sets의 시스템 뒤로 단추에 대한 지원

앱이 앱 내 단추가 아닌 시스템 뒤로 단추를 사용하는 경우 시스템 UI는 이전 버전과 호환성을 유지하기 위해 여전히 시스템 뒤로 단추를 렌더링합니다.

- 앱이 Set에 없는 경우 제목 표시줄 내에 뒤로 단추가 렌더링됩니다. 뒤로 단추에 대한 시각적 환경과 사용자 상호 작용은 변경되지 않습니다.
- 앱이 Set에 있는 경우 시스템 뒤로 표시줄 내에 뒤로 단추가 렌더링됩니다.

시스템 뒤로 표시줄은 탭 밴드와 앱의 콘텐츠 영역 사이에 삽입되는 "밴드"입니다. 밴드는 앱의 가로를 따라 흐르며 왼쪽 가장자리에 뒤로 단추가 표시됩니다. 밴드는 뒤로 단추에 대한 적절한 터치 대상 크기만큼 큰 세로 높이를 가집니다.

![Sets의 시스템 뒤로 표시줄](images/sets-system-back-bar.png)

> 시스템 뒤로 표시줄이 앱에 표시됩니다.

시스템 뒤로 표시줄은 뒤로 단추 표시 여부에 따라 동적으로 나타납니다. 뒤로 단추가 표시된 경우, 시스템 뒤로 표시줄이 삽입되며 앱 콘텐츠가 탭 밴드 아래로 이동됩니다. 뒤로 단추 숨겨진 경우, 시스템 뒤로 표시줄이 동적으로 제거되며 앱 콘텐츠가 탭 밴드로 이동됩니다.

앱의 UI가 위나 아래로 이동하지 않도록 하려면 시스템 뒤로 단추 대신 앱 내 뒤로 단추를 사용하는 것이 좋습니다.

앱 탭 및 시스템 뒤로 표시줄 모두 제목 표시줄 사용자 지정을 적용할 수 있습니다. ApplicationViewTitleBar API를 사용하여 배경색 및 전경색 속성을 지정하면 해당 색상이 탭 및 시스템 뒤로 표시줄에 적용됩니다.

## <a name="related-articles"></a>관련 문서

- [제목 표시줄 사용자 지정](title-bar.md)
- [탐색 기록 및 뒤로 탐색](../basics/navigation-history-and-backwards-navigation.md)
- [색상](../style/color.md)
