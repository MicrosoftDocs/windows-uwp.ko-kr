---
Description: 기본 액세스 가능성 정보는 종종 이름, 역할 및 값으로 분류 됩니다. 이 항목에서는 앱이 보조 기술에 필요한 기본 정보를 노출 하는 데 도움이 되는 코드에 대해 설명 합니다.
ms.assetid: 9641C926-68C9-4842-8B55-C38C39A9E5C5
title: 기본적인 접근성 정보 표시
label: Expose basic accessibility information
template: detail.hbs
ms.date: 09/24/2020
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: da0ad6c0121f81a4854728f4441e0407a6302f54
ms.sourcegitcommit: eda7bbe9caa9d61126e11f0f1a98b12183df794d
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 09/24/2020
ms.locfileid: "91217466"
---
# <a name="expose-basic-accessibility-information"></a>기본적인 접근성 정보 표시  



기본 액세스 가능성 정보는 종종 이름, 역할 및 값으로 분류 됩니다. 이 항목에서는 앱이 보조 기술에 필요한 기본 정보를 노출 하는 데 도움이 되는 코드에 대해 설명 합니다.

<span id="accessible_name"/>
<span id="ACCESSIBLE_NAME"/>

## <a name="accessible-name"></a>액세스 가능한 이름  
액세스 가능한 이름은 화면 읽기 프로그램에서 UI 요소를 알리기 위해 사용 하는 간단한 설명 텍스트 문자열입니다. 콘텐츠를 이해 하거나 UI와 상호 작용 하는 데 중요 한 의미를 갖도록 UI 요소에 액세스할 수 있는 이름을 설정 합니다. 이러한 요소에는 일반적으로 이미지, 입력 필드, 단추, 컨트롤 및 지역이 포함 됩니다.

이 표에서는 XAML UI에서 다양 한 형식의 요소에 대해 액세스 가능한 이름을 정의 하거나 가져오는 방법을 설명 합니다.

| 요소 형식 | 설명 |
|--------------|-------------|
| 정적 텍스트 | [**TextBlock**](/uwp/api/Windows.UI.Xaml.Controls.TextBlock) 및 [**RichTextBlock**](/uwp/api/Windows.UI.Xaml.Controls.RichTextBlock) 요소의 경우 액세스 가능한 이름이 표시 (내부) 텍스트에서 자동으로 결정 됩니다. 해당 요소의 모든 텍스트가 이름으로 사용 됩니다. [내부 텍스트에서 이름을](#name_from_inner_text)참조 하세요. |
| 이미지 | XAML [**이미지**](/uwp/api/Windows.UI.Xaml.Controls.Image) 요소는 **img** 및 유사한 요소의 HTML **alt** 특성에 대 한 직접적인 아날로그를 포함 하지 않습니다. [**AutomationProperties.Name**](/dotnet/api/system.windows.automation.automationproperties.name) 를 사용 하 여 이름을 제공 하거나 캡션 기술을 사용 합니다. [이미지에 액세스할 수 있는 이름을](#images)참조 하세요. |
| 양식 요소 | Form 요소에 대해 액세스할 수 있는 이름은 해당 요소에 대해 표시 되는 레이블과 동일 해야 합니다. [레이블 및 있으면 labeledby](#labels)를 참조 하세요. |
| 단추 및 링크 | 기본적으로 액세스할 수 있는 단추 또는 링크의 이름은 표시 되는 텍스트를 기반으로 하며, [내부 텍스트에서 이름](#name_from_inner_text)에 설명 된 것과 동일한 규칙을 사용 합니다. 단추만 이미지를 포함 하는 경우 [**AutomationProperties.Name**](/dotnet/api/system.windows.automation.automationproperties.name) 를 사용 하 여 단추의 의도 된 작업에 해당 하는 텍스트 전용 텍스트를 제공 합니다. |

패널과 같은 대부분의 컨테이너 요소는 해당 콘텐츠를 액세스 가능한 이름으로 승격 하지 않습니다. 이는 해당 컨테이너가 아니라 이름과 해당 역할을 보고 해야 하는 항목 콘텐츠 이기 때문입니다. 컨테이너 요소는 보조 기술 논리가이를 트래버스할 수 있도록 Microsoft UI 자동화 표현의 자식이 있는 요소 임을 보고할 수 있습니다. 그러나 보조 기술 사용자는 일반적으로 컨테이너에 대해 알 필요가 없으므로 대부분의 컨테이너에 이름이 지정하지 않습니다.

<span id="role_value"/>
<span id="ROLE_VALUE"/>

## <a name="role-and-value"></a>역할 및 값  
XAML 어휘에 속하는 컨트롤 및 기타 UI 요소는 해당 정의의 일부로 보고 역할 및 값에 대 한 UI 자동화 지원을 구현 합니다. UI 자동화 도구를 사용 하 여 컨트롤에 대 한 역할 및 값 정보를 검사 하거나 각 컨트롤의 [**Automationpeer**](/uwp/api/Windows.UI.Xaml.Automation.Peers.AutomationPeer) 구현에 대 한 설명서를 읽을 수 있습니다. UI 자동화 프레임 워크에서 사용 가능한 역할은 [**AutomationControlType**](/uwp/api/Windows.UI.Xaml.Automation.Peers.AutomationControlType) 열거형에 정의 되어 있습니다. 보조 기술과 같은 UI 자동화 클라이언트는 UI 자동화 프레임 워크가 컨트롤의 **Automationpeer**를 사용 하 여 노출 하는 메서드를 호출 하 여 역할 정보를 얻을 수 있습니다.

일부 컨트롤에는 값이 없습니다. 값이 있는 컨트롤은이 정보를 해당 컨트롤에서 지 원하는 피어 및 패턴을 통해 UI 자동화에 보고 합니다. 예를 들어 [**TextBox**](/uwp/api/Windows.UI.Xaml.Controls.TextBox) 폼 요소에는 값이 있습니다. 보조 기술은 UI 자동화 클라이언트 일 수 있으며, 값이 존재 하는 값을 모두 검색할 수 있습니다. 이 경우 **텍스트 상자** 는 [**TextBoxAutomationPeer**](/uwp/api/Windows.UI.Xaml.Automation.Peers.TextBoxAutomationPeer) 정의를 통해 [**ivalueprovider**](/uwp/api/Windows.UI.Xaml.Automation.Provider.IValueProvider) 패턴을 지원 합니다.

> [!NOTE]
> [**AutomationProperties.Name**](/dotnet/api/system.windows.automation.automationproperties.name) 또는 기타 기술을 사용 하 여 액세스 가능한 이름을 명시적으로 제공 하는 경우 컨트롤 역할에 사용 되는 것과 동일한 텍스트를 포함 하거나 액세스 가능한 이름에 정보를 입력 하지 마십시오. 예를 들어 이름에 "button" 또는 "list"와 같은 문자열은 포함 하지 않습니다. 역할 및 형식 정보는 UI 자동화에 대 한 기본 컨트롤 지원에서 제공 하는 다른 UI 자동화 속성 (**LocalizedControlType**)에서 제공 됩니다. 많은 보조 기술에서 액세스 가능한 이름에 **LocalizedControlType** 를 추가 하므로 액세스 가능한 이름의 역할을 복제 하면 불필요 하 게 반복 되는 단어가 발생할 수 있습니다. 예를 들어 "button"의 액세스 가능한 이름을 [**단추**](/uwp/api/Windows.UI.Xaml.Controls.Button) 컨트롤에 지정 하거나 이름의 마지막 부분으로 "button"을 포함 하는 경우 화면 판독기에서 "단추 단추"로 읽을 수 있습니다. 내레이터를 사용 하 여 내게 필요한 옵션 정보의이 측면을 테스트 해야 합니다.

<span id="Influencing_the_UI_Automation_tree_views"/>
<span id="influencing_the_ui_automation_tree_views"/>
<span id="INFLUENCING_THE_UI_AUTOMATION_TREE_VIEWS"/>

## <a name="influencing-the-ui-automation-tree-views"></a>UI 자동화 트리 보기에 영향  
Ui 자동화 프레임 워크에는 ui 자동화 클라이언트가 세 개의 가능한 뷰 (raw, 컨트롤 및 내용)를 사용 하 여 UI의 요소 간 관계를 검색할 수 있는 트리 보기의 개념이 있습니다. 컨트롤 뷰는 ui 자동화 클라이언트에서 대화형 UI의 요소에 대 한 적절 한 표현과 조직을 제공 하기 때문에 자주 사용 하는 뷰입니다. 일반적으로 테스트 도구를 사용 하면 도구에서 요소 조직을 표시할 때 사용할 트리 뷰를 선택할 수 있습니다.

UI 자동화 프레임 워크가 Windows 앱에 대 한 UI를 나타내는 경우 기본적으로 컨트롤 파생 클래스와 몇 가지 다른 [**요소가 컨트롤 뷰에**](/uwp/api/Windows.UI.Xaml.Controls.Control) 표시 됩니다. 하지만 경우에 따라 요소가 정보를 복제 하거나 접근성 시나리오에서 중요 하지 않은 정보를 제공 하는 UI 컴퍼지션 때문에 컨트롤이 컨트롤 뷰에 표시 되는 것을 원하지 않을 수 있습니다. 연결 된 속성 [**Automationproperties**](/uwp/api/windows.ui.xaml.automation.automationproperties.accessibilityviewproperty) 를 사용 하 여 요소가 트리 뷰에 노출 되는 방식을 변경 합니다. 요소를 **원시** 트리에 배치 하면 대부분의 보조 기술에서 해당 요소를 뷰의 일부로 보고 하지 않습니다. 기존 컨트롤에서 작동 하는 방법에 대 한 몇 가지 예제를 보려면 텍스트 편집기에서 일반 .xaml 디자인 참조 XAML 파일을 열고 템플릿에서 **Automationproperties** 를 검색 합니다.

<span id="name_from_inner_text"/>
<span id="NAME_FROM_INNER_TEXT"/>

## <a name="name-from-inner-text"></a>내부 텍스트의 이름  
액세스 가능한 이름 값에 대해 표시 되는 UI에 이미 존재 하는 문자열을 더 쉽게 사용할 수 있도록 하기 위해 많은 컨트롤 및 기타 UI 요소는 요소 내의 내부 텍스트 또는 콘텐츠 속성의 문자열 값을 기반으로 하는 기본 액세스 가능 이름을 자동으로 결정 하는 기능을 제공 합니다.

* [**TextBlock**](/uwp/api/Windows.UI.Xaml.Controls.TextBlock), [**RichTextBlock**](/uwp/api/Windows.UI.Xaml.Controls.RichTextBlock), [**TextBox**](/uwp/api/Windows.UI.Xaml.Controls.TextBox) 및 **RichTextBlock** 는 각각 **Text** 속성 값을 기본 액세스 가능한 이름으로 승격 합니다.
* 모든 [**ContentControl**](/uwp/api/windows.ui.xaml.controls.contentcontrol.content) 하위 클래스는 반복적인 "ToString" 기술을 사용 하 여 해당 [**콘텐츠**](/uwp/api/windows.ui.xaml.controls.contentcontrol.content) 값에서 문자열을 찾고 이러한 문자열을 액세스 가능한 기본 이름으로 승격 합니다.

> [!NOTE]
> UI 자동화에 의해 적용 되는 경우 액세스 가능한 이름 길이는 2048 자를 초과할 수 없습니다. 자동으로 액세스 가능한 이름 결정에 사용 되는 문자열이이 제한을 초과 하면 액세스 가능한 이름이 해당 지점에서 잘립니다.

<span id="images"/>
<span id="IMAGES"/>

## <a name="accessible-names-for-images"></a>이미지에 대 한 액세스 가능한 이름
화면 읽기 프로그램을 지원 하 고 UI의 각 요소에 대 한 기본적인 식별 정보를 제공 하기 위해 이미지 및 차트와 같은 텍스트가 아닌 정보에 대 한 텍스트 대체를 제공 해야 하는 경우가 있습니다 (순수한 장식용 또는 구조 요소 제외). 이러한 요소에는 내부 텍스트가 없으므로 액세스할 수 있는 이름에는 계산 된 값이 없습니다. 이 예제에 표시 된 대로 [**AutomationProperties.Name**](/dotnet/api/system.windows.automation.automationproperties.name) 연결 된 속성을 설정 하 여 액세스 가능한 이름을 직접 설정할 수 있습니다.

XAML
```xml
<!-- Comment -->
<Image Source="product.png"
  AutomationProperties.Name="An image of a customer using the product."/>
```

또는 표시 되는 UI에 표시 되 고 이미지 콘텐츠에 대 한 레이블 관련 액세스 가능성 정보로도 사용 되는 텍스트 캡션을 포함 하는 것이 좋습니다. 예를 들면 다음과 같습니다.

XAML
```xml
<Image HorizontalAlignment="Left" Width="480" x:Name="img_MyPix"
  Source="snoqualmie-NF.jpg"
  AutomationProperties.LabeledBy="{Binding ElementName=caption_MyPix}"/>
<TextBlock x:Name="caption_MyPix">Mount Snoqualmie Skiing</TextBlock>
```

<span id="labels"/>
<span id="LABELS"/>

## <a name="labels-and-labeledby"></a>레이블 및 있으면 labeledby  
레이블을 폼 요소와 연결 하는 기본 방법은 레이블 텍스트에 **x:Name** 을 사용 하 여 [**textblock**](/uwp/api/Windows.UI.Xaml.Controls.TextBlock) 을 사용한 다음 form 요소에 [**있으면 labeledby**](/previous-versions/windows/silverlight/dotnet-windows-silverlight/ms591292(v=vs.95)) 연결 된 속성을 설정 하 여 XAML 이름으로 레이블 지정 **TextBlock** 을 참조 하는 것입니다. 이 패턴을 사용 하는 경우 사용자가 레이블을 클릭 하면 포커스가 연결 된 컨트롤로 이동 하 고 보조 기술에서는 레이블 텍스트를 양식 필드에 액세스할 수 있는 이름으로 사용할 수 있습니다. 이 기술을 보여 주는 예제는 다음과 같습니다.

XAML
```xml
<StackPanel x:Name="LayoutRoot" Background="White">
   <StackPanel Orientation="Horizontal">
     <TextBlock Name="lbl_FirstName">First name</TextBlock>
     <TextBox
      AutomationProperties.LabeledBy="{Binding ElementName=lbl_FirstName}"
      Name="tbFirstName" Width="100"/>
   </StackPanel>
   <StackPanel Orientation="Horizontal">
     <TextBlock Name="lbl_LastName">Last name</TextBlock>
     <TextBox
      AutomationProperties.LabeledBy="{Binding ElementName=lbl_LastName}"
      Name="tbLastName" Width="100"/>
   </StackPanel>
 </StackPanel>
```

<span id="accessible_description"/>
<span id="ACCESSIBLE_DESCRIPTION"/>

## <a name="accessible-description-optional"></a>액세스 가능한 설명 (선택 사항)  
액세스 가능한 설명은 특정 UI 요소에 대 한 내게 필요한 옵션 정보를 추가로 제공 합니다. 일반적으로 액세스 가능한 이름 만으로는 요소의 용도를 적절 하 게 전달 하지 않는 경우 액세스할 수 있는 설명을 제공 합니다.

내레이터 화면 판독기는 사용자가 CapsLock + F를 눌러 요소에 대 한 자세한 정보를 요청 하는 경우에만 요소의 액세스 가능한 설명을 읽습니다.

액세스 가능한 이름은 동작을 완벽 하 게 문서화 하는 것이 아니라 컨트롤을 식별 하기 위한 것입니다. 간단한 설명으로는 컨트롤을 설명 하는 데 충분 하지 않은 경우 [**AutomationProperties.Name**](/dotnet/api/system.windows.automation.automationproperties.name)외에 [**HelpText**](/dotnet/api/system.windows.automation.automationproperties.helptext) 연결 된 속성을 설정할 수 있습니다.

<span id="Testing_accessibility_early_and_often"/>
<span id="testing_accessibility_early_and_often"/>
<span id="TESTING_ACCESSIBILITY_EARLY_AND_OFTEN"/>

## <a name="testing-accessibility-early-and-often"></a>조기에 자주 액세스 가능성 테스트  
궁극적으로 화면 판독기를 지원 하기 위한 가장 좋은 방법은 화면 판독기를 사용 하 여 앱을 테스트 하는 것입니다. 그러면 화면 판독기가 동작 하는 방식과 앱에서 누락 될 수 있는 기본 접근성 정보가 표시 됩니다. 그에 맞게 UI 또는 UI 자동화 속성을 조정할 수 있습니다. 자세한 내용은 [내게 필요한 옵션 테스트](accessibility-testing.md)를 참조 하십시오.

액세스 가능성 테스트에 사용할 수 있는 도구 중 하나를 **Accscope**라고 합니다. 사용자가 응용 프로그램을 자동화 트리로 볼 수 있는 방법을 나타내는 UI의 시각적 표시를 볼 수 있기 때문에 **Accscope** 도구는 특히 유용 합니다. 특히 내레이터가 앱에서 텍스트를 가져오는 방법 및 UI에서 요소를 구성 하는 방법에 대 한 보기를 제공 하는 내레이터 모드가 있습니다. AccScope는 예비 디자인 단계 에서도 사용할 수 있고 앱에 대 한 개발 주기 전체에서 유용 하 게 사용할 수 있도록 설계 되었습니다. 자세한 내용은 [Accscope](/windows/desktop/WinAuto/accscope)를 참조 하세요.

<span id="Accessible_names_from_dynamic_data"/>
<span id="accessible_names_from_dynamic_data"/>
<span id="ACCESSIBLE_NAMES_FROM_DYNAMIC_DATA"/>

## <a name="accessible-names-from-dynamic-data"></a>동적 데이터에서 액세스할 수 있는 이름  
Windows에서는 *데이터 바인딩*이라는 기능을 통해 연결 된 데이터 소스에서 가져온 값을 표시 하는 데 사용할 수 있는 많은 컨트롤을 지원 합니다. 목록을 데이터 항목으로 채우면 초기 목록이 채워진 후 데이터 바인딩된 목록 항목에 액세스할 수 있는 이름을 설정 하는 기술을 사용 해야 할 수 있습니다. 자세한 내용은 [XAML 접근성 샘플](https://github.com/microsoftarchive/msdn-code-gallery-microsoft/tree/411c271e537727d737a53fa2cbe99eaecac00cc0/Official%20Windows%20Platform%20Sample/XAML%20accessibility%20sample)에서 "Scenario 4"를 참조 하십시오.

<span id="Accessible_names_and_localization"/>
<span id="accessible_names_and_localization"/>
<span id="ACCESSIBLE_NAMES_AND_LOCALIZATION"/>

## <a name="accessible-names-and-localization"></a>액세스 가능한 이름 및 지역화  
액세스 가능한 이름이 지역화 된 요소 이기도 한지 확인 하려면 지역화할 수 있는 문자열을 리소스로 저장 한 다음 [x:Uid 지시문](../../xaml-platform/x-uid-directive.md) 값을 사용 하 여 리소스 연결을 참조 하는 데 올바른 기술을 사용 해야 합니다. 액세스할 수 있는 이름이 명시적으로 설정 된 [**AutomationProperties.Name**](/dotnet/api/system.windows.automation.automationproperties.name) 사용에서 오는 경우에도 지역화할 수 있는 문자열이 있는지 확인 합니다.

[**Automationproperties**](/uwp/api/Windows.UI.Xaml.Automation.AutomationProperties) 속성과 같은 연결 된 속성은 리소스 이름에 대해 특별 한 한정 구문을 사용 하므로 리소스가 특정 요소에 적용 되는 연결 된 속성을 참조할 수 있습니다. 예를 들어, 이라는 UI 요소에 적용 되는 [**AutomationProperties.Name**](/dotnet/api/system.windows.automation.automationproperties.name) 의 리소스 이름은 `MediumButton` 입니다. `MediumButton.[using:Windows.UI.Xaml.Automation]AutomationProperties.Name`

<span id="related_topics"/>

## <a name="related-topics"></a>관련 항목  
* [접근성](accessibility.md)
* [**AutomationProperties.Name**](/dotnet/api/system.windows.automation.automationproperties.name)
* [XAML 접근성 샘플](https://github.com/microsoftarchive/msdn-code-gallery-microsoft/tree/411c271e537727d737a53fa2cbe99eaecac00cc0/Official%20Windows%20Platform%20Sample/XAML%20accessibility%20sample)
* [접근성 테스트](accessibility-testing.md)
