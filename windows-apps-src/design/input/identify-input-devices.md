---
description: Windows 앱 장치에 연결 된 입력 장치를 식별 하 고 해당 기능 및 특성을 식별 합니다.
title: 입력 디바이스 식별
ms.assetid: B2E93FBF-C508-44D9-BA46-ECFDAA8746F4
label: Identify input devices
template: detail.hbs
keywords: 장치, 디지타이저, 입력, 상호 작용
ms.date: 09/24/2020
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 51d5ea7bf6776c2c728fd000ffea63b79a4d7464
ms.sourcegitcommit: a3bbd3dd13be5d2f8a2793717adf4276840ee17d
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/30/2020
ms.locfileid: "93030426"
---
# <a name="identify-input-devices"></a>입력 디바이스 식별


Windows 앱 장치에 연결 된 입력 장치를 식별 하 고 해당 기능 및 특성을 식별 합니다.

> **중요 한 api** : [**windows**](/uwp/api/Windows.Devices.Input). input, [**windows**](/uwp/api/Windows.UI.Core). uiinput, windows. [**.xaml**](/uwp/api/Windows.UI.Input)

## <a name="retrieve-mouse-properties"></a>마우스 속성 검색


MouseCapabilities [**네임 스페이스**](/uwp/api/Windows.Devices.Input) 는 하나 이상의 연결 된 마우스에 의해 노출 되는 속성을 검색 하는 데 사용 되는 [**MouseCapabilities**](/uwp/api/Windows.Devices.Input.MouseCapabilities) 클래스를 포함 합니다. 새 **MouseCapabilities** 개체를 만들고 관심 있는 속성을 가져옵니다.

**참고**  여기서 설명 하는 속성에 의해 반환 되는 값은 검색 된 모든 마우스를 기반으로 합니다. 하나 이상의 마우스가 특정 기능을 지 원하는 경우 부울 속성은 0이 아닌 값을 반환 하 고 숫자 속성은 한 마우스에서 노출 하는 최대값을 반환 합니다.

 

다음 코드에서는 일련의 [**TextBlock**](/uwp/api/Windows.UI.Xaml.Controls.TextBlock) 요소를 사용 하 여 개별 마우스 속성 및 값을 표시 합니다.

```CSharp
private void GetMouseProperties()
{
    MouseCapabilities mouseCapabilities = new Windows.Devices.Input.MouseCapabilities();
    MousePresent.Text = mouseCapabilities.MousePresent != 0 ? "Yes" : "No";
    VertWheel.Text = mouseCapabilities.VerticalWheelPresent != 0 ? "Yes" : "No";
    HorzWheel.Text = mouseCapabilities.HorizontalWheelPresent != 0 ? "Yes" : "No";
    SwappedButtons.Text = mouseCapabilities.SwapButtons != 0 ? "Yes" : "No";
    NumButtons.Text = mouseCapabilities.NumberOfButtons.ToString();
}
```

## <a name="retrieve-keyboard-properties"></a>키보드 속성 검색


KeyboardCapabilities [**네임 스페이스**](/uwp/api/Windows.Devices.Input) 는 키보드의 연결 여부를 검색 하는 데 사용 되는 [**KeyboardCapabilities**](/uwp/api/Windows.Devices.Input.KeyboardCapabilities) 클래스를 포함 합니다. 새 **KeyboardCapabilities** 개체를 만들고 [**KeyboardPresent**](/uwp/api/windows.devices.input.keyboardcapabilities.keyboardpresent) 속성을 가져옵니다.

다음 코드에서는 [**TextBlock**](/uwp/api/Windows.UI.Xaml.Controls.TextBlock) 요소를 사용 하 여 키보드 속성 및 값을 표시 합니다.

```CSharp
private void GetKeyboardProperties()
{
    KeyboardCapabilities keyboardCapabilities = new Windows.Devices.Input.KeyboardCapabilities();
    KeyboardPresent.Text = keyboardCapabilities.KeyboardPresent != 0 ? "Yes" : "No";
}
```

## <a name="retrieve-touch-properties"></a>터치 속성 검색


TouchCapabilities [**네임 스페이스**](/uwp/api/Windows.Devices.Input) 는 터치 디지타이저가 연결 되어 있는지 여부를 검색 하는 데 사용 되는 [**TouchCapabilities**](/uwp/api/Windows.Devices.Input.TouchCapabilities) 클래스를 포함 합니다. 새 **TouchCapabilities** 개체를 만들고 관심 있는 속성을 가져옵니다.

**참고**  여기서 설명 하는 속성에 의해 반환 되는 값은 검색 된 모든 터치 디지타이저를 기반으로 합니다. 하나 이상의 디지타이저가 특정 기능을 지원 하 고 숫자 속성이 단일 디지타이저에서 노출 하는 최대값을 반환 하는 경우 부울 속성은 0이 아닌 값을 반환 합니다.

 

다음 코드는 일련의 [**TextBlock**](/uwp/api/Windows.UI.Xaml.Controls.TextBlock) 요소를 사용 하 여 터치 속성 및 값을 표시 합니다.

```CSharp
private void GetTouchProperties()
{
    TouchCapabilities touchCapabilities = new Windows.Devices.Input.TouchCapabilities();
    TouchPresent.Text = touchCapabilities.TouchPresent != 0 ? "Yes" : "No";
    Contacts.Text = touchCapabilities.Contacts.ToString();
}
```

## <a name="retrieve-pointer-properties"></a>포인터 속성 검색


[**Windows. 입력**](/uwp/api/Windows.Devices.Input) 네임 스페이스는 검색 된 장치에서 포인터 입력 (터치, 터치 패드, 마우스 또는 펜)을 지원 하는지 여부를 검색 하는 데 사용 되는 [**pointerdevice**](/uwp/api/Windows.Devices.Input.PointerDevice) 클래스를 포함 합니다. 새 **Pointerdevice** 개체를 만들고 관심 있는 속성을 가져옵니다.

**참고**  여기서 설명 하는 속성에 의해 반환 되는 값은 검색 된 모든 포인터 장치를 기반으로 합니다. 하나 이상의 장치에서 특정 기능을 지 원하는 경우 부울 속성은 0이 아닌 값을 반환 하 고, 숫자 속성은 하나의 포인터 장치에서 노출 하는 최대값을 반환 합니다.

다음 코드에서는 테이블을 사용 하 여 각 포인터 장치에 대 한 속성 및 값을 표시 합니다.

```CSharp
private void GetPointerDevices()
{
    IReadOnlyList<PointerDevice> pointerDevices = Windows.Devices.Input.PointerDevice.GetPointerDevices();
    int gridRow = 0;
    int gridColumn = 0;

    for (int i = 0; i < pointerDevices.Count; i++)
    {
        // Pointer device type.
        TextBlock textBlock1 = new TextBlock();
        Grid_PointerProps.Children.Add(textBlock1);
        textBlock1.Text = (i + 1).ToString() + " Pointer Device Type:";
        Grid.SetRow(textBlock1, gridRow);
        Grid.SetColumn(textBlock1, gridColumn);

        TextBlock textBlock2 = new TextBlock();
        textBlock2.Text = pointerDevices[i].PointerDeviceType.ToString();
        Grid_PointerProps.Children.Add(textBlock2);
        Grid.SetRow(textBlock2, gridRow++);
        Grid.SetColumn(textBlock2, gridColumn + 1);

        // Is external?
        TextBlock textBlock3 = new TextBlock();
        Grid_PointerProps.Children.Add(textBlock3);
        textBlock3.Text = (i + 1).ToString() + " Is External?";
        Grid.SetRow(textBlock3, gridRow);
        Grid.SetColumn(textBlock3, gridColumn);

        TextBlock textBlock4 = new TextBlock();
        Grid_PointerProps.Children.Add(textBlock4);
        textBlock4.Text = pointerDevices[i].IsIntegrated.ToString();
        Grid.SetRow(textBlock4, gridRow++);
        Grid.SetColumn(textBlock4, gridColumn + 1);

        // Maximum contacts.
        TextBlock textBlock5 = new TextBlock();
        Grid_PointerProps.Children.Add(textBlock5);
        textBlock5.Text = (i + 1).ToString() + " Max Contacts:";
        Grid.SetRow(textBlock5, gridRow);
        Grid.SetColumn(textBlock5, gridColumn);

        TextBlock textBlock6 = new TextBlock();
        Grid_PointerProps.Children.Add(textBlock6);
        textBlock6.Text = pointerDevices[i].MaxContacts.ToString();
        Grid.SetRow(textBlock6, gridRow++);
        Grid.SetColumn(textBlock6, gridColumn + 1);

        // Physical device rectangle.
        TextBlock textBlock7 = new TextBlock();
        Grid_PointerProps.Children.Add(textBlock7);
        textBlock7.Text = (i + 1).ToString() + " Physical Device Rect:";
        Grid.SetRow(textBlock7, gridRow);
        Grid.SetColumn(textBlock7, gridColumn);

        TextBlock textBlock8 = new TextBlock();
        Grid_PointerProps.Children.Add(textBlock8);
        textBlock8.Text = pointerDevices[i].PhysicalDeviceRect.X.ToString() + "," +
            pointerDevices[i].PhysicalDeviceRect.Y.ToString() + "," +
            pointerDevices[i].PhysicalDeviceRect.Width.ToString() + "," +
            pointerDevices[i].PhysicalDeviceRect.Height.ToString();
        Grid.SetRow(textBlock8, gridRow++);
        Grid.SetColumn(textBlock8, gridColumn + 1);

        // Screen rectangle.
        TextBlock textBlock9 = new TextBlock();
        Grid_PointerProps.Children.Add(textBlock9);
        textBlock9.Text = (i + 1).ToString() + " Screen Rect:";
        Grid.SetRow(textBlock9, gridRow);
        Grid.SetColumn(textBlock9, gridColumn);

        TextBlock textBlock10 = new TextBlock();
        Grid_PointerProps.Children.Add(textBlock10);
        textBlock10.Text = pointerDevices[i].ScreenRect.X.ToString() + "," +
            pointerDevices[i].ScreenRect.Y.ToString() + "," +
            pointerDevices[i].ScreenRect.Width.ToString() + "," +
            pointerDevices[i].ScreenRect.Height.ToString();
        Grid.SetRow(textBlock10, gridRow++);
        Grid.SetColumn(textBlock10, gridColumn + 1);

        gridColumn += 2;
        gridRow = 0;
    }
```

## <a name="related-articles"></a>관련된 문서

### <a name="samples"></a>샘플

- [기본 입력 샘플](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/BasicInput)
- [짧은 대기 시간 입력 샘플](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/LowLatencyInput)
- [사용자 상호 작용 모드 샘플](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/UserInteractionMode)

### <a name="archive-samples"></a>보관 샘플

- [입력: 장치 기능 샘플](https://github.com/microsoftarchive/msdn-code-gallery-microsoft/tree/411c271e537727d737a53fa2cbe99eaecac00cc0/Official%20Windows%20Platform%20Sample/Windows%208%20app%20samples/%5BC%23%5D-Windows%208%20app%20samples/C%23/Windows%208%20app%20samples/Input%20Device%20capabilities%20sample%20(Windows%208))
