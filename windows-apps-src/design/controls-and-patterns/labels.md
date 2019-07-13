---
Description: 레이블을 사용하여 인접한 컨트롤에 입력해야 하는 내용을 사용자에게 표시합니다. 관련 컨트롤 그룹에 레이블을 지정하거나 관련 컨트롤 그룹 근처에 지침 텍스트를 표시할 수도 있습니다.
title: Labels(레이블)
ms.assetid: CFACCCD4-749F-43FB-947E-2591AE673804
label: Labels
template: detail.hbs
ms.date: 05/19/2017
ms.topic: article
keywords: windows 10, uwp
pm-contact: miguelrb
design-contact: ksulliv
doc-status: Published
ms.localizationpriority: medium
ms.openlocfilehash: 2b642f1d6c3f2a04bacdf293858492ea095af1a8
ms.sourcegitcommit: 6f32604876ed480e8238c86101366a8d106c7d4e
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 06/21/2019
ms.locfileid: "67319525"
---
# <a name="labels"></a>Labels(레이블)

 

레이블은 컨트롤 또는 관련 컨트롤 그룹의 이름이나 제목입니다.

> **중요 API**: Header 속성, [TextBlock 클래스](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.TextBlock)

XAML에서 대부분의 컨트롤에는 레이블 표시에 사용하는 기본 제공 Header 속성이 있습니다. 컨트롤에 Header 속성이 없거나 컨트롤 그룹에 레이블을 지정하려는 경우에는 [TextBlock](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.TextBlock)을 사용할 수 있습니다.

![표준 레이블 컨트롤을 보여 주는 스크린샷](images/label-standard.png)

## <a name="recommendations"></a>권장 사항


-   레이블을 사용하여 인접한 컨트롤에 입력해야 하는 내용을 사용자에게 표시합니다. 관련 컨트롤 그룹에 레이블을 지정하거나 관련 컨트롤 그룹 근처에 지침 텍스트를 표시할 수도 있습니다.
-   컨트롤에 레이블을 지정할 경우 문장이나 지침 텍스트가 아니라 명사나 간결한 명사구로 레이블을 작성하세요. 콜론 또는 기타 문장 부호를 사용하지 마세요.
-   레이블에 지침 텍스트가 있는 경우 텍스트 문자열 길이를 더욱 자유롭게 사용할 수 있으며 문장 부호도 사용할 수 있습니다.


## <a name="get-the-sample-code"></a>샘플 코드 다운로드
* [XAML UI 기본 사항 샘플](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlUIBasics)

## <a name="related-topics"></a>관련 항목
* [텍스트 컨트롤](text-controls.md)
* [TextBox.Header 속성](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.textbox.header)
* [PasswordBox.Header 속성](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.passwordbox.header)
* [ToggleSwitch.Header 속성](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.toggleswitch.header)
* [DatePicker.Header 속성](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.datepicker.header)
* [TimePicker.Header 속성](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.timepicker.header)
* [Slider.Header 속성](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.slider.header)
* [ComboBox.Header 속성](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.combobox.header)
* [RichEditBox.Header 속성](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.richeditbox.header)
* [TextBlock 클래스](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.TextBlock)

 

 




