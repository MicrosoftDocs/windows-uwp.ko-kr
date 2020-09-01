---
ms.assetid: D06AA3F5-CED6-446E-94E8-713D98B13CAA
title: 디바이스 선택기 빌드
description: 장치 선택기를 빌드하면 장치를 열거할 때 검색 하는 장치를 제한할 수 있습니다.
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 057b6d3087ae08cab6704dd83ebf1bf93b933a21
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89159697"
---
# <a name="build-a-device-selector"></a>디바이스 선택기 빌드



**중요 API**

- [**Windows.Devices.Enumeration**](/uwp/api/Windows.Devices.Enumeration)

장치 선택기를 빌드하면 장치를 열거할 때 검색 하는 장치를 제한할 수 있습니다. 이렇게 하면 관련 결과만 얻을 수 있으며 시스템 성능도 향상 됩니다. 대부분의 시나리오에서는 장치 스택에서 장치 선택기를 가져옵니다. 예를 들어, USB에서 검색 된 장치에 대해 [**Getdeviceselector**](/uwp/api/windows.devices.usb.usbdevice.getdeviceselector) 를 사용할 수 있습니다. 이러한 장치 선택기는 AQS (고급 쿼리 구문) 문자열을 반환 합니다. AQS 형식을 잘 모르는 경우에는 [프로그래밍 방식으로 고급 쿼리 구문 사용](/windows/desktop/search/-search-3x-advancedquerysyntax)에서 자세히 알아볼 수 있습니다.

## <a name="building-the-filter-string"></a>필터 문자열 작성

디바이스를 열거해야 하는데 제공된 디바이스 선택기를 시나리오에 사용할 수 없는 경우도 있습니다. 장치 선택기는 다음 정보를 포함 하는 AQS 필터 문자열입니다. 필터 문자열을 만들기 전에 열거 하려는 장치에 대 한 몇 가지 주요 정보를 알고 있어야 합니다.

-   원하는 장치의 [**Deviceinformationkind**](/uwp/api/Windows.Devices.Enumeration.DeviceInformationKind) 입니다. **Deviceinformationkind** 에서 장치를 열거 하는 방법에 대 한 자세한 내용은 [장치 열거](enumerate-devices.md)를 참조 하세요.
-   AQS 필터 문자열을 작성 하는 방법에 대 한 자세한 내용은이 항목에서 설명 합니다.
-   관심이 있는 속성입니다. 사용 가능한 속성은 [**Deviceinformationkind**](/uwp/api/Windows.Devices.Enumeration.DeviceInformationKind)에 따라 달라 집니다. 자세한 내용은 [장치 정보 속성](device-information-properties.md) 을 참조 하세요.
-   쿼리 하는 프로토콜입니다. 무선 또는 유선 네트워크를 통해 장치를 검색 하는 경우에만 필요 합니다. 이 작업을 수행 하는 방법에 대 한 자세한 내용은 [네트워크를 통한 장치 열거](enumerate-devices-over-a-network.md)를 참조 하세요.

[**Windows.**](/uwp/api/Windows.Devices.Enumeration) . i d api를 사용 하는 경우 사용자가 관심 있는 장치 종류와 장치 선택기를 결합 하는 경우가 많습니다. 사용 가능한 장치 종류 목록은 [**Deviceinformationkind**](/uwp/api/Windows.Devices.Enumeration.DeviceInformationKind) 열거에 의해 정의 됩니다. 이렇게 요소를 결합하면 사용 가능한 디바이스를 관심 있는 디바이스로 제한할 수 있습니다. **Deviceinformationkind**를 지정 하지 않거나 사용 중인 메서드에서 **deviceinformationkind** 매개 변수를 제공 하지 않는 경우 기본 종류는 **deviceinterface**입니다.

[**Windows. Enumeration**](/uwp/api/Windows.Devices.Enumeration) api는 정식 AQS 구문을 사용 하지만 일부 연산자는 지원 되지 않습니다. 필터 문자열을 생성할 때 사용할 수 있는 속성 목록은 [장치 정보 속성](device-information-properties.md)을 참조 하세요.

**주의**    형식을 사용 하 여 정의 된 사용자 지정 속성은 `{GUID} PID` AQS 필터 문자열을 생성할 때 사용할 수 없습니다. 이는 속성 형식이 잘 알려진 속성 이름에서 파생 되었기 때문입니다.

 

다음 표에서는 AQS 연산자와 해당 연산자가 지 원하는 매개 변수 유형을 나열 합니다.

| 연산자                       | 지원되는 형식                                                             |
|--------------------------------|-----------------------------------------------------------------------------|
| **복사 \_ 같음**                 | 문자열, 부울, GUID, UInt16, UInt32                                       |
| **복사 \_ 참고**              | 문자열, 부울, GUID, UInt16, UInt32                                       |
| **복사 \_ LESSTHAN**              | UInt16, UInt32                                                              |
| **복사 \_ GREATERTHAN**           | UInt16, UInt32                                                              |
| **복사 \_ LESSTHANOREQUAL**       | UInt16, UInt32                                                              |
| **복사 \_ GREATERTHANOREQUAL**    | UInt16, UInt32                                                              |
| **복사 \_ 값 \_ 포함**       | 문자열, 문자열 배열, 부울 배열, GUID 배열, UInt16 배열, UInt32 배열 |
| **복사 \_ VALUE \_ NOTCONTAINS**    | 문자열, 문자열 배열, 부울 배열, GUID 배열, UInt16 배열, UInt32 배열 |
| **복사 \_ 값 \_ STARTSWITH**     | String                                                                      |
| **복사 \_ 값 \_ ENDSWITH**       | String                                                                      |
| **복사 \_ DOSWILDCARDS**          | 지원되지 않음                                                               |
| **복사 \_ 단어 \_ 와 같음**           | 지원되지 않음                                                               |
| **복사 \_ WORD \_ STARTSWITH**      | 지원되지 않음                                                               |
| **복사 \_ 응용 프로그램 \_ 관련** | 지원되지 않음                                                               |


> **팁**    **NULL** 은 **복사 \_ EQUAL** 또는 복사를 지정할 **수 \_ **있습니다. 이는 값이 없는 속성으로 변환 되거나 값이 존재 하지 않습니다. AQS에서는 빈 대괄호를 사용 하 여 **NULL** 을 지정 \[ \] 합니다.

> **중요**    **복사 \_ value \_ contains** 및 **복사 \_ value \_ notcontains** 연산자를 사용 하는 경우 문자열 및 문자열 배열과 다르게 동작 합니다. 문자열의 경우 시스템은 대/소문자를 구분 하지 않는 검색을 수행 하 여 장치에 표시 된 문자열이 부분 문자열로 포함 되어 있는지 확인 합니다. 문자열 배열의 경우 부분 문자열은 검색 되지 않습니다. 문자열 배열이 있으면 배열을 검색 하 여 지정 된 전체 문자열이 포함 되어 있는지 확인 합니다. 문자열 배열을 검색 하 여 배열의 요소에 부분 문자열이 포함 되어 있는지 확인할 수 없습니다.

결과 범위를 적절 하 게 조정 하는 단일 AQS 필터 문자열을 만들 수 없는 경우 받은 후 결과를 필터링 할 수 있습니다. 그러나이 작업을 수행 하도록 선택 하는 경우에는 초기 [**AQS api에**](/uwp/api/Windows.Devices.Enumeration) 제공 하는 것과 같은 방법으로 초기 필터 문자열의 결과를 제한 하는 것이 좋습니다. 이렇게 하면 응용 프로그램의 성능을 향상 시킬 수 있습니다.

## <a name="aqs-string-examples"></a>AQS 문자열 예제

다음 예에서는 AQS 구문을 사용 하 여 열거 하려는 장치를 제한 하는 방법을 보여 줍니다. 이러한 필터 문자열은 모두 [**Deviceinformationkind**](/uwp/api/Windows.Devices.Enumeration.DeviceInformationKind) 와 결합 되어 완전 한 필터를 만듭니다. 종류가 지정 되지 않은 경우 기본 종류는 **Deviceinterface**입니다.

이 필터가 [**Deviceinformationkind**](/uwp/api/Windows.Devices.Enumeration.DeviceInformationKind) 의 **deviceinterface**와 쌍으로 연결 되 면 오디오 캡처 인터페이스 클래스를 포함 하 고 현재 사용 하도록 설정 된 모든 개체를 열거 합니다. **=****복사 \_ EQUALS**로 변환 합니다.

``` syntax
System.Devices.InterfaceClassGuid:="{2eef81be-33fa-4800-9670-1cd474972c3f}" AND
System.Devices.InterfaceEnabled:=System.StructuredQueryType.Boolean#True
```

이 필터가 [**Deviceinformationkind**](/uwp/api/Windows.Devices.Enumeration.DeviceInformationKind) **장치와**쌍으로 연결 된 경우 하나 이상의 하드웨어 id가 gencdrom 인 모든 개체를 열거 합니다. **~~****복사 \_ VALUE \_ CONTAINS**로 변환 합니다.

``` syntax
System.Devices.HardwareIds:~~"GenCdRom"
```

이 필터는 **DeviceContainer**의 [**deviceinformationkind**](/uwp/api/Windows.Devices.Enumeration.DeviceInformationKind) 와 쌍으로 연결 된 경우 Microsoft 부분 문자열을 포함 하는 모델 이름이 있는 모든 개체를 열거 합니다. **~~****복사 \_ VALUE \_ CONTAINS**로 변환 합니다.

``` syntax
System.Devices.ModelName:~~"Microsoft"
```

이 필터가 [**Deviceinformationkind**](/uwp/api/Windows.Devices.Enumeration.DeviceInformationKind) 의 **deviceinterface**와 쌍으로 연결 된 경우 이름이 인 개체를 모두 열거 Microsoft로 시작 합니다. **~&lt;****복사 \_ STARTSWITH**로 변환 합니다.

``` syntax
System.ItemNameDisplay:~<"Microsoft"
```

이 필터가 [**Deviceinformationkind**](/uwp/api/Windows.Devices.Enumeration.DeviceInformationKind) **장치와**쌍으로 연결 된 경우에는 **system.object** 속성이 설정 된 모든 개체를 열거 합니다. **&lt;&gt;\[\]** 는 **NULL** 값을 사용 하 여 **복사 \_ 참고** 로 변환 됩니다.

``` syntax
System.Devices.IpAddress:<>[]
```

이 필터가 [**Deviceinformationkind**](/uwp/api/Windows.Devices.Enumeration.DeviceInformationKind) **장치와**쌍으로 연결 된 경우에는 **system.web. IpAddress** 속성이 설정 되지 않은 모든 개체를 열거 합니다. **=\[\]** 복사와 ** \_ EQUALS** 를 모두 **NULL** 값으로 변환 합니다.

``` syntax
System.Devices.IpAddress:=[]
```

 

 