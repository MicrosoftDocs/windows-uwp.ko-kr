---
author: muhsinking
Description: The date picker gives you a standardized way to let users pick a localized date value using touch, mouse, or keyboard input.
title: 날짜 선택기
ms.assetid: d4a01425-4dee-4de3-9a05-3e85c3fc03cb
isNew: true
label: Date picker
template: detail.hbs
ms.author: mukin
ms.date: 05/19/2017
ms.topic: article
keywords: windows 10, uwp
pm-contact: kisai
design-contact: ksulliv
dev-contact: joyate
doc-status: Published
ms.localizationpriority: medium
ms.openlocfilehash: 1f258ff63d2cf9badfc1066f46c97ecc14b90f5f
ms.sourcegitcommit: ed0304b8a214c03b8aab74b8ef12c9f82b8e3c5f
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/19/2018
ms.locfileid: "7305155"
---
# <a name="date-picker"></a>날짜 선택기

 

날짜 선택은 사용자가 터치, 마우스 또는 키보드 입력을 사용하여 지역화된 날짜 값을 선택할 수 있는 표준화된 방법을 제공합니다. 

> **중요 API**: [DatePicker 클래스](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.datepicker.aspx), [Date 속성](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.datepicker.date.aspx)


## <a name="is-this-the-right-control"></a>올바른 컨트롤인가요?
날짜 선택을 사용하여 사용자는 달력의 컨텍스트가 중요하지 않은 생일과 같은 알려진 날짜를 선택할 수 있습니다.

올바른 날짜 컨트롤을 선택하는 방법에 대한 자세한 내용은 [날짜 및 시간 컨트롤](date-and-time.md) 문서를 참조하세요.

## <a name="examples"></a>예

<table>
<th align="left">XAML 컨트롤 갤러리<th>
<tr>
<td><img src="images/xaml-controls-gallery-sm.png" alt="XAML controls gallery"></img></td>
<td>
    <p><strong style="font-weight: semi-bold">XAML 컨트롤 갤러리</strong> 앱이 설치된 경우 여기를 클릭하여 <a href="xamlcontrolsgallery:/item/DatePicker">앱을 열고 작동 중인 DatePicker를 확인</a>합니다.</p>
    <ul>
    <li><a href="https://www.microsoft.com/store/productId/9MSVH128X2ZT">XAML 컨트롤 갤러리 앱 다운로드(Microsoft Store)</a></li>
    <li><a href="https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlUIBasics">소스 코드 다운로드(GitHub)</a></li>
    </ul>
</td>
</tr>
</table>

진입점에 선택한 날짜가 표시되고 사용자가 진입점을 선택하면 선택할 수 있게 선택 화면이 가운데에서 세로로 확장됩니다. 이 날짜 선택 컨트롤은 다른 UI에 겹쳐지며 다른 UI를 보이지 않도록 합니다.

![날짜 선택 확장 예](images/controls_datepicker_expand.png)

## <a name="create-a-date-picker"></a>날짜 선택 만들기

이 예제에서는 머리글을 사용하여 간단한 날짜 선택을 만드는 방법을 보여 줍니다.

```xaml
<DatePicker x:Name=birthDatePicker Header="Date of birth"/>
```

```csharp
DatePicker birthDatePicker = new DatePicker();
birthDatePicker.Header = "Date of birth";
```

결과 날짜 선택은 다음과 같습니다.

![날짜 선택의 예](images/date-picker-closed.png)

> **참고**&nbsp;&nbsp;날짜 값에 대한 중요한 내용은 날짜 및 시간 컨트롤 문서의 [DateTime 및 Calendar 값](date-and-time.md#datetime-and-calendar-values)을 참조하세요.

## <a name="get-the-sample-code"></a>샘플 코드 다운로드

- [XAML 컨트롤 갤러리 샘플](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlUIBasics) - 대화형 형식의 모든 XAML 컨트롤을 보여줍니다.

## <a name="related-articles"></a>관련 문서

- [날짜 및 시간 컨트롤](date-and-time.md)
- [달력 날짜 선택](calendar-date-picker.md)
- [달력 보기](calendar-view.md)
- [시간 선택기](time-picker.md)
