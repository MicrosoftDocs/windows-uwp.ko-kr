---
Description: UWP(유니버설 Windows 플랫폼) 장치에 연결된 입력 장치를 식별하고 해당 기능 및 특성을 식별합니다.
title: 입력 장치 식별
ms.assetid: B2E93FBF-C508-44D9-BA46-ECFDAA8746F4
label: Identify input devices
template: detail.hbs
keywords: 장치, 디지타이저, 입력, 조작
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: b2a17d1f4664326cb54d9c53d828eb372ef93fe4
ms.sourcegitcommit: b52ddecccb9e68dbb71695af3078005a2eb78af1
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/20/2019
ms.locfileid: "74257889"
---
# <a name="identify-input-devices"></a>입력 장치 식별


UWP(유니버설 Windows 플랫폼) 장치에 연결된 입력 장치를 식별하고 해당 기능 및 특성을 식별합니다.

> **중요 API**: [**Windows.Devices.Input**](https://docs.microsoft.com/uwp/api/Windows.Devices.Input), [**Windows.UI.Input**](https://docs.microsoft.com/uwp/api/Windows.UI.Core), [**Windows.UI.Xaml.Input**](https://docs.microsoft.com/uwp/api/Windows.UI.Input)

## <a name="retrieve-mouse-properties"></a>마우스 속성 검색


[  **Windows.Devices.Input**](https://docs.microsoft.com/uwp/api/Windows.Devices.Input) 네임스페이스에는 하나 이상의 연결된 마우스가 제공하는 속성을 검색하는 데 사용되는 [**MouseCapabilities**](https://docs.microsoft.com/uwp/api/Windows.Devices.Input.MouseCapabilities) 클래스가 포함되어 있습니다. 새 **MouseCapabilities** 개체를 만들고 관심 있는 속성을 가져옵니다.

**Note**  The values returned by the properties discussed here are based on all detected mice: Boolean properties return non-zero if at least one mouse supports a specific capability, and numeric properties return the maximum value exposed by any one mouse.

 

다음 코드에서는 [**TextBlock**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.TextBlock) 요소를 여러 개 사용하여 개별 마우스 속성과 값을 표시합니다.

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


[  **Windows.Devices.Input**](https://docs.microsoft.com/uwp/api/Windows.Devices.Input) 네임스페이스에는 키보드가 연결되었는지 여부를 검색하는 데 사용되는 [**KeyboardCapabilities**](https://docs.microsoft.com/uwp/api/Windows.Devices.Input.KeyboardCapabilities) 클래스가 포함되어 있습니다. 새 **KeyboardCapabilities** 개체를 만들고 [**KeyboardPresent**](https://docs.microsoft.com/uwp/api/windows.devices.input.keyboardcapabilities.keyboardpresent) 속성을 가져옵니다.

다음 코드에서는 [**TextBlock**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.TextBlock) 요소를 한 개 사용하여 키보드 속성과 값을 표시합니다.

```CSharp
private void GetKeyboardProperties()
{
    KeyboardCapabilities keyboardCapabilities = new Windows.Devices.Input.KeyboardCapabilities();
    KeyboardPresent.Text = keyboardCapabilities.KeyboardPresent != 0 ? "Yes" : "No";
}
```

## <a name="retrieve-touch-properties"></a>터치 속성 검색


[  **Windows.Devices.Input**](https://docs.microsoft.com/uwp/api/Windows.Devices.Input) 네임스페이스에는 터치 디지타이저가 연결되었는지 여부를 검색하는 데 사용되는 [**TouchCapabilities**](https://docs.microsoft.com/uwp/api/Windows.Devices.Input.TouchCapabilities) 클래스가 포함되어 있습니다. 새 **TouchCapabilities** 개체를 만들고 관심 있는 속성을 가져옵니다.

**Note**  The values returned by the properties discussed here are based on all detected touch digitizers: Boolean properties return non-zero if at least one digitizer supports a specific capability, and numeric properties return the maximum value exposed by any one digitizer.

 

다음 코드에서는 [**TextBlock**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.TextBlock) 요소를 여러 개 사용하여 터치 속성과 값을 표시합니다.

```CSharp
private void GetTouchProperties()
{
    TouchCapabilities touchCapabilities = new Windows.Devices.Input.TouchCapabilities();
    TouchPresent.Text = touchCapabilities.TouchPresent != 0 ? "Yes" : "No";
    Contacts.Text = touchCapabilities.Contacts.ToString();
}
```

## <a name="retrieve-pointer-properties"></a>포인터 속성 검색


[  **Windows.Devices.Input**](https://docs.microsoft.com/uwp/api/Windows.Devices.Input) 네임스페이스에는 감지된 장치 지원 포인터 입력(터치, 터치 패드, 마우스 또는 펜)을 지원하는 장치가 연결되어 있는지 여부를 검색하는 데 사용되는 [**PointerDevice**](https://docs.microsoft.com/uwp/api/Windows.Devices.Input.PointerDevice) 클래스가 포함되어 있습니다. 새 **PointerDevice** 개체를 만들고 관심 있는 속성을 가져옵니다.

**Note**  The values returned by the properties discussed here are based on all detected pointer devices: Boolean properties return non-zero if at least one device supports a specific capability, and numeric properties return the maximum value exposed by any one pointer device.

다음 코드에서는 표를 사용하여 각 포인터 장치의 속성과 값을 표시합니다.

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

## <a name="related-articles"></a>관련 문서


**샘플**
* [Basic input sample](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/BasicInput)
* [Low latency input sample](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/LowLatencyInput)
* [User interaction mode sample](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/UserInteractionMode)

**Archive samples**
* [Input: Device capabilities sample](https://code.msdn.microsoft.com/windowsapps/Input-device-capabilities-31b67745)
 

 




