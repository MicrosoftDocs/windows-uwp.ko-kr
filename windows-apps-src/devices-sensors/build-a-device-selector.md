---
author: DBirtolo
ms.assetid: D06AA3F5-CED6-446E-94E8-713D98B13CAA
title: "디바이스 선택기 빌드"
description: "디바이스 선택기를 빌드하면 디바이스를 열거할 때 검색하는 디바이스를 제한할 수 있습니다."
translationtype: Human Translation
ms.sourcegitcommit: 6530fa257ea3735453a97eb5d916524e750e62fc
ms.openlocfilehash: 091767d6f223ce2b4538dafb1c81595015589013

---
# 디바이스 선택기 빌드

\[ Windows 10의 UWP 앱에 맞게 업데이트되었습니다. Windows 8.x 문서는 [보관](http://go.microsoft.com/fwlink/p/?linkid=619132)을 참조하세요. \]


** 중요 API **

-   [**Windows.Devices.Enumeration**](https://msdn.microsoft.com/library/windows/apps/BR225459)

디바이스 선택기를 빌드하면 디바이스를 열거할 때 검색하는 디바이스를 제한할 수 있습니다. 이를 통해 관련된 결과만 가져올 수 있으며 시스템 성능도 향상됩니다. 대부분의 시나리오에서는 디바이스 스택에서 디바이스 선택기를 가져옵니다. 예를 들어 USB를 통해 검색된 디바이스에 [**GetDeviceSelector**](https://msdn.microsoft.com/library/windows/apps/Dn264015)를 사용할 수 있습니다. 이러한 디바이스 선택기는 AQS(고급 쿼리 구문) 문자열을 반환합니다. AQS 형식을 잘 모르는 경우 [프로그래밍 방식으로 고급 쿼리 구문 사용](https://msdn.microsoft.com/library/windows/desktop/Bb266512)에서 자세히 알아볼 수 있습니다.

## 필터 문자열 작성

디바이스를 열거해야 하는데 제공된 디바이스 선택기를 시나리오에 사용할 수 없는 경우도 있습니다. 디바이스 선택기는 다음 정보를 포함하는 AQS 필터 문자열입니다. 필터 문자열을 만들기 전에 열거하려는 디바이스에 대한 몇 가지 주요 정보를 알아야 합니다.

-   관심 있는 디바이스의 [**DeviceInformationKind**](https://msdn.microsoft.com/library/windows/apps/Dn948991). **DeviceInformationKind**가 열거 디바이스에 영향을 미치는 방법에 대한 자세한 내용은 [디바이스 열거](enumerate-devices.md)를 참조하세요.
-   이 항목에 설명된 AQS 필터 문자열을 작성하는 방법
-   관심 있는 속성. 사용 가능한 속성은 [**DeviceInformationKind**](https://msdn.microsoft.com/library/windows/apps/Dn948991)에 따라 달라집니다. 자세한 내용은 [디바이스 정보 속성](device-information-properties.md)을 참조하세요.
-   쿼리할 프로토콜. 무선 또는 유선 네트워크를 통해 디바이스를 검색하는 경우에만 필요합니다. 이 작업을 수행하는 방법에 대한 자세한 내용은 [네트워크를 통해 디바이스 열거](enumerate-devices-over-a-network.md)를 참조하세요.

[**Windows.Devices.Enumeration**](https://msdn.microsoft.com/library/windows/apps/BR225459) API를 사용할 경우 디바이스 선택기를 관심 있는 디바이스 종류와 결합하는 경우가 많습니다. 사용 가능한 디바이스 종류 목록은 [**DeviceInformationKind**](https://msdn.microsoft.com/library/windows/apps/Dn948991) 열거형으로 정의됩니다. 이렇게 요소를 결합하면 사용 가능한 디바이스를 관심 있는 디바이스로 제한할 수 있습니다. **DeviceInformationKind**를 지정하지 않거나 사용하는 메서드에서 **DeviceInformationKind** 매개 변수를 제공하지 않으면 기본 유형은 **DeviceInterface**입니다.

[**Windows.Devices.Enumeration**](https://msdn.microsoft.com/library/windows/apps/BR225459) API는 정식 AQS 구문을 사용하지만 일부 연산자가 지원되지 않습니다. 필터 문자열을 만들 때 사용할 수 있는 속성 목록은 [디바이스 정보 속성](device-information-properties.md)을 참조하세요.

**주의** `{GUID} PID` 형식을 사용하여 정의하는 사용자 지정 속성은 AQS 필터 문자열을 생성할 때 사용할 수 없습니다. 이 속성 유형은 잘 알려진 속성 이름에서 파생되기 때문입니다.

 

다음 표에는 AQS 연산자와 각 연산자에서 지원하는 매개 변수 형식이 나와 있습니다.

| 연산자                       | 지원되는 형식                                                             |
|--------------------------------|-----------------------------------------------------------------------------|
| **COP\_EQUAL**                 | 문자열, 부울, GUID, UInt16, UInt32                                       |
| **COP\_NOTEQUAL**              | 문자열, 부울, GUID, UInt16, UInt32                                       |
| **COP\_LESSTHAN**              | UInt16, UInt32                                                              |
| **COP\_GREATERTHAN**           | UInt16, UInt32                                                              |
| **COP\_LESSTHANOREQUAL**       | UInt16, UInt32                                                              |
| **COP\_GREATERTHANOREQUAL**    | UInt16, UInt32                                                              |
| **COP\_VALUE\_CONTAINS**       | 문자열, 문자열 배열, 부울 배열, 배열 GUID, UInt16 배열, UInt32 배열 |
| **COP\_VALUE\_NOTCONTAINS**    | 문자열, 문자열 배열, 부울 배열, 배열 GUID, UInt16 배열, UInt32 배열 |
| **COP\_VALUE\_STARTSWITH**     | 문자열                                                                      |
| **COP\_VALUE\_ENDSWITH**       | 문자열                                                                      |
| **COP\_DOSWILDCARDS**          | 지원되지 않음                                                               |
| **COP\_WORD\_EQUAL**           | 지원되지 않음                                                               |
| **COP\_WORD\_STARTSWITH**      | 지원되지 않음                                                               |
| **COP\_APPLICATION\_SPECIFIC** | 지원되지 않음                                                               |


> **팁** **COP\_EQUAL** 또는 **COP\_NOTEQUAL**에 대해 **NULL**을 지정할 수 있습니다. 이는 값을 가지지 않는 속성 또는 값이 존재하지 않는 속성으로 변환됩니다. AQS에서는 빈 대괄호 \[\]를 사용하여 **NULL**을 지정합니다.

> **중요** **COP\_VALUE\_CONTAINS** 및 **COP\_VALUE\_NOTCONTAINS** 연산자를 사용할 때는 문자열 및 문자열 배열에서 다르게 작동합니다. 문자열의 경우 시스템은 대/소문자를 구분하지 않는 검색을 수행하여 지정된 문자열이 부분 문자열로 디바이스에 포함되어 있는지 확인합니다. 문자열 배열의 경우에는 부분 문자열이 검색되지 않습니다. 문자열 배열에서는 배열을 검색하여 지정된 전체 문자열이 배열에 포함되어 있는지 확인합니다. 문자열 배열을 검색하여 배열의 요소에 부분 문자열이 포함되어 있는지 확인할 수는 없습니다.

결과의 범위를 적절히 지정할 AQS 필터 문자열 하나를 만들 수 없으면 결과를 받은 후 결과를 필터링할 수 있습니다. 그러나 이 방법을 선택하는 경우 초기 AQS 필터 문자열의 결과를 [**Windows.Devices.Enumeration**](https://msdn.microsoft.com/library/windows/apps/BR225459) API에 제공할 때 가능한 많이 제한하는 것이 좋습니다. 이렇게 하면 응용 프로그램의 성능을 개선하는 데 도움이 됩니다.

## AQS 문자열 예제

다음 예제에서는 AQS 구문을 사용하여 열거하려는 디바이스를 제한하는 방법을 보여 줍니다. 이러한 필터 문자열은 모두 [**DeviceInformationKind**](https://msdn.microsoft.com/library/windows/apps/Dn948991)와 쌍을 이루어 전체 필터를 만듭니다. 종류를 지정하지 않으면 기본 종류는 **DeviceInterface**입니다.

이 필터가 **DeviceInterface**의 [**DeviceInformationKind**](https://msdn.microsoft.com/library/windows/apps/Dn948991)와 쌍을 이룬 경우에는 오디오 캡처 인터페이스 클래스를 포함하고 현재 사용하도록 설정된 모든 개체가 열거됩니다. **=**는 **COP\_EQUALS**로 변환됩니다.

``` syntax
System.Devices.InterfaceClassGuid:="{2eef81be-33fa-4800-9670-1cd474972c3f}" AND 
System.Devices.InterfaceEnabled:=System.StructuredQueryType.Boolean#True
```

이 필터가 **Device**의 [**DeviceInformationKind**](https://msdn.microsoft.com/library/windows/apps/Dn948991)와 쌍을 이룬 경우에는 GenCdRom의 하드웨어 ID를 하나 이상 가진 모든 개체가 열거됩니다. **~~**는 **COP\_VALUE\_CONTAINS**로 변환됩니다.

``` syntax
System.Devices.HardwareIds:~~"GenCdRom"
```

이 필터가 **DeviceContainer**의 [**DeviceInformationKind**](https://msdn.microsoft.com/library/windows/apps/Dn948991)와 쌍을 이룬 경우에는 모델 이름에 부분 문자열 Microsoft가 포함된 모든 개체가 열거됩니다. **~~**는 **COP\_VALUE\_CONTAINS**로 변환됩니다.

``` syntax
System.Devices.ModelName:~~"Microsoft"
```

이 필터가 **DeviceInterface**의 [**DeviceInformationKind**](https://msdn.microsoft.com/library/windows/apps/Dn948991)와 쌍을 이룬 경우에는 모델 이름에 부분 문자열 Microsoft가 포함된 모든 개체가 열거됩니다. **~&lt;**는 **COP\_STARTSWITH**로 변환됩니다.

``` syntax
System.ItemNameDisplay:~<"Microsoft"
```

이 필터가 **Device**의 [**DeviceInformationKind**](https://msdn.microsoft.com/library/windows/apps/Dn948991)와 쌍을 이룬 경우에는 **System.Devices.IpAddress** 속성 집합이 있는 모든 개체가 열거됩니다. **&lt;&gt;\[\]**는 **NULL** 값과 결합된 **COP\_NOTEQUALS**로 변환됩니다.

``` syntax
System.Devices.IpAddress:<>[]
```

이 필터가 **Device**의 [**DeviceInformationKind**](https://msdn.microsoft.com/library/windows/apps/Dn948991)와 쌍을 이룬 경우에는 **System.Devices.IpAddress** 속성 집합이 없는 모든 개체가 열거됩니다. **=\[\]**는 **NULL** 값과 결합된 **COP\_EQUALS**로 변환됩니다.

``` syntax
System.Devices.IpAddress:=[]
```

 

 







<!--HONumber=Aug16_HO3-->


