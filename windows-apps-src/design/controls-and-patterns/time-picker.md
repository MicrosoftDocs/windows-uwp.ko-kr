---
Description: 시간 선택기는 사용자가 터치, 마우스 또는 키보드 입력을 사용하여 시간 값을 선택할 수 있는 표준화된 방법을 제공합니다.
title: 시간 선택기
ms.assetid: 5124ecda-09e6-449e-9d4a-d969dca46aa3
label: Time picker
template: detail.hbs
ms.date: 05/08/2017
ms.topic: article
keywords: windows 10, uwp
pm-contact: kisai
design-contact: ksulliv
dev-contact: joyate
doc-status: Published
ms.localizationpriority: medium
ms.openlocfilehash: 71f9d7e42b39b91186c9a2aa3ab1b87ed7f4d8b4
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89168717"
---
# <a name="time-picker"></a>시간 선택기
 

시간 선택기는 사용자가 터치, 마우스 또는 키보드 입력을 사용하여 시간 값을 선택할 수 있는 표준화된 방법을 제공합니다.

![시간 선택기의 예](images/time-picker-closed.png)

**Windows UI 라이브러리 가져오기**

|  |  |
| - | - |
| ![WinUI 로고](images/winui-logo-64x64.png) | Windows UI 라이브러리 2.2 이상에는 둥근 모서리를 사용하는 이 컨트롤의 새 템플릿이 포함되어 있습니다. 자세한 내용은 [모서리 반경](../style/rounded-corner.md)을 참조하세요. WinUI는 Windows 앱에 대한 새 컨트롤 및 UI 기능이 포함된 NuGet 패키지입니다. 설치 지침을 비롯한 자세한 내용은 [Windows UI 라이브러리](/uwp/toolkits/winui/)를 참조하세요. |

> **플랫폼 API**: [TimePicker 클래스](/uwp/api/Windows.UI.Xaml.Controls.TimePicker), [Time 속성](/uwp/api/windows.ui.xaml.controls.timepicker.time)


## <a name="is-this-the-right-control"></a>올바른 컨트롤인가요?
사용자가 단일 시간 값을 선택할 수 있게 하려면 시간 선택기를 사용합니다.

올바른 컨트롤을 선택하는 방법에 대한 자세한 내용은 [날짜 및 시간 컨트롤](date-and-time.md) 문서를 참조하세요.

## <a name="examples"></a>예

<table>
<th align="left">XAML Controls Gallery<th>
<tr>
<td><img src="images/xaml-controls-gallery-app-icon-sm.png" alt="XAML controls gallery"></img></td>
<td>
    <p><strong style="font-weight: semi-bold">XAML 컨트롤 갤러리</strong> 앱이 설치된 경우 여기를 클릭하여 <a href="xamlcontrolsgallery:/item/TimePicker">앱을 열고 실제로 작동하는 TimePicker를 확인</a>합니다.</p>
    <ul>
    <li><a href="https://www.microsoft.com/store/productId/9MSVH128X2ZT">XAML Controls Gallery 앱 가져오기(Microsoft Store)</a></li>
    <li><a href="https://github.com/Microsoft/Xaml-Controls-Gallery">소스 코드 가져오기(GitHub)</a></li>
    </ul>
</td>
</tr>
</table>

진입점에 선택한 시간이 표시되고 사용자가 진입점을 선택하면 선택할 수 있게 선택기 화면이 가운데에서 세로로 확장됩니다. 이 시간 선택 컨트롤은 다른 UI에 겹쳐지며 다른 UI를 보이지 않도록 합니다.

![확장되는 시간 선택기의 예](images/controls_timepicker_expand.png)

## <a name="create-a-time-picker"></a>시간 선택기 만들기

이 예제에서는 머리글을 사용하여 간단한 시간 선택기를 만드는 방법을 보여 줍니다.

```xaml
<TimePicker x:Name="arrivalTimePicker" Header="Arrival time"/>
```

```csharp
TimePicker arrivalTimePicker = new TimePicker();
arrivalTimePicker.Header = "Arrival time";
```

이에 따라 표시되는 시간 선택기는 다음과 같습니다.

![시간 선택기의 예](images/time-picker-closed.png)

> [!NOTE]
> 날짜 및 시간 값에 대한 중요한 정보는 *날짜 및 시간 컨트롤* 문서의 [DateTime 및 Calendar 값](date-and-time.md#datetime-and-calendar-values)을 참조하세요.

## <a name="get-the-sample-code"></a>샘플 코드 가져오기

- [XAML 컨트롤 갤러리 샘플](https://github.com/Microsoft/Xaml-Controls-Gallery) - 대화형 형식으로 모든 XAML 컨트롤을 보여줍니다.

## <a name="related-topics"></a>관련 항목

- [날짜 및 시간 컨트롤](date-and-time.md)
- [달력 날짜 선택](calendar-date-picker.md)
- [달력 보기](calendar-view.md)
- [날짜 선택기](date-picker.md)