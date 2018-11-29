---
Description: Basic accessibility info is often categorized into name, role, and value. This topic describes code to help your app expose the basic information that assistive technologies need.
ms.assetid: 9641C926-68C9-4842-8B55-C38C39A9E5C5
title: 기본적인 접근성 정보 표시
label: Expose basic accessibility information
template: detail.hbs
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 8e7526ec4f32f641f152709e6968f3dc442c2a06
ms.sourcegitcommit: 89ff8ff88ef58f4fe6d3b1368fe94f62e59118ad
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/29/2018
ms.locfileid: "8214874"
---
# <a name="expose-basic-accessibility-information"></a>기본적인 접근성 정보 표시  



경우에 따라 기본 접근성 정보는 이름, 역할 및 값으로 분류됩니다. 이 항목에서는 보조 기술이 필요로 하는 기본 정보를 앱에 표시하는 데 도움이 되는 코드에 대해 설명합니다.

<span id="accessible_name"/>
<span id="ACCESSIBLE_NAME"/>

## <a name="accessible-name"></a>접근성 있는 이름  
접근성 있는 이름은 화면 읽기 프로그램이 UI 요소를 읽기 위해 사용하는 짧은 설명 텍스트 문자열입니다. 콘텐츠를 이해하거나 UI를 조작하는 데 있어 중요한 의미가 있는 UI 요소에 접근성 있는 이름을 설정합니다. 그러한 요소에는 일반적으로 이미지, 입력 필드, 단추, 컨트롤, 영역 등이 포함됩니다.

다음 표에서는 XAML UI의 다양한 요소 형식에 대해 접근성 있는 이름을 정의하거나 가져오는 방법에 대해 설명합니다.

| 요소 형식 | 설명 |
|--------------|-------------|
| 정적 텍스트 | [**TextBlock**](https://msdn.microsoft.com/library/windows/apps/BR209652) 및 [**RichTextBlock**](https://msdn.microsoft.com/library/windows/apps/BR227565) 요소의 경우 접근성 있는 이름이 표시되는(내부) 텍스트에 따라 자동으로 결정됩니다. 해당 요소의 모든 텍스트는 이름으로 사용됩니다. [내부 텍스트의 이름](#name_from_inner_text)을 참조하세요. |
| 이미지 | XAML [**Image**](https://msdn.microsoft.com/library/windows/apps/BR242752) 요소에는 **img** 및 유사한 요소의 HTML **alt** 특성과 완전히 유사한 특징이 없습니다. [**AutomationProperties.Name**](https://msdn.microsoft.com/library/windows/apps/Hh759770)을 사용하여 이름을 제공하거나 캡션 기술을 사용합니다. [이미지의 접근성 있는 이름](#images)을 참조하세요. |
| 양식 요 소 | 양식 요소의 접근성 있는 이름은 해당 요소에 대해 표시되는 레이블과 동일해야 합니다. [레이블 및 LabeledBy](#labels)를 참조하세요. |
| 단추 및 링크 | 기본적으로 단추 또는 링크의 접근성 있는 이름은 표시되는 텍스트를 기반으로 하며, [내부 텍스트의 이름](#name_from_inner_text)에 설명된 것과 동일한 규칙을 사용합니다. 단추에 이미지만 포함된 경우 [**AutomationProperties.Name**](https://msdn.microsoft.com/library/windows/apps/Hh759770)을 사용하여 단추의 의도된 동작 중 텍스트 전용 동작만 제공합니다. |

패널과 같은 대부분의 컨테이너 요소는 해당 콘텐츠를 접근성 있는 이름으로 승격시키지 않습니다. 이 콘텐츠는 컨테이너가 아닌 이름 및 해당 역할을 보고해야 하는 항목 콘텐츠이기 때문입니다. 컨테이너 요소는 보조 기술 논리에서 해당 요소를 트래버스할 수 있도록 Microsoft UI 자동화 표현에 자식이 있는 요소로 보고할 수 있습니다. 그러나 보조 기술 사용자는 일반적으로 컨테이너에 대해 알 필요가 없으므로 대부분의 컨테이너에 이름이 지정하지 않습니다.

<span id="role_value"/>
<span id="ROLE_VALUE"/>

## <a name="role-and-value"></a>역할 및 값  
XAML 용어의 일부인 컨트롤 및 기타 UI 요소는 해당 정의의 일부로 역할 및 값을 보고하기 위해 UI 자동화 지원을 구현합니다. UI 자동화 도구를 사용하여 컨트롤에 대한 역할 및 값 정보를 검사하거나 각 컨트롤의 [**AutomationPeer**](https://msdn.microsoft.com/library/windows/apps/BR209185) 구현에 대한 설명서를 읽어볼 수 있습니다. UI 자동화 프레임워크에서 사용 가능한 역할은 [**AutomationControlType**](https://msdn.microsoft.com/library/windows/apps/BR209182) 열거에 정의되어 있습니다. 보조 기술과 같은 UI 자동화 클라이언트는 UI 자동화 프레임워크가 컨트롤의 **AutomationPeer**를 사용하여 노출하는 메서드를 호출하여 역할 정보를 구할 수 있습니다.

일부 컨트롤에는 값이 없습니다. 값이 없는 컨트롤은 해당 컨트롤에서 지원하는 피어 및 패턴을 통해 UI 자동화에 이 정보를 보고합니다. 예를 들어 [**TextBox**](https://msdn.microsoft.com/library/windows/apps/BR209683) 양식 요소에는 값이 있습니다. 보조 기술이 UI 자동화 클라이언트가 될 수 있으므로 값이 존재하는지와 해당 값이 무엇인지를 검색할 수 있습니다. 이 특정한 경우에 **TextBox**는 [**TextBoxAutomationPeer**](https://msdn.microsoft.com/library/windows/apps/BR242550) 정의를 통해 [**IValueProvider**](https://msdn.microsoft.com/library/windows/apps/BR242663) 패턴을 지원합니다.

> [!NOTE]
> [**AutomationProperties.Name**](https://msdn.microsoft.com/library/windows/apps/Hh759770)이나 기타 기술을 사용하여 접근성 있는 이름을 명시적으로 제공하는 경우 컨트롤 역할이나 형식 정보에서 사용하는 것과 동일한 텍스트를 접근성 있는 이름에 포함하지 마세요. 예를 들어 "단추"나 "목록" 같은 문자열을 이름에 포함하지 마세요. 역할 및 형식 정보는 UI 자동화를 위한 기본 컨트롤 지원에서 제공하는 다양한 UI 자동화 속성(**LocalizedControlType**)에서 제공됩니다. 많은 보조 기술에서 **LocalizedControlType**을 접근성 있는 이름에 추가하므로 접근성 있는 이름에서 역할을 중복하면 단어가 불필요하게 반복될 수 있습니다. 예를 들어 [**Button**](https://msdn.microsoft.com/library/windows/apps/BR209265) 컨트롤에 액세스 가능한 "단추" 이름을 지정하거나 "단추"라는 단어를 이름의 마지막 부분으로 포함하는 경우 화면 읽기 프로그램에서는 이를 "단추 단추"로 읽을 수 있습니다. 내레이터를 사용하여 접근성 정보의 다음 측면을 테스트해야 합니다.

<span id="Influencing_the_UI_Automation_tree_views"/>
<span id="influencing_the_ui_automation_tree_views"/>
<span id="INFLUENCING_THE_UI_AUTOMATION_TREE_VIEWS"/>

## <a name="influencing-the-ui-automation-tree-views"></a>UI 자동화 트리 보기에 미치는 영향  
UI 자동화 프레임워크에는 UI 자동화 클라이언트가 원시, 컨트롤, 콘텐츠 등 세 가지 보기를 사용하여 UI에 있는 요소 사이의 관계를 검색할 수 있는 트리 보기 개념이 있습니다. 컨트롤 보기는 표현 기능이 좋고 UI 요소가 대화형으로 구성되어 있기 때문에 UI 자동화 클라이언트에서 자주 사용되는 보기입니다. 일반적으로 테스트 도구를 사용하면 도구에서 요소 구성을 표시할 때 사용할 트리 보기를 선택할 수 있습니다.

기본적으로 [**Control**](https://msdn.microsoft.com/library/windows/apps/BR209390) 파생 클래스 및 다수의 다른 요소는 UI 자동화 프레임워크가 UWP(유니버설 Windows 플랫폼) 앱의 UI를 표시할 때 컨트롤 보기에 나타납니다. 하지만 특정 요소가 정보를 복제하거나 접근성 시나리오에 중요하지 않은 정보를 표시하는 UI 컴퍼지션으로 인해 해당 요소를 컨트롤 보기에 표시하지 않으려는 경우가 가끔 있습니다. 연결된 속성 [**AutomationProperties.AccessibilityView**](https://msdn.microsoft.com/library/windows/apps/Dn251788)를 사용하면 요소가 트리 보기에 표시되는 방식을 변경할 수 있습니다. 요소를 **Raw** 트리에 넣으면 대부분의 보조 기술이 해당 요소를 보기의 일부로 보고하지 않습니다. 기존 컨트롤에서 작동하는 방식의 몇 가지 예를 보려면 generic.xaml 디자인 참조 XAML 파일을 텍스트 편집기에서 열고 템플릿에서 **AutomationProperties.AccessibilityView**를 검색합니다.

<span id="name_from_inner_text"/>
<span id="NAME_FROM_INNER_TEXT"/>

## <a name="name-from-inner-text"></a>내부 텍스트의 이름  
표시되는 UI에 이미 있는 문자열을 접근성 있는 이름 값에 사용하기가 더 쉬워지도록 하기 위해 대부분의 컨트롤 및 기타 UI 요소에서는 요소 내에 있는 내부 텍스트를 기반으로 또는 콘텐츠 속성의 문자열 값에서 기본 접근성 있는 이름을 자동으로 결정하는 기능을 지원합니다.

* [**TextBlock**](https://msdn.microsoft.com/library/windows/apps/BR209652), [**RichTextBlock**](https://msdn.microsoft.com/library/windows/apps/BR227565), [**TextBox**](https://msdn.microsoft.com/library/windows/apps/BR209683) 및 **RichTextBlock**은 각각 **Text** 속성의 값을 기본 접근성 있는 이름으로 승격시킵니다.
* 모든 [**ContentControl**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.contentcontrol.content) 하위 클래스는 반복적인 "ToString" 기술을 사용하여 [**Content**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.contentcontrol.content) 값에서 문자열을 찾고 이러한 문자열을 기본 접근성 있는 이름으로 승격시킵니다.

> [!NOTE]
> UI 자동화에 의해 적용되는 접근성 있는 이름 길이는 2048자보다 클 수 없습니다. 접근성 있는 이름 자동 결정에 사용된 문자열이 해당 제한을 초과하는 경우 접근성 있는 이름은 해당 지점에서 잘립니다.

<span id="images"/>
<span id="IMAGES"/>

## <a name="accessible-names-for-images"></a>이미지의 접근성 있는 이름
화면 읽기 프로그램을 지원하고 UI의 각 요소에 대한 기본 식별 정보를 제공하려면 경우에 따라 이미지 및 차트(완전한 장식 또는 구조 요소 제외)와 같이 텍스트가 아닌 정보 대신 텍스트를 제공해야 합니다. 이 요소에는 내부 텍스트가 없으므로, 액세스 가능한 이름에는 계산된 값이 없습니다. [**AutomationProperties.Name**](https://msdn.microsoft.com/library/windows/apps/Hh759770) 연결된 속성을 다음 예제와 같이 설정하면 액세스 가능한 이름을 직접 설정할 수 있습니다.

XAML
```xml
<!-- Comment -->
<Image Source="product.png"
  AutomationProperties.Name="An image of a customer using the product."/>
```

또는 표시되는 UI에 나타나고 이미지 콘텐츠에 대한 레이블 관련 접근성 정보로도 제공되는 텍스트 자막을 포함해 보세요. 예를 들면 다음과 같습니다.

XAML
```xml
<Image HorizontalAlignment="Left" Width="480" x:Name="img_MyPix"
  Source="snoqualmie-NF.jpg"
  AutomationProperties.LabeledBy="{Binding ElementName=caption_MyPix}"/>
<TextBlock x:Name="caption_MyPix">Mount Snoqualmie Skiing</TextBlock>
```

<span id="labels"/>
<span id="LABELS"/>

## <a name="labels-and-labeledby"></a>레이블 및 LabeledBy  
레이블을 양식 요소와 연결하는 기본 방법은 [**TextBlock**](https://msdn.microsoft.com/library/windows/apps/BR209652)을 **x:Name**과 함께 레이블 텍스트에 사용한 다음 양식 요소에서 [**AutomationProperties.LabeledBy**](https://msdn.microsoft.com/library/windows/apps/Hh759769) 연결된 속성을 설정하여 XAML 이름으로 레이블 지정 **TextBlock**을 참조하는 것입니다. 이 패턴을 사용하는 경우 사용자가 레이블을 클릭하면 포커스가 연결된 컨트롤로 이동하고 보조 기술에서 레이블 텍스트를 양식 필드의 접근성 있는 이름으로 사용할 수 있습니다. 다음은 이 기술을 보여 주는 예입니다.

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

## <a name="accessible-description-optional"></a>접근성 있는 설명(선택)  
접근성 있는 설명은 특정 UI 요소에 대해 추가적인 접근성 정보를 제공합니다. 일반적으로 접근성 있는 이름만으로 요소의 용도를 정확하게 전달할 수 없을 때 접근성 있는 설명을 사용합니다.

내레이터 화면 읽기 프로그램은 사용자가 CapsLock + F를 눌러 요소에 대한 추가 정보를 요청하는 경우에만 요소의 접근성 있는 설명을 읽습니다.

접근성 있는 이름은 컨트롤의 동작을 완전히 문서화하기 위한 것이 아니라 컨트롤을 식별하기 위한 것입니다. 간략한 설명이 컨트롤을 설명하기에 충분하지 않으면 [**AutomationProperties.Name**](https://msdn.microsoft.com/library/windows/apps/Hh759770) 외에 [**AutomationProperties.HelpText**](https://msdn.microsoft.com/library/windows/apps/Hh759765) 연결된 속성을 추가로 설정할 수 있습니다.

<span id="Testing_accessibility_early_and_often"/>
<span id="testing_accessibility_early_and_often"/>
<span id="TESTING_ACCESSIBILITY_EARLY_AND_OFTEN"/>

## <a name="testing-accessibility-early-and-often"></a>초기에 자주 접근성 테스트  
궁극적으로 화면 읽기 프로그램을 지원하는 최고의 접근 방법은 직접 화면 읽기 프로그램을 사용하여 앱을 테스트하는 것입니다. 이 테스트에서는 화면 읽기 프로그램의 작동 방식과 앱에서 누락될 수 있는 기본 접근성 정보를 보여 줍니다. 그에 맞게 UI 또는 UI 자동화 속성을 조정할 수 있습니다. 자세한 내용은 [접근성 테스트](accessibility-testing.md)를 참조하세요.

접근성 테스트에 사용할 수 있는 도구 중 하나는 **AccScope**입니다. 보조 기술에서 앱을 자동화 트리로 보는 방법을 나타내는 UI의 시각적 표현이 표시되기 때문에 **AccScope** 도구는 특히 유용합니다. 특히 내레이터가 앱에서 텍스트를 가져오는 방식 및 UI에서 요소를 구성하는 방식을 확인할 수 있는 내레이터 모드가 있습니다. AccScope는 앱 개발 주기 전체에서, 심지어 예비 디자인 단계 동안에도 유용하게 사용할 수 있도록 설계되었습니다. 자세한 내용은 [AccScope](https://msdn.microsoft.com/library/windows/desktop/Dn433239)를 참조하세요.

<span id="Accessible_names_from_dynamic_data"/>
<span id="accessible_names_from_dynamic_data"/>
<span id="ACCESSIBLE_NAMES_FROM_DYNAMIC_DATA"/>

## <a name="accessible-names-from-dynamic-data"></a>동적 데이터의 접근성 있는 이름  
Windows는 *데이터 바인딩*이라는 기능을 통해 연결된 데이터 원본에서 제공되는 값을 표시하는 데 사용할 수 있는 많은 컨트롤을 지원합니다. 데이터 항목으로 목록을 채우는 경우 초기 목록이 채워지면 데이터 바인딩 목록 항목의 접근성 있는 이름을 설정하는 기술을 사용해야 할 수 있습니다. 자세한 내용은 [XAML 접근성 샘플](http://go.microsoft.com/fwlink/p/?linkid=238570)의 "시나리오 4"를 참조하세요.

<span id="Accessible_names_and_localization"/>
<span id="accessible_names_and_localization"/>
<span id="ACCESSIBLE_NAMES_AND_LOCALIZATION"/>

## <a name="accessible-names-and-localization"></a>접근성 있는 이름 및 지역화  
접근성 있는 이름이 지역화되는 요소도 되도록 하려면 지역화 가능한 문자열을 리소스로 저장한 다음 [x:Uid directive](https://msdn.microsoft.com/library/windows/apps/Mt204791) 값으로 리소스 연결을 참조하는 올바른 기술을 사용해야 합니다. 접근성 있는 이름이 명시적으로 설정된 [**AutomationProperties.Name**](https://msdn.microsoft.com/library/windows/apps/Hh759770) 사용에서 제공되는 경우 해당 문자열도 지역화할 수 있는지 확인합니다.

[**AutomationProperties**](https://msdn.microsoft.com/library/windows/apps/BR209081) 속성과 같은 연결된 속성은 특수 정식 구문을 리소스 이름으로 사용하여, 리소스는 특정 요소에 적용된 연결된 속성을 참조합니다. 예를 들어 `MediumButton`이라는 UI 요소에 적용된 [**AutomationProperties.Name**](https://msdn.microsoft.com/library/windows/apps/Hh759770)은 `MediumButton.[using:Windows.UI.Xaml.Automation]AutomationProperties.Name`입니다.

<span id="related_topics"/>

## <a name="related-topics"></a>관련 항목  
* [접근성](accessibility.md)
* [**AutomationProperties.Name**](https://msdn.microsoft.com/library/windows/apps/Hh759770)
* [XAML 접근성 샘플](http://go.microsoft.com/fwlink/p/?linkid=238570)
* [접근성 테스트](accessibility-testing.md)
