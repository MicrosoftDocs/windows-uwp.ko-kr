---
author: Jwmsft
Description: Use a label to indicate to the user what they should enter into an adjacent control. You can also label a group of related controls, or display instructional text near a group of related controls.
title: 레이블
ms.assetid: CFACCCD4-749F-43FB-947E-2591AE673804
label: Labels
template: detail.hbs
ms.author: jimwalk
ms.date: 05/19/2017
ms.topic: article
keywords: windows 10, uwp
pm-contact: miguelrb
design-contact: ksulliv
doc-status: Published
ms.localizationpriority: medium
ms.openlocfilehash: c17b2a539a01572bed984b86f72f5439896fac87
ms.sourcegitcommit: 144f5f127fc4fbd852f2f6780ef26054192d68fc
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/02/2018
ms.locfileid: "5991745"
---
# <a name="labels"></a>레이블

 

레이블은 컨트롤 또는 관련 컨트롤 그룹의 이름이나 제목입니다.

> **중요 API**: Header 속성, [TextBlock 클래스](https://msdn.microsoft.com/library/windows/apps/br209652)

XAML에서 대부분의 컨트롤에는 레이블 표시에 사용하는 기본 제공 Header 속성이 있습니다. 컨트롤에 Header 속성이 없거나 컨트롤 그룹에 레이블을 지정하려는 경우에는 [TextBlock](https://msdn.microsoft.com/library/windows/apps/br209652)을 사용할 수 있습니다.

![표준 레이블 컨트롤을 보여 주는 스크린샷](images/label-standard.png)

## <a name="recommendations"></a>권장 사항


-   레이블을 사용하여 인접한 컨트롤에 입력해야 하는 내용을 사용자에게 표시합니다. 관련 컨트롤 그룹에 레이블을 지정하거나 관련 컨트롤 그룹 근처에 지침 텍스트를 표시할 수도 있습니다.
-   컨트롤에 레이블을 지정할 경우 문장이나 지침 텍스트가 아니라 명사나 간결한 명사구로 레이블을 작성하세요. 콜론 또는 기타 문장 부호를 사용하지 마세요.
-   레이블에 지침 텍스트가 있는 경우 텍스트 문자열 길이를 더욱 자유롭게 사용할 수 있으며 문장 부호도 사용할 수 있습니다.


## <a name="get-the-sample-code"></a>샘플 코드 다운로드
* [XAML UI 기본 사항 샘플](https://github.com/Microsoft/Windows-universal-samples/blob/master/Samples/XamlUIBasics)

## <a name="related-topics"></a>관련 항목
* [텍스트 컨트롤](text-controls.md)
* [TextBox.Header 속성](https://msdn.microsoft.com/library/windows/apps/dn252861)
* [PasswordBox.Header 속성](https://msdn.microsoft.com/library/windows/apps/dn299051)
* [ToggleSwitch.Header 속성](https://msdn.microsoft.com/library/windows/apps/br209713)
* [DatePicker.Header 속성](https://msdn.microsoft.com/library/windows/apps/dn279460)
* [TimePicker.Header 속성](https://msdn.microsoft.com/library/windows/apps/dn299286)
* [Slider.Header 속성](https://msdn.microsoft.com/library/windows/apps/dn252829)
* [ComboBox.Header 속성](https://msdn.microsoft.com/library/windows/apps/dn279416)
* [RichEditBox.Header 속성](https://msdn.microsoft.com/library/windows/apps/dn252726)
* [TextBlock 클래스](https://msdn.microsoft.com/library/windows/apps/br209652)

 

 




